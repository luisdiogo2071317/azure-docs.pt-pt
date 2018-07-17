---
title: Diretrizes de transcrição para formação de voz | Documentos da Microsoft
description: Saiba como preparar o texto para personalizar acústico e modelos de linguagem e tipos de voz para o serviço de voz.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: PanosPeriorellis
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/01/2018
ms.author: panosper
ms.openlocfilehash: 6043d01e26d9f28f90e45c2cc194863b3a9cd017
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39072287"
---
# <a name="transcription-guidelines-for-using-speech-service"></a>Diretrizes de transcrição para utilizar o serviço de voz

Para personalizar **conversão de voz em texto** ou **texto em voz**, tem de fornecer o texto, juntamente com a conversão de voz. Cada linha no texto corresponde a uma única expressão. O texto deve corresponder a voz como exatamente quanto possível. O texto é chamado um *transcrição*, e deverá criá-la num formato específico.

O serviço de voz normaliza a entrada para manter o texto consistente. 

Este artigo descreve os dois tipos de normalizations. As diretrizes são ligeiramente diferentes para vários idiomas.

## <a name="us-english-en-us"></a>Inglês dos Estados Unidos (en-US)

Dados de texto devem ser escritos uma expressão por linha, em texto simples, usando apenas o conjunto de carateres ASCII.

Evite o uso de expandida (Latim 1) ou carateres de pontuação do Unicode. Esses caracteres podem ser incluídos inadvertidamente quando preparar os dados num programa de processamento de texto ou a captura de dados a partir de páginas da web. Substitua estes carateres com as substituições apropriadas das ASCII. Por exemplo:

| Carateres para evitar | Substituição |
|----- | ----- |
| "Hello world" (abrir e fechar as aspas duplas) | "Hello world" (as aspas duplas) |
| Dia de John (à direita plica) | Dia de John (apóstrofe) |
| isso foi bom — não, ele era ótimo! (em dash) | isso foi bom, não, era ótimo! (hífenes) |

### <a name="text-normalization-rules-for-english"></a>Regras de normalização do texto para inglês

O serviço de voz realiza as seguintes regras de normalização.

*   Inferior-casing todo o texto
*   Remover todos os pontuação, exceto apóstrofos internas do word
*   Expansão de números ao formulário falado, incluindo quantidades de dólar

Aqui estão alguns exemplos

| Texto original | Depois de normalização |
|----- | ----- |
| "" Caramba "!" Batman disse. | "caramba" disse batman |
| "E"? sidekick disse Batman, Round Robin. | round robin de sidekick que disse batman |
| Vá obter - em! | Aceda get em |
| Eu sou double-jointed | Eu sou double jointed |
| 104 elm Rua | um AH quatro Elm Rua |
| Ajustar às 102.7 | Otimizar para um AH dois sete |
| Instalador de plataforma está prestes a 3.14 | Instalador de plataforma é aproximadamente três quatro de um de ponto |
| Custa us $3.14 | custa três quatorze |

Aplicam-se a normalização seguinte para suas transcrições de texto.

*   Abreviações devem ser escritas em palavras
*   Cadeias de caracteres numéricas não padrão (por exemplo, alguns data ou formulários de contabilidade) devem ser escritas em palavras
*   Palavras com carateres não alfabéticos ou caracteres mistos de alfanuméricos devem ser transcrito tão pronunciados
*   Deixe abreviaturas pronunciadas como palavras inalteradas. Por exemplo, planos, laser, RAM, NATO.
*   Escreva abreviaturas pronunciadas como letras separadas, com letras separadas por espaços. Por exemplo, IBM, CPU, FBI, TBD, NaN. 

Eis alguns exemplos:

| Texto original | Depois de normalização |
|----- | ----- |
| 14 NE 3rd Dr. | unidade Nordeste da terceira quatorze |
| Dr. Bruce faixa | O doutor Bruce faixa |
| James Bond, 007 | James Bond, faça duplo AH sete |
| KE$ ha | Kesha |
| Quanto tempo dura 2 x 4 | Quanto tempo dura a duas por quatro |
| A reunião avança a partir de 1-3 pm | A reunião avança entre uma a três pm |
| é o meu tipo de sangue S + | Meu tipo de sangue será O positivo |
| água é H20 | água é O de 2 de H |
| reproduzir OU812 através de Van Halen | reproduzir O U 2 de 1 de 8 através de Van Halen |
| UTF-8 com BOM | 8 de F T U com BOM |

