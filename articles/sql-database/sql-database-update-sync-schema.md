---
title: Automatizar a replicação das alterações de esquema na sincronização de dados SQL do Azure | Microsoft Docs
description: Saiba como automatizar a replicação das alterações de esquema na sincronização de dados SQL do Azure.
services: sql-database
ms.date: 06/19/2018
ms.topic: conceptual
ms.service: sql-database
author: allenwux
ms.author: xiwu
ms.reviewer: douglasl
manager: craigg
ms.custom: data-sync
ms.openlocfilehash: a39e060708514fdca11a5d89858486b442a18309
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37019592"
---
# <a name="automate-the-replication-of-schema-changes-in-azure-sql-data-sync"></a>Automatizar a replicação das alterações de esquema na sincronização de dados SQL do Azure

Sincronização de dados do SQL Server permite que os utilizadores a sincronizar dados entre bases de dados do Azure SQL Server e no local do SQL Server na direção de um ou em ambas as direções. Uma das atuais limitações de sincronização de dados do SQL Server é uma falta de suporte para a replicação das alterações de esquema. Sempre que alterar o esquema da tabela, terá de aplicar as alterações manualmente em todos os pontos finais, incluindo o hub e todos os membros e, em seguida, atualize o esquema de sincronização.

Este artigo apresenta uma solução para replicar as alterações de esquema automaticamente para todos os pontos finais de sincronização de dados do SQL Server.
1. Esta solução utiliza um acionador DDL para controlar as alterações de esquema.
2. O acionador insere os comandos de alteração de esquema de uma tabela de controlo.
3. Este controlo de tabela está sincronizada com todos os pontos finais utilizando o serviço de sincronização de dados.
4. Acionadores DML após a inserção são utilizados para aplicar as alterações de esquema em outros pontos finais.

Este artigo utiliza ALTER TABLE como um exemplo de uma alteração de esquema, mas esta solução também funciona para outros tipos de alterações do esquema.

> [!IMPORTANT]
> Recomendamos que leia este artigo cuidadosamente, especialmente as secções sobre [resolução de problemas](#troubleshooting) e [outras considerações](#other), antes de começar a implementar a replicação de alteração de esquema automatizada em o ambiente de sincronização. Também recomendamos que leia [sincronizar os dados em várias bases de dados na nuvem e no local com sincronização de dados do SQL Server](sql-database-sync-data.md). Algumas operações de base de dados poderão interromper a solução descrita neste artigo. Dados de conhecimento de domínio adicionais do SQL Server e o Transact-SQL poderão ser necessário para resolver esses problemas.

![Automatizar a replicação das alterações de esquema](media/sql-database-update-sync-schema/automate-schema-changes.png)

## <a name="set-up-automated-schema-change-replication"></a>Configurar a replicação de alteração de esquema automatizada

### <a name="create-a-table-to-track-schema-changes"></a>Criar uma tabela para controlar as alterações de esquema

Crie uma tabela para controlar as alterações de esquema em todas as bases de dados no grupo de sincronização:

```sql
CREATE TABLE SchemaChanges (
ID bigint IDENTITY(1,1) PRIMARY KEY,
SqlStmt nvarchar(max),
[Description] nvarchar(max)
)
```

Esta tabela tem uma coluna de identidade para controlar a ordem das alterações ao esquema. Pode adicionar mais campos para obter mais informações de registo, se necessário.

### <a name="create-a-table-to-track-the-history-of-schema-changes"></a>Criar uma tabela para controlar o histórico de alterações do esquema

Em todos os pontos finais, crie uma tabela para controlar o ID do comando de alteração de esquema mais recentemente aplicado.

```sql
CREATE TABLE SchemaChangeHistory (
LastAppliedId bigint PRIMARY KEY
)
GO

INSERT INTO SchemaChangeHistory VALUES (0)
```

### <a name="create-an-alter-table-ddl-trigger-in-the-database-where-schema-changes-are-made"></a>Criar um acionador de DDL ALTER de tabela na base de dados em que são efetuadas alterações de esquema

Crie um acionador DDL para operações de ALTER TABLE. Só tem de criar este acionador na base de dados em que são efetuadas alterações ao esquema. Para evitar conflitos, permitir apenas alterações de esquema numa base de dados num grupo de sincronização.

```sql
CREATE TRIGGER AlterTableDDLTrigger
ON DATABASE
FOR ALTER_TABLE
AS

-- You can add your own logic to filter ALTER TABLE commands instead of replicating all of them.

IF NOT (EVENTDATA().value('(/EVENT_INSTANCE/SchemaName)[1]', 'nvarchar(512)') like 'DataSync')

INSERT INTO SchemaChanges (SqlStmt, Description)
    VALUES (EVENTDATA().value('(/EVENT_INSTANCE/TSQLCommand/CommandText)[1]', 'nvarchar(max)'), 'From DDL trigger')
```

O acionador insere um registo na tabela para cada comando ALTER TABLE do registo de alterações de esquema. Este exemplo adiciona um filtro para evitar a replicar as alterações de esquema feitas em esquema **DataSync**, porque estas provavelmente efetuada pelo serviço de sincronização de dados. Adicione mais filtros se pretender replicar determinados tipos de alterações do esquema.

Também pode adicionar mais acionadores para replicar outros tipos de alterações ao esquema. Por exemplo, crie acionadores CREATE_PROCEDURE, ALTER_PROCEDURE e DROP_PROCEDURE para replicar as alterações para procedimentos armazenados.

### <a name="create-a-trigger-on-other-endpoints-to-apply-schema-changes-during-insertion"></a>Criar um acionador em outros pontos finais para aplicar alterações de esquema durante a inserção

Este acionador executa o comando de alteração de esquema ao que está sincronizado com outros pontos finais. Tem de criar este acionador em todos os pontos finais, exceto o onde são efetuadas alterações de esquema (ou seja, na base de dados onde o DDL acionar `AlterTableDDLTrigger` é criado no passo anterior).

```sql
CREATE TRIGGER SchemaChangesTrigger
ON SchemaChanges
AFTER INSERT
AS
DECLARE \@lastAppliedId bigint
DECLARE \@id bigint
DECLARE \@sqlStmt nvarchar(max)
SELECT TOP 1 \@lastAppliedId=LastAppliedId FROM SchemaChangeHistory
SELECT TOP 1 \@id = id, \@SqlStmt = SqlStmt FROM SchemaChanges WHERE id \> \@lastAppliedId ORDER BY id
IF (\@id = \@lastAppliedId + 1)
BEGIN
    EXEC sp_executesql \@SqlStmt
        UPDATE SchemaChangeHistory SET LastAppliedId = \@id
    WHILE (1 = 1)
    BEGIN
        SET \@id = \@id + 1
        IF exists (SELECT id FROM SchemaChanges WHERE ID = \@id)
            BEGIN
                SELECT \@sqlStmt = SqlStmt FROM SchemaChanges WHERE ID = \@id
                EXEC sp_executesql \@SqlStmt
                UPDATE SchemaChangeHistory SET LastAppliedId = \@id
            END
        ELSE
            BREAK;
    END
END
```

Este acionador é executado após a inserção e verifica se o comando atual deve executar seguinte. A lógica de código assegura que nenhuma instrução de alteração de esquema é ignorada e todas as alterações são aplicadas, mesmo que esteja fora de ordem a inserção.

### <a name="sync-the-schema-change-tracking-table-to-all-endpoints"></a>A alteração do esquema controlo de tabela para todos os pontos finais de sincronização

Pode sincronizar a alteração do esquema controlo de tabela para todos os pontos finais utilizando o grupo de sincronização existente ou um novo grupo de sincronização. Certifique-se de que podem ser sincronizadas as alterações na tabela de controlo para todos os pontos finais, especialmente quando estiver a utilizar uma direção sincronização.

Não sincroniza a tabela de histórico de alterações de esquema, uma vez que essa tabela mantém o estado diferente em pontos finais diferentes.

### <a name="apply-the-schema-changes-in-a-sync-group"></a>Aplicar as alterações de esquema de um grupo de sincronização

São replicadas apenas esquema as alterações efetuadas na base de dados onde o acionador DDL é criado. Alterações de esquema noutras bases de dados não são replicadas.

Depois das alterações do esquema são replicadas para todos os pontos finais, também terá de efetuar passos adicionais para atualizar o esquema de sincronização para iniciar ou parar a sincronização das novas colunas.

#### <a name="add-new-columns"></a>Adicionar novas colunas

1.  Se o esquema alterar.

2.  Evite qualquer alteração de dados em que as novas colunas envolvidas até concluir o passo que cria o acionador.

3.  Aguarde até que as alterações do esquema são aplicadas a todos os pontos finais.

4.  Atualize o esquema de base de dados e adicionar a nova coluna para o esquema de sincronização.

5.  Dados na coluna novo estão sincronizados durante a próxima operação de sincronização.

#### <a name="remove-columns"></a>Remover colunas

1.  Remova as colunas do esquema de sincronização. Sincronização de dados para a sincronizar dados estas colunas.

2.  Se o esquema alterar.

3.  Atualize o esquema de base de dados.

#### <a name="update-data-types"></a>Atualizar os tipos de dados

1.  Se o esquema alterar.

2.  Aguarde até que as alterações do esquema são aplicadas a todos os pontos finais.

3.  Atualize o esquema de base de dados.

4.  Se os tipos de dados antiga e não são totalmente compatíveis - por exemplo, se alterar do `int` para `bigint` -sincronização poderão falhar antes dos passos que criar os acionadores são concluídos. Sincronização é concluída com êxito após uma nova tentativa.

#### <a name="rename-columns-or-tables"></a>Mudar o nome de tabelas ou colunas

Mudar o nome de tabelas ou colunas faz a sincronização de dados deixam de funcionar. Criar uma nova tabela ou coluna de preenchimento os dados e, em seguida, elimine a antiga tabela ou coluna em vez de mudar o nome.

#### <a name="other-types-of-schema-changes"></a>Outros tipos de alterações do esquema

Para outros tipos de alterações do esquema - por exemplo, criar procedimentos armazenados ou remover um índice - atualizar o esquema de sincronização não é necessário.

## <a name="troubleshoot"></a> Resolver problemas de replicação de alteração de esquema automatizada

A lógica de replicação descrita neste artigo deixa de funcionar em algumas situações - por exemplo, se tiver efetuado um esquema alterar numa base de dados no local que não é suportado no SQL Database do Azure. Nesse caso, a tabela de registo de alterações do esquema de sincronização irá falhar. Tem de corrigir este problema manualmente:

1.  Desative o acionador DDL e evitar quaisquer alterações de esquema adicionais até que o problema esteja corrigido.

2.  A base de dados de ponto final, onde o problema está a acontecer, desative o acionador AFTER INSERT no ponto final onde não é possível efetuar a alteração de esquema. Esta ação permite que o comando de alteração de esquema para ser sincronizada com êxito.

3.  Acionar a sincronização ao sincronizar a tabela de registo de alterações de esquema.

4.  A base de dados de ponto final, onde o problema está a acontecer, consulta o esquema de alterar a tabela de histórico para obter o ID da última comandos de alteração de esquema aplicados.

5.  Consulta a tabela para listar todos os comandos com um ID maior que o valor de ID que obteve no passo anterior do registo de alterações de esquema.

    a.  Ignore os comandos que não não possível executar a base de dados do ponto final. Tem de lidar com a inconsistência de esquema. Reverta as alterações de esquema original se a inconsistência afeta a sua aplicação.

    b.  Aplica manualmente os comandos que devem ser aplicados.

6.  Atualizar a tabela de histórico de alterações de esquema e definir o último ID aplicado para o valor correto.

7.  Verifique se o esquema é atualizado.

8.  Volte a ativar o acionador AFTER INSERT desativado no segundo passo.

9.  Volte a ativar o acionador DDL desativado no primeiro passo.

Se pretende limpar os registos na tabela de controlo de alterações de esquema, utilize DELETE em vez de TRUNCATE. Nunca reseed a coluna de identidade na tabela de registo utilizando DBCC CHECKIDENT de alterações de esquema. Pode criar tabelas de controlo de nova alteração de esquema e atualizar o nome da tabela no acionador DDL se reseeding é necessária.

## <a name="other"></a> Outras considerações

-   Os utilizadores de base de dados que configurar as bases de dados do hub e o membro tem de ter permissão suficiente para executar os comandos de alteração de esquema.

-   Pode adicionar mais filtros no acionador DDL para replicar apenas alterações de esquema em tabelas selecionadas nem de operações.

-   Só pode efetuar alterações de esquema na base de dados onde o acionador DDL é criado.

-   Se estiver a efetuar uma alteração numa base de dados do SQL Server no local, certifique-se que a alteração do esquema é suportada no SQL Database do Azure.

-   Caso sejam feitas alterações de esquema em bases de dados que não seja a base de dados em que o acionador DDL está criado, não são replicadas as alterações. Para evitar este problema, pode criar os acionadores DDL para bloquear as alterações nos outros pontos finais.

-   Se precisar de alterar o esquema da tabela de controlo de alterações de esquema, desative o acionador DDL antes de efetuar a alteração e, em seguida, aplicar a alteração manualmente para todos os pontos finais. Atualizar o esquema num acionador AFTER INSERT na mesma tabela não funciona.

-   Não reseed a coluna de identidade ao utilizar DBCC CHECKIDENT.

-   Não utilize TRUNCATE a limpeza dos dados em tabela do registo de alterações de esquema.
