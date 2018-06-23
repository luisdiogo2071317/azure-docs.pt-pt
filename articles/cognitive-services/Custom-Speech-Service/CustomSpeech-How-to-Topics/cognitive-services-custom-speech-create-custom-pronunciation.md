---
title: Utilizar pronunciação personalizada com o serviço de reconhecimento de voz personalizadas no Azure | Microsoft Docs
description: Saiba como criar um modelo de idioma com o serviço de reconhecimento de voz personalizadas nos serviços cognitivos.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 11/23/2017
ms.author: panosper
ms.openlocfilehash: a74b69b84cc80809a25f18b580a18abb5721b8b1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351685"
---
# <a name="enable-custom-pronunciation"></a>Ativar pronunciação personalizada
Pronunciação personalizada permite aos utilizadores definir o formulário fonético e apresentação de uma palavra ou termo. É útil para processar os termos de personalizados, tais como nomes de produto ou a utilização de acrónimos. Tudo o que precisa é um ficheiro de pronunciação (um ficheiro. txt simples).

Eis como funciona. Num ficheiro. txt único, pode introduzir várias entradas de pronunciação personalizado. A estrutura é o seguinte:

```
Display form <Tab> Spoken form <Newline>
```

*Exemplos*:

| Formato de apresentação | Formulário ditas |
|----------|-------|
| C3PO | Consulte pea três Nã |
| L8R | são tarde |
| CNTK | Consulte n tea k|

## <a name="requirements-for-the-spoken-form"></a>Requisitos para o formulário ditas
O formulário ditas tem de ser em minúsculas, que pode ser forçada durante a importação. Além disso, tem de fornecer as verificações o importador de dados. Não existem separador do formulário ditas ou o formato de apresentação é permitida. Não existe pode, no entanto, mais ser proibido carateres no formato de apresentação (por exemplo, ~ e ^).

Cada ficheiro. txt pode ter várias entradas. Por exemplo, consulte a captura de ecrã seguinte:

![Captura de ecrã do bloco de notas com várias entradas para pronunciação acrónimo](../../../media/cognitive-services/custom-speech-service/custom-speech-pronunciation-file.png)

O formulário ditas está a sequência fonético o formato de apresentação. Este é composto por letras, palavras ou sílabas. Atualmente, não há nenhum orientações adicionais ou um conjunto de normas para o ajudar a formular a forma ditas. 

## <a name="supported-pronunciation-characters"></a>Pronunciação suportados carateres
Pronunciação personalizada é atualmente suportada para inglês (en-US) e alemão (Alemanha-). O conjunto de carateres que pode ser utilizado para expressar a forma ditas de um termo (no ficheiro de pronunciação personalizado) é apresentado na seguinte tabela: 

| Idioma | Carateres |
|---------- |----------|
| Inglês (en-US) | a, b, c, d, i, f, g, h, i, j, k, l, Nã, p, q, r, s, t, u, v, m, x, y, z |
| Alemão (Alemanha) | ä, ö, ü, ẞ, a, b, c, d, i, f, g, h, i, j, k, l, Nã, p, q, r, s, t, u, v, m, x, y, z |

>[NOTA] Formato de apresentação de um termo (num ficheiro pronunciação) deve ser escrito da mesma forma de um conjunto de dados de adaptation de idioma.

## <a name="requirements-for-the-display-form"></a>Requisitos para o formato de apresentação
Um formulário de visualização só pode ser uma palavra personalizada, termo, acrónimo ou compostas palavras que combinam palavras existentes. Também pode introduzir pronunciations alternativos para palavras comuns. 

>[!NOTE]
Não é recomendada a utilizar esta funcionalidade para reformulate palavras comuns ou modificar o formulário ditas. É melhor executar o descodificador para ver se alguns palavras invulgares (por exemplo, abreviaturas, palavras técnicas e palavras externas) não estão corretamente descodificar. Se forem, adicioná-los para o ficheiro de pronunciação personalizado. No modelo de linguagem, deve sempre e apenas utilizar o formato de apresentação de uma palavra. 

## <a name="requirements-for-the-file-size"></a>Requisitos para o tamanho do ficheiro
O tamanho do ficheiro. txt que contenha as entradas de pronunciação está limitado a 1 MB. Normalmente, não terá de carregar grandes quantidades de dados através deste ficheiro. A maioria dos ficheiros de pronunciação personalizado são provável que haja apenas alguns KBs de tamanho. A codificação de ficheiro. txt para todas as regiões deve ser LM UTF-8. Para a região em inglês, ANSI também é aceitável.

## <a name="next-steps"></a>Passos Seguintes
* Melhorar a exatidão do reconhecimento através da criação do [personalizado modelo acústica](cognitive-services-custom-speech-create-acoustic-model.md).
* [Criar um ponto final de reconhecimento de voz para texto personalizado](cognitive-services-custom-speech-create-endpoint.md), que pode utilizar a partir de uma aplicação.
