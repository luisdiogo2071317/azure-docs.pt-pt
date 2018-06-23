---
title: Analisador de nomenclatura estrutura na API Analysis linguístico | Microsoft Docs
description: Saiba como a API de análise linguístico utiliza a respetiva estrutura de nomenclatura para analisadores para permitir flexibilidade e precisão.
services: cognitive-services
author: RichardSunMS
manager: wkwok
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: article
ms.date: 03/23/2016
ms.author: lesun
ms.openlocfilehash: 2729b7126e82862660fc8e1a995cc87ae996ea03
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351571"
---
# <a name="analyzer-names"></a>Nomes de analisador

Utilizamos uma estrutura de nomenclatura um pouco mais complicada para analisadores para permitir que os dois flexibilidade em analisadores e a precisão em compreender o que significa um nome.
Os nomes de analisador consistem em quatro partes: um ID, um tipo, uma especificação e uma implementação.
A função de cada componente está definida abaixo.

## <a name="id"></a>ID
Em primeiro lugar, um analisador tem um ID exclusivo; um GUID.
Estes GUIDs devem alterar relativamente raramente, mas estão a única forma de descrever exclusivamente um analisador específico.

## <a name="kind"></a>Variante
Em seguida, é o analisador de cada um **tipo**.
Isto define termos muito abrangente devolveu o tipo de análise e exclusivamente deve definir a estrutura de dados utilizada para representar essa análise.
Atualmente, existem três tipos diferentes:
 - [Tokens](Sentences-and-Tokens.md)
 - [POS etiquetas](Pos-Tagging.md)
 - [Árvore de constituency](constituency-parsing.md)

## <a name="specification"></a>especificação
Dentro de um determinado tipo, no entanto, especialistas diferentes poderão disagree na forma como um determinado phenomenon deve ser analisada.
Ao contrário de linguagens de programação, há sem uma definição clara e exata de como isto deve ser feito.

Por exemplo, imagine que foram a tentar localizar os tokens no frase inglês "Ele não aceda."
Em particular, considere a cadeia "não".
Uma interpretação possíveis é que este deve ser dividida em duas tokens: "foi" e "not".
Em seguida, o frase alternativo "ele não ir" teria o mesmo conjunto de tokens.
Possibilidade de outra é dizer que deve ser dividido nos tokens "não" e "n't".
O token anterior será ignorado seria não normalmente ser considerado uma palavra, mas esta abordagem mantém mais informações sobre a cadeia superfície, o que por vezes, pode ser útil.
Ou, talvez esse redução deve ser considerada uma única palavra.

Independentemente disso opção é efetuada que escolha deve ser efetuada de forma consistente.
Esta é precisamente a função de um **especificação**: para decidir o que deve ser uma representação correta.

Só é possível comparar bastante saídas de analisador para os dados que está em conformidade com a especificação do mesma.

## <a name="implementation"></a>Implementação

Muitas vezes, existem vários modelos que tentam alcançar os mesmos resultados, mas com as características de desempenho diferentes.
Um modelo pode ser mais rápido apesar menos exato; outro poderá tornar um compromisso diferentes.

O **implementação** parte de um nome de analisador é utilizado para identificar este tipo de informações, para que os utilizadores podem escolher o analisador de mais adequado para as suas necessidades.
