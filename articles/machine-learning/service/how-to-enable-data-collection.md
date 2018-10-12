---
title: Ativar a recolha de dados de modelos em produção - Azure Machine Learning
description: Saiba como recolher dados de entrada de modelo do Azure Machine Learning num armazenamento de Blobs do Azure.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: marthalc
author: marthalc
ms.date: 09/24/2018
ms.openlocfilehash: 70c023fc8fe996060d3eff3d5a700b5f910097b4
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/11/2018
ms.locfileid: "49113636"
---
# <a name="collect-data-for-models-in-production"></a>Recolher dados para modelos em produção

Neste artigo, pode aprender a recolher dados de entrada de modelo dos serviços do Azure Machine Learning que implementou no Cluster de Kubernetes do Azure (AKS) para um armazenamento de Blobs do Azure. 

Uma vez ativada, estes dados a que recolher ajudam-o:
* Monitorizar dados drifts como dados de produção inserem seu modelo

* Tome decisões melhores sobre quando voltar a preparar ou otimizar o seu modelo

* Voltar a preparar seu modelo com os dados recolhidos

## <a name="what-is-collected-and-where-does-it-go"></a>O que é recolhido e onde ela vai?

Os seguintes dados podem ser recolhidos:
* Modelo **entrada** dados de serviços web implementados no Cluster de Kubernetes do Azure (AKS) (voz, imagens e vídeo são **não** recolhidos) 
  
* Predições de modelos com dados de entrada de produção.

> [!Note]
> Pré-agregação ou cálculos anteriores sobre estes dados não fazem parte do serviço neste momento.   

A saída é salvo num Blob do Azure. Uma vez que os dados são adicionados a um Blob do Azure, em seguida, pode escolher sua ferramenta favorita para executar a análise. 

O caminho para os dados de saída no blob segue esta sintaxe:

```
/modeldata/<subscriptionid>/<resourcegroup>/<workspace>/<webservice>/<model>/<version>/<identifier>/<year>/<month>/<day>/data.csv
# example: /modeldata/1a2b3c4d-5e6f-7g8h-9i10-j11k12l13m14/myresourcegrp/myWorkspace/aks-w-collv9/best_model/10/inputs/2018/12/31/data.csv
```

## <a name="prerequisites"></a>Pré-requisitos

- Uma subscrição do Azure. Se não tiver uma, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

- Uma Azure Machine Learning serviço área de trabalho, um diretório local que contém os scripts e o SDK do Azure Machine Learning para Python instalada. Saiba como obter estes pré-requisitos com o [como configurar um ambiente de desenvolvimento](how-to-configure-environment.md) documento.

- Um modelo de aprendizagem de máquina preparado para ser implementada para o Azure Kubernetes Service (AKS). Se não tiver uma, veja a [preparar o modelo de classificação de imagem](tutorial-train-models-with-aml.md) tutorial.

- Uma [cluster do AKS](how-to-deploy-to-aks.md).

- As seguintes dependências e módulo instalado [no seu ambiente](how-to-configure-environment.md):
  + No Linux:
    ```shell
    sudo apt-get install libxml++2.6-2v5
    pip install azureml-monitoring
    ```

  + No Windows:
    ```shell
    pip install azureml-monitoring
    ```

## <a name="enable-data-collection"></a>Ativar a recolha de dados
Recolha de dados pode ser ativada, independentemente do modelo que está a ser implementado através do serviço do Azure Machine Learning ou outras ferramentas. 

Para ativá-la, terá de:

1. Abra o ficheiro de classificação. 

1. Adicione o seguinte código na parte superior do ficheiro:

   ```python 
   from azureml.monitoring import ModelDataCollector
   ```

2. Declarar as variáveis de coleção de dados no seu `init()` função:

    ```python
    global inputs_dc, prediction_dc
    inputs_dc = ModelDataCollector("best_model", identifier="inputs", feature_names=["feat1", "feat2", "feat3". "feat4", "feat5", "feat6"])
    prediction_dc = ModelDataCollector("best_model", identifier="predictions", feature_names=["prediction1", "prediction2"])
    ```

    *CorrelationId* é um parâmetro opcional, não é necessário configurá-lo se seu modelo não requê-lo. Ter uma correlationId em vigor ajudá-lo para o mapeamento mais fácil com outros dados. (Os exemplos incluem: LoanNumber, CustomerId, etc.)
    
    *Identificador* posteriormente utilizado para criar a estrutura de pasta no seu Blob, pode ser utilizado para dividir os dados "brutos" versus "processados".

3.  Adicione as seguintes linhas de código para o `run(input_df)` função:

    ```python
    data = np.array(data)
    result = model.predict(data)
    inputs_dc.collect(data) #this call is saving our input data into Azure Blob
    prediction_dc.collect(result) #this call is saving our input data into Azure Blob
    ```

4. Recolha de dados foi **não** automaticamente definido para **verdadeiro** quando implementa um serviço no AKS, por isso, tem de atualizar o ficheiro de configuração, tais como: 

    ```python
    aks_config = AksWebservice.deploy_configuration(collect_model_data=True)
    ```
    Também pode ser ativado AppInsights para o monitoramento de serviço ao alterar esta configuração:
    ```python
    aks_config = AksWebservice.deploy_configuration(collect_model_data=True, enable_app_insights=True)
    ``` 

5. [Criar nova imagem e implementar o seu serviço.](how-to-deploy-to-aks.md) 


Se já tiver um serviço com as dependências instaladas no seu **ficheiro de ambiente** e **ficheiro de classificação**, ativar a recolha de dados por:

1. Aceda a [Portal do Azure](https://portal.azure.com).

1. Abra a área de trabalho.

1. Aceda a **implementações** -> **selecionar serviço** -> **editar**.

   ![Editar serviço](media/how-to-enable-data-collection/EditService.png)

1. Na **definições avançadas**, Desselecionar **recolha de dados de modelo permitir**. 

   ![Desmarque a opção de recolha de dados](media/how-to-enable-data-collection/CheckDataCollection.png)

   Nessa janela, também pode optar por "Ativar o diagnóstico do Appinsights" para controlar o estado de funcionamento do seu serviço.  

1. Selecione **atualização** para aplicar a alteração.


## <a name="disable-data-collection"></a>Desativar a recolha de dados
Pode parar a recolha de dados de qualquer altura. Utilize o código de Python ou o portal do Azure para desativar a recolha de dados.

+ Opção 1 - desativar no portal do Azure: 
  1. Inicie sessão no [portal do Azure](https://portal.azure.com).

  1. Abra a área de trabalho.

  1. Aceda a **implementações** -> **selecionar serviço** -> **editar**.

     ![Editar serviço](media/how-to-enable-data-collection/EditService.png)

  1. Na **definições avançadas**, Desselecionar **recolha de dados de modelo permitir**. 

     ![Desmarque a opção de recolha de dados](media/how-to-enable-data-collection/UncheckDataCollection.png) 

  1. Selecione **atualização** para aplicar a alteração.

* Opção 2 – utilizar o Python para desativar a recolha de dados:

  ```python 
  ## replace <service_name> with the name of the web service
  <service_name>.update(collect_model_data=False)
  ```

## <a name="example-notebook"></a>Bloco de notas de exemplo

O [00. obtendo Started/12.enable-data-collection-for-models-in-aks.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/12.enable-data-collection-for-models-in-aks) bloco de notas demonstra conceitos deste artigo.  

Obter este bloco de notas:
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]