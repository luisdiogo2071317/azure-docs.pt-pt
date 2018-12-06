---
title: 'Tutorial: Criar a sua primeira base de dados SQL do Azure com o SSMS | Microsoft Docs'
description: Saiba como criar a sua primeira base de dados SQL do Azure com o SQL Server Management Studio.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.topic: tutorial
author: CarlRabeler
ms.author: carlrab
ms.reviewer: v-masebo
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: 9fa36b9b87a8e9591b0c863826cd2278a29ba28e
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52956062"
---
# <a name="tutorial-design-your-first-azure-sql-database-using-ssms"></a>Tutorial: Criar a sua primeira base de dados SQL do Azure com o SSMS

Base de dados SQL do Azure é um relacional da base de dados-como-serviço (DBaaS) no Microsoft Cloud (Azure). Neste tutorial, irá aprender a utilizar o portal do Azure e o [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) para:

> [!div class="checklist"]
> * Criar uma base de dados no portal do Azure*
> * Configurar uma regra de firewall ao nível do servidor no portal do Azure
> * Ligar à base de dados com o SSMS
> * Criar tabelas com o SSMS
> * Carregar dados em massa com o BCP
> * Consultar dados com o SSMS

* Se não tiver uma subscrição do Azure, [criar uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

> [!NOTE]
> Para efeitos deste tutorial, estamos a utilizar o [modelo de compras com base na DTU](sql-database-service-tiers-dtu.md), mas tem a opção de escolher o [modelo de compras com base no vCore](sql-database-service-tiers-vcore.md).

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, certifique-se de que instalou:

- [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (versão mais recente)
- [BCP e SQLCMD](https://www.microsoft.com/download/details.aspx?id=36433) (versão mais recente)

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [portal do Azure](https://portal.azure.com/).

## <a name="create-a-blank-database"></a>Criar uma base de dados vazia

É criada uma base de dados SQL do Azure com um conjunto definido de [recursos de armazenamento e computação](sql-database-service-tiers-dtu.md). A base de dados é criada dentro de um [grupo de recursos do Azure](../azure-resource-manager/resource-group-overview.md) e, num [servidor lógico de base de dados SQL do Azure](sql-database-features.md).

Siga estes passos para criar uma base de dados SQL vazia.

1. Clique em **Criar um recurso**, no canto superior esquerdo do portal do Azure.

1. Na página **Nova**, selecione **Bases de Dados** na secção de Microsoft Azure Marketplace e, em seguida, clique em **Base de Dados SQL** na secção **Em Destaque**.

   ![create empty-database](./media/sql-database-design-first-database/create-empty-database.png)

   1. Preencha os **base de dados SQL** de formulário com as informações seguintes, conforme mostrado na imagem anterior:

      | Definição       | Valor sugerido | Descrição |
      | ------------ | ------------------ | ------------------------------------------------- |
      | **Nome da base de dados** | *yourDatabase* | Para nomes de base de dados válida, veja [identificadores de bases de dados](/sql/relational-databases/databases/database-identifiers). |
      | **Subscrição** | *yourSubscription*  | Para obter detalhes sobre as suas subscrições, veja [Subscriptions](https://account.windowsazure.com/Subscriptions) (Subscrições). |
      | **Grupo de recursos** | *yourResourceGroup* | Para nomes de grupo de recursos válidos, veja [Naming rules and restrictions](/azure/architecture/best-practices/naming-conventions) (Atribuição de nomes de regras e restrições). |
      | **Selecionar origem** | Base de dados vazia | Especifica que deve ser criada uma base de dados vazia. |

   1. Clique em **servidor** para utilizar um servidor existente ou criar e configurar um novo servidor da base de dados. Selecione o servidor ou clique em **criar um novo servidor** e preencha a **novo servidor** formulário com as seguintes informações:

      | Definição       | Valor sugerido | Descrição |
      | ------------ | ------------------ | ------------------------------------------------- |
      | **Nome do servidor** | Qualquer nome globalmente exclusivo | Para nomes de servidores válidos, veja [Naming rules and restrictions](/azure/architecture/best-practices/naming-conventions) (Atribuição de nomes de regras e restrições). |
      | **Início de sessão de administrador do servidor** | Qualquer nome válido | Para nomes de início de sessão válido, veja [identificadores de bases de dados](/sql/relational-databases/databases/database-identifiers). |
      | **Palavra-passe** | Qualquer palavra-passe válida | A palavra-passe tem de ter, pelo menos, oito carateres e tem de utilizar carateres de três das seguintes categorias: carateres maiúsculos, carateres minúsculos, números e carateres não alfanuméricos. |
      | **Localização** | Nenhuma localização válida | Para obter mais informações sobre regiões, veja [Azure Regions](https://azure.microsoft.com/regions/) (Regiões do Azure). |

      ![criar servidor de base de dados](./media/sql-database-design-first-database/create-database-server.png)

      Clique em **Selecionar**.

   1. Clique em **Escalão de preço** para especificar a camada de serviço, o número de DTUs ou vCores e a quantidade de armazenamento. Pode explorar as opções para o número de DTUs/vCores e armazenamento que está disponível para cada escalão de serviço. Por predefinição, o **padrão** [modelo de compra baseado em DTU](sql-database-service-tiers-dtu.md) está selecionada, mas tem a opção de escolher o [modelo de compra baseado em vCore](sql-database-service-tiers-vcore.md).

      > [!IMPORTANT]
      > Mais de 1 TB de armazenamento no escalão Premium está atualmente disponível em todas as regiões, exceto o seguinte: norte do Reino Unido, e.u.a. centro-oeste, South2 do Reino Unido, leste da China, USDoDCentral, Alemanha Central, Sudoeste do USDoDEast, US Gov, Gov Sul dos EUA, Nordeste da Alemanha, China Norte, E.U. a leste Gov. Noutras regiões, o armazenamento máximo no escalão Premium está limitado a 1 TB. Ver [Limitações Atuais P11-P15]( sql-database-dtu-resource-limits-single-databases.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).

      Depois de selecionar o escalão de serviço, o número de DTUs e a quantidade de armazenamento, clique em **aplicar**.

   1. Introduza um **agrupamento** para a base de dados em branco (para este tutorial, utilize o valor predefinido). Para obter mais informações sobre agrupamentos, veja [Agrupamentos](/sql/t-sql/statements/collations)

1. Agora que concluiu o **base de dados SQL** de formulário, clique em **criar** para aprovisionar a base de dados. Este passo pode demorar alguns minutos.

1. Na barra de ferramentas, clique em **Notificações** para monitorizar o processo de implementação.

     ![notificação](./media/sql-database-design-first-database/notification.png)

## <a name="create-a-firewall-rule"></a>Criar uma regra de firewall

O serviço de base de dados SQL cria uma firewall ao nível do servidor. O firewall impede que aplicações e ferramentas externas estabeleçam uma ligação ao servidor e a quaisquer bases de dados no servidor. Para ativar a conectividade externa ao seu banco de dados, primeiro tem de adicionar uma regra para o seu endereço IP para o firewall. Siga estes passos para criar uma [regra de firewall ao nível do servidor de base de dados SQL](sql-database-firewall-configure.md).

> [!NOTE]
> Base de dados SQL comunica através da porta 1433. Se estiver a tentar ligar a partir de uma rede empresarial, o tráfego de saída através da porta 1433 poderá não ser permitido pela firewall da rede. Se assim for, não é possível ligar ao seu servidor de base de dados do Azure SQL, a menos que o administrador abra a porta 1433.

1. Depois de concluída a implementação, clique em **bases de dados SQL** no menu do lado esquerdo e clique em *yourDatabase* sobre o **bases de dados SQL** página. Abre a página de descrição geral da base de dados, mostrando a completamente qualificado **nome do servidor** (tal como *yourserver.database.windows.net*) e oferece opções para configuração adicional.

1. Copie este nome de servidor completamente qualificado para utilizar para ligar ao servidor e bases de dados do SQL Server Management Studio.

   ![nome do servidor](./media/sql-database-design-first-database/server-name.png)

1. Clique em **Definir firewall do servidor** na barra de ferramentas. O **definições da Firewall** é aberta a página do servidor de base de dados SQL.

   ![regra de firewall do servidor](./media/sql-database-design-first-database/server-firewall-rule.png)

   1. Clique em **Adicionar IP de cliente**, na barra de ferramentas, para adicionar o seu endereço IP atual a uma nova regra de firewall. Uma regra de firewall consegue abrir a porta 1433 para um único endereço IP ou para um intervalo de endereços IP.

   1. Clique em **Guardar**. É criada uma regra de firewall ao nível do servidor para a sua porta de abertura 1433 do endereço IP atual no servidor lógico.

   1. Clique em **OK** e, em seguida, feche a página **Definições da firewall**.

Agora pode transmitir o seu endereço IP através da firewall. Pode agora ligar para o servidor de base de dados SQL e respetivas bases de dados com SQL Server Management Studio ou outra ferramenta à sua escolha. Certifique-se de que utilizar a conta de administrador de servidor que criou anteriormente.

> [!IMPORTANT]
> Por predefinição, o acesso através da firewall de base de dados SQL está ativado para todos os serviços do Azure. Clique em **DESATIVAR** nesta página para desativar todos os serviços do Azure.

## <a name="connect-to-the-database"></a>Ligar à base de dados

Uso [SQL Server Management Studio](/sql/ssms/sql-server-management-studio-ssms) para estabelecer uma ligação ao seu servidor de base de dados SQL do Azure.

1. Abra o SQL Server Management Studio.

1. Na caixa de dialogo **Ligar ao Servidor**, introduza as seguintes informações:

   | Definição       | Valor sugerido | Descrição |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Tipo de servidor** | Motor de base de dados | Este valor é preciso. |
   | **Nome do servidor** | O nome de servidor completamente qualificado | Por exemplo, *yourserver.database.windows.net*. |
   | **Autenticação** | Autenticação do SQL Server | Autenticação do SQL é o único tipo de autenticação que configuramos neste tutorial. |
   | **Início de sessão** | A conta de administrador do servidor | A conta que especificou quando criou o servidor. |
   | **Palavra-passe** | A palavra-passe da sua conta de administrador do servidor | A palavra-passe que especificou quando criou o servidor. |

   ![ligar ao servidor](./media/sql-database-design-first-database/connect.png)

   1. Clique em **Opções** na caixa de diálogo **Ligar ao servidor**. Na **ligar à base de dados** , digite *yourDatabase* para ligar a esta base de dados.

      ![ligar à base de dados no servidor](./media/sql-database-design-first-database/options-connect-to-db.png)  

   1. Clique em **Ligar**. O **Object Explorer** janela é aberta no SSMS.

1. Na **Object Explorer**, expanda **bases de dados** e, em seguida, expanda *yourDatabase* para ver os objetos na base de dados de exemplo.

   ![objetos da base de dados](./media/sql-database-design-first-database/connected.png)  

## <a name="create-tables-in-the-database"></a>Criar tabelas na base de dados

Crie um esquema de base de dados com quatro tabelas para um sistema de gestão de estudantes para universidades que utilizam o [Transact-SQL](/sql/t-sql/language-reference):

- Pessoa
- Curso
- Estudante
- Crédito

O diagrama seguinte mostra como estas tabelas estão relacionadas entre si. Algumas destas tabelas referenciam colunas noutras tabelas. Por exemplo, o *estudante* referências de tabela a *PersonId* coluna do *pessoa* tabela. Estude o diagrama para compreender a forma como as tabelas neste tutorial estão relacionadas entre si. Para uma visão detalhada de como criar tabelas de base de dados efetivas, veja [Criar tabelas de base de dados efetivas](https://msdn.microsoft.com/library/cc505842.aspx). Para obter informações sobre como selecionar tipos de dados, veja [Tipos de dados](/sql/t-sql/data-types/data-types-transact-sql).

> [!NOTE]
> Também pode utilizar o [estruturador de tabelas no SQL Server Management Studio](/sql/ssms/visual-db-tools/design-database-diagrams-visual-database-tools) para criar e estruturar as suas tabelas.

![Relações de tabelas](./media/sql-database-design-first-database/tutorial-database-tables.png)

1. Na **Object Explorer**, clique com botão direito *yourDatabase* e selecione **nova consulta**. É aberta uma janela de consulta em branco que está ligada à sua base de dados.

1. Na janela da consulta, execute a consulta seguinte para criar quatro tabelas na base de dados:

   ```sql
   -- Create Person table
   CREATE TABLE Person
   (
       PersonId INT IDENTITY PRIMARY KEY,
       FirstName NVARCHAR(128) NOT NULL,
       MiddelInitial NVARCHAR(10),
       LastName NVARCHAR(128) NOT NULL,
       DateOfBirth DATE NOT NULL
   )

   -- Create Student table
   CREATE TABLE Student
   (
       StudentId INT IDENTITY PRIMARY KEY,
       PersonId INT REFERENCES Person (PersonId),
       Email NVARCHAR(256)
   )

   -- Create Course table
   CREATE TABLE Course
   (
       CourseId INT IDENTITY PRIMARY KEY,
       Name NVARCHAR(50) NOT NULL,
       Teacher NVARCHAR(256) NOT NULL
   )

   -- Create Credit table
   CREATE TABLE Credit
   (
       StudentId INT REFERENCES Student (StudentId),
       CourseId INT REFERENCES Course (CourseId),
       Grade DECIMAL(5,2) CHECK (Grade <= 100.00),
       Attempt TINYINT,
       CONSTRAINT [UQ_studentgrades] UNIQUE CLUSTERED
       (
           StudentId, CourseId, Grade, Attempt
       )
   )
   ```

   ![Criar tabelas](./media/sql-database-design-first-database/create-tables.png)

1. Expanda a **tabelas** no nó *yourDatabase* no **Object Explorer** para ver as tabelas que criou.

   ![ssms tables-created](./media/sql-database-design-first-database/ssms-tables-created.png)

## <a name="load-data-into-the-tables"></a>Carregar dados para as tabelas

1. Criar uma pasta denominada *sampleData* na pasta transferências para armazenar dados de exemplo para a base de dados.

1. Com o botão direito as ligações seguintes e guarde-as para o *sampleData* pasta.

   - [SampleCourseData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCourseData)
   - [SamplePersonData](https://sqldbtutorial.blob.core.windows.net/tutorials/SamplePersonData)
   - [SampleStudentData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleStudentData)
   - [SampleCreditData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCreditData)

1. Abra uma janela de linha de comandos e navegue para o *sampleData* pasta.

1. Execute os seguintes comandos para inserir dados de exemplo nas tabelas ao substituir os valores para *servidor*, *base de dados*, *utilizador*, e *depalavra-passe* com os valores para o seu ambiente.
  
   ```cmd
   bcp Course in SampleCourseData -S <server>.database.windows.net -d <database> -U <user> -P <password> -q -c -t ","
   bcp Person in SamplePersonData -S <server>.database.windows.net -d <database> -U <user> -P <password> -q -c -t ","
   bcp Student in SampleStudentData -S <server>.database.windows.net -d <database> -U <user> -P <password> -q -c -t ","
   bcp Credit in SampleCreditData -S <server>.database.windows.net -d <database> -U <user> -P <password> -q -c -t ","
   ```

Carregou dados de exemplo para as tabelas que criou anteriormente.

## <a name="query-data"></a>Consultar dados

Execute as seguintes consultas para obter informações das tabelas de base de dados. Ver [consultas SQL escrever](https://technet.microsoft.com/library/bb264565.aspx) para saber mais sobre como escrever consultas SQL. A primeira consulta associa as quatro tabelas para localizar os estudantes ensinados por "Dominick Pope", que tem uma classificação superior a 75%. A segunda consulta associa as quatro tabelas e localiza os cursos em que "Noe Coleman se" inscreveu.

1. Numa janela de consulta do SQL Server Management Studio, execute a seguinte consulta:

   ```sql
   -- Find the students taught by Dominick Pope who have a grade higher than 75%
   SELECT  person.FirstName, person.LastName, course.Name, credit.Grade
   FROM  Person AS person
       INNER JOIN Student AS student ON person.PersonId = student.PersonId
       INNER JOIN Credit AS credit ON student.StudentId = credit.StudentId
       INNER JOIN Course AS course ON credit.CourseId = course.courseId
   WHERE course.Teacher = 'Dominick Pope'
       AND Grade > 75
   ```

1. Na janela de consulta, execute a seguinte consulta:

   ```sql
   -- Find all the courses in which Noe Coleman has ever enrolled
   SELECT  course.Name, course.Teacher, credit.Grade
   FROM  Course AS course
       INNER JOIN Credit AS credit ON credit.CourseId = course.CourseId
       INNER JOIN Student AS student ON student.StudentId = credit.StudentId
       INNER JOIN Person AS person ON person.PersonId = student.PersonId
   WHERE person.FirstName = 'Noe'
       AND person.LastName = 'Coleman'
   ```

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, aprendeu muitas tarefas de base de dados básica. Aprendeu a:

> [!div class="checklist"]
> * Criar uma base de dados
> * Configurar uma regra de firewall
> * Ligar à base de dados com o [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS)
> * Criar tabelas
> * Carregar dados em massa
> * Consultar os dados

Avance para o tutorial seguinte para aprender a criar uma base de dados com o Visual Studio e C#.

> [!div class="nextstepaction"]
> [Criar uma base de dados SQL do Azure e estabelecer ligação com C# e ADO.NET](sql-database-design-first-database-csharp.md)
