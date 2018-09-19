---
title: Contagens de caracteres de API de texto do tradutor
titlesuffix: Azure Cognitive Services
description: Como a API de texto do Translator contabiliza os carateres.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: conceptual
ms.date: 12/20/2017
ms.author: v-jansko
ms.openlocfilehash: c6234a46ae55d73739dcc23110c5e0f6375c3f96
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46128745"
---
# <a name="how-the-translator-text-api-counts-characters"></a>Como a API de texto do Translator contabiliza os carateres

A API de texto do Translator contagens de cada caractere da entrada. Caracteres no sentido de Unicode, não em bytes. Substitutos Unicode contagem como dois caracteres. Espaço em branco e a marcação contam como carateres. O comprimento da resposta não é relevante.

Chamadas para os métodos de deteção e BreakSentence não são contabilizadas no consumo de caráter. No entanto, esperamos que as chamadas para os métodos de deteção e BreakSentence estão numa proporção razoável para a utilização de outras funções que são contadas. A Microsoft se reserva o direito de iniciar a deteção e BreakSentence de contagem. 

Obter mais informações sobre as contagens de caracteres são no [FAQ do Microsoft Translator](https://www.microsoft.com/en-us/translator/faq.aspx).
