---
title: Diretrizes de transcription no serviço de reconhecimento de voz personalizadas no Azure | Microsoft Docs
description: Saiba como preparar os dados para o serviço de reconhecimento de voz personalizadas nos serviços cognitivos.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: 2785a35ac7583ac3d9503cb721d10078d86aa365
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351830"
---
# <a name="transcription-guidelines"></a>Diretrizes de transcription
Para garantir a melhor utilização dos seus dados de texto para personalização de modelo acústica e de idioma, as seguintes diretrizes de transcription devem ser seguidas. Estas diretrizes são específicas do idioma.

## <a name="text-normalization"></a>Normalização de texto

Para utilização ideal na personalização de modelo do idioma ou acústica, os dados de texto tem sejam normalizados, o que significa transformado um formulário padrão, inequívoca legível pelo sistema. Esta secção descreve a normalização de texto efetuada pelo serviço de reconhecimento de voz personalizado quando for importados dados e a normalização de texto que o utilizador tem de executar antes da importação de dados.

## <a name="inverse-text-normalization"></a>Normalização de texto inverso

O processo de conversão de texto não formatado "não processado" para texto formatado, ou seja, com maiúsculas/minúsculas e pontuação, chama-se a normalização de texto inverso (ITN). ITN é executada nos resultados devolvidos pela API de reconhecimento de voz de serviços cognitivos da Microsoft. Um personalizado o ponto final implementado utilizando o serviço de reconhecimento de voz personalizadas utiliza o mesmo ITN como a API de reconhecimento de voz de serviços cognitivos da Microsoft. No entanto, este serviço não suporta atualmente ITN personalizado, pelo que os novos termos introduzidos por um modelo de idioma personalizado não serão formatados nos resultados de reconhecimento.

## <a name="transcription-guidelines-for-en-us"></a>Diretrizes de transcription para en-US

Dados de texto carregados para este serviço devem ser escritos em texto simples utilizando apenas a ASCII imprimível conjunto de carateres. Cada linha do ficheiro deve conter o texto de um único utterance apenas.

É importante evitar a utilização de carateres de pontuação de Unicode. Isto pode acontecer inadvertidamente se preparar os dados no word um programa de processamento ou scraping dados a partir de páginas web. Substitua estes carateres substituições de ASCII adequadas. Por exemplo:

| Unicode para evitar | Substituição de ASCII |
|----- | ----- |
| "Olá mundo" (abrir e fechar aspas duplas) | "Olá mundo" (aspas) |
| Dia de João (à direita plica) | Dia de João (apóstrofo) |

### <a name="text-normalization-performed-by-the-custom-speech-service"></a>Efetuada pelo serviço de reconhecimento de voz personalizadas de normalização de texto

Este serviço irá efetuar a normalização de texto seguinte nos dados importados como um conjunto de dados de idioma ou transcriptions para um conjunto de dados acústica. Isto inclui

*   Minúscula-maiúsculas e minúsculas todo o texto
*   Remover todos os pontuação exceto apostrophes word interno
*   Expansão dos números existentes para formar ditas, incluindo quantidades dólar

Seguem-se alguns exemplos

| Texto original | Depois de normalização |
|----- | ----- |
| Starbucks café | Starbucks café |
| "Holy cow!" Batman referida. | Holy cow consiga aceder tal batman |
| "E"? sidekick referida Batman, Round Robin. | round robin de sidekick que referida batman |
| Aceda obter - IT! | Aceda get em |
| Sou double-jointed | i estou jointed duplo |
| 104 Main Rua | um esqueceu quatro Rua principal |
| Otimizar a 102.7 | otimizar a um esqueceu dois ponto sete |
| Instalador de plataforma está prestes a 3.14 | Instalador de plataforma está prestes três ponto um quatro |
| Custos de $3.14 | custos de três fourteen |

### <a name="text-normalization-required-by-users"></a>Necessário para os utilizadores de normalização de texto

Para garantir a melhor utilização dos seus dados, as seguintes regras de normalização devem ser aplicadas aos seus dados antes de importá-lo.

*   Abreviaturas devem ser escritas na palavras para refletir o formulário ditas
*   Não padrão cadeias numérico devem ser escritas no palavras
*   Devem ser transcribed palavras com carateres alfanuméricos mistos ou carateres não alfabéticos como pronunciados
*   Utilização de acrónimos comum pode ser deixada como uma única entidade sem pontos ou espaços entre as letras, mas todas as outras utilização de acrónimos deve ser escritas em separado letras, com cada letra separada por um único espaço

Seguem-se alguns exemplos

| Texto original | Depois de normalização |
|----- | ----- |
| 14 NE 3rd Dr. | unidade northeast terceiro fourteen |
| Dr. Strangelove | Doctor Strangelove |
| James Bond 007 | James bond duplo esqueceu sete |
| KE$ ha | Kesha |
| Quanto tempo é 2, 4 | Quanto tempo é dois por quatro |
| A reunião fica de 1-3 pm | A reunião passa uma a três pm |
| o tipo do meu blood é Nã + | O tipo do meu blood é Nã positivo |
| máximo é H20 | máximo é O de 2 H |
| reproduzir OU812 por Van Halen | reproduzir O U 8 1 2 por Van Halen |

## <a name="transcription-guidelines-for-zh-cn"></a>Diretrizes de transcription para zh-CN

Devem utilizar dados de texto carregados para o serviço de reconhecimento de voz personalizadas **(incl. LM) de codificação UTF-8**. Cada linha do ficheiro deve conter o texto de um único utterance apenas.

