---
title: incluir ficheiro
description: incluir ficheiro
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 08/16/2018
ms.author: diberry
ms.openlocfilehash: 14678a143e1b14b9b0b89435f356ac5df98ef40c
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55478688"
---
O acesso ao ponto final de predição é fornecido com uma chave de ponto final. Para efeitos deste guia de início rápido, utilize a chave de iniciante gratuita associada à conta do LUIS. 
 
1. Inicie sessão com a conta do LUIS. 

    [![Captura de ecrã a mostrar a lista de aplicações do Language Understanding (LUIS)](media/cognitive-services-luis/app-list.png "Captura de ecrã a mostrar a lista de aplicações do Language Understanding (LUIS)")](media/cognitive-services-luis/app-list.png)

2. Selecione o seu nome no menu superior direito e, em seguida, selecione **Definições**.

    ![Acesso ao menu de definições de utilizador do LUIS](media/cognitive-services-luis/get-user-settings-in-luis.png)

3. Copie o valor da **Chave de criação**. Iremos utilizá-la mais tarde no guia de início rápido. 

    [![Captura de ecrã a mostrar as definições de utilizador do Language Understanding (LUIS)](media/cognitive-services-luis/get-user-authoring-key.png "Captura de ecrã a mostrar as definições de utilizador do Language Understanding (LUIS)")](media/cognitive-services-luis/get-user-authoring-key.png)

    A chave de criação permite pedidos ilimitados gratuitos à API de criação e até 1000 consultas à API de ponto final de predição por mês para todas as aplicações LUIS. <!--Once the prediction endpoint quota from the authoring key is used for the month, you need to create a **Language Understanding** key from the Azure portal. The key created in the portal is known as the endpoint key. The endpoint key is used _only_ for prediction endpoint queries.-->
