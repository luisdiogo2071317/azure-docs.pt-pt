---
title: Melhor prática de gerir os assemblies de U-SQL no pipeline de CI/CD para o Azure Data Lake
description: Saiba a melhor prática de gerir o U-SQL C# pipeline de assemblies no CI/CD com o Azure DevOps.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: ''
ms.assetid: ''
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 10/30/2018
ms.openlocfilehash: 0f6f581d90b025dd538eb9b79da1a0addd4259f7
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50634609"
---
# <a name="best-practice-of-managing-u-sql-assemblies-in-cicd-pipeline"></a>Melhor prática de gerir os assemblies de U-SQL no pipeline CI/CD

Neste artigo, irá aprender a gerenciar código-fonte assembly U-SQL com o novo projeto de banco de dados U-SQL introduzido. Também pode saber como configurar um pipeline de implementação (CI/CD) para o registo de assemblagem através do Azure DevOps e integração contínua.

## <a name="use-the-u-sql-database-project-to-manage-assembly-source-code"></a>Utilizar o projeto de banco de dados U-SQL para gerir o código de origem da assemblagem

[O projeto de banco de dados U-SQL](data-lake-analytics-data-lake-tools-develop-usql-database.md) é um tipo de projeto no Visual Studio que ajuda os desenvolvedores a desenvolver, gerir e implementar seus bancos de dados U-SQL de forma rápida e fácil. Todos os objetos de base de dados de U-SQL (exceto para credenciais) podem ser geridos com o projeto de banco de dados U-SQL. 

A forma recomendada de gerir o C# assembly código e o assembly registo DDL U-SQL scripts de origem é:

* Uso **projeto de banco de dados U-SQL** para gerir scripts do U-SQL de registo de assemblagem.
* Uso **biblioteca de classes (para aplicação U-SQL)** para gerir o C# da origem de código e as dependências para operadores definidos pelo utilizador, funções e agregadores (UDOs/UDFs/UDAGs).
* Utilize o projeto de banco de dados U-SQL para o projeto de biblioteca de classes de referência. 

Um projeto de banco de dados U-SQL, pode referenciar um projeto de biblioteca de classe (para aplicação U-SQL). Assemblies registrados na base de dados U-SQL podem ser criadas usando a referenciada C# origem de código deste projeto de biblioteca de classes (para aplicação U-SQL).

