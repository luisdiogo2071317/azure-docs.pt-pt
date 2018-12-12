---
title: Consumir o serviço web com o modelo de aplicação web - Azure Machine Learning Studio | Documentos da Microsoft
description: Utilize um modelo de aplicação web no Azure Marketplace para consumir um serviço web preditivo no Azure Machine Learning.
keywords: serviço Web, operacionalização, API de REST do machine learning
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: seodec18
ms.author: amlstudiodocs
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
ms.openlocfilehash: 3eaf881ea1b8831891588310bfcec5e8fb146978
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53078587"
---
# <a name="consume-an-azure-machine-learning-web-service-by-using-a-web-app-template"></a>Consumir um serviço web Azure Machine Learning utilizando um modelo de aplicação web

Pode desenvolver um modelo preditivo e implementá-la como um serviço web do Azure ao utilizar:
- Azure Machine Learning Studio.
- Ferramentas como o R ou Python. 

Depois disso, pode acessar o modelo operacionalizado com uma API de REST.

Existem várias formas de utilizar a API REST e aceder ao serviço web. Por exemplo, pode escrever uma aplicação C#, R ou Python com o código de exemplo gerado quando implementou o serviço web. (O código de exemplo está disponível na [portal de serviços Web Machine Learning](https://services.azureml.net/quickstart) ou no dashboard do serviço web no Machine Learning Studio.) Em alternativa, pode utilizar o livro do Microsoft Excel de exemplo criado para si, ao mesmo tempo.

Mas a maneira mais rápida e fácil de acessar o serviço web é através de modelos de aplicação web disponíveis na [do Azure Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/).

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="azure-machine-learning-web-app-templates"></a>Modelos de aplicação de web do Azure Machine Learning
Modelos de aplicações web disponíveis no Azure Marketplace, podem criar uma aplicação web personalizado que sabe que os dados de entrada e os resultados esperados do seu serviço da web. Tudo o que precisa fazer é permitir o acesso a aplicações web ao seu serviço web e os dados e o modelo faz o resto.

Dois modelos estão disponíveis:

