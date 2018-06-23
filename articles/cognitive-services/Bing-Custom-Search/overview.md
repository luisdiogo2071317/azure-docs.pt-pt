---
title: O que é a pesquisa do Bing personalizado? | Microsoft Docs
description: Fornece uma descrição geral de alto nível da pesquisa do Bing personalizada
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/29/2017
ms.author: v-brapel
ms.openlocfilehash: 7cd61fc63d0d7734b842ed222c67c6753da9a418
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352880"
---
# <a name="what-is-bing-custom-search"></a>O que é a pesquisa do Bing personalizado?

Pesquisa do Bing personalizada permite-lhe criar experiências de pesquisa personalizáveis para tópicos que mais lhe interessam. Por exemplo, se tiver um site que fornece uma experiência de pesquisa, pode especificar os domínios, sites e páginas Web que pesquisa do Bing. Os seus utilizadores verão os resultados da pesquisa adaptados para o conteúdo que mais importantes para si em vez de ter a página percorrer os resultados de pesquisa que possuem conteúdo irrelevantes.

Para criar a vista personalizada da web, utilize a pesquisa de personalizados do Bing [portal](https://customsearch.ai). O portal permite-lhe criar uma instância de pesquisa personalizada que especifica os domínios, sites e páginas Web que pretende que o Bing para pesquisar e os sites que não pretende que procura. Para além de especificar os URLs do conteúdo que sabe mais sobre, também pode utilizar o portal para localizar o conteúdo relevante que pretende adicionar.

O portal também lhe permite afixar uma página Web específica na parte superior do resultado da pesquisa, se o utilizador introduzir um termo de pesquisa específico. 

Depois de definir a sua instância, pode integrar pesquisa personalizada para a aplicação móvel, aplicações de ambiente de trabalho ou Web site ao chamar a API de pesquisa personalizada. Se tiver um baseada na web site ou aplicação, pode permitir que a IU alojada composição da interface de pesquisa para si.

A imagem seguinte mostra a simplicidade da integração de pesquisa personalizada.

![alt imagem](./media/bcs-overview.png "funciona como pesquisa do Bing personalizada.")

## <a name="customize-search-suggestions"></a>Personalizar sugestões de pesquisa

Se subscrever personalizada pesquisa ao nível do adequado (consulte o [preços páginas](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), pode personalizar as sugestões de pesquisa efetuadas na sua experiência de pesquisa personalizada. A API de sugestão automática personalizada devolve uma lista de consultas sugeridas baseadas numa cadeia de consulta parcial que o utilizador fornece. Com personalizada de sugestão automática, que fornece sugestões de pesquisa personalizada relevantes para a sua experiência de pesquisa. Especifique se pretende devolver apenas personalizados sugestões ou incluem sugestões do Bing. Se forem incluídos sugestões Bing, sugestões personalizadas de aparecer antes das sugestões que Bing fornece. Sugestões de Bing estão limitadas para o contexto da sua instância de pesquisa personalizada.

## <a name="next-steps"></a>Passos Seguintes

Para começar a trabalhar rapidamente, consulte o artigo [criar a primeira instância de pesquisa do Bing personalizada](quick-start.md).

Para obter detalhes sobre as opções disponíveis para personalizar a sua instância de pesquisa, consulte [definir uma instância de pesquisa personalizada](define-your-custom-view.md).

Familiarize-se com o [API de pesquisa personalizada](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference) referência. A referência contém a lista de pontos finais, cabeçalhos e os parâmetros de consulta que pretende utilizar para pedir os resultados da pesquisa. Também inclui as definições dos objetos de resposta.

Para saber como personalizar sugestões, consulte [definir sugestões de pesquisa personalizada](define-custom-suggestions.md).

Não se esqueça de ler [utilizar do Bing e requisitos de apresentação](./use-and-display-requirements.md) , de modo não quebrar qualquer uma das regras sobre como utilizar os resultados da pesquisa.