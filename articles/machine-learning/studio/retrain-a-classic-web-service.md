---
title: Reenviar um serviço da web clássico - Azure Machine Learning Studio | Documentos da Microsoft
description: Saiba como voltar a preparar um modelo e atualize o serviço web para utilizar o modelo treinado recentemente no Azure Machine Learning programaticamente.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=yahajiza, author=YasinMSFT)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: e36e1961-9e8b-4801-80ef-46d80b140452
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.openlocfilehash: e94fa4b6382a4672e5763b382a22fb812991cb03
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/26/2018
ms.locfileid: "52308475"
---
# <a name="retrain-a-classic-web-service"></a>Voltar a preparar um Serviço Web Clássico
O serviço Web preditivo implementou predefinição é o ponto final de classificação. Pontos de extremidade padrão são mantidos em sincronização com a formação original e experiências de classificação e, portanto, o modelo preparado para o ponto final predefinido não pode ser substituído. Para voltar a preparar o serviço web, tem de adicionar um novo ponto final para o serviço web. 

## <a name="prerequisites"></a>Pré-requisitos
Deverá ter sido configurada uma experimentação de preparação e de uma experimentação preditiva como mostrado na [Retrain Machine Learning dos modelos](retrain-models-programmatically.md). 

> [!IMPORTANT]
> A experimentação preditiva tem de ser implementada como um serviço web de aprendizagem automática de clássico. 
> 
> 

Para obter informações adicionais em Implantando serviços da web, consulte [implementar um serviço web do Azure Machine Learning](publish-a-machine-learning-web-service.md).

## <a name="add-a-new-endpoint"></a>Adicionar um novo ponto final
O serviço Web preditivo que implementou contém um padrão de ponto final que é mantido em sincronia com a formação original de classificação e modelo treinado de experimentações de classificação. Para atualizar o seu serviço web com um novo modelo preparado, tem de criar um novo ponto de final de classificação. 

Para criar um novo ponto de final classificação, no serviço Web preditivo que podem ser atualizados com o modelo treinado:

> [!NOTE]
> Certifique-se de que está a adicionar o ponto de extremidade ao serviço Web preditivo, não o serviço da Web de treinamento. Se tiver implementado um treinamento e um serviço Web preditivo corretamente, deverá ver dois serviços da web separado listados. O serviço Web preditivo deve terminar com "[exp preditiva.]".
> 
> 

Existem duas formas na qual pode adicionar um novo ponto final a um serviço web:

1. Através de programação
2. Utilizar o portal de serviços da Web do Microsoft Azure

### <a name="programmatically-add-an-endpoint"></a>Adicionar programaticamente um ponto final
Pode adicionar pontos finais de classificação usando o código de exemplo fornecido neste [repositório do github](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint).

### <a name="use-the-microsoft-azure-web-services-portal-to-add-an-endpoint"></a>Utilizar o portal de serviços da Web do Microsoft Azure para adicionar um ponto final
1. No Machine Learning Studio, na coluna de navegação esquerda, clique em serviços da Web.
2. Na parte inferior do dashboard do serviço web, clique em **pré-visualização de pontos finais de gerir**.
3. Clique em **Adicionar**.
4. Escreva um nome e descrição para o novo ponto final. Selecione o nível de registo e se os dados de exemplo estão ativados. Para obter mais informações sobre o registo, consulte [ativar o registo de serviços web Machine Learning](web-services-logging.md).

## <a name="update-the-added-endpoints-trained-model"></a>Atualizar o modelo treinado do ponto de extremidade adicionado
Para concluir o processo de reparametrização, tem de atualizar o modelo treinado do novo ponto final que adicionou.

Se tiver adicionado o ponto final com o código de exemplo, isto inclui a localização do URL de ajuda identificado pelos *HelpLocationURL* valor na saída.

Para obter o URL de caminho:

1. Copie e cole o URL no seu browser.
2. Clique na ligação de recursos de atualização.
3. Copie o URL de publicação do pedido PATCH. Por exemplo:
   
     URL DE PATCH: https://management.azureml.net/workspaces/00bf70534500b34rebfa1843d6/webservices/af3er32ad393852f9b30ac9a35b/endpoints/newendpoint2

Agora, pode utilizar o modelo preparado para atualizar o ponto de final de classificação que criou anteriormente.

O código de exemplo seguinte mostra como utilizar o *BaseLocation*, *RelativeLocation*, *SasBlobToken*e o URL de aplicar o PATCH para atualizar o ponto final.

    private async Task OverwriteModel()
    {
        var resourceLocations = new
        {
            Resources = new[]
            {
                new
                {
                    Name = "Census Model [trained model]",
                    Location = new AzureBlobDataReference()
                    {
                        BaseLocation = "https://esintussouthsus.blob.core.windows.net/",
                        RelativeLocation = "your endpoint relative location", //from the output, for example: “experimentoutput/8946abfd-79d6-4438-89a9-3e5d109183/8946abfd-79d6-4438-89a9-3e5d109183.ilearner”
                        SasBlobToken = "your endpoint SAS blob token" //from the output, for example: “?sv=2013-08-15&sr=c&sig=37lTTfngRwxCcf94%3D&st=2015-01-30T22%3A53%3A06Z&se=2015-01-31T22%3A58%3A06Z&sp=rl”
                    }
                }
            }
        };

        using (var client = new HttpClient())
        {
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", apiKey);

            using (var request = new HttpRequestMessage(new HttpMethod("PATCH"), endpointUrl))
            {
                request.Content = new StringContent(JsonConvert.SerializeObject(resourceLocations), System.Text.Encoding.UTF8, "application/json");
                HttpResponseMessage response = await client.SendAsync(request);

                if (!response.IsSuccessStatusCode)
                {
                    await WriteFailedResponse(response);
                }

                // Do what you want with a successful response here.
            }
        }
    }

O *apiKey* e o *endpointUrl* para a chamada pode ser obtida a partir do dashboard de ponto final.

O valor do *Name* parâmetro na *recursos* deve corresponder ao nome do recurso do modelo guardado preparado na experimentação preditiva. Para obter o nome do recurso:

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No menu à esquerda, clique em **Machine Learning**.
3. No nome, clique em sua área de trabalho e, em seguida, clique em **serviços da Web**.
4. Em nome, clique em **recenseamento modelo [exp preditiva].** .
5. Clique em novo ponto final que adicionou.
6. No dashboard do ponto final, clique em **recursos de atualização**.
7. Na página de documentação de API do recurso de atualização para o serviço web, pode encontrar os **nome do recurso** sob **recursos Atualizáveis**.

Se o token SAS expirar antes de terminar de atualizar o ponto final, tem de executar um GET com o Id da tarefa para obter um novo token.

Quando o código tenha sido executada com êxito, o novo ponto final deve começar a utilizar o modelo retrained em aproximadamente 30 segundos.

## <a name="summary"></a>Resumo
Com as APIs de reparametrização, pode atualizar o modelo treinado de um serviço Web preditivo permitir cenários, tais como:

* Modelo periódico reparametrização com novos dados.
* Distribuição de um modelo para os clientes com o objetivo de informá-los de voltar a preparar o modelo usando seus próprios dados.

## <a name="next-steps"></a>Passos Seguintes
[Resolução de problemas relativos a novas preparações de um serviço web clássico do Azure Machine Learning](troubleshooting-retraining-models.md)

