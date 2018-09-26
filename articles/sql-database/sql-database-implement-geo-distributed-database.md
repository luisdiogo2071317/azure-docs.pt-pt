---
title: Implementar uma solução de Base de Dados SQL do Azure distribuída geograficamente | Microsoft Docs
description: Saiba como configurar a sua Base de Dados SQL do Azure e a aplicação para ativação pós-falha para uma base de dados replicada e ativação pós-falha de teste.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 09/07/2018
ms.openlocfilehash: 65cf954f5d91176715181620671f620264069bdc
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166270"
---
# <a name="implement-a-geo-distributed-database"></a>Implementar uma base de dados distribuída geograficamente

Neste tutorial, pode configurar uma base de dados SQL do Azure e a aplicação para ativação pós-falha para uma região remota e, em seguida, testar o seu plano de ativação pós-falha. Saiba como: 

> [!div class="checklist"]
> * Criar utilizadores da base de dados e conceder-lhes permissões
> * Configurar uma regra de firewall ao nível da base de dados
> * Criar um [grupo de ativação pós-falha de georreplicação](sql-database-geo-replication-overview.md)
> * Criar e compilar uma aplicação Java para consultar uma base de dados SQL do Azure
> * Efetuar um teste de recuperação após desastre

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.


## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, confirme que conclui os pré-requisitos seguintes:

- Instalar a versão mais recente do [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs). 
- Instalar uma base de dados SQL do Azure. Este tutorial utiliza a base de dados de exemplo AdventureWorksLT com o nome **mySampleDatabase** a partir de um dos seguintes inícios rápidos:

   - [Criar BD - Portal](sql-database-get-started-portal.md)
   - [Criar BD - CLI](sql-database-cli-samples.md)
   - [Criar BD - PowerShell](sql-database-powershell-samples.md)

