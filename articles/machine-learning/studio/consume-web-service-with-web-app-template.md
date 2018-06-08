---
title: Consumir um serviço web do Machine Learning utilizando um modelo de aplicação web | Microsoft Docs
description: Utilize um modelo de aplicação web no Azure Marketplace para consumir um serviço web preditiva no Azure Machine Learning.
keywords: serviço Web, operationalization, REST API, machine learning
services: machine-learning
documentationcenter: ''
author: YasinMSFT
ms.author: yahajiza
manager: hjerez
editor: cgronlun
ms.assetid: e0d71683-61b9-4675-8df5-09ddc2f0d92d
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.openlocfilehash: 03729a5b94b355869367e7f356e299f9afe38f75
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "34835003"
---
# <a name="consume-an-azure-machine-learning-web-service-by-using-a-web-app-template"></a>Consumir um serviço web Azure Machine Learning utilizando um modelo de aplicação web

Pode desenvolver um modelo preditivo e implementá-lo como um serviço web do Azure utilizando:
- Azure Machine Learning Studio.
- Ferramentas como o R ou Python. 

Depois disso, pode aceder ao modelo de operacionalizado, utilizando uma API REST.

Existem várias formas de utilizar a API REST e aceder ao serviço web. Por exemplo, pode escrever uma aplicação em c#, R ou Python utilizando o código de exemplo gerado quando implementou o serviço web. (O código de exemplo está disponível no [portal de serviços Web Machine Learning](https://services.azureml.net/quickstart) ou no dashboard do serviço web no Machine Learning Studio.) Ou pode utilizar o livro do Microsoft Excel de exemplo criado para si ao mesmo tempo.

Mas é a forma mais rápida e mais fácil de aceder ao seu serviço web através de modelos de aplicação web disponíveis no [Azure Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/).

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="azure-machine-learning-web-app-templates"></a>Modelos de aplicação de web do Azure Machine Learning
Modelos de aplicação web disponíveis no Azure Marketplace, podem criar uma aplicação web personalizada que sabe o seu serviço web dados de entrada e resultados esperados. Tudo o que precisa de fazer é permitir o acesso de aplicação web ao seu serviço web e os dados, e o modelo não o resto.

Estão disponíveis dois modelos:

