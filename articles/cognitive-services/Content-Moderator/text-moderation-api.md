---
title: Moderator conteúdo do Azure - moderação de interrupção de texto | Microsoft Docs
description: Utilize a moderação de interrupção de texto para possíveis indesejável texto, PII, e personalizada apresenta uma lista dos termos de licenciamento.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/30/2018
ms.author: sajagtap
ms.openlocfilehash: 5783a7a06d75a409969abad011de3bbd31dec292
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352724"
---
# <a name="text-moderation"></a>Moderação de texto

Utilize a moderação de interrupção do Moderator de conteúdo texto assistida por máquina e [humanos no ciclo](Review-Tool-User-Guide/human-in-the-loop.md) capacidades para moderada de conteúdos de texto.

As empresas utilizam o serviço de moderação interrupção de texto para bloquear, aprovar ou revisar o conteúdo com base nas respetivas políticas e limiares. O serviço de moderação interrupção de texto pode ser utilizado para melhorar a moderação de interrupção humana dos ambientes que necessitam de parceiros, funcionários e os consumidores gerar o conteúdo de texto. Estes incluem gabinetes de chat, quadros de debate, chatbots, eCommerce catálogos, documentos e muito mais. 

A API analisa o texto de entrada (máximo 1024 carateres) para profanity, classifica para texto indesejado possíveis (pré-visualização), autocorrects texto e Deteta potenciais informação identificativa (PII). Também corresponde à contra listas personalizadas dos termos de licenciamento. A funcionalidade de autocorrection ajuda a detetar palavras deliberadamente com erros ortográficos. Depois do conteúdo é processado, o serviço devolve uma resposta de detalhado. Utilize a resposta para criar uma revisão humana na ferramenta de revisão ou tirá-lo, etc.

A resposta do serviço inclui as seguintes informações:

- Profanity: com base no prazo de correspondência de lista incorporada dos termos profanas em vários idiomas
- Classificação: classificação assistida por máquina em três categorias
- Informações de identificação pessoal (PII)
- Texto auto-corrigidos
- Texto original
- Idioma

## <a name="profanity"></a>Profanity

