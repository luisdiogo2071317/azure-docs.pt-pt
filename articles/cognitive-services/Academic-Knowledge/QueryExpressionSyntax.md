---
title: Consultar a sintaxe de expressão na API de conhecimento académico | Microsoft Docs
description: Saiba como utilizar a sintaxe de expressão de consulta na API de conhecimento académico cognitivos nos serviços da Microsoft.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: 6ec338fff09954e2f14066ce2b83bc1228794af8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351434"
---
# <a name="query-expression-syntax"></a>Sintaxe de expressão de consulta

Podemos ter visto que a resposta a um **interpretar** pedido inclui uma expressão de consulta. A gramática interpretado consulta do utilizador criar uma expressão de consulta para cada interpretação. Uma expressão de consulta, em seguida, pode ser utilizada para emitir um **avaliar** pedido para obter os resultados da pesquisa de entidade.

Também pode construir as suas próprias expressões de consulta e utilizá-los num **avaliar** pedido. Isto pode ser útil se está a criar o seu próprio interface de utilizador que cria uma expressão de consulta em resposta a ações do utilizador. Para tal, terá de conhecer a sintaxe para expressões de consulta.  

Cada atributo de entidade que pode ser incluído numa expressão de consulta tem um tipo de dados específicos e um conjunto de operadores de consulta possíveis. O conjunto de atributos de entidade e operadores suportados para cada atributo especificado no [entidade atributos](EntityAttributes.md). Uma consulta de valor único requer o atributo para suportar o *é igual a* operação. Uma consulta de prefixo requer o atributo para suportar o *StartsWith* operação. Consultas de intervalo numérico requer o atributo para suportar o *IsBetween* operação.

Alguns dos dados de entidades são armazenados como atributos compostos, conforme indicado por um ponto '.' no nome do atributo. Por exemplo, as informações de autor/afiliação são representadas como um atributo composto. Contém 4 componentes: AuN, AuId, AfN, AfId. Estes componentes são peças separadas dos dados que formam um valor de atributo de entidade única.


**Atributo de cadeia: Único valor** (inclui correspondências contra sinónimos)  
TI = 'Indexação por latentes analysis semântica'  
Compostos (AA. AuN = 'sue dumais')

**Atributo de cadeia: Exacta valor único** (corresponde à apenas valores canónicos)  
TI = = 'Indexação por latentes analysis semântica'  
Compostos (AA. AuN = = 'susan t dumais')
     
**Atributo de cadeia: O valor de prefixo**   
TI = 'Indexação por seman latentes'...  
Compostos (AA. AuN = 'sue du'...)

**Atributo numérico: Valor único**  
Y = 2010
 
**Atributo numérico: O valor de intervalo**  
Y &GT; 2005  
Y &GT; = 2005  
Y &LT; 2010  
Y &LT; = 2010  
Y =\[2010, 2012\) (inclui o valor do limite esquerdo apenas: 2010, 2011)  
Y =\[2010, 2012\] (inclui os dois valores de limite: 2010, 2011, 2012)
 
**Atributo numérico: O valor de prefixo**  
Y = '19'... (qualquer valor numérico que começa com 19) 
 
**Atributo data: Valor único**  
D = "2010-02-04'

**Datas atributo: O valor de intervalo**  
D &GT; "2010-02-03'  
D = ["2010-02-03', ' 2010-02-05']

**E/ou consultas:**  
E (Y = 1985, Ti = 'Eletrónicos disordered sistemas operativos')  
Ou (Ti = 'disordered Eletrónicos sistemas operativos', Ti = 'princípios de tolerância a falhas e prática')  
And(or(Y=1985,Y=2008), Ti = 'Eletrónicos disordered sistemas operativos')
 
**Consultas compostas:**  
Componentes de consulta de um atributo composto, tem de incluir a parte da expressão de consulta que referencia o atributo na função Composite() composto. 

Por exemplo, para consultar papers pelo nome do autor, utilize a seguinte consulta:
```
Composite(AA.AuN='mike smith')
```
<br>Para consultar papers por um determinado autor enquanto o autor estava a uma instituição específica, utilize a seguinte consulta:
```
Composite(And(AA.AuN='mike smith',AA.AfN='harvard university'))
```
<br>A função de Composite() vincula duas partes do atributo composto em conjunto. Isto significa que apenas obtemos papers onde um dos autores é "Mike Santos" durante a ele na Harvard. 

Para consultar papers por um autor específico no políticas com (outros) autores de uma instituição específico, utilize a seguinte consulta:
```
And(Composite(AA.AuN='mike smith'),Composite(AA.AfN='harvard university'))
```
<br>Nesta versão, porque Composite() é aplicada ao autor e afiliação individualmente antes And(), vamos obter todos os papers onde um dos autores é "Mike Santos", não sendo uma das políticas dos autores "Harvard". Isto SOA semelhante ao anterior exemplo de consulta, mas não é a mesma coisa.

Em geral, considere o seguinte exemplo: temos um atributo composto C tem dois componentes A e B. Uma entidade pode ter vários valores para C. Estes são os nossas entidades:
```
E1: C={A=1, B=1}  C={A=1,B=2}  C={A=2,B=3}
E2: C={A=1, B=3}  C={A=3,B=2}
```

<br>A consulta 
```
Composite(And(C.A=1, C.B=2))
```

<br>corresponde à apenas as entidades que tenham um valor para C em que o componente C.A é 1 e o componente C.B é 2. Apenas E1 corresponde a esta consulta.

A consulta 
```
And(Composite(C.A=1), Composite(C.B=2))
```
<br>corresponde a entidades que tenham um valor para C onde C.A é 1 e também de ter um valor para C onde C.B é 2. E1 e E2 correspondem esta consulta.

Atenção:  
- Não é possível referenciar uma parte de um atributo composto fora de uma função de Composite().
- Não é possível referenciar partes dos dois atributos compostos diferentes dentro da mesma função Composite().
- Não é possível referenciar um atributo que não faz parte de um atributo no interior de uma função de Composite() composto.
