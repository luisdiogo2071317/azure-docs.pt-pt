---
title: Diretrizes de transcription para formação de reconhecimento de voz | Microsoft Docs
description: Saiba como preparar o texto a personalizar acústica e modelos de idioma e tipos de letra de voz para o serviço de reconhecimento de voz.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 93ab7c81a773f692b2b970bb1901d82b7aceb5a2
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "35356152"
---
# <a name="transcription-guidelines-for-using-speech-service"></a>Diretrizes de transcription para utilizar o serviço de reconhecimento de voz

Para personalizar **reconhecimento de voz para texto** ou **reconhecimento de voz do texto para**, tem de fornecer o texto, juntamente com o reconhecimento de voz. Cada linha no texto corresponde a um único utterance. O texto é chamado um *transcript*, e tem de criá-lo num formato específico.

O serviço de reconhecimento de voz efetua algumas normalizations por si para tornar o seu texto consistente. Outras tarefas de normalização têm de ser executadas antes do texto foi submetido para formação. 

Este artigo descreve os dois tipos de normalizations. As diretrizes variam ligeiramente para vários idiomas.

## <a name="us-english-en-us"></a>Inglês dos Estados Unidos (en-US)

Dados de texto carregados para este serviço devem ser escritos em texto simples utilizando apenas a ASCII conjunto de carateres. Cada linha do ficheiro deve conter o texto de um único utterance.

É importante evitar a utilização de expandida (Latim-1) ou carateres de pontuação de Unicode. Estes carateres podem ser incluídos inadvertidamente quando preparar os dados de um programa word-processing ou scraping dados a partir de páginas web. Substitua estes carateres substituições de ASCII adequadas. Por exemplo:

| Carateres para evitar | Substituição |
|----- | ----- |
| "Olá mundo" (abrir e fechar aspas duplas) | "Olá mundo" (aspas) |
| Dia de João (à direita plica) | Dia de João (apóstrofo) |
| foi boa — não, foi excelente! (dash IT) | foi boa – não, foi excelente! (hífenes) |

### <a name="text-normalization-performed-by-the-service"></a>Efetuada pelo serviço de normalização de texto

O serviço de reconhecimento de voz efetua a normalização de texto seguinte transcrições de texto.

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

### <a name="text-normalization-you-must-perform"></a>Tem de efetuar de normalização de texto

Aplica a normalização seguinte à sua transcrições de texto.

*   Abreviaturas devem ser escritas na palavras para refletir o formulário ditas
*   Não padrão cadeias numéricas (por exemplo, alguns data ou formulários de contabilidade) devem ser escritas no palavras
*   Devem ser transcribed palavras com carateres alfanuméricos mistos ou carateres não alfabéticos como pronunciados
*   Deixe abreviaturas pronuncia-se como palavras inalteradas. Por exemplo, radar, laser, RAM, NATO e Mr.
*   Escreva abreviaturas pronuncia-se como letras separadas, com letras separadas por espaços. Por exemplo, IBM, CPU, FBI, TBD, NaN. 

Eis alguns exemplos:

| Texto original | Depois de normalização |
|----- | ----- |
| 14 NE 3rd Dr. | unidade northeast terceiro fourteen |
| Dr. Strangelove | Doctor Strangelove |
| James Bond 007 | James Bond duplo esqueceu sete |
| KE$ ha | Kesha |
| Quanto tempo é 2, 4 | Quanto tempo é dois por quatro |
| A reunião fica de 1-3 pm | A reunião passa uma a três pm |
| o tipo do meu blood é Nã + | O tipo do meu blood é Nã positivo |
| máximo é H20 | máximo é O de 2 H |
| reproduzir OU812 por Van Halen | reproduzir O U 8 1 2 por Van Halen |
| UTF-8 com LM | U T F 8 com LM |

## <a name="chinese-zh-cn"></a>Chinês (zh-CN)

Dados de texto carregados para o serviço de reconhecimento de voz personalizadas devem utilizar a codificação UTF-8 com o marcador de ordem de bytes. Cada linha do ficheiro deve conter o texto de um único utterance.

É importante evitar a utilização de carateres de pontuação de largura do meio. Estes carateres podem ser incluídos inadvertidamente quando preparar os dados de um programa word-processing ou scraping dados a partir de páginas web. Substitua-as com substituições de largura de completo adequadas. Por exemplo:

| Carateres para evitar | Substituição |
|----- | ----- |
| "你好" (abrir e fechar aspas duplas) | "你好" (aspas duplas) |
| 需要什么帮助? (ponto de interrogação) | 需要什么帮助? |

### <a name="text-normalization-performed-by-the-service"></a>Efetuada pelo serviço de normalização de texto