Se a API Deteta quaisquer termos profanas em nenhum do [idiomas suportados](Text-Moderation-API-Languages.md), esses termos estão incluídos na resposta. A resposta também contém a respetiva localização (`Index`) no texto original. O `ListId` no seguinte exemplo JSON refere-se termos encontrados na [termo personalizado listas](try-terms-list-api.md) se estiver disponível.

    "Terms": [
    {
        "Index": 118,
        "OriginalIndex": 118,
        "ListId": 0,
        "Term": "crap"
    }

> [!NOTE]
> Para o **idioma** parâmetro, atribuir `eng` ou deixe vazio para ver a máquina assistida por **classificação** resposta (funcionalidade de pré-visualização). **Esta funcionalidade só suporta o inglês**.
>
> Para **termos profanity** deteção, utilize o [código ISO 639 3](http://www-01.sil.org/iso639-3/codes.asp) dos idiomas suportados listados neste artigo, ou deixe em branco.

## <a name="classification"></a>Classificação

Moderator conteúdo da máquina assistida **funcionalidade de classificação de texto** suporta **apenas inglês**, e ajuda a detetar conteúdo potencialmente indesejado. O conteúdo sinalizado pode ser considerado como inapropriado, dependendo de contexto. Para além de conveying a probabilidade de cada categoria, este poderá recomendar uma revisão humana do conteúdo. A funcionalidade utiliza um modelo preparado para identificar possíveis idioma abusivo, derogatory ou discriminatory. Isto inclui slang, abreviadas palavras, palavras ofensivas e intencionalmente com erros ortográficos para revisão. 

O extrair no extrair JSON seguinte mostra um exemplo de saída:

> [!NOTE]
> A funcionalidade de 'Classificação' assistida por máquina está em pré-visualização.

    "Classification": {
        "ReviewRecommended": true,
        "Category1": {
            "Score": 1.5113095059859916E-06
            },
        "Category2": {
            "Score": 0.12747249007225037
            },
        "Category3": {
            "Score": 0.98799997568130493
        }
    }

### <a name="explanation"></a>Explicação

- `Category1` representa a presença potencial de idioma que pode ser considerada sexually explícita ou para adultos em determinadas situações.
- `Category2` representa a presença potencial de idioma que pode ser considerada sexually suggestive ou madura em determinadas situações.
- `Category3` representa a presença potencial de idioma que pode ser considerada ofensiva em determinadas situações.
- `Score` está entre 0 e 1. Quanto maior for a classificação, quanto maior for o modelo é a previsão que a categoria pode ser aplicável. Esta pré-visualização depende de um modelo de análises, em vez de resultados manualmente codificados. Recomendamos que teste com o seus próprios conteúdo para determinar a forma como cada categoria alinha os seus requisitos.
- `ReviewRecommended` é true ou false, dependendo do modelo de pontuação interno limiares. Os clientes devem avaliar se pretende utilizar este valor ou opte por utilizar limiares personalizados com base nas políticas os respetivos conteúdas.

## <a name="personally-identifiable-information-pii"></a>Informações de identificação pessoal (PII)

A funcionalidade PII Deteta a presença potencial estas informações:

- Endereço de e-mail
- Endereço de Mailing E.U.A.
- Endereço IP
- Número de telefone E.U.A.
- Número de telefone do RU
- Número de segurança social (SSN)

O exemplo seguinte mostra uma resposta de exemplo:

    "PII": {
        "Email": [{
            "Detected": "abcdef@abcd.com",
            "SubType": "Regular",
            "Text": "abcdef@abcd.com",
            "Index": 32
            }],
        "IPA": [{
            "SubType": "IPV4",
            "Text": "255.255.255.255",
            "Index": 72
            }],
        "Phone": [{
            "CountryCode": "US",
            "Text": "6657789887",
            "Index": 56
            }, {
            "CountryCode": "US",
            "Text": "870 608 4000",
            "Index": 212
            }, {
            "CountryCode": "UK",
            "Text": "+44 870 608 4000",
            "Index": 208
            }, {
            "CountryCode": "UK",
            "Text": "0344 800 2400",
            "Index": 228
            }, {
            "CountryCode": "UK",
            "Text": "0800 820 3300",
            "Index": 245
            }],
        "Address": [{
            "Text": "1 Microsoft Way, Redmond, WA 98052",
            "Index": 89
            }],
        "SSN": [{
            "Text": "999999999",
            "Index": 56
            }, {
            "Text": "999-99-9999",
            "Index": 267
            }]
        }

## <a name="auto-correction"></a>Correção automática

Suponha que é o texto de entrada (as 'lzay' e 'f0x' são intencional):

    The qu!ck brown f0x jumps over the lzay dog.

Se lhe pedir correção automática, a resposta contém a versão corrigida do texto:

    The quick brown fox jumps over the lazy dog.

## <a name="creating-and-managing-your-custom-lists-of-terms"></a>Criar e gerir a sua listas personalizadas dos termos de licenciamento

Apesar do predefinido, uma lista global dos termos funciona muito bem para a maioria dos casos, poderá querer ecrã contra termos específicos para as suas necessidades de negócio. Por exemplo, poderá filtrar quaisquer competitivos nomes de marca de publicações pelos utilizadores. O limiar de conteúdo permitido texto pode ser diferente da lista predefinida.

> [!NOTE]
> Há um limite máximo de **apresenta uma lista de 5 termo** com cada lista para **não pode exceder 10 000 termos**.
>

O exemplo seguinte mostra o ID de lista correspondente:

    "Terms": [
    {
        "Index": 118,
        "OriginalIndex": 118,
        "ListId": 231.
        "Term": "crap"
    }

O conteúdo Moderator fornece um [termo lista API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f) com operações de gestão personalizado termo apresenta uma lista. Começar a utilizar o [termo apresenta uma lista de API consola](try-terms-list-api.md) e utilizar os exemplos de código da REST API. Consulte também o [início rápido do termo apresenta uma lista de .NET](term-lists-quickstart-dotnet.md) se estiver familiarizado com o Visual Studio e c#.

## <a name="next-steps"></a>Passos Seguintes

Testar o [consola de moderação de interrupção API texto](try-text-api.md) e utilizar os exemplos de código da REST API. Consulte também o [guia de introdução do texto moderação interrupção .NET](text-moderation-quickstart-dotnet.md) se estiver familiarizado com o Visual Studio e c#.
