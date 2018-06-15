---
title: O que é o serviço de decisão personalizado? -Serviços cognitivos as do azure | Microsoft Docs
description: Este artigo descrições gerais de serviço de decisão de personalizada do Azure, uma API baseado na nuvem para a nível contextual das decision-making que sharpens com experiência.
services: cognitive-services
author: alekh
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/08/2018
ms.author: slivkins;marcozo;alekh;marossi
ms.openlocfilehash: 774467446513dcd7ade7255d998b11f41824cafe
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "35356296"
---
# <a name="what-is-custom-decision-service"></a>O que é o serviço de decisão personalizado?

Num típico web ou aplicação móvel, uma ligações de página para vários artigos ou outros tipos de conteúdo. Como a página de front-carrega, conseguiria pedir ao serviço de decisão personalizada para classificação artigos incluídos nessa página front. Por isso, quando um utilizador escolhe um artigo ao clicar nela, foi enviado um pedido de segundo para o serviço de decisão personalizada, que irá iniciar o resultado da decisão utilizador.

Serviço de decisão personalizado é fácil de utilizar, porque requer apenas um feed RSS para o conteúdo e algumas linhas de JavaScript para ser adicionado a sua aplicação.

Serviço de decisão personalizado converte o conteúdo funcionalidades do machine learning. O sistema utiliza estas funcionalidades para compreender o conteúdo em termos do respetivo texto, imagens, vídeos e sentimento geral. Utiliza vários outros [serviços cognitivos Microsoft](https://www.microsoft.com/cognitive-services), como [entidade de ligação](../entitylinking/home.md), [análise de texto](../text-analytics/overview.md), [emoções](../emotion/home.md)e [Computador visão](../computer-vision/home.md).

Alguns casos de utilização comuns para o serviço de decisão personalizada incluem:

* Personalizar artigos num Web site notícias de última hora
* Personalizar as vídeo conteúdos de um portal de suporte de dados
* Otimizar possam ser posicionadas livremente do ad ou páginas web que direciona o ad para
* Classificação recomendada itens num Web site compras.

Serviço de decisão personalizada está atualmente no *pré-visualização pública livre*. Pode personalizar a uma lista dos artigos num Web site ou uma aplicação. A extração da funcionalidade funciona melhor para conteúdo de idioma inglês. [Limitada a funcionalidade](../text-analytics/overview.md) é oferecido para outros idiomas, espanhol, francês, alemão, português e japonês. Esta documentação irá ser revista medida que novas funcionalidades ficarem disponível.

Personalizada de decisão de serviço pode ser utilizada em aplicações que não estão no domínio de personalização de conteúdo. Estas aplicações podem ser uma boa opção para uma versão de pré-visualização personalizado. [Contacte-nos](https://azure.microsoft.com/overview/sales-number/) para obter mais informações.

## <a name="api-usage-modes"></a>Modos de utilização de API

Decisão de serviços personalizados podem ser aplicados para páginas Web e aplicações móveis. As APIs pode ser chamadas a partir de um browser ou uma aplicação. A utilização da API é semelhante em ambos, mas alguns dos detalhes são diferentes.

## <a name="glossary-of-terms"></a>Glossário de termos

Alguns termos com frequência ocorrerem nesta documentação:

* **Conjunto de ação**: O conjunto de itens de conteúdo para o serviço de decisão de personalizada a classificação. Este conjunto pode ser especificado como um *RSS* ou *Atom* ponto final.
* **Classificação**: cada pedido de serviço de decisão personalizada Especifica um ou vários conjuntos de ação. O sistema responde por diretriz todas as opções de conteúdo destes conjuntos e devolve-os por ordem classificado.
* **Função de chamada de retorno**: esta função, o que pode especificar, compõe o conteúdo na sua IU. O conteúdo está ordenado pela ordenação classificação devolvidos pelo serviço de decisão personalizada.
* **Reward**: uma medida de como o utilizador respondeu ao conteúdo composto. Serviço de decisão personalizado mede resposta do utilizador através da utilização de cliques. Os cliques são comunicadas ao sistema utilizando código personalizado inserido na sua aplicação.

## <a name="next-steps"></a>Passos Seguintes

* [Registar a sua aplicação](custom-decision-service-get-started-register.md) com o serviço de decisão personalizada
* Começar a utilizar para otimizar [uma página Web](custom-decision-service-get-started-browser.md) ou [uma aplicação de smartphone](custom-decision-service-get-started-app.md).
* Consulte o [referência da API](custom-decision-service-api-reference.md) para saber mais sobre a funcionalidade fornecida.