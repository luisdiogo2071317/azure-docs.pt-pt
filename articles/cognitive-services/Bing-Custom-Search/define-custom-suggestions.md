---
title: 'A pesquisa personalizada do Bing: Definir sugestões de sugestão automática personalizada | Documentos da Microsoft'
description: Descreve como configurar o acesso de sugestão automática personalizada com sugestões personalizadas
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.technology: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: e7a62a79bdc2e486fb6bfca34eb4addeba2bde0e
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44158318"
---
# <a name="configure-your-custom-autosuggest-experience"></a>Configurar a sua experiência de sugestão automática personalizada
Se subscrever a pesquisa personalizada ao nível apropriado (consulte a [páginas de preços](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), pode personalizar as sugestões de pesquisa feitas na sua experiência de pesquisa personalizada. Sugestão automática personalizada devolve uma lista de consultas sugeridas com base numa cadeia de caracteres de consulta parcial que o utilizador fornece. Com a sugestão automática personalizada, fornecer sugestões de pesquisa personalizada relevantes para a sua experiência de pesquisa. Especifique se pretende devolver sugestões apenas personalizadas ou também incluem sugestões do Bing. Se incluir sugestões do Bing, sugestões personalizadas aparecem antes das sugestões do Bing. Sugestões de Bing estão limitadas ao contexto da sua instância de pesquisa personalizada.

## <a name="configure-custom-autosuggest"></a>Configurar personalizado de sugestão automática
Utilize as seguintes instruções para configurar a sugestão automática personalizada para a sua instância de pesquisa personalizada.

1.  Inicie sessão no [pesquisa personalizada](https://customsearch.ai).
2.  Clique numa instância de pesquisa personalizada. Para criar uma instância, veja [criar a primeira instância de pesquisa personalizada do Bing](quick-start.md).
3.  Clique nas **sugestão automática** separador.

## <a name="enable-bing-suggestions"></a>Ativar as sugestões do Bing
Para ativar as sugestões de Bing, alternar entre o **sugestões automática Bing** controlo de deslize para a posição no. O controlo de deslize torna-se azul.

## <a name="add-suggestions"></a>Adicionar sugestões
Para adicionar uma sugestão, introduza-o na caixa de texto. Prima a tecla enter ou clique a **+** ícone. Sugestões personalizadas podem ser em qualquer linguagem e irão aparecer antes de sugestões do Bing.

## <a name="upload-suggestions"></a>Carregar sugestões
Pode carregar uma lista de sugestões de um ficheiro. Coloque cada sugestão numa linha separada. Clique no ícone de carregamento e selecione o seu ficheiro.

## <a name="remove-suggestions"></a>Remova as sugestões
Para remover uma sugestão, clique no ícone de remover junto a sugestão de que pretende remover.

[!INCLUDE [publish or revert](./includes/publish-revert.md)]

  >[!NOTE]  
  >Pode demorar até 24 horas para alterações de configuração de sugestão automática personalizada entrar em vigor.

## <a name="next-steps"></a>Passos Seguintes

- [Obtenha sugestões personalizadas](./get-custom-suggestions.md)
- [Pesquisar a instância personalizada](./search-your-custom-view.md)
- [Configurar e consumir da interface do Usuário hospedado personalizado](./hosted-ui.md)