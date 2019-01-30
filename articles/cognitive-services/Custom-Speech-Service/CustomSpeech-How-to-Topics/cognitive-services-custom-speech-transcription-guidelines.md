---
title: Diretrizes de transcrição no serviço de voz personalizada no Azure | Documentos da Microsoft
description: Saiba como preparar os dados para o serviço de voz personalizada nos serviços cognitivos.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: 7d9b8b5a2ad8ccb447462ef7d34a8c25e5af6ef4
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55225028"
---
# <a name="transcription-guidelines"></a>Diretrizes de transcrição

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Para garantir o melhor uso dos seus dados de texto para personalização do modelo de acústica e idioma, as seguintes diretrizes de transcrição devem ser seguidas. Essas diretrizes são específicas do idioma.

## <a name="text-normalization"></a>Normalização de texto

Para o melhor uso na personalização de modelos acústicos ou linguagem, os dados de texto têm de ser normalizados, o que significa transformado num formulário padrão, inequívoca legível pelo sistema. Esta secção descreve a normalização do texto realizada pelo serviço de voz personalizada quando dados são importados e a normalização do texto que o utilizador tem de efetuar antes da importação de dados.

## <a name="inverse-text-normalization"></a>Normalização do texto inversa

O processo de conversão de texto formatado "bruto" voltar ao texto formatado, ou seja, com o uso e a pontuação, é chamado de normalização do texto inversa (ITN). ITN é executada nos resultados devolvidos pela API de voz dos serviços cognitivos da Microsoft. Um ponto final personalizado implementado com o serviço de voz personalizada utiliza a mesma ITN como a API de voz de serviços cognitivos da Microsoft. No entanto, este serviço não suporta atualmente ITN personalizado, para que os novos termos introduzidos por um modelo de idioma personalizado não serão formatados nos resultados de reconhecimento.

## <a name="transcription-guidelines-for-en-us"></a>Diretrizes de transcrição para en-US

Carregado para este serviço de dados de texto devem ser escritos em texto simples utilizando apenas o ASCII imprimível conjunto de carateres. Cada linha do ficheiro deve conter o texto para apenas uma expressão única.

É importante evitar a utilização de carateres de pontuação do Unicode. Isto pode acontecer de inadvertidamente se preparar os dados de uma palavra de programa de processamento ou captura de dados a partir de páginas da web. Substitua estes carateres com as substituições apropriadas das ASCII. Por exemplo:

| Unicode para evitar | Substituição de ASCII |
|----- | ----- |
| "Hello world" (abrir e fechar as aspas duplas) | "Hello world" (as aspas duplas) |
| Dia de John (à direita plica) | Dia de John (apóstrofe) |

### <a name="text-normalization-performed-by-the-custom-speech-service"></a>Normalização do texto efetuada pelo serviço de voz personalizada

Este serviço irá efetuar a normalização do texto seguinte nos dados importados como um conjunto de dados de idioma ou transcrições para um conjunto de dados acústico. Isto inclui

*   Inferior-casing todo o texto
*   Remover todos os pontuação, exceto apóstrofos internas do word
*   Expansão de números ao formulário falado, incluindo quantidades de dólar

Aqui estão alguns exemplos

| Texto original | Depois de normalização |
|----- | ----- |
| Starbucks café | Starbucks café |
| "" Caramba "!" Batman disse. | "caramba" disse batman |
| "E"? sidekick disse Batman, Round Robin. | round robin de sidekick que disse batman |
| Vá obter - em! | Aceda get em |
| Eu sou double-jointed | i double estou jointed |
| 104 Main Street | um AH quatro Rua principal |
| Ajustar às 102.7 | Ligue-se a um AH sete de ponto de dois |
| Instalador de plataforma está prestes a 3.14 | Instalador de plataforma é aproximadamente três quatro de um de ponto |
| Custa us $3.14 | custa três quatorze |

### <a name="text-normalization-required-by-users"></a>Normalização do texto necessária para os utilizadores

Para garantir o melhor uso dos seus dados, as seguintes regras de normalização devem ser aplicadas aos seus dados antes de importá-lo.

*   Abreviações devem ser escritas em palavras para refletir o formulário falado
*   Cadeias de caracteres numéricas não padrão devem ser escritas em palavras
*   Palavras com carateres não alfabéticos ou caracteres mistos de alfanuméricos devem ser transcrito tão pronunciados
*   Acrônimos comuns podem ser deixados como uma única entidade sem pontos ou espaços entre as letras, mas todos os outros acrônimos devem ser escritos em letras separadas, com cada letra separada por um único espaço

Aqui estão alguns exemplos

| Texto original | Depois de normalização |
|----- | ----- |
| 14 NE 3rd Dr. | unidade Nordeste da terceira quatorze |
| Dr. Strangelove | O doutor Strangelove |
| James Bond 007 | James bond double AH sete |
| Ke$ha | Kesha |
| Quanto tempo dura 2 x 4 | Quanto tempo dura a duas por quatro |
| A reunião avança a partir de 1-3 pm | A reunião avança entre uma a três pm |
| é o meu tipo de sangue S + | Meu tipo de sangue será O positivo |
| água é H20 | água é O de 2 de H |
| reproduzir OU812 através de Van Halen | reproduzir O U 2 de 1 de 8 através de Van Halen |

## <a name="transcription-guidelines-for-zh-cn"></a>Diretrizes de transcrição para zh-CN

Dados de texto carregados para o serviço de voz personalizada devem usar **(incl BOM) de codificação UTF-8**. Cada linha do ficheiro deve conter o texto para apenas uma expressão única.

