---
title: Modelo de aprendiz de conversação de demonstração, palavra-passe reposta - serviços cognitivos da Microsoft | Documentos da Microsoft
titleSuffix: Azure
description: Saiba como criar um modelo de aprendiz de conversação de demonstração.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: f633dd375d690a1c3e66a2a6e02ae69665dbe960
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39170638"
---
# <a name="demo-password-reset"></a>Demonstração: Reposição de palavra-passe
Essa demonstração ilustra um bot de suporte técnico simples que pode ajudar na reposições de palavra-passe. 

Ele mostra como o aprendiz de conversação pode aprender fluxos de caixa de diálogo não trivial, ative a multi sequências, incluindo uma classe de fora do domínio. Esta demonstração não utiliza qualquer código ou entidades.

## <a name="video"></a>Vídeo

[![Pré-visualização de palavra-passe de demonstração](http://aka.ms/cl-demo-password-preview)](http://aka.ms/blis-demo-password)

## <a name="requirements"></a>Requisitos
Este tutorial requer que o bot de reposição de palavra-passe está em execução

    npm run demo-password

### <a name="open-the-demo"></a>Abra a demonstração

Na lista de modelo da IU da web, clique na reposição de palavra-passe de demonstração do Tutorial. 

### <a name="actions"></a>Ações

Criámos o conjunto de ações em que o utilizador está à procura de ajuda com a palavra-passe, incluindo soluções.

![](../media/tutorial_pw_reset_actions.PNG)

### <a name="training-dialogs"></a>Caixas de diálogo de treinamento

Há uma série de caixas de diálogo de treinamento. Também existem demonstrações de um fora da classe de domínio – por exemplo, pedidos de utilizador, como 'informações' estão fora do domínio; o bot, deu os exemplos de algumas fora de solicitações de domínio e pode responder com "Eu não pode ajudar com isso."

![](../media/tutorial_pw_reset_entities.PNG)

Por exemplo, vamos tentar uma sessão de ensino.

1. Clique em caixas de diálogo do Train, em seguida, nova caixa de diálogo de comboio.
1. Introduza "Eu perdi minha palavra-passe".
2. Clique em ação de pontuação.
3. Clique para selecionar "Isso é para a sua conta local ou uma conta Microsoft?"
4. Introduza "Conta Local".
5. Clique em ações de pontuação.
3. Clique para selecionar "qual versão do Windows tem?"
4. Introduza "Windows 8'.
5. Clique em ações de pontuação.
6. Selecione "solução: como repor a palavra-passe no Windows 8."
4. Clique em concluído ensino.

Vamos experimentar como o bot pode aprender uma classe de fora do domínio.

1. Clique em caixas de diálogo do Train, em seguida, nova caixa de diálogo de comboio.
1. Introduza "pesquisa na web".
    - Este é um exemplo da classe de fora do domínio. 
2. Clique em ação de pontuação.
3. Clique para selecionar "Lamentamos, mas eu não pode ajudar com isso."
    - Tenha em atenção que a classificação para esta opção é baixa. Mas, após um pouco mais de infância, a pontuação obterá superior.
4. Clique em concluído ensino.

Agora viu como criar uma demonstração de suporte técnico básico, e como pode saber fornecer soluções e também lidar com fora de consultas de exemplo.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Demonstração - order de pizza](./demo-pizza-order.md)
