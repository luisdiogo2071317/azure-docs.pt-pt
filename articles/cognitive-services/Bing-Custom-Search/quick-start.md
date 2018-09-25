---
title: Criar uma primeira instância de pesquisa personalizada do Bing - serviços cognitivos da Microsoft
description: Para utilizar a pesquisa personalizada do Bing, terá de criar uma instância de pesquisa personalizada que define o modo de exibição ou setor da web. A instância contém definições que especificar os domínios público, subsites e páginas da Web que pretende que o Bing para pesquisar e os ajustes de classificação.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 05/07/2017
ms.author: v-brapel
ms.openlocfilehash: 25d622772fe47ffad001834d476e612f8c606904
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46981663"
---
# <a name="create-your-first-bing-custom-search-instance"></a>Criar a primeira instância de pesquisa personalizada do Bing
Para utilizar a pesquisa personalizada do Bing, terá de criar uma instância de pesquisa personalizada que define o modo de exibição ou setor da web. A instância contém definições que especificar os domínios público, Web sites e páginas da Web que pretende que o Bing para pesquisar e os ajustes de classificação. Para criar a instância, utilize a pesquisa personalizada do Bing [portal](https://customsearch.ai). 

## <a name="create-a-custom-search-instance"></a>Criar uma instância de pesquisa personalizada

Para criar uma instância de pesquisa personalizada do Bing:

1.  Obtenha uma chave para a API de pesquisa personalizada. Ver [experimente os serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).
2.  Clique nas **iniciar sessão** botão e inicie sessão no portal com uma conta Microsoft (MSA). 
    - Se não tiver uma MSA, clique em **criar uma conta Microsoft**. O portal pede-lhe permissões para aceder aos seus dados. Clique em **Sim**.
    - Aceite os termos de serviços cognitivos. Verifique **concordo** e clique em **Concordo**.  
3.  Depois de iniciar sessão, clique em **nova instância** e dê o nome da instância. Utilize um nome que é significativo e descreve o tipo de conteúdo que a pesquisa retorna. Pode alterar o nome em qualquer altura. 
4.  Sobre o **Active Directory** separador sob **experiência de pesquisa**, introduza o URL de um ou mais sites que pretende incluir na sua pesquisa.
5.  Para confirmar que a sua instância retorna resultados, introduza uma consulta no painel de pré-visualização no lado direito. Se não existirem resultados, especifique um novo Web site. O Bing devolve resultados apenas para sites públicos, que tem indexados.
6.  Clique em **publicar** para publicar as alterações de configuração para produção. Quando lhe for pedido, clique em **publicar** para confirmar.
7.  Clique em **produção** > **pontos finais** e copie o **ID de configuração personalizada**. Vai precisar deste ID para chamar a API de pesquisa personalizada.

## <a name="next-steps"></a>Passos Seguintes

Continue a trabalhar com a instância de pesquisa personalizada que acabou de criar ao seguir as instruções nestes guias de procedimentos:

- [Configurar a sua experiência de pesquisa personalizada](./define-your-custom-view.md)
- [Chamar a sua pesquisa personalizada](./search-your-custom-view.md)
- [Partilhar a sua pesquisa personalizada](./share-your-custom-search.md)
- [Configurar a sua experiência de interface do Usuário alojada](./hosted-ui.md)
- [Utilizar marcadores decoration para realçar o texto](./hit-highlighting.md)
- [Páginas Web de página](./page-webpages.md)
