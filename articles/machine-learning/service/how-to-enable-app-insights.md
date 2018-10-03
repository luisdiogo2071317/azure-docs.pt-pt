---
title: Ativar o application insights para o serviço Azure Machine Learning na produção
description: Saiba como configurar o Application Insights para o serviço do Azure Machine Learning que foram implementadas no serviço Kubernetes do Azure
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: marthalc
author: marthalc
ms.date: 10/01/2018
ms.openlocfilehash: 8fbddd7bca8750317490c35473730ff11f07374c
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/03/2018
ms.locfileid: "48243026"
---
# <a name="monitor-your-azure-machine-learning-models-in-production-with-application-insights"></a>Monitorizar os seus modelos do Azure Machine Learning na produção com o Application Insights

Neste artigo, pode aprender a configurar **Application Insights** para a sua **do Azure Machine Learning** serviço. Uma vez ativada, o Application Insights dá-lhe a oportunidade para monitorizar:
* Pedido de taxas, tempos de resposta e taxas de falhas
* Taxas de dependência, tempos de resposta e taxas de falhas
* Exceções

Saiba mais sobre o Application Insights [aqui](../../application-insights/app-insights-overview.md). 

## <a name="prerequisites"></a>Pré-requisitos
* Uma subscrição do Azure. Se não tiver uma, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.
* Trabalho do Azure Machine Learning, um diretório local que contém os scripts e o SDK do Azure Machine Learning para Python instalada. Saiba como obter estes pré-requisitos com o [como configurar um ambiente de desenvolvimento](how-to-configure-environment.md) documento.
* Um modelo de aprendizagem de máquina preparado para ser implementada para o Azure Kubernetes Service (AKS). Se não tiver uma, veja a [preparar o modelo de classificação de imagem](tutorial-train-models-with-aml.md) tutorial.
* Uma [cluster do AKS](how-to-deploy-to-aks.md).

## <a name="enable--disable-in-the-portal"></a>Ativar e desativar no portal

Pode ativar e desativar o Application Insights no portal do Azure.

### <a name="enable"></a>Ativar

1. Na [portal do Azure](https://portal.azure.com), abra a área de trabalho.

1. Ir para as suas implementações e selecione o serviço em que pretende ativar o Application Insights.

   [![Implementações](media/how-to-enable-app-insights/Deployments.PNG)](./media/how-to-enable-app-insights/Deployments.PNG#lightbox)

3. Clique em **edite** e aceda à **definições avançadas**.

   [![Editar](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

4. Na **definições avançadas** selecionar **diagnóstico de ativar o Application Insights**.

   [![Editar](media/how-to-enable-app-insights/AdvancedSettings.png)](./media/how-to-enable-app-insights/AdvancedSettings.png)#lightbox)

1. Selecione **atualização** na parte inferior do ecrã, para aplicar as alterações. 

### <a name="disable"></a>Desativar
Para desativar o Application Insights no portal do Azure, siga os passos abaixo:

1. Inicie sessão portal do Azure em https://portal.azure.com.
1. Vá para a área de trabalho.
1. Escolher **implementações**, em seguida, **selecionar serviço**e, em seguida **editar**.

   [![Editar](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

1. Na **definições avançadas**, desmarque a opção **diagnóstico do AppInsights ativar**. 

   [![Desmarque a opção](media/how-to-enable-app-insights/uncheck.png)](./media/how-to-enable-app-insights/uncheck.png#lightbox)

1. Selecione **atualização** na parte inferior do ecrã, para aplicar as alterações. 

## <a name="enable--disable-from-the-sdk"></a>Ativar e desativar a partir do SDK

### <a name="update-a-deployed-service"></a>Atualizar um serviço implementado
1. Identificar o serviço na sua área de trabalho (ws = o nome da sua área de trabalho)

    ```python
    aks_service= Webservice(ws, "my-service-name")
    ```
2. Atualizar o seu serviço e ativar o Application Insights. 

    ```python
    aks_service.update(enable_app_insights=True)
    ```

### <a name="log-custom-traces-in-your-service"></a>Rastreios personalizados no seu serviço
Se quiser rastreios de pedido personalizado, deve seguir a [processo de implementação padrão para o AKS](how-to-deploy-to-aks.md) e irá:

1. Atualize ficheiro de classificação ao adicionar instruções de impressa.
    
    ```python
    print ("model initialized" + time.strftime("%H:%M:%S"))
    ```

2. Atualize a configuração do aks.
    
    ```python
    aks_config = AksWebservice.deploy_configuration(enable_app_insights=True)
    ```

3. [Criar imagem e implementá-la.](how-to-deploy-to-aks.md)  

### <a name="disable-tracking-in-python"></a>Desativar o controle no Python

Para desativar o Application Insights, utilize o seguinte código:

```python 
## replace <service_name> with the name of the web service
<service_name>.update(enable_app_insights=False)
```
    

## <a name="evaluate-data"></a>Avaliar dados
Os dados do seu serviço são armazenados na sua conta do Application Insights dentro do mesmo grupo de recursos que tenha a sua área de trabalho do serviço do Azure Machine Learning.
Para vê-la:
1. Aceda ao seu grupo de recursos no [portal do Azure](https://portal.azure.com) e clique no recurso do Application Insights. 
2. O **descrição geral** Guia mostra-lhe o conjunto básico de métricas para o seu serviço.

   [![Descrição geral](media/how-to-enable-app-insights/overview.png)](./media/how-to-enable-app-insights/overview.png#lightbox)

3. Para ver os rastreios personalizados clique **Analytics**.
4. Dentro da seção de esquema, clique em **rastreios** e, em seguida **executar** sua consulta. Os dados devem aparecer num formato de tabela abaixo para baixo e devem ser mapeada para suas chamadas personalizadas no seu ficheiro de classificação. 

   [![Rastreios personalizados](media/how-to-enable-app-insights/logs.png)](./media/how-to-enable-app-insights/logs.png#lightbox)

Clique em [aqui](../../application-insights/app-insights-overview.md) para saber mais sobre como utilizar o Application Insights.
    

## <a name="example-notebook"></a>Bloco de notas de exemplo

O `00.Getting Started/13.enable-app-insights-in-production-service.ipynb` bloco de notas demonstra conceitos deste artigo.  Obter este bloco de notas:
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Passos Seguintes
Também é possível recolher dados em seus modelos em produção. Leia o artigo [recolher dados para modelos em produção](how-to-enable-data-collection.md) 
