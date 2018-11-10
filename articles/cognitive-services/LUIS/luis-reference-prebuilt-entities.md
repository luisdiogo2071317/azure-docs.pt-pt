---
title: Entidades pré-concebidas - LUIS
titleSuffix: Azure Cognitive Services
description: Este artigo contém listas de entidades pré-criadas que estão incluídas na compreensão de idiomas (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/24/2018
ms.author: diberry
ms.openlocfilehash: bc23f2e5d8304400802c74093d4d78a1fcd8cf22
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219495"
---
# <a name="entities-per-culture"></a>Entidades por cultura

Compreensão de idiomas (LUIS) fornece entidades pré-concebidas. Quando uma entidade pré-criados está incluída na sua aplicação, o LUIS inclui a predição de entidade correspondente na resposta do ponto final. Todas as expressões de exemplo também são rotuladas com a entidade. O comportamento das entidades pré-concebidas **não é possível** ser modificado. Salvo indicação em contrário, entidades pré-concebidas estão disponíveis em todas as localidades de aplicação LUIS (culturas). A tabela seguinte mostra as entidades pré-concebidas que são suportadas para cada cultura.

Entidade pré-criados   |   Inglês (Estados Unidos)<br>```En-us```   |   Francês (França)<br>```fr-FR```   |   Italiano (Itália)<br>```it-IT```   |   Espanhol (Espanha)<br>```es-ES```   |   Chinês<br>```zh-CN```   |   Alemão<br>```de-DE```   |   Português (Brasil)<br>```pt-BR```   |   Japonês (Japão)<br>```ja-JP```   |   Coreano (Coreia)<br>```ko-kr```   | Francês (Canadá)<br>```fr-CA```   |   Espanhol (México)<br>```es-MX```   |   Holandês (Países Baixos)<br>```nl-NL```   |
------|:------:|------|------|------|------|------|------|------|------|------|------|------|
[Idade](luis-reference-prebuilt-age.md):<br>ano<br>mês<br>semana<br>dia   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[Moeda](luis-reference-prebuilt-currency.md):<br>Dólar<br>unidade fracionária (ex: mínima)  |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>hora<br>timerange   |    ✔   |   ✔   |   -   |   ✔   |    ✔   |   -   |   ✔   |   -   |   -   |   -   |   -   |   -   |
[Dimensão](luis-reference-prebuilt-dimension.md):<br>volume<br>área<br>peso<br>informações (ex: bits/byte)<br>comprimento (ex: medidor)<br>velocidade (ex: lá por hora)  |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[E-mail](luis-reference-prebuilt-email.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   |   ✔   |   ✔   |   ✔   |   -   |   ✔   |   ✔   |   ✔   |   ✔   |   ✔   |   ✔   |   ✔   |
[Número](luis-reference-prebuilt-number.md)   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[Ordinal](luis-reference-prebuilt-ordinal.md)   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[Percentagem](luis-reference-prebuilt-percentage.md)   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[PersonName](luis-reference-prebuilt-person.md)   |    ✔   |    -   |    -   |    -   |    ✔   |    -   |    -   |    -   |   -   |   -   |   -   |   -   |
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |
[Temperatura](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>Kelvin<br>Rankine<br>delisle<br>Celsius   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[URL](luis-reference-prebuilt-url.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |

Consulte as notas sobre [preterido entidades pré-concebidas](luis-reference-prebuilt-deprecated.md)

KeyPhrase não está disponível em todos os subcultures de Português (Brasil) - ```pt-BR```.

## <a name="contribute-to-prebuilt-entity-cultures"></a>Contribuir para culturas de entidade predefinidos
As entidades pré-concebidas são desenvolvidas no projeto de código-fonte aberto reconhecedores-texto. [Contribuir](https://github.com/Microsoft/Recognizers-Text) ao projeto. Esse projeto inclui exemplos de moeda por cultura. 

GeographyV2 e PersonName não estão incluídos no projeto reconhecedores-texto. Para problemas com estas entidades previamente concebidas, abra um [pedido de suporte](../../azure-supportability/how-to-create-azure-support-request.md). 

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o [número](luis-reference-prebuilt-number.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md), e [moeda](luis-reference-prebuilt-currency.md) entidades. 
