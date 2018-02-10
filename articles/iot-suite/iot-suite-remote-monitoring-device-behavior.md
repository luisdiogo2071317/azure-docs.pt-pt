---
title: "Simulated comportamento de dispositivo numa solução de monitorização remota - Azure | Microsoft Docs"
description: "Este artigo descreve como utilizar o JavaScript para definir o comportamento de um dispositivo simulado na solução de monitorização remota."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 01/29/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: e5846893166c3e65b75e84d02849c2b8ab78e079
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="implement-the-device-model-behavior"></a>Implementar o comportamento de modelo do dispositivo

O artigo [compreender o esquema do modelo de dispositivo](iot-suite-remote-monitoring-device-schema.md) descrito o esquema que define um modelo do dispositivo simulado. Esse artigo referido dois tipos de ficheiro JavaScript que implementam o comportamento de um dispositivo simulado:

- **Estado** JavaScript ficheiros que são executadas em intervalos fixos para atualizar o estado interno do dispositivo.
- **Método** JavaScript ficheiros que são executados quando a solução invoca um método no dispositivo.

Neste artigo, saiba como:

>[!div class="checklist"]
> * Controlar o estado de um dispositivo simulado
> * Definir como reponds um dispositivo simulado para um método de chamada da solução de monitorização remota
> * Os scripts de depuração

## <a name="state-behavior"></a>Comportamento de estado

