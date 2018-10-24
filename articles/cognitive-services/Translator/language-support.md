---
title: Suporte de idiomas - API de texto do tradutor
titleSuffix: Azure Cognitive Services
description: Uma lista de idiomas suportados pela API de texto do Translator.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 09/25/2018
ms.author: v-jansko
ms.openlocfilehash: 222e37e38772b82e9d9849e3a955b865d43d3c63
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49957404"
---
# <a name="language-and-region-support-for-the-translator-text-api"></a>Suporte de idioma e região para a API de texto do tradutor

A API de texto do Translator suporta os seguintes idiomas para a tradução de texto para texto. Tradução automática neuronal (NMT) é o novo padrão para conversões de máquina de com tecnologia de ia de alta qualidade e está disponível como sendo o padrão usando o V3 da API de texto do Translator quando um sistema neural disponível. Tradução automática neural está disponível no V2, utilizando a categoria "generalnn".

[Saiba mais sobre como funciona a tradução automática](https://www.microsoft.com/translator/mt.aspx)

| Idioma    | Tipo de tradução |Código do idioma |
|:----------- |:-------:|:-------------:|
| Afrikaans      | Estatísticas |`af`          |
| Árabe      | Neural | `ar`          |
| Árabe, Levantine    | Neural | `apc`
| Bangla      | Neural |`bn`          |
| Bósnio (Latim)      | Estatísticas |`bs`          |
| Búlgaro     |  Neural |`bg`          |
| Cantonês (tradicional)      | Estatísticas |`yue`          |
| Catalão      | Estatísticas |`ca`          |
| Chinês simplificado        |  Neural |`zh-Hans`          |
| Chinês tradicional        |  Neural |`zh-Hant`          |
| Croata      | Neural |`hr`          |
| Checo        |  Neural |`cs`          |
| Dinamarquês        |  Neural |`da`          |
| Neerlandês        |  Neural |`nl`          |
| Português       |  Neural |`en`          |
| Estónio      | Neural |`et`          |
| Fijian      | Estatísticas |`fj`          |
| Filipino      | Estatísticas |`fil`          |
| Finlandês      | Neural |`fi`          |
| Francês        |  Neural |`fr`          |
| Alemão       |  Neural |`de`          |
| Grego      | Neural |`el`          |
| Haitian Creole      | Estatísticas |`ht`          |
| Hebraico      | Neural |`he`          |
| Hindi        |  Neural |`hi`          |
| Hmong Daw      | Estatísticas |`mww`          |
| Húngaro      | Neural |`hu`          |
| Islandês      |  Neural |`is`           |
| Indonésio      | Estatísticas |`id`          |
| Italiano        |  Neural |`it`          |
| Japonês        |  Neural |`ja`          |
| Kiswahili      | Estatísticas |`sw`          |
| Klingon      | Estatísticas |`tlh`          |
| Klingon (plqaD)      | Estatísticas |`tlh-Qaak`          |
| Coreano        |  Neural |`ko`          |
| Letão      | Neural |`lv`          |
| Lituano      | Neural |`lt`          |
| Ariary      | Estatísticas |`mg`          |
| Malaio      | Estatísticas |`ms`          |
| Maltês      | Estatísticas |`mt`          |
| Norueguês        |  Neural |`nb`          |
| Persa      | Estatísticas |`fa`          |
| Polaco        |  Neural |`pl`          |
| Português        |  Neural |`pt`          |
| Queretaro Otomi      | Estatísticas |`otq`          |
| Romeno        |  Neural |`ro`          |
| Russo        |  Neural |`ru`          |
| Samoan      | Estatísticas |`sm`          |
| Sérvio (Cirílico)      | Estatísticas |`sr-Cyrl`          |
| Sérvio (Latim)      | Estatísticas |`sr-Latn`          |
| Eslovaco     | Neural |`sk`          |
| Esloveno      | Neural |`sl`          |
| Espanhol        |  Neural |`es`          |
| Sueco        |  Neural |`sv`          |
| Tahitian      | Estatísticas |`ty`          |
| Tamil      | Estatísticas |`ta`          |
| Télego   | Neural   | `te` |
| Tailandês      | Neural |`th`          |
| Tongan      | Estatísticas |`to`          |
| Turco       |  Neural |`tr`          |
| Ucraniano      | Neural |`uk`          |
| Urdu      | Estatísticas |`ur`          |
| Vietnamita      | Neural |`vi`          |
| Galês      | Neural |`cy`          |
| Yucatec Maya      | Estatísticas |`yua`          |

## <a name="transliteration"></a>Transliteração

O método Transliterate suporta os seguintes idiomas. Na "para/de", "<> –" indica que o idioma pode ser Transliterado de ou para qualquer um dos scripts listados. O "-->" indica que a linguagem pode apenas transliterada a partir de um script para outro.

| Idioma    | Código do idioma | Script | Em | Script|
|:----------- |:-------------:|:-------------:|:-------------:|:-------------:|
| Árabe | ar | Árabe | <--> | Latim |
|Bangla  | Bn | Bengali | <--> | Latim |
| Chinês (Simplificado) | zh-Hans | Chinês simplificado | <--> | Latim |
| Chinês (Simplificado) | zh-Hans | Chinês simplificado | <--> | Chinês tradicional |
| Chinês (Tradicional) | zh-Hant | Chinês tradicional | <--> | Latim |
| Chinês (Tradicional) | zh-Hant | Chinês tradicional | <--> | Chinês simplificado |
| Guzarate | gu  | Guzarate | --> | Latim |
| Hebraico | ele | Hebraico | <--> | Latim |
| Hindi | Olá! | Devanágari | <--> | Latim |
| Japonês | ja | Japonês | <--> | Latim |
| Canarim | kN | Canarim | --> | Latim |
| Malayalam | ml | Malayalam | --> | Latim |
| Marata | MR | Devanágari | --> | Latim |
| Odia | ou | Odia | <--> | Latim |
| Punjabi | Pa | Gurmuqui | <--> | Latim  |
| Sérvio (Cirílico) | SR-Cyrl | Cirílico  | --> | Latim |
| Sérvio (Latim) | SR-Latn | Latim | --> | Cirílico |
| Tamil | es | Tamil | --> | Latim |
| Télego | te | Télego | --> | Latim |
| Tailandês | º | Tailandês | <--> | Latim |

## <a name="dictionary"></a>Dicionário

O dicionário suporta os seguintes idiomas de ou para inglês usando os métodos de pesquisa e exemplos.

| Idioma    | Código do idioma |
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

Os seguintes idiomas podem ser detetados pelo método de deteção. Detetar Maio detetar idiomas que não é possível converter o Microsoft Translator.

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
| Chinês (Simplificado) |
| Chinês (Tradicional) |
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
| Haitian Creole |
| Hebraico |
| Hindi |
| Húngaro |
| Islandês |
| Indonésio |
| Irlandês |
| Italiano |
| Japonês |
| Coreano |
| Curdo (Árabe) |
| Curdo (Latim) |
| Latim |
| Letão |
| Lituano |
| Macedónio |
| Malaio |
| Maltês |
| Norueguês |
| Norueguês (Nynorsk) |
| Pastó |
| Persa |
| Polaco |
| Português |
| Romeno |
| Russo |
| Sérvio (Cirílico) |
| Sérvio (Latim) |
| Eslovaco |
| Esloveno |
| Somali |
| Espanhol |
| kiswahili |
| Sueco |
| Tagalog |
| Télego |
| Tailandês |
| Turco |
| Ucraniano |
| Urdu |
| Usbeque (Cirílico) |
| Usbeque (Latim) |
| Vietnamita |
| Galês |
| Ídiche |

## <a name="access-the-list-programmatically"></a>Aceder à lista por meio de programação

Pode acessar a lista de idiomas suportados programaticamente usando a operação de idiomas da API de texto V3.0. Pode ver a lista por funcionalidade, o código de idioma, bem como o nome do idioma em inglês ou qualquer outro idioma suportado. Esta lista é atualizada automaticamente pelo serviço do Microsoft Translator à medida que novos idiomas forem disponíveis.

[Ver documentação de referência de operação de idiomas](reference/v3-0-languages.md)

## <a name="access-the-list-on-the-microsoft-translator-website"></a>Aceder à lista no site do Microsoft Translator

Para examinar os idiomas, o site do Microsoft Translator mostra todos os idiomas suportados pelo texto de tradutor e APIs de voz. Esta lista não inclui informações específicas de desenvolvedor como códigos de idioma.

[Consulte a lista de idiomas](https://www.microsoft.com/translator/languages.aspx)
