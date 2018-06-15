---
title: Utilizar domínios prebuilt LUIS aplicações do Azure | Microsoft Docs
description: Saiba como utilizar domínios prebuilt em aplicações de serviço de inteligente compreensão do idioma (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: 72ca0def8528adae5e46a02fa5bfccd14a3b6ab4
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355777"
---
# <a name="use-prebuilt-domains-in-luis-apps"></a>Utilizar domínios prebuilt LUIS aplicações  

Compreensão de idiomas (LUIS) fornece *domínios prebuilt*, que são prebuilt conjuntos de [pendentes](luis-how-to-add-intents.md) e [entidades](luis-concept-entity-types.md) que funcionam em conjunto para domínios ou categorias comuns de aplicações de cliente. Os domínios prebuilt tem sido previamente preparados e estão prontos para adicionar à sua aplicação LUIS. As entidades num domínio prebuilt e pendentes são totalmente personalizáveis depois adicionou-los para a sua aplicação - pode dar formação-los com utterances do sistema para que funcionam para os seus utilizadores. Pode utilizar um domínio prebuilt completo como um ponto de partida para personalização ou borrow apenas alguns pendentes ou entidades de um domínio prebuilt. 

Procurar o **domínios Prebuilt** separador para ver outros domínios prebuilt pode utilizar na sua aplicação. Clique no mosaico para um domínio para o adicionar à sua aplicação, ou clique em **mais** no seu mosaico para saber mais sobre a entidades e pendentes.

> [!TIP]
> Pode encontrar uma listagem completa dos domínios prebuilt no [referência de domínios Prebuilt](./luis-reference-prebuilt-domains.md).

![Adicionar domínio prebuilt](./media/luis-how-to-prebuilt-domain-entities/add-prebuilt-domain.png)


## <a name="add-a-prebuilt-domain"></a>Adicionar um domínio prebuilt
No **domínios Prebuilt** separador, localizar o domínio RestaurantReservation e clique em **Adicionar domínio**. Depois do domínio prebuilt foi adicionado à sua aplicação LUIS, abra **pendentes** e clique na intenção RestaurantReservation.Reserve. Pode ver que muitas utterances de exemplo já foram fornecidos e etiquetadas com entidades.

![Intenção de RestaurantReservation.Reserve](./media/luis-how-to-prebuilt-domain-entities/prebuilt-domain-restaurant-reservation.png)


## <a name="designing-luis-apps-from-prebuilt-domains"></a>Conceber aplicações LUIS prebuilt domínios
Ao utilizar domínios prebuilt na sua aplicação LUIS, pode personalizar um domínio prebuilt completo ou apenas a começar com algumas das respetivos entidades e pendentes.

## <a name="customizing-an-entire-prebuilt-domain"></a>Personalizar um domínio prebuilt completo
Domínios prebuilt foram concebidos para serem geral. Contêm muitas entidades, o que pode escolher para personalizar uma aplicação para as suas necessidades e pendentes. Se iniciar a partir de personalizar um domínio prebuilt completo, elimine as pendentes e entidades que a aplicação não tem de utilizar. Também pode adicionar alguns pendentes ou entidades para o conjunto de que o domínio prebuilt já fornece. Por exemplo, se estiver a utilizar o **eventos** prebuilt domínio para uma aplicação de evento desporto, pode adicionar entidades para equipas desporto. Quando inicia [fornecer utterances](luis-how-to-add-example-utterances.md) para LUIS, inclua termos específicos para a sua aplicação. LUIS aprende reconhecê-los e tailors o domínio prebuilt pendentes e entidades para as necessidades da sua aplicação. 

> [!TIP]
> As entidades num domínio prebuilt e pendentes funcionam melhor em conjunto. É melhor combinar pendentes e entidades do mesmo domínio quando possível.
> * Uma melhor prática consiste em utilizar pendentes relacionados ao mesmo domínio. Por exemplo, se estiver a personalizar o `MakeReservation` intenção no **locais** domínio, em seguida, selecione o `Cancel` intenção do **locais** domínio em vez da tentativa de cancelar a **Eventos** ou **utilitários** domínios.

## <a name="changing-the-behavior-of-a-prebuilt-domain-intent"></a>Alterar o comportamento de um objetivo de domínio prebuilt
Poderá achar que um domínio prebuilt contém um objetivo semelhante à que pretende ter na sua aplicação LUIS objetivo, mas pretende que se comportam de forma diferente. Por exemplo, o **locais** domínio prebuilt fornece um `MakeReservation` intenção para efetuar uma reserva de restaurante, mas pretende a sua aplicação utilizar esse objetivo para tornar as reservas de átrios. Nesse caso, pode modificar o comportamento desse objetivo, fornecendo utterances para LUIS sobre tornando reservas átrios e etiquetagem-los utilizando o `MakeReservation` intenção, por isso, em seguida, LUIS pode ser retrained para reconhecer o `MakeReservation` intenção num pedido para o livro um átrios .

> [!TIP]
> Consulte o domínio de utilitários para prebuilt pendentes que pode personalizar para utilização em qualquer domínio. Por exemplo, pode adicionar `Utilities.Repeat` a sua aplicação e formação reconhece qualquer utilizador ações querer repetir na sua aplicação.


## <a name="next-step"></a>Passo seguinte

Personalize um domínio prebuilt adicionando mais utterances de exemplo para a mesma.

> [!div class="nextstepaction"]
> [Adicionar utterances de exemplo](./luis-how-to-add-example-utterances.md)

## <a name="additional-resources"></a>Recursos adicionais

Consulte o [referência de domínios Prebuilt](./luis-reference-prebuilt-domains.md) para mais detalhes sobre os domínios prebuilt.
