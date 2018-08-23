---
title: Restaurar uma cópia de segurança para uma Instância Gerida da Base de Dados SQL do Azure | Microsoft Docs
description: Restaure uma cópia de segurança para uma Instância Gerida da Base de Dados SQL do Azure através do SSMS.
keywords: tutorial de base de dados sql, instância gerida da base de dados sql, restaurar uma cópia de segurança
services: sql-database
author: bonova
ms.reviewer: carlrab, srbozovi
ms.service: sql-database
ms.custom: managed instance
ms.topic: tutorial
ms.date: 07/06/2018
ms.author: bonova
manager: craigg
ms.openlocfilehash: 5dd8b1f662f1ae6d6502743c6d976db4b58e962f
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/15/2018
ms.locfileid: "41919180"
---
# <a name="restore-a-database-backup-to-an-azure-sql-database-managed-instance"></a>Restaurar uma cópia de segurança da base de dados para uma Instância Gerida da Base de Dados SQL do Azure

Este tutorial demonstra como restaurar uma cópia de segurança de uma base de dados no armazenamento de blobs do Azure para a Instância Gerida através do Wide World Importers - ficheiro de cópia de segurança Standard. Este método requer algum período de indisponibilidade. Para obter um tutorial, utilizando o Azure Database Migration Service (DMS) para migração, veja [Migração de Instância Gerida com o DMS](../dms/tutorial-sql-server-to-managed-instance.md). Para ver um debate dos vários métodos de migração, veja [Migração da instância do SQL Server para a Instância Gerida da Base de Dados SQL do Azure](sql-database-managed-instance-migrate.md).

> [!div class="checklist"]
> * Transferir o Wide World Importers - ficheiro de cópia de segurança Standard
> * Criar conta de armazenamento do Azure e carregar o ficheiro de cópia de segurança
> * Restaurar a base de dados Wide World Importers a partir de um ficheiro de cópia de segurança

## <a name="prerequisites"></a>Pré-requisitos

Este tutorial utiliza como ponto de partida os recursos criados neste tutorial: [Criar uma Instância Gerida da Base de Dados SQL do Azure](sql-database-managed-instance-get-started.md).

## <a name="download-the-wide-world-importers---standard-backup-file"></a>Transferir o Wide World Importers - ficheiro de cópia de segurança Standard

Utilize os seguintes passos para transferir o Wide World Importers - ficheiro de cópia de segurança Standard.

Com o Internet Explorer, introduza https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImporters-Standard.bak na caixa de endereço do URL e, em seguida, quando for solicitado, clique em **Guardar** para guardar este ficheiro na pasta **Transferências**.

## <a name="log-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [Portal do Azure](https://portal.azure.com/#create/Microsoft.SQLManagedInstance).

## <a name="create-azure-storage-account-and-upload-backup-file"></a>Criar conta de armazenamento do Azure e carregar o ficheiro de cópia de segurança

1. Clique em **Criar um recurso**, no canto superior esquerdo do portal do Azure.
2. Localize **Armazenamento** e, em seguida, clique em **Conta de Armazenamento** para abrir o formulário da conta de armazenamento.

   ![conta de armazenamento](./media/sql-database-managed-instance-tutorial/storage-account.png)

3. Preencha o formulário da conta de armazenamento com as informações pedidas, utilizando as informações da tabela seguinte:

   | Definição| Valor sugerido | Descrição |
   | ------ | --------------- | ----------- |
   |**Nome**|Qualquer nome válido|Para nomes válidos, veja [Regras e restrições de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Deployment model** (Modelo de implementação)|Modelo de recursos||
   |**Account kind** (Tipo de conta)|Armazenamento de blobs||
   |**Performance** (Desempenho)|Standard ou Premium|Unidades magnéticas ou SSDs|
   |**Replicação**|Armazenamento localmente redundante||
   |** Camada de acesso (predefinição)|Frequente ou esporádico||
   |**Secure transfer required** (Transferência segura necessária)|Desativado||
   |**Subscrição**|A sua subscrição|Para obter detalhes sobre as suas subscrições, veja [Subscriptions](https://account.windowsazure.com/Subscriptions) (Subscrições).|
   |**Grupo de recursos**|O grupo de recursos que criou anteriormente|| 
   |**Localização**|A localização que selecionou anteriormente||
   |**Redes virtuais**|Desativado||

4. Clique em **Criar**.

   ![detalhes da conta de armazenamento](./media/sql-database-managed-instance-tutorial/storage-account-details.png)

5. Após a implementação da conta de armazenamento ser bem sucedida, abra a sua nova conta de armazenamento.
6. Em **Definições**, clique em **Assinatura de Acesso Partilhado** para abrir o formulário da assinatura de acesso partilhado (SAS).

   ![formulário da sas](./media/sql-database-managed-instance-tutorial/sas-form.png)

7. No formulário da SAS, modifique os valores predefinidos, conforme pretendido. Tenha em atenção que a data/hora de expiração é, por predefinição, apenas de 8 horas.
8. Clique em **Gerar SAS**.

   ![formulário da sas concluído](./media/sql-database-managed-instance-tutorial/sas-generate.png)

9. Copie e guarde o **token SAS** e o **URL da SAS do servidor de blobs**.
10. Em **Definições**, clique em **Contentores**.

    ![containers](./media/sql-database-managed-instance-tutorial/containers.png)

11. Clique em **+ Contentor** para criar um contentor para conter o ficheiro de cópia de segurança.
12. Preencha o formulário do contentor com as informações pedidas, utilizando as informações da tabela seguinte:

    | Definição| Valor sugerido | Descrição |
   | ------ | --------------- | ----------- |
   |**Nome**|Qualquer nome válido|Para nomes válidos, veja [Regras e restrições de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Nível de acesso público**|Privado (sem acesso anónimo)||

    ![detalhe de contentor](./media/sql-database-managed-instance-tutorial/container-detail.png)

13. Clique em **OK**.
14. Depois de o contentor estar criado, abra o contentor.

    ![contentor](./media/sql-database-managed-instance-tutorial/container.png)

15. Clique em **Propriedades do contentor** e, em seguida, copie o URL para o contentor.

    ![URL do contentor](./media/sql-database-managed-instance-tutorial/container-url.png)

16. Clique em **Carregar** para abrir o formulário **Carregar blob**.

    ![carregar](./media/sql-database-managed-instance-tutorial/upload.png)

17. Navegue até à sua pasta de transferências e selecione o ficheiro **WideWorldIimporters-Standard.bak**.

    ![carregar](./media/sql-database-managed-instance-tutorial/upload-bak.png)

18. Clique em **Carregar**.
19. Não continue até que o carregamento esteja concluído.

    ![carregamento concluído](./media/sql-database-managed-instance-tutorial/upload-complete.png)


## <a name="restore-the-wide-world-importers-database-from-a-backup-file"></a>Restaurar a base de dados Wide World Importers a partir de um ficheiro de cópia de segurança

Com o SQL Server Management Studio, utilize os seguintes passos para restaurar a base de dados Wide World Importers para a sua Instância Gerida, a partir do ficheiro de cópia de segurança.

1. No SQL Server Management Studio, abra uma nova janela de consulta.
2. Utilize o seguinte script para criar uma credencial SAS, ao fornecer o URL para o contentor da conta de armazenamento e a chave SAS, conforme indicado.

   ```sql
   CREATE CREDENTIAL [https://<storage_account_name>.blob.core.windows.net/<container>] 
      WITH IDENTITY = 'SHARED ACCESS SIGNATURE'
      , SECRET = '<shared_access_signature_key_with_removed_first_?_symbol>' 
   ```

    ![credencial](./media/sql-database-managed-instance-tutorial/credential.png)

3. Utilize o seguinte script para criar e verificar a credencial SAS e a validade da cópia de segurança - fornecendo o URL do contentor com o ficheiro de cópia de segurança:

   ```sql
   RESTORE FILELISTONLY FROM URL = 
      'https://<storage_account_name>.blob.core.windows.net/<container>/WideWorldImporters-Standard.bak'
   ```

    ![lista de ficheiros](./media/sql-database-managed-instance-tutorial/file-list.png)

4. Utilize o seguinte script para restaurar a base de dados Wide World Importers a partir de um ficheiro de cópia de segurança - ao fornecer o URL do contentor com o ficheiro de cópia de segurança:

   ```sql
   RESTORE DATABASE [Wide World Importers] FROM URL =
     'https://<storage_account_name>.blob.core.windows.net/<container>/WideWorldImporters-Standard.bak'`
   ```

    ![execução de restauro](./media/sql-database-managed-instance-tutorial/restore-executing.png)

5. Para controlar o estado do restauro, execute a seguinte consulta numa nova sessão de consulta:

   ```sql
   SELECT session_id as SPID, command, a.text AS Query, start_time, percent_complete
      , dateadd(second,estimated_completion_time/1000, getdate()) as estimated_completion_time 
   FROM sys.dm_exec_requests r 
   CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a 
   WHERE r.command in ('BACKUP DATABASE','RESTORE DATABASE')`
   ```

    ![percentagem de restauro concluída](./media/sql-database-managed-instance-tutorial/restore-percent-complete.png)

6. Quando o restauro estiver concluído, irá vê-lo no Object Explorer. 

    ![restauro concluído](./media/sql-database-managed-instance-tutorial/restore-complete.png)

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a restaurar uma cópia de segurança de uma base de dados no armazenamento de blobs do Azure para a Instância Gerida através do Wide World Importers - ficheiro de cópia de segurança Standard. Aprendeu a: 

> [!div class="checklist"]
> * Transferir o Wide World Importers - ficheiro de cópia de segurança Standard
> * Criar conta de armazenamento do Azure e carregar o ficheiro de cópia de segurança
> * Restaurar a base de dados Wide World Importers a partir de um ficheiro de cópia de segurança

Avance para o próximo tutorial para saber como migrar o SQL Server para uma Instância Gerida da Base de Dados SQL do Azure com o DMS.

> [!div class="nextstepaction"]
>[Migrar o SQL Server para a Instância Gerida da Base de Dados SQL do Azure com o DMS](../dms/tutorial-sql-server-to-managed-instance.md)
