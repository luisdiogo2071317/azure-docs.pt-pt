---
title: Dispositivo de limite de IoT do Azure de aprovisionamento automático com DPS - Windows | Microsoft Docs
description: Utilizar um dispositivo simulado no seu computador Windows para testar o aprovisionamento de dispositivos automático para limite de IoT do Azure com o serviço de aprovisionamento de dispositivos
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 0a973b248022cf3c0497f72bc2fcdd45a6527e65
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37116286"
---
# <a name="create-and-provision-a-simulated-tpm-edge-device-on-windows"></a>Criar e aprovisionar um dispositivo simulado do limite do TPM no Windows

Dispositivos de limite de IoT do Azure podem ser automaticamente aprovisionado utilizando o [serviço de aprovisionamento de dispositivos](../iot-dps/index.yml) que apenas os dispositivos que não estão preparados para o limite. Se não estiver familiarizado com o processo de aprovisionamento automático, reveja o [conceitos de aprovisionamento automático](../iot-dps/concepts-auto-provisioning.md) antes de continuar. 

Este artigo mostra como testar aprovisionamento automático num dispositivo simulado contorno com os seguintes passos: 

* Crie uma instância do IoT Hub dispositivo aprovisionamento de serviço (DPS).
* Crie um dispositivo simulado no seu computador Windows com um simulada Trusted Platform Module (TPM) para segurança de hardware.
* Crie um registo individual para o dispositivo.
* Instale o runtime de limite de IoT e ligue o dispositivo ao IoT Hub.

## <a name="prerequisites"></a>Pré-requisitos

* Uma máquina de desenvolvimento do Windows. Este artigo utiliza o Windows 10. 
* Um IoT Hub Active Directory. 

## <a name="set-up-the-iot-hub-device-provisioning-service"></a>Configurar o serviço de aprovisionamento de dispositivos do IoT Hub

Criar uma nova instância do serviço de aprovisionamento de dispositivos do IoT Hub no Azure e ligá-lo ao seu IoT hub. Pode seguir as instruções em [configurar DPS de Hub IoT](../iot-dps/quick-setup-auto-provision.md).

Depois de ter a execução do serviço de aprovisionamento de dispositivos, copie o valor da **ID âmbito** da página de descrição geral. Utilize este valor quando configurar o tempo de execução do limite de IoT. 

## <a name="simulate-a-tpm-device"></a>Simular um dispositivo TPM

Crie um dispositivo simulado do TPM no seu computador de desenvolvimento do Windows. Obter o **ID de registo** e **chave de endossamento** para o seu dispositivo e utilizá-los para criar uma entrada de registo individuais no DPS. 

Quando cria uma inscrição no DPS, terá a oportunidade de declarar um **inicial dispositivo duplo estado**. No dispositivo duplo pode definir etiquetas para agrupar dispositivos por qualquer métrica que precisa na sua solução, como o tipo de dispositivo, localização, ambiente ou região. Estas etiquetas são utilizadas para criar [implementações automáticas](how-to-deploy-monitor.md). 

Escolha o idioma SDK que pretende utilizar para criar o dispositivo simulado e siga os passos até que crie a inscrição individuais. 

Quando cria a inscrição individuais, selecione **ativar** para declarar que a máquina virtual é um **dispositivo de limite de IoT**.

Dispositivo simulado e guias de inscrição individuais: 
* [C](../iot-dps/quick-create-simulated-device.md)
* [Java](../iot-dps/quick-create-simulated-device-tpm-java.md)
* [C#](../iot-dps/quick-create-simulated-device-tpm-csharp.md)
* [Node.js](../iot-dps/quick-create-simulated-device-tpm-node.md)
* [Python](../iot-dps/quick-create-simulated-device-tpm-python.md)

Depois de criar a inscrição individuais, guardar o valor da **ID de registo**. Utilize este valor quando configurar o tempo de execução do limite de IoT. 

## <a name="install-the-iot-edge-runtime"></a>Instalar o runtime de limite de IoT

O runtime do IoT Edge é implementado em todos os dispositivos do IoT Edge. Os respetivos componentes executam nos contentores e permitem-lhe implementar contentores adicionais para o dispositivo para que possam executar código no limite. Em dispositivos com Windows, pode optar por utilizar contentores do Windows ou Linux contentores. Escolha o tipo de contentores que pretende utilizar e siga os passos. Certifique-se configurar o tempo de execução de limite de IoT para o aprovisionamento automático, não manual. 

Siga as instruções para instalar o runtime de IoT Edge no dispositivo que está a executar o TPM simulado da secção anterior. 

Conhecer o DPS **ID âmbito** e dispositivo **ID de registo** antes de iniciar estes artigos. 

* [Contentores do Windows](how-to-install-iot-edge-windows-with-windows.md)
* [Contentores do Linux](how-to-install-iot-edge-windows-with-linux.md)

## <a name="create-a-tpm-environment-variable"></a>Criar uma variável de ambiente do TPM

No computador com o seu dispositivo simulado, modifique o **iotedge** registo do serviço para definir uma variável de ambiente.

1. Do **iniciar** menu, abra **regedit**. 
2. Navegue para **Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\iotedge**. 
3. Selecione **editar** > **novo** > **valor de múltiplas cadeias**. 
4. Introduza o nome **ambiente**. 
5. Faça duplo clique em nova variável e defina os dados de valor **IOTEDGE_USE_TPM_DEVICE = ON**. 
6. Clique em **OK** para guardar as alterações. 

## <a name="restart-the-iot-edge-runtime"></a>Reinicie o tempo de execução do limite de IoT

Reinicie o tempo de execução do limite de IoT para que esta seleciona todas as alterações de configuração efetuadas no dispositivo. 

```powershell
Stop-Service iotedge -NoWait
sleep 5
Start-Service iotedge
```

Certifique-se de que o tempo de execução do limite de IoT está em execução. 

   ```bash
   sudo systemctl status iotedge
   ```

## <a name="verify-successful-installation"></a>Certifique-se a instalação com êxito

Se o tempo de execução iniciado com êxito, pode ir para o seu IoT Hub e ver que o seu novo dispositivo foi automaticamente aprovisionado e está pronto para ser executado o limite de IoT módulos. 

Verifique o estado do serviço IoT Edge.

```powershell
Get-Service iotedge
```

Examine os registos do serviço de últimos 5 minutos.

```powershell
# Displays logs from last 5 min, newest at the bottom.

Get-WinEvent -ea SilentlyContinue `
  -FilterHashtable @{ProviderName= "iotedged";
    LogName = "application"; StartTime = [datetime]::Now.AddMinutes(-5)} |
  select TimeCreated, Message |
  sort-object @{Expression="TimeCreated";Descending=$false}
```

Liste módulos em execução.

```powershell
iotedge list
```

## <a name="next-steps"></a>Passos Seguintes

O processo de inscrição do serviço de aprovisionamento de dispositivos permite-lhe definir o ID de dispositivo e etiquetas do dispositivo duplo ao mesmo tempo, como aprovisionar o novo dispositivo. Pode utilizar esses valores para os dispositivos individuais ou grupos de gestão de dispositivos automático de dispositivos de destino. Saiba como [implementar e monitorizar Edge IoT no portal do Azure de escala de módulos em](how-to-deploy-monitor.md) ou [utilizando a CLI do Azure](how-to-deploy-monitor-cli.md)