- Depois de identificar um método para executar scripts SQL na base de dados, pode utilizar uma das seguintes ferramentas de consulta:
   - O editor de consultas no [portal do Azure](https://portal.azure.com). Para obter mais informações sobre como utilizar o editor de consultas no portal do Azure, veja [Ligar e consultar com o Editor de Consultas](sql-database-get-started-portal.md#query-the-sql-database).
   - Instale a versão mais recente do [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), que é um ambiente integrado para gerir qualquer infraestrutura SQL, do SQL Server à Base de Dados SQL do Microsoft Windows.
   - Instale a versão mais recente do [Visual Studio Code](https://code.visualstudio.com/docs), que é um editor de código gráfico para Linux, macOS e Windows que suporta extensões, incluindo a [extensão mssql](https://aka.ms/mssql-marketplace) para consultar o Microsoft SQL Server, a Base de Dados SQL do Azure e o SQL Data Warehouse. Para obter mais informações sobre como utilizar esta ferramenta com a Base de Dados SQL do Azure, veja [Ligar e consultar com o VS Code](sql-database-connect-query-vscode.md). 

## <a name="create-database-users-and-grant-permissions"></a>Criar utilizadores da base de dados e conceder permissões

Ligue à base de dados e crie contas de utilizador com uma das seguintes ferramentas de consulta:

- O Editor de Consultas no portal do Azure
- SQL Server Management Studio
- Visual Studio Code

Estas contas de utilizador são replicadas automaticamente para o servidor secundário (e são mantidas em sincronização). Para utilizar o SQL Server Management Studio ou o Visual Studio Code, pode ter de configurar uma regra de firewall se estiver a ligar a partir de um cliente num endereço IP para o qual ainda não configurou uma firewall. Para obter passos detalhados, veja [Criar uma regra de firewall ao nível do servidor](sql-database-get-started-portal-firewall.md).

- Numa janela de consulta, execute a consulta seguinte para criar duas contas de utilizador na base de dados. Este script concede permissões **db_owner** à conta **app_admin** e atribui permissões **SELECT** e **UPDATE** à conta **app_user**. 

   ```sql
   CREATE USER app_admin WITH PASSWORD = 'ChangeYourPassword1';
   --Add SQL user to db_owner role
   ALTER ROLE db_owner ADD MEMBER app_admin; 
   --Create additional SQL user
   CREATE USER app_user WITH PASSWORD = 'ChangeYourPassword1';
   --grant permission to SalesLT schema
   GRANT SELECT, INSERT, DELETE, UPDATE ON SalesLT.Product TO app_user;
   ```

## <a name="create-database-level-firewall"></a>Criar uma firewall ao nível da base de dados

Crie uma [regra de firewall ao nível da base de dados](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) para a sua base de dados SQL. Esta regra de firewall ao nível da base de dados é replicada automaticamente para o servidor secundário que vai criar neste tutorial. Para maior simplicidade (neste tutorial), utilize o endereço IP público do computador onde está a efetuar os passos neste tutorial. Para determinar o endereço IP utilizado para a regra de firewall ao nível do servidor para o computador atual, veja [Criar uma firewall ao nível do servidor](sql-database-get-started-portal-firewall.md).  

- Na janela de consulta aberta, substitua a consulta anterior pela consulta seguinte, ao substituir os endereços IP pelos endereços IP adequados para o seu ambiente.  

   ```sql
   -- Create database-level firewall setting for your public IP address
   EXECUTE sp_set_database_firewall_rule @name = N'myGeoReplicationFirewallRule',@start_ip_address = '0.0.0.0', @end_ip_address = '0.0.0.0';
   ```

## <a name="create-an-active-geo-replication-auto-failover-group"></a>Criar um grupo de ativação pós-falha automática de georreplicação ativa 

Com o Azure PowerShell, crie um [grupo de ativação pós-falha automática de georreplicação ativa](sql-database-geo-replication-overview.md) entre o servidor SQL do Azure existente e o novo servidor SQL do Azure vazio numa região do Azure e, em seguida, adicione a base de dados de exemplo ao grupo de ativação pós-falha.

> [!IMPORTANT]
> Estes cmdlets precisam do Azure PowerShell 4.0. [!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install-no-ssh.md)]
>

1. Preencha as variáveis dos scripts do PowerShell com os valores do servidor existente e da base de dados de exemplo, e forneça um valor exclusivo global para o nome do grupo de ativação pós-falha.

   ```powershell
   $adminlogin = "ServerAdmin"
   $password = "ChangeYourAdminPassword1"
   $myresourcegroupname = "<your resource group name>"
   $mylocation = "<your resource group location>"
   $myservername = "<your existing server name>"
   $mydatabasename = "mySampleDatabase"
   $mydrlocation = "<your disaster recovery location>"
   $mydrservername = "<your disaster recovery server name>"
   $myfailovergroupname = "<your unique failover group name>"
   ```

2. Crie um servidor de cópia de segurança vazio na sua região de ativação pós-falha.

   ```powershell
   $mydrserver = New-AzureRmSqlServer -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername `
      -Location $mydrlocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $mydrserver   
   ```

3. Crie um grupo de ativação pós-falha entre os dois servidores.

   ```powershell
   $myfailovergroup = New-AzureRMSqlDatabaseFailoverGroup `
      –ResourceGroupName $myresourcegroupname `
      -ServerName $myservername `
      -PartnerServerName $mydrservername  `
      –FailoverGroupName $myfailovergroupname `
      –FailoverPolicy Automatic `
      -GracePeriodWithDataLossHours 2
   $myfailovergroup   
   ```

4. Adicione a base de dados ao grupo de ativação pós-falha.

   ```powershell
   $myfailovergroup = Get-AzureRmSqlDatabase `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $myservername `
      -DatabaseName $mydatabasename | `
    Add-AzureRmSqlDatabaseToFailoverGroup `
      -ResourceGroupName $myresourcegroupname ` `
      -ServerName $myservername `
      -FailoverGroupName $myfailovergroupname
   $myfailovergroup   
   ```

## <a name="install-java-software"></a>Instalar o software de Java

Os passos nesta secção assumem que está familiarizado com programação com Java e que nunca trabalhou com a Base de Dados SQL do Azure. 

### <a name="mac-os"></a>**Mac OS**
Abra o terminal e navegue para um diretório no qual pretenda criar o seu projeto Java. Instale **brew** e **Maven** ao introduzir os seguintes comandos: 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install maven
```

Para obter instruções detalhadas sobre como instalar e configurar o ambiente de Java e Maven, aceda a [Criar uma aplicação com o SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), selecione **Java**, selecione **MacOS** e, em seguida, siga as instruções detalhadas para configurar o Java e o Maven nos passos 1.2 e 1.3.

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Abra o terminal e navegue para um diretório no qual pretenda criar o seu projeto Java. Instale **Maven** ao introduzir os seguintes comandos:

```bash
sudo apt-get install maven
```

Para obter instruções detalhadas sobre como instalar e configurar o ambiente de Java e Maven, aceda a [Criar uma aplicação com o SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), selecione **Java**, selecione **Ubuntu** e, em seguida, siga as instruções detalhadas para configurar o Java e o Maven nos passos 1.2, 1.3 e 1.4.

### <a name="windows"></a>**Windows**
Instale [Maven](https://maven.apache.org/download.cgi) ao utilizar o instalador oficial. Utilize o Maven para ajudar a gerir dependências e criar, testar e executar o projeto de Java. Para obter instruções detalhadas sobre como instalar e configurar o ambiente de Java e Maven, aceda a [Criar uma aplicação com o SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), selecione **Java**, selecione Windows e, em seguida, siga as instruções detalhadas para configurar o Java e o Maven nos passos 1.2 e 1.3.

## <a name="create-sqldbsample-project"></a>Criar projeto SqlDbSample

1. Na consola de comandos (por exemplo, Bash), crie um projeto Maven. 
   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```
2. Escreva **Y** e clique em **Enter**.
3. Altere os diretórios no projeto criado recentemente.

   ```bash
   cd SqlDbSamples
   ```

4. Com o seu editor favorito, abra o ficheiro pom.xml na pasta do projeto. 

5. Adicione o Microsoft JDBC Driver para a dependência de SQL Server ao projeto Maven ao abrir o seu editor de texto favorito e ao copiar e colar as seguintes linhas no ficheiro pom.xml. Não substitua os valores existentes pré-preenchidos no ficheiro. A dependência JDBC tem de ser colada na secção "dependencies" maior ( ).   

   ```xml
   <dependency>
     <groupId>com.microsoft.sqlserver</groupId>
     <artifactId>mssql-jdbc</artifactId>
    <version>6.1.0.jre8</version>
   </dependency>
   ```

6. Especifique a versão de Java para compilar o projeto ao adicionar a secção "properties" seguinte no ficheiro pom.xml depois da secção "dependencies". 

   ```xml
   <properties>
     <maven.compiler.source>1.8</maven.compiler.source>
     <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```
7. Adicione a secção "build" seguinte no ficheiro pom.xml depois da secção "properties" para suportar os ficheiros de manifesto em jars.       

   ```xml
   <build>
     <plugins>
       <plugin>
         <groupId>org.apache.maven.plugins</groupId>
         <artifactId>maven-jar-plugin</artifactId>
         <version>3.0.0</version>
         <configuration>
           <archive>
             <manifest>
               <mainClass>com.sqldbsamples.App</mainClass>
             </manifest>
           </archive>
        </configuration>
       </plugin>
     </plugins>
   </build>
   ```
8. Guarde e feche o ficheiro pom.xml.
9. Abra o ficheiro App.java (C:\apache-maven-3.5.0\SqlDbSample\src\main\java\com\sqldbsamples\App.java) e substitua o conteúdo pelo conteúdo seguinte. Substitua o nome do grupo de ativação pós-falha pelo nome do seu grupo de ativação pós-falha. Se tiver alterado os valores do nome da base de dados, utilizador ou palavra-passe, altere também esses valores.

   ```java
   package com.sqldbsamples;

   import java.sql.Connection;
   import java.sql.Statement;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.Timestamp;
   import java.sql.DriverManager;
   import java.util.Date;
   import java.util.concurrent.TimeUnit;

   public class App {

      private static final String FAILOVER_GROUP_NAME = "myfailovergroupname";
  
      private static final String DB_NAME = "mySampleDatabase";
      private static final String USER = "app_user";
      private static final String PASSWORD = "ChangeYourPassword1";

      private static final String READ_WRITE_URL = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);
      private static final String READ_ONLY_URL = String.format("jdbc:sqlserver://%s.secondary.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);

      public static void main(String[] args) {
         System.out.println("#######################################");
         System.out.println("## GEO DISTRIBUTED DATABASE TUTORIAL ##");
         System.out.println("#######################################");
         System.out.println(""); 

         int highWaterMark = getHighWaterMarkId();

         try {
            for(int i = 1; i < 1000; i++) {
                //  loop will run for about 1 hour
                System.out.print(i + ": insert on primary " + (insertData((highWaterMark + i))?"successful":"failed"));
                TimeUnit.SECONDS.sleep(1);
                System.out.print(", read from secondary " + (selectData((highWaterMark + i))?"successful":"failed") + "\n");
                TimeUnit.SECONDS.sleep(3);
            }
         } catch(Exception e) {
            e.printStackTrace();
      }
   }

   private static boolean insertData(int id) {
      // Insert data into the product table with a unique product name that we can use to find the product again later
      String sql = "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";

      try (Connection connection = DriverManager.getConnection(READ_WRITE_URL); 
              PreparedStatement pstmt = connection.prepareStatement(sql)) {
         pstmt.setString(1, "BrandNewProduct" + id);
         pstmt.setInt(2, 200989 + id + 10000);
         pstmt.setString(3, "Blue");
         pstmt.setDouble(4, 75.00);
         pstmt.setDouble(5, 89.99);
         pstmt.setTimestamp(6, new Timestamp(new Date().getTime()));
         return (1 == pstmt.executeUpdate());
      } catch (Exception e) {
         return false;
      }
   }

   private static boolean selectData(int id) {
      // Query the data that was previously inserted into the primary database from the geo replicated database
      String sql = "SELECT Name, Color, ListPrice FROM SalesLT.Product WHERE Name = ?";

      try (Connection connection = DriverManager.getConnection(READ_ONLY_URL); 
              PreparedStatement pstmt = connection.prepareStatement(sql)) {
         pstmt.setString(1, "BrandNewProduct" + id);
         try (ResultSet resultSet = pstmt.executeQuery()) {
            return resultSet.next();
         }
      } catch (Exception e) {
         return false;
      }
   }

   private static int getHighWaterMarkId() {
      // Query the high water mark id that is stored in the table to be able to make unique inserts 
      String sql = "SELECT MAX(ProductId) FROM SalesLT.Product";
      int result = 1;
        
      try (Connection connection = DriverManager.getConnection(READ_WRITE_URL); 
              Statement stmt = connection.createStatement();
              ResultSet resultSet = stmt.executeQuery(sql)) {
         if (resultSet.next()) {
             result =  resultSet.getInt(1);
            }
         } catch (Exception e) {
          e.printStackTrace();
         }
         return result;
      }
   }
   ```
6. Guarde e feche o ficheiro App.java.

## <a name="compile-and-run-the-sqldbsample-project"></a>Compilar e executar o projeto SqlDbSample

1. Na consola de comandos, execute o comando seguinte.

   ```bash
   mvn package
   ```
2. Quando terminar, execute o comando seguinte para executar a aplicação (é executada durante cerca de 1 hora, a menos que seja parada manualmente):

   ```bash
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   
   #######################################
   ## GEO DISTRIBUTED DATABASE TUTORIAL ##
   #######################################

   1. insert on primary successful, read from secondary successful
   2. insert on primary successful, read from secondary successful
   3. insert on primary successful, read from secondary successful
   ```

## <a name="perform-disaster-recovery-drill"></a>Efetuar teste de recuperação após desastre

1. Chame a ativação pós-falha manual do grupo de ativação pós-falha. 

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $mydrservername `
   -FailoverGroupName $myfailovergroupname
   ```

2. Observe os resultados da aplicação durante a ativação pós-falha. Alguns inserções falham durante a atualização da cache DNS.     

3. Determine a função do servidor de recuperação após desastre que está a ser executada.

   ```powershell
   $mydrserver.ReplicationRole
   ```

4. Reativação pós-falha.

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $myservername `
   -FailoverGroupName $myfailovergroupname
   ```

5. Observe os resultados da aplicação durante a reativação pós-falha. Alguns inserções falham durante a atualização da cache DNS.     

6. Determine a função do servidor de recuperação após desastre que está a ser executada.

   ```powershell
   $fileovergroup = Get-AzureRMSqlDatabaseFailoverGroup `
      -FailoverGroupName $myfailovergroupname `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername
   $fileovergroup.ReplicationRole
   ```

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a configurar uma base de dados SQL do Azure e a aplicação para ativação pós-falha para uma região remota e, em seguida, testar o seu plano de ativação pós-falha.  Aprendeu a: 

> [!div class="checklist"]
> * Criar utilizadores da base de dados e conceder-lhes permissões
> * Configurar uma regra de firewall ao nível da base de dados
> * Criar um grupo de ativação pós-falha de georreplicação
> * Criar e compilar uma aplicação Java para consultar uma base de dados SQL do Azure
> * Efetuar um teste de recuperação após desastre

Avance para o próximo tutorial para migrar o SQL Server para o Azure SQL Database Managed Instance com o DMS.

> [!div class="nextstepaction"]
>[Migrar o SQL Server para a Instância Gerida da Base de Dados SQL do Azure com o DMS](../dms/tutorial-sql-server-to-managed-instance.md)

