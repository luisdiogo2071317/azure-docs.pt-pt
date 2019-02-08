---
title: Suporte a localidades e idiomas no serviço de voz personalizada no Azure | Documentos da Microsoft
description: Descrição geral dos idiomas suportados do serviço de voz personalizada nos serviços cognitivos.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: nitinme
ms.openlocfilehash: c40cf951e59094470443d7244052e6214aaeba82
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55864269"
---
# <a name="supported-locales-in-custom-speech-service"></a>Regiões suportadas no serviço de voz personalizada

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

O serviço de voz personalizada atualmente suporta a personalização dos modelos nas seguintes regiões:

| Tipo de modelo | Suporte de Idiomas |
|----|-----|
| Modelos acústicos | Inglês dos Estados Unidos (en-US) |
| Modelos de linguagem | Inglês dos Estados Unidos (en-US), chinês (zh-CN) |

Embora a personalização do modelo acústico só é suportada no inglês Americano, importar dados acústicos chinês é suportada para efeitos de teste offline dos modelos de idioma chinês personalizado.

Tem de selecionar a região apropriada antes de realizar qualquer ação. A região atual é indicada no título da tabela em todas as páginas de dados, modelo e implementação. Para alterar a Localidade, clique no botão "Alterar a Localidade" localizado sob o título da tabela. Isto leva-o para uma página de confirmação de localidade. Clique em "OK" para regressar à tabela.

Deve seguir com os passos seguintes
* Saiba mais [como criar um modelo acústico personalizado](cognitive-services-custom-speech-create-acoustic-model.md) para melhorar a precisão de reconhecimento
* Saiba mais [como criar um modelo de idioma personalizado](cognitive-services-custom-speech-create-language-model.md) para melhorar a taxa de reconhecimento
* Siga os [diretrizes de transcrição](cognitive-services-custom-speech-transcription-guidelines.md) para preparar seus dados
