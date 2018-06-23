---
title: Microsoft tradutor texto API - migrar v3 | Microsoft Docs
description: Saiba como migrar a partir de V2 v3 da API de texto tradutor.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: translator
ms.topic: article
ms.date: 03/27/2018
ms.author: v-jansko
ms.openlocfilehash: 16fec351af5b5b3875657ee244c18f305311d965
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354985"
---
# <a name="microsoft-translator-text-api-v2-to-v3-migration"></a>Microsoft tradutor texto API V2 para a migração V3

A equipa de Translator Microsoft lançou a versão 3 (V3) da API de conversão de texto. Esta versão inclui novas funcionalidades, métodos preteridos e um novo formato para enviar para e receber dados do Microsoft tradutor Service. Este documento fornece informações para alterar as aplicações utilizem V3. V2 será preterido no 30 de Abril de 2018 e serão descontinuados no 30 de Abril de 2019.

O fim deste documento contém ligações úteis para si obter mais informações.

## <a name="summary-of-features"></a>Resumo das funcionalidades

* Nenhum rastreio - V3 não-rastreio aplica-se a todos os escalões de preço no portal do Azure. Isto significa que nenhum texto submetido para a API de V3, serão guardados pela Microsoft.
* JSON - XML é substituído pela JSON. Todos os dados enviados para o serviço e recebeu do serviço está no formato JSON.
* Várias linguagens de destino num único pedido - traduzir o método aceita vários 'para' idiomas para tradução num único pedido. Por exemplo, um único pedido pode ser 'from' inglês e 'até' alemão, espanhol e japonês ou qualquer outro grupo de idiomas.
* Dicionário bilingues - um método de dicionário bilingues foi adicionado à API. Este método inclui 'pesquisa' e 'exemplos'.
* Transliterate - um método transliterate foi adicionado à API. Este método irá converter palavras e frases um script (por ex Arabic) para outro script (por ex Latim).
* Idiomas - um novo método de 'idiomas' fornece informações de idioma, no formato JSON, para utilização com o 'Traduzir', 'dicionário' e 'transliterate' métodos.
* Novo para Translate - foram adicionadas novas funcionalidades para o método 'Traduzir' para suportar a algumas das funcionalidades que foram na V2 API como métodos separados. Um exemplo é TranslateArray.
* Enunciar método - funcionalidade de reconhecimento de voz do texto para já não é suportada na API tradutor Microsoft. Funcionalidade de reconhecimento de voz do texto para está disponível nos serviços Microsoft Azure cognitivos API de reconhecimento de voz do Bing.

A seguinte lista de métodos V2 e V3 identifica o APIs que irá fornecer a funcionalidade fornecido com o V2 e V3 métodos.

| V2 o método API   | V3 Compatibilidade de API |
|:----------- |:-------------|
| Traduzir     | Traduzir          |
| TranslateArray      | Traduzir          |
| GetLanguageNames      | Linguagens          |
| GetLanguagesForTranslate     | Linguagens        |
| GetLanguagesForSpeak      | Serviços cognitivos voz API         |
| Enunciar     | Serviços cognitivos voz API          |
| Detetar     | Detetar         |
| DetectArray     | Detetar         |
| AddTranslation     | Microsoft tradutor HUB API         |
| AddTranslationArray    | Microsoft tradutor HUB API          |
| BreakSentences      | BreakSentence         |
| GetTranslations      | A funcionalidade já não é suportada         |
| GetTranslationsArray      | A funcionalidade já não é suportada         |

## <a name="move-to-json-format"></a>Mover para o formato JSON

Microsoft tradutor texto tradução V2 aceite e devolveu dados no formato XML. Na V3 todos os dados enviados e recebidos utilizando a API está no formato JSON. XML já não será aceite ou devolvido na V3. 

Esta alteração afetará vários aspetos de uma aplicação de escrita para a API de conversão de texto V2. Por exemplo: A API de idiomas devolve informações de idioma para a conversão de texto, transliteration e os métodos de dicionário de dois. Pode pedir todas as informações de idioma para todos os métodos numa chamada ou pedi-los individualmente.

O método de idiomas não necessita de autenticação; ao clicar na ligação seguinte, pode ver todas as informações de idioma para V3 no JSON:

[https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation, dicionário, transliteration](https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation,dictionary,transliteration)

## <a name="authentication-key"></a>Chave de Autenticação

A chave de autenticação que está a utilizar para V2 serão aceites para V3. Não terá de obter uma nova subscrição. Será capaz de combinar V2 e V3 nas suas aplicações durante o período de migração yearlong, tornando mais fácil para si libertar novas versões enquanto ainda estiver a migrar a partir de V2 XML para V3 JSON.

## <a name="pricing-model"></a>Modelo de Determinação de Preço

Microsoft tradutor V3 tem um preço da mesma forma que v2 foi um preço; por carateres, incluindo espaços. As novas funcionalidades no V3 fazer algumas alterações em que carateres são contados para faturação.

| Método de v3   | Carateres contabilizadas para faturação |
|:----------- |:-------------|
| Linguagens     | Não existem carateres submetidos, none contados, sem encargos.          |
| Traduzir     | Contagem baseiam quantos carateres são submetidos para conversão e idiomas quantos carateres são convertidos. 50 carateres submetido e 5 idiomas pedidos serão 50 x 5.           |
| Transliterate     | Número de carateres submetido para transliteration é contado.         |
| Pesquisa de dicionário & exemplo     | Número de carateres submetido para pesquisa de dicionário e exemplos é contado.         |
| BreakSentence     | Sem qualquer encargo.       |
| Detetar     | Sem qualquer encargo.      |

## <a name="v3-end-points"></a>V3 pontos finais

Global

* API.cognitive.microsofttranslator.com


## <a name="v3-api-text-translations-methods"></a>Métodos de traduções de texto de API de v3

[Idiomas](reference/v3-0-languages.md)

[Traduzir](reference/v3-0-translate.md)

[Transliterate](reference/v3-0-transliterate.md)

[BreakSentence](reference/v3-0-break-sentence.md)

[Detetar](reference/v3-0-detect.md)

[Dicionário/pesquisa](reference/v3-0-dictionary-lookup.md)

[Dicionário/exemplo](reference/v3-0-dictionary-examples.md)

## <a name="customization"></a>Personalização

Microsoft tradutor V3 utiliza neuronal tradução por predefinição. Como tal, não pode ser utilizado com o Microsoft tradutor Hub que só suporta tradução análises legada. Personalização de tradução neuronal está agora disponível o conversor de personalizada a utilizar. [Saiba mais sobre como personalizar neuronal tradução](customization.md)

Tradução neuronal com o texto de V3 API não suporta a utilização das categorias standard (smt, voz, texto, generalnn).


## <a name="links"></a>Ligações

* [Política de privacidade da Microsoft](https://privacy.microsoft.com/privacystatement)
* [Informação Legal do Microsoft Azure](https://azure.microsoft.com/support/legal)
* [Termos de licenciamento de serviços online](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Documentação de v 3.0 de vista](reference/v3-0-reference.md)