O [simulação](iot-suite-remote-monitoring-device-schema.md#simulation) secção do esquema de modelo do dispositivo define o estado interno de um dispositivo simulado:

- `InitialState`Define valores iniciais para todas as propriedades do objeto de estado do dispositivo.
- `Script`identifica um ficheiro de JavaScript é executado numa agenda, para atualizar o estado do dispositivo.

O exemplo seguinte mostra a definição do objeto de estado do dispositivo para um dispositivo simulado chiller:

```json
"Simulation": {
  "InitialState": {
    "online": true,
    "temperature": 75.0,
    "temperature_unit": "F",
    "humidity": 70.0,
    "humidity_unit": "%",
    "pressure": 150.0,
    "pressure_unit": "psig",
    "simulation_state": "normal_pressure"
  },
  "Script": {
    "Type": "javascript",
    "Path": "chiller-01-state.js",
    "Interval": "00:00:05"
  }
}
```

O estado do dispositivo simulado, como definido no `InitialState` secção, é mantido em memória pelo serviço de simulação. As informações de estado são transmitidas como entrada para o `main` função definida no **chiller-01-state.js**. Neste exemplo, o serviço de simulação é executado o **chiller-01-state.js** ficheiro a cada cinco segundos. O script pode modificar o estado do dispositivo simulado.

O seguinte mostra o contorno de típica `main` função:

```javascript
function main(context, previousState) {

  // Use the previous device state to
  // generate the new device state
  // returned by the function.

  return state;
}
```

O `context` parâmetro tem as seguintes propriedades:

- `currentTime`como uma cadeia com formato`yyyy-MM-dd'T'HH:mm:sszzz`
- `deviceId`, por exemplo`Simulated.Chiller.123`
- `deviceModel`, por exemplo`Chiller`

O `state` parâmetro contém o estado do dispositivo como mantido pelo serviço de simulação de dispositivo. Este valor é o `state` objecto devolvido por chamada anterior para `main`.

O exemplo seguinte mostra uma implementação típica do `main` método para processar o estado do dispositivo mantido pelo serviço de simulação:

```javascript
// Default state
var state = {
  online: true,
  temperature: 75.0,
  temperature_unit: "F",
  humidity: 70.0,
  humidity_unit: "%",
  pressure: 150.0,
  pressure_unit: "psig",
  simulation_state: "normal_pressure"
};

function restoreState(previousState) {
  // If the previous state is null, force a default state
  if (previousState !== undefined && previousState !== null) {
      state = previousState;
  } else {
      log("Using default state");
  }
}

function main(context, previousState) {

  restoreState(previousState);

  // Update state

  return state;
}
```

O seguinte exemplo mostra como as `main` método pode simular valores de telemetria que variam ao longo do tempo:

```javascript
/**
 * Simple formula generating a random value
 * around the average and between min and max
 */
function vary(avg, percentage, min, max) {
  var value = avg * (1 + ((percentage / 100) * (2 * Math.random() - 1)));
  value = Math.max(value, min);
  value = Math.min(value, max);
  return value;
}


function main(context, previousState) {

    restoreState(previousState);

    // 75F +/- 5%,  Min 25F, Max 100F
    state.temperature = vary(75, 5, 25, 100);

    // 70% +/- 5%,  Min 2%, Max 99%
    state.humidity = vary(70, 5, 2, 99);

    log("Simulation state: " + state.simulation_state);
    if (state.simulation_state === "high_pressure") {
        // 250 psig +/- 25%,  Min 50 psig, Max 300 psig
        state.pressure = vary(250, 25, 50, 300);
    } else {
        // 150 psig +/- 10%,  Min 50 psig, Max 300 psig
        state.pressure = vary(150, 10, 50, 300);
    }

    return state;
}
```

Pode ver o concluída [chiller-01-state.js](https://github.com/Azure/device-simulation-dotnet/blob/master/Services/data/devicemodels/scripts/chiller-01-state.js) no Github.

## <a name="method-behavior"></a>Comportamento de método

O [CloudToDeviceMethods](iot-suite-remote-monitoring-device-schema.md#cloudtodevicemethods) secção do esquema de modelo do dispositivo define os métodos de um dispositivo simulado responde a.

O exemplo seguinte mostra a lista de métodos suportados por um dispositivo simulado chiller:

```json
"CloudToDeviceMethods": {
  "Reboot": {
    "Type": "javascript",
    "Path": "Reboot-method.js"
  },
  "FirmwareUpdate": {
    "Type": "javascript",
    "Path": "FirmwareUpdate-method.js"
  },
  "EmergencyValveRelease": {
    "Type": "javascript",
    "Path": "EmergencyValveRelease-method.js"
  },
  "IncreasePressure": {
    "Type": "javascript",
    "Path": "IncreasePressure-method.js"
  }
}
```

Cada método tem um ficheiro JavaScript associado que implementa o comportamento do método.

O estado do dispositivo simulado, como definido no `InitialState` secção do esquema, está retido na memória pelo serviço de simulação. As informações de estado são transmitidas como entrada para o `main` função definida no ficheiro JavaScript quando o método é chamado. O script pode modificar o estado do dispositivo simulado.

O seguinte mostra o contorno de típica `main` função:

```javascript
function main(context, previousState) {

}
```

O `context` parâmetro tem as seguintes propriedades:

- `currentTime`como uma cadeia com formato`yyyy-MM-dd'T'HH:mm:sszzz`
- `deviceId`, por exemplo`Simulated.Chiller.123`
- `deviceModel`, por exemplo`Chiller`

O `state` parâmetro contém o estado do dispositivo como mantido pelo serviço de simulação de dispositivo.

Existem duas funções globais, que pode utilizar para ajudar a implementar o comportamento do método:

- `updateState`Atualize o Estado Retido, o serviço de simulação.
- `sleep`para colocar em pausa a execução para simular uma tarefa de longa execução.

O exemplo seguinte mostra uma versão abreviada do **IncreasePressure method.js** script utilizado pelos dispositivos simulados chiller:

```javascript
function main(context, previousState) {

    log("Starting 'Increase Pressure' method simulation (5 seconds)");

    // Pause the simulation and change the simulation mode so that the
    // temperature will fluctuate at ~250 when it resumes
    var state = {
        simulation_state: "high_pressure",
        CalculateRandomizedTelemetry: false
    };
    updateState(state);

    // Increase
    state.pressure = 210;
    updateState(state);
    log("Pressure increased to " + state.pressure);
    sleep(1000);

    // Increase
    state.pressure = 250;
    updateState(state);
    log("Pressure increased to " + state.pressure);
    sleep(1000);

    // Resume the simulation
    state.CalculateRandomizedTelemetry = true;
    updateState(state);

    log("'Increase Pressure' method simulation completed");
}
```

## <a name="debugging-script-files"></a>Ficheiros de script de depuração

Não é possível anexar um depurador para o interpretador de Javascript utilizado pelo serviço de simulação de dispositivo para executar scripts de estado e o método. No entanto, pode registar informações no registo do serviço. Incorporada `log()` função permite-lhe guardar informações de controlar e depurar a execução de função.

Se existir um erro de sintaxe o interpretador falha, e escreve um `Jint.Runtime.JavaScriptException` entrada no registo de serviço.

O [criar um dispositivo simulado](iot-suite-remote-monitoring-test.md) artigo mostra como executar o serviço de simulação de dispositivo localmente. Executar o serviço localmente torna mais fácil depurar os seus dispositivos simulados antes de implementá-las para a nuvem.

## <a name="next-steps"></a>Passos Seguintes

Este artigo descrito como definir o comportamento do seu próprio modelo personalizadas do dispositivo simulado. Este artigo mostrou, como para:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Controlar o estado de um dispositivo simulado
> * Definir como reponds um dispositivo simulado para um método de chamada da solução de monitorização remota
> * Os scripts de depuração

Agora que aprendeu como especificar o comportamento de um dispositivo simulado, o passo seguinte sugerido é saber como [criar um dispositivo simulado](iot-suite-remote-monitoring-test.md).

Para obter mais informações de programador sobre a solução de monitorização remota, consulte:

* [Guia de Referência para Programadores](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Guia de Resolução de Problemas de Programadores](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->