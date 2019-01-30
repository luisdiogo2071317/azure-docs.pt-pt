---
title: Caráter contagens - API de texto do tradutor
titlesuffix: Azure Cognitive Services
description: Como a API de texto do Translator contabiliza os carateres.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 12/20/2017
ms.author: v-jansko
ms.openlocfilehash: c56622ee5e25fc422d02cec6ecfaa1f847e9e769
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55226439"
---
# <a name="how-the-translator-text-api-counts-characters"></a>Como a API de texto do Translator contabiliza os carateres

A API de texto do Translator contagens de cada caractere da entrada. Caracteres no sentido de Unicode, não em bytes. Substitutos Unicode contagem como dois caracteres. Espaço em branco e a marcação contam como carateres. O comprimento da resposta não é relevante.

Chamadas para os métodos de deteção e BreakSentence não são contabilizadas no consumo de caráter. No entanto, esperamos que as chamadas para os métodos de deteção e BreakSentence estão numa proporção razoável para a utilização de outras funções que são contadas. A Microsoft se reserva o direito de iniciar a deteção e BreakSentence de contagem.

Obter mais informações sobre as contagens de caracteres são no [FAQ do Microsoft Translator](https://www.microsoft.com/en-us/translator/faq.aspx).
