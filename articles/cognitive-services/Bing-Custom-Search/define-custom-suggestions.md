---
title: 'Pesquisa do Bing personalizada: Definir sugestões de sugestão automática do personalizado | Microsoft Docs'
description: Descreve como configurar a sugestão automática do personalizado com sugestões personalizados
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.technology: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: a41b4e5b6c268ec68488c6764d4192cf8d2345a4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352849"
---
# <a name="configure-your-custom-autosuggest-experience"></a>Configurar a sua experiência de autosuggest personalizado
Se subscrever personalizada pesquisa ao nível do adequado (consulte o [preços páginas](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), pode personalizar as sugestões de pesquisa efetuadas na sua experiência de pesquisa personalizada. Autosuggest personalizado devolve uma lista de consultas sugeridas baseadas numa cadeia de consulta parcial que o utilizador fornece. Com personalizada de sugestão automática, que fornece sugestões de pesquisa personalizada relevantes para a sua experiência de pesquisa. Especifique se pretende devolver apenas personalizados sugestões ou também incluem sugestões do Bing. Se incluir sugestões Bing, sugestões personalizadas de aparecer antes das sugestões do Bing. Sugestões de Bing estão limitadas para o contexto da sua instância de pesquisa personalizada.

## <a name="configure-custom-autosuggest"></a>Configurar personalizada de sugestão automática
Utilize as seguintes instruções para configurar a sugestão automática do personalizada para a instância de pesquisa personalizada.

1.  Inicie sessão no [pesquisa personalizada](https://customsearch.ai).
2.  Clique uma instância de pesquisa personalizada. Para criar uma instância, consulte [criar a primeira instância de pesquisa do Bing personalizada](quick-start.md).
3.  Clique em de **Autosuggest** separador.

## <a name="enable-bing-suggestions"></a>Ativar as sugestões do Bing
Para ativar as sugestões de Bing, ative o **sugestões Bing automática** controlo de deslize para a posição on. O controlo de deslize fica azul.

## <a name="add-suggestions"></a>Adicionar sugestões
Para adicionar uma sugestão, introduza-o na caixa de texto. Prima a tecla enter, ou clique em de **+** ícone. Sugestões personalizados podem estar em qualquer idioma e irão aparecer antes de sugestões do Bing.

## <a name="upload-suggestions"></a>Carregar sugestões
Pode carregar uma lista de sugestões de um ficheiro. Coloque cada sugestão numa linha separada. Clique no ícone de carregamento e selecione o ficheiro.

## <a name="remove-suggestions"></a>Remova as sugestões
Para remover uma sugestão, clique no ícone de remover junto a sugestão de que pretende remover.

[!INCLUDE[publish or revert](./includes/publish-revert.md)]

  >[!NOTE]  
  >Pode demorar até 24 horas para personalizada de sugestão automática as alterações à configuração entrem em vigor.

## <a name="next-steps"></a>Passos Seguintes

- [Obter sugestões personalizados](./get-custom-suggestions.md)
- [Procure a instância personalizada](./search-your-custom-view.md)
- [Configurar e consumir IU alojada personalizada](./hosted-ui.md)