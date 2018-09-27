---
title: Utilizar pronúncia personalizada - serviço de voz personalizada
titlesuffix: Azure Cognitive Services
description: Saiba como criar um modelo de idioma com o serviço de voz personalizada.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: conceptual
ms.date: 11/23/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: 6442eec3e622282ecf6a3bf884110cd435fc18be
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223479"
---
# <a name="enable-custom-pronunciation"></a>Ativar pronúncia personalizada
Pronúncia personalizada permite aos utilizadores definir o formulário fonético e a exibição de uma palavra ou o termo. É útil para lidar com os termos personalizados, tais como nomes de produto ou acrônimos. Tudo o que precisa é um ficheiro de pronúncia (um ficheiro. txt simples).

Eis como funciona. Num arquivo. txt único, pode inserir várias entradas de pronúncia personalizado. A estrutura é o seguinte:

```
Display form <Tab> Spoken form <Newline>
```

*Exemplos*:

| Formulário de apresentação | Formulário falado |
|----------|-------|
| C3PO | Veja três pea s |
| L8R | são mais tarde |
| CNTK | Veja o n chá k|

## <a name="requirements-for-the-spoken-form"></a>Requisitos para o formulário falado
O formulário falado tem de estar em minúsculas, que pode ser forçado durante a importação. Além disso, tem de fornecer verificações no importador de dados. Nenhum separador no formulário falado ou o formulário de apresentação é permitida. Aí pode, no entanto, mais ser proibido carateres no formulário de apresentação (por exemplo, ~ e ^).

Cada arquivo. txt pode ter várias entradas. Por exemplo, veja a captura de ecrã seguinte:

![Captura de ecrã do bloco de notas com várias entradas para pronúncia de acrônimos](../../../media/cognitive-services/custom-speech-service/custom-speech-pronunciation-file.png)

O formulário falado é a sequência de fonética do formulário de apresentação. Ele é composto por letras, palavras ou sílabas. Atualmente, não existe mais orientações ou conjunto de padrões para o ajudar a formular a forma falada. 

## <a name="supported-pronunciation-characters"></a>Pronúncia suportados carateres
Pronúncia personalizada é atualmente suportada para inglês (en-US) e alemão (Alemanha-de). O conjunto de caracteres que pode ser utilizado para expressar o formulário falado de um período (no arquivo pronúncia personalizado) é mostrado na tabela a seguir: 

| Idioma | Carateres |
|---------- |----------|
| Inglês (en-US) | a, b, c, d, e, f, g, h, i, j, k, l, s, p, p, r, s, t, u, v, w, x, y, z |
| Alemão (Alemanha-de) | ä, ö, ü, ẞ, a, b, c, d, e, f, g, h, i, j, k, l, s, p, p, r, s, t, u, v, w, x, y, z |

>[NOTA] Formulário de apresentação de um termo (num arquivo de pronúncia) deve ser escrito da mesma forma num conjunto de dados de adaptação de idioma.

## <a name="requirements-for-the-display-form"></a>Requisitos para o formulário de apresentação
Um formulário de apresentação só pode ser uma palavra personalizada, termo, acrônimos ou palavras compostas que combinam palavras existentes. Também pode introduzir pronunciations alternativas para palavras comuns. 

>[!NOTE]
Não é recomendável utilizar esta funcionalidade para reformulate palavras comuns ou para modificar o formato falado. É melhor executar o Decodificador para ver se algumas palavras pouco habitual (por exemplo, abreviaturas, palavras técnicas e palavras estrangeiras) não estão corretamente decodificadas. Se forem, adicioná-los para o ficheiro de pronúncia personalizado. No modelo de idioma, deve sempre e apenas usar o formulário de apresentação de uma palavra. 

## <a name="requirements-for-the-file-size"></a>Requisitos para o tamanho do ficheiro
O tamanho do ficheiro. txt que contenha as entradas de pronúncia está limitado a 1 MB. Normalmente, não é necessário carregar grandes quantidades de dados por meio deste ficheiro. A maioria dos arquivos de pronúncia personalizados são probabilidade de serem apenas alguns KBs de tamanho. A codificação do ficheiro. txt para todas as localidades deve ser o UTF-8 BOM. Para a Localidade do inglês, ANSI também é aceitável.

## <a name="next-steps"></a>Passos Seguintes
* Melhorar a precisão de reconhecimento através da criação de seus [um modelo acústico personalizado](cognitive-services-custom-speech-create-acoustic-model.md).
* [Criar um ponto de final de voz em texto personalizado](cognitive-services-custom-speech-create-endpoint.md), que pode utilizar a partir de uma aplicação.