* [Modelo de aplicação Web de serviço do Azure ML de solicitação-resposta](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
* [Modelo de aplicação de Web de serviço de execução de lote do Azure ML](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

Cada modelo cria um exemplo de aplicação de ASP.NET com o URI de API e a chave para o seu serviço web. O modelo, em seguida, implementa a aplicação como um Web site para o Azure. 

O modelo de serviço de solicitação-resposta (RRS) cria uma aplicação web que pode utilizar para enviar uma única linha de dados para o serviço web para obter um resultado único. O modelo de serviço de execução de lote (BES) cria uma aplicação web que pode utilizar para enviar o número de linhas de dados para obter vários resultados.

Sem codificação é necessário para utilizar estes modelos. Apenas fornecer a chave de API e o URI e o modelo baseia-se o aplicativo para.

Para obter a chave de API e o URI do pedido para um serviço web:

1. Na [portal dos serviços Web](https://services.azureml.net/quickstart), selecione **serviços da Web** na parte superior. Ou para um serviço web clássico, selecione **serviços Web clássicos**.
2. Selecione o serviço web que pretende aceder.
3. Para um serviço web clássico, selecione o ponto final que pretende aceder.
4. Selecione **Consume** na parte superior.
5. Copie a chave primária ou secundária e guardá-lo.
6. Se estiver a criar um modelo RRS, copie os **solicitação-resposta** URI e guarde-o. Se estiver a criar um modelo de BES, copie os **pedidos de lote** URI e guarde-o.


## <a name="how-to-use-the-request-response-service-template"></a>Como utilizar o modelo de serviço de solicitação-resposta
Siga estas etapas para usar o modelo de aplicativo de web RRS, conforme mostrado no diagrama seguinte.

![Processo para utilizar o modelo de web RRS][image1]


<!--    ![API Key][image3] -->

<!-- This value will look like this:
   
        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true
   
    ![Request URI][image4] -->

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **New**, procure e selecione **aplicação de Web do serviço de solicitação-resposta do Azure ML**e, em seguida, selecione **criar**. 
3. Na **criar** painel:
   
   * Dê um nome exclusivo ao seu aplicativo web. O URL da aplicação web será este nome seguido **. azurewebsites.net**. Um exemplo é **http://carprediction.azurewebsites.net**.
   * Selecione a subscrição do Azure e serviços na qual o seu serviço web está a ser executada.
   * Selecione **Criar**.
     
   ![Criar aplicação Web][image5]

4. Quando o Azure tiver concluído a implementação da aplicação web, selecione o **URL** nas definições da aplicação web página no Azure, ou introduza o URL num navegador da web. Por exemplo, introduza **http://carprediction.azurewebsites.net**.
5. Quando a aplicação web é executada pela primeira vez, ele solicita que para o **URL de publicação de API** e **chave de API**. Introduza os valores que guardou anteriormente (solicitar chave do URI e a API, respectivamente). Selecione **submeter**.
     
   ![Após introduzir chave de URI e a API][image6]

6. A aplicação de web apresenta seus **configuração da aplicação Web** página com as definições do serviço web atual. Aqui pode efetuar alterações às definições de que a aplicação web utiliza.
   
   > [!NOTE]
   > Alterar as configurações só podem ser alterados-los para esta aplicação web. Ele não altera as configurações padrão do seu serviço web. Por exemplo, se alterar o texto na **Descrição** aqui, ele não altera a descrição apresentada no dashboard do serviço web no Machine Learning Studio.
   > 
   > 
   
    Quando tiver terminado, selecione **guardar alterações**e, em seguida, selecione **vá para a Home Page**.

7. Na home page, pode introduzir valores para enviar ao seu serviço web. Selecione **submeter** quando tiver terminado, e o resultado será devolvido.

Se quiser retornar para o **Configuration** página, vá para o **setting.aspx** página da aplicação web. Por exemplo, aceda a **http://carprediction.azurewebsites.net/setting.aspx**. Lhe for pedido para introduzir novamente a chave de API. Precisa para aceder à página e Atualize as definições.

Pode parar, reiniciar ou eliminar a aplicação web no portal do Azure, como qualquer outra aplicação web. Enquanto estiver em execução, pode navegar para o endereço web de raiz e introduza valores novos.

## <a name="how-to-use-the-batch-execution-service-template"></a>Como utilizar o modelo de serviço de execução do Batch
Pode usar o modelo de aplicativo da web de BES da mesma forma como o modelo RRS. A diferença é que pode utilizar a aplicação web criada para submeter várias linhas de dados e receber vários resultados.

Os valores de entrada para um web service de execução do batch podem vir de armazenamento do Azure ou um ficheiro local. Os resultados são armazenados no contentor de armazenamento do Azure. Por isso, precisa de um contentor de armazenamento do Azure para armazenar os resultados que retorna a aplicação web. Também terá de preparar-se os dados de entrada.

![Processo para utilizar o modelo de web BES][image2]

1. Siga o mesmo procedimento para criar a aplicação de web BES como para o modelo RRS. Mas nesse caso, aceda a [modelo de aplicação ao Web do serviço de execução do Azure ML Batch](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) para abrir o modelo de BES no Azure Marketplace. Selecione **Criar aplicação Web**.

2. Para especificar onde pretende que os resultados armazenados, introduza as informações de contentor de destino na home page da aplicação web. Também especificar em que a aplicação web pode obter os valores de entrada: num arquivo local ou num contentor de armazenamento do Azure.
   Selecione **submeter**.
   
   ![Informações de armazenamento][image7]

A aplicação web apresenta uma página com o estado da tarefa. Quando a tarefa é concluída, obtém a localização dos resultados no armazenamento de Blobs do Azure. Tem também a opção de transferir os resultados num ficheiro local.

## <a name="for-more-information"></a>Para obter mais informações:
Para saber mais sobre:

* Criar uma experimentação do machine learning com o Machine Learning Studio, consulte [criar a sua primeira experiência no Azure Machine Learning Studio](create-experiment.md).
* Como implementar a sua experimentação de machine learning como um serviço web, consulte [implementar um web service do Azure Machine Learning](publish-a-machine-learning-web-service.md).
* Outras formas de acessar o serviço web, consulte [como consumir um serviço web Azure Machine Learning](consume-web-services.md).

[image1]: media/consume-web-service-with-web-app-template/rrs-web-template-flow.png
[image2]: media/consume-web-service-with-web-app-template/bes-web-template-flow.png
[image3]: media/consume-web-service-with-web-app-template/api-key.png
[image4]: media/consume-web-service-with-web-app-template/post-uri.png
[image5]: media/consume-web-service-with-web-app-template/create-web-app.png
[image6]: media/consume-web-service-with-web-app-template/web-service-info.png
[image7]: media/consume-web-service-with-web-app-template/storage.png
