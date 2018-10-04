---
title: Problemas e resolução de problemas de conhecidos para o serviço Azure Machine Learning
description: Obter uma lista dos problemas conhecidos, soluções alternativas e resolução de problemas
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.component: core
ms.topic: article
ms.date: 10/01/2018
ms.openlocfilehash: 02cee5a3e088c919ec94aee6f46ef6f428b9bb48
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/03/2018
ms.locfileid: "48249422"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Problemas conhecidos e resolução de problemas serviço do Azure Machine Learning
 
Este artigo ajuda-o, encontre e corrija erros ou falhas encontrados ao utilizar o serviço Azure Machine Learning. 

## <a name="sdk-installation-issues"></a>Problemas de instalação do SDK

**Mensagem de erro: não é possível desinstalar 'PyYAML'** 

PyYAML é um projeto instalado distutils. Por conseguinte, não é possível com precisão determinar quais os ficheiros que pertencem ao mesmo em caso de uma desinstalação parcial. Para continuar a instalação do SDK ao ignorar este erro, utilize:
```Python 
pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
```

## <a name="image-building-failure"></a>Falha de criação de imagem

Imagem de criação Falha ao implementar o serviço web. Solução alternativa é adicionar "pynacl = = 1.2.1" como uma dependência de pip Conda ficheiro para a configuração de imagem.  

## <a name="pipelines"></a>Pipelines
Ocorre um erro ao chamar PythonScriptStep várias vezes numa linha sem alterar o script ou parâmetros. Solução alternativa é recriar o objeto de PipelineData.

## <a name="fpgas"></a>FPGAs
Não será capaz de implementar os modelos no FPGAs até que tiver solicitado e foi aprovada para a quota FPGA. Para pedir acesso, preencha o formulário de pedido de quota: https://aka.ms/aml-real-time-ai

## <a name="databricks"></a>Databricks

Problemas de Databricks e o Azure Machine Learning.

1. Recomendação de cluster do Databricks:
   
   Crie o cluster do Azure Databricks como v4.x com o Python 3. Recomendamos um cluster de elevada simultaneidade.
 
1. SDK de AML instalar falha no Databricks quando mais pacotes são instalados.

   Alguns pacotes e, por exemplo, `psutil upgrade libs`, pode causar conflitos. Para evitar erros de instalação, instale pacotes pela versão de lib congelar. Este problema é relacionados com o Databricks e não relacionadas com o SDK de AML. Exemplo:
   ```python
   pstuil cryptography==1.5 pyopenssl==16.0.0 ipython=2.2.0
   ```

## <a name="diagnostic-logs"></a>Registos de diagnósticos
Por vezes, pode ser útil se pode fornecer informações de diagnóstico quando pedir ajuda. É aqui onde residem os ficheiros de registo:

## <a name="resource-quotas"></a>Quotas de recursos

Saiba mais sobre o [quotas de recursos](how-to-manage-quotas.md) que poderá encontrar ao trabalhar com o Azure Machine Learning.

## <a name="get-more-support"></a>Obter suporte mais

Pode submeter pedidos de suporte e obtenha ajuda de suporte técnico, fóruns e muito mais. [Saiba mais...](support-for-aml-services.md)
