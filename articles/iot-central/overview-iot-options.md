---
title: Opções de IoT do Microsoft Azure | Microsoft Docs
description: Escolha como pretende implementar a sua solução do Azure IoT com o Azure IoT Central, IoT Suite ou Hub IoT.
services: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 11/30/2017
ms.topic: overview
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 8ed524e87f820f6c1e2e05da0bcbc7241bdd1ec1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34201463"
---
# <a name="compare-azure-iot-central-and-azure-iot-options"></a>Comparar as opções do Azure IoT Central e do Azure IoT

Para implementar uma solução de IoT, o Microsoft Azure IoT Central e o Azure IoT oferecem várias opções, cada uma delas adequada para diferentes conjuntos de requisitos de cliente:

* O [Azure IoT Central](overview-iot-central.md) é uma solução SaaS (Software-as-a-Service) que utiliza uma abordagem baseada no modelo, que lhe permite criar soluções de IoT de nível empresarial sem a necessidade de conhecimentos de desenvolvimento de soluções na cloud.

* O [Azure IoT Suite](https://docs.microsoft.com/azure/iot-suite/) é uma coleção de nível empresarial de [soluções pré-configuradas](https://docs.microsoft.com/azure/iot-suite/iot-suite-what-are-preconfigured-solutions) incorporadas na plataforma-como-um-serviço (PaaS) do Azure, que lhe permite acelerar o desenvolvimento de soluções de IoT personalizadas.

## <a name="azure-iot-hub"></a>Azure IoT Hub

O Hub IoT é a principal PaaS do Azure que o Azure IoT Central e o Azure IoT Suite utilizam. O Hub IoT permite comunicações bidirecionais fiáveis e seguras entre milhões de dispositivos IoT e uma solução na cloud. O Hub IoT ajuda a cumprir os desafios de implementação de IoT, como:

* Conectividade e gestão de dispositivos de volume elevado.
* Ingestão de telemetria de volume elevado.
* Comando e controlo de dispositivos.
* Imposição de segurança do dispositivo.

## <a name="compare-azure-iot-central-and-azure-iot-suite"></a>Comparar o Azure IoT Central e o Azure IoT Suite

Escolher o produto do Azure IoT é uma parte crítica do planeamento da solução de IoT. O Hub IoT é um serviço do Azure individual que, por si só, não fornece uma solução de IoT ponto a ponto. O Hub IoT pode ser utilizado como um ponto de partida para qualquer solução de IoT e não precisa de utilizar o Azure IoT Suite ou o Azure IoT Central para utilizá-lo. Tanto o IoT Suite como Azure IoT Central utilizam o Hub IoT, juntamente com outros serviços do Azure. A tabela seguinte resume as principais diferenças entre o IoT Suite e o Azure IoT Central, para o ajudar a escolher o mais adequado para os seus requisitos:

|     | Azure IoT Central | Azure IoT Suite |
| --- | ----------- | --------- |
| Utilização principal                      | Acelerar o tempo de colocação no mercado para soluções de IoT simples que não necessitam de personalização avançada do serviço.                                                    | Acelerar o desenvolvimento de uma solução de IoT personalizada que precisa da máxima flexibilidade.                                                                                                                             |
| Acesso aos serviços PaaS subjacentes | SaaS. Solução totalmente gerida, os serviços subjacentes não são expostos.                                                                                            | Tem acesso aos serviços do Azure subjacentes para geri-los ou substituí-los, conforme necessário.                                                                                                                    |
| Flexibilidade                        | Média. Pode utilizar a experiência de utilizador baseada no browser incorporado para personalizar o modelo de solução e os aspetos da IU. A infraestrutura não é personalizável porque os diferentes componentes não são expostos. | Elevada. O código para os microsserviços é open source e pode modificá-lo conforme adequado. Além disso, pode personalizar a infraestrutura de implementação.                                               |
| Nível de competência                        | Baixo. Precisa de competências de modelação para personalizar a solução. Não são necessárias competências de codificação.                                                                          | Médio-Elevado. Precisa de competências em Java ou .NET para personalizar o back-end da solução. Precisa de competências em JavaScript para personalizar a visualização.                                                                       |
| Experiência de introdução             | Os modelos de aplicações e os modelos de dispositivos disponibilizam modelos pré-criados. Pode ser implementado em segundos.                                                                                                  | As soluções pré-configuradas implementam cenários de IoT comuns. Pode ser implementado em segundos.                                                                                                                            |
| Preços                            | Estrutura de preços simples e previsível.                                                                                                                           | Pode ajustar os serviços para controlar o custo.                                                                                                                                                            |

A decisão do produto a utilizar para criar a sua solução de IoT é, em última análise, determinada por:

* Os seus requisitos comerciais.
* O tipo de solução que pretende criar
* O conjunto de competências da organização para criar e manter a solução a longo prazo.

## <a name="next-steps"></a>Passos seguintes

Com base no produto e na abordagem escolhidos, os passos seguintes sugeridos são:

* **Azure IoT Central**: [Azure IoT Central](overview-iot-central.md).
* **IoT Suite**: [Quais são as soluções pré-configuradas do Azure IoT?](https://docs.microsoft.com/azure/iot-suite/iot-suite-what-are-preconfigured-solutions).
* **Hub IoT**: [ Descrição geral do serviço Hub IoT do Azure](https://docs.microsoft.com/azure/iot-hub/iot-hub-what-is-iot-hub).