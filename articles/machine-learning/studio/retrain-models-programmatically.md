---
title: Reparametrizar modelos do Machine Learning Studio por meio de programação - Azure | Documentos da Microsoft
description: Saiba como voltar por meio de programação a preparar um modelo com C# e o serviço de Machine Learning Batch Execution.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: seodec18
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: 7ae4f977-e6bf-4d04-9dde-28a66ce7b664
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.openlocfilehash: 7a1381ce43056607486a27710cd6ee6181b9a5c1
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2018
ms.locfileid: "53257398"
---
# <a name="retrain-azure-machine-learning-studio-models-programmatically"></a>Reparametrizar modelos do Azure Machine Learning Studio por meio de programação
Nestas instruções, irá aprender a reenviar programaticamente um através do serviço do Azure Machine Learning Studio web C# e o serviço de Machine Learning Batch Execution.

Depois de ter de reestruturar o modelo, os guias seguintes mostram como atualizar o modelo no seu serviço web preditivo:

* Se tiver de implementar um serviço da web clássico no portal de serviços Web Machine Learning, veja [reenviar um serviço web clássico](retrain-a-classic-web-service.md). 
* Se tiver implementado um novo serviço web, consulte [reenviar um serviço web novo utilizando os cmdlets de gestão de Machine Learning](retrain-new-web-service-using-powershell.md).

Para uma descrição geral do processo de reparametrização, consulte [voltar a preparar um modelo de aprendizagem automática](retrain-machine-learning-model.md).

Se quiser iniciar no serviço de web existente do novo Azure baseado no Resource Manager, veja [reenviar um serviço web preditivo existente](retrain-existing-resource-manager-based-web-service.md).

## <a name="create-a-training-experiment"></a>Criar uma experimentação de preparação
Irá utilizar neste exemplo, "exemplo 5: Preparar, testar, avalie para classificação binária: Um conjunto de dados para adultos"de exemplos do Microsoft Azure Machine Learning. 

Para criar a experimentação:

1. Inicie sessão no Microsoft Azure Machine Learning Studio. 
2. No canto inferior direito do dashboard, clique em **New**.
3. A partir de Samples Microsoft, selecione exemplo 5.
4. Para mudar o nome da experimentação, na parte superior da tela de experimentação, selecione o nome de experimentação "exemplo 5: Preparar, testar, avalie para classificação binária: Conjunto de dados para adultos".
5. Modelo de censo de tipo.
6. Na parte inferior da tela de experimentação, clique em **executar**.
7. Clique em **configurar o serviço de web** e selecione **reparametrização do serviço web**. 

O código a seguir mostra a experimentação inicial.
   
   ![Experiência inicial.][2]


## <a name="create-a-predictive-experiment-and-publish-as-a-web-service"></a>Criar uma experimentação preditiva e publicar como um serviço web
Em seguida, vai criar uma experimentação de Predicative.

1. Na parte inferior da tela de experimentação, clique em **segurança de serviço Web** e selecione **serviço de Web preditiva**. Isto guarda o modelo como um modelo preparado e adiciona módulos de entrada e saída do serviço de web. 
2. Clique em **Executar**. 
3. Depois da experimentação estiver concluída em execução, clique em **implementar o serviço Web [clássica]** ou **implementar o Web Service [novo]**.

> [!NOTE] 
> Para implementar um novo serviço web tem de ter permissões suficientes na subscrição para a qual estiver a implementar o serviço web. Para obter mais informações, consulte [gerir um serviço Web através do portal do Azure Machine Learning Web Services](manage-new-webservice.md). 

## <a name="deploy-the-training-experiment-as-a-training-web-service"></a>Implementar a experimentação de preparação como um serviço da web de treinamento
Para voltar a preparar o modelo preparado, deve implantar a experimentação de preparação que criou como um serviço da web Retraining. Este serviço web tem um *saída de serviço Web* módulo ligado ao *[Train Model] [ train-model]* treinados de módulo, para poder gerar novos modelos.

