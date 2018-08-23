---
title: Contagens de caractere de API de texto do Microsoft Translator | Documentos da Microsoft
description: Como a API de texto do Microsoft Translator contabiliza os carateres.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/20/2017
ms.author: v-jansko
ms.openlocfilehash: 1b4987509c17e4064d7c54608395e272efa8de3b
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41988145"
---
# <a name="how-the-microsoft-translator-text-api-counts-characters"></a>Como a API de texto do Microsoft Translator contabiliza os carateres

Microsoft Translator contagens de cada caractere da entrada. Caracteres no sentido de Unicode, não em bytes. Substitutos Unicode contagem como dois caracteres. Espaço em branco e a marcação contam como carateres. O comprimento da resposta não é relevante.

Chamadas para os métodos de deteção e BreakSentence não são contabilizadas no consumo de caráter. No entanto, esperamos que as chamadas para os métodos de deteção e BreakSentence estão numa proporção razoável para a utilização de outras funções que são contadas. A Microsoft se reserva o direito de iniciar a deteção e BreakSentence de contagem. 

Obter mais informações sobre as contagens de caracteres são no [FAQ do Microsoft Translator](https://www.microsoft.com/en-us/translator/faq.aspx).
