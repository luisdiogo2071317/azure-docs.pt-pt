---
title: Como instalar o Azure IoT Edge no Windows com contentores do Linux | Documentos da Microsoft
description: O Azure IoT Edge as instruções de instalação no Windows com contentores do Linux
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: kgremban
ms.openlocfilehash: 7bae81f2f8b9ef9f7da45aed3bf8d7b6e15c9d0b
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/12/2018
ms.locfileid: "51568272"
---
# <a name="install-the-azure-iot-edge-runtime-on-windows-to-use-with-linux-containers"></a>Instalar o runtime do Azure IoT Edge no Windows para utilização com contentores do Linux

O tempo de execução do Azure IoT Edge é o que transforma um dispositivo num dispositivo IoT Edge. O tempo de execução pode ser implementado nos dispositivos como pequena como um Raspberry Pi ou tão grande quanto um servidor industrial. Quando um dispositivo estiver configurado com o runtime do IoT Edge, pode começar a implementar lógica de negócios para o mesmo da cloud. 

Para saber mais sobre como funciona o runtime do IoT Edge e que componentes são incluídos, veja [compreender o tempo de execução do Azure IoT Edge e respetiva arquitetura](iot-edge-runtime.md).

Este artigo lista os passos para instalar o runtime do Azure IoT Edge com contentores do Linux no seu Windows x64 (AMD/Intel) system. Suporte do Windows está atualmente em pré-visualização.

>[!NOTE]
Através de contentores de Linux no Windows sytems não é uma configuração de produção recomendada ou suportado para o Azure IoT Edge. No entanto, ele pode ser usado para fins de testes e de desenvolvimento.

## <a name="supported-windows-versions"></a>Versões suportadas do Windows
O Azure IoT Edge pode ser utilizada para o desenvolvimento e teste nas seguintes versões do Windows, ao utilizar contentores do Linux:
  * Windows 10 ou sistemas operativos de ambiente de trabalho mais recentes.
  * Windows Server 2016 ou novos sistemas operacionais de servidor.

Para obter mais informações sobre os quais sistemas operacionais são atualmente suportados, consulte [suporte do Azure IoT Edge](support.md#operating-systems). 

## <a name="install-the-container-runtime"></a>Instalar o runtime de contentor 

O Azure IoT Edge se baseia numa [compatível com o OCI](https://www.opencontainers.org/) tempo de execução do contentor (por exemplo, o Docker). 

Pode usar [Docker para Windows](https://www.docker.com/docker-windows) para desenvolvimento e teste. Configurar Docker para Windows [utilizar contentores de Linux](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers).

## <a name="install-the-azure-iot-edge-security-daemon"></a>Instale o Daemon de segurança do Azure IoT Edge

>[!NOTE]
>Pacotes de software do Azure IoT Edge são sujeita aos termos de licença localizados nos pacotes (no diretório de licença). Leia os termos de licença antes de utilizar o pacote. A instalação e a utilização do pacote constitui a sua aceitação destes termos. Se não concordar com os termos de licenciamento, não utilize o pacote.

Um único dispositivo IoT Edge pode ser aprovisionado manualmente através de uma cadeia de ligações de dispositivo fornecida pelo IoT Hub. Em alternativa, pode utilizar o serviço de aprovisionamento de dispositivos para aprovisionar automaticamente os dispositivos, que é útil quando tem o número de dispositivos para aprovisionar. Dependendo de sua escolha de aprovisionamento, escolha o script de instalação apropriado. 

### <a name="option-1-install-and-manually-provision"></a>Opção 1: Instalar e aprovisionar manualmente

1. Siga os passos em [registar um novo dispositivo Azure IoT Edge](how-to-register-device-portal.md) para registar o seu dispositivo e obter a cadeia de ligação do dispositivo. 

2. No seu dispositivo IoT Edge, execute o PowerShell como administrador. 

3. Baixe e instale o runtime do IoT Edge. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Install-SecurityDaemon -Manual -ContainerOs Linux
   ```

4. Quando lhe for pedido para um **DeviceConnectionString**, fornecem a cadeia de ligação que obteve a partir do IoT Hub. Não inclua a cadeia de ligação entre aspas. 

### <a name="option-2-install-and-automatically-provision"></a>Opção 2: Instalar e aprovisionar automaticamente

1. Siga os passos em [criar e aprovisionar um dispositivo de periferia de TPM simulado no Windows](how-to-auto-provision-simulated-device-windows.md) para configurar o serviço de aprovisionamento de dispositivos e obter seu **ID de âmbito**, simular um dispositivo TPM e recuperar seu  **ID de registo**, em seguida, criar uma inscrição individual. Depois do dispositivo estiver registado no IoT Hub, continue com a instalação.  

   >[!TIP]
   >Mantenha a janela que está a executar o simulador de TPM aberta durante a instalação e teste. 

2. No seu dispositivo IoT Edge, execute o PowerShell como administrador. 

3. Baixe e instale o runtime do IoT Edge. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Install-SecurityDaemon -Dps -ContainerOs Linux
   ```

4. Quando solicitado, forneça o **ScopeID** e **RegistrationID** para o seu serviço de aprovisionamento e o dispositivo.

## <a name="verify-successful-installation"></a>Certifique-se a instalação com êxito

Pode verificar o estado do serviço IoT Edge por: 

```powershell
Get-Service iotedge
```

Examine os registos dos últimos 5 minutos através do serviço:

```powershell

# Displays logs from last 5 min, newest at the bottom.

Get-WinEvent -ea SilentlyContinue `
  -FilterHashtable @{ProviderName= "iotedged";
    LogName = "application"; StartTime = [datetime]::Now.AddMinutes(-5)} |
  select TimeCreated, Message |
  sort-object @{Expression="TimeCreated";Descending=$false} |
  format-table -autosize -wrap
```

E lista de módulos com a executar:

```powershell
iotedge list
```

## <a name="tips-and-suggestions"></a>Dicas e sugestões

Se a rede tiver um servidor proxy, siga os passos em [configurar o seu dispositivo IoT Edge para comunicar através de um servidor proxy](how-to-configure-proxy-support.md) para instalar e iniciar o runtime do IoT Edge.

## <a name="next-steps"></a>Passos Seguintes

Agora que tiver um dispositivo IoT Edge aprovisionado com o tempo de execução instalado, pode [implementar módulos IoT Edge](how-to-deploy-modules-portal.md).

Se estiver a ter problemas com o runtime do Edge corretamente a instalação, consulte a [resolução de problemas](troubleshoot.md) página.
