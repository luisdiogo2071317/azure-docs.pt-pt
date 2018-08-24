---
title: incluir ficheiro
description: incluir ficheiro
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: include
ms.custom: include file
ms.date: 08/16/2018
ms.author: diberry
ms.openlocfilehash: 25a70d314ecb690a34e89a3dff7d2c4fad8b7a00
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42820003"
---
Acesso ao ponto final de predição é fornecido com uma chave de ponto final. Para efeitos deste início rápido, utilize a chave de iniciante gratuita à conta do LUIS. 
 
1. Inicie sessão com a sua conta do LUIS. 

    [![Lista de aplicações de captura de ecrã de compreensão de idiomas (LUIS)](media/cognitive-services-luis/app-list.png "lista de aplicações de captura de ecrã de compreensão de idiomas (LUIS)")](media/cognitive-services-luis/app-list.png)

2. Selecione o nome no menu superior direito, em seguida, selecione **definições**.

    ![Acesso do menu de definições de utilizador do LUIS](media/cognitive-services-luis/get-user-settings-in-luis.png)

3. Copie o valor do **chave de criação de conteúdos**. Irá utilizá-lo mais tarde no início rápido. 

    [![Definições de utilizador de captura de ecrã de compreensão de idiomas (LUIS)](media/cognitive-services-luis/get-user-authoring-key.png "as definições de utilizador de captura de ecrã de compreensão de idiomas (LUIS)")](media/cognitive-services-luis/get-user-authoring-key.png)

    A chave de criação permite pedidos ilimitados gratuitos para a API de criação e até 1000 consultas ao predição ponto final de API por mês para todas as aplicações de LUIS. <!--Once the prediction endpoint quota from the authoring key is used for the month, you need to create a **Language Understanding** key from the Azure portal. The key created in the portal is known as the endpoint key. The endpoint key is used _only_ for prediction endpoint queries.-->