* [Modelo de aplicação do Azure ML resposta-pedido serviço Web](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
* [Modelo de aplicação de Web de serviço de execução de lote do Azure ML](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

Cada modelo cria um exemplo de aplicação ASP.NET, utilizando o URI de API e chave para o seu serviço web. O modelo, em seguida, implementa a aplicação como um Web site do Azure. 

O modelo de serviço de resposta-pedido (RRS) cria uma aplicação web que pode utilizar para enviar uma única linha de dados para o serviço web para obter um resultado único. O modelo de serviço de execução de lote (BES) cria uma aplicação web que pode utilizar para enviar o número de linhas de dados para obter resultados vários.

Sem codificação é necessária para utilizar estes modelos. Basta fornecer a chave de API e o URI e o modelo baseia-se a aplicação por si.

Para obter a chave de API e URI de pedido para um serviço web:

1. No [portal dos serviços Web](https://services.azureml.net/quickstart), selecione **serviços Web** na parte superior. Ou para um serviço web clássico, selecione **serviços Web clássicos**.
2. Selecione o serviço web que pretende aceder.
3. Para um serviço web clássico, selecione o ponto final que pretende aceder.
4. Selecione **Consume** na parte superior.
5. Copie a chave primária ou secundária e guardá-lo.
6. Se estiver a criar um modelo RRS, copie o **pedido-resposta** URI e guardá-lo. Se estiver a criar um modelo de BES, copie o **pedidos de Batch** URI e guardá-lo.


## <a name="how-to-use-the-request-response-service-template"></a>Como utilizar o modelo de serviço de resposta-pedido
Siga estes passos para utilizar o modelo de aplicação de web RRS, conforme mostrado no diagrama seguinte.

![Processo para utilizar o modelo de web RRS][image1]


<!--    ![API Key][image3] -->

<!-- This value will look like this:
   
        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true
   
    ![Request URI][image4] -->

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **novo**, procure e selecione **do Azure ML resposta-pedido Web do App Service**e, em seguida, selecione **criar**. 
3. No **criar** painel:
   
   * Dê um nome exclusivo da aplicação web. O URL da aplicação web será este nome seguido **. azurewebsites.net**. Um exemplo é **http://carprediction.azurewebsites.net**.
   * Selecione a subscrição do Azure e serviços em que o seu serviço web está em execução.
   * Selecione **Criar**.
     
   ![Criar aplicação Web][image5]

4. Quando o Azure foi concluída a implementação da aplicação web, selecione o **URL** nas definições da aplicação web página no Azure, ou introduza o URL num browser. Por exemplo, introduza **http://carprediction.azurewebsites.net**.
5. Quando a aplicação web é executada pela primeira vez, este pede-lhe para o **API Post URL** e **chave de API**. Introduza os valores que guardou anteriormente (solicitar a chave de URI e a API, respetivamente). Selecione **submeter**.
     
   ![Introduza post chave URI e a API][image6]

6. A aplicação de web apresenta o **configuração de aplicação Web** página com as definições do serviço web atual. Aqui pode efetuar alterações às definições que utiliza a aplicação web.
   
   > [!NOTE]
   > Alterar as definições aqui apenas altera-los para esta aplicação web. Não altera as definições predefinidas do seu serviço web. Por exemplo, se alterar o texto no **Descrição** aqui, não altera a descrição a apresentar no dashboard do serviço web no Machine Learning Studio.
   > 
   > 
   
    Quando estiver pronto, selecione **guardar alterações**e, em seguida, selecione **aceda à Home Page**.

7. Na home page, pode introduzir valores para enviar ao seu serviço web. Selecione **submeter** quando tiver terminado, e o resultado será devolvido.

Se pretender regressar ao **configuração** página, vá para o **setting.aspx** página da aplicação web. Por exemplo, aceda a **http://carprediction.azurewebsites.net/setting.aspx**. É-lhe pedido que introduza novamente a chave de API. É necessário que para aceder à página e atualizar as definições.

Pode parar, reiniciar ou eliminar a aplicação web no portal do Azure como qualquer outra aplicação web. Enquanto estiver em execução, pode navegar para o endereço web de raiz e introduza novos valores.

## <a name="how-to-use-the-batch-execution-service-template"></a>Como utilizar o modelo de serviço de execução do Batch
Pode utilizar o modelo de aplicação web BES da mesma forma que o modelo RRS. A diferença é que pode utilizar a aplicação web criada para submeter várias linhas de dados e receber os resultados de múltiplos.

Os valores de entrada para um serviço de web de execução do batch podem ser proveniente de armazenamento do Azure ou um ficheiro local. Os resultados são armazenados no contentor de armazenamento do Azure. Por isso, é necessário um contentor de armazenamento do Azure para armazenar os resultados que devolve a aplicação web. Também terá de preparar os dados de entrada.

![Processo para utilizar BES web modelo][image2]

1. Siga o mesmo procedimento para criar a aplicação de web BES para o modelo RRS. Mas neste caso, aceda a [modelo de aplicação ao Web do serviço de execução do Azure ML Batch](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) para abrir o modelo de BES no Azure Marketplace. Selecione **Criar aplicação Web**.

2. Para especificar onde pretende que os resultados armazenados, introduza as informações de contentor de destino na página inicial da aplicação web. Também especificar onde a aplicação web pode obter os valores de entrada: num ficheiro local ou um contentor de armazenamento do Azure.
   Selecione **submeter**.
   
   ![Informações de armazenamento][image7]

A aplicação web apresenta uma página com o estado da tarefa. Quando a tarefa estiver concluída, pode obter a localização dos resultados no Blob storage do Azure. Também tem a opção de transferir os resultados num ficheiro local.

## <a name="for-more-information"></a>Para obter mais informações:
Para saber mais sobre:

* Criar uma experimentação de aprendizagem com Machine Learning Studio, consulte [criar a sua primeira experimentação no Azure Machine Learning Studio](create-experiment.md).
* Como implementar a sua experimentação de aprendizagem como um serviço web, consulte [implementar um serviço web do Azure Machine Learning](publish-a-machine-learning-web-service.md).
* Outras formas de aceder ao seu serviço web, consulte [como consumir um serviço web Azure Machine Learning](consume-web-services.md).

[image1]: media/consume-web-service-with-web-app-template/rrs-web-template-flow.png
[image2]: media/consume-web-service-with-web-app-template/bes-web-template-flow.png
[image3]: media/consume-web-service-with-web-app-template/api-key.png
[image4]: media/consume-web-service-with-web-app-template/post-uri.png
[image5]: media/consume-web-service-with-web-app-template/create-web-app.png
[image6]: media/consume-web-service-with-web-app-template/web-service-info.png
[image7]: media/consume-web-service-with-web-app-template/storage.png