O serviço de reconhecimento de voz efetua a normalização de texto seguinte transcrições de texto.

*   Remover todos os pontuação
*   Números de expansão para formar ditas
*   Converter letras largura completo ao meio-largura letras
*   Maiúscula-maiúsculas e minúsculas all inglês palavras

Eis alguns exemplos.

| Texto original | Depois de normalização |
|----- | ----- |
| 3.1415 | 三 点 一 四 一 五 |
| ￥3.5 | 三 元 五 角 |
| w f y z | W F Y Z |
| 1992年8月8日 | 一 九 九 二 年 八 月 八 日 |
| 你吃饭了吗? | 你 吃饭 了 吗 |
| 下午5:00的航班 | 下午 五点 的 航班 |
| 我今年21岁 | 我 今年 二十 一 岁 |

### <a name="text-normalization-you-must-perform"></a>Tem de efetuar de normalização de texto

Aplica a normalização seguinte para o texto antes de importá-lo.

*   Abreviaturas devem ser escritas na palavras para refletir o formulário ditas
*   Este serviço não abrange todas as quantidades numérico. É mais fiável para escrever um valor numérico cadeias de formato ditas.

Eis alguns exemplos.

| Texto original | Depois de normalização |
|----- | ----- |
| 我今年21 | 我今年二十一 |
| 3号楼504 | 三号 楼 五 零 四 |

## <a name="other-languages"></a>Outros idiomas

Transferência de dados de texto para o **reconhecimento de voz para texto** serviço tem de utilizar a codificação UTF-8 com o marcador de ordem de bytes. Cada linha do ficheiro deve conter o texto de um único utterance.

> [!NOTE]
> Estes exemplos utilizam alemão. No entanto, estas orientações são aplicáveis a todos os idiomas que não sejam inglês ou chinês.

### <a name="text-normalization-performed-by-the-service"></a>Efetuada pelo serviço de normalização de texto

O serviço de reconhecimento de voz efetua a normalização de texto seguinte transcrições de texto.

*   Minúscula-maiúsculas e minúsculas todo o texto
*   Remover todos os pontuação, incluindo vários tipos de aspas ("teste", "test", "teste" ou «testar» é ok)
*   Rejeitar qualquer linha que contém caracteres especiais do conjunto de ^ ¢ £ ¤ ¥ ¦ § © ª ¬® ° ± ² µ × ÿ Ø¬¬
*   Expansão dos números existentes para o formato do word, incluindo quantidades dólar ou euro
*   Umlauts são aceites apenas para um, o u; outras pessoas serão substituídas por "ésimo" ou eliminadas

Seguem-se alguns exemplos

| Texto original | Depois de normalização |
|----- | ----- |
| Frankfurter anel | anel frankfurter |
| "Hallo, Mama!" sagt die Tochter. | hallo mama sagt die tochter |
| ¡Eine Frage! | eine frage |
| wir, haben | wir haben |
| Das macht $10 | das macht utilizados no compromisso zehn |

### <a name="text-normalization-you-must-perform"></a>Tem de efetuar de normalização de texto

Aplica a normalização seguinte para o texto antes de importá-lo.

*   Deve ser a vírgula decimal ","e não".": 2,3% e não 2.3%
*   Separador de tempo entre as horas e minutos deve ser ":"e não".": 12:00 Uhr
*   Abreviaturas, tal como 'ca.', 'bzw'. não são substituídas. Recomendamos que utilize o formato completo.
*   Os operadores de matemática principais cinco são removidos: +, -, \*, /. Recomendamos substituir pelo respetivo formulário literal: plus, menos que o soft, geteilt.
*   Mesmo se aplica para operadores de comparação (=, <>,) - gleich, kleiner als, grösser als
*   Utilizar frações, tais como 3/4, em formato word (por exemplo, 'viertel drei' em vez de ¾)
*   Substitua o símbolo de € com o formato do word "Euro"

Eis alguns exemplos.

| Texto original | Depois de normalização do utilizador | Depois de normalização de sistema
|--------  | ----- | -------- |
| Es ist 12.23Uhr | Es ist 12:23Uhr | es ist zwölf uhr drei und zwanzig uhr |
| {12.45} | {12,45} | zwölf komma vier fünf |
| 3 < 5 | 3 kleiner als 5 | vier de als drei kleiner |
| 2 + 3 4 | 2 e 3 menos 4 | zwei plus drei menos vier|
| Das macht 12€ | Das macht Euros 12 | das macht zwölf euros |

## <a name="next-steps"></a>Passos Seguintes

- [Obter a sua subscrição de avaliação de reconhecimento de voz](https://azure.microsoft.com/try/cognitive-services/)
- [Reconhecer o reconhecimento de voz em c#](quickstart-csharp-windows.md)
