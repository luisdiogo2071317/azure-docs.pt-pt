---
title: Regiões do serviço de reconhecimento de voz | Microsoft Docs
description: Referência para regiões do serviço de reconhecimento de voz.
services: cognitive-services
author: mahilleb-msft
manager: wolmfa61
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 06/27/2018
ms.author: mahilleb
ms.openlocfilehash: a201cc043f673e2285ea48950804d97b96f881ed
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37055007"
---
# <a name="regions-and-endpoints-of-the-speech-service"></a>Regiões e os pontos finais do serviço de reconhecimento de voz

> [!NOTE]
> Os nomes de região no [SDK de reconhecimento de voz](speech-sdk.md) corresponde à primeira parte do domínio dos pontos finais indicados abaixo.
> Por exemplo, utilizar `westus` para especificar a região EUA oeste no SDK do reconhecimento de voz.

## <a name="speech-to-text"></a>Conversão de Voz em Texto

[!include[](includes/endpoints-speech-to-text.md)]

## <a name="text-to-speech"></a>Conversão de Texto em Voz

[!include[](includes/endpoints-text-to-speech.md)]

## <a name="authentication"></a>Autenticação

[!include[](includes/endpoints-token-service.md)]

Consulte [aqui](rest-apis.md#authentication) para detalhes de obtenção e atualização de tokens de autorização.

## <a name="language-understanding-speech-sdk-only"></a>Compreensão de idiomas (apenas SDK de reconhecimento de voz)

São listadas regiões para o serviço de compreensão de idiomas [aqui](/azure/cognitive-services/luis/luis-reference-regions).
O SDK de reconhecimento de voz, especificar estas regiões, a primeira parte do nome de domínio do ponto final (por exemplo, `westus`).