## <a name="chinese-zh-cn"></a>Chinês (zh-CN)

Dados de texto carregados para o serviço de voz personalizada devem usar a codificação UTF-8 com o marcador de ordem de byte. O ficheiro deve ser escrito de uma expressão por linha.

Evite a utilização de carateres de pontuação de meia largura. Esses caracteres podem ser incluídos inadvertidamente quando preparar os dados num programa de processamento de texto ou a captura de dados a partir de páginas da web. Substituí-los com as substituições apropriadas das largura total. Por exemplo:

| Carateres para evitar | Substituição |
|----- | ----- |
| "你好" (open e close aspas duplas) | "你好" (aspas duplas) |
| 需要什么帮助? (ponto de interrogação) | 需要什么帮助? |

### <a name="text-normalization-rules-for-chinese"></a>Regras de normalização do texto para chinês

O serviço de voz realiza as seguintes regras de normalização.

*   Remover todos os pontuação
*   Expandir números ao formulário falado
*   Conversão de letras de largura total para as letras de meia largura
*   Canto superior-casing all inglês palavras

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

Aplicam-se a normalização seguinte para o seu texto antes de importá-lo.

*   Abreviações devem ser escritas em palavras (como no formulário falado)
*   Escreva cadeias de caracteres numérica no formato falado.

Eis alguns exemplos.

| Texto original | Depois de normalização |
|----- | ----- |
| 我今年21 | 我今年二十一 |
| 3号楼504 | 三号 楼 五 零 四 |

## <a name="other-languages"></a>Outros idiomas

Texto de dados carregados para o **conversão de voz em texto** serviço deve usar a codificação UTF-8 com o marcador de ordem de byte. O ficheiro deve ser escrito de uma expressão por linha.

> [!NOTE]
> Estes exemplos utilizam alemão. No entanto, essas diretrizes se aplicam a todos os idiomas que não sejam inglês Americano ou chinês.

### <a name="text-normalization-rules-for-german"></a>Regras de normalização do texto para o alemão

O serviço de voz realiza as seguintes regras de normalização.

*   Inferior-casing todo o texto
*   Remover todos os pontuação, incluindo vários tipos de aspas ("teste", "teste", "teste" ou «testar» é ok)
*   Linhas com qualquer caráter especial de ¢ o conjunto de descarte ¤ ¥ ¦ § © ª ¬® ° ± ² nossa × ÿ Ø¬¬
*   Expansão de números a forma de palavra, incluindo as quantidades de dólar ou euro
*   Os tremas são aceites apenas para um, o u; outras pessoas serão substituídas por "th" ou eliminadas

Aqui estão alguns exemplos

| Texto original | Depois de normalização |
|----- | ----- |
| Anel de Frankfurter | anel de frankfurter |
| ¡Eine Frage! | eine frage |
| wir, haben | wir haben |

Aplicam-se a normalização seguinte para o seu texto antes de importá-lo.

*   Deve ser o ponto decimal ","e não"."
*   Separador de tempo entre horas e minutos deve ser ":"e não".": 12:00 Uhr
*   Abreviações como 'ca'. não são substituídos. Recomendamos que utilize o formulário completo.
*   Os operadores de matemáticos cinco principais são removidos: +, -, \*, /. Recomendamos que substituí-los com o formato de literal: Além disso, a menos que, geteilt.
*   Mesmo se aplica aos operadores de comparação (=, <>,) – gleich, kleiner als, grösser als
*   Frações de uso, como 3/4, na forma de palavra (por exemplo, "viertel drei" em vez de ¾)
*   Substituir o símbolo ' € com o formulário do word "Euro"

Eis alguns exemplos.

| Texto original | Depois de normalização do utilizador | Depois de normalização de sistema
|--------  | ----- | -------- |
| Es ist 12.23Uhr | Es ist 12:23Uhr | es ist zwölf uhr drei und zwanzig uhr |
| {12.45} | {12,45} | zwölf komma vier fünf ||
| 2 + 3 a 4 | 2 e 3 menos 4 | zwei plus drei menos vier|

## <a name="next-steps"></a>Passos Seguintes

- [Obter a subscrição de avaliação de Voz](https://azure.microsoft.com/try/cognitive-services/)
- [Reconhecer a conversão de voz em c#](quickstart-csharp-dotnet-windows.md)
