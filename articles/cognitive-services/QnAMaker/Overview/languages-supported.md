---
title: Serviços cognitivos do Azure - Maker de QnA - de idiomas suportados pelo | Microsoft Docs
description: Saiba que idiomas são suportados para Maker de QnA.
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: b210f59129a962046787b27d003c2872a54f6c8e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354938"
---
# <a name="supported-languages"></a>Linguagens suportadas

O idioma de uma base de dados de conhecimento afeta a capacidade de QnA Maker extrair automaticamente perguntas e respostas de [origens](../Concepts/data-sources-supported.md), bem como a importância dos resultados Maker de QnA fornece em resposta a consultas do utilizador.

## <a name="auto-extraction"></a>Extração automática
Maker de QnA suporta extração pergunta/resposta em qualquer página de idioma, mas a eficácia da extração é muito superior para os seguintes idiomas, conforme Maker de QnA utiliza palavras-chave para identificar perguntas.

|Idiomas suportados| Região|
|-----|----|
|Português|EN-*|
|Francês|FR-*|
|Italiano|IT-*|
|Alemão|Alemanha-*|
|Espanhol|es-*|

## <a name="query-matching-and-relevance"></a>Correspondência de consulta e relevância
Depende de QnA Maker [analisadores de idioma](https://docs.microsoft.com/en-us/rest/api/searchservice/language-support) na pesquisa do Azure para fornecer os resultados. Funcionalidades de função de classificação novamente especiais estão disponíveis para En-* idiomas que ativam melhor relevância.

Maker de QnA auto-Deteta o idioma de base de dados de conhecimento durante a criação e define o analisador em conformidade. Pode criar bases de dados de conhecimento nos seguintes idiomas. Leitura [isto](../How-To/language-knowledge-base.md) para obter mais detalhes sobre como Maker de QnA processa idiomas.


> [!Tip]
> Analisadores de idiomas, uma vez definida, não pode ser alterada. Além disso, o analisador de idioma aplica-se a todas as bases de dados de conhecimento num [serviço Maker de QnA](../How-To/set-up-qnamaker-service-azure.md). Se planear ter bases de dados de conhecimento no idioma diferente, deve criá-los em serviços de QnA Maker separados.

|Idiomas suportados|
|-----|
|Árabe|
|Arménio|,
Bengali|
|Basco (Basco)|
|Búlgaro|
|Catalão|
|Chinese_Simplified|
|Chinese_Traditional|
|Croata|
|Checo|
|Dinamarquês|
|Neerlandês|
|Português|
|Estónio|
|Finlandês|
|Francês|
|Galego|
|Alemão|
|Grego|
|Guzarate|
|Hebraico|
|Hindi|
|Húngaro|
|Islandês|
|Indonésio|
|Irlandês|
|Italiano|
|Japonês|
|Canarim|
|Coreano|
|Letão|
|Lituano|
|Malayalam|
|Malaio|
|Norueguês|
|Polaco|
|Português|
|Punjabi|
|Romeno|
|Russo|
|Serbian_Cyrillic|
|Serbian_Latin|
|Eslovaco|
|Esloveno|
|Espanhol|
|Sueco|
|Tamil|
|Télego|
|Tailandês|
|Turco|
|Ucraniano|
|Urdu|
|Vietnamita|
