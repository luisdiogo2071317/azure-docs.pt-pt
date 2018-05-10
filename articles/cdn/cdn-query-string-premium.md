---
title: Controlar o comportamento com cadeias de consulta - escalão premium a colocação em cache a CDN do Azure | Microsoft Docs
description: Cadeia de consulta do Azure CDN controla como os ficheiros são colocadas em cache quando um pedido web contém uma cadeia de consulta a colocação em cache. Este artigo descreve a colocação em cache na CDN do Azure Premium da Verizon produto de cadeia de consulta.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: 99db4a85-4f5f-431f-ac3a-69e05518c997
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: mazha
ms.openlocfilehash: a1ef5f3e502b5383343dbec2c427d36054a3edc4
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings---premium-tier"></a>Controlo do Azure CDN comportamento com cadeias de consulta - escalão premium a colocação em cache
> [!div class="op_single_selector"]
> * [Escalão Standard](cdn-query-string.md)
> * [Escalão Premium](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a>Descrição geral
Com o Azure entrega rede conteúdos (CDN), pode controlar a forma como os ficheiros são colocadas em cache para um pedido web que contém uma cadeia de consulta. Um pedido web com uma cadeia de consulta, a cadeia de consulta é que parte do pedido que ocorre após um ponto de interrogação (?). Uma cadeia de consulta pode conter um ou mais pares de valor de chave, em que o nome do campo e o respetivo valor são separados por um sinal de igual (=). Cada par chave-valor é separado por um e comercial (&). Por exemplo, http:\//www.contoso.com/content.mov?field1=value1 & field2 = value2. Se existir mais do que um par chave-valor de uma cadeia de consulta de um pedido, não importa a sua ordem. 

> [!IMPORTANT]
> Os produtos da CDN standard e premium fornecem a mesma cadeia de consulta funcionalidade a colocação em cache, mas a interface de utilizador é diferente. Este artigo descreve a interface para **CDN do Azure Premium da Verizon**. Para a cadeia de consulta de colocação em cache com produtos de padrão da CDN do Azure, consulte o artigo [CDN do Azure de controlo de colocação em cache comportamento com cadeias de consulta - escalão standard](cdn-query-string.md).
>


Estão disponíveis três modos de cadeia de consulta:

- **cache de padrão**: modo predefinido. Neste modo, o nó do CDN ponto de presença (POP) transfere as cadeias de consulta do requerente para o servidor de origem no primeiro pedido e coloca em cache o elemento. Todos os pedidos subsequentes para o elemento que são servidos do servidor POP ignorar as cadeias de consulta até que o elemento em cache expira.

    >[!IMPORTANT] 
    > Se autorização token está ativada para qualquer caminho esta conta, o modo de cache padrão é o único modo que pode ser utilizado. 

- **cache não**: neste modo, os pedidos com cadeias de consulta não estão em cache no nó POP do CDN. O nó POP obtém o elemento diretamente a partir do servidor de origem e passa-a para o requerente com cada pedido.

- **cache exclusivo**: neste modo, a cada pedido com um URL único, incluindo a cadeia de consulta é tratado como um recurso com a sua própria cache exclusivo. Por exemplo, a resposta do servidor de origem para um pedido para example.ashx?q=test1 é colocado em cache no nó de POP e devolvida para caches subsequentes com a mesma cadeia de consulta. Um pedido para example.ashx?q=test2 é colocado em cache como um recurso com a sua própria definição time-to-live separado.
   
    >[!IMPORTANT] 
    > Não utilize este modo quando a cadeia de consulta contém parâmetros mudará com cada pedido, tal como um ID de sessão ou um nome de utilizador, porque irá resultar num rácio acertos na cache de baixo.

## <a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Alterar as definições de perfis da CDN premium a colocação em cache de cadeia de consulta
1. Abrir um perfil CDN, em seguida, clique em **gerir**.
   
    ![Botão de gerir do perfil de CDN](./media/cdn-query-string-premium/cdn-manage-btn.png)
   
    É aberto o portal de gestão do CDN.
2. Coloque o cursor sobre o **HTTP grande** separador, em seguida, coloque o cursor sobre o **definições da Cache** flyout menu. Clique em **colocação em cache de cadeia de consulta**.
   
    Opções de colocação em cache de cadeias de consulta são apresentadas.
   
    ![Cadeia de consulta CDN opções a colocação em cache](./media/cdn-query-string-premium/cdn-query-string.png)
3. Selecione um modo de cadeia de consulta, em seguida, clique em **atualização**.

> [!IMPORTANT]
> Uma vez demora algum tempo para que o registo propagar pela CDN, alterações de definições de cadeia de cache poderão não ser imediatamente visíveis. Para **CDN do Azure Premium da Verizon** perfis, propagação normalmente for concluída dentro de 90 minutos.
 

