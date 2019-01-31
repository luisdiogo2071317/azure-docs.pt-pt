---
title: Configurar a replicação na instância gerida da base de dados SQL do Azure | Documentos da Microsoft
description: Saiba mais sobre como configurar a replicação transacional na instância gerida da base de dados SQL do Azure
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: allenwux
ms.author: xiwu
ms.reviewer: mathoma
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: b0188a0983ea18490f3997b857386e313daa58ed
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55467668"
---
# <a name="configure-replication-in-azure-sql-database-managed-instance"></a>Configurar a replicação na instância gerida da base de dados SQL do Azure

Replicação transacional permite-lhe replicar os dados dos bancos de dados do SQL Server ou de instância gerida da base de dados SQL do Azure para a instância gerida, ou para enviar por push as alterações feitas em seus bancos de dados na instância gerida para outro SQL Server, base de dados única base de dados SQL ou conjunto elástico ou outra instância gerida. A replicação está em pré-visualização pública no [instância gerida da base de dados SQL do Azure](sql-database-managed-instance.md). Uma instância gerida pode alojar as bases de dados do publicador, o distribuidor e o subscritor. Ver [configurações de replicação transacional](sql-database-managed-instance-transactional-replication.md#common-configurations) para configurações disponíveis.

## <a name="requirements"></a>Requisitos

Editor e distribuidor na base de dados do Azure SQL requer:

- O Azure SQL Database Managed Instance que não esteja na configuração de Geo-DR.

   >[!NOTE]
   >Bases de dados SQL do Azure que não estão configurados com a instância gerida só é possível os subscritores.

- Todas as instâncias do SQL Server tem de estar na mesma vNet.

- Conectividade utiliza a autenticação do SQL entre os participantes de replicação.

- Um compartilhamento de conta de armazenamento do Azure para o diretório de trabalho de replicação.

- Porta 445 (saída de TCP) tem de estar abertas nas regras de segurança de sub-rede da instância gerida para aceder à partilha de ficheiros do Azure

## <a name="features"></a>Funcionalidades

Suporta:

- Combinação de replicação de instantâneo e transacional de instâncias de instância gerida da base de dados SQL do Azure e no local.

- Os assinantes podem ser bases de dados agrupadas em conjuntos elásticos da base de dados do Azure SQL, base de dados na base de dados do Azure SQL ou no local.

- Unidirecional ou bidirecional de replicação.

Não são suportadas as seguintes funcionalidades:

- Subscrições atualizáveis.

- Georreplicação ativa.

## <a name="configure-publishing-and-distribution-example"></a>Configurar o exemplo de publicação e distribuição

1. [Criar uma instância de gerida de base de dados do Azure SQL](sql-database-managed-instance-create-tutorial-portal.md) no portal.
2. [Criar uma conta de armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account#create-a-storage-account) para o diretório de trabalho.

   Certifique-se de que copie as chaves de armazenamento. Ver [ver e copiar chaves de acesso de armazenamento](../storage/common/storage-account-manage.md#access-keys
).
3. Crie uma base de dados para o publicador.

   Nos scripts de exemplo abaixo, substitua `<Publishing_DB>` com o nome desta base de dados.

4. Crie um utilizador de base de dados com a autenticação de SQL para o distribuidor. Utilize uma palavra-passe segura.

   Nos scripts de exemplo abaixo, utilize `<SQL_USER>` e `<PASSWORD>` com esta conta do SQL Server da base de dados utilizador e palavra-passe.

5. [Ligue-se para a instância gerida da base de dados SQL](sql-database-connect-query-ssms.md).

6. Execute a consulta seguinte para adicionar o distribuidor e a base de dados de distribuição.

   ```sql
   USE [master]
   GO
   EXEC sp_adddistributor @distributor = @@ServerName;
   EXEC sp_adddistributiondb @database = N'distribution';
   ```

7. Para configurar um publicador para utilizar uma base de dados de distribuição especificado, atualize e execute a consulta seguinte.

   Substitua `<SQL_USER>` e `<PASSWORD>` com a conta do SQL Server e a palavra-passe.

   Substitua `\\<STORAGE_ACCOUNT>.file.core.windows.net\<SHARE>` com o valor da sua conta de armazenamento.  

   Substitua `<STORAGE_CONNECTION_STRING>` com a cadeia de ligação a partir de **chaves de acesso** separador da sua conta de armazenamento do Microsoft Azure.

   Depois de atualizar a seguinte consulta, executá-lo.

   ```sql
   USE [master]
   EXEC sp_adddistpublisher @publisher = @@ServerName,
                @distribution_db = N'distribution',
                @security_mode = 0,
                @login = N'<SQL_USER>',
                @password = N'<PASSWORD>',
                @working_directory = N'\\<STORAGE_ACCOUNT>.file.core.windows.net\<SHARE>',
                @storage_connection_string = N'<STORAGE_CONNECTION_STRING>';
   GO
   ```

8. Configure o publicador para a replicação.

    Na consulta seguinte, substitua `<Publishing_DB>` com o nome da sua base de dados do publicador.

    Substitua `<Publication_Name>` com o nome da sua publicação.

    Substitua `<SQL_USER>` e `<PASSWORD>` com a conta do SQL Server e a palavra-passe.

    Depois de atualizar a consulta, execute-o para criar a publicação.

   ```sql
   USE [<Publishing_DB>]
   EXEC sp_replicationdboption @dbname = N'<Publishing_DB>',
                @optname = N'publish',
                @value = N'true';

   EXEC sp_addpublication @publication = N'<Publication_Name>',
                @status = N'active';

   EXEC sp_changelogreader_agent @publisher_security_mode = 0,
                @publisher_login = N'<SQL_USER>',
                @publisher_password = N'<PASSWORD>',
                @job_login = N'<SQL_USER>',
                @job_password = N'<PASSWORD>';

   EXEC sp_addpublication_snapshot @publication = N'<Publication_Name>',
                @frequency_type = 1,
                @publisher_security_mode = 0,
                @publisher_login = N'<SQL_USER>',
                @publisher_password = N'<PASSWORD>',
                @job_login = N'<SQL_USER>',
                @job_password = N'<PASSWORD>'
   ```

9. Adicione o artigo, a subscrição e o agente de subscrição de push.

   Para adicionar esses objetos, atualize o seguinte script.

   - Substitua `<Object_Name>` com o nome do objeto de publicação.
   - Substitua `<Object_Schema>` com o nome do esquema de origem.
   - Substitua os outros parâmetros parênteses angulares `<>` de acordo com os valores nos scripts do anteriores.

   ```sql
   EXEC sp_addarticle @publication = N'<Publication_Name>',
                @type = N'logbased',
                @article = N'<Object_Name>',
                @source_object = N'<Object_Name>',
                @source_owner = N'<Object_Schema>'

   EXEC sp_addsubscription @publication = N'<Publication_Name>',
                @subscriber = @@ServerName,
                @destination_db = N'<Subscribing_DB>',
                @subscription_type = N'Push'

   EXEC sp_addpushsubscription_agent @publication = N'<Publication_Name>',
                @subscriber = @@ServerName,
                @subscriber_db = N'<Subscribing_DB>',
                @subscriber_security_mode = 0,
                @subscriber_login = N'<SQL_USER>',
                @subscriber_password = N'<PASSWORD>',
                @job_login = N'<SQL_USER>',
                @job_password = N'<PASSWORD>'
   GO
   ```
   
## <a name="see-also"></a>Consultar Também

- [Replicação transacional](sql-database-managed-instance-transactional-replication.md)
- [O que é uma instância gerida?](sql-database-managed-instance.md)