É importante evitar a utilização de carateres de pontuação de meia largura. Isto pode acontecer de inadvertidamente se preparar os dados de uma palavra de programa de processamento ou captura de dados a partir de páginas da web. Substitua estes carateres com as substituições apropriadas das largura total. Por exemplo:

| Unicode para evitar | Substituição de ASCII |
|----- | ----- |
| "你好" (open e close aspas duplas) | "你好" (aspas duplas) |
| 需要什么帮助? (ponto de interrogação) | 需要什么帮助? |

### <a name="text-normalization-performed-by-the-custom-speech-service"></a>Normalização do texto efetuada pelo serviço de voz personalizada

Este serviço de voz executará a normalização do texto seguinte nos dados importados como um conjunto de dados de idioma ou transcrições para um conjunto de dados acústico. Isto inclui

*   Remover todos os pontuação
*   Expansão de números ao formulário falado
*   Converta as letras de largura total letras de meia largura.
*   Canto superior-casing all inglês palavras

Eis alguns exemplos:

| Texto original | Depois de normalização |
|----- | ----- |
| 3.1415 | 三 点 一 四 一 五 |
| ￥3.5 | 三 元 五 角 |
| w f y z | W F Y Z |
| 1992年8月8日 | 一 九 九 二 年 八 月 八 日 |
| 你吃饭了吗? | 你 吃饭 了 吗 |
| 下午5:00的航班 | 下午 五点 的 航班 |
| 我今年21岁 | 我 今年 二十 一 岁 |

### <a name="text-normalization-required-by-users"></a>Normalização do texto necessária para os utilizadores

Para garantir o melhor uso dos seus dados, as seguintes regras de normalização devem ser aplicadas aos seus dados _anteriores_ para importá-lo.

*   Abreviações devem ser escritas em palavras para refletir o formulário falado
*   Este serviço não abrange todas as quantidades numéricas. É mais confiável para escrever cadeias de caracteres numéricas no formulário falado

Aqui estão alguns exemplos

| Texto original | Depois de normalização |
|----- | ----- |
| 我今年21 | 我今年二十一 |
| 3号楼504 | 三号 楼 五 零 四 |

## <a name="transcription-guidelines-for-de-de"></a>Diretrizes de transcrição de-DE

Só devem utilizar os dados de texto carregados para o serviço de voz personalizada **(incl BOM) de codificação UTF-8**. Cada linha do ficheiro deve conter o texto para apenas uma expressão única.

### <a name="text-normalization-performed-by-the-custom-speech-service"></a>Normalização do texto efetuada pelo serviço de voz personalizada

Este serviço irá efetuar a normalização do texto seguinte nos dados importados como um conjunto de dados de idioma ou transcrições para um conjunto de dados acústico. Isto inclui

*   Inferior-casing todo o texto
*   Remover todos os pontuação, incluindo o inglês ou alemães aspas ("teste", "teste", "teste" ou «testar» é ok)
*   Descartar qualquer linha que contém qualquer caráter especial, incluindo: ^ ¢ £ ¤ ¥ ¦ § © ª ¬® ° ± ² nossa × ÿ Ø¬¬
*   Expansão de números a forma de palavra, incluindo as quantidades de dólar ou euro
*   Aceitamos tremas do apenas para um, o u; outras pessoas serão substituídas por "th" ou eliminadas

Aqui estão alguns exemplos

| Texto original | Depois de normalização |
|----- | ----- |
| Anel de Frankfurter | anel de frankfurter |
| "Hallo, Mama!" sagt morrem Tochter. | hallo mama sagt die tochter |
| ¡Eine Frage! | eine frage |
| wir, haben | wir haben |
| Das macht US $10 | das macht zehn dólares |


### <a name="text-normalization-required-by-users"></a>Normalização do texto necessária para os utilizadores

Para garantir o melhor uso dos seus dados, as seguintes regras de normalização devem ser aplicadas aos seus dados antes de importá-lo.

*   Deve ser o ponto decimal e não. Por exemplo, 2,3% e não 2.3%
*   Separador de tempo entre horas e minutos deve ser: e não., por exemplo, 12:00 Uhr
*   Abreviações como "autoridade de certificação.',"bzw." não são substituídas. Recomendamos que utilize o formulário completo para ter a pronúncia correta.
*   Os operadores de matemáticos cinco principais são removidos: +, -, \*, /.
 Recomendamos que substituí-los pelo respetivo formato literal além disso, a menos que, geteilt.
*   Mesmo se aplica para os comparators (=, <>,) - gleich, kleiner als, grösser als
*   Utilizar fração como 3/4 na forma de palavra "drei viertel" em vez de ¾
*   Substituir o símbolo ' € com o formulário do word "Euro"


Aqui estão alguns exemplos

| Texto original | Depois de normalização do utilizador | Depois de normalização de sistema
|--------  | ----- | -------- |
| Es ist 12.23Uhr | Es ist 12:23Uhr | es ist zwölf uhr drei und zwanzig uhr |
| {12.45} | {12,45} | zwölf komma vier fünf |
| 3 < 5 | 3 kleiner als 5 | vier de als drei kleiner |
| 2 + 3 - 4 | 2 e 3 menos 4 | zwei plus drei minus vier|
| Das macht 12€ | Das macht Euros 12 | das macht zwölf euros |



## <a name="next-steps"></a>Passos Seguintes
* [Como utilizar um ponto de final de voz em texto personalizado](cognitive-services-custom-speech-create-endpoint.md)
* Melhorar a precisão com sua [um modelo acústico personalizado](cognitive-services-custom-speech-create-acoustic-model.md)
* Melhorar a precisão com um [modelo de idioma personalizado](cognitive-services-custom-speech-create-language-model.md)
