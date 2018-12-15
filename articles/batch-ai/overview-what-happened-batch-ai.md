---
title: O que está a acontecer ao Azure Batch AI? | Microsoft Docs
description: Saiba mais sobre o que está a acontecer ao Azure Batch AI e a opção de computação de serviço do Azure Machine Learning.
services: batch-ai
author: garyericson
ms.service: batch-ai
ms.topic: overview
ms.date: 12/14/2018
ms.author: garye
ms.openlocfilehash: 6803a47ae77c072eff05df65e37156c90aabf3e6
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/15/2018
ms.locfileid: "53436880"
---
# <a name="whats-happening-to-azure-batch-ai"></a>O que está a acontecer ao Azure Batch AI?

**O serviço Azure Batch AI está desativando em Março.** A preparação de à escala e classificação de recursos do Batch AI estão agora disponíveis na [serviço Azure Machine Learning](../machine-learning/service/overview-what-is-azure-ml.md), que foi disponibilizado no dia 4 de Dezembro de 2018.

Juntamente com muitas outras capacidades de machine learning, o serviço Azure Machine Learning inclui um destino de computação gerida baseado na nuvem para treinamento, implementação e modelos de aprendizagem automática de classificação. Este destino de computação é chamado [computação do Azure Machine Learning](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute). [Começar a migrar e usá-lo hoje](#migrate). Pode interagir com o serviço Azure Machine Learning através da respetiva [SDKs de Python](../machine-learning/service/quickstart-create-workspace-with-python.md), interface de linha de comandos e o [portal do Azure](../machine-learning/service/quickstart-get-started.md).

## <a name="support-timeline"></a>Linha cronológica de suporte

| Date | Detalhes de suporte do serviço de IA do batch |
| ---- |-----------------|
| Dezembro&nbsp;14&#x2c;&nbsp;2018| Continue a utilizar as suas subscrições do Azure Batch AI existentes, como antes. No entanto, a registar **novas subscrições** é não possível mais tempo e sem novos investimentos serão efetuados para este serviço.|
| Março&nbsp;31&#x2c;&nbsp;2019 | Após esta data, as assinaturas existentes do Batch AI deixará de funcionar. |

<a name="migrate"></a>
## <a name="how-do-i-migrate"></a>Como posso migrar?

Para evitar interrupções às suas aplicações e beneficiar das funcionalidades mais recentes, siga os passos seguintes antes de 31 de Março de 2019:

1. Criar uma área de trabalho do serviço do Azure Machine Learning e começar a utilizar:
    + [Guia de introdução do Python com base em](../machine-learning/service/quickstart-create-workspace-with-python.md)
    + [Portal com base início rápido do Azure](../machine-learning/service/quickstart-get-started.md)

1. Configurar uma [computação do Azure Machine Learning](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute) para preparação de modelos.

1. Atualize os scripts para usar a computação do Azure Machine Learning.

## <a name="support"></a>Suporte

O suporte está disponível para os clientes existentes que desejam migrar para o mais abrangente [serviço Azure Machine Learning](https://aka.ms/aml-docs).

Se o serviço Azure Machine Learning não atende às suas necessidades em que uma funcionalidade suportada existe no serviço de Batch AI, abra um suporte de Batch AI solicitar com a equipa de suporte à lista de permissões sua subscrição para utilizar o Batch AI até extinção do serviço.

## <a name="next-steps"></a>Passos Seguintes

+ Leitura a [descrição geral do serviço Azure Machine Learning](../machine-learning/service/overview-what-is-azure-ml.md).

+ [Configurar um destino de computação para a preparação de modelos](../machine-learning/service/how-to-set-up-training-targets.md) com o serviço Azure Machine Learning.

+ Reveja os [mapa do Azure](https://azure.microsoft.com/updates/) para saber de outras atualizações de serviço do Azure.
