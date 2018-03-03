---
title: "Referência do motor de regras do Azure CDN | Microsoft Docs"
description: "Documentação de referência para a CDN do Azure regras de funcionalidades e as condições de correspondência do motor."
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: 602b4303dd1940791c11b8b71ac6a27f0474a6d5
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/02/2018
---
# <a name="azure-cdn-rules-engine-reference"></a>Referência do motor de regras CDN do Azure
Este artigo apresenta uma lista de descrições detalhadas das condições de correspondência disponíveis e funcionalidades de rede do Azure da entrega de conteúdos (CDN) [motor de regras](cdn-rules-engine.md).

O motor de regras foi concebido para ser a autoridade final em específicos como tipos de pedidos processados pela CDN.

**Utilizações comuns**:

- Substituição ou definir uma política de cache personalizada.
- Proteger ou negar pedidos para conteúdo confidencial.
- Redirecionar pedidos.
- Armazenar dados de registo personalizado.

## <a name="terminology"></a>Terminologia
Uma regra está definida através da utilização de [ **expressões condicionais**](cdn-rules-engine-reference-conditional-expressions.md), [ **corresponder condições**](cdn-rules-engine-reference-match-conditions.md), e [ **funcionalidades**](cdn-rules-engine-reference-features.md). Estes elementos são realçados na ilustração seguinte:

 ![Condição de correspondência CDN](./media/cdn-rules-engine-reference/cdn-rules-engine-terminology.png)

## <a name="syntax"></a>Sintaxe

Forma no qual os carateres especiais são tratados varia de acordo com a forma como uma condição de correspondência ou funcionalidade processa os valores de texto. Uma condição de correspondência ou a funcionalidade pode interpretar o texto de uma das seguintes formas:

1. [**Valores literais**](#literal-values) 
2. [**Valores de caráter universal**](#wildcard-values)
3. [**Expressões regulares**](#regular-expressions)

### <a name="literal-values"></a>Valores literais
Texto que é interpretado como um valor literal trata todos os carateres especiais, à exceção do símbolo %, como parte do valor deve ser correspondido. Por outras palavras, um literal corresponde à condição definida como `\'*'\` for satisfeita apenas quando exacta que o valor (ou seja, `\'*'\`) foi encontrado.
 
Um símbolo de percentagem é utilizado para indicar a codificação do URL (por exemplo, `%20`).

### <a name="wildcard-values"></a>Valores de caráter universal
Texto que é interpretado como um valor de caráter universal atribui significado adicional para carateres especiais. A tabela seguinte descreve a forma como o seguinte conjunto de carateres é interpretado:

Caráter | Descrição
----------|------------
\ | Uma barra invertida é utilizada para o escape qualquer um dos carateres especificados nesta tabela. É necessário especificar uma barra invertida diretamente antes do caráter especial que deve ser caráter de escape correto.<br/>Por exemplo, a seguinte sintaxe escapes um asterisco: `\*`
% | Um símbolo de percentagem é utilizado para indicar a codificação do URL (por exemplo, `%20`).
* | Um asterisco é um caráter universal que representa um ou mais carateres.
Espaço | Um caráter de espaço indica que uma condição de correspondência pode ser satisfeita pelo qualquer um dos valores especificados ou padrões.
'value' | Uma aspa simples não têm um significado especial. No entanto, um conjunto de plicas é utilizado para indicar que um valor deverá ser tratado como um valor literal. Podem ser utilizado das seguintes formas:<br><br/>-Permite que uma condição de correspondência ser satisfeito sempre que o valor especificado corresponde a qualquer parte do valor de comparação.  Por exemplo, `'ma'` corresponderia a qualquer uma das seguintes cadeias: <br/><br/>/business/**ma**rathon/asset.htm<br/>**ma**p.gif<br/>/business/template.**ma**p<br /><br />-Permite que um caráter especial especificado como um caráter literal. Por exemplo, pode especificar um caráter de espaço literal por envolvente um caráter de espaço dentro de um conjunto de plicas (ou seja, `' '` ou `'sample value'`).<br/>-Permite que um valor em branco ser especificado. Especifique um valor em branco, especificando um conjunto de plicas (ou seja, ').<br /><br/>**Importante:**<br/>-Se o valor especificado não contém um caráter universal, em seguida, considera-se automaticamente um valor literal, o que significa que não é necessário especificar um conjunto de plicas.<br/>-Se uma barra invertida não outro como carácter de escape nesta tabela, é ignorada, se for especificado dentro de um conjunto de plicas.<br/>-Outra forma de especificar um caráter especial, como um caráter literal é para o escape-la utilizando uma barra invertida (ou seja, `\`).

### <a name="regular-expressions"></a>Expressões regulares

As expressões regulares definem um padrão que é procurado dentro de um valor de texto. A notação de expressão regular define significados específicos para uma variedade de símbolos. A tabela seguinte indica os carateres especiais como são tratadas pelo correspondência condições e funcionalidades que suportam expressões regulares.

Caráter especial | Descrição
------------------|------------
\ | Uma barra invertida escapes o caráter de forma departamento de TI que faz com que esse caráter deve ser tratado como um valor literal em vez de demorar no significado da expressão regular. Por exemplo, a seguinte sintaxe escapes um asterisco: `\*`
% | O significado de um símbolo de percentagem depende da sua utilização.<br/><br/> `%{HTTPVariable}`: Este sintaxe identifica uma variável HTTP.<br/>`%{HTTPVariable%Pattern}`: Este sintaxe utiliza um símbolo de percentagem, para identificar um HTTP variável e como um delimitador.<br />`\%`: Escape um símbolo de percentagem permite a ser utilizado como um valor literal ou para indicar a codificação do URL (por exemplo, `\%20`).
* | Um asterisco permite que o carácter anterior a ser correspondido zero ou mais vezes. 
Espaço | Um caráter de espaço, normalmente, é tratado como um caráter literal. 
'value' | Plicas são tratadas como literais carateres. Um conjunto de plicas não têm um significado especial.


## <a name="next-steps"></a>Passos Seguintes
* [Condições de correspondência do motor de regras](cdn-rules-engine-reference-match-conditions.md)
* [Motor de regras de expressões condicionais](cdn-rules-engine-reference-conditional-expressions.md)
* [Funcionalidades do motor de regras](cdn-rules-engine-reference-features.md)
* [Comportamento HTTP utilizando o motor de regras de substituição](cdn-rules-engine.md)
* [Descrição geral CDN do Azure](cdn-overview.md)
