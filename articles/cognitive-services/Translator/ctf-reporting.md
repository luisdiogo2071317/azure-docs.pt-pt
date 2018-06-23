---
title: Tradução de colaboração tradutor Microsoft Reporting Framework (CTF)
description: Como utilizar os relatórios de colaboração tradução Framework (CTF).
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: cefc630a82a56703ba4942bcad18f6e0a38b1ee5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352604"
---
# <a name="how-to-use-collaborative-translation-framework-ctf-reporting"></a>Como utilizar os relatórios de colaboração tradução Framework (CTF)

> [!NOTE]
> Este método foi preterido. Não está disponível no v 3.0 da API de texto tradutor.

> A colaboração traduções Framework (CTF), anteriormente disponíveis para v 2.0 da API de texto tradutor, foi preterido a partir de 1 de Fevereiro de 2018. As funções AddTranslation e AddTranslationArray permitir que os utilizadores ativar correções através do Framework de tradução de colaboração. Após a 31 de Janeiro de 2018, estas duas funções não aceitou novo submissões de frases e os utilizadores recebem uma mensagem de erro. Estas funções foram vai ser reformadas e não serão substituídas. 

>Uma funcionalidade semelhante está disponível na API Hub tradutor, permitindo-lhe criar um sistema de tradução personalizadas com a terminologia e estilo, e pode invocar com o ID de categoria na API tradutor texto. Hub de conversor: [ https://hub.microsofttranslator.com ](https://hub.microsofttranslator.com). API de Hub de conversor: [ https://hub.microsofttranslator.com/swagger ](https://hub.microsofttranslator.com/swagger).

A API de relatórios de colaboração tradução Framework (CTF) devolve o conteúdo propriamente dito e estatísticas no arquivo de CTF. Esta API é diferente do método GetTranslations() porque-lo:
* Devolve o conteúdo traduzido e a contagem total apenas a partir da sua conta (appId ou a conta do Azure Marketplace).
* Devolve o conteúdo traduzido e a contagem total, sem necessidade de uma correspondência do frase de origem.
* Não devolveu a tradução automática (tradução).

## <a name="endpoint"></a>Ponto Final
O ponto final da API de relatórios CTF http://api.microsofttranslator.com/v2/beta/ctfreporting.svc
                        

## <a name="methods"></a>Métodos
| Nome |    Descrição|
|:---|:---|
| Método de GetUserTranslationCounts | Obtenha contagens de traduções que são criadas pelo utilizador. |
| Método de GetUserTranslations | Obtém as traduções que são criadas pelo utilizador. |

Estes métodos permitem-lhe:
* Obter o conjunto completo de traduções de utilizador e as correções com o ID de conta para transferência.
* Obter a lista dos contribuintes frequentes. Certifique-se de que o nome de utilizador correto é fornecido em AddTranslation().
* Crie uma interface de utilizador (IU) que permite aos utilizadores fidedignos ver todos os candidatos disponíveis, se necessário restringidas a uma parte do seu site, com base no prefixo de URI.

> [!NOTE]
> Ambos os métodos são relativamente lenta e dispendioso. Recomenda-se a utilizá-los com moderação.

## <a name="getusertranslationcounts-method"></a>Método de GetUserTranslationCounts

Este método obtém a contagem de traduções que são criados pelo utilizador. Fornece a lista de contagens de tradução agrupados por uriPrefix, para os parâmetros do pedido maxRating, minRating e utilizador.

**Sintaxe**

> [!div class="tabbedCodeSnippets"]
```cs
UserTranslationCount[]GetUserTranslationCounts(
           string appId,
           string uriPrefix,
           string from,
           string to,
           int? minRating,
           int? maxRating,
           string user, 
           string category
           DateTime? minDateUtc,
           DateTime? maxDateUtc,
           int? skip,
           int? take);
```

**Parâmetros**

| Parâmetro | Descrição |
|:---|:---|
| appId | **Necessário** se for utilizado o cabeçalho de autorização, deixe o campo de appid vazio ou especifique uma cadeia contendo "Portador" + "" + o token de acesso.|
| uriPrefix | **Opcional** uma cadeia contendo o prefixo de URI da tradução.|
| de | **Opcional** uma cadeia representando o código de idioma do texto tradução. |
| para | **Opcional** uma cadeia representando o código de idioma para traduzir texto para.|
| minRating| **Opcional** um número inteiro que representa a classificação de qualidade mínima para o texto traduzida. O valor válido é entre -10 e 10. O valor predefinido é 1.|
| maxRating| **Opcional** um número inteiro que representa a classificação de qualidade máximo para o texto traduzida. O valor válido é entre -10 e 10. O valor predefinido é 1.|
| Utilizador | **Opcional** uma cadeia que é utilizada para filtrar os resultados com base no originador da submissão. |
| categoria| **Opcional** uma cadeia que contém a categoria ou o domínio da tradução. Este parâmetro suporta apenas a opção de predefinida geral.|
| minDateUtc| **Opcional** a data no que se pretende obter as traduções. A data tem de estar no formato UTC. |
| maxDateUtc| **Opcional** data até quando pretende obter as traduções. A data tem de estar no formato UTC. |
| Ignorar| **Opcional** o número de resultados que pretende ignorar numa página. Por exemplo, se pretender que a ignorar as linhas primeiro 20 dos resultados e a vista do registo de resultado dia 21, especificar 20 para este parâmetro. O valor predefinido para este parâmetro é 0.|
| tirar | **Opcional** o número de resultados que pretende obter. O número máximo de cada pedido é 100. A predefinição é 100.|

> [!NOTE]
> Os parâmetros de pedido ignorar e tomar ativar a paginação de um grande número de registos de resultado.

**Valor devolvido**

O conjunto de resultados contém a matriz do **UserTranslationCount**. Cada UserTranslationCount tem os seguintes elementos:

| Campo | Descrição |
|:---|:---|
| Contagem| O número de resultados que é obtido|
| De | O idioma de origem|
| Classificação| A classificação que é aplicada pelo submissor na chamada de método AddTranslation()|
| Para| O idioma de destino|
| Uri| O URI aplicado na chamada de método AddTranslation()|
| Utilizador| O nome de utilizador|

**Exceções**

| Exceção | Mensagem | Condições |
|:---|:---|:---|
| ArgumentOutOfRangeException | O parâmetro '**maxDateUtc**'tem de ser maior que ou igual a'**minDateUtc**'.| O valor do parâmetro **maxDateUtc** é menor que o valor do parâmetro **minDateUtc**.|
| TranslateApiException | O IP está sobre a quota.| <ul><li>Foi atingido o limite para o número de pedidos por minuto.</li><li>O tamanho do pedido permanece limitado em 10000 carateres.</li><li>Uma hora a hora e uma quota diária limitam o número de carateres que a Microsoft tradutor API aceitará.</li></ul>|
| TranslateApiException | AppId está sobre a quota.| O ID da aplicação excedeu a quota de hora ou diária.|

> [!NOTE]
> Irá ajustar a quota para garantir a justiça entre todos os utilizadores do serviço.

**Exemplos de código de vista no GitHib**
* [C#](https://github.com/MicrosoftTranslator/Documentation-Code-TextAPI/blob/master/ctf/ctf-getusertranslationcounts-example-csharp.md)
* [PHP](https://github.com/MicrosoftTranslator/Documentation-Code-TextAPI/blob/master/ctf/ctf-getusertranslationcounts-example-php.md)

## <a name="getusertranslations-method"></a>Método de GetUserTranslations

Este método obtém as traduções que são criadas pelo utilizador. Fornece que as traduções agrupados por uriPrefix, como, utilizador e minRating e maxRating parâmetros do pedido.

**Sintaxe**

> [!div class="tabbedCodeSnippets"]
```cs
UserTranslation[] GetUserTranslations (
            string appId,
            string uriPrefix,
            string from,
            string to,
            int? minRating,
            int? maxRating,
            string user, 
            string category
            DateTime? minDateUtc,
            DateTime? maxDateUtc,
            int? skip,
            int? take); 
```

**Parâmetros**

| Parâmetro | Descrição |
|:---|:---|
| appId | **Necessário** se for utilizado o cabeçalho de autorização, deixe o campo de appid vazio ou especifique uma cadeia contendo "Portador" + "" + o token de acesso.|
| uriPrefix| **Opcional** uma cadeia contendo o prefixo de URI da tradução.|
| de| **Opcional** uma cadeia representando o código de idioma do texto tradução.|
| para| **Opcional** uma cadeia representando o código de idioma para traduzir texto para.|
| minRating| **Opcional** um número inteiro que representa a classificação de qualidade mínima para o texto traduzida. O valor válido é entre -10 e 10. O valor predefinido é 1.|
| maxRating| **Opcional** um número inteiro que representa a classificação de qualidade máximo para o texto traduzida. O valor válido é entre -10 e 10. O valor predefinido é 1.|
| Utilizador| **Opcional. Uma cadeia que é utilizada para filtrar os resultados com base no originador da submissão**|
| categoria| **Opcional** uma cadeia que contém a categoria ou o domínio da tradução. Este parâmetro suporta apenas a opção de predefinida geral.| 
| minDateUtc| **Opcional** a data no que se pretende obter as traduções. A data tem de estar no formato UTC.| 
| maxDateUtc| **Opcional** data até quando pretende obter as traduções. A data tem de estar no formato UTC.|
| Ignorar| **Opcional** o número de resultados que pretende ignorar numa página. Por exemplo, se pretender que a ignorar as linhas primeiro 20 dos resultados e a vista do registo de resultado dia 21, especificar 20 para este parâmetro. O valor predefinido para este parâmetro é 0.|
| tirar| **Opcional** o número de resultados que pretende obter. O número máximo de cada pedido é 100. A predefinição é 50.|

> [!NOTE]
> Os parâmetros de pedido ignorar e tomar ativar a paginação de um grande número de registos de resultado.

**Valor devolvido**

O conjunto de resultados contém a matriz do **UserTranslation**. Cada UserTranslation tem os seguintes elementos:

| Campo | Descrição |
|:---|:---|
| CreatedDateUtc| A data de criação de entrada, usando AddTranslation()|
| De| O idioma de origem|
| OriginalText| O texto de origem de idioma que é utilizado quando a submeter o pedido|
|Classificação |A classificação que é aplicada pelo submissor na chamada de método AddTranslation()|
|Para|    O idioma de destino|
|TranslatedText|    A tradução como submetido na chamada de método AddTranslation()|
|Uri|   O URI aplicado na chamada de método AddTranslation()|
|Utilizador   |O nome de utilizador|

**Exceções**

| Exceção | Mensagem | Condições |
|:---|:---|:---|
| ArgumentOutOfRangeException | O parâmetro '**maxDateUtc**'tem de ser maior que ou igual a'**minDateUtc**'.| O valor do parâmetro **maxDateUtc** é menor que o valor do parâmetro **minDateUtc**.|
| TranslateApiException | O IP está sobre a quota.| <ul><li>Foi atingido o limite para o número de pedidos por minuto.</li><li>O tamanho do pedido permanece limitado em 10000 carateres.</li><li>Uma hora a hora e uma quota diária limitam o número de carateres que a Microsoft tradutor API aceitará.</li></ul>|
| TranslateApiException | AppId está sobre a quota.| O ID da aplicação excedeu a quota de hora ou diária.|

> [!NOTE]
> Irá ajustar a quota para garantir a justiça entre todos os utilizadores do serviço.

**Exemplos de código de vista no GitHib**
* [C#](https://github.com/MicrosoftTranslator/Documentation-Code-TextAPI/blob/master/ctf/ctf-getusertranslations-example-csharp.md)
* [PHP](https://github.com/MicrosoftTranslator/Documentation-Code-TextAPI/blob/master/ctf/ctf-getusertranslations-example-php.md)


















