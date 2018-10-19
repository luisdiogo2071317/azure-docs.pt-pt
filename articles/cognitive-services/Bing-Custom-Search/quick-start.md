---
title: 'Início Rápido: criar uma primeira instância de Pesquisa Personalizada do Bing'
titlesuffix: Azure Cognitive Services
description: Para utilizar a Pesquisa Personalizada do Bing, tem de criar uma instância de pesquisa personalizada que define a vista ou o setor da Web. A instância contém definições que especificam os domínios, sub-sites e páginas Web públicos que pretende que o Bing pesquise, e quaisquer ajustes de classificação.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: quickstart
ms.date: 05/07/2017
ms.author: v-brapel
ms.openlocfilehash: 866d32aa4de45076fcbc4e413d8c2e67d5346878
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816244"
---
# <a name="quickstart-create-your-first-bing-custom-search-instance"></a>Início Rápido: criar a sua primeira instância de Pesquisa Personalizada do Bing
Para utilizar a Pesquisa Personalizada do Bing, tem de criar uma instância de pesquisa personalizada que define a vista ou o setor da Web. A instância contém definições que especificam os domínios, sites e páginas Web públicos que pretende que o Bing pesquise, e quaisquer ajustes de classificação. Para criar a instância, utilize o portal de [Pesquisa Personalizada do Bing](https://customsearch.ai). 

## <a name="create-a-custom-search-instance"></a>Criar uma instância de pesquisa personalizada

Para criar uma instância de Pesquisa Personalizada do Bing:

1.  Obtenha uma chave para a API de Pesquisa Personalizada. Ver [Experimente os Serviços Cognitivos](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).
2.  Clique no botão **Iniciar Sessão** e inicie sessão no portal com uma conta Microsoft (MSA). 
    - Se não tiver uma MSA, clique em **Criar uma conta Microsoft**. O portal pede-lhe permissões para aceder aos seus dados. Clique em **Sim**.
    - Aceite os Termos dos Serviços Cognitivos. Marque **Concordo** e clique em **Aceito**.  
3.  Depois de iniciar sessão, clique em **Nova instância** e dê um nome à instância. Utilize um nome que seja significativo e descreva o tipo de conteúdo que a pesquisa irá devolver. Pode alterar o nome em qualquer altura. 
4.  No separador **Ativo** em **Experiência de Pesquisa**, introduza o URL de um ou mais sites que pretende incluir na sua pesquisa.
5.  Para confirmar que a sua instância devolve resultados, introduza uma consulta no painel de pré-visualização no lado direito. Se não forem apresentados resultados, especifique um novo site. O Bing só devolve resultados para sites públicos que lhe estejam indexados.
6.  Clique em **Publicar** para publicar as alterações de configuração para produção. Quando lhe for pedido, clique em **Publicar** para confirmar.
7.  Clique em **Produção** > **Pontos Finais** e copie o **ID de Configuração Personalizada**. Vai precisar deste ID para chamar a API de Pesquisa Personalizada.

## <a name="next-steps"></a>Passos seguintes

Continue a trabalhar com a instância de pesquisa personalizada que acabou de criar ao seguir as instruções nestes manuais de instruções:

- [Configurar a sua experiência de pesquisa personalizada](./define-your-custom-view.md)
- [Chamar a sua pesquisa personalizada](./search-your-custom-view.md)
- [Partilhar a sua pesquisa personalizada](./share-your-custom-search.md)
- [Configurar a sua experiência de IU alojada](./hosted-ui.md)
- [Utilizar marcadores decorativos para realçar o texto](./hit-highlighting.md)
- [Páginas Web da página](./page-webpages.md)