É importante evitar a utilização de carateres de pontuação de largura do meio. Isto pode acontecer inadvertidamente se preparar os dados no word um programa de processamento ou scraping dados a partir de páginas web. Substitua estes carateres substituições de largura de completo adequadas. Por exemplo:

| Unicode para evitar | Substituição de ASCII |
|----- | ----- |
| "你好" (abrir e fechar aspas duplas) | "你好" (aspas duplas) |
| 需要什么帮助? (ponto de interrogação) | 需要什么帮助? |

### <a name="text-normalization-performed-by-the-custom-speech-service"></a>Efetuada pelo serviço de reconhecimento de voz personalizadas de normalização de texto

Este serviço de reconhecimento de voz irão efetuar a normalização de texto seguinte nos dados importados como um conjunto de dados de idioma ou transcriptions para um conjunto de dados acústica. Isto inclui

*   Remover todos os pontuação
*   Expansão dos números existentes para formar ditas
*   Converta completo largura letras letras largura do meio.
*   Maiúscula-maiúsculas e minúsculas all inglês palavras

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

### <a name="text-normalization-required-by-users"></a>Necessário para os utilizadores de normalização de texto

Para garantir a melhor utilização dos seus dados, as seguintes regras de normalização devem ser aplicadas aos seus dados _anterior_ para importá-lo.

*   Abreviaturas devem ser escritas na palavras para refletir o formulário ditas
*   Este serviço não abrange todas as quantidades numérico. É mais fiável para escrever um valor numérico cadeias no formulário ditas

Seguem-se alguns exemplos

| Texto original | Depois de normalização |
|----- | ----- |
| 我今年21 | 我今年二十一 |
| 3号楼504 | 三号 楼 五 零 四 |

## <a name="transcription-guidelines-for-de-de"></a>Diretrizes de transcription para a Alemanha da Alemanha

Dados de texto carregados para o serviço de reconhecimento de voz personalizadas só devem utilizar **(incl. LM) de codificação UTF-8**. Cada linha do ficheiro deve conter o texto de um único utterance apenas.

### <a name="text-normalization-performed-by-the-custom-speech-service"></a>Efetuada pelo serviço de reconhecimento de voz personalizadas de normalização de texto

Este serviço irá efetuar a normalização de texto seguinte nos dados importados como um conjunto de dados de idioma ou transcriptions para um conjunto de dados acústica. Isto inclui

*   Minúscula-maiúsculas e minúsculas todo o texto
*   Remover todos os pontuação incluindo em inglês ou alemão aspas ("teste", "test", "teste" ou «testar» é ok)
*   Eliminar qualquer linha que contém os carateres especiais, incluindo: ^ ¢ £ ¤ ¥ ¦ § © ª ¬® ° ± ² µ × ÿ Ø¬¬
*   Expansão dos números existentes para o formato do word, incluindo quantidades dólar ou euro
*   Podemos aceitar umlauts apenas para um, o u; outras pessoas serão substituídas por "ésimo" ou rejeitadas

Seguem-se alguns exemplos

| Texto original | Depois de normalização |
|----- | ----- |
| Frankfurter anel | anel frankfurter |
| "Hallo, Mama!" sagt die Tochter. | hallo mama sagt die tochter |
| ¡Eine Frage! | eine frage |
| wir, haben | wir haben |
| Das macht $10 | das macht utilizados no compromisso zehn |


### <a name="text-normalization-required-by-users"></a>Necessário para os utilizadores de normalização de texto

Para garantir a melhor utilização dos seus dados, as seguintes regras de normalização devem ser aplicadas aos seus dados antes de importá-lo.

*   Deve ser a vírgula decimal e não. Por exemplo, 2,3% e não 2.3%
*   Separador de tempo entre as horas e minutos deve ser: e não., por exemplo, 12:00 Uhr
*   Abreviaturas, tal como 'ca.', 'bzw'. não são substituídas. Recomendamos que utilize o formato completo para ter a pronunciação correta.
*   Os operadores de matemática principais cinco são removidos: +, -, \*, /.
 Recomendamos que para os substituir pelo respetivo formulário literal plus, a menos que o soft, geteilt.
*   Mesmo se aplica para os comparators (=, <>,) - gleich, kleiner als, grösser als
*   Utilizar fração como 3/4 em formato word 'drei viertel' em vez de ¾
*   Substitua o símbolo de € com o formato do word "Euro"


Seguem-se alguns exemplos

| Texto original | Depois de normalização do utilizador | Depois de normalização de sistema
|--------  | ----- | -------- |
| Es ist 12.23Uhr | Es ist 12:23Uhr | es ist zwölf uhr drei und zwanzig uhr |
| {12.45} | {12,45} | zwölf komma vier fünf |
| 3 < 5 | 3 kleiner als 5 | vier de als drei kleiner |
| 2 + 3 4 | 2 e 3 menos 4 | zwei plus drei menos vier|
| Das macht 12€ | Das macht Euros 12 | das macht zwölf euros |



## <a name="next-steps"></a>Passos Seguintes
* [Como utilizar um ponto final de reconhecimento de voz para texto personalizado](cognitive-services-custom-speech-create-endpoint.md)
* Melhorar a precisão com seu [modelo acústica personalizado](cognitive-services-custom-speech-create-acoustic-model.md)
* Melhorar a precisão com um [modelo idioma personalizado](cognitive-services-custom-speech-create-language-model.md)
