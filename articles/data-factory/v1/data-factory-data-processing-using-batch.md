---
title: Processar os conjuntos de dados em grande escala utilizando o Data Factory e Batch | Microsoft Docs
description: Descreve como processar quantidades enormes de dados de um pipeline do Azure Data Factory utilizando o capacidade de lote do Azure de processamento de paralelo.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 688b964b-51d0-4faa-91a7-26c7e3150868
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: spelluru
robots: noindex
ms.openlocfilehash: af2c12cac5846ae1c4bc693bacaf72ab327fb87f
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="process-large-scale-datasets-by-using-data-factory-and-batch"></a>Conjuntos de dados em grande escala do processo através da utilização de fábrica de dados e de Batch
> [!NOTE]
> Este artigo aplica-se versão 1 do Azure Data Factory, que é geralmente disponível. Se utilizar a versão 2 do serviço do Data Factory, o que está em pré-visualização, consulte [atividades personalizadas no Factory de dados versão 2](../transform-data-using-dotnet-custom-activity.md).

Este artigo descreve uma arquitetura de uma solução de exemplo que se move e processa os conjuntos de dados em grande escala de forma automática e agendada. Também fornece uma instruções ponto a ponto para implementar a solução utilizando o Data Factory e do Azure Batch.

Este artigo é maior do que um artigo típico porque contém uma explicação passo a passo de uma solução de exemplo completo. Se estiver familiarizado com o Batch e do Data Factory, pode saber sobre estes serviços e como funcionam em conjunto. Se souber algo sobre os serviços e são estruturar/arquitetar uma solução, poder concentrar no [secção de arquitetura](#architecture-of-sample-solution) do artigo. Se estiver a desenvolver um protótipo ou uma solução, poderá querer experimentar as instruções passo a passo o [instruções](#implementation-of-sample-solution). Convidamo-os seus comentários sobre este conteúdo e como utilizá-lo.

Em primeiro lugar, vamos ver como os serviços de fábrica de dados e Batch podem ajudá-lo processo grandes conjuntos de dados na nuvem.     

## <a name="why-azure-batch"></a>Por que motivo o Azure Batch?
 Pode utilizar o Batch para executar de forma eficiente em grande escala paralelas e elevado desempenho aplicações de computação (HPC) e na nuvem. É um serviço de plataforma que agenda trabalho de computação intensiva para executar numa coleção gerida de máquinas virtuais (VMs). Pode dimensionar automaticamente recursos de computação para satisfazer as necessidades das suas tarefas.

Com o serviço Batch, define os recursos de computação do Azure para executar as aplicações em paralelo e com dimensionamento. Pode executar a pedido ou tarefas agendadas. Não precisa de criar, configurar e gerir um cluster HPC, VMs individuais, redes virtuais, ou uma tarefa complexa e infraestrutura de agendamento de tarefas manualmente.

 Se não estiver familiarizado com o Batch, os artigos seguintes ajudá-lo a compreender a arquitetura de implementação da solução descrita neste artigo:   

* [Noções básicas do Batch](../../batch/batch-technical-overview.md)
* [Descrição geral da funcionalidade do Batch](../../batch/batch-api-basics.md)

Opcionalmente, para saber mais sobre o Batch, consulte o artigo [percurso de aprendizagem do lote](https://azure.microsoft.com/documentation/learning-paths/batch/).

## <a name="why-azure-data-factory"></a>Por que motivo fábrica de dados do Azure?
Data Factory é um serviço de integração de dados baseado na nuvem que orquestra e automatiza o movimento e a transformação de dados. Pode utilizar o Data Factory para criar pipelines de dados geridos que movem os dados no local e na nuvem arquivos de dados para um arquivo de dados centralizada. Um exemplo é Blob storage do Azure. Pode utilizar o Data Factory para processo/transformação dados utilizando os serviços como o Azure HDInsight e o Azure Machine Learning. Também pode agendar pipelines de dados para ser executada de forma agendada (por exemplo, a hora a hora, diariamente e a semanal). Pode monitorizar e gerir pipelines rapidamente para identificar problemas e tomar medidas.

  Se não estiver familiarizado com o Data Factory, os artigos seguintes ajudá-lo a compreender a arquitetura de implementação da solução descrita neste artigo:  

* [Introdução ao Data Factory](data-factory-introduction.md)
* [Criar o seu primeiro pipeline de dados](data-factory-build-your-first-pipeline.md)   

Opcionalmente, para saber mais sobre o Data Factory, consulte o artigo [percurso de aprendizagem do Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/).

## <a name="data-factory-and-batch-together"></a>Fábrica de dados e em conjunto do Batch
Fábrica de dados inclui atividades incorporadas. Por exemplo, a atividade de cópia é utilizada para copiar/mover dados de um arquivo de dados de origem para um arquivo de dados de destino. A atividade do ramo de registo é utilizada para processar os dados através da utilização de clusters do Hadoop (HDInsight) no Azure. Para obter uma lista de atividades de transformação suportados, consulte [atividades de transformação de dados](data-factory-data-transformation-activities.md).

Também pode criar atividades personalizadas do .NET para mover ou processar os dados com a sua própria lógica. Pode executar estas atividades num cluster do HDInsight ou um conjunto do Batch de VMs. Quando utilizar o Batch, pode configurar o conjunto de dimensionamento automático (adicionar ou remover VMs com base na carga de trabalho) com base numa fórmula que fornecer.     

## <a name="architecture-of-a-sample-solution"></a>Arquitetura de uma solução de exemplo
  A arquitetura descrita neste artigo é uma solução simples. Também é relevante para cenários complexos, tais como modelação por serviços financeiros, processamento de imagem e composição e análise genomic riscos.

O diagrama ilustra como o Data Factory orquestra movimento de dados e processamento. Também mostra como o Batch processa os dados de forma paralela. Transferir e imprimir o diagrama para fácil referência (11 x 17 polegadas ou tamanho A3). Para aceder ao diagrama para que a poder imprimir, consulte [orchestration HPC e dados utilizando o Batch e do Data Factory](http://go.microsoft.com/fwlink/?LinkId=717686).

[![Diagrama de processamento de dados em grande escala](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)

A lista seguinte fornece os passos básicos do processo. A solução inclui código e explicações para compilar a solução ponto-a-ponto.

* **Configure o Batch com um conjunto de nós de computação (VMs).** Pode especificar o número de nós e o tamanho de cada nó.

* **Criar uma instância de fábrica de dados** que está configurado com entidades que representam o blob storage, o serviço de computação do Batch, dados de entrada/saída e um fluxo de trabalho pipeline com atividades que mover e transforme dados.

* **Crie uma atividade personalizada do .NET no pipeline de fábrica de dados.** A atividade é o código de utilizador que é executado o conjunto do Batch.

* **Armazenar grandes quantidades de dados de entrada que os blobs no armazenamento do Azure.** Dados estão divididos em setores lógicas (normalmente por hora).

* **Fábrica de dados copia os dados que estão a ser processados em paralelo** para a localização secundária.

* **Fábrica de dados é executada a atividade personalizada através do agrupamento de atribuído pelo Batch.** Fábrica de dados pode ser executados atividades em simultâneo. Cada atividade processa um setor de dados. Os resultados são armazenados no armazenamento.

* **Fábrica de dados move os resultados finais para uma localização terceira,** para distribuição através de uma aplicação ou para processamento adicional por outras ferramentas.

## <a name="implementation-of-the-sample-solution"></a>Implementação da solução de exemplo
A solução de exemplo é intencionalmente simple. Foi concebido para lhe mostrar como utilizar o Data Factory e lote em conjunto para conjuntos de dados do processo. A solução de conta o número de ocorrências do termo de pesquisa "Microsoft" nos ficheiros de entrada que estão organizados por uma série de tempo. -Lo, em seguida, devolve a contagem de ficheiros de saída.

**Hora:** se estiver familiarizado com as noções básicas do Azure, a fábrica de dados e o Batch e concluiu os seguintes pré-requisitos, esta solução assume uma ou duas horas a concluir.

### <a name="prerequisites"></a>Pré-requisitos
#### <a name="azure-subscription"></a>Subscrição do Azure
Se não tiver uma subscrição do Azure, pode criar rapidamente uma conta de avaliação gratuita. Para obter mais informações, consulte [avaliação gratuita](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="azure-storage-account"></a>Conta de armazenamento do Azure
Utilize uma conta do storage para armazenar os dados neste tutorial. Se não tiver uma conta de armazenamento, consulte [criar uma conta de armazenamento](../../storage/common/storage-create-storage-account.md#create-a-storage-account). A solução de exemplo utiliza o blob storage.

#### <a name="azure-batch-account"></a>Conta de Batch do Azure
Criar uma conta do Batch, utilizando o [portal do Azure](http://portal.azure.com/). Para obter mais informações, consulte [criar e gerir uma conta do Batch](../../batch/batch-account-create-portal.md). Tenha em atenção a chave de conta e o nome da conta de Batch. Também pode utilizar o [New-AzureRmBatchAccount](https://msdn.microsoft.com/library/mt603749.aspx) cmdlet para criar uma conta do Batch. Para obter instruções sobre como utilizar este cmdlet, consulte [introdução aos cmdlets do Batch PowerShell](../../batch/batch-powershell-cmdlets-get-started.md).

A solução de exemplo utiliza o Batch (indiretamente através de um pipeline de fábrica de dados) para processar dados de forma parallel num conjunto de nós de computação (uma coleção de VMs geridos).

#### <a name="azure-batch-pool-of-virtual-machines"></a>Conjunto do Batch do Azure de máquinas virtuais
Crie um conjunto do Batch com, pelo menos, dois nós de computação.

1. No [portal do Azure](https://portal.azure.com), selecione **procurar** no menu à esquerda e selecione **contas do Batch**.

2. Selecione a sua conta de Batch para abrir o **conta do Batch** painel.

3. Selecione o **agrupamentos** mosaico.

4. No **agrupamentos** painel, selecione o **adicionar** botão na barra de ferramentas para adicionar um conjunto.

   a. Introduza um ID para o conjunto (**ID do conjunto**). Tenha em atenção o ID do conjunto. Apenas quando criar a solução de fábrica de dados.

   b. Especifique **Windows Server 2012 R2** para o **família de sistemas operativos** definição.

   c. Selecione um **escalão de preço de nó**.

   d. Introduza **2** como o valor para o **destino dedicado** definição.

   e. Introduza **2** como o valor para o **máx. tarefas por nó** definição.

   f. Selecione **OK** para criar o conjunto.

#### <a name="azure-storage-explorer"></a>Explorador do Storage do Azure
Utilizar [6 de Explorador de armazenamento do Azure](https://azurestorageexplorer.codeplex.com/) ou [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (a partir do ClumsyLeaf Software) para inspecionar e alterar os dados nos seus projetos de armazenamento. Também pode inspecionar e alterar os dados nos registos das suas aplicações alojadas na nuvem.

1. Criar um contentor com o nome **mycontainer** com acesso privado (sem acesso anónimo).

2. Se utilizar CloudXplorer, crie pastas e subpastas com a estrutura seguinte:

   ![Estrutura de pasta e subpasta](./media/data-factory-data-processing-using-batch/image3.png)

   `Inputfolder`e `outputfolder` são pastas de nível superior no `mycontainer`. O `inputfolder` pasta com subpastas com carimbos de data / hora (AAAA-MM-DD-HH).

   Se utilizar o Explorador de armazenamento, no próximo passo, carregar ficheiros com os seguintes nomes: `inputfolder/2015-11-16-00/file.txt`, `inputfolder/2015-11-16-01/file.txt`, e assim sucessivamente. Este passo cria automaticamente as pastas.

3. Criar um ficheiro de texto **file.txt** no seu computador com o conteúdo que tenha a palavra-chave **Microsoft**. Um exemplo é "atividades personalizadas do atividade personalizada Microsoft teste Microsoft de teste."

4. Carregar o ficheiro para as seguintes pastas de entrada no armazenamento de BLOBs:

   ![Pastas de entrada](./media/data-factory-data-processing-using-batch/image4.png)

   Se utilizar o Explorador de armazenamento, carregue o **file.txt** do ficheiro para **mycontainer**. Selecione **cópia** na barra de ferramentas para criar uma cópia do blob. No **copiar Blob** caixa de diálogo, altere o **nome do blob de destino** para `inputfolder/2015-11-16-00/file.txt`. Repita este passo para criar `inputfolder/2015-11-16-01/file.txt`, `inputfolder/2015-11-16-02/file.txt`, `inputfolder/2015-11-16-03/file.txt`, `inputfolder/2015-11-16-04/file.txt`, e assim sucessivamente. Esta ação cria automaticamente as pastas.

5. Criar outro contentor com o nome `customactivitycontainer`. Carregar o ficheiro zip de atividade personalizado para este contentor.

#### <a name="visual-studio"></a>Visual Studio
Instale o Visual Studio 2012 ou posterior, ao criar a atividade de lote personalizada para ser utilizado na solução de fábrica de dados.

### <a name="high-level-steps-to-create-the-solution"></a>Passos de alto nível para criar a solução
1. Crie uma atividade personalizada que contém a lógica de processamento de dados.

2. Crie uma fábrica de dados que utiliza a atividade personalizada.

### <a name="create-the-custom-activity"></a>Criar a atividade personalizada
A atividade personalizada da fábrica de dados é o heart desta solução de exemplo. A solução de exemplo utiliza o Batch para executar a atividade personalizada. Para obter informações sobre como desenvolver atividades personalizadas e utilizá-los pipelines de fábrica de dados, consulte [utilizar atividades personalizadas num pipeline de fábrica de dados](data-factory-use-custom-activities.md).

Para criar uma atividade personalizada do .NET que pode utilizar um pipeline de fábrica de dados, criar um projeto de biblioteca de classe de .NET com uma classe que implementa a interface IDotNetActivity. Esta interface tem apenas um método: executar. Segue-se a assinatura do método:

```csharp
public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices,
            IEnumerable<Dataset> datasets,
            Activity activity,
            IActivityLogger logger)
```

O método tem alguns componentes chave que é necessário compreender:

* O método utiliza quatro parâmetros:

  * **linkedServices**. Este parâmetro é uma lista enumeráveis de serviços ligados para ligar a origens de dados de entrada/saída (por exemplo, armazenamento de BLOBs) à fábrica de dados. Neste exemplo, não há apenas um serviço ligado do tipo de armazenamento do Azure utilizados para a entrada e saída.
  * **datasets**. Este parâmetro é uma lista enumeráveis de conjuntos de dados. Pode utilizar este parâmetro para obter as localizações e esquemas definidas conjuntos de dados de entrada e de saída.
  * **atividade**. Este parâmetro representa a entidade de cálculo atual. Neste caso, é um serviço de Batch.
  * **logger**. Pode utilizar o registo para escrever comentários de depuração esse superfície como o registo de "Utilizador" para o pipeline.
* O método devolve um dicionário que pode ser utilizado encadear atividades personalizadas em conjunto no futuro. Esta funcionalidade ainda não está implementada, por isso, basta devolver um dicionário vazio do método.

#### <a name="procedure-create-the-custom-activity"></a>Procedimento: Criar a atividade personalizada
1. Crie um projeto de biblioteca de classe de .NET no Visual Studio.

   a. Inicie o Visual Studio 2012/2013/2015.

   b. Selecione **Ficheiro** > **Novo** > **Projeto**.

   c. Expanda **modelos**e selecione **Visual C\#**. Nestas instruções, utilizar o C\#, mas pode utilizar qualquer linguagem .NET para desenvolver a atividade personalizada.

   d. Selecione **biblioteca de classes** da lista de tipos de projeto à direita.

   e. Introduza **MyDotNetActivity** para o **nome**.

   f. Selecione **c:\\ADF** para o **localização**. Criar a pasta **ADF** se não existir.

   g. Selecione **OK** para criar o projeto.

2. Selecione **ferramentas** > **Gestor de pacotes NuGet** > **consola do Gestor de pacotes**.

3. Na consola do Gestor de pacotes, execute o seguinte comando para importar Microsoft.Azure.Management.DataFactories:

    ```powershell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. Importar o **Storage do Azure** pacote NuGet para o projeto. Este pacote é necessário uma vez que utilizam a API de armazenamento de BLOBs neste exemplo:

    ```powershell
    Install-Package Azure.Storage
    ```
5. Adicione o seguinte utilizando diretivas para o ficheiro de origem no projeto:

    ```csharp
    using System.IO;
    using System.Globalization;
    using System.Diagnostics;
    using System.Linq;
    
    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Runtime;
    
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```
6. Alterar o nome do espaço de nomes para **MyDotNetActivityNS**.

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. Alterar o nome de classe para **MyDotNetActivity**e derivar-à partir de **IDotNetActivity** interface conforme mostrado:

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. Implementar (adicionar) a **executar** método o **IDotNetActivity** interface para o **MyDotNetActivity** classe. Copie o seguinte código de exemplo para o método. Para obter uma explicação sobre a lógica utilizada neste método, consulte o [executar o método](#execute-method) secção.

    ```csharp
    /// <summary>
    /// The Execute method is the only method of IDotNetActivity interface you must implement.
    /// In this sample, the method invokes the Calculate method to perform the core logic.  
    /// </summary>
    public IDictionary<string, string> Execute(
       IEnumerable<LinkedService> linkedServices,
       IEnumerable<Dataset> datasets,
       Activity activity,
       IActivityLogger logger)
    {
    
       // Declare types for the input and output data stores.
       AzureStorageLinkedService inputLinkedService;
    
       Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
       foreach (LinkedService ls in linkedServices)
           logger.Write("linkedService.Name {0}", ls.Name);
    
       // Use the First method instead of Single because we are using the same
       // Azure Storage linked service for input and output.
       inputLinkedService = linkedServices.First(
           linkedService =>
           linkedService.Name ==
           inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
           as AzureStorageLinkedService;
    
       string connectionString = inputLinkedService.ConnectionString; // To create an input storage client.
       string folderPath = GetFolderPath(inputDataset);
       string output = string.Empty; // for use later.
    
       // Create the storage client for input. Pass the connection string.
       CloudStorageAccount inputStorageAccount = CloudStorageAccount.Parse(connectionString);
       CloudBlobClient inputClient = inputStorageAccount.CreateCloudBlobClient();
    
       // Initialize the continuation token before using it in the do-while loop.
       BlobContinuationToken continuationToken = null;
       do
       {   // get the list of input blobs from the input storage client object.
           BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                    true,
                                    BlobListingDetails.Metadata,
                                    null,
                                    continuationToken,
                                    null,
                                    null);
    
           // The Calculate method returns the number of occurrences of
           // the search term "Microsoft" in each blob associated
           // with the data slice.
           //
           // The definition of the method is shown in the next step.
           output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
       } while (continuationToken != null);
    
       // Get the output dataset by using the name of the dataset matched to a name in the Activity output collection.
       Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    
       folderPath = GetFolderPath(outputDataset);
    
       logger.Write("Writing blob to the folder: {0}", folderPath);
    
       // Create a storage object for the output blob.
       CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
       // Write the name of the file.
       Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
       logger.Write("output blob URI: {0}", outputBlobUri.ToString());
       // Create a blob and upload the output text.
       CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
       logger.Write("Writing {0} to the output blob", output);
       outputBlob.UploadText(output);
    
       // The dictionary can be used to chain custom activities together in the future.
       // This feature is not implemented yet, so just return an empty dictionary.
       return new Dictionary<string, string>();
    }
    ```
9. Adicione os seguintes métodos de programa auxiliar para a classe. Estes métodos são invocados pelo **executar** método. Mais importante, o **Calculate** método isola o código que itera através de cada blob.

    ```csharp
    /// <summary>
    /// Gets the folderPath value from the input/output dataset.
    /// </summary>
    private static string GetFolderPath(Dataset dataArtifact)
    {
       if (dataArtifact == null || dataArtifact.Properties == null)
       {
           return null;
       }
    
       AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
       if (blobDataset == null)
       {
           return null;
       }
    
       return blobDataset.FolderPath;
    }
    
    /// <summary>
    /// Gets the fileName value from the input/output dataset.
    /// </summary>
    
    private static string GetFileName(Dataset dataArtifact)
    {
       if (dataArtifact == null || dataArtifact.Properties == null)
       {
           return null;
       }
    
       AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
       if (blobDataset == null)
       {
           return null;
       }
    
       return blobDataset.FileName;
    }
    
    /// <summary>
    /// Iterates through each blob (file) in the folder, counts the number of instances of the search term in the file,
    /// and prepares the output text that is written to the output blob.
    /// </summary>
    
    public static string Calculate(BlobResultSegment Bresult, IActivityLogger logger, string folderPath, ref BlobContinuationToken token, string searchTerm)
    {
       string output = string.Empty;
       logger.Write("number of blobs found: {0}", Bresult.Results.Count<IListBlobItem>());
       foreach (IListBlobItem listBlobItem in Bresult.Results)
       {
           CloudBlockBlob inputBlob = listBlobItem as CloudBlockBlob;
           if ((inputBlob != null) && (inputBlob.Name.IndexOf("$$$.$$$") == -1))
           {
               string blobText = inputBlob.DownloadText(Encoding.ASCII, null, null, null);
               logger.Write("input blob text: {0}", blobText);
               string[] source = blobText.Split(new char[] { '.', '?', '!', ' ', ';', ':', ',' }, StringSplitOptions.RemoveEmptyEntries);
               var matchQuery = from word in source
                                where word.ToLowerInvariant() == searchTerm.ToLowerInvariant()
                                select word;
               int wordCount = matchQuery.Count();
               output += string.Format("{0} occurrences(s) of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
           }
       }
       return output;
    }
    ```
    O método de GetFolderPath devolve o caminho para a pasta que o dataset aponta para e o método GetFileName devolve o nome do que o dataset aponta para o ficheiro/blob.

    ```csharp

    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
    ```

    O método de Calculate calcula o número de instâncias da palavra-chave "Microsoft" nos ficheiros de entrada (blobs na pasta). O termo de pesquisa "Microsoft" é "hard-coded" no código.

10. Compile o projeto. Selecione **criar** no menu e, em seguida, selecione **compilar solução**.

11. Inicie o Explorador do Windows e vá para o **bin\\depurar** ou **bin\\versão** pasta. A escolha de pasta depende do tipo de compilação.

12. Criar um ficheiro zip **MyDotNetActivity.zip** que contém todos os binários no  **\\bin\\depurar** pasta. Pode querer incluir o MyDotNetActivity. **pdb** para que obtenha detalhes adicionais, tais como o número de linha com o código de origem que causou o problema, quando ocorre uma falha de ficheiros.

   ![A lista de pasta bin\Debug](./media/data-factory-data-processing-using-batch/image5.png)

13. Carregar **MyDotNetActivity.zip** como um blob para o contentor de blob `customactivitycontainer` no armazenamento de BLOBs que o StorageLinkedService ligados serviço no ADFTutorialDataFactory utiliza. Criar o contentor de blob `customactivitycontainer` se ainda não existir.

#### <a name="execute-method"></a>Executar o método
Esta secção fornece mais detalhes sobre o código no método de execução.

1. Os membros para repetir a coleção de entrada se encontram no [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) espaço de nomes. Para iterar através da coleção de blob, está a necessários para utilizar o **BlobContinuationToken** classe. Essencialmente, tem de utilizar um efetue-ao ciclo com o token de como o mecanismo para sair do ciclo. Para obter mais informações, consulte [utilizar o Blob storage a partir do .NET](../../storage/blobs/storage-dotnet-how-to-use-blobs.md). Um ciclo básico é mostrado aqui:

    ```csharp
    // Initialize the continuation token.
    BlobContinuationToken continuationToken = null;
    do
    {
    // Get the list of input blobs from the input storage client object.
    BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
    
                         true,
                                   BlobListingDetails.Metadata,
                                   null,
                                   continuationToken,
                                   null,
                                   null);
    // Return a string derived from parsing each blob.
    
     output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
    } while (continuationToken != null);

    ```
   Para obter mais informações, consulte a documentação para o [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) método.

2. O código para trabalhar com o conjunto de blobs logicamente passa dentro ao não-ao ciclo. No **executar** método, ao não-enquanto ciclo transfere a lista de blobs para um método denominado **Calculate**. O método devolve uma variável de cadeia denominada **saída** que é o resultado da ter iterated através de todos os blobs no segmento.

   Devolve o número de ocorrências do termo de pesquisa "Microsoft" no blob transmitido para o **Calculate** método.

    ```csharp
    output += string.Format("{0} occurrences of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
    ```
3. Depois do **Calculate** método estiver concluído, têm de ser escrito para um blob de novo. Para cada conjunto de blobs processados, um novo blob pode ser escrito com os resultados. Para escrever um novo blob, localize primeiro o conjunto de dados de saída.

    ```csharp
    // Get the output dataset by using the name of the dataset matched to a name in the Activity output collection.
    Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    ```
4. O código também chama o método de programa auxiliar **GetFolderPath** para obter o caminho da pasta (o nome do contentor de armazenamento).

    ```csharp
    folderPath = GetFolderPath(outputDataset);
    ```
   O método GetFolderPath casts o objeto do conjunto de dados para um AzureBlobDataSet, que tem uma propriedade chamada FolderPath.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FolderPath;
    ```
5. As chamadas de código a **GetFileName** método para obter o nome de ficheiro (nome do blob). O código é semelhante do código anterior, que foi utilizado para obter o caminho da pasta.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FileName;
    ```
6. O nome do ficheiro é escrito através da criação de um objeto URI. O construtor URI utiliza o **BlobEndpoint** propriedade para devolver o nome do contentor. O nome de ficheiro e caminho da pasta são adicionados ao construir o URI de blob de saída.  

    ```csharp
    // Write the name of the file.
    Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    ```
7. Depois do nome do ficheiro é escrito, pode escrever a cadeia de saída do **Calculate** método para um novo blob:

    ```csharp
    // Create a blob and upload the output text.
    CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
    logger.Write("Writing {0} to the output blob", output);
    outputBlob.UploadText(output);
    ```

### <a name="create-the-data-factory"></a>Criar fábrica de dados
No [criar a atividade personalizada](#create-the-custom-activity) secção, criou uma atividade personalizada e carregado o ficheiro zip com binários e o ficheiro PDB para um contentor de blob. Nesta secção, vai criar uma fábrica de dados com um pipeline que utiliza a atividade personalizada.

O conjunto de dados de entrada para a atividade personalizada representa os blobs (ficheiros) na pasta de entrada (`mycontainer\\inputfolder`) no armazenamento de Blobs. O conjunto de dados de saída para a atividade representa os blobs de saída na pasta de saída (`mycontainer\\outputfolder`) no armazenamento de Blobs.

Remova um ou mais ficheiros para as pastas de entrada:

```
mycontainer -\> inputfolder
    2015-11-16-00
    2015-11-16-01
    2015-11-16-02
    2015-11-16-03
    2015-11-16-04
```

Por exemplo, remova um ficheiro (file.txt) com o seguinte conteúdo para cada uma das pastas:

```
test custom activity Microsoft test custom activity Microsoft
```

Cada pasta de entrada corresponde a um setor na fábrica de dados, mesmo se a pasta tem dois ou mais ficheiros. Quando cada setor é processado pelo pipeline, a atividade personalizada itera através de todos os blobs na pasta de entrada para esse setor.

Pode ver os cinco ficheiros de saída com o mesmo conteúdo. Por exemplo, o ficheiro de saída do processamento do ficheiro na pasta 2015-11-16-00 tem o seguinte conteúdo:

```
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
```

Se remover vários ficheiros (file.txt file2.txt, file3.txt) com os mesmos conteúdos para a pasta de entrada, consulte o conteúdo seguinte no ficheiro de saída. Cada pasta (2015-11-16-00, etc.) corresponde a um setor neste exemplo, apesar da pasta tem vários ficheiros de entrada.

```csharp
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file2.txt.
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file3.txt.
```

O ficheiro de saída tem três linhas agora, um para cada ficheiro de entrada (blob) na pasta associado o setor (2015-11-16-00).

Uma tarefa é criada para cada execução da atividade. Neste exemplo, não há apenas uma atividade no pipeline. Quando um setor é processado pelo pipeline, a atividade personalizada é executada em Batch para processar o setor. Porque não existem setores de cinco (cada setor pode ter vários blobs ou ficheiro), são criadas cinco tarefas no Batch. Quando é executada uma tarefa do batch, é a atividade personalizada que está em execução.

As instruções seguintes fornece detalhes adicionais.

#### <a name="step-1-create-the-data-factory"></a>Passo 1: Criar a fábrica de dados
1. Depois de iniciar sessão para o [portal do Azure](https://portal.azure.com/), siga os passos seguintes:

   a. Selecione **novo** no menu da esquerda.

   b. Selecione **dados + análise** no **novo** painel.

   c. Selecione **Data Factory** no **análise de dados** painel.

2. No **nova fábrica de dados** painel, introduza **CustomActivityFactory** para o nome. O nome da fábrica de dados tem de ser globalmente exclusivo. Se receber o erro "o nome da fábrica de dados CustomActivityFactory não está disponível", altere o nome do data factory. Por exemplo, utilize yournameCustomActivityFactory e criar a fábrica de dados novamente.

3. Selecione **nome do grupo de recursos**e selecione um grupo de recursos existente ou crie um grupo de recursos.

4. Certifique-se de que a subscrição e região onde pretende criar fábrica de dados estão corretas.

5. Selecione **criar** no **nova fábrica de dados** painel.

6. A fábrica de dados é criada no dashboard do portal.

7. Depois da fábrica de dados criada com êxito, verá o **fábrica de dados** página, que mostra o conteúdo do data factory.

   ![Page da fábrica de dados](./media/data-factory-data-processing-using-batch/image6.png)

#### <a name="step-2-create-linked-services"></a>Passo 2: Criar serviços ligados
Os serviços ligados ligam os arquivos de dados ou serviços de computação a uma fábrica de dados. Neste passo, ligar a conta de armazenamento e a conta do Batch à fábrica de dados.

#### <a name="create-an-azure-storage-linked-service"></a>Criar um serviço ligado do Armazenamento do Azure
1. Selecione o **autor e implementar** mosaico a **fábrica de dados** painel **CustomActivityFactory**. É apresentado o Editor do Data Factory.

2. Selecione **novo arquivo de dados** na barra de comandos e escolha **storage do Azure.** O script JSON que utilizar para criar um serviço ligado no editor de aparece de armazenamento.

   ![Novo arquivo de dados](./media/data-factory-data-processing-using-batch/image7.png)

3. Substitua **nome da conta** com o nome da conta de armazenamento. Substitua **chave da conta** com a chave de acesso da conta de armazenamento. Para saber como obter a chave de acesso de armazenamento, consulte o artigo [ver, copiar e armazenamento voltar a gerar chaves de acesso](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).

4. Selecione **implementar** na barra de comando para implementar o serviço ligado.

   ![Implementação](./media/data-factory-data-processing-using-batch/image8.png)

#### <a name="create-an-azure-batch-linked-service"></a>Criar um serviço ligado do Azure Batch
Neste passo, vai criar um serviço ligado para a sua conta de Batch que é utilizada para executar a atividade personalizada da fábrica de dados.

1. Selecione **nova computação** na barra de comandos e escolha **do Azure Batch.** O script JSON que utilizar para criar um lote aparece de serviço ligado no editor.

2. O script de JSON:

   a. Substitua **nome da conta** com o nome da sua conta do Batch.

   b. Substitua **chave de acesso** com a chave de acesso da conta do Batch.

   c. Introduza o ID do conjunto para o **poolName** propriedade. Para esta propriedade, pode especificar o nome do conjunto ou o ID do conjunto.

   d. Introduza o URI do lote para o **batchUri** propriedade JSON.

      > [!IMPORTANT]
      > O URL do **conta do Batch** painel é o seguinte formato: \<accountname\>.\< região\>. batch.azure.com. Para o **batchUri** propriedade no script de JSON, terá de remover a88 "accountname." * * a partir do URL. Um exemplo é `"batchUri": "https://eastus.batch.azure.com"`.
      >
      >

      ![Painel de conta do batch](./media/data-factory-data-processing-using-batch/image9.png)

      Para o **poolName** propriedade, também pode especificar o ID do conjunto em vez do nome do conjunto.

      > [!NOTE]
      > O serviço fábrica de dados não suporta uma opção a pedido de Batch que realiza HDInsight. Pode utilizar apenas o seus próprios conjunto do Batch numa fábrica de dados.
      >
      >
   
   e. Especifique **StorageLinkedService** para o **linkedServiceName** propriedade. Este serviço ligado que criou no passo anterior. Este tipo de armazenamento é utilizado como uma área de transição para ficheiros e registos.

3. Selecione **implementar** na barra de comando para implementar o serviço ligado.

#### <a name="step-3-create-datasets"></a>Passo 3: Criar conjuntos de dados
Neste passo, vai criar conjuntos de dados para representar os dados de entrada e de saída.

#### <a name="create-the-input-dataset"></a>Criar o conjunto de dados de entrada
1. No Editor de fábrica de dados, selecione o **novo conjunto de dados** botão na barra de ferramentas. Selecione **Blob storage do Azure** na lista pendente.

2. Substitua o script JSON no painel à direita com o fragmento JSON seguinte:

    ```json
    {
       "name": "InputDataset",
       "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
               "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
               "format": {
                   "type": "TextFormat"
               },
               "partitionedBy": [
                   {
                       "name": "Year",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "yyyy"
                       }
                   },
                   {
                       "name": "Month",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "MM"
                       }
                   },
                   {
                       "name": "Day",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "dd"
                       }
                   },
                   {
                       "name": "Hour",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "HH"
                       }
                   }
               ]
           },
           "availability": {
               "frequency": "Hour",
               "interval": 1
           },
           "external": true,
           "policy": {}
       }
    }
    ```

    Pode cria um pipeline mais tarde esta explicação passo a passo com a hora de início de 2015-11-16T00:00:00Z e o fim do tempo de 2015-11-16T05:00:00Z. Foi agendado para produzir os dados de hora a hora, pelo que existem cinco setores de entrada/saída (entre **00**: 00:00 -\> **05**: 00:00).

    O **frequência** e **intervalo** para o conjunto de dados de entrada estão definidas como **hora** e **1**, o que significa que o setor de entrada está disponível por hora.

    A hora de início de cada setor é representada pelo **SliceStart** variável de sistema no fragmento JSON anterior. Seguem-se as horas de início de cada setor.

    | **Slice** | **Hora de início**          |
    |-----------|-------------------------|
    | 1         | 2015-11-16T**00**:00:00 |
    | 2         | 2015-11-16T**01**:00:00 |
    | 3         | 2015-11-16T**02**:00:00 |
    | 4         | 2015-11-16T**03**:00:00 |
    | 5         | 2015-11-16T**04**:00:00 |

    O **folderPath** é calculado utilizando a parte ano, mês, dia e hora a hora de início do setor (**SliceStart**). Eis como uma pasta de entrada está mapeada para um setor.

    | **Slice** | **Hora de início**          | **Pasta de entrada**  |
    |-----------|-------------------------|-------------------|
    | 1         | 2015-11-16T**00**:00:00 | 2015-11-16-**00** |
    | 2         | 2015-11-16T**01**:00:00 | 2015-11-16-**01** |
    | 3         | 2015-11-16T**02**:00:00 | 2015-11-16-**02** |
    | 4         | 2015-11-16T**03**:00:00 | 2015-11-16-**03** |
    | 5         | 2015-11-16T**04**:00:00 | 2015-11-16-**04** |

3. Selecione **implementar** na barra de ferramentas para criar e implementar o **InputDataset** tabela.

#### <a name="create-the-output-dataset"></a>Criar o conjunto de dados de saída
Neste passo, crie outro conjunto de dados do tipo AzureBlob para representar os dados de saída.

1. No Editor de fábrica de dados, selecione o **novo conjunto de dados** botão na barra de ferramentas. Selecione **Blob storage do Azure** na lista pendente.

2. Substitua o script JSON no painel à direita com o fragmento JSON seguinte:

    ```json
    {
       "name": "OutputDataset",
       "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
               "fileName": "{slice}.txt",
               "folderPath": "mycontainer/outputfolder",
               "partitionedBy": [
                   {
                       "name": "slice",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "yyyy-MM-dd-HH"
                       }
                   }
               ]
           },
           "availability": {
               "frequency": "Hour",
               "interval": 1
           }
       }
    }
    ```

    Um ficheiro/blob de saída é gerado para cada setor de entrada. Eis como um ficheiro de saída é designado para cada setor. Todos os ficheiros de saída são gerados por uma pasta de saída, `mycontainer\\outputfolder`.

    | **Slice** | **Hora de início**          | **Ficheiro de saída**       |
    |-----------|-------------------------|-----------------------|
    | 1         | 2015-11-16T**00**:00:00 | 2015-11-16-**00.txt** |
    | 2         | 2015-11-16T**01**:00:00 | 2015-11-16-**01.txt** |
    | 3         | 2015-11-16T**02**:00:00 | 2015-11-16-**02.txt** |
    | 4         | 2015-11-16T**03**:00:00 | 2015-11-16-**03.txt** |
    | 5         | 2015-11-16T**04**:00:00 | 2015-11-16-**04.txt** |

    Lembre-se de que todos os ficheiros numa pasta entrada (por exemplo, 2015-11-16-00) fazem parte de um setor com a hora de início de 2015-11-16-00. Quando este setor é processado, a atividade personalizada analisa através de cada ficheiro e produz uma linha no ficheiro de saída com o número de ocorrências do termo de pesquisa "Microsoft". Se existirem três ficheiros na pasta 2015-11-16-00, existem três linhas de saída ficheiro 2015-11-16-00.txt.

3. Selecione **implementar** na barra de ferramentas para criar e implementar o **OutputDataset**.

#### <a name="step-4-create-and-run-the-pipeline-with-a-custom-activity"></a>Passo 4: Criar e executar o pipeline com uma atividade personalizada
Neste passo, vai criar um pipeline com uma atividade, a atividade personalizada que criou anteriormente.

> [!IMPORTANT]
> Se ainda não adicionou **file.txt** para pastas no contentor do blob de entrada, fazer antes de criar o pipeline. O **isPaused** propriedade está definida como false no pipeline JSON, pelo que o pipeline é executado imediatamente porque o **iniciar** data está no passado.
>
>

1. No Editor de fábrica de dados, selecione **novo pipeline** na barra de comandos. Se não vir o comando, selecione o símbolo de botão de reticências para apresentá-lo.

2. Substitua o script JSON no painel à direita com o fragmento JSON seguinte:

    ```json
    {
       "name": "PipelineCustom",
       "properties": {
           "description": "Use custom activity",
           "activities": [
               {
                   "type": "DotNetActivity",
                   "typeProperties": {
                       "assemblyName": "MyDotNetActivity.dll",
                       "entryPoint": "MyDotNetActivityNS.MyDotNetActivity",
                       "packageLinkedService": "AzureStorageLinkedService",
                       "packageFile": "customactivitycontainer/MyDotNetActivity.zip"
                   },
                   "inputs": [
                       {
                           "name": "InputDataset"
                       }
                   ],
                   "outputs": [
                       {
                           "name": "OutputDataset"
                       }
                   ],
                   "policy": {
                       "timeout": "00:30:00",
                       "concurrency": 5,
                       "retry": 3
                   },
                   "scheduler": {
                       "frequency": "Hour",
                       "interval": 1
                   },
                   "name": "MyDotNetActivity",
                   "linkedServiceName": "AzureBatchLinkedService"
               }
           ],
           "start": "2015-11-16T00:00:00Z",
           "end": "2015-11-16T05:00:00Z",
           "isPaused": false
      }
    }
    ```
   Tenha em atenção os seguintes pontos:

   * Apenas uma atividade no pipeline, e é do tipo **DotNetActivity**.
   * **AssemblyName** está definido como o nome da DLL de **MyDotNetActivity.dll**.
   * **EntryPoint** está definido como **MyDotNetActivityNS.MyDotNetActivity**. É basicamente \<espaço de nomes\>.\< ClassName\> no seu código.
   * **PackageLinkedService** está definido como **StorageLinkedService**, que aponta para o armazenamento de blob que contém o ficheiro zip de atividade personalizado. Se utilizar contas de armazenamento diferentes para os ficheiros de entrada/saída e o ficheiro zip de atividade personalizado, terá de criar outro serviço ligado de armazenamento. Este artigo pressupõe que utilize a mesma conta de armazenamento.
   * **PackageFile** está definido como **customactivitycontainer/MyDotNetActivity.zip**. Está no formato \<containerforthezip\>/\<nameofthezip.zip\>.
   * A atividade personalizada demora **InputDataset** como entrada e **OutputDataset** como saída.
   * O **linkedServiceName** propriedade da atividade personalizada aponta para **AzureBatchLinkedService**, que indica a fábrica de dados que a atividade personalizada tem de executar no Batch.
   * O **simultaneidade** definição é importante. Se utilizar o valor predefinido, que é 1, mesmo se tiver dois ou mais nós de computação num conjunto do Batch, são processados os setores umas a seguir. Por conseguinte, não são tirando partido de paralelo processamento capacidade do Batch. Se definir **simultaneidade** para um valor superior, diga 2, significa que dois setores (corresponde à duas tarefas no Batch) podem ser processados em simultâneo. Neste caso, ambas as VMs de conjunto do Batch são utilizadas. Defina a propriedade de concorrência adequadamente.
   * Por predefinição, apenas uma tarefa (setor) é executada numa VM em qualquer momento. Por predefinição, **máximo de tarefas por VM** está definido como 1 para um conjunto do Batch. Como parte dos pré-requisitos, criou um conjunto com esta propriedade definida como 2. Por conseguinte, setores de fábrica de dados de dois podem ser executado numa VM ao mesmo tempo.
    - O **isPaused** propriedade está definida como false, por predefinição. O pipeline executa imediatamente neste exemplo, porque os setores de início no passado. Pode definir esta propriedade **verdadeiro** para colocar em pausa o pipeline e set-lo novamente para **falso** reiniciar.
    -   O **iniciar** e **final** vezes são, à excepção de cinco horas. Setores são produzidos de hora a hora, pelo que os cinco setores são produzidos pelo pipeline.

3. Selecione **implementar** na barra de comandos para implementar o pipeline.

#### <a name="step-5-test-the-pipeline"></a>Passo 5: Testar o pipeline
Neste passo, teste o pipeline, arrastando ficheiros para as pastas de entrada. Comece por testar o pipeline com um ficheiro para cada pasta de entrada.

1. No **fábrica de dados** painel no portal do Azure, selecione **diagrama**.

   ![Diagrama](./media/data-factory-data-processing-using-batch/image10.png)

2. No **diagrama** ver, faça duplo clique o conjunto de dados de entrada **InputDataset**.

   ![InputDataset](./media/data-factory-data-processing-using-batch/image11.png)

3. O **InputDataset** é apresentado o painel com todos os setores de cinco prontos. Tenha em atenção o **hora de início do SETOR** e **hora de fim de SETOR** para cada setor.

   ![Início do setor de entrada e de fim](./media/data-factory-data-processing-using-batch/image12.png)

4. No **diagrama** visualizar, selecione **OutputDataset**.

5. Os setores de cinco saída aparecem no **pronto** se foram produzidos de estado.

   ![Início do setor de saída e de fim](./media/data-factory-data-processing-using-batch/image13.png)

6. Utilizar o portal para ver as tarefas associadas os setores e ver que VM cada setor é executado no. Para obter mais informações, consulte o [integração com o Data Factory e Batch](#data-factory-and-batch-integration) secção.

7. Os ficheiros de saída são apresentados em `mycontainer` no `outputfolder` no seu armazenamento de Blobs.

   ![Ficheiros de saída no armazenamento](./media/data-factory-data-processing-using-batch/image15.png)

   Cinco ficheiros de saída estão listados, um para cada setor de entrada. Cada um dos ficheiros de saída tem conteúdo semelhante ao seguinte resultado:

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
    ```
   O diagrama seguinte ilustra a forma como os setores de fábrica de dados mapeiam tarefas no Batch. Neste exemplo, um setor tem apenas uma execução.

   ![Diagrama de mapeamento de setor](./media/data-factory-data-processing-using-batch/image16.png)

8. Experimente agora com vários ficheiros numa pasta. Criar os ficheiros **file2.txt**, **file3.txt**, **file4.txt**, e **file5.txt** com os mesmos conteúdos como file.txt na pasta **2015-11-06-01**.

9. Na pasta de saída, elimine o ficheiro de saída **2015-11-16-01.txt**.

10. No **OutputDataset** painel, o setor com o botão direito do rato **hora de início do SETOR** definido como **11/16/2015 01:00:00 AM**. Selecione **executar** para Reprocessar/volte a executar o setor. O setor tem agora os cinco ficheiros em vez de um ficheiro.

    ![Executar](./media/data-factory-data-processing-using-batch/image17.png)

11. Depois de executa o setor e o respetivo estado é **pronto**, verifique o conteúdo no ficheiro de saída neste setor (**2015-11-16-01.txt**). O ficheiro de saída é apresentado em `mycontainer` no `outputfolder` no seu armazenamento de Blobs. Deve ser uma linha para cada ficheiro do setor.

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file2.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file3.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file4.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file5.txt.
    ```

> [!NOTE]
> Se não eliminar a saída ficheiro 2015-11-16-01.txt antes de tentou com cinco ficheiros de entrada, verá uma linha de execução anterior do setor e cinco linhas da execução do setor atual. Por predefinição, o conteúdo é acrescentado para o ficheiro de saída, se já existir.
>
>

#### <a name="data-factory-and-batch-integration"></a>Integração de fábrica de dados e de Batch
O serviço Data Factory cria uma tarefa no Batch com o nome `adf-poolname:job-xxx`.

![Tarefas do batch](media/data-factory-data-processing-using-batch/data-factory-batch-jobs.png)

Uma tarefa a tarefa é criada para cada execução de atividade de um setor. Se 10 setores, estará pronto ser processado, são criadas 10 tarefas na tarefa. Pode ter mais do que um setor ser executado em paralelo se tiver vários nós de computação no conjunto. Se o número máximo de tarefas por nó de computação é definido como maior do que um, pode executar a segmentação de mais do que uma na mesma computação.

Neste exemplo, existem cinco setores, pelo que existem cinco tarefas no Batch. Com **simultaneidade** definido como **5** no pipeline JSON na fábrica de dados e **máximo de tarefas por VM** definido como **2** no conjunto do Batch com **2** VMs, as tarefas executadas rápida. (Verifique os tempos de início e de fim para tarefas).

Utilizar o portal para ver a tarefa do Batch e das respetivas tarefas associadas os setores e ver que VM cada setor é executado no.

![Tarefas de lote](media/data-factory-data-processing-using-batch/data-factory-batch-job-tasks.png)

### <a name="debug-the-pipeline"></a>Depurar o pipeline
Depuração é composta por alguns técnicas básicas.

1. Se o setor de entrada não está definido como **pronto**, confirme que a estrutura da pasta de entrada está correta e que file.txt existe nas pastas de entrada.

   ![Estrutura da pasta de entrada](./media/data-factory-data-processing-using-batch/image3.png)

2. No **executar** método da atividade personalizada, utilize o **IActivityLogger** objeto para registar informações que o ajuda a resolver problemas. As mensagens anteriormente registadas apresentada ao utilizador\_0. ficheiro de registo.

   No **OutputDataset** painel, selecione o setor para ver o **setor de dados** painel para essa setor. Em **atividade é executada**, verá uma atividade executar para o setor. Se selecionar **executar** na barra de comandos, pode começar a executar para o mesmo setor de outra atividade.

   Quando seleciona a execução da atividade, consulte o **detalhes da execução da atividade** painel com uma lista de ficheiros de registo. Consulte as mensagens anteriormente registadas ao utilizador\_0. ficheiro de registo. Quando ocorre um erro, consulte três execuções de atividade porque a contagem de repetições está definida para 3 no pipeline/JSON de atividade. Quando seleciona a execução de atividade, consulte os ficheiros de registo que poderá consultar para resolver o erro.

   ![Painéis de setor de dados e OutputDataset](./media/data-factory-data-processing-using-batch/image18.png)

   Na lista de ficheiros de registo, selecione **utilizador 0.log**. No painel à direita, os resultados de utilizar o **IActivityLogger.Write** método aparecer.

   ![Painel de detalhes de execução da atividade](./media/data-factory-data-processing-using-batch/image19.png)

   Verifique o sistema-0.log para quaisquer mensagens de erro de sistema e exceções.

    ```
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Loading assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Creating an instance of MyDotNetActivityNS.MyDotNetActivity from assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Executing Module
    
    Trace\_T\_D\_12/6/2015 1:43:38 AM\_T\_D\_\_T\_D\_Information\_T\_D\_0\_T\_D\_Activity e3817da0-d843-4c5c-85c6-40ba7424dce2 finished successfully
    ```
3. Incluir o **PDB** no ficheiro zip do ficheiro para que os detalhes do erro tenham informações tais como a pilha de chamadas quando ocorre um erro.

4. Todos os ficheiros no ficheiro zip para a atividade personalizada tem de estar no nível superior com sem subpastas.

   ![Lista de ficheiros zip de atividade personalizado](./media/data-factory-data-processing-using-batch/image20.png)

5. Certifique-se de que **assemblyName** (MyDotNetActivity.dll), **entryPoint** (MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer / MyDotNetActivity.zip), e **packageLinkedService** (devem apontar para o armazenamento de blob que contém o ficheiro zip) estão definidos para os valores corretos.

6. Se corrigido um erro e pretender Reprocessar o setor, faça duplo clique o setor no **OutputDataset** painel e selecione **executar**.

   ![Painel OutputDataset opção executar](./media/data-factory-data-processing-using-batch/image21.png)

   > [!NOTE]
   > É um contentor de armazenamento de Blobs com o nome `adfjobs`. Este contentor não está a ser eliminado automaticamente, mas pode eliminar em segurança depois de concluir o teste a solução. Da mesma forma, a solução de fábrica de dados cria uma tarefa de lote designada `adf-\<pool ID/name\>:job-0000000001`. É possível eliminar esta tarefa depois de testar a solução se assim o desejar.
   >
   >
7. A atividade personalizada não utiliza o **App. config** ficheiro a partir do seu pacote. Por conseguinte, se o seu código lê quaisquer cadeias de ligação no ficheiro de configuração, não funciona no tempo de execução. A melhor prática quando utilizar o Batch é reter os segredos no Cofre de chaves do Azure. Em seguida, utilize um principal de serviço baseada em certificado para proteger o Cofre de chaves e distribuir o certificado para o conjunto do Batch. A atividade personalizada do .NET pode aceder segredos do Cofre de chaves durante a execução. Esta solução genérica pode dimensionar a qualquer tipo de segredo, não apenas uma cadeia de ligação.

    Existe uma solução mais fácil, mas não é uma melhor prática. Pode criar um serviço de base de dados ligado de SQL com ligação as definições de cadeia. Em seguida, pode criar um conjunto de dados que utiliza o serviço ligado e encadeiam o conjunto de dados como um conjunto de dados de entrada fictício para a atividade personalizada do .NET. Em seguida, pode aceder a cadeia de ligação de serviço ligado com o código de atividade personalizado. Deve trabalhar ajustar no tempo de execução.  

#### <a name="extend-the-sample"></a>Expandir o exemplo
Pode expandir este exemplo para obter mais informações sobre as funcionalidades de fábrica de dados e de Batch. Por exemplo, para processar setores um intervalo de hora diferente, siga os passos seguintes:

1. Adicione as seguintes subpastas na `inputfolder`: 2015-11-16-05, 2015-11-16-06 201-11-16-07, 2011-11-16-08 e 2015-11-16-09. Colocar os ficheiros de entrada nessas pastas. Alterar a hora de fim para o pipeline de `2015-11-16T05:00:00Z` para `2015-11-16T10:00:00Z`. No **diagrama** ver, faça duplo clique em **InputDataset** e confirme que os setores de entrada estão prontos. Faça duplo clique em **OutputDataset** para ver o estado dos setores de saída. Se não estiverem no **pronto** de estado, verifique a pasta de saída para os ficheiros de saída.

2. Aumentar ou diminuir o **simultaneidade** definição para compreender como afeta o desempenho da sua solução, especialmente o processamento ocorre no Batch. Para mais informações sobre o **simultaneidade** definição, consulte "passo 4: criar e executar o pipeline com uma atividade personalizada."

3. Criar um conjunto com maior/menor **máximo de tarefas por VM**. Para utilizar o novo conjunto que criou, atualize o serviço de Batch ligado na solução de fábrica de dados. Para mais informações sobre o **máximo de tarefas por VM** definição, consulte "passo 4: criar e executar o pipeline com uma atividade personalizada."

4. Criar um conjunto do Batch com a **dimensionamento automático** funcionalidade. Dimensionar automaticamente nós de computação de um conjunto do Batch é o ajustamento dinâmico do processamento de energia utilizada pela sua aplicação. 

    A fórmula de exemplo aqui distribui o comportamento seguinte. Quando o conjunto for criado inicialmente, começa com uma VM. A métrica $PendingTasks define o número de tarefas na execução e Estados de Active Directory (em fila). A fórmula localiza o número médio de tarefas pendentes nos últimos 180 segundos e define TargetDedicated em conformidade. Garante que nunca chegam TargetDedicated para além de 25 VMs. Como são submetidas novas tarefas, o conjunto que cresce automaticamente. As tarefas são concluídas, VMs fiquem livre um por um e o dimensionamento automático diminui dessas VMs. Pode ajustar startingNumberOfVMs e maxNumberofVMs às suas necessidades.
 
    Fórmula de dimensionamento automático:

    ``` 
    startingNumberOfVMs = 1;
    maxNumberofVMs = 25;
    pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
    pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
    $TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
    ```

   Para obter mais informações, consulte [automaticamente escala nós de computação num conjunto do Batch](../../batch/batch-automatic-scaling.md).

   Se o conjunto utiliza a predefinição [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), o serviço Batch pode demorar 15 a 30 minutos para preparar a VM antes de executar a atividade personalizada. Se o conjunto utiliza um autoScaleEvaluationInterval diferentes, o serviço Batch pode demorar autoScaleEvaluationInterval mais de 10 minutos.

5. Na solução de exemplo, o **executar** invoca o método de **Calculate** método que processe um setor de dados de entrada para produzir um setor de dados de saída. Pode escrever o seu próprio método para processar dados de entrada e substitua o **Calculate** chamada de método **executar** método com uma chamada para o método.

### <a name="next-steps-consume-the-data"></a>Próximos passos: consumir dados
Depois de processar dados, que pode consumir com ferramentas online como o Power BI. Seguem-se ligações para o ajudar a compreender o Power BI e como utilizá-la no Azure:

* [Explore um conjunto de dados no Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-data/)
* [Introdução ao Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/)
* [Atualizar dados no Power BI](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/)
* [Azure e o Power BI: Descrição geral básica](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)

## <a name="references"></a>Referências
* [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)

  * [Introdução ao serviço Data Factory](data-factory-introduction.md)
  * [Introdução ao Data Factory](data-factory-build-your-first-pipeline.md)
  * [Utilizar atividades personalizadas num pipeline fábrica de dados](data-factory-use-custom-activities.md)
* [O Azure Batch](https://azure.microsoft.com/documentation/services/batch/)

  * [Noções básicas do Batch](../../batch/batch-technical-overview.md)
  * [Descrição geral das funcionalidades do Batch](../../batch/batch-api-basics.md)
  * [Criar e gerir uma conta do Batch no portal do Azure](../../batch/batch-account-create-portal.md)
  * [Introdução à biblioteca de cliente do Batch para .NET](../../batch/batch-dotnet-get-started.md)

[batch-explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch-explorer-walkthrough]: http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx
