---
title: Modelo de consumo no Maps do Azure | Microsoft Docs
description: Saiba mais sobre o modelo de consumo no Maps do Azure
author: subbarayudukamma
ms.author: skamma
ms.date: 05/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: cf20c7dbfbf7cd3f09579b03b835148c1c295137
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34600634"
---
# <a name="consumption-model"></a>Modelo de consumo

Encaminhamento online fornece um conjunto de parâmetros para uma descrição detalhada do modelo de consumo vehicle específicos.
Dependendo do valor de **vehicleEngineType**, são suportados dois modelos de consumo principal: _Combustion_ e _Electric_. Especificar os parâmetros que pertencem a modelos diferentes no mesmo pedido é um erro.
Não é possível utilizar o modelo de consumo com **travelMode** valores _bicycle_ e _pedestrian_.

## <a name="parameter-constraints-for-consumption-model"></a>Restrições de parâmetro de modelo de consumo

Em ambos os modelos de consumo, especificar explicitamente alguns parâmetros requer a especificação de outros bem. Estas dependências são:

* Todos os parâmetros requerem **constantSpeedConsumption** especificado pelo utilizador. É um erro para especificar qualquer outro consumo modelo parâmetro, com exceção do **vehicleWeight**, se **constantSpeedConsumption*** não está especificado.
* **accelerationEfficiency** e **decelerationEfficiency** sempre tem de ser especificado como um par (ou seja, ambos none).
* Se **accelerationEfficiency** e **decelerationEfficiency** especificado, não pode ser superior a 1 (para evitar perpetual movimento) produto dos respetivos valores.
* **uphillEfficiency** e **downhillEfficiency** sempre tem de ser especificado como um par (ou seja, ambos none).
* Se **uphillEfficiency** e **downhillEfficiency** especificado, não pode ser superior a 1 (para evitar perpetual movimento) produto dos respetivos valores.
* Se \* **eficiência** parâmetros especificados pelo utilizador, em seguida, **vehicleWeight** também tem de ser especificado. Quando **vehicleEngineType** é _combustion_, **fuelEnergyDensityInMJoulesPerLiter** tem de ser especificado, bem como.
* **maxChargeInkWh** e **currentChargeInkWh** sempre tem de ser especificado como um par (ou seja, ambos none).

> [!NOTE]
> Se apenas **constantSpeedConsumption** for especificado, não existem outros aspetos de consumo como slopes e aceleração vehicle são contemplados para cálculos de consumo.

## <a name="combustion-consumption-model"></a>Modelo de consumo Combustion

O modelo de consumo Combustion é utilizado quando **vehicleEngineType** está definido como _combustion_.
A lista de parâmetros que pertencem a este modelo são abaixo. Consulte a secção de parâmetros para descrição detalhada.

* constantSpeedConsumptionInLitersPerHundredkm
* VehicleWeight
* currentFuelInLiters
* auxiliaryPowerInLitersPerHour
* fuelEnergyDensityInMJoulesPerLiter
* accelerationEfficiency
* decelerationEfficiency
* uphillEfficiency
* downhillEfficiency

## <a name="electric-consumption-model"></a>Modelo de consumo Electric

O modelo de consumo Electric é utilizado quando **vehicleEngineType** está definido como _electric_.
A lista de parâmetros que pertencem a este modelo são abaixo. Consulte a secção de parâmetros para descrição detalhada.

* constantSpeedConsumptionInkWhPerHundredkm
* VehicleWeight
* currentChargeInkWh
* maxChargeInkWh
* auxiliaryPowerInkW
* accelerationEfficiency
* decelerationEfficiency
* uphillEfficiency
* downhillEfficiency

## <a name="sensible-values-of-consumption-parameters"></a>Sensible valores dos parâmetros de consumo

Um conjunto específico de parâmetros de consumo pode ser rejeitado, mesmo que pode satisfazer os requisitos de explícitos especificados acima. Ocorre quando o valor de um parâmetro específico, ou uma combinação de valores de vários parâmetros, considera-se que para levar a magnitudes unreasonable dos valores de consumo. Se isto acontecer, provavelmente indica um erro de entrada, conforme adequado cuidado para acomodar os sensible todos os valores dos parâmetros de consumo. No caso de um determinado conjunto de parâmetros de consumo é rejeitado, a mensagem de erro associada irá conter uma explicação textual dos motivos.
As descrições detalhadas dos parâmetros têm os exemplos de valores sensible para ambos os modelos.
