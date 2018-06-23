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
ms.openlocfilehash: ff36202b67f6262b7ba67fe48ef37f2b656b91fa
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353353"
---
<a name="definitions"></a>
## <a name="definitions"></a>Definições

<a name="point"></a>
### <a name="point"></a>Ponto de

|Nome|Descrição|Esquema|
|---|---|---|
|**Timestamp**  <br>*Opcional*|O carimbo para o ponto de dados. Certifique-se de que está alinhada com a meia-noite e utilize uma UTC data hora da cadeia, por exemplo, 2017-08-01T00:00:00Z.|cadeia (data / hora)|
|**Valor**  <br>*Opcional*|Um valor de medida de dados.|número (double)|


<a name="request"></a>
### <a name="request"></a>Pedir

|Nome|Descrição|Esquema|
|---|---|---|
|**Período**  <br>*Opcional*|O período de pontos de dados. Se o valor é nulo ou não estão presentes, a API irá determinar o período de automaticamente.|número (double)|
|**Pontos**  <br>*Opcional*|Pontos de dados de séries de tempo. Os dados devem ser ordenados por timestamp ascendente para fazer corresponder o resultado de anomalias. Se os dados não estão ordenados corretamente ou não existe timestamp duplicado, a API irá detetar os pontos de anomalias corretamente, mas não foi possível corresponder bem pontos devolvidos com a entrada. Esse caso, será adicionada uma mensagem de aviso na resposta.|< [ponto](#point) > matriz|


<a name="response"></a>
### <a name="response"></a>Resposta

|Nome|Descrição|Esquema|
|---|---|---|
|**ExpectedValues**  <br>*Opcional*|O valor previsto pelo learning baseada em modelo. Se os pontos de dados de entrada estão ordenados por timestamp ascendente, o índice da matriz pode ser utilizado para mapear o valor original e o valor esperado.|matriz de < número (double) >|
|**IsAnomaly**  <br>*Opcional*|O resultado indica se os pontos de dados estiverem anomalias ou não em ambas as direções (picos ou dips). TRUE significa que o ponto de anomalias, false significa que o ponto não anomalias. Se os pontos de dados de entrada estão ordenados por timestamp ascendente, o índice da matriz pode ser utilizado para mapear o valor original e o valor esperado.|matriz de < booleano >|
|**IsAnomaly_Neg**  <br>*Opcional*|O resultado indica se os pontos de dados estiverem anomalias na direção negativa (dips). TRUE significa que a direção da anomalias é negativa. Se os pontos de dados de entrada estão ordenados por timestamp ascendente, o índice da matriz pode ser utilizado para mapear o valor original e o valor esperado.|matriz de < booleano >|
|**IsAnomaly_Pos**  <br>*Opcional*|O resultado indica se os pontos de dados estiverem anomalias na direção positiva (picos). TRUE significa que a direção da anomalias positiva. Se os pontos de dados de entrada estão ordenados por timestamp ascendente, o índice da matriz pode ser utilizado para mapear o valor esperado e original.|matriz de < booleano >|
|**LowerMargin**  <br>*Opcional*|(ExpectedValue - LowerMargin) determina o limite inferior que o ponto de dados é ainda considerar como normal. Se os pontos de dados de entrada estão ordenados por timestamp ascendente, o índice da matriz pode ser utilizado para mapear o valor original e o valor esperado.|matriz de < número (double) >|
|**Período**  <br>*Opcional*|O período de que a API utilizada para detetar os pontos de anomalias.|número (número de vírgula flutuante)|
|**UpperMargin**  <br>*Opcional*|A soma de ExpectedValue e UpperMargin determina o limite superior que o ponto de dados é ainda considerar como normal. Se os pontos de dados de entrada estão ordenados por timestamp ascendente, o índice da matriz pode ser utilizado para mapear o valor original e o valor esperado.|matriz de < número (double) >|
|**WarningText**  <br>*Opcional*|Se os pontos de dados de entrada fornecidos não estão a seguir a regra que requer a API e os dados ainda podem ser detetados pela API, a API irá analisar os dados e de acréscimo as informações de aviso neste campo.|cadeia|



