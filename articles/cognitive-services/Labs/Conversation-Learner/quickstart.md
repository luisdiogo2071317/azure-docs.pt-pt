---
title: Como criar uma aplicação de Learner de conversação com o Node.js - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba como criar uma aplicação de Learner de conversação com o Node.js.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: a3a51aa86a30b060c8dc4113da69462904d7df54
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355087"
---
# <a name="create-a-conversation-learner-application-using-nodejs"></a>Criar uma aplicação de Learner de conversação com o Node.js

Conversação Learner reduz a complexidade da criação bots. Permite que um híbrida desenvolvimento-fluxo de trabalho escrito à mão código e a aprendizagem automática reduzir a quantidade de código necessário escrever bots. Determinados fixas partes da sua aplicação, tais como a verificar se o utilizador inicia sessão ou efetuar um pedido de API para verificar o inventário de arquivo, podem ainda ser codificadas. No entanto, outras alterações na seleção de estado e a ação podem ser adquiridas a partir de caixas de diálogo de exemplo fornecidas pelo programador ou especialista de domínio.

## <a name="invitation-required"></a>Convite necessário

*É necessário um convite para aceder à Learner de conversação do projeto.*

Projeto conversação Learner é composta por um SDK que adiciona ao seu bot e um serviço em nuvem que acede ao SDK para o machine learning.  Atualmente, o acesso ao serviço de nuvem de projeto conversação Leaner requer um convite.  Se que ainda não foi convidado já, [pedir um convite](https://aka.ms/conversation-learner-request-invite).  Se não recebeu um convite, será possível aceder à API de nuvem.

## <a name="prerequisites"></a>Pré-requisitos

- Nó 8.5.0 ou superior e npm 5.3.0 ou superior. Instalar a partir do [ https://nodejs.org ](https://nodejs.org).
  
- Chave de criação de LUIS:

  1. Inicie sessão no [ http://www.luis.ai ](http://www.luis.ai).

  2. Clique em seu nome no canto superior direito, em seguida, em "definições"

  3. Chave de criação é apresentado na página resultante

  (A sua chave de criação de LUIS serve 2 funções.  Em primeiro lugar, irá servir como a Learner conversação chave de criação.  Segundo, conversação Learner utiliza LUIS para extração de entidade; o chave de criação de LUIS é utilizado para criar modelos de LUIS em seu nome)

- Do browser Google Chrome. Instalar a partir do [ https://www.google.com/chrome/index.html ](https://www.google.com/chrome/index.html).

- Git. Instalar a partir do [ https://git-scm.com/downloads ](https://git-scm.com/downloads).

- VSCode. Instalar a partir do [ https://code.visualstudio.com/ ](https://code.visualstudio.com/). Tenha em atenção de que esta seja recomendada, não é necessária.

## <a name="quick-start"></a>Início rápido 

1. Instalar e criar:

    ```bash    
    git clone https://github.com/Microsoft/ConversationLearner-Samples cl-bot-01
    cd cl-bot-01
    npm install
    npm run build
    ```

    > [!NOTE]
    > Durante a `npm install`, pode ignorar este erro, se ocorrer: `gyp ERR! stack Error: Can't find Python executable`

2. Configure:

   Crie um ficheiro chamado `.env` no diretório `cl-bot-01`.  O conteúdo do ficheiro deve ser:

   ```
   LUIS_AUTHORING_KEY=<your LUIS authoring key>
   ```

3. Inicie bot:

    ```
    npm start
    ```

    Isto é executado o genérico bot vazio `cl-bot-01/src/app.ts`.

3. Execute a conversação Learner IU:

    ```bash
    [open second command prompt window]
    cd cl-bot-01
    npm run ui
    ```

4. Browser abrir http://localhost:5050 

Agora está a utilizar a conversação Learner e pode criar e ensinar um modelo de conversação Learner.  

> [!NOTE]
> Na iniciação, Learner de conversação do projeto está disponível por convite.  Se http://localhost:5050 mostra um HTTP `403` erro, isto significa que a conta não foi convidada.  . [Pedir um convite](https://aka.ms/conversation-learner-request-invite).

## <a name="tutorials-demos-and-switching-between-bots"></a>Tutoriais, demonstrações e alternar entre bots

As instruções acima iniciado o genérico bot vazio.  Para executar um tutorial ou demonstração bot em vez disso:

1. Se tiver web conversação Learner IU abrir, regressar à lista de aplicações em http://localhost:5050/home.
    
2. Se estiver a executar outro bot (como `npm start` ou `npm run demo-pizza`), pare.  Não é necessário parar o processo de IU ou feche o browser web.

3. Execute um bot de demonstração na linha de comandos (passo 2 acima).  Demonstrações incluem:

  ```bash
  npm run tutorial-general
  npm run tutorial-entity-detection
  npm run tutorial-session-callbacks
  npm run tutorial-api-calls
  npm run demo-password
  npm run demo-pizza
  npm run demo-storage
  npm run demo-vrapp
  ```

4. Se não estiver já, mudar para a web conversação Learner IU no Chrome por carregar http://localhost:5050/home. 

5. Clique em "Tutoriais de importação" (apenas tem de ser feita uma vez).  Isto irá demorar sobre um minuto e irá copiar os modelos de conversação Learner para todos os tutoriais para a conta de conversação Learner.

6. Clique no modelo de demonstração na conversação Learner IU que corresponde a demonstração de que foi iniciado.

Ficheiros de origem para os demonstrações estão no `cl-bot-01/src/demos`

## <a name="create-a-bot-which-includes-back-end-code"></a>Criar um bot que inclui o código de back-end

1. Se tiver web conversação Learner IU abrir, regressar à lista de aplicações em http://localhost:5050/home.
    
2. Se estiver a executar um bot (como `npm run demo-pizza`), pare.  Não é necessário parar o processo de IU ou feche o browser web.

3. Se assim o desejar, editar o código no `cl-bot-01/src/app.ts`.

4. Reconstruir e voltar a iniciar bot:

    ```bash    
    npm run build
    npm start
    ```

5. Se não estiver já, mudar para a web conversação Learner IU no Chrome por carregar http://localhost:5050/home. 

6. Criar uma nova aplicação de conversação Learner na IU e inicie de ensino.

7. Para efetuar alterações de código no `cl-bot-01/src/app.ts`, repita os passos acima, a partir do passo 2.

## <a name="vscode"></a>VSCode

No VSCode, existe são executadas configurações para cada demonstração e para "bot vazio" no `cl-bot-01/src/app.ts`.  Abra o `cl-bot-01` pasta VSCode.

## <a name="advanced-configuration"></a>Configuração avançada

Há um modelo `.env.example` ficheiro mostra que ambiente pode ser definido para configurar os exemplos de variáveis.

Pode ajustar estas portas para evitar conflitos entre a outros serviços em execução no seu computador através da adição de um `.env` ficheiro raiz do projeto:

```bash
cp .env.example .env
```

Esta opção utiliza a configuração padrão, que permite-lhe executar localmente a sua bot e começar a utilizar Learner conversação.  (Mais tarde no, para implementar o bot o Framework Bot, algumas edições para este ficheiro serão necessária.)

## <a name="support"></a>Suporte

- Marque as perguntas [Stack Overflow](https://stackoverflow.com) com "microsoft cognitivos"
- Pedir uma funcionalidade no nosso [página de voz do utilizador](https://aka.ms/conversation-learner-uservoice)
- Abra um problema no nosso [repositório do github](https://github.com/Microsoft/ConversationLearner-Samples)

## <a name="contributing"></a>Contribuir

Este projeto adotou o [Microsoft Open Source Code of Conduct (Código de Conduta do Microsoft Open Source)](https://opensource.microsoft.com/codeofconduct/). Para obter mais informações, consulte o [código de conduta FAQ](https://opensource.microsoft.com/codeofconduct/faq/) ou contacte [ opencode@microsoft.com ](mailto:opencode@microsoft.com) com quaisquer perguntas ou comentários adicionais.

## <a name="source-repositories"></a>Repositórios de origem

- [Exemplos de conversationlearner](https://github.com/Microsoft/ConversationLearner-Samples)
- [conversationlearner-sdk](https://github.com/Microsoft/ConversationLearner-SDK)
- [modelos de conversationlearner](https://github.com/Microsoft/ConversationLearner-Models)
- [IU conversationlearner](https://github.com/Microsoft/ConversationLearner-UI)
- [conversationlearner webchat](https://github.com/Microsoft/ConversationLearner-WebChat)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Olá, mundo](./tutorials/1-hello-world.md)
