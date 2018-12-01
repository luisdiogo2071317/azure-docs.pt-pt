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
ms.openlocfilehash: 0b1187083c14fc7c536f6a32f3a41957f53f299b
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/30/2018
ms.locfileid: "52679720"
---
# <a name="language-and-region-support-for-the-translator-text-api"></a>Suporte de idioma e região para a API de texto do tradutor

A API de texto do Translator suporta os seguintes idiomas para a tradução de texto para texto. Tradução automática neuronal (NMT) é o novo padrão para conversões de máquina de com tecnologia de ia de alta qualidade e está disponível como sendo o padrão usando o V3 da API de texto do Translator quando um sistema neural disponível. 

[Saiba mais sobre como funciona a tradução automática](https://www.microsoft.com/translator/mt.aspx)

**V2 Translator API**

> [!NOTE]
> V2 foi preterido no dia 30 de Abril de 2018 e será descontinuada a 30 de Abril de 2019.

* Apenas estatística: nenhum sistema neural está disponível para este idioma.
* Disponível Neural: um sistema neural está disponível. Utilize o parâmetro `category=generalnn` para acessar o sistema neural.
* Predefinição neural: Neural é o sistema de tradução de predefinição. Utilize o parâmetro `category=smt` para aceder ao sistema de estatístico para utilização com o Microsoft Translator Hub.
* Apenas Neural: só de tradução neural está disponível.

**API do Microsoft Translator v3** a API do Microsoft Translator V3 é neural por predefinição e sistemas estatísticos só estão disponíveis quando nenhum sistema neural existe. Tradutor personalizado só pode ser utilizado com linguagens neurais. 

|Idioma|  Código do idioma|  V2 API| V3 API|
|:-----|:-----:|:-----|:-----|
|Afrikaans| `af`    |Apenas estatística|  Neural|
|Árabe|    `ar`    |Disponível Neural|  Neural|
|Árabe, Levantine| `apc`   |Disponível Neural|  Neural|
|Bangla|    `bn`    |Disponível Neural|  Neural|
|Bósnio (Latim)|   `bs`    |Apenas estatística|  Estatísticas|
|Búlgaro| `bg`    |Disponível Neural|  Neural|
|Cantonês (tradicional)|   `yue`   |Apenas estatística|  Estatísticas|
|Catalão|   `ca`    |Apenas estatística|  Estatísticas|
|Chinês simplificado|    `zh-Hans`   |Predefinição neural |Neural|
|Chinês tradicional|   `zh-Hant`   |Predefinição neural |Neural|
|Croata|  `hr`    |Disponível Neural|  Neural|
|Checo| `cs`    |Disponível Neural|  Neural|
|Dinamarquês|    `da`    |Disponível Neural   |Neural|
|Neerlandês| `nl`    |Disponível Neural|  Neural|
|Português|   `en`    |Disponível Neural|  Neural|
|Estónio|  `et`    |Disponível Neural|  Neural|
|Fijian|    `fj`    |Apenas estatística|  Estatísticas|
|Filipino|  `fil`   |Apenas estatística|  Estatísticas|
|Finlandês|   `fi`    |Disponível Neural|  Neural|
|Francês|    `fr`    |Disponível Neural|  Neural|
|Alemão|    `de`    |Disponível Neural|  Neural|
|Grego| `el`    |Disponível Neural|  Neural|
|Haitian Creole|    `ht`    |Apenas estatística   |Estatísticas|
|Hebraico |`he`   |Disponível Neural   |Neural|
|Hindi| `hi`    |Predefinição neural|    Neural|
|Hmong Daw| `mww`   |Apenas estatística|  Estatísticas|
|Húngaro| `hu`    |Disponível Neural|  Neural|
|Islandês| `is`    |Apenas Neural|   Neural|
|Indonésio|    `id`    |Apenas estatística|  Estatísticas|
|Italiano|   `it`    |Disponível Neural|  Neural|
|Japonês|  `ja`    |Disponível Neural|  Neural|
|Suaíli| `sw`    |Apenas estatística|  Estatísticas|
|Klingon|   `tlh`   |Apenas estatística|  Estatísticas|
|Klingon (plqaD)|   `tlh-Qaak`  |Apenas estatística|  Estatísticas|
|Coreano |`ko`   |Disponível Neural|  Neural|
|Letão|   `lv`    |Disponível Neural|  Neural|
|Lituano|    `lt`    |Disponível Neural|  Neural|
|Ariary|  `mg`    |Apenas estatística|  Estatísticas|
|Malaio| `ms`    |Apenas estatística   |Estatísticas|
|Maltês|   `mt`    |Apenas estatística|  Estatísticas|
|Norueguês| `nb`    |Disponível Neural|  Neural|
|Persa|   `fa`    |Apenas estatística|  Estatísticas|
|Polaco|    `pl`    |Disponível Neural|  Neural|
|Português|    `pt`    |Disponível Neural|  Neural|
|Queretaro Otomi|   `otq`   |Apenas estatística|  Estatísticas|
|Romeno|  `ro`    |Disponível Neural|  Neural|
|Russo|   `ru`    |Disponível Neural|  Neural|
|Samoan|    `sm`    |Apenas estatística|  Estatísticas|
|Sérvio (Cirílico)|    `sr-Cyrl`   |Apenas estatística|  Estatísticas|
|Sérvio (Latim)|   `sr-Latn`   |Apenas estatística   |Estatísticas|
|Eslovaco|    `sk`    |Disponível Neural|  Neural|
|Esloveno| `sl`    |Disponível Neural|  Neural|
|Espanhol|   `es`    |Disponível Neural|  Neural|
|Sueco|   `sv`    |Disponível Neural   |Neural|
|Tahitian|  `ty`    |Apenas estatística|  Estatísticas|
|Tamil| `ta`    |Apenas estatística|  Estatísticas|
|Télego|    `te`    |Apenas Neural|   Neural|
|Tailandês|  `th`    |Disponível Neural|  Neural|
|Tongan|    `to`    |Apenas estatística|  Estatísticas|
|Turco|   `tr`    |Disponível Neural   |Neural|
|Ucraniano| `uk`    |Disponível Neural|  Neural|
|Urdu|  `ur`    |Apenas estatística|  Estatísticas|
|Vietnamita|    `vi`    |Disponível Neural|  Neural|
|Galês| `cy`    |Disponível Neural|  Neural|
|Yucatec Maya|  `yua`   |Apenas estatística|  Estatísticas|

## <a name="transliteration"></a>Transliteração

O método Transliterate suporta os seguintes idiomas. Na "para/de", "<> –" indica que o idioma pode ser Transliterado de ou para qualquer um dos scripts listados. O "-->" indica que a linguagem pode apenas transliterada a partir de um script para outro.

| Idioma    | Código do idioma | Script | Em | Script|
|:----------- |:-------------:|:-------------:|:-------------:|:-------------:|
| Árabe | ar | Árabe | <--> | Latim |
|Bangla  | Bn | Bengali | <--> | Latim |
| Chinês (Simplificado) | zh Hans | Chinês simplificado | <--> | Latim |
| Chinês (Simplificado) | zh Hans | Chinês simplificado | <--> | Chinês tradicional |
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
| Suaíli      | `sw`          |
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
