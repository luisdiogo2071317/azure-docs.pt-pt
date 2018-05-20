---
title: Conceitos de arquitetura no Azure IoT Central | Microsoft Docs
description: Este artigo apresenta os conceitos chave relacionadas com a arquitetura do Azure IoT Central
services: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 11/30/2017
ms.topic: conceptual
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: c97db3eb2c0fe1a5ec3c743ca75c595ec127823e
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="azure-iot-central-architecture"></a>Arquitetura do Centro de IoT do Azure

Este artigo fornece uma descrição geral da arquitetura do Microsoft Azure IoT Central.

![Arquitetura de nível superior](media/concepts-architecture/architecture.png)

## <a name="devices"></a>Dispositivos

Dispositivos trocam dados com a sua aplicação do Azure IoT Central. Um dispositivo pode:

- Envie medidas como telemetria.
- Sincronize definições com a sua aplicação.

No Azure IoT Central, os dados que um dispositivo pode trocar com a sua aplicação for especificados num modelo de dispositivo. Para obter mais informações sobre os modelos de dispositivos, consulte [metadados gestão](#metadata-management).

Para obter mais informações sobre como os dispositivos ligados à sua aplicação do Azure IoT Central, consulte o artigo [conectividade do dispositivo](concepts-connectivity.md).

## <a name="cloud-gateway"></a>Gateway de nuvem

Centro de IoT do Azure utiliza o IoT Hub do Azure como um gateway de nuvem que ativa a conectividade do dispositivo. Permite que o IoT Hub:

- Ingestão de dados à escala na nuvem.
- Gestão de dispositivos.
- Proteger a conectividade do dispositivo.

Para saber mais sobre o IoT Hub, consulte [IoT Hub do Azure](https://docs.microsoft.com/azure/iot-hub/).

Para obter mais informações acerca da conetividade do dispositivo no Azure IoT Central, consulte [conectividade do dispositivo](concepts-connectivity.md).

## <a name="data-stores"></a>Arquivos de dados

Centro de IoT do Azure armazena os dados de aplicação na nuvem. Os dados de aplicação armazenados incluem:

- Modelos de dispositivos.
- Identidades de dispositivo.
- Metadados do dispositivo.
- Dados de utilizador e da função.

Centro de IoT do Azure utiliza um armazenar série de tempo para os dados de medição enviados dos seus dispositivos. Dados de séries de tempo dos dispositivos utilizados pelo serviço de análise.

## <a name="analytics"></a>Análise

O serviço de análise é responsável pela geração os dados de relatórios personalizados que apresenta a aplicação. Um operador pode [personalizar a análise](howto-create-analytics.md) apresentado na aplicação. O serviço de análise baseia-se de [informações de séries de tempo de Azure](https://azure.microsoft.com/services/time-series-insights/) e processa os dados de medição enviados dos seus dispositivos.

## <a name="rules-and-actions"></a>Regras e ações

[Regras e ações](howto-create-telemetry-rules.md) trabalho em para automatizar tarefas na aplicação. Um construtor pode definir regras com base na telemetria do dispositivo, como a temperatura exceder um limiar definido. Centro de IoT do Azure utiliza um processador de fluxo para determinar quando são satisfeitas as condições de regra. Quando for cumprida uma condição de regra, aciona uma ação definida pelo construtor. Por exemplo, uma ação pode enviar uma mensagem de e-mail a notificar um engenheiro de que a temperatura o dispositivo é demasiado elevada.

## <a name="metadata-management"></a>Gestão de metadados

Numa aplicação do Azure IoT Central, modelos de dispositivo definem o comportamento e a capacidade dos tipos de dispositivo. Por exemplo, um modelo de dispositivo refrigerator Especifica a telemetria que envia uma refrigerator à sua aplicação.

![Arquitetura de modelo](media/concepts-architecture/template_architecture.png)

Num modelo de dispositivo:

- **Medidas** especificar envia a telemetria do dispositivo para a aplicação.
- **Definições** especificar configurações que pode definir um operador.
- **Propriedades** especificar metadados que pode definir um operador.
- **Regras** automatizar o comportamento da aplicação com base nos dados enviados a partir de um dispositivo.
- **Dashboards** são personalizáveis vistas de um dispositivo na aplicação.

Uma aplicação pode ter um ou mais dispositivos simulados e reais com base em cada modelo de dispositivo.

## <a name="rbac"></a>RBAC

Um [administrador pode definir regras de acesso](howto-administer.md) para uma aplicação do Azure IoT Central utiliza as funções predefinidas. Um administrador pode atribuir utilizadores a funções que determinam que áreas da aplicação do utilizador tem acesso.

## <a name="security"></a>Segurança

As funcionalidades de segurança no Azure IoT Central incluem:

- Os dados são encriptados em trânsito e rest.
- A autenticação é fornecida ou ao Azure Active Directory ou Account Microsoft. É suportada a autenticação de dois fatores.
- Isolamento de inquilinos completa.
- Segurança ao nível do dispositivo.

## <a name="ui-shell"></a>Shell de IU

A shell de IU é uma moderna, responder, HTML5 baseada no browser aplicação.

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu sobre a arquitetura do Azure IoT Central, o passo seguinte sugerido é saber mais sobre [conectividade do dispositivo](concepts-connectivity.md) no Azure IoT Central.