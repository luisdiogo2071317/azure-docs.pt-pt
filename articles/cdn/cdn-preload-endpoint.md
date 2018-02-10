---
title: "Pré-carregar recursos num ponto final do Azure CDN | Microsoft Docs"
description: "Saiba como carregar previamente conteúdo em cache num ponto final de CDN do Azure."
services: cdn
documentationcenter: 
author: dksimpson
manager: erikre
editor: 
ms.assetid: 5ea3eba5-1335-413e-9af3-3918ce608a83
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: mazha
ms.openlocfilehash: acd6eae12ff338c64cc8879aa8c27b226e3d2f84
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Pré-carregar recursos num ponto final da CDN do Azure
[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Por predefinição, ativos são colocadas em cache apenas quando forem necessários. Como resultado, o primeiro pedido de cada região pode demorar mais do que os pedidos subsequentes. O motivo é porque os servidores edge tem não ainda em cache o conteúdo e precisam reencaminhar o pedido para o servidor de origem. Através da pré-carregamento de conteúdo, pode evitar esta latência de acessos primeiro.

Além de proporcionarem uma melhor experiência de cliente, pré-carregar os recursos em cache também pode reduzir o tráfego de rede no servidor de origem.

> [!NOTE]
> Pré-carregar recursos é útil para grandes eventos ou conteúdo que torna-se em simultâneo disponível para um grande número de utilizadores, tal como uma nova versão de filmes ou uma atualização de software.
> 
> 

Este tutorial orienta-o através da pré-carregamento de conteúdo em cache em todos os nós de limite da CDN do Azure.

## <a name="to-pre-load-assets"></a>A pré-carregar recursos
1. No [portal do Azure](https://portal.azure.com), navegue para o perfil CDN com o ponto final que pretende carregar previamente. É aberto o painel de perfil.
    
2. Clique no ponto final na lista. É aberto o painel de ponto final.
3. A partir do painel de ponto final de CDN, selecione **carga**.
   
    ![Painel de ponto final CDN](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)
   
    O **carga** abre o painel.
   
    ![Painel de carga CDN](./media/cdn-preload-endpoint/cdn-load-blade.png)
4. Para **caminho de conteúdo**, introduza o caminho completo de cada recurso que pretende carregar (por exemplo, `/pictures/kitten.png`).
   
   > [!TIP]
   > Mais **caminho de conteúdo** caixas de texto serão apresentada depois de iniciar a introduzir texto para permitir-lhe criar uma lista de vários recursos. Para eliminar os recursos na lista, selecione o botão de reticências (…), em seguida, selecione **eliminar**.
   > 
   > Cada caminho de conteúdo tem de ser um URL relativo, que se adeque às seguintes [expressões regulares](https://msdn.microsoft.com/library/az24scfc.aspx):  
   > - Carregar um caminho de ficheiro único: `@"^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$"`;  
   > - Carregar um único ficheiro com a cadeia de consulta:`@"^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$";` 
   > 
   > Cada elemento tem de ter o suas próprias caminho. Não há nenhuma funcionalidade de caráter universal para recursos de pré-ao carregar.
   > 
   > 
   
    ![Botão de carga](./media/cdn-preload-endpoint/cdn-load-paths.png)
5. Quando tiver terminado de introduzir os caminhos de conteúdo, selecione **carga**.
   

> [!NOTE]
> Há uma limitação de pedidos de carga de 10 por minuto por perfil da CDN. 50 caminhos simultâneos podem ser processados em simultâneo. Cada caminho tem um limite de comprimento do caminho de 1024 carateres.
> 
> 

## <a name="see-also"></a>Consulte também
* [Remover um ponto final de CDN do Azure](cdn-purge-endpoint.md)
* [Referência da API de REST de CDN do Azure: pré-carregar conteúdo num ponto final](https://docs.microsoft.com/en-us/rest/api/cdn/endpoints/loadcontent)
* [Referência da API de REST de CDN do Azure: remover o conteúdo de um ponto final](https://docs.microsoft.com/en-us/rest/api/cdn/endpoints/purgecontent)

