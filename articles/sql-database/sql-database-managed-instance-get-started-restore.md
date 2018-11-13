---
title: Restaurar uma cópia de segurança para uma Instância Gerida da Base de Dados SQL do Azure | Microsoft Docs
description: Restaure uma cópia de segurança para uma Instância Gerida da Base de Dados SQL do Azure através do SSMS.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: carlrab, bonova
manager: craigg
ms.date: 11/01/2018
ms.openlocfilehash: bc27ece2eddc842a81698aaa685cbe6d63c6a1df
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50912259"
---
# <a name="quickstart-restore-a-database-backup-to-an-azure-sql-database-managed-instance"></a>Início Rápido: Restaurar uma cópia de segurança da base de dados para uma Instância Gerida da Base de Dados SQL do Azure

Este guia de início rápido demonstra como restaurar uma cópia de segurança de uma base de dados no armazenamento de blobs do Azure para a Instância Gerida através do Wide World Importers – ficheiro de cópia de segurança Standard. Este método requer algum período de indisponibilidade. 

> [!VIDEO https://www.youtube.com/embed/RxWYojo_Y3Q]

Para obter um tutorial, utilizando o Azure Database Migration Service (DMS) para migração, veja [Migração de Instância Gerida com o DMS](../dms/tutorial-sql-server-to-managed-instance.md). Para ver um debate dos vários métodos de migração, veja [Migração da instância do SQL Server para a Instância Gerida da Base de Dados SQL do Azure](sql-database-managed-instance-migrate.md).

## <a name="prerequisites"></a>Pré-requisitos

Este guia de início rápido:
- Utiliza como ponto de partida os recursos criados neste guia de início rápido: [Criar uma Instância Gerida](sql-database-managed-instance-get-started.md).
- Necessita da versão mais recente do [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) no computador cliente no local
- Necessita de conectividade à Instância Gerida com o SQL Server Management Studio. Veja estes guias de início rápido para obter opções de conectividade:
  - [Ligar a uma Instância Gerida da Base de Dados SQL do Azure a partir de uma VM do Azure](sql-database-managed-instance-configure-vm.md)
  - [Ligar a uma Instância Gerida da Base de Dados SQL do Azure a partir do local através de uma ligação Ponto a Site](sql-database-managed-instance-configure-p2s.md).
- Utiliza uma conta de armazenamento de blobs do Azure pré-configurada com o Wide World Importers – ficheiro de cópia de segurança Standard (transferido a partir de https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImporters-Standard.bak).

> [!NOTE]
> Para obter mais informações sobre a cópia de segurança e o restauro de uma base de dados do SQL Server através do armazenamento de blobs do Azure e de uma Assinatura de Acesso Partilhado (SAS), veja [Cópia de Segurança do SQL Server para URL](sql-database-managed-instance-get-started-restore.md).

## <a name="restore-the-wide-world-importers-database-from-a-backup-file"></a>Restaurar a base de dados Wide World Importers a partir de um ficheiro de cópia de segurança

Com o SQL Server Management Studio, utilize os seguintes passos para restaurar a base de dados Wide World Importers para a sua Instância Gerida, a partir do ficheiro de cópia de segurança.

1. Abra o SQL Server Management Studio (SSMS) e ligue-se à Instância Gerida.
2. No SQL Server Management Studio, abra uma nova janela de consulta.
3. Utilize o seguinte script para criar uma credencial na Instância Gerida com a conta de armazenamento pré-configurada e a chave SAS.

   ```sql
   CREATE CREDENTIAL [https://mitutorials.blob.core.windows.net/databases] 
   WITH IDENTITY = 'SHARED ACCESS SIGNATURE'
   , SECRET = 'sv=2017-11-09&ss=bfqt&srt=sco&sp=rwdlacup&se=2028-09-06T02:52:55Z&st=2018-09-04T18:52:55Z&spr=https&sig=WOTiM%2FS4GVF%2FEEs9DGQR9Im0W%2BwndxW2CQ7%2B5fHd7Is%3D' 
   ```

    ![Criar credencial](./media/sql-database-managed-instance-get-started-restore/credential.png)

    > [!NOTE]
    > Remova sempre o **?** inicial da chave SAS gerada.
  
3. Utilize o seguinte script para verificar a credencial SAS e a validade da cópia de segurança ao fornecer o URL do contentor com o ficheiro de cópia de segurança:

   ```sql
   RESTORE FILELISTONLY FROM URL = 
      'https://mitutorials.blob.core.windows.net/databases/WideWorldImporters-Standard.bak'
   ```

    ![lista de ficheiros](./media/sql-database-managed-instance-get-started-restore/file-list.png)

4. Utilize o seguinte script para restaurar a base de dados Wide World Importers a partir de um ficheiro de cópia de segurança - ao fornecer o URL do contentor com o ficheiro de cópia de segurança:

   ```sql
   RESTORE DATABASE [Wide World Importers] FROM URL =
     'https://mitutorials.blob.core.windows.net/databases/WideWorldImporters-Standard.bak'
   ```

    ![restore](./media/sql-database-managed-instance-get-started-restore/restore.png)

5. Para controlar o estado do restauro, execute a seguinte consulta numa nova sessão de consulta:

   ```sql
   SELECT session_id as SPID, command, a.text AS Query, start_time, percent_complete
      , dateadd(second,estimated_completion_time/1000, getdate()) as estimated_completion_time 
   FROM sys.dm_exec_requests r 
   CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a 
   WHERE r.command in ('BACKUP DATABASE','RESTORE DATABASE')`
   ```

6. Quando o restauro estiver concluído, irá vê-lo no Object Explorer. 

## <a name="next-steps"></a>Passos seguintes

- Para a resolução de problemas com a cópia de segurança para URL, veja [Melhores Práticas e Resolução de Problemas da Cópia de Segurança do SQL Server para URL](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting).
- Para uma descrição geral das opções de ligação para aplicações, veja [Ligar as suas aplicações à Instância Gerida](sql-database-managed-instance-connect-app.md).
- Para consultar através de uma das suas ferramentas ou linguagens favoritas, veja [ligar e consultar](sql-database-connect-query.md).
