---
title: Caráter contagens - API de texto do tradutor
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
ms.openlocfilehash: 8e04158d34765b8a077fc56f2108ea6d7b4b03a6
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49649199"
---
# <a name="how-the-translator-text-api-counts-characters"></a>Como a API de texto do Translator contabiliza os carateres

A API de texto do Translator contagens de cada caractere da entrada. Caracteres no sentido de Unicode, não em bytes. Substitutos Unicode contagem como dois caracteres. Espaço em branco e a marcação contam como carateres. O comprimento da resposta não é relevante.

Chamadas para os métodos de deteção e BreakSentence não são contabilizadas no consumo de caráter. No entanto, esperamos que as chamadas para os métodos de deteção e BreakSentence estão numa proporção razoável para a utilização de outras funções que são contadas. A Microsoft se reserva o direito de iniciar a deteção e BreakSentence de contagem.

Obter mais informações sobre as contagens de caracteres são no [FAQ do Microsoft Translator](https://www.microsoft.com/en-us/translator/faq.aspx).
