---
title: Dispositivo do Azure IoT Edge aprovisionar automaticamente com o DPS - Windows | Documentos da Microsoft
description: Utilizar um dispositivo simulado no seu computador Windows para testar o aprovisionamento automático de dispositivos do Azure IoT Edge com o serviço de aprovisionamento de dispositivos
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 46970d5628df3b46ec88df998a328928f60e15b4
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090236"
---
# <a name="create-and-provision-a-simulated-tpm-edge-device-on-windows"></a>Criar e aprovisionar um dispositivo de periferia de TPM simulado no Windows

Dispositivos do IoT Edge do Azure podem ser aprovisionado automaticamente com o [serviço aprovisionamento de dispositivos](../iot-dps/index.yml) tal como os dispositivos que não são habilitados no edge. Se não estiver familiarizado com o processo de aprovisionamento automático, reveja os [conceitos de aprovisionamento automático](../iot-dps/concepts-auto-provisioning.md) antes de continuar. 

Este artigo mostra-lhe como testar aprovisionamento automático num dispositivo Edge simulado com os seguintes passos: 

* Crie uma instância do IoT Hub dispositivo aprovisionamento DPS (serviço).
* Crie um dispositivo simulado no seu computador Windows com um simulado Trusted Platform Module (TPM) para a segurança de hardware.
* Crie uma inscrição individual para o dispositivo.
* Instalar o runtime do IoT Edge e ligar o dispositivo ao IoT Hub.

## <a name="prerequisites"></a>Pré-requisitos

* Uma máquina de desenvolvimento do Windows. Este artigo utiliza o Windows 10. 
* Um IoT Hub Active Directory. 

## <a name="set-up-the-iot-hub-device-provisioning-service"></a>Configurar o serviço de aprovisionamento de dispositivos do IoT Hub

Criar uma nova instância do serviço de aprovisionamento de dispositivos do IoT Hub no Azure e ligá-lo ao seu hub IoT. Pode seguir as instruções em [configurar o Hub IoT DPS](../iot-dps/quick-setup-auto-provision.md).

Depois de ter a execução do serviço aprovisionamento de dispositivos, copie o valor da **âmbito do ID** da página de descrição geral. Utilize este valor quando configurar o runtime do IoT Edge. 

## <a name="simulate-a-tpm-device"></a>Simular um dispositivo TPM

Crie um dispositivo TPM simulado no seu computador de desenvolvimento do Windows. Obter o **ID de registo** e **chave de endossamento** para o seu dispositivo e usá-los para criar uma entrada de inscrição individual no DPS. 

Quando criar uma inscrição em pontos de distribuição, terá a oportunidade para declarar uma **inicial estado do dispositivo duplo**. No dispositivo duplo pode definir etiquetas para agrupar dispositivos com qualquer métrica que terá na sua solução, como o tipo de dispositivo, localização, ambiente ou região. Essas marcas são usadas para criar [implementações automáticas](how-to-deploy-monitor.md). 

Escolha o idioma SDK que pretende utilizar para criar o dispositivo simulado e siga os passos até criar a inscrição individual. 

Ao criar a inscrição individual, selecione **habilitar** declarar que esta máquina virtual é uma **dispositivo IoT Edge**.

Dispositivo simulado e guias de inscrição individual: 
* [C](../iot-dps/quick-create-simulated-device.md)
* [Java](../iot-dps/quick-create-simulated-device-tpm-java.md)
* [C#](../iot-dps/quick-create-simulated-device-tpm-csharp.md)
* [Node.js](../iot-dps/quick-create-simulated-device-tpm-node.md)
* [Python](../iot-dps/quick-create-simulated-device-tpm-python.md)

Depois de criar a inscrição individual, guardar o valor do **ID de registo**. Utilize este valor quando configurar o runtime do IoT Edge. 

## <a name="install-the-iot-edge-runtime"></a>Instalar o runtime do IoT Edge

O runtime do IoT Edge é implementado em todos os dispositivos do IoT Edge. Seus componentes executadas em contentores e permitem-lhe implementar contentores adicionais no dispositivo para que pode executar o código na periferia. Em dispositivos com Windows, pode optar por utilizar contentores do Windows ou de contentores do Linux. Escolha o tipo de contentores que pretende utilizar e siga os passos. Certifique-se configurar o runtime do IoT Edge para aprovisionamento automático e não manual. 

Siga as instruções para instalar o runtime do IoT Edge no dispositivo que está a executar o TPM simulado da secção anterior. 

Saber o DPS **âmbito do ID** e o dispositivo **ID de registo** antes de iniciar estes artigos. 

* [Contentores do Windows](how-to-install-iot-edge-windows-with-windows.md)
* [Contentores do Linux](how-to-install-iot-edge-windows-with-linux.md)

## <a name="create-a-tpm-environment-variable"></a>Criar uma variável de ambiente de TPM

No computador com o seu dispositivo simulado, modificar os **iotedge** registo de serviço para definir uma variável de ambiente.

1. Partir do **começar** menu, abra **regedit**. 
2. Navegue para **Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\iotedge**. 
3. Selecione **edite** > **nova** > **valor de múltiplas cadeias**. 
4. Introduza o nome **ambiente**. 
5. Faça duplo clique em nova variável e defina os dados de valor como **IOTEDGE_USE_TPM_DEVICE = ON**. 
6. Clique em **OK** para guardar as alterações. 

## <a name="restart-the-iot-edge-runtime"></a>Reiniciar o runtime do IoT Edge

Reinicie o runtime do IoT Edge, para que ele seleciona todas as alterações de configuração feitas no dispositivo. 

```powershell
Stop-Service iotedge -NoWait
sleep 5
Start-Service iotedge
```

## <a name="verify-successful-installation"></a>Certifique-se a instalação com êxito

Se o tempo de execução foi iniciado com êxito, pode entrar no seu IoT Hub e ver que o seu novo dispositivo foi aprovisionado automaticamente e está pronto para executar os módulos do IoT Edge. 

Verifique o estado do serviço IoT Edge.

```powershell
Get-Service iotedge
```

Examine os registos do serviço dos últimos 5 minutos.

```powershell
# Displays logs from last 5 min, newest at the bottom.

Get-WinEvent -ea SilentlyContinue `
  -FilterHashtable @{ProviderName= "iotedged";
    LogName = "application"; StartTime = [datetime]::Now.AddMinutes(-5)} |
  select TimeCreated, Message |
  sort-object @{Expression="TimeCreated";Descending=$false}
```

Lista de módulos em execução.

```powershell
iotedge list
```

## <a name="next-steps"></a>Passos Seguintes

O processo de inscrição do serviço aprovisionamento de dispositivos permite-lhe definir o ID de dispositivo e etiquetas do dispositivo duplo ao mesmo tempo, como aprovisionar o novo dispositivo. Pode utilizar esses valores para dispositivos individuais ou grupos de dispositivos com a gestão de dispositivos automático de destino. Saiba como [implementar e monitorizar módulos em dimensionar no portal do Azure do IoT Edge](how-to-deploy-monitor.md) ou [com a CLI do Azure](how-to-deploy-monitor-cli.md)