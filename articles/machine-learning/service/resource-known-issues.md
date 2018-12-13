---
title: Problemas conhecidos e resolução de problemas
titleSuffix: Azure Machine Learning service
description: Obter uma lista dos problemas conhecidos, soluções alternativas e resolução de problemas do serviço Azure Machine Learning.
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.component: core
ms.topic: article
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 45eb24bb8a49fb59ae44533e59b2760940ee5c1a
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53097711"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Problemas conhecidos e resolução de problemas serviço do Azure Machine Learning
 
Este artigo ajuda-o, encontre e corrija erros ou falhas encontrados ao utilizar o serviço Azure Machine Learning. 

## <a name="sdk-installation-issues"></a>Problemas de instalação do SDK

**Mensagem de erro: não é possível desinstalar 'PyYAML'** 

SDK de Aprendizado de máquina do Azure para Python: PyYAML é um projeto de distutils instalado. Por conseguinte, não é possível com precisão determinar quais os ficheiros que pertencem ao mesmo em caso de uma desinstalação parcial. Para continuar a instalação do SDK ao ignorar este erro, utilize:
```Python 
pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
```

## <a name="trouble-creating-azure-machine-learning-compute"></a>Problemas ao criar a computação do Azure Machine Learning
Há uma chance de rara que alguns usuários que criou a sua área de trabalho do Azure Machine Learning do portal do Azure antes do lançamento de DG poderão não conseguir criar a computação do Azure Machine Learning nessa área de trabalho. Pode emitir um pedido de suporte com o serviço ou criar uma nova área de trabalho através do Portal ou o SDK para desbloquear-se imediatamente. 

## <a name="image-building-failure"></a>Falha de criação de imagem

Imagem de criação Falha ao implementar o serviço web. Solução alternativa é adicionar "pynacl = = 1.2.1" como uma dependência de pip Conda ficheiro para a configuração de imagem.  

## <a name="fpgas"></a>FPGAs
Não será capaz de implementar os modelos no FPGAs até que tiver solicitado e foi aprovada para a quota FPGA. Para pedir acesso, preencha o formulário de pedido de quota: https://aka.ms/aml-real-time-ai

## <a name="databricks"></a>Databricks

Problemas de Databricks e o Azure Machine Learning.

1. Recomendação de cluster do Databricks:
   
   Crie o cluster do Azure Databricks como v4.x com o Python 3. Recomendamos um cluster de elevada simultaneidade.
 
2. SDK de AML instalar falha no Databricks quando mais pacotes são instalados.

   Alguns pacotes e, por exemplo, `psutil`, pode causar conflitos. Para evitar erros de instalação, instale pacotes pela versão de lib congelar. Este problema é relacionados com o Databricks e não relacionadas com o SDK do Azure ML – pode encarar os fatos com outras bibliotecas demasiado. Exemplo:
   ```python
   pstuil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
   ```
   Em alternativa, pode utilizar scripts de init se manter a enfrentar problemas de instalação com bibliotecas de Python. Essa abordagem não é uma abordagem oficialmente suportada. Pode consultar [este documento](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts).

3. Ao utilizar o Machine Learning automatizada no Databricks, se vir `Import error: numpy.core.multiarray failed to import`

   Solução: importar a biblioteca de Python `numpy==1.14.5` ao seu Databricks cluster através de criar uma biblioteca para [instalar e anexar](https://docs.databricks.com/user-guide/libraries.html#create-a-library).

## <a name="azure-portal"></a>Portal do Azure
Se vá diretamente para ver a sua área de trabalho a partir de uma ligação de partilha do SDK ou o portal, não será capaz de exibir a página de descrição geral normal com informações de subscrição na extensão. Também não será capaz de alternar para outra área de trabalho. Se precisar de ver outro da área de trabalho, a solução é ir diretamente para o [portal do Azure](https://portal.azure.com) e procure o nome de área de trabalho.

## <a name="diagnostic-logs"></a>Registos de diagnósticos
Por vezes, pode ser útil se pode fornecer informações de diagnóstico quando pedir ajuda. É aqui onde residem os ficheiros de registo:

## <a name="resource-quotas"></a>Quotas de recursos

Saiba mais sobre o [quotas de recursos](how-to-manage-quotas.md) que poderá encontrar ao trabalhar com o Azure Machine Learning.

## <a name="get-more-support"></a>Obter suporte mais

Pode submeter pedidos de suporte e obtenha ajuda de suporte técnico, fóruns e muito mais. [Saiba mais...](support-for-aml-services.md)
