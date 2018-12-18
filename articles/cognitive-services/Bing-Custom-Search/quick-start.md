---
title: 'Início rápido: Criar uma primeira instância de pesquisa personalizada do Bing | Documentos da Microsoft'
titlesuffix: Azure Cognitive Services
description: Utilize este artigo para criar um personalizada Bing instância que pode domínios de pesquisa e páginas da Web que definir.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: quickstart
ms.date: 05/07/2017
ms.author: aahi
ms.openlocfilehash: ba1444b3115ffd2c73e1773aa926d11fed81f830
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/18/2018
ms.locfileid: "53556123"
---
# <a name="quickstart-create-your-first-bing-custom-search-instance"></a>Início rápido: Criar a primeira instância de pesquisa personalizada do Bing

Para utilizar a Pesquisa Personalizada do Bing, tem de criar uma instância de pesquisa personalizada que define a vista ou o setor da Web. Esta instância contém os domínios público, Web sites e páginas da Web que pretende pesquisar, juntamente com os ajustes de classificação, que pode desejar. 

Para criar a instância, utilize o [portal de pesquisa personalizada do Bing](https://customsearch.ai). 

![Uma imagem do portal de pesquisa personalizada do Bing](media/blockedCustomSrch.png)

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]

## <a name="create-a-custom-search-instance"></a>Criar uma instância de pesquisa personalizada

Para criar uma instância de Pesquisa Personalizada do Bing:

1. Clique em **começar a utilizar** sobre o [portal de pesquisa personalizada do Bing](https://customsearch.ai) página da Web e inicie sessão com a sua conta Microsoft.

2. Clique em **nova instância**e introduza um nome descritivo. Pode alterar o nome da sua instância em qualquer altura.
 
3. No separador **Ativo** em **Experiência de Pesquisa**, introduza o URL de um ou mais sites que pretende incluir na sua pesquisa. 

    > [!NOTE]
    > Instâncias de pesquisa personalizada do Bing só irão devolver resultados de domínios e páginas Web que sejam públicos e foram indexada pela Bing.

4. Pode usar o lado direito do portal de pesquisa personalizada do Bing para inserir uma consulta e examinar os resultados da pesquisa devolvidos pela sua instância de pesquisa. Se não são devolvidos resultados, tente introduzir um URL diferente.  

5. Clique em **publicar** para publicar as alterações ao ambiente de produção e atualizar pontos de extremidade da instância.

6.  Clique no **produção** separador. sob **pontos de extremidade**, cópia seu **ID de configuração personalizada**. Vai precisar deste ID para chamar a API de pesquisa personalizada, acrescentando-o para o `customconfig=` parâmetro em suas chamadas de consulta.


## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Início rápido: Chamar o ponto final de pesquisa personalizada do Bing](./call-endpoint-csharp.md)