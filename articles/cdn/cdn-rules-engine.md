---
title: Substituir o comportamento HTTP utilizando o motor de regras do Azure CDN | Microsoft Docs
description: "O motor de regras permite-lhe personalizar como os pedidos de HTTP são processados pela CDN do Azure, tais como o fornecimento de determinados tipos de conteúdo a bloquear, definir uma política de colocação em cache e modificar os cabeçalhos de HTTP."
services: cdn
documentationcenter: 
author: dksimpson
manager: akucer
editor: 
ms.assetid: 625a912b-91f2-485d-8991-128cc194ee71
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: mazha
ms.openlocfilehash: fe3df703f7eb244a52756c4d015e9ea598224ce1
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/02/2018
---
# <a name="override-http-behavior-using-the-azure-cdn-rules-engine"></a>Substituir o comportamento HTTP utilizando o motor de regras da CDN do Azure
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Descrição geral
O motor de regras da CDN do Azure permite-lhe personalizar a forma como os pedidos de HTTP são processados. Por exemplo, a bloquear a entrega de determinados tipos de conteúdo, definir uma política de colocação em cache ou modificar um cabeçalho de HTTP. Este tutorial demonstra como criar uma regra que altera o comportamento de colocação em cache de elementos da CDN. Para obter mais informações sobre a sintaxe do motor de regras, consulte [regras da CDN do Azure motor referência](cdn-rules-engine-reference.md).

## <a name="access"></a>Access
Para o motor de regras de acesso, tem de selecionar primeiro **gerir** da parte superior do **perfil da CDN** página para aceder à página de gestão CDN do Azure. Dependendo se o ponto final está otimizado para a aceleração de dinâmicas do site (DSA), em seguida, aceder o motor de regras com o conjunto de regras adequadas para o tipo de ponto final:

- Pontos finais otimizados para entrega geral web ou outra otimização não DSA: 
    
    Selecione o **HTTP grande** separador, em seguida, selecione **motor de regras**.

    ![Motor de regras para HTTP](./media/cdn-rules-engine/cdn-http-rules-engine.png)

- Pontos finais otimizados para DSA: 
    
    Selecione o **ADN** separador, em seguida, selecione **motor de regras**. 
    
    ADN é um termo utilizado da Verizon para especificar o conteúdo DSA. Quaisquer regras criadas aqui são ignoradas por quaisquer pontos finais no seu perfil que não estão otimizados para DSA. 

    ![Motor de regras para DSA](./media/cdn-rules-engine/cdn-dsa-rules-engine.png)

## <a name="tutorial"></a>Tutorial
1. Do **perfil da CDN** página, selecione **gerir**.
   
    ![Botão de gerir do perfil de CDN](./media/cdn-rules-engine/cdn-manage-btn.png)
   
    É aberto o portal de gestão do CDN.
2. Selecione o **HTTP grande** separador, em seguida, selecione **motor de regras**.
   
    São apresentadas as opções para uma nova regra.
   
    ![Novas opções de regra CDN](./media/cdn-rules-engine/cdn-new-rule.png)
   
   > [!IMPORTANT]
   > A ordem em que várias regras estão listadas afeta a forma como são processadas. Uma regra subsequente pode substituir as ações especificadas por uma regra de anterior.
   > 
3. Introduza um nome no **nome / descrição** caixa de texto.
4. Identifique o tipo de pedidos que se aplica a regra. Utilizar a condição de correspondência predefinido, **sempre**. 
   
   ![Condição de correspondência de regra de CDN](./media/cdn-rules-engine/cdn-request-type.png)
   
   > [!NOTE]
   > Várias condições de correspondência estão disponíveis na lista pendente. Para obter informações sobre a correspondência atualmente selecionado condição, selecione o ícone azul informativo à esquerda.
   > 
   >  Para obter uma lista detalhada das expressões condicionais, consulte [regras motor expressões condicionais](cdn-rules-engine-reference-match-conditions.md).
   >  
   > Para obter uma lista detalhada dos termos de correspondência, consulte [regras condições de correspondência do motor](cdn-rules-engine-reference-match-conditions.md).
   > 
   > 
1. Para adicionar uma nova funcionalidade, selecione o  **+**  junto a **funcionalidades**.  Na lista pendente no lado esquerdo, selecione **idade de máxima de interno Force**.  Na caixa de texto que aparece, introduza **300**. Não altere os valores predefinidos restantes.
   
   ![Funcionalidade de regra CDN](./media/cdn-rules-engine/cdn-new-feature.png)
   
   > [!NOTE]
   > Várias funcionalidades estão disponíveis na lista pendente. Para informações sobre a funcionalidade selecionada atualmente, selecione o ícone informativo azul à esquerda. 
   >
   > Para **idade de máxima de interno Force**, o elemento `Cache-Control` e `Expires` cabeçalhos são substituídos para controlar quando o nó de extremidade CDN atualiza o elemento da origem. Neste exemplo, o nó de extremidade CDN coloca em cache o elemento de 300 segundos ou 5 minutos, antes de que atualiza o elemento de origem.
   > 
   > Para obter uma lista detalhada das funcionalidades, consulte [regras motor funcionalidades](cdn-rules-engine-reference-features.md).
   > 
   > 
1. Clique em de **adicionar** botão para guardar a nova regra.  A nova regra agora está a aguardar aprovação. Depois de tiver sido aprovado, o estado é alterado de **XML pendente** para **XML Active Directory**.
   
   > [!IMPORTANT]
   > Alterações de regras podem demorar até 90 minutos para propagar pela CDN.
   > 
   > 

## <a name="see-also"></a>Consulte também
* [Descrição geral CDN do Azure](cdn-overview.md)
* [Referência do motor de regras](cdn-rules-engine-reference.md)
* [Condições de correspondência do motor de regras](cdn-rules-engine-reference-match-conditions.md)
* [Motor de regras de expressões condicionais](cdn-rules-engine-reference-conditional-expressions.md)
* [Funcionalidades do motor de regras](cdn-rules-engine-reference-features.md)
* [Azure sextas: Nova premium funcionalidades poderosas CDN do Azure](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (vídeo)