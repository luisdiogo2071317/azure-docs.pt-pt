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
ms.openlocfilehash: e143c0c8ef09af49aed656d479bcad4dd35e2211
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/16/2019
ms.locfileid: "54351803"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Problemas conhecidos e resolução de problemas serviço do Azure Machine Learning
 
Este artigo ajuda-o, encontre e corrija erros ou falhas encontrados ao utilizar o serviço Azure Machine Learning. 

## <a name="sdk-installation-issues"></a>Problemas de instalação do SDK

**Mensagem de erro: Não é possível desinstalar 'PyYAML'** 

Azure Machine Learning SDK para Python: PyYAML é um projeto instalado distutils. Por conseguinte, não é possível com precisão determinar quais os ficheiros que pertencem ao mesmo em caso de uma desinstalação parcial. Para continuar a instalação do SDK ao ignorar este erro, utilize:
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
 
1. SDK de AML instalar falha no Databricks quando mais pacotes são instalados.

   Alguns pacotes e, por exemplo, `psutil`, pode causar conflitos. Para evitar erros de instalação, instale pacotes pela versão de lib congelar. Este problema é relacionados com o Databricks e não relacionadas com o SDK do Azure ML – pode encarar os fatos com outras bibliotecas demasiado. Exemplo:
   ```python
   pstuil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
   ```
   Em alternativa, pode utilizar scripts de init se manter a enfrentar problemas de instalação com bibliotecas de Python. Essa abordagem não é uma abordagem oficialmente suportada. Pode consultar [este documento](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts).

2. Quando utilizar automatizada Machine Learning no Databricks, se pretender cancelar uma execução e inicie uma nova experiência de execução, reinicie o seu cluster do Azure Databricks.

3. Nas definições de ml automatizada, quando tiver > 10 iterações defina show_output como False ao submeter a sua execução.


## <a name="azure-portal"></a>Portal do Azure
Se vá diretamente para ver a sua área de trabalho a partir de uma ligação de partilha do SDK ou o portal, não será capaz de exibir a página de descrição geral normal com informações de subscrição na extensão. Também não será capaz de alternar para outra área de trabalho. Se precisar de ver a outra área de trabalho, a solução é ir diretamente para o [portal do Azure](https://portal.azure.com) e procure o nome de área de trabalho.

## <a name="diagnostic-logs"></a>Registos de diagnósticos
Por vezes, pode ser útil se pode fornecer informações de diagnóstico quando pedir ajuda. É aqui onde residem os ficheiros de registo:

## <a name="resource-quotas"></a>Quotas de recursos

Saiba mais sobre o [quotas de recursos](how-to-manage-quotas.md) que poderá encontrar ao trabalhar com o Azure Machine Learning.

## <a name="get-more-support"></a>Obter suporte mais

Pode submeter pedidos de suporte e obtenha ajuda de suporte técnico, fóruns e muito mais. [Saiba mais...](support-for-aml-services.md)
