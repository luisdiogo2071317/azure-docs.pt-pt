---
title: Dicionário de dinâmica de API de texto do Microsoft Translator | Documentos da Microsoft
description: Como utilizar a funcionalidade de dicionário dinâmica da API de texto do Microsoft Translator.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: dbc754093827730b8709d67f314e5b327518ef50
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/11/2018
ms.locfileid: "41987532"
---
# <a name="how-to-use-the-dynamic-dictionary-feature-of-the-microsoft-translator-text-api"></a>Como utilizar a funcionalidade de dicionário dinâmica da API de texto do Microsoft Translator

Se já sabe a tradução que pretende aplicar a uma palavra ou frase, pode fornecer como marcação dentro do pedido. O dicionário dinâmico só é seguro para substantivos compostos, como nomes próprios e nomes de produtos. 

**Sintaxe:** 

< a tradução de mstrans:dictionary = "tradução de frase" > frase < / mstrans:dictionary >

**Exemplo: en-de:**

Entrada de origem: A palavra < mstrans:dictionary tradução =\"wordomatic\"> palavra ou frase < / mstrans:dictionary > é uma entrada de dicionário.

Saída de destino: Das Wort "wordomatic" ist ein Wörterbucheintrag.

Esta funcionalidade funciona da mesma forma com e sem modo HTML. 

O recurso deve ser usado com moderação. A forma adequada e muito melhor de personalização de tradução é com o Translator personalizado. Tradutor personalizado utiliza completa de contexto e probabilidades de estatísticas. Se tiver ou pode criar dados de treinamento que mostra a sua empresa ou frase no contexto, obtém resultados muito melhores. Pode encontrar mais informações sobre o Translator personalizado na [ http://aka.ms/CustomTranslator ](http://aka.ms/CustomTranslator).

