---
title: Como testar seu código do Azure Data Lake Analytics | Documentos da Microsoft
description: Saiba como adicionar casos de teste para o seu U-SQL e o código c# expandido para o Azure Data Lake Analytics.
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
ms.openlocfilehash: bfb314348caf1d5bf83c940c0bce79e87d6d2593
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889675"
---
# <a name="how-to-test-your-azure-data-lake-analytics-code"></a>Como testar seu código do Azure Data Lake Analytics

O Azure Data Lake fornece U-SQL como uma linguagem que combina SQL declarativa com imperativo c# para processar dados em qualquer escala. Neste documento, saiba como criar casos de teste para seu U-SQL e o código do c# UDO (definidas pelo utilizador operador) expandido.

## <a name="test-u-sql-scripts"></a>Testar scripts U-SQL

O script de U-SQL é compilado e otimizado para código executável e executado em máquinas na cloud ou no seu computador local. O processo de compilação e a otimização de processa todo o script de U-SQL como um todo para executar. Não é possível fazer tradicional "teste de unidade" para cada instrução. No entanto, ao utilizar o SDK de execução local e de teste de U-SQL SDK, pode fazer testes de nível de script.

### <a name="create-test-cases-for-u-sql-script"></a>Criar casos de teste de script de U-SQL

O Azure Data Lake Tools para Visual Studio fornece uma boa experiência para o ajudar a criar casos de teste do script de U-SQL.

1.  Faça duplo clique de um script de U-SQL no Solution Explorer e escolha **criar teste de unidade**.
2.  Configure para criar um novo projeto de teste ou insira o caso de teste para um projeto de teste existente.

    ![Ferramentas do Data Lake para Visual Studio crie o projeto de teste de u-sql](./media/data-lake-analytics-cicd-test/data-lake-tools-create-usql-test-project.png) 

    ![Ferramentas do Data Lake para Visual Studio criar configuração de projeto de teste de u-sql](./media/data-lake-analytics-cicd-test/data-lake-tools-create-usql-test-project-configure.png) 

### <a name="manage-test-data-source"></a>Gerir a origem de dados de teste

Quando testar scripts U-SQL, ficheiros de entrada de teste são necessários. Pode gerir estes testar dados configurando **origem de dados de teste** em U-SQL propriedade do projeto. Quando chama o `Initialize()` interface no teste de U-SQL SDK, uma pasta de raiz de dados local temporária é criado no diretório de trabalho do projeto de teste e todos os ficheiros e subpastas (e ficheiros em subpastas) na pasta de origem de dados de teste são copiados para temporário pasta de raiz de dados local antes de executar casos de teste do script de U-SQL. Pode adicionar mais pastas de origem de dados de teste, spitting caminho de pasta de dados de teste com ponto e vírgula.

![Ferramentas do Data Lake para Visual Studio configurar origem de dados de teste do projeto](./media/data-lake-analytics-cicd-test/data-lake-tools-configure-project-test-data-source.png)

### <a name="manage-database-environment-for-test"></a>Gerir o ambiente de base de dados para teste

Se seus scripts U-SQL utilizam ou consultar com objetos de banco de dados U-SQL, por exemplo, chamar procedimentos armazenados, em seguida, terá de inicializar o ambiente de base de dados antes de executar casos de teste de U-SQL. O `Initialize()` interface no SDK do teste de U-SQL ajuda-o a implementar todas as bases de dados que são referenciados pelo projeto U-SQL para a pasta de raiz de dados local temporário no diretório de trabalho do projeto de teste. 

Saiba mais sobre [como pode gerir a base de dados U-SQL projetos referências de projeto U-SQL](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project).

### <a name="verify-test-results"></a>Verifique se os resultados do teste

O `Run()` interface devolve o resultado da execução de tarefa, 0 significa êxito e 1 significa falha. Também pode utilizar funções de assert do c# para verificar as saídas. 

### <a name="execute-test-cases-in-visual-studio"></a>Executar casos de teste no Visual Studio

Projeto de teste do script de U-SQL é criado sobre estrutura de teste c# unidade. Após a compilação do projeto, pode percorrer todos os casos de teste **Test Explorer > lista de reprodução**, ou com o botão direito do arquivo. cs e escolha **executar testes**.

## <a name="test-c-udos"></a>Teste c# UDOs

### <a name="create-test-cases-for-c-udos"></a>Criar casos de teste para c# UDOs

Pode utilizar a estrutura de teste c# unidade para testar sua UDOs(User-Defined Operator) c#. Ao testar UDOs, tem de preparar correspondente **IRowset** objeto como entradas.

Existem duas formas de criar IRowset:

