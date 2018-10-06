---
title: Procurar uma exibição personalizada - pesquisa personalizada do Bing
titlesuffix: Azure Cognitive Services
description: Descreve como pesquisar uma vista personalizada da web.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 235062c1b3e54843b5e64f4ef16091ae5d630894
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48814360"
---
# <a name="call-your-custom-search"></a>Chamar a sua pesquisa personalizada

Antes de efetuar a primeira chamada para a API de pesquisa personalizada para obter os resultados da pesquisa para a sua instância, terá de obter uma chave de subscrição de serviços cognitivos. Para obter uma chave de API de pesquisa personalizada, consulte [experimentar os serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).


## <a name="try-it-out"></a>Experimentar

Após configurar a sua experiência de pesquisa personalizada, pode testar a configuração a partir do portal de pesquisa personalizada. 

1. Inicie sessão no [pesquisa personalizada](https://customsearch.ai).
2. Clique numa instância de pesquisa personalizada da sua lista de instâncias.
3. Clique nas **produção** separador. 
4. Sob o **pontos de extremidade** separador, selecione um ponto de extremidade (por exemplo, API Web). A subscrição determina quais pontos de extremidade são exibidos (consulte [preços](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/) para as opções de assinatura). 
5. Especifique os valores de parâmetro. 

    Seguem-se os possíveis parâmetros que pode definir (a lista real depende do ponto de extremidade selecionado). Para obter mais informações sobre estes parâmetros, consulte [a API de pesquisa personalizada](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters) referência.

    - **Consulta**: O termo de pesquisa para procurar. Disponível apenas para pontos finais de Web, imagem, vídeo e de sugestão automática.
    - **ID de configuração personalizada**: O ID de configuração de instância de pesquisa personalizada selecionada. Este campo é só de leitura.
    - **Mercado**: O mercado de onde vêm os resultados. Disponível apenas para pontos finais de Web, imagem, vídeo e hospedado da interface do Usuário.
    - **Chave de subscrição**: A chave de subscrição para testar com. Pode selecionar uma chave na lista pendente, ou introduza um manualmente.  
      
    Clicar **parâmetros adicionais** revela os seguintes parâmetros:  
      
    - **Pesquisa segura**: um filtro utilizado para filtrar as páginas Web para o conteúdo para adultos. Disponível apenas para pontos finais de Web, imagem, vídeo e hospedado da interface do Usuário.
    - **Idioma da Interface do usuário**: O idioma usado para cadeias de caracteres de interface de utilizador. Por exemplo, se ativar imagens e vídeos na interface do Usuário do hospedado, o **imagem** e **vídeo** separadores utilizam o idioma especificado.
    - **Contagem de**: O número de resultados de pesquisa para devolver na resposta. Disponível apenas para pontos finais de Web, imagem e vídeo.
    - **Deslocamento**: O número de resultados de pesquisa a ignorar antes de retornar resultados. Disponível apenas para pontos finais de Web, imagem e vídeo.

6. Depois de especificar todas as opções necessárias, clique em **chamar** para ver a resposta JSON no painel da direita. 

Se selecionar o ponto de extremidade hospedado da interface do Usuário, pode testar a experiência de pesquisa no painel inferior.

## <a name="next-steps"></a>Passos Seguintes

- [Chamar a sua vista personalizada com c#](./call-endpoint-csharp.md)
- [Chamar a sua vista personalizada com Java](./call-endpoint-java.md)
- [Chamar a sua vista personalizada com NodeJs](./call-endpoint-nodejs.md)
- [Chamar a sua vista personalizada com Python](./call-endpoint-python.md)

- [Chamar a sua vista personalizada com o SDK do c#](./sdk-csharp-quick-start.md)