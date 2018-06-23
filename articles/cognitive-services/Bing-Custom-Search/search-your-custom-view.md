---
title: 'Pesquisa do Bing personalizada: Procurar uma vista personalizada | Microsoft Docs'
description: Descreve como pesquisar uma vista personalizada da web
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 75f6c8d299c7eed901dda0631fca74b040f72e30
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352850"
---
# <a name="call-your-custom-search"></a>Chamar a pesquisa personalizada
Antes de efetuar a primeira chamada para a API de pesquisa personalizada para obter os resultados da pesquisa para a sua instância, terá de obter uma chave de subscrição de serviços cognitivos. Para obter uma chave de API de pesquisa personalizada, consulte [tente serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).

> [!NOTE]
> Os clientes de pesquisa do Bing personalizado existentes que tem uma chave de pré-visualização aprovisionada no ou antes do dia 15 de Outubro de 2017 poderá utilizar as respetivas chaves até 30 de Novembro de 2017, ou até que possam tem esgotado o número máximo de consultas permitido. Seguidamente, precisam de migrar para a versão geralmente disponível no Azure.

## <a name="try-it-out"></a>Experimentar
Depois de configurar a experiência de pesquisa personalizada, pode testar a configuração a partir do portal de pesquisa personalizada. Inicie sessão no [pesquisa personalizada](https://customsearch.ai), clique uma instância de pesquisa personalizada e, no **produção** separador. O **pontos finais** separador é apresentado. A subscrição irá determinar quais os pontos finais estão disponíveis para experimentar, consulte [preços páginas](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/). Para testar um ponto final, selecione-o na lista pendente e definir as opções de configuração associados. 

Seguem-se as opções disponíveis.

- **Consulta**: O termo de pesquisa para procurar. Disponível apenas para pontos finais Web, a imagem e Autosuggest.
- **ID de configuração personalizada**: O ID de configuração da instância de pesquisa personalizada selecionada. Este campo é só de leitura.
- **Mercado**: mercado onde os resultados provenientes. Disponível apenas para pontos finais Web, a imagem e IU alojado.
- **Chave de subscrição**: A chave de subscrição para testar com. Pode selecionar uma chave na lista pendente ou introduza um manualmente.
- **Pesquisa segura**: um filtro utilizado para filtrar as páginas Web de conteúdo para adultos. Disponível apenas para pontos finais Web, a imagem e IU alojado.
- **Contagem**: O número de resultados de pesquisa devolver na resposta. Disponível apenas para pontos finais Web e de imagem.
- **O desvio**: O número de resultados de pesquisa devolver na resposta. Disponível apenas para pontos finais Web e de imagem.

Depois de especificadas todas as opções necessárias para a Web, Image ou Autosuggest, clique em **chamar** para ver a resposta JSON no painel direito. 

Se selecionar o ponto final alojado IU, pode testar a experiência de pesquisa do painel da direita.

## <a name="next-steps"></a>Passos Seguintes
- [Chamar a vista personalizada com c#](./call-endpoint-csharp.md)
- [Chamar a vista personalizada com o Java](./call-endpoint-java.md)
- [Chamar a vista personalizada com NodeJs](./call-endpoint-nodejs.md)
- [Chamar a vista personalizada com o Python](./call-endpoint-python.md)