---
title: Regiões suportadas e idiomas - serviço de voz personalizada
titlesuffix: Azure Cognitive Services
description: Descrição geral dos idiomas suportados do serviço de voz personalizada nos serviços cognitivos.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: conceptual
ms.date: 02/08/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: 34af6673689244364ab3a1fe3f2a6ab056ea8598
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223960"
---
# <a name="supported-locales-in-custom-speech-service"></a>Regiões suportadas no serviço de voz personalizada
O serviço de voz personalizada atualmente suporta a personalização dos modelos nas seguintes regiões:

| Tipo de modelo | Suporte de idiomas |
|----|-----|
| Modelos acústicos | Inglês dos Estados Unidos (en-US) |
| Modelos de linguagem | Inglês dos Estados Unidos (en-US), chinês (zh-CN) |

Embora a personalização do modelo acústico só é suportada no inglês Americano, importar dados acústicos chinês é suportada para efeitos de teste offline dos modelos de idioma chinês personalizado.

Tem de selecionar a região apropriada antes de realizar qualquer ação. A região atual é indicada no título da tabela em todas as páginas de dados, modelo e implementação. Para alterar a Localidade, clique no botão "Alterar a Localidade" localizado sob o título da tabela. Isto leva-o para uma página de confirmação de localidade. Clique em "OK" para regressar à tabela.

Deve seguir com os passos seguintes
* Saiba mais [como criar um modelo acústico personalizado](cognitive-services-custom-speech-create-acoustic-model.md) para melhorar a precisão de reconhecimento
* Saiba mais [como criar um modelo de idioma personalizado](cognitive-services-custom-speech-create-language-model.md) para melhorar a taxa de reconhecimento
* Siga os [diretrizes de transcrição](cognitive-services-custom-speech-transcription-guidelines.md) para preparar seus dados
