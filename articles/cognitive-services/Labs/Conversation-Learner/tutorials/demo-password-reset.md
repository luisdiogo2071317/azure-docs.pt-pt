---
title: Demonstração de aplicação de conversação Learner, reposição de palavra-passe - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba como criar uma aplicação de conversação Learner de demonstração.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 24d61787a79ee1a1a9737c417aa966cc8fd75930
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353995"
---
# <a name="demo-password-reset"></a>Demonstração: Reposição de palavra-passe
Esta demonstração ilustra um bot de suporte técnico simples que pode ajudar a reposições de palavra-passe. 

Mostra como a conversação Learner pode saber mais fluxos de caixa de diálogo não trivial, ative multi sequências, incluindo uma classe de fora do domínio. Esta demonstração não utilizar qualquer código ou entidades.

## <a name="requirements"></a>Requisitos
Este tutorial, necessita que o bot de reposição de palavra-passe está em execução

    npm run demo-password

### <a name="open-the-demo"></a>Abra a demonstração

Na lista de aplicações a IU da web, clique na reposição de palavra-passe de demonstração do Tutorial. 

### <a name="actions"></a>Ações

Criámos o conjunto de ações em que o utilizador está à procura de ajuda com a palavra-passe, incluindo as soluções.

![](../media/tutorial_pw_reset_actions.PNG)

### <a name="training-dialogs"></a>Caixas de diálogo de formação

Há uma série de caixas de diálogo de formação. Também existem demonstrações de um fora da classe de domínio – por exemplo, pedidos de utilizador, tal como 'impulsionar as direções' está fora do domínio; o bot foi indicado exemplos de algumas fora do domínio pedidos e pode responder com 'Posso não consegue ajudar com que.'

![](../media/tutorial_pw_reset_entities.PNG)

Por exemplo, vamos tentar uma sessão de ensino.

1. Clique em caixas de diálogo de formação, caixa de diálogo de formação, em seguida, novo.
1. Introduza 'Posso perdeu a minha palavra-passe'.
2. Clique em ação de pontuação.
3. Clique para seleccionar "Destina-se que a conta local ou uma conta Microsoft?"
4. Introduza 'Conta de Local'.
5. Clique em ações de pontuação.
3. Clique para selecionar a "versão do Windows tem?"
4. Introduza ' Windows 8'.
5. Clique em ações de pontuação.
6. Selecione ' solução: como repor a palavra-passe no Windows 8.'
4. Clique em concluído de ensino.

Vamos tentar como o bot pode obter uma classe de fora do domínio.

1. Clique em caixas de diálogo de formação, caixa de diálogo de formação, em seguida, novo.
1. Introduza "pesquisa na web".
    - Este é um exemplo da classe de fora do domínio. 
2. Clique em ação de pontuação.
3. Clique para seleccionar 'Lamentamos, posso que não pode ajudar.'
    - Tenha em atenção de que a classificação para esta opção é baixa. Mas após um pouco mais de ensino, a classificação obterão superior.
4. Clique em concluído de ensino.

Constatou agora como criar uma demonstração de suporte técnico básico e como pode saber fornecer soluções e também processar fora de consultas de exemplo.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Demonstração - ordem pizza](./demo-pizza-order.md)
