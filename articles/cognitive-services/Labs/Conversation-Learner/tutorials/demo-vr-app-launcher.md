---
title: Iniciador de aplicações de realidade virtual, modelos de aprendiz de conversação - serviços cognitivos da Microsoft de demonstração | Documentos da Microsoft
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
ms.openlocfilehash: 896ec007c03e30e5c20a5344430be040271bc00b
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171155"
---
# <a name="demo-virtual-reality-app-launcher"></a>Demonstração: Iniciador de aplicações de realidade Virtual

Essa demonstração ilustra um iniciador de modelo realidade virtual, que suporta os comandos, como "Iniciar Skype e colocar na parede." Um usuário precisa inserir um nome de aplicação e a localização para iniciar a aplicação. Iniciar o modelo é processado por uma chamada à API. Quando um nome de aplicação é reconhecido do usuário, o entityDetectionCallback verifica se a aplicação pedida corresponde a uma ou mais aplicações na lista de aplicações instaladas. Ele manipula o caso em que a aplicação pedida não está instalada e em que o nome da aplicação é ambíguo (corresponde a mais do que uma aplicação instalada).

## <a name="video"></a>Vídeo

[![Demonstração VR aplicação pré-visualização](http://aka.ms/cl-demo-vrapp-preview)](http://aka.ms/blis-demo-vrapp)

## <a name="requirements"></a>Requisitos

Este tutorial requer que o bot VRAppLauncher está em execução:

    npm run demo-vrapp
    
### <a name="open-the-demo"></a>Abra a demonstração

Na lista de modelo da IU da web, clique em VRAppLauncher. 

## <a name="entities"></a>Entidades

Nós acabamos de criar quatro entidades:

- AppName: por exemplo Skype
- PlacementLocation: de parede de exemplo
- UnknownAppName: uma entidade programática, que o sistema define quando ele não reconhece o nome de entidade o usuário diz, por exemplo, porque ainda não foi instalado.
- DisAmbigAppNames: disse que uma matriz de dois ou mais nomes de aplicação instalada que correspondem aos quais o utilizador. 

![](../media/tutorial_vrapplauncher_entities.PNG)

### <a name="actions"></a>Ações

Criamos um conjunto de ações que inclui uma API chamada LaunchApp, que irá iniciar a chamada de função para iniciar uma aplicação.

![](../media/tutorial_vrapplauncher_actions.PNG)

### <a name="training-dialogs"></a>Caixas de diálogo de treinamento
Definimos um número de caixas de diálogo de treinamento.

![](../media/tutorial_vrapplauncher_dialogs.PNG)

Por exemplo, vamos tentar uma sessão de ensino.

1. Clique em caixas de diálogo do Train, em seguida, nova caixa de diálogo de comboio.
1. Introduza "Olá".
2. Clique em ação de pontuação.
3. Clique para selecionar "aplicação que pretende iniciar?"
4. Introduza "outlook".
    - LUIS reconhece-o como uma entidade.
5. Clique em ações de pontuação.
3. Clique para selecionar "onde quer que ela colocado?"
4. Introduza "na parede".
    - LUIS reconhece-o como um PlacementLocation.
2. Introduza as ações de pontuação.
6. Selecione 'LaunchApp'
7. System: "a iniciar o outlook na parede".
    - Isso acionada uma chamada à API. O código para esta chamada está em c:\<\installedpath > \src\demos\demoVRAppLauncher.ts. No entanto, não, na verdade, contém o código para iniciar o Outlook para esta demonstração.
    - Limpa as entidades AppName e PlacementLocation. O retorna a cadeia acima como resposta.
4. Clique em concluído ensino.

![](../media/tutorial_vrapplauncher_callbackcode.PNG)

Vamos começar outra sessão de treinamento para manipular entidades desconhecidas e ambíguas.

1. Clique em nova caixa de diálogo de comboio.
1. Introduza "start OneNote". 
    - O modelo reconhece OneNote como um nome de aplicação. O `EntityDetectionCallback` função definida no código de resolvido o nome introduzido pelo utilizador para um nome de aplicação, fazendo-o corresponder para a lista de aplicações definida no código. Em seguida, devolve o conjunto de todas as aplicações correspondentes. 
    - A lista de correspondências de zero, significa que a aplicação não está instalada. Coloca-o no unknownAppName.
    - Se ele encontrar mais do que uma aplicação, copia-as para o `DisambigAppNames` e limpa a entidade de AppName.
2. Clique em ação de pontuação.
3. Clique para selecionar "Lamentamos, mas eu não conheço a aplicação $UknownAppName."
4. Introduza "start Amazon". Tentaremos o outro caminho.
5. Clique em ações de pontuação.
    - Vídeo do Amazon e música da Amazon estão agora no `DisambigAppNames` memória e OneNote foi limpo.
3. Clique em Selecionar para "Há alguns aplicativos que pareçam como esse..."
    - A pontuação não é muito alta, porque definimos apenas algumas caixas de diálogo de treinamento até este ponto. Definir mais caixas de diálogo de treinamento seria tornar o modelo mais decisivo.
2. Introduza as ações de pontuação.
4. Clique em concluído ensino.

Agora viu como fazer a resolução de entidades. A demonstração também ilustrado retornos de chamada de API e mostrei um modelo para coletar informações, verificar a existência de presença e ambiguidade e tomar a ação correta em conformidade.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Implementar um bot de aprendiz de conversação](../deploy-to-bf.md)
