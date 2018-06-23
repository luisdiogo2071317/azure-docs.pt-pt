---
title: Microsoft tradutor texto dinâmico de API dicionário | Microsoft Docs
description: Como utilizar a funcionalidade de dicionário dinâmica da API de texto do conversor de Microsoft.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: a18348c9786669ac41c4e149577d97cd631d5531
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355651"
---
# <a name="how-to-use-the-dynamic-dictionary-feature-of-the-microsoft-translator-text-api"></a>Como utilizar a funcionalidade de dicionário dinâmica da Microsoft tradutor texto API

Se já conhece a tradução de que pretende aplicar a uma palavra ou uma frase de acesso, pode fornecê-lo como markup dentro do pedido. O dicionário dinâmico apenas é seguro para substantivos compostos como nomes de adequado e produto. 

**Sintaxe:** 

< mstrans:dictionary tradução = "conversão de expressão" > expressão < / mstrans:dictionary >

**Exemplo: en-Alemanha:**

Origem de entrada: A palavra < mstrans:dictionary tradução =\"wordomatic\"> palavra ou expressão < / mstrans:dictionary > é uma entrada de dicionário.

Saída de destino: Das Wort "wordomatic" ist ein Wörterbucheintrag.

Esta funcionalidade funciona da mesma forma com e sem o modo HTML. 

A funcionalidade deve ser utilizada com moderação. A forma adequada e até que ponto melhor da personalização tradução é utilizando o Microsoft tradutor Hub. O Hub faz com que a utilização total de contexto e análises probabilidades. Se tiver ou pode suportar os dados de formação que mostra o seu trabalho ou frase no contexto de criar, obter muito melhores resultados. Pode encontrar mais informações sobre o hub em [ http://hub.microsofttranslator.com ](http://hub.microsofttranslator.com).

