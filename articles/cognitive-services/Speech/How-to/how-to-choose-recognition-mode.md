---
title: Como escolher o modo de reconhecimento | Microsoft Docs
description: Como escolher o modo de reconhecimento melhor.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: 4f02b683dde16b537ae5554e6435c068f0fcb808
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352172"
---
# <a name="speech-recognition-modes"></a>Modos de reconhecimento de voz

Microsoft *reconhecimento de voz para texto* APIs suportam vários modos de reconhecimento de voz. Selecione o modo que produza os melhores resultados de reconhecimento para a sua aplicação.

| Modo | Descrição |
|---|---|
| *interativo* | Reconhecimento "Comando e controlo" para cenários de aplicação de um utilizador interativo. Os utilizadores enunciar curtos expressões que se destina como comandos para uma aplicação. |
| *ditado* | Reconhecimento contínuo para cenários de ditado. Os utilizadores enunciar mais frases que são apresentados como texto. Os utilizadores adotar um estilo falantes mais formal. |
| *conversação* | Reconhecimento contínuo para transcribing conversações entre humans. Os utilizadores adotar um estilo falantes menos formal e poderão alternativa entre mais frases e expressões mais curtos.

> [!NOTE]
> Estes modos são aplicáveis quando utiliza o [REST APIs](../GetStarted/GetStartedREST.md). O [bibliotecas de cliente](../GetStarted/GetStartedClientLibraries.md) utilizar parâmetros diferentes para especificar o modo de reconhecimento. Para obter mais informações, consulte a biblioteca de clientes à sua escolha.

Para obter mais informações, consulte o [modos de reconhecimento](../concepts.md#recognition-modes) página.
