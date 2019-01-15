---
title: Voltar a preparar um existente preditiva Studio web service titleSuffix: Descrição do Azure Machine Learning Studio: Saiba como voltar a preparar um modelo e atualizar o serviço web para utilizar o modelo treinado recentemente no Azure Machine Learning Studio.
services: machine-learning ms.service: machine-learning ms.component: studio ms.topic: article

autor: ericlicoding MS. Author: amlstudiodocs MS. Custom: anterior ms.author=yahajiza, autor anterior = YasinMSFT MS: 11/07/2017
---
# <a name="retrain-an-existing-predictive-azure-machine-learning-studio-web-service"></a>Reenviar um serviço de web do Azure Machine Learning Studio preditivo existente

Este documento descreve o processo de reparametrização para o seguinte cenário:

* Tem uma experimentação de preparação e de uma experimentação preditiva que tenha implementado como um serviço web operacionalizado.
* Tiver novos dados que pretende que o seu serviço web de previsão a utilizar para efetuar a respetiva classificação.

> [!NOTE]
> Para implementar um novo serviço web tem de ter permissões suficientes na subscrição para a qual estiver a implementar o serviço web. Para obter mais informações, consulte [gerir um serviço Web através do portal do Azure Machine Learning Web Services](manage-new-webservice.md).

A partir do seu serviço web existente e experiências, tem de seguir estes passos:

1. Atualize o modelo.
   1. Modificar a sua experimentação de preparação para permitir o web service entradas e saídas.
   2. Implemente a experimentação de preparação como um serviço web reparametrização.
   3. Utilize serviço BES a experimentação de preparação (Batch Execution) para voltar a preparar o modelo.
2. Utilize os cmdlets do PowerShell do Azure Machine Learning para atualizar a experimentação preditiva.
   1. Inicie sessão na sua conta do Azure Resource Manager.
   2. Obter a definição de serviço da web.
   3. Exporte a definição de serviço da web como JSON.
   4. Atualize a referência para o blob de ilearner no JSON.
   5. Importe o JSON para uma definição de serviço da web.
   6. Atualize o serviço web com uma nova definição de serviço da web.

## <a name="deploy-the-training-experiment"></a>Implementar a experimentação de preparação
Para implementar a experimentação de preparação como um serviço web reparametrização, tem de adicionar web service entradas e saídas para o modelo. Conectando-se um *saída de serviço Web* módulo para a experimentação *[Train Model][train-model]* módulo, habilitar a experimentação de preparação para produza um novo modelo treinado que pode utilizar na sua experimentação preditiva. Se tiver uma *Evaluate Model* módulo, também pode anexar saída do serviço web para obter os resultados da avaliação como saída.

Para atualizar a sua experimentação de preparação:

1. Ligar um *entrada de serviço da Web* módulo para a sua entrada de dados (por exemplo, uma *Clean Missing Data* módulo). Normalmente, desejar garantir que os seus dados de entrada são processados da mesma forma como os dados de treinamento original.
2. Ligar um *saída de serviço Web* módulo à saída do seu *Train Model* módulo.
3. Se tiver uma *Evaluate Model* módulo e pretende enviar os resultados da avaliação, ligue-se um *saída de serviço Web* módulo à saída do seu *Evaluate Model* módulo.

Execute a sua experimentação.

Em seguida, tem de implementar a experimentação de preparação como um serviço web que produz um modelo preparado e resultados de avaliação do modelo.

Na parte inferior da tela de experimentação, clique em **segurança de serviço Web**e, em seguida, selecione **implementar o Web Service [novo]**. O portal de serviços da Web do Azure Machine Learning abre-se para o **implementar serviço Web** página. Escreva um nome para o seu serviço web, escolha um plano de pagamento e, em seguida, clique em **Deploy**. Só pode utilizar o método de execução de lotes para a criação de modelos de formação.

## <a name="retrain-the-model-with-new-data-by-using-bes"></a>Voltar a preparar o modelo com novos dados utilizando BES
Neste exemplo, estamos a utilizar c# para criar o aplicativo de reparametrização. Também pode utilizar o código de exemplo do Python ou R para realizar esta tarefa.

Para chamar as APIs de reparametrização:

1. Criar um C# consola de aplicação no Visual Studio: **Novos** > **projeto** > **Visual C#**   >  **ambiente de trabalho clássico do Windows**  >   **Aplicação da consola (.NET Framework)**.
2. Inicie sessão no portal de serviços Web Machine Learning.
3. Clique no serviço de web que está trabalhando com.
4. Clique em **consumir**.
5. Na parte inferior a **Consume** página, além do **código de exemplo** secção, clique em **Batch**.
6. Copie o código de exemplo em C# para execução de lotes e colá-lo no ficheiro Program.cs. Certifique-se de que o espaço de nomes permanece intacto.

