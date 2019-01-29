---
title: Método de semelhança - API de conhecimento académico
titlesuffix: Azure Cognitive Services
description: Utilize o método de semelhança para calcular a semelhança para instituições académicas cadeias de caracteres de dois.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 01/18/2017
ms.author: alch
ms.openlocfilehash: 7484b570784f5f058ebd23b1e3c225c5d858a274
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55183344"
---
# <a name="similarity-method"></a>Método de semelhança

O **semelhança** REST API é utilizada para calcular a semelhança académica entre duas cadeias de caracteres. 
<br>

**Ponto final REST:**
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/similarity?
```

## <a name="request-parameters"></a>Parâmetros do Pedido
Parâmetro        |Tipo de Dados      |Necessário | Descrição
----------|----------|----------|------------
**s1**        |Cadeia   |Sim  |Cadeia de caracteres * ser comparadas
**s2**        |Cadeia   |Sim  |Cadeia de caracteres * ser comparadas
<sub> * As cadeias de caracteres para comparar tem um comprimento máximo de 1MB. </sub>
<br>
## <a name="response"></a>Resposta
Nome | Descrição
--------|---------
**SimilarityScore**        |Um ponto flutuante valor que representa a semelhança do cosseno de s1 e s2, com valores mais próximo para 1.0 significado mais semelhante e os valores mais perto de -1,0, que significa menos
<br>

## <a name="successerror-conditions"></a>Condições de erros/com êxito
Estado de HTTP | Razão | Resposta
-----------|----------|--------
**200**         |Êxito | Número de vírgula flutuante
**400**         | Pedido incorreto ou a pedido inválido | Mensagem de erro      
**500**         |Erro de servidor interno | Mensagem de erro
**Atingiu o tempo limite**     | O pedido excedeu o limite de tempo.  | Mensagem de erro
<br>
## <a name="example-calculate-similarity-of-two-partial-abstracts"></a>Exemplo: Calcular a semelhança de dois resumos parciais
#### <a name="request"></a>Pedido:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/similarity?s1=Using complementary priors, we derive a fast greedy algorithm that can learn deep directed belief networks one layer at a time, provided the top two layers form an undirected associative memory
&s2=Deepneural nets with a large number of parameters are very powerful machine learning systems. However, overfitting is a serious problem in such networks
```
Neste exemplo, geramos a pontuação de semelhança entre dois resumos parciais com o **semelhança** API.
#### <a name="response"></a>Resposta:
```
0.520
```
#### <a name="remarks"></a>Observações:
A pontuação de semelhança é determinada pelo avaliar os conceitos académicos através de incorporação do word. Neste exemplo, 0.52 significa que os resumos de parciais dois são um pouco semelhantes.
<br>