Pode seguir os passos de criação de projetos e adição de referências abaixo:
1. Criar um projeto de biblioteca de classes (para aplicação U-SQL) por meio **ficheiro > novo > projeto**. O projeto está sob **do Azure Data Lake > U-SQL** nó.
   ![Data Lake Tools para Visual Studio-- criar C# projeto de biblioteca de classe](./media/data-lake-analytics-cicd-manage-assemblies/create-c-sharp-class-library-project.png)
2. Adicionar a sua definidas pelo utilizador C# código no projeto da biblioteca de classe (para aplicação U-SQL). 
3. Criar um projeto U-SQL através de **ficheiro > novo > projeto**. O projeto está sob **do Azure Data Lake > U-SQL** nó.
   ![Projeto de banco de dados do Data Lake Tools para Visual Studio--criar U-SQL](media/data-lake-analytics-cicd-manage-assemblies/create-u-sql-database-project.png)
4. Adicionar a referência para o C# projeto de biblioteca de classes para o projeto de banco de dados U-SQL.

    ![Referência do projeto de banco de dados do Data Lake Tools para Visual Studio – adicionar U-SQL](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-add-project-reference.png) 

    ![Referência do projeto de banco de dados do Data Lake Tools para Visual Studio – adicionar U-SQL](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-add-project-reference-wizard.png)
5. Criar um script de assembly no projeto de banco de dados U-SQL através de **clique com o botão direito do rato em projeto > Adicionar Novo Item**.
   ![Data Lake Tools para Visual Studio, adicione o script de assemblagem](media/data-lake-analytics-cicd-manage-assemblies/add-assembly-script.png)
6. Abra o script de assembly, na vista de estrutura de assembly, escolha a assemblagem referenciada a partir **criar o assembly de referência** menu pendente.

    ![Ferramentas do Data Lake para Visual Studio-- criar o assembly de referência](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-create-assembly-from-reference.png)

7. Adicione **dependências geridos** e **ficheiros adicionais** se existir alguma. Ao adicionar ficheiros adicionais, a ferramenta usa o caminho relativo para se certificar de que ele possa encontrar os assemblies no seu computador local e na máquina de compilação mais tarde. 

**@_DeployTempDirectory** no editor de janela na parte inferior é uma variável predefinida que aponta a ferramenta para a pasta de saída da compilação. Sob a pasta de saída da compilação, a cada assembly possui uma subpasta com o nome com o nome do assembly. Todas as DLLs e ficheiros adicionais são nessa subpasta. 

## <a name="build-a-u-sql-database-project"></a>Criar um projeto de banco de dados U-SQL

A compilação de saída para um projeto de banco de dados U-SQL é um pacote de implementação da base de dados U-SQL, com o nome com o sufixo `.usqldbpack`. O `.usqldbpack` pacote é um arquivo. zip que inclui todas as instruções DDL num único script de U-SQL no **DDL** pasta e criados de todas as dlls e outros ficheiros para os assemblies no **Temp** pasta.

## <a name="deploy-a-u-sql-database"></a>Implementar uma base de dados U-SQL

O `.usqldbpack` pacote pode ser implementado para uma conta local ou uma conta do Azure Data Lake Analytics com o Visual Studio ou a implementação do SDK. 

### <a name="deploy-a-u-sql-database-in-visual-studio"></a>Implementar uma base de dados U-SQL no Visual Studio

Pode implementar uma base de dados U-SQL através de um projeto de banco de dados U-SQL ou um `.usqldbpack` pacote no Visual Studio.

#### <a name="deploy-through-a-u-sql-database-project"></a>Implementar através de um projeto de banco de dados U-SQL

1.  O projeto de banco de dados U-SQL com o botão direito e, em seguida, selecione **Deploy**.
2.  Na **Assistente de base de dados de implementação de U-SQL**, selecione a **conta ADLA** para a qual pretende implementar a base de dados. Contas locais e contas ADLA são suportadas.
3.  **Base de dados de origem** é preenchida automaticamente, e a pasta de saída de compilação de pontos para o pacote de .usqldbpack do projeto.
4.  Introduza um nome na **nome da base de dados** para criar uma base de dados. Se já existir uma base de dados com o mesmo nome no destino conta do Azure Data Lake Analytics, todos os objetos que estão definidos no projeto de banco de dados são criados sem recriar a base de dados.
5.  Para implementar a base de dados U-SQL, selecione **submeter**. Todos os recursos (assemblies e arquivos adicionais) são carregados e é submetida uma tarefa de U-SQL que inclui todas as instruções DDL.

    ![Projeto de banco de dados do Data Lake Tools para Visual Studio – implemente U-SQL](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-deploy-usql-database-project.png)

    ![Assistente de projeto de banco de dados do Data Lake Tools para Visual Studio – implemente U-SQL](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-deploy-usql-database-project-wizard.png)

### <a name="deploy-u-sql-database-in-azure-devops"></a>Implementar a base de dados U-SQL no Azure DevOps

`PackageDeploymentTool.exe` Fornece as interfaces de linha de comandos que ajudam a implementar bases de dados U-SQL e programação. O SDK está incluído nos [pacote Nuget do SDK U-SQL](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/), localizado em `build/runtime/PackageDeploymentTool.exe`.

Em DevOps do Azure, pode utilizar tarefas de linha de comandos e este SDK para configurar os pipelines de automatização para a atualização da base de dados U-SQL. [Saiba mais sobre o SDK e como configurar o pipeline de CI/CD para a implementação de base de dados U-SQL](data-lake-analytics-cicd-overview.md#deploy-u-sql-database-through-azure-pipelines).

## <a name="next-steps"></a>Passos Seguintes

* [Como configurar um pipeline CI/CD para o Azure Data Lake Analytics](data-lake-analytics-cicd-overview.md)
* [Como testar seu código do Azure Data Lake Analytics](data-lake-analytics-cicd-test.md)
* [Execute o script de U-SQL no seu computador local](data-lake-analytics-data-lake-tools-local-run.md)
