---
title: Controlar o comportamento com cadeias de consulta - escalão standard a colocação em cache a CDN do Azure | Microsoft Docs
description: Cadeia de consulta do Azure CDN controla como os ficheiros são colocadas em cache quando um pedido web contém uma cadeia de consulta a colocação em cache. Este artigo descreve a cadeia de consulta a colocação em cache em produtos padrão da CDN do Azure.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: 17410e4f-130e-489c-834e-7ca6d6f9778d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: mazha
ms.openlocfilehash: fcb4676325066dd6960070d996b1779fb3471dd9
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings---standard-tier"></a>Controlo do Azure CDN comportamento com cadeias de consulta - escalão standard a colocação em cache
> [!div class="op_single_selector"]
> * [Escalão Standard](cdn-query-string.md)
> * [Escalão Premium](cdn-query-string-premium.md)
> 

## <a name="overview"></a>Descrição geral
Com o Azure entrega rede conteúdos (CDN), pode controlar a forma como os ficheiros são colocadas em cache para um pedido web que contém uma cadeia de consulta. Um pedido web com uma cadeia de consulta, a cadeia de consulta é que parte do pedido que ocorre após um ponto de interrogação (?). Uma cadeia de consulta pode conter um ou mais pares de valor de chave, em que o nome do campo e o respetivo valor são separados por um sinal de igual (=). Cada par chave-valor é separado por um e comercial (&). Por exemplo, http:\//www.contoso.com/content.mov?field1=value1 & field2 = value2. Se existir mais do que um par chave-valor de uma cadeia de consulta de um pedido, não importa a sua ordem. 

> [!IMPORTANT]
> A CDN do Azure standard e produtos de premium fornecem a mesma cadeia de consulta funcionalidade a colocação em cache, mas a interface de utilizador é diferente. Este artigo descreve a interface para **CDN do Azure Standard da Microsoft**, **CDN do Azure Standard da Akamai** e **CDN do Azure Standard da Verizon**. Para a cache de cadeia de consulta com **CDN do Azure Premium da Verizon**, consulte [CDN do Azure de controlo de colocação em cache comportamento com cadeias de consulta - escalão premium](cdn-query-string-premium.md).

Estão disponíveis três modos de cadeia de consulta:

- **Ignorar as cadeias de consulta**: modo predefinido. Neste modo, o nó do CDN ponto de presença (POP) transfere as cadeias de consulta do requerente para o servidor de origem no primeiro pedido e coloca em cache o elemento. Todos os pedidos subsequentes para o elemento que são servidos do POP ignorar as cadeias de consulta até que o elemento em cache expira.

- **Ignorar a colocação em cache para cadeias de consulta**: neste modo, os pedidos com cadeias de consulta não estão em cache no nó POP do CDN. O nó POP obtém o elemento diretamente a partir do servidor de origem e passa-a para o requerente com cada pedido.

- **Colocar em cache todos os URLs únicos**: neste modo, a cada pedido com um URL único, incluindo a cadeia de consulta é tratado como um recurso com a sua própria cache exclusivo. Por exemplo, a resposta do servidor de origem para um pedido para example.ashx?q=test1 é colocado em cache no nó de POP e devolvida para caches subsequentes com a mesma cadeia de consulta. Um pedido para example.ashx?q=test2 é colocado em cache como um recurso com a sua própria definição time-to-live separado.
   
    >[!IMPORTANT] 
    > Não utilize este modo quando a cadeia de consulta contém parâmetros mudará com cada pedido, tal como um ID de sessão ou um nome de utilizador, porque irá resultar num rácio acertos na cache de baixo.

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Alterar as definições de perfis da CDN padrão a colocação em cache de cadeia de consulta
1. Abrir um perfil CDN, em seguida, selecione o ponto final da CDN que pretende gerir.
   
   ![Pontos finais de perfil CDN](./media/cdn-query-string/cdn-endpoints.png)
   
2. No painel esquerdo em definições, clique em **regras a colocação em cache**.
   
    ![Botão de regras de colocação em cache de CDN](./media/cdn-query-string/cdn-caching-rules-btn.png)
   
3. No **comportamento de colocação em cache de cadeia de consulta** lista, selecione um modo de cadeia de consulta, em seguida, clique em **guardar**.
   
   ![Cadeia de consulta CDN opções a colocação em cache](./media/cdn-query-string/cdn-query-string.png)

> [!IMPORTANT]
> Uma vez demora algum tempo para que o registo propagar pela CDN, alterações de definições de cadeia de cache poderão não ser imediatamente visíveis:
> - Para **CDN do Azure Standard da Microsoft** perfis, propagação normalmente conclusão na dez minutos. 
> - Para **CDN do Azure Standard da Akamai** perfis, propagação normalmente concluída num minuto. 
> - Para **CDN do Azure Standard da Verizon** e **CDN do Azure Premium da Verizon** perfis, propagação normalmente for concluída dentro de 90 minutos. 



