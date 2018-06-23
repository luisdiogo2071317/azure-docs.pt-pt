---
title: Suportado regiões e idiomas no serviço de reconhecimento de voz personalizadas no Azure | Microsoft Docs
description: Descrição geral dos idiomas suportados do serviço de reconhecimento de voz personalizadas nos serviços cognitivos.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: c378fd951f9cd04884f44fbec5accb5d9a886bfe
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351404"
---
# <a name="supported-locales-in-custom-speech-service"></a>Regiões suportadas no serviço de reconhecimento de voz personalizadas
O serviço de reconhecimento de voz personalizadas suporta atualmente personalização dos modelos nas seguintes regiões:

| Tipo de modelo | Suporte de idiomas |
|----|-----|
| Modelos acústica | Inglês dos Estados Unidos (en-US) |
| Modelos de idioma | E.u. a inglês (en-US), chinês (zh-CN) |

Apesar de personalização de modelo acústica só é suportada em inglês, importar dados acústica chinês é suportada para efeitos de teste offline dos modelos de idioma chinês personalizado.

Tem de selecionar a região adequada antes de efetuar qualquer ação. A região atual é indicada no título de tabela em todos os dados, o modelo e páginas de implementação. Para alterar a região, clique no botão "Alterar região", localizado sob o título da tabela. Isto leva-o para uma página de confirmação da região. Clique em "OK" para regressar à tabela.

Deve seguir a cópia de segurança com os passos seguintes
* Saiba [como criar um modelo personalizado acústica](cognitive-services-custom-speech-create-acoustic-model.md) para melhorar a precisão do reconhecimento
* Saiba [como criar um modelo de idioma personalizado](cognitive-services-custom-speech-create-language-model.md) para melhorar a taxa de reconhecimento
* Siga o [diretrizes transcription](cognitive-services-custom-speech-transcription-guidelines.md) para preparar os dados
