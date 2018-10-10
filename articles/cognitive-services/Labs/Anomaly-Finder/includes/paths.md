---
title: incluir ficheiro
description: incluir ficheiro
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.component: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: fb02496d9c107a2c21acca6c65ef69fdfceb4597
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/09/2018
ms.locfileid: "48904442"
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
|**Corpo**|**Corpo**  <br>*Necessário*|Série de tempo os pontos de dados e o período, se for necessário.|[Pedido](#request)|


#### <a name="responses"></a>Respostas

|Código de HTTP|Descrição|Esquema|
|---|---|---|
|**200**|Operação concluída com êxito.|< [resposta](#response) > matriz|
|**400**|Não é possível analisar o pedido do JSON.|Sem Conteúdo|
|**403**|O certificado que forneceu não é aceite pelo servidor.|Sem Conteúdo|
|**405**|Método não permitido.|Sem Conteúdo|
|**500**|Erro Interno do Servidor.|Sem Conteúdo|


#### <a name="consumes"></a>Consome

* `application/json`


#### <a name="produces"></a>Produz

* `application/json`


#### <a name="tags"></a>Etiquetas

* anomalydetection



