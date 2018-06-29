---
title: Instalar o limite de IoT do Azure no IoT Core | Microsoft Docs
description: Instalar o runtime do Azure IoT Edge num dispositivo Windows IoT Core
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: veyalla
ms.date: 03/05/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: ae5644a62b794dc8d6ace52f21a452fa70027d39
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37029565"
---
# <a name="install-the-iot-edge-runtime-on-windows-iot-core---preview"></a>Instalar o runtime de limite de IoT no Windows IoT Core - pré-visualização

Limite de IoT do Azure e [Windows IoT Core](https://docs.microsoft.com/windows/iot-core/) funcionam em conjunto para ativar o limite de computação nos dispositivos, mesmo pequenos. Pode executar o tempo de execução do limite de IoT do Azure, mesmo em dispositivos muito pequena do único quadro computador (SBC) que são muito prevalente atualmente da indústria de IoT. 

Este artigo explica o tempo de execução num quadro de desenvolvimento com o Windows IoT Core de aprovisionamento. 

**Atualmente, o Windows IoT Core suporta o Azure IoT Edge apenas nos processadores baseados em x64 do Intel.**

## <a name="install-the-container-runtime"></a>Instalar o runtime do contentor

1. Configurar o seu dashboard com **criar 17134 (RS4)** imagem de núcleos de IoT. 
1. Ativar no dispositivo, em seguida, [início de sessão remotamente com o PowerShell][lnk-powershell].
1. Na consola do PowerShell, instale o tempo de execução do contentor: 

   ```powershell
   Invoke-WebRequest https://master.dockerproject.org/windows/x86_64/docker-0.0.0-dev.zip -o temp.zip
   Expand-Archive .\temp.zip $env:ProgramFiles -f
   Remove-Item .\temp.zip
   $env:Path += ";$env:programfiles\docker"
   SETX /M PATH "$env:Path"
   dockerd --register-service
   start-service docker
   ```

   >[!NOTE]
   >Este contentor runtime é a partir do servidor de compilação do projeto de Moby e destina-se apenas a fins de avaliação. -Não testado, aprovadas ou suportada pelo Docker.

## <a name="finish-installing"></a>Concluir a instalação

Instalar o Daemon de segurança de limite de IoT e configurá-lo com as instruções no [neste artigo][lnk-install-windows-on-windows]

## <a name="next-steps"></a>Passos Seguintes

Agora que tem um dispositivo com o tempo de execução do limite do IoT, saiba como [implementar e monitorizar os módulos de limite de IoT à escala][lnk-deploy].

<!--Links-->
[lnk-install-windows-on-windows]: how-to-install-iot-edge-windows-with-windows.md#download-the-edge-daemon-package-and-install
[lnk-powershell]: https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell
[lnk-deploy]: how-to-deploy-monitor.md
[lnk-docker-install]: https://docs.docker.com/engine/installation/linux/docker-ce/binaries#install-server-and-client-binaries-on-windows
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers