---
title: Método de semelhança na API de conhecimento académico | Microsoft Docs
description: Utilize o método de semelhança para calcular a semelhança académicas de duas cadeias nos serviços cognitivos da Microsoft.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 01/18/2017
ms.author: alch
ms.openlocfilehash: 472498d6bfe06ae4477a30f892d44e79c901acf5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351392"
---
# <a name="similarity-method"></a>Método de semelhança

O **semelhança** REST API é utilizada para calcular a semelhança académico entre duas cadeias. 
<br>

**Ponto final REST:**
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/similarity?
```

## <a name="request-parameters"></a>Parâmetros do Pedido
Parâmetro        |Tipo de Dados      |Necessário | Descrição
----------|----------|----------|------------
**S1**        |Cadeia   |Sim  |Cadeia * a ser comparada
**S2**        |Cadeia   |Sim  |Cadeia * a ser comparada
<sub> * Cadeias a comparar têm um comprimento de máximo de 1MB. </sub>
<br>
## <a name="response"></a>Resposta
Nome | Descrição
--------|---------
**SimilarityScore**        |Uma vírgula flutuante do ponto de valor que representa a semelhança de co-seno de s1 e s2, com os valores próximo 1.0 significado mais semelhante e valores próximo para-1.0 significado inferior
<br>

## <a name="successerror-conditions"></a>Condições de erros/com êxito
Estado de HTTP | Razão | Resposta
-----------|----------|--------
**200**         |Êxito | Número de vírgula flutuante
**400**         | Pedido incorreto ou a pedido inválido | Mensagem de erro      
**500**         |Erro de servidor interno | Mensagem de erro
**Foi excedido**     | O pedido excedeu o limite de tempo.  | Mensagem de erro
<br>
## <a name="example-calculate-similarity-of-two-partial-abstracts"></a>Exemplo: Calcular semelhança de dois abstracts parciais
#### <a name="request"></a>Pedido:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/similarity?s1=Using complementary priors, we derive a fast greedy algorithm that can learn deep directed belief networks one layer at a time, provided the top two layers form an undirected associative memory
&s2=Deepneural nets with a large number of parameters are very powerful machine learning systems. However, overfitting is a serious problem in such networks
```
Neste exemplo, vamos gerar a classificação de semelhança entre duas abstracts parciais utilizando o **semelhança** API.
#### <a name="response"></a>Resposta:
```
0.520
```
#### <a name="remarks"></a>Comentários:
A classificação de semelhança é determinada pelo avaliar os conceitos académicas através do word incorporar. Neste exemplo, 0.52 significa que os dois abstracts parciais são um pouco semelhantes.
<br>