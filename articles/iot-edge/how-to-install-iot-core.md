---
title: Instalar o Azure IoT Edge no IoT Core | Documentos da Microsoft
description: Instalar o runtime do Azure IoT Edge num dispositivo Windows IoT Core
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: veyalla
ms.date: 03/05/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: ac729963e63bd97c83719e21dad3ad2cfc9b4fee
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49392799"
---
# <a name="install-the-iot-edge-runtime-on-windows-iot-core---preview"></a>Instalar o runtime do IoT Edge no Windows IoT Core - pré-visualização

O Azure IoT Edge e [Windows IoT Core](https://docs.microsoft.com/windows/iot-core/) funcionam em conjunto para ativar o edge em dispositivos pequenos de computação. O tempo de execução do Azure IoT Edge pode ser executado mesmo em dispositivos único quadro computador (SBC) muito pequeno, que são muito predominantes da indústria de IoT. 

Este artigo explica o tempo de execução num quadro de desenvolvimento com o Windows IoT Core de aprovisionamento. 

**Atualmente, o Windows IoT Core suporta o Azure IoT Edge apenas em processadores baseados em x64 do Intel.**

## <a name="install-the-container-runtime"></a>Instalar o runtime de contentor

1. Configurar o seu quadro com **17134 criar (RS4)** imagem IoT Core. 
1. Ativar o dispositivo, em seguida, [início de sessão remotamente com o PowerShell](https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell).
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
   >Este tempo de execução do contentor é a partir do servidor de compilação do projeto Moby e destina-se apenas a fins de avaliação. Não-lo com testado, apoiado pelo ou suportada pelo Docker.

## <a name="finish-installing"></a>Conclua a instalação

Instale o Daemon de segurança do IoT Edge e configurá-lo com as instruções em [neste artigo](how-to-install-iot-edge-windows-with-windows.md)

## <a name="next-steps"></a>Passos Seguintes

Agora que tiver um dispositivo com o runtime do IoT Edge, saiba como [implementar e monitorizar os módulos do IoT Edge em escala](how-to-deploy-monitor.md).