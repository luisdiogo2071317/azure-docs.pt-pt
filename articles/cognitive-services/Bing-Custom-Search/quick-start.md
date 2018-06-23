---
title: Criar uma instância de pesquisa do Bing personalizada primeiro - Microsoft de serviços cognitivos
description: Para utilizar a procura personalizados do Bing, terá de criar uma instância de pesquisa personalizada que define a vista ou o setor de web. A instância contém as definições que especificam o pública domínios, subsites e páginas Web que pretende que o Bing para pesquisar e ajustamentos de classificação.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 05/07/2017
ms.author: v-brapel
ms.openlocfilehash: 35f0bca01de1c2087f6ae30949cca9b03192b838
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352885"
---
# <a name="create-your-first-bing-custom-search-instance"></a>Criar a primeira instância de pesquisa do Bing personalizada
Para utilizar a procura personalizados do Bing, terá de criar uma instância de pesquisa personalizada que define a vista ou o setor de web. A instância contém as definições que especificam o pública domínios, sites e páginas Web que pretende que o Bing para pesquisar e ajustamentos de classificação. Para criar a instância, utilize a pesquisa do Bing personalizada [portal](https://customsearch.ai). 

## <a name="create-a-custom-search-instance"></a>Criar uma instância de pesquisa personalizada

Para criar uma instância de pesquisa do Bing personalizada:

1.  Obter uma chave personalizada da API de pesquisa. Consulte [experimentar serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).
2.  Inicie sessão no portal utilizando uma conta Microsoft (MSA). Clique em de **sessão** botão. Se for a primeira vez utilizando a portal siga os passos adicionais abaixo, caso contrário, avance para o passo 3.
    - Se não tiver uma MSA, clique em **criar uma conta Microsoft**. O portal pede-lhe permissões para aceder aos seus dados. Clique em **Sim**.
    - Aceite os termos de serviços cognitivos. Verifique **concordo** e clique em **Concordo**.  
3.  Depois de iniciarem sessão, clique em **nova instância** e atribua o nome de instância. Utilize um nome que faça sentido e descreve o tipo de conteúdo que devolve a pesquisa. Pode alterar o nome em qualquer altura. 
4.  No **Active Directory** separador **experiência de pesquisa**, introduza o URL de um ou mais sites que pretende incluir na sua pesquisa.
5.  Para confirmar que a sua instância devolve resultados, introduza uma consulta no painel de pré-visualização no lado direito. Se não existirem resultados, especifique um novo site. Bing devolve resultados apenas para os sites públicas que tem indexada.
6.  Clique em **publicar** para publicar as alterações de configuração para produção. Quando lhe for pedido, clique em **publicar** para confirmar.
7.  Clique em **produção** > **pontos finais** e copie o **ID de configuração personalizada**. Este ID para chamar a API de pesquisa personalizada é necessário.

## <a name="next-steps"></a>Passos Seguintes

Continue a trabalhar com a instância de pesquisa personalizada que acabou de criar ao seguir as instruções estes guias de procedimentos:

- [Configurar a sua experiência de pesquisa personalizada](./define-your-custom-view.md)
- [Chamar a pesquisa personalizada](./search-your-custom-view.md)
- [Partilhar a sua pesquisa personalizada](./share-your-custom-search.md)
- [Configurar a sua experiência de IU alojada](./hosted-ui.md)
- [Utilizar marcadores decoration para realçar texto](./hit-highlighting.md)
- [Páginas de página Web](./page-webpages.md)
