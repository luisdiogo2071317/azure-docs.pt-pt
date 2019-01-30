---
title: O que é a Visão Personalizada do Azure?
titlesuffix: Azure Cognitive Services
description: Aprenda a utilizar o serviço de Visão Personalizada para criar classificadores de imagem personalizada na cloud do Azure.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: overview
ms.date: 01/10/2019
ms.author: anroth
ms.openlocfilehash: 887c2fa09a1108b6254f4dd13b1ca211c80f8f70
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55219265"
---
# <a name="what-is-azure-custom-vision"></a>O que é a Visão Personalizada do Azure?

A API de Visão Personalizada do Azure é um serviço cognitivo que lhe permite criar, implementar e melhorar classificadores de imagem personalizada. Um classificador de imagem é um serviço de IA que classifica as imagens em classes (etiquetas), de acordo com determinadas características. Ao contrário do serviço de [Imagem Digitalizada](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home), a Visão Personalizada permite-lhe criar as suas próprias classificações.

## <a name="what-it-does"></a>O que faz

O serviço de Visão Personalizada utiliza um algoritmo de aprendizagem automática para classificar as imagens. O programador, tem de submeter grupos de imagens que têm e não têm classificação(ões) em questão. Especifique as etiquetas corretas das imagens no momento da submissão. Em seguida, o algoritmo prepara estes dados e calcula a sua própria precisão ao testar-se a si próprio nesses mesmos dados. Assim que o modelo é preparado, pode testar, voltar a preparar e, eventualmente, utilizá-lo para classificar as novas imagens de acordo com as necessidades da sua aplicação. Também pode exportar o próprio modelo para utilização offline.

### <a name="classification-and-object-detection"></a>Classificação e deteção de objetos

A funcionalidade de Visão Personalizada pode ser dividida em duas funcionalidades. A **Classificação de imagens** atribui uma distribuição de classificações a cada imagem. As várias classes (uma marca por imagem) e classificação de multilabel (qualquer número de etiquetas por imagem) modelos são suportados. **Deteção de objeto** é semelhante à classificação multilabel, mas também devolve as coordenadas na imagem em que as etiquetas aplicadas podem ser encontradas.

### <a name="optimization"></a>Otimização

Em geral, os métodos que o serviço de Visão Personalizada utiliza são robustos às diferenças, o que lhe permite iniciar a criação de protótipos com poucos dados. 50 imagens por etiqueta são, geralmente, um bom começo. Isto significa que, no entanto, o serviço não é ideal para a deteção de diferenças subtis nas imagens (por exemplo, detetar falhas pequenas ou dents em cenários de garantia de qualidade).

Além disso, pode escolher entre diversas variedades do algoritmo de Visão Personalizada que estão otimizadas para determinado material de assunto&mdash;por exemplo, pontos de referência ou itens de retalho. Veja o guia [Criar um classificador](getting-started-build-a-classifier.md) para obter mais informações sobre os mesmos.

## <a name="what-it-includes"></a>O que inclui
O Serviço de Visão Personalizada está disponível como um conjunto de SDKs nativos, bem como através de uma interface baseada na Web sobre a [home page de Visão Personalizada](https://customvision.ai/). Pode criar, testar e formar um modelo através da interface, ou ambos.

![Home page da Visão Personalizada numa janela do browser Chrome](media/browser-home.png)

## <a name="data-privacy-and-security"></a>Segurança e privacidade dos dados

Como com todos os serviços cognitivos, os desenvolvedores que usam o serviço de visão personalizada devem estar ciente das políticas da Microsoft em dados do cliente. Consulte a [página dos serviços cognitivos](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) no Microsoft Trust Center para obter mais informações.

## <a name="next-steps"></a>Passos Seguintes

Siga o guia [Criar um classificador](getting-started-build-a-classifier.md) para começar a utilizar a Visão Personalizada na Web ou para concluir um [Tutorial de classificação de imagens](csharp-tutorial.md) para implementar o cenário em código.