1. Para voltar para a experimentação de preparação, clique no ícone de experimentações no painel esquerdo e clique na experimentação com o nome o modelo de censo.  
2. Na caixa de pesquisa de itens de experiência de pesquisa, escreva o serviço web. 
3. Arrastar um *entrada de serviço Web* módulo para a experimentação baseadas em telas e ligue-se a sua saída para o *Clean Missing Data* módulo.  Isto garante que os seus dados reparametrização são processados da mesma forma que os dados de treinamento original.
4. Arraste dois *saída de serviço web* módulos para a tela de experimentação. Ligue a saída dos *Train Model* módulo para um e o resultado do *Evaluate Model* módulo para outro. A saída do serviço web para **Train Model** nos dá o novo modelo treinado. A saída ligado ao **Evaluate Model** retorna esse módulo de saída, que é que os resultados de desempenho.
5. Clique em **Executar**. 

Em seguida tem de implementar a experimentação de preparação como um serviço web que produz um modelo preparado e resultados de avaliação do modelo. Para tal, o próximo conjunto de ações dependem se estiver a trabalhar com um serviço web clássico ou um novo serviço web.  

**Serviço web clássico**

Na parte inferior da tela de experimentação, clique em **segurança de serviço Web** e selecione **implementar o serviço Web [clássica]**. O serviço Web **Dashboard** é apresentado com a chave de API e a página de ajuda da API para execução de lotes. Apenas o método de execução de lotes pode ser utilizado para a criação de modelos de formação.

**Novo serviço web**

Na parte inferior da tela de experimentação, clique em **segurança de serviço Web** e selecione **implementar o Web Service [novo]**. O portal de serviços de Web do Web Service do Azure Machine Learning abre-se para a página de serviço do web Deploy. Escreva um nome para o seu serviço web e escolha um plano de pagamento, em seguida, clique em **Deploy**. Apenas o método de execução de lotes pode ser utilizado para a criação de modelos de formação

Em ambos os casos, depois de experimentação estiver concluída, o fluxo de trabalho resultante deve ter o seguinte aspeto:

![Resultante fluxo de trabalho após a execução.][4]



## <a name="retrain-the-model-with-new-data-using-bes"></a>Voltar a preparar o modelo com novos dados usando BES
Neste exemplo, estiver a utilizar o C# para criar o aplicativo de reparametrização. Também pode utilizar o código de exemplo do Python ou R para realizar esta tarefa.

Para chamar as APIs de reparametrização:

1. Criar um C# consola de aplicação no Visual Studio: **Novos** > **projeto** > **Visual C#**   >  **ambiente de trabalho clássico do Windows**  >   **Aplicação da consola (.NET Framework)**.
2. Inicie sessão no portal do serviço Web Machine Learning.
3. Se estiver a trabalhar com um serviço web clássico, clique em **serviços Web clássicos**.
   1. Clique no serviço de web que estiver a trabalhar com.
   2. Clique no ponto de final do padrão.
   3. Clique em **consumir**.
   4. Na parte inferior a **Consume** página, além do **código de exemplo** secção, clique em **Batch**.
   5. Continue para o passo 5 deste procedimento.
4. Se estiver a trabalhar com um novo serviço web, clique em **serviços da Web**.
   1. Clique no serviço de web que estiver a trabalhar com.
   2. Clique em **consumir**.
   3. Na parte inferior do Consume página, além da **código de exemplo** secção, clique em **Batch**.
5. Copie o exemplo C# de código para execução de lotes e cole-o no ficheiro Program.cs, certificar-se de que o espaço de nomes permanece intacto.

