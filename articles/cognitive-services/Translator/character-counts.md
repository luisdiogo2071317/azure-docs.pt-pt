---
title: Caráter de API do Microsoft tradutor texto contagens | Microsoft Docs
description: Como a API de texto do Microsoft tradutor contagens de carateres.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/20/2017
ms.author: v-jansko
ms.openlocfilehash: ebe3e3606a0413730e1fbfd704a6403f77275f89
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352585"
---
# <a name="how-the-microsoft-translator-text-api-counts-characters"></a>Como a API de texto do Microsoft tradutor contagens de carateres

Microsoft Translator contagens cada caráter da entrada. Carateres no sentido de Unicode, não em bytes. Substitutos Unicode contagem como dois carateres. Espaço em branco e markup contam como carateres. O comprimento da resposta é relevante.

Chamadas para os métodos de deteção e BreakSentence não são contabilizadas no consumo de caráter. No entanto, esperamos que as chamadas para os métodos de deteção e BreakSentence estão a ser uma proporção razoável para a utilização de outras funções que são contados. Microsoft reserva-se o direito de iniciar a detetar e BreakSentence de contagem. 

Converte a deteção automática ofertas se omitir o parâmetro de idioma From. 

Obter mais informações sobre as contagens de caráter estão no [FAQ do Microsoft tradutor](https://www.microsoft.com/en-us/translator/faq.aspx).
