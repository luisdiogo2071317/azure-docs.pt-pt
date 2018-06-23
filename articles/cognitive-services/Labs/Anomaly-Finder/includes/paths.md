---
title: incluir ficheiro
description: incluir ficheiro
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: a806cac410eb57e59dacb42da9be954b2f962956
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353330"
---
<a name="paths"></a>
## <a name="paths"></a>Caminhos

<a name="anomalydetection-post"></a>
### <a name="detect-anomaly-points-for-the-time-series-data-points-requested"></a>Detetar pontos de anomalias para o tempo de pontos de dados de série pedido
```
POST /anomalydetection
```


#### <a name="parameters"></a>Parâmetros

|Tipo|Nome|Descrição|Esquema|
|---|---|---|---|
|**Corpo**|**Corpo**  <br>*Necessário*|A série de tempo pontos de dados e o período, se necessário.|[Pedido](#request)|


#### <a name="responses"></a>Respostas

|Código de HTTP|Descrição|Esquema|
|---|---|---|
|**200**|Operação concluída com êxito.|< [resposta](#response) > matriz|
|**400**|Não consegue analisar o pedido JSON.|Sem Conteúdo|
|**403**|O certificado que forneceu não é aceite pelo servidor.|Sem Conteúdo|
|**405**|Método não permitido.|Sem Conteúdo|
|**500**|Erro Interno do Servidor.|Sem Conteúdo|


#### <a name="consumes"></a>Consome

* `application/json`


#### <a name="produces"></a>Produz

* `application/json`


#### <a name="tags"></a>Etiquetas

* anomalydetection



