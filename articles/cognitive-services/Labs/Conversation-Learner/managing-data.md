---
title: Gestão de dados de utilizador com conversação Learner - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba como gerir os dados de utilizador com Learner conversação.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 8d42f903559a1e07b42ded33972be4b552f21b5e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353977"
---
# <a name="managing-user-data"></a>Gestão de dados de utilizador

Esta página descreve o que o serviço de nuvem de conversação Learner regista quando realizar caixas de diálogo com os utilizadores finais.  Também descreve como associar registos de conversação Learner IDs de utilizador, para que possa obter ou eliminar todos os registos associados a um utilizador específico.

## <a name="overview-of-end-user-data-logging"></a>Descrição geral do registo de dados do utilizador final

Por predefinição, o serviço de nuvem de conversação Learner regista interações entre os utilizadores finais e a sua bot.  Estes registos são importantes para melhorar a sua bot, permitindo-lhe identificar os casos onde a bot extraídos a entidade incorreta ou selecionar a ação incorreta.  Estes erros, em seguida, podem ser corrigidos pela vai para a página "Registo diálogos" da IU, efetuar as correções e armazenar esta caixa de diálogo corrigida como uma caixa de diálogo de formação novo. Para obter mais informações, consulte o tutorial num "Registo caixas de diálogo."

## <a name="how-to-disable-logging"></a>Como desativar o registo

Pode controlar se conversações com os utilizadores finais estão na página "Definições" para a sua aplicação Learner conversação.  Há uma caixa de verificação "Conversações de registo".  Desmarcando esta caixa, não serão registadas conversações com os utilizadores finais.

## <a name="what-is-logged"></a>O que é registado 

Caixas de diálogo de registo, conversação Learner armazena a intervenção do utilizador, os valores de entidade, ações selecionadas e carimbos de cada vez.  Estes registos são armazenados durante um período de tempo e, em seguida, são eliminados (consulte a página de ajuda "valor predefinido e limites" para obter detalhes).  

Conversação Learner cria um ID exclusivo para cada caixa de diálogo com sessão iniciada.  Conversação Learner *não* armazenar um identificador de utilizador com caixas de diálogo com sessão iniciada.  

## <a name="associating-logged-dialogs-with-a-user-id"></a>Associar registados caixas de diálogo com um ID de utilizador

Muitas vezes, é importante ser capaz de associar as caixas de diálogo registadas com o ID do utilizador – por exemplo, para conseguir obter ou eliminar caixas de diálogo com sessão iniciada a partir de um utilizador específico.  Uma vez que Learner de conversação não armazena um identificador de utilizador, esta associação tem de ser mantida por código do programador.  

Para criar esse mapeamento, obter o ID da caixa de diálogo com sessão iniciada no `EntityDetectionCallback`; em seguida, no armazenamento do seu bot, armazene a associação entre o ID de utilizador e esta caixa de diálogo com sessão iniciada.  

```
cl.EntityDetectionCallback(async (text: string, memoryManager: ClientMemoryManager): Promise<void> => {

    // Get user and session info
    var sessionData = memoryManager.SessionInfo();

    // sessionData.sessionId is the ID of this logged dialog.
    // In your bot-specific datastore, store an association
    // bewteen your user identifier and this session ID.
    console.log(sessionData.logDialogId)

    // sessionData.userId and sessionData.userName are the 
    // user ID and user name as defined by the channel the user
    // is on (eg, Skype, Teams, Facebook Messenger, etc.).
    // For some channels, this will be undefined.
    // This information is NOT stored with the logged dialog.
    console.log(sessionData.userId);
    console.log(sessionData.userName);

})
```

## <a name="headers-for-http-calls"></a>Cabeçalhos para chamadas HTTP

Em cada um das chamadas HTTP abaixo, adicione o seguinte cabeçalho:

```
Ocp-Apim-Subscription-Key=<LUIS_AUTHORING_KEY>
```

onde `<LUIS_AUTHORING_KEY>` é LUIS chave utilizada para aceder às suas aplicações de Learner conversação de criação.

## <a name="how-to-obtain-raw-data-for-a-logged-dialog"></a>Como obter os dados não processados para uma caixa de diálogo com sessão iniciada

Para obter os dados não processados para uma caixa de diálogo de registo, pode utilizar esta chamada HTTP:

```
GET https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/logdialog/<logDialogId>
```

Onde `<appId>` é o GUID para esta aplicação de conversação Learner e `<logDialgoId>` é o ID da caixa de diálogo de registo que pretende obter.  

Tenha em atenção que caixas de diálogo de registo podem ser editadas pelo programador e, em seguida, armazenadas como preparar as caixas de diálogo.  Quando isto é feito, conversação Learner armazena o ID da caixa de diálogo de registo "origem" com a caixa de diálogo de formação.  Além disso, uma caixa de diálogo de formação pode ser "branched" na IU do; Se uma caixa de diálogo de formação tem um ID de caixa de diálogo de registo de origem associada, em seguida, ramos essa caixa de diálogo de formação serão marcados com o mesmo ID de caixa de diálogo de registo.

Para obter todas as caixas de diálogo de formação que foram derivadas de uma caixa de diálogo de registo, siga estes passos.

Em primeiro lugar, obter todas as caixas de diálogo de formação:

```
GET https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/traindialogs
```

Onde `<appId>` é o GUID para esta aplicação Learner conversação.  

Esta ação devolve todas as caixas de diálogo de formação.  Procurar nesta lista associada `sourceLogDialogId`e tenha em atenção associada `trainDialogId`. 

Para um único preparar diálogo por ID:

```
GET https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/traindialog/<trainDialogId>
```

Onde `<appId>` é o GUID para esta aplicação de conversação Learner e `<trainDialogId>` é o ID da caixa de diálogo de formação que pretende obter.  

## <a name="how-to-delete-a-logged-dialog"></a>Como eliminar uma caixa de diálogo com sessão iniciada

Se pretender eliminar uma caixa de diálogo de registo recebe o ID, pode utilizar esta chamada HTTP:

```
DELETE https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/logdialog/<logDialogId>
```

Onde `<appId>` é o GUID para esta aplicação de conversação Learner e `<logDialogId>` é o ID da caixa de diálogo de registo que pretende eliminar. 

Se pretender eliminar uma caixa de diálogo de formação dada o respetivo ID, pode utilizar esta chamada HTTP:

```
DELETE https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/traindialog/<trainDialogId>
```

Onde `<appId>` é o GUID para esta aplicação de conversação Learner e `<trainDialogId>` é o ID da caixa de diálogo de formação que pretende eliminar. 