Adicione o pacote de Nuget Microsoft.AspNet.WebApi.Client conforme especificado nos comentários. Para adicionar a referência ao Microsoft.WindowsAzure.Storage.dll, primeiro precisará de instalar a biblioteca de cliente para serviços de armazenamento do Microsoft Azure. Para obter mais informações, consulte [serviços de armazenamento do Windows](https://www.nuget.org/packages/WindowsAzure.Storage).

### <a name="update-the-apikey-declaration"></a>Atualizar a declaração de apikey
Localize a **apikey** declaração.

    const string apiKey = "abc123"; // Replace this with the API key for the web service

No **informações básicas de consumo** secção a **Consume** página, localize a chave primária e copie-o para o **apikey** declaração.

### <a name="update-the-azure-storage-information"></a>Atualizar as informações de armazenamento do Azure
O código de exemplo BES carrega um ficheiro de uma unidade local (por exemplo "C:\temp\CensusIpnput.csv") ao armazenamento do Azure, processa-os e grava os resultados de volta ao armazenamento do Azure.  

Para realizar esta tarefa, tem de obter as informações de nome, a chave e o contentor da conta de armazenamento para a sua conta de armazenamento a partir do portal clássico do Azure e os valores correspondentes da atualização no código. 

1. Inicie sessão no portal clássico do Azure.
2. Na coluna de navegação esquerda, clique em **armazenamento**.
3. Na lista de contas de armazenamento, selecione um para armazenar o modelo retrained.
4. Na parte inferior da página, clique em **gerir chaves de acesso**.
5. Copie e guarde o **chave de acesso primária** e fechar a caixa de diálogo. 
6. Na parte superior da página, clique em **contentores**.
7. Selecione um contentor existente ou crie um novo e guarde o nome.

Localize a *StorageAccountName*, *StorageAccountKey*, e *StorageContainerName* declarações e atualize os valores que guardou a partir do portal do Azure.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure Storage Account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage Key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage Container name

Também deve verificar que o ficheiro de entrada está disponível na localização que especificar no código. 

### <a name="specify-the-output-location"></a>Especifique a localização de saída
Ao especificar a localização de saída no Payload do pedido, a extensão do arquivo especificado na *RelativeLocation* deve ser especificado como ilearner. 

Veja o seguinte exemplo:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you would like to use for your output file, and valid file extension (usually .csv for scoring results, or .ilearner for trained models)*/
            }
        },

> [!NOTE]
> Os nomes de suas localizações de saída podem ser diferentes nestas instruções com base na ordem em que adicionou os módulos de saída do serviço web. Uma vez que configurar esta experimentação de preparação com duas saídas, os resultados incluem informações de localização de armazenamento para ambos os parâmetros.  
> 
> 

![Saída de reparametrização][6]

Diagrama de 4: Saída de reparametrização.

## <a name="evaluate-the-retraining-results"></a>Avaliar os resultados de reparametrização
Quando executar o aplicativo, a saída inclui o token de URL e SAS necessário para acessar os resultados da avaliação.

Pode ver os resultados de desempenho do modelo retrained ao combinar a *BaseLocation*, *RelativeLocation*, e *SasBlobToken* nos resultados da saída para *output2* (conforme mostrado na imagem de saída de reparametrização anterior) e colar o URL completo na barra de endereço do browser.  

Examine os resultados para determinar se o modelo treinado recentemente executa bem o suficiente para substituir a existente.

Copiar o *BaseLocation*, *RelativeLocation*, e *SasBlobToken* nos resultados da saída, irá usá-los durante o processo de reparametrização.

## <a name="next-steps"></a>Passos Seguintes
Se tiver implementado o serviço web preditivo clicando **implementar o serviço Web [clássica]**, consulte [voltar a preparar um serviço web clássico](retrain-a-classic-web-service.md).

Se tiver implementado o serviço web preditivo clicando **implementar o Web Service [novo]**, consulte [voltar a preparar um novo serviço web utilizando os cmdlets de gestão de Machine Learning](retrain-new-web-service-using-powershell.md).

<!-- Retrain a New web service using the Machine Learning Management REST API -->


[1]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
