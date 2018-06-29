---
title: Idiomas suportados na Microsoft tradutor API | Microsoft Docs
description: Ver os idiomas suportados pela Microsoft tradutor texto API.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: v-jansko
ms.openlocfilehash: 5542c1c0eee5e4c7c85c566908dc8999b118f06e
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37032455"
---
# <a name="supported-languages-in-the-microsoft-translator-text-api"></a>Idiomas suportados na Microsoft tradutor texto API

A API de texto do Microsoft tradutor suporta os seguintes idiomas para a conversão de texto para texto. Tradução automática (NMT neuronal) é o novo padrão para alta qualidade com tecnologia AI máquina traduções e está disponível como predefinição V3 da API de texto tradutor a utilizar quando um sistema neuronal está disponível. Tradução neuronal está disponível na V2 utilizando a categoria "generalnn".

[Saiba mais sobre como funciona a tradução automática](https://www.microsoft.com/translator/mt.aspx)

| Idioma    | Tipo de conversão |Código de idioma |
|:----------- |:-------:|:-------------:|
| Afrikaans      | Estatísticas |`af`          |
| Árabe      | Neuronal | `ar`          |
| Árabe, Levantine    | Neuronal | `apc`
| Bangla      | Neuronal |`bn`          |
| Bósnio (Latim)      | Estatísticas |`bs`          |
| Búlgaro     |  Neuronal |`bg`          |
| Cantonês (tradicional)      | Estatísticas |`yue`          |
| Catalão      | Estatísticas |`ca`          |
| Chinês simplificado        |  Neuronal |`zh-Hans`          | 
| Chinês tradicional        |  Neuronal |`zh-Hant`          |
| Croata      | Neuronal |`hr`          |
| Checo        |  Neuronal |`cs`          |
| Dinamarquês        |  Neuronal |`da`          |
| Neerlandês        |  Neuronal |`nl`          |
| Português       |  Neuronal |`en`          |
| Estónio      | Neuronal |`et`          |
| Fijian      | Estatísticas |`fj`          |
| Filipino      | Estatísticas |`fil`          |
| Finlandês      | Neuronal |`fi`          |
| Francês        |  Neuronal |`fr`          |
| Alemão       |  Neuronal |`de`          |
| Grego      | Neuronal |`el`          |
| Haitian Creole      | Estatísticas |`ht`          |
| Hebraico      | Neuronal |`he`          |
| Hindi        |  Neuronal |`hi`          |
| Hmong Daw      | Estatísticas |`mww`          |
| Húngaro      | Neuronal |`hu`          |
| Islandês      |  Neuronal |`is`           |
| Indonésio      | Estatísticas |`id`          |
| Italiano        |  Neuronal |`it`          |
| Japonês        |  Neuronal |`ja`          |
| Kiswahili      | Estatísticas |`sw`          |
| Klingon      | Estatísticas |`tlh`          |
| Klingon (plqaD)      | Estatísticas |`tlh-Qaak`          |
| Coreano        |  Neuronal |`ko`          |
| Letão      | Neuronal |`lv`          |
| Lituano      | Neuronal |`lt`          |
| Malagasy      | Estatísticas |`mg`          |
| Malaio      | Estatísticas |`ms`          |
| Maltês      | Estatísticas |`mt`          |
| Norueguês        |  Neuronal |`nb`          |
| Persa      | Estatísticas |`fa`          |
| Polaco        |  Neuronal |`pl`          |
| Português        |  Neuronal |`pt`          |
| Queretaro Otomi      | Estatísticas |`otq`          |
| Romeno        |  Neuronal |`ro`          |
| Russo        |  Neuronal |`ru`          |
| Samoan      | Estatísticas |`sm`          |
| Sérvio (Cirílico)      | Estatísticas |`sr-Cyrl`          |
| Sérvio (Latim)      | Estatísticas |`sr-Latn`          |
| Eslovaco     | Neuronal |`sk`          |
| Esloveno      | Neuronal |`sl`          |
| Espanhol        |  Neuronal |`es`          |
| Sueco        |  Neuronal |`sv`          |
| Tahitian      | Estatísticas |`ty`          |
| Tamil      | Estatísticas |`ta`          |
| Tailandês      | Neuronal |`th`          |
| Tongan      | Estatísticas |`to`          |
| Turco       |  Neuronal |`tr`          |
| Ucraniano      | Neuronal |`uk`          |
| Urdu      | Estatísticas |`ur`          |
| Vietnamita      | Neuronal |`vi`          |
| Galês      | Neuronal |`cy`          |
| Yucatec Maya      | Estatísticas |`yua`          |

## <a name="transliteration"></a>Transliteração

O método Transliterate suporta os seguintes idiomas. Na "para/de", "<>-de-" indica que pode ser transliterated o idioma de ou para qualquer um dos scripts listados. O "-->" indica que o idioma só pode ser transliterated a partir de um script, para outro.

| Idioma    | Código de idioma | Script | Ao/do | Script|
|:----------- |:-------------:|:-------------:|:-------------:|:-------------:|
| Árabe | ar | Árabe | <--> | Latina |
|Bangla  | Bn | Bengali | <--> | Latina |
| Chinês (Simplificado) | zh-Hans | Chinês simplificado | <--> | Latina |
| Chinês (Simplificado) | zh-Hans | Chinês simplificado | <--> | Chinês tradicional |
| Chinês (Tradicional) | zh-Hant | Chinês tradicional | <--> | Latina |
| Chinês (Tradicional) | zh-Hant | Chinês tradicional | <--> | Chinês simplificado |
| Guzarate | gu  | Guzarate | --> | Latina |
| Hebraico | ele | Hebraico | <--> | Latina |
| Hindi | Olá | Devanagari | <--> | Latina |
| Japonês | ja | Japonês | <--> | Latina |
| Canarim | kN | Canarim | --> | Latina |
| Malasian | ml | Malayalam | --> | Latina |
| Marati | MR | Devanagari | --> | Latina |
| Odia | ou | Odia | <--> | Latina |
| Punjabi | Pa | Gurmukhi | <--> | Latina  |
| Sérvio (Cirílico) | SR-Cyrl | Carateres cirílicos  | --> | Latina |
| Sérvio (Latim) | SR-Latn | Latina | --> | Carateres cirílicos |
| Tamil | dados | Tamil | --> | Latina |
| Télego | te | Télego | --> | Latina |
| Tailandês | º | Tailandês | <--> | Latina |

## <a name="dictionary"></a>Dicionário

O dicionário suporta os seguintes idiomas de ou para inglês utilizando os métodos de pesquisa e exemplos.

| Idioma    | Código de idioma |
|:----------- |:-------------:|
| Afrikaans      | `af`          |
| Árabe       | `ar`          |
| Bangla      | `bn`          |
| Bósnio (Latim)      | `bs`          |
| Búlgaro      | `bg`          |
| Catalão      | `ca`          |
| Chinês simplificado      | `zh-Hans`          | 
| Croata      | `hr`          |
| Checo      | `cs`          |
| Dinamarquês      | `da`          |
| Neerlandês      | `nl`          |
| Estónio      | `et`          |
| Finlandês      | `fi`          |
| Francês      | `fr`          |
| Alemão      | `de`          |
| Grego      | `el`          |
| Haitian Creole      | `ht`          |
| Hebraico      | `he`          |
| Hindi      | `hi`          |
| Hmong Daw      | `mww`          |
| Húngaro      | `hu`          |
| Islandês    | `is`  |
| Indonésio      | `id`          |
| Italiano      | `it`          |
| Japonês      | `ja`          |
| Kiswahili      | `sw`          |
| Klingon      | `tlh`          |
| Coreano      | `ko`          |
| Letão      | `lv`          |
| Lituano      | `lt`          |
| Malaio      | `ms`          |
| Maltês      | `mt`          |
| Norueguês      | `nb`          |
| Persa      | `fa`          |
| Polaco      | `pl`          |
| Português      | `pt`          |
| Romeno      | `ro`          |
| Russo      | `ru`          |
| Sérvio (Latim)      | `sr-Latn`          |
| Eslovaco     | `sk`          |
| Esloveno      | `sl`          |
| Espanhol      | `es`          |
| Sueco      | `sv`          |
| Tamil      | `ta`          |
| Tailandês      | `th`          |
| Turco      | `tr`          |
| Ucraniano      | `uk`          |
| Urdu      | `ur`          |
| Vietnamita      | `vi`          |
| Galês      | `cy`          |

## <a name="languages-detected-by-the-detect-method"></a>Idiomas detetados pelo método de deteção

Os seguintes idiomas podem ser detetados pelo método de deteção. Detetar pode detetar os idiomas que não é possível converter Translator Microsoft.

| Idioma    |
|:----------- |
| Afrikaans |
| Albanês |
| Árabe |
| Basco (Basco) |
| Bielorrusso |
| Búlgaro |
| Catalão |
| Chinês |
| Chinese_Simplified |
| Chinese_Traditional |
| Croata |
| Checo |
| Dinamarquês |
| Neerlandês |
| Português |
| Esperanto |
| Estónio |
| Finlandês |
| Francês |
| Galego |
| Alemão |
| Grego |
| Haitian_Creole |
| Hebraico |
| Hindi |
| Húngaro |
| Islandês |
| Indonésio |
| Irlandês |
| Italiano |
| Japonês |
| Coreano |
| Kurdish_Arabic |
| Kurdish_Latin |
| Latina |
| Letão |
| Lituano |
| Macedónio |
| Malaio |
| Maltês |
| Norueguês |
| Norwegian_Nynorsk |
| Pastó |
| Persa |
| Polaco |
| Português |
| Romeno |
| Russo |
| Serbian_Cyrillic |
| Serbian_Latin |
| Eslovaco |
| Esloveno |
| Somali |
| Espanhol |
| kiswahili |
| Sueco |
| Tagalog |
| Tailandês |
| Turco |
| Ucraniano |
| Urdu |
| Uzbek_Cyrillic |
| Uzbek_Latin |
| Vietnamita |
| Galês |
| Yiddish |

## <a name="access-the-list-programmatically"></a>A lista de acesso através de programação

Pode aceder a lista de linguagens de programação utilizando a operação de idiomas da API de texto v 3.0. Pode ver a lista por funcionalidade, o código de idioma, bem como o nome de idioma em inglês ou qualquer outro idioma suportado. Esta lista é atualizada automaticamente pelo serviço Microsoft Translator à novos idiomas ficam disponíveis.

[Ver a documentação de referência de operação de idiomas](reference/v3-0-languages.md)

## <a name="access-the-list-on-the-microsoft-translator-website"></a>Aceder à lista de no Web site da Microsoft Translator

Para ver rapidamente os idiomas, o Web site Microsoft Translator mostra todos os idiomas suportados pelo tradutor Text e APIs de reconhecimento de voz. Esta lista não incluem informações específicas do programador, tais como os códigos de idioma.

[Consulte a lista de idiomas](https://www.microsoft.com/translator/languages.aspx)
