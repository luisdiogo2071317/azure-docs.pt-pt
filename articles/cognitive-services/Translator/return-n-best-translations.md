---
title: Devolver traduções de melhor N com o texto de Microsoft tradutor API | Microsoft Docs
description: Devolva traduções de melhor N utilizando a API de texto do conversor de Microsoft.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: 3eafe50f69ae1a6748342e64a414ecee4467d0d1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352598"
---
# <a name="how-to-return-n-best-translations"></a>Como devolver traduções de melhor N

> [!NOTE]
> Este método foi preterido. Não está disponível no v 3.0 da API de texto tradutor.

Os métodos GetTranslations() e GetTranslationsArray() da API de tradutor Microsoft incluem um sinalizador booleano opcional "IncludeMultipleMTAlternatives".
O método regressará até maxTranslations alternativas onde o delta é fornecido na lista de N melhor do motor de conversor.

A assinatura é:

**Sintaxe**

| C# |
|:---|
| GetTranslationsResponse Microsoft.Translator.GetTranslations(appId, text, from, to, maxTranslations, options); |

**Parâmetros**

| Parâmetro | Descrição |
|:---|:---|
| appId | **Necessário** se for utilizado o cabeçalho de autorização, deixe o campo de appid vazio ou especifique uma cadeia contendo "Portador" + "" + o token de acesso.|
| texto | **Necessário** uma cadeia que representa o texto a converter. O tamanho do texto não pode exceder 10000 carateres.|
| de | **Necessário** uma cadeia representando o código de idioma do texto a converter. |
| para | **Necessário** uma cadeia representando o código de idioma para traduzir texto para. |
| maxTranslations | **Necessário** um int que representa o número máximo de traduções para devolver. |
| opções | **Opcional** TranslateOptions de um objeto que contém os valores apresentados abaixo. Estes são todos os opcionais e predefinido para as definições mais comuns.

* Categoria: O único suportado e a predefinição, a opção é "geral".
* ContentType: O único suportado, não sendo o predefinido, opção "text/plain".
* Estado: Estado do utilizador ajudar correlacionar pedido e resposta. O conteúdo mesmo será devolvido na resposta.
* IncludeMultipleMTAlternatives: sinalizador para determinar se deve devolver mais do que um alternativas do motor MT. Predefinição é falsa e inclui apenas 1 alternativo.

## <a name="ratings"></a>Classificações
As classificações são aplicadas da seguinte forma: A tradução automática melhor tem uma classificação de 5.
As alternativas de tradução (melhor-N) geradas automaticamente têm uma classificação de 0 e têm um grau de correspondência de 100.

## <a name="number-of-alternatives"></a>Número de alternativas
O número de alternativas devolvidos até maxTranslations, mas pode ser menor.

## <a name="language-pairs"></a>Pares de idioma
Esta funcionalidade não está disponível para traduções entre simplificado e chinês tradicional, ambas as direções. Está disponível para todos os outros pares de idioma Translator Microsoft suportadas.