1.  Carregar dados a partir de um ficheiro para criar IRowset

    ```csharp
    //Schema: "a:int, b:int"
    USqlColumn<int> col1 = new USqlColumn<int>("a");
    USqlColumn<int> col2 = new USqlColumn<int>("b");
    List<IColumn> columns = new List<IColumn> { col1, col2 };
    USqlSchema schema = new USqlSchema(columns);

    //Generate one row with default values
    IUpdatableRow output = new USqlRow(schema, null).AsUpdatable();

    //Get data from file
    IRowset rowset = UnitTestHelper.GetRowsetFromFile(@"processor.txt", schema, output.AsReadOnly(), discardAdditionalColumns: true, rowDelimiter: null, columnSeparator: '\t');
    ```

2.  Utilizar dados de recolha de dados para criar IRowset

    ```csharp
    //Schema: "a:int, b:int"
    USqlSchema schema = new USqlSchema(
        new USqlColumn<int>("a"),
        new USqlColumn<int>("b")
    );

    IUpdatableRow output = new USqlRow(schema, null).AsUpdatable();

    //Generate Rowset with specified values
    List<object[]> values = new List<object[]>{
        new object[2] { 2, 3 },
        new object[2] { 10, 20 }
    };

    IEnumerable<IRow> rows = UnitTestHelper.CreateRowsFromValues(schema, values);
    IRowset rowset = UnitTestHelper.GetRowsetFromCollection(rows, output.AsReadOnly());
    ```

### <a name="verify-test-results"></a>Verifique se os resultados do teste

Depois de chamar funções UDO, pode verificar o resultado por meio de esquema e a verificação de valor de conjunto de linhas com as funções c# de declaração. Código de exemplo pode estar em U-SQL c# UDO unidade teste projeto de exemplo através de **ficheiro > novo > projeto** no Visual Studio.

### <a name="execute-test-cases-in-visual-studio"></a>Executar casos de teste no Visual Studio

Depois de criar o projeto de teste, pode executar todos os casos de teste entanto **Test Explorer > lista de reprodução**, ou com o botão direito do arquivo. cs e escolha **executar testes**.

## <a name="run-test-cases-in-visual-studio-team-service"></a>Executa casos de teste no Visual Studio Team Service

Ambos **projeto de teste do script de U-SQL** e **projeto de teste do c# UDO** herdar c# projeto teste de unidade. [Tarefa de teste do Studio Visual](https://docs.microsoft.com/vsts/pipelines/test/getting-started-with-continuous-testing?view=vsts) no Visual Studio Team Service pode executar os casos de teste. 

### <a name="run-u-sql-test-cases-in-visual-studio-team-service"></a>Executa casos de teste de U-SQL no Visual Studio Team Service

Para teste de U-SQL, certifique-se de que carregue `CPPSDK` no seu computador de compilação e passe o `CPPSDK` caminho para USqlScriptTestRunner (cppSdkFolderFullPath: @"").

**O que é CPPSDK?**

CPPSDK é que um pacote inclui o Microsoft Visual C++ 14 e SDK do Windows 10.0.10240.0, que é o ambiente necessário pelo tempo de execução do U-SQL. Pode obter esse pacote em Azure Data Lake Tools para a pasta de instalação do Visual Studio:

- Para o Visual Studio 2015, está sob `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\Microsoft Azure Data Lake Tools for Visual Studio 2015\X.X.XXXX.X\CppSDK`
- Para Visual Studio 2017, é em `C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\SDK\ScopeCppSDK`

**Como preparar CPPSDK no agente de compilação do Visual Studio Team Service**

A forma comum de preparação esta dependência CPPSDK no Visual Studio Team Service é:

1.  Zip na pasta inclui bibliotecas de CPPSDK.
2.  Devolver o arquivo zip para o sistema de controle de origem. (Arquivo zip pode verificar se fizer check-in todas as bibliotecas na pasta CPPSDK ou alguns ficheiros serão ignorados pelo ". gitignore".)
3.  Deszipe o ficheiro zip no pipeline de compilação.
4.  Ponto `USqlScriptTestRunner` para esta pasta descompactada na máquina de compilação.

### <a name="run-c-udo-test-cases-in-visual-studio-team-service"></a>Executa casos de teste de UDO c# no Visual Studio Team Service

Para teste de c# UDO, certifique-se fazer referência abaixo assemblies que são necessários para UDOs. Se fizer referência através de [o pacote de Nuget Microsoft.Azure.DataLake.USQL.Interfaces](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.Interfaces/), certifique-se de adicionar uma tarefa de restauro do NuGet no seu pipeline de compilação.

* Microsoft.Analytics.Interfaces
* Microsoft.Analytics.Types
* Microsoft.Analytics.UnitTest

## <a name="next-steps"></a>Próximos Passos

- [Como configurar o pipeline CI/CD para o Azure Data Lake Analytics](data-lake-analytics-cicd-overview.md)
- [Execute o script de U-SQL no seu computador local](data-lake-analytics-data-lake-tools-local-run.md)
- [Utilizar o projeto de banco de dados U-SQL para desenvolver a base de dados U-SQL](data-lake-analytics-data-lake-tools-develop-usql-database.md)