Adicione o pacote de NuGet Microsoft.AspNet.WebApi.Client, conforme especificado nos comentários. Para adicionar a referência ao Microsoft.WindowsAzure.Storage.dll, primeiro poderá ter de instalar o [biblioteca de clientes para serviços de armazenamento do Azure](https://www.nuget.org/packages/WindowsAzure.Storage).

A captura de ecrã a seguir mostra a **Consume** página no portal do Azure Machine Learning Web Services.

![Consumir de página][1]

### <a name="update-the-apikey-declaration"></a>Atualizar a declaração de apikey
Localize a **apikey** declaração:

    const string apiKey = "abc123"; // Replace this with the API key for the web service

No **informações básicas de consumo** secção a **Consume** página, localize a chave primária e copie-o para o **apikey** declaração.

### <a name="update-the-azure-storage-information"></a>Atualizar as informações de armazenamento do Azure
O código de exemplo BES carrega um ficheiro de uma unidade local (por exemplo, "C:\temp\CensusIpnput.csv") ao armazenamento do Azure, processa-os e grava os resultados de volta ao armazenamento do Azure.

Depois de executar a experimentação, o fluxo de trabalho resultante deve ser semelhante ao seguinte:

![Fluxo de trabalho resultante após a execução][4]

1. Inicie sessão no Portal do Azure.
2. Na coluna de navegação esquerda, clique em **mais serviços**, procure **contas de armazenamento**e selecioná-lo.
3. Na lista de contas de armazenamento, selecione um para armazenar o modelo retrained.
4. Na coluna de navegação esquerda, clique em **chaves de acesso**.
5. Copie e guarde o **chave de acesso primária**.
6. Na coluna de navegação esquerda, clique em **contentores**.
7. Selecione um contentor existente, ou criar um novo e guarde o nome.

Localize a *StorageAccountName*, *StorageAccountKey*, e *StorageContainerName* declarações e atualize os valores que guardou a partir do portal.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

Também tem de garantir que o ficheiro de entrada está disponível na localização que especificou no código.

### <a name="specify-the-output-location"></a>Especifique a localização de saída
Quando especificar a localização de saída no Payload do pedido, a extensão do ficheiro que é especificada na *RelativeLocation* deve ser especificado como `ilearner`. Veja o seguinte exemplo:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you want to use for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

Segue-se um exemplo de saída reparametrização:

![Saída de reparametrização][6]

## <a name="evaluate-the-retraining-results"></a>Avaliar os resultados de reparametrização
Quando executar o aplicativo, a saída inclui o URL e o token de assinaturas de acesso partilhado que são necessárias para acessar os resultados da avaliação.

Pode ver os resultados de desempenho do modelo retrained ao combinar a *BaseLocation*, *RelativeLocation*, e *SasBlobToken* nos resultados da saída para *output2* (conforme mostrado na imagem de saída de reparametrização anterior) e colar o URL completo na barra de endereço do browser.

Examine os resultados para determinar se o modelo treinado recentemente executa bem o suficiente para substituir a existente.

Copiar o *BaseLocation*, *RelativeLocation*, e *SasBlobToken* nos resultados da saída.

## <a name="retrain-the-web-service"></a>Voltar a preparar o serviço web
Quando voltar a preparar um novo serviço web, atualizar a definição de serviço web preditivo para referenciar o novo modelo treinado. A definição de serviço web é uma representação interna do modelo treinado do serviço web e não é modificável diretamente. Certifique-se de que estão a obter a definição de serviço da web para a sua experimentação preditiva e não a sua experimentação de preparação.

## <a name="sign-in-to-azure-resource-manager"></a>Inicie sessão para o Azure Resource Manager
Tem primeiro de iniciar sessão sua conta do Azure a partir de ambiente do PowerShell, utilizando o [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) cmdlet.

## <a name="get-the-web-service-definition-object"></a>Obter o objeto de definição de serviço da Web
Em seguida, obtém o objeto de definição de serviço Web chamando o [Get-AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/get-azurermmlwebservice) cmdlet.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Para determinar o nome do grupo de recursos de um serviço web existente, execute o cmdlet Get-AzureRmMlWebService sem quaisquer parâmetros para apresentar os serviços web na sua subscrição. Localize o serviço web e, em seguida, examinar sua ID de serviço web. O nome do grupo de recursos é o quarto elemento no ID, logo a seguir a *resourceGroups* elemento. No exemplo a seguir, o nome do grupo de recursos é a predefinição-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Em alternativa, para determinar o nome do grupo de recursos de um serviço web existente, inicie sessão no portal do Azure Machine Learning Web Services. Selecione o serviço web. O nome do grupo de recursos é o elemento quinto do URL do serviço web, logo a seguir a *resourceGroups* elemento. No exemplo a seguir, o nome do grupo de recursos é a predefinição-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-object-as-json"></a>Exportar o objeto de definição de serviço da Web como JSON
Para modificar a definição do modelo preparado para utilizar o modelo treinado recentemente, primeiro tem de utilizar o [Export-AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/export-azurermmlwebservice) cmdlet para os exportar para um ficheiro de formato JSON.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob"></a>Atualizar a referência para o blob de ilearner
No ativos, localize o [modelo preparado], atualize o *uri* valor no *locationInfo* nó com o URI do ilearner blob. O URI é gerado ao combinar a *BaseLocation* e o *RelativeLocation* da saída do BES reparametrização chamada.

     "asset3": {
        "name": "Retrain Sample [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition-object"></a>Importe o JSON para um objeto de definição de serviço da Web
Tem de utilizar o [Import-AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/import-azurermmlwebservice) cmdlet para converter o ficheiro JSON modificado novamente para um objeto de definição de serviço da Web que pode utilizar para atualizar a experimentação predicative.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service"></a>Atualizar o serviço web
Por último, utilize o [AzureRmMlWebService atualização](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/update-azurermmlwebservice) cmdlet para atualizar a experimentação preditiva.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

[1]: ./media/retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
