---
title: Base de dados SQL do Azure geridos instância auditoria | Documentos da Microsoft
description: Saiba como começar com o Azure SQL da base de dados geridos instância auditoria com T-SQL
services: sql-database
author: giladm
manager: craigg
ms.reviewer: carlrab
ms.service: sql-database
ms.custom: security
ms.topic: conceptual
ms.date: 08/28/2018
ms.author: giladm
ms.openlocfilehash: a43ca95717c712c932d29a619b7f1a0671c500bf
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/28/2018
ms.locfileid: "43125446"
---
# <a name="get-started-with-azure-sql-database-managed-instance-auditing"></a>Introdução ao Azure SQL da base de dados geridos instância de auditoria

[O Azure SQL Database Managed Instance](sql-database-managed-instance.md) faixas de auditoria de base de dados eventos e escreve-os para uma auditoria registo na sua conta de armazenamento do Azure. Auditoria também:
- Ajuda-o a manter a conformidade regulamentar, compreender a atividade de base de dados e obter informações sobre discrepâncias e anomalias que podem indicar preocupações empresariais ou suspeitas de violações de segurança.
- Ativa e facilita o cumprimento das normas de conformidade, embora ele não garante a conformidade. Para obter mais informações sobre o Azure programas de conformidade de padrões esse suporte, consulte a [Centro de fidedignidade do Azure](https://azure.microsoft.com/support/trust-center/compliance/).


## <a name="set-up-auditing-for-your-server"></a>Configurar a auditoria para o seu servidor

A seguinte secção descreve a configuração de auditoria na sua instância gerida.
1. Aceda ao [Portal do Azure](https://portal.azure.com).
2. Os passos seguintes criam um armazenamento do Azure **contentor** onde estão armazenados os registos de auditoria.

   - Navegue para o armazenamento do Azure onde pretende armazenar os seus registos de auditoria.

     > [!IMPORTANT]
     > Utilize uma conta de armazenamento na mesma região que o servidor de instância gerida para evitar leituras/escritas entre regiões.

   - Na conta de armazenamento, aceda a **descrição geral** e clique em **Blobs**.

     ![Painel de navegação][1]

   - No menu superior, clique em **+ contentor** para criar um novo contentor.

     ![Painel de navegação][2]

   - Fornecem um recipiente **nome**, defina o acesso público ao nível para **privada**e, em seguida, clique em **OK**.

     ![Painel de navegação][3]

   - Na lista de contentores, clique com o contentor criado recentemente e, em seguida, clique em **propriedades do contentor**.

     ![Painel de navegação][4]

   - Copie o URL de contentor ao clicar no ícone de copiar e guardar o URL (por exemplo, no bloco de notas) para utilização futura. Deve ser o formato de URL do contentor `https://<StorageName>.blob.core.windows.net/<ContainerName>`

     ![Painel de navegação][5]

3. Os passos seguintes geram um armazenamento do Azure **Token de SAS** utilizados para conceder direitos de acesso de auditoria de instância gerida para a conta de armazenamento.

   - Navegue para a conta de armazenamento do Azure onde criou o contentor no passo anterior.

   - Clique em **assinatura de acesso partilhado** no menu de definições de armazenamento.

     ![Painel de navegação][6]

   - Configure a SAS da seguinte forma:
     - **Permitido serviços**: Blob
     - **Data de início**: para evitar problemas relacionados com o fuso horário, é recomendável usar a data de ontem.
     - **Data de fim**: escolher a data em que este Token de SAS expire. 

       > [!NOTE]
       > Renove o token após a expiração: para evitar falhas de auditoria.

     - Clique em **Gerar SAS**.

       ![Painel de navegação][7]

   - Depois de clicar em gerar SAS, o Token de SAS é apresentada na parte inferior. Copie o token ao clicar no ícone de copiar e guardá-lo (por exemplo, no bloco de notas) para utilização futura.

     > [!IMPORTANT]
     > Remover o ponto de interrogação ("?") caráter desde o início do token.

     ![Painel de navegação][8]

4. Ligar à sua instância gerida através do SQL Server Management Studio (SSMS).

5. Execute a seguinte instrução T-SQL para **criar uma nova credencial** através do URL de contentor e Token SAS que criou nos passos anteriores:

    ```SQL
    CREATE CREDENTIAL [<container_url>]
    WITH IDENTITY='SHARED ACCESS SIGNATURE',
    SECRET = '<SAS KEY>'
    GO
    ```

6. Execute a seguinte instrução T-SQL para criar um novo Server Audit (escolher o seu próprio nome de auditoria, utilize o URL de contentor que criou nos passos anteriores):

    ```SQL
    CREATE SERVER AUDIT [<your_audit_name>]
    TO URL ( PATH ='<container_url>' [, RETENTION_DAYS =  integer ])
    GO
    ```

    Se não for especificado, `RETENTION_DAYS` padrão é 0 (retenção ilimitada).

    Para obter informações adicionais:
    - [Auditoria de diferenças entre a instância gerida, BD SQL do Azure e SQL Server](#subheading-3)
    - [CRIAR A AUDITORIA DE SERVIDOR](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)
    - [ALTER SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)

7. Crie uma especificação de auditoria de servidor ou a especificação de auditoria de base de dados, tal como faria para o SQL Server:
    - [Criar o guia de T-SQL de especificação de auditoria de servidor](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-specification-transact-sql)
    - [Criar o guia de T-SQL de especificação de auditoria de base de dados](https://docs.microsoft.com/sql/t-sql/statements/create-database-audit-specification-transact-sql)

8. Ative a auditoria de servidor que criou no passo 6:

    ```SQL
    ALTER SERVER AUDIT [<your_audit_name>]
    WITH (STATE=ON);
    GO
    ```

## <a name="analyze-audit-logs"></a>Analisar registos de auditoria
Existem vários métodos que pode utilizar para ver os registos de auditoria de Blobs.

- Utilize a função de sistema `sys.fn_get_audit_file` (T-SQL) para retornar os dados de registo de auditoria num formato tabular. Para obter mais informações sobre como utilizar esta função, consulte a [sys.fn_get_audit_file documentação](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).

- Para obter uma lista completa dos métodos de consumo de log de auditoria, consulte a [introdução à auditoria da base de dados SQL](https://docs.microsoft.com/ azure/sql-database/sql-database-auditing).

> [!IMPORTANT]
> O método para visualizar os registos de auditoria do portal do Azure (painel de registos de auditoria) está atualmente indisponível para a instância gerida.

## <a name="auditing-differences-between-managed-instance-azure-sql-database-and-sql-server"></a>Auditoria de diferenças entre a instância gerida, base de dados do Azure SQL e SQL Server

As principais diferenças entre auditoria de SQL na instância gerida, base de dados do Azure SQL e SQL Server no local são:

- Na instância gerida, a auditoria de SQL funciona no nível do servidor e arquivos `.xel` ficheiros na conta de armazenamento de Blobs do Azure de registo.
- Na base de dados SQL do Azure, a auditoria de SQL funciona ao nível da base de dados.
- No SQL Server no local / virtual máquinas, funciona de auditoria de SQL no servidor de nível, mas os eventos de arquivos no sistema de ficheiros/windows registos de eventos.

Auditoria de XEvent na instância gerida suporta destinos de armazenamento de Blobs do Azure. Os registos de ficheiros e do windows estão **nepodporuje**.

A chave de diferenças no `CREATE AUDIT` sintaxe para a auditoria para o armazenamento de Blobs do Azure são:
- Uma nova sintaxe `TO URL` é fornecido e permite-lhe especificar o URL do contentor de armazenamento de Blobs do Azure onde o `.xel` ficheiros são colocados.
- A sintaxe `TO FILE` é **nepodporuje** porque a instância gerida não é possível aceder a partilhas de ficheiros do Windows.
- É a opção Encerrar **nepodporuje**.
- `queue_delay` 0 é **nepodporuje**.


## <a name="next-steps"></a>Passos Seguintes

- Para obter uma lista completa dos métodos de consumo de log de auditoria, consulte a [introdução à auditoria da base de dados SQL](https://docs.microsoft.com/ azure/sql-database/sql-database-auditing).
- Para obter mais informações sobre o Azure programas de conformidade de padrões esse suporte, consulte a [Centro de fidedignidade do Azure](https://azure.microsoft.com/support/trust-center/compliance/).


<!--Anchors-->
[Set up auditing for your server]: #subheading-1
[Analyze audit logs]: #subheading-2
[Auditing differences between Managed Instance, Azure SQL DB and SQL Server]: #subheading-3

<!--Image references-->
[1]: ./media/sql-managed-instance-auditing/1_blobs_widget.png
[2]: ./media/sql-managed-instance-auditing/2_create_container_button.png
[3]: ./media/sql-managed-instance-auditing/3_create_container_config.png
[4]: ./media/sql-managed-instance-auditing/4_container_properties_button.png
[5]: ./media/sql-managed-instance-auditing/5_container_copy_name.png
[6]: ./media/sql-managed-instance-auditing/6_storage_settings_menu.png
[7]: ./media/sql-managed-instance-auditing/7_sas_configure.png
[8]: ./media/sql-managed-instance-auditing/8_sas_copy.png
