---
title: Projeto de banco de dados de U-SQL de uso para desenvolver a base de dados U-SQL para o Azure Data Lake | Documentos da Microsoft
description: Saiba como desenvolver o banco de dados U-SQL com o Azure Data Lake Tools para Visual Studio.
services: data-lake-analytics
documentationcenter: ''
author: yanancai
manager: ''
editor: ''
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/03/2018
ms.author: yanacai
ms.openlocfilehash: a8099a8c9bcec8ed5a7bb480dae58d2c077b8fe0
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889682"
---
# <a name="use-u-sql-database-project-to-develop-u-sql-database-for-azure-data-lake"></a>Utilizar o projeto de banco de dados U-SQL para desenvolver a base de dados U-SQL para o Azure Data Lake

Base de dados U-SQL fornece vistas estruturadas sobre os dados não estruturados, dados estruturados em tabelas de gerir e fornece um sistema de catálogo de metadados geral para organizar os seus dados estruturados e o código personalizado. A base de dados é o conceito que agrupa esses objetos relacionados.

Saiba mais sobre [base de dados U-SQL e de linguagem DDL (Data Definition)](https://msdn.microsoft.com/azure/data-lake-analytics/u-sql/data-definition-language-ddl-statements-u-sql). 

Projeto de banco de dados U-SQL é um tipo de projeto no Visual Studio que ajuda os desenvolvedores a desenvolver, gerir e implementar seus bancos de dados U-SQL, rápida e facilmente.

## <a name="create-a-u-sql-database-project"></a>Criar um projeto de banco de dados U-SQL

O Azure Data Lake Tools para Visual Studio adicionado um novo modelo de projeto chamado projeto de banco de dados U-SQL após a versão 2.3.3000.0. Para criar um projeto U-SQL, percorrer **ficheiro > novo > projeto**, o projeto de banco de dados U-SQL pode ser encontrado na **do Azure Data Lake > nó de U-SQL**.

![Ferramentas do Data Lake para Visual Studio criar o projeto de banco de dados u-sql](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-create-usql-database-project-creation.png) 

## <a name="develop-u-sql-database-objects-using-database-project"></a>Desenvolver os objetos de banco de dados U-SQL com o projeto de banco de dados

O projeto de banco de dados U-SQL com o botão direito, clique em **adicionar > novo item**, todos com suporte a tipos de objeto podem ser encontrados no Assistente para adicionar novo Item. 

1.  Para os assemblies não objeto, por exemplo, função com valor de tabela, um novo script de U-SQL é criado depois de adicionar novo item. Pode começar a desenvolver a instrução DDL para esse objeto no editor.
2.  Para o objeto de assembly, a ferramenta fornece um editor de interface do Usuário amigável que ajuda a registrar o assembly e implantar. dll e arquivos adicionais. Siga os passos para adicionar uma definição de objeto de assemblagem para o projeto de banco de dados U-SQL abaixo:

1.  Adicione referências de projeto c# que inclui a UDO/UDAG/UDF para o projeto de banco de dados U-SQL.

    ![Ferramentas do Data Lake para Visual Studio Adicionar referência de projeto de banco de dados u-sql](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-project-reference.png) 

    ![Ferramentas do Data Lake para Visual Studio Adicionar referência de projeto de banco de dados u-sql](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-project-reference-wizard.png)

2.  Na vista de estrutura de assembly, escolha a assemblagem referenciada a partir **criar o assembly de referência** pendente.

    ![Ferramentas do Data Lake para Visual Studio criar o assembly de referência](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-create-assembly-from-reference.png)

3.  Adicione **geridos dependências** e **ficheiros adicionais** se houver. Ao adicionar ficheiros adicionais, a ferramenta irá utilizar o caminho relativo para se certificar de que ele possa encontrar os assemblies no seu computador local e a máquina de compilação mais tarde. @_DeployTempDirectory é uma variável predefinida que aponta a ferramenta para a pasta de saída da compilação. Na saída da compilação, a cada assembly possui uma subpasta com o nome porque o nome do assembly, todas as dlls e ficheiros adicionais podem ser encontrados aqui na subpasta. 
 
## <a name="build-u-sql-database-project"></a>Criar projeto de banco de dados U-SQL

A compilação de saída para o projeto de banco de dados U-SQL é um pacote de implementação da base de dados U-SQL, com o nome com o sufixo `.usqldbpack`. O `.usqldbpack` pacote é um ficheiro zip inclui todas as instruções DDL num único script de U-SQL no **DDL** pasta e todas as dlls e outros ficheiros para os assemblies no **Temp** pasta.

Saiba mais sobre [como criar o projeto de banco de dados U-SQL com a linha de comando do MSBuild e tarefas de compilação do Visual Studio Team Service](data-lake-analytics-cicd-overview.md#build-u-sql-database-project).

## <a name="deploy-u-sql-database"></a>Implementar a base de dados U-SQL

O pacote de .usqldbpack pode ser implementado para a conta local ou conta do Azure Data Lake Analytics com o Visual Studio ou a implementação do SDK. 

### <a name="deploy-u-sql-database-in-visual-studio"></a>Implementar a base de dados U-SQL no Visual Studio

Pode implementar uma base de dados U-SQL através de um projeto de banco de dados U-SQL ou um pacote de .usqldbpack no Visual Studio.

#### <a name="deploy-through-u-sql-database-project"></a>Implementar através do projeto de banco de dados U-SQL

1.  O projeto de banco de dados U-SQL com o botão direito e escolher **Deploy**.
2.  No Assistente para implementar a base de dados de U-SQL, selecione o **conta ADLA** gostaria de implementar a base de dados. Conta (Local) e conta ADLA são suportados.
3.  **Base de dados de origem** são preenchidos automaticamente a apontar para o pacote de .usqldbpack na pasta de saída de compilação do projeto
4.  Introduza **nome da base de dados** para a criação de uma base de dados. Se existir uma base de dados com o mesmo existente no destino conta do Azure Data Lake Analytics, todos os objetos definidos no projeto de banco de dados são criados sem recriar a base de dados.
5.  Clique em **submeter** para implementar a base de dados U-SQL. Todos os recursos (assemblies e arquivos adicionais) são carregados e uma tarefa U-SQL inclui todas as instruções DDL são submetidas.

    ![Ferramentas do Data Lake para Visual Studio implementar o projeto de banco de dados u-sql](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-project.png)

    ![Assistente de projeto do u-sql da base de dados de implementação de ferramentas do Data Lake para Visual Studio](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-project-wizard.png)

#### <a name="deploy-through-u-sql-database-deployment-package"></a>Implementar através do pacote de implementação do U-SQL da base de dados

1.  Open **Explorador de servidores**, expanda o **conta do Azure Data Lake Analytics** gostaria de implementar a base de dados.
2.  Clique com o botão direito do rato em bases de dados U-SQL e escolha **implementar a base de dados**.
3.  Definir **da base de dados de origem** para o caminho de pacote (ficheiro .usqldbpack) de implementação do U-SQL da base de dados.
4.  Introduza o **nome da base de dados** para a criação de uma base de dados. Se existir uma base de dados com o mesmo existente no destino conta do Azure Data Lake Analytics, todos os objetos que estão definidos no projeto de banco de dados são criados sem recriar a base de dados.

    ![Ferramentas do Data Lake para Visual Studio implementar o pacote do u-sql da base de dados](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-package.png)

    ![Assistente de pacote do u-sql da base de dados de implementação de ferramentas do Data Lake para Visual Studio](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-package-wizard.png)
  
### <a name="deploy-u-sql-database-using-sdk"></a>Implementar a base de dados U-SQL com o SDK

`PackageDeploymentTool.exe` Fornece as interfaces de linha de comandos que ajudam a implementar bases de dados U-SQL e programação. O SDK está incluído nos [pacote Nuget do SDK U-SQL](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/), localizar em `build/runtime/PackageDeploymentTool.exe`.

[Saiba mais sobre o SDK e como configurar o pipeline de CI/CD para a implementação de base de dados U-SQL](data-lake-analytics-cicd-overview.md#deploy-u-sql-database-through-visual-studio-team-service).

## <a name="reference-a-u-sql-database-project"></a>Referenciar um projeto de banco de dados U-SQL

Um projeto U-SQL, pode referenciar um projeto de banco de dados U-SQL. A referência afeta duas cargas de trabalho:

- Compilação do projeto: os ambientes de banco de dados referenciada são configurados antes de compilar os scripts U-SQL. 
- Local a executar com a conta (projeto Local): os ambientes de banco de dados referenciada são implementados para a conta (projeto Local) antes da execução de script de U-SQL. [Saiba mais sobre a execução local e a diferença entre (máquina Local) e a conta (projeto Local) aqui](data-lake-analytics-data-lake-tools-local-run.md).

### <a name="how-to-add-u-sql-database-reference"></a>Como adicionar referência de base de dados U-SQL

1. Com o botão direito no projeto U-SQL no **Explorador de soluções**e escolha **Adicionar referência do U-SQL da base de dados...** .

    ![Ferramentas do Data Lake para Visual Studio Adicionar referência de projeto de banco de dados](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-database-project-reference.png)

2. Configure a referência de bases de dados de um projeto de banco de dados U-SQL na solução atual ou um ficheiro de pacote do U-SQL da base de dados.
3. Forneça o nome da base de dados.

    ![Ferramentas do Data Lake para Visual Studio Adicionar Assistente de referência do projeto de banco de dados](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-database-project-reference-wizard.png)

## <a name="next-steps"></a>Próximos Passos

- [Como configurar o pipeline CI/CD para o Azure Data Lake Analytics](data-lake-analytics-cicd-overview.md)
- [Como testar seu código do Azure Data Lake Analytics](data-lake-analytics-cicd-test.md)
- [Execute o script de U-SQL no seu computador local](data-lake-analytics-data-lake-tools-local-run.md)
