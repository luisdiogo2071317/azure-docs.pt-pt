---
title: Utilizar domínios pré-concebidos para criar aplicações mais rápido em LUIS aplicações
titleSuffix: Azure Cognitive Services
description: Compreensão de idiomas (LUIS) fornece domínios pré-concebidos, que são conjuntos pré-criados de objetivos e entidades que funcionam em conjunto para domínios ou categorias comuns de aplicativos de cliente. Os domínios pré-concebidos previamente treinados e estão prontos para adicionar à sua aplicação LUIS.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 80a83fdb1e2b84ac03d0ae0cf969f1635f00cca0
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44053728"
---
# <a name="use-prebuilt-domains-in-luis-apps"></a>Utilizar domínios pré-concebidos nas aplicações de LUIS  

Compreensão de idiomas (LUIS) fornece *domínios pré-concebidos*, que são conjuntos pré-criados de [intenções](luis-how-to-add-intents.md) e [entidades](luis-concept-entity-types.md) que funcionam em conjunto para domínios ou categorias comuns de aplicações de cliente. Os domínios pré-concebidos previamente treinados e estão prontos para adicionar à sua aplicação LUIS. Os objetivos e entidades num domínio pré-criado são totalmente personalizáveis depois de adicioná-los à sua aplicação – pode treiná-la com expressões com do sistema para que eles funcionam para os seus utilizadores. Pode utilizar um domínio pré-criado completo como um ponto de partida para personalização ou emprestados apenas alguns objetivos ou entidades de um domínio pré-criado. 

Procurar os **domínios pré-concebidos** guia para ver outros domínios pré-concebidos pode utilizar na sua aplicação. Clique no mosaico para um domínio para o adicionar à sua aplicação, ou clique em **Saiba mais** em seu bloco para saber mais sobre suas intenções e entidades.

> [!TIP]
> Pode encontrar uma lista completa dos domínios criados previamente no [referência de domínios pré-concebidos](./luis-reference-prebuilt-domains.md).

![Adicionar domínio pré-criado](./media/luis-how-to-prebuilt-domain-entities/add-prebuilt-domain.png)


## <a name="add-a-prebuilt-domain"></a>Adicionar um domínio pré-concebido
Na **domínios pré-concebidos** separador, encontrar o domínio de RestaurantReservation e clique em **Adicionar domínio**. Depois do domínio pré-criado foi adicionado à sua aplicação LUIS, abra **intenções** e clique na intenção RestaurantReservation.Reserve. Pode ver que muitas expressões de exemplo já foram fornecidos e rotulados com entidades.

![Intenção de RestaurantReservation.Reserve](./media/luis-how-to-prebuilt-domain-entities/prebuilt-domain-restaurant-reservation.png)


## <a name="designing-luis-apps-from-prebuilt-domains"></a>Conceber aplicações de LUIS de domínios pré-concebidos
Ao utilizar domínios pré-concebidos na sua aplicação LUIS, pode personalizar um domínio pré-criado completo ou apenas a começar com algumas das suas intenções e entidades.

## <a name="customizing-an-entire-prebuilt-domain"></a>Personalizando um domínio pré-criado completo
Domínios pré-concebidos são concebidos para serem geral. Eles contêm muitos intenções e entidades, o que pode escolher para personalizar uma aplicação para as suas necessidades. Se iniciar a partir de personalizar um domínio pré-criado completo, elimine as intenções e entidades que seu aplicativo não precisa usar. Também pode adicionar alguns objetivos ou entidades para o conjunto de que o domínio pré-criado já fornece. Por exemplo, se estiver a utilizar o **eventos** domínio pré-criado para uma aplicação de eventos desportivos, pode adicionar entidades para as equipes do desporto. Quando inicia [fornecendo expressões com](luis-how-to-add-example-utterances.md) para LUIS, incluem os termos que são específicos para a sua aplicação. LUIS aprende a reconhecê-los e tailors do domínio pré-criado intenções e entidades às necessidades da sua aplicação. 

> [!TIP]
> Os objetivos e entidades num domínio pré-criado funcionam melhor em conjunto. É melhor combinar as intenções e entidades do mesmo domínio sempre que possível.
> * Uma prática recomendada é usar os objetivos relacionados ao mesmo domínio. Por exemplo, se estiver personalizando o `MakeReservation` intenção no **locais** domínio, em seguida, selecione o `Cancel` intenção do **locais** domínio em vez da intenção de cancelamento no **Eventos** ou **utilitários** domínios.

## <a name="changing-the-behavior-of-a-prebuilt-domain-intent"></a>Alterar o comportamento de uma intenção de domínio pré-criado
Pode achar que um domínio pré-criado contém um objetivo semelhante a um objetivo a que pretende ter na sua aplicação LUIS, mas deseja ter um comportamento diferente. Por exemplo, o **lugares** domínio pré-criado fornece um `MakeReservation` intenção para fazer uma reserva do restaurante, mas quiser a sua aplicação para utilizar esse propósito para fazer reservas de hotel. Nesse caso, pode modificar o comportamento desse objetivo, fornecendo expressões para LUIS sobre fazer reservas de hotel e a etiquetagem-los utilizando o `MakeReservation` intenção, então, LUIS pode reestruturar os reconheça o `MakeReservation` intenção num pedido para reservar um hotel .

> [!TIP]
> Verifique o domínio de utilitários para objetivos pré-criados que pode personalizar para utilização em qualquer domínio. Por exemplo, pode adicionar `Utilities.Repeat` à sua aplicação e train ele reconhece qualquer utilizador de ações, poderá querer repetir em seu aplicativo.


## <a name="next-step"></a>Passo seguinte

Personalize um domínio pré-criado com a adição de mais expressões de exemplo.

> [!div class="nextstepaction"]
> [Adicionar expressões de exemplo](./luis-how-to-add-example-utterances.md)

## <a name="additional-resources"></a>Recursos adicionais

Consulte a [referência de domínios pré-concebidos](./luis-reference-prebuilt-domains.md) para obter mais detalhes sobre os domínios criados previamente.
