---
title: Como instalar o Azure IoT Edge no Windows com contentores do Windows | Documentos da Microsoft
description: O Azure IoT Edge as instruções de instalação no Windows com contentores do Windows
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 08/06/2018
ms.author: kgremban
ms.openlocfilehash: 39e0de6b378ed61ab375c6468b58c8c4a87b5fb9
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39575969"
---
# <a name="install-azure-iot-edge-runtime-on-windows-to-use-with-windows-containers"></a>Instalar o runtime do Azure IoT Edge no Windows para utilização com contentores do Windows

O tempo de execução do Azure IoT Edge é implementado em todos os dispositivos do IoT Edge. Tem três componentes. O **daemon de segurança de IoT Edge** fornece e mantém as normas de segurança no dispositivo Edge. O daemon começa em cada inicialização e inicializa o dispositivo ao iniciar o agente do IoT Edge. O **agente do IoT Edge** facilita a implementação e monitorização de módulos no dispositivo do Edge, incluindo o hub IoT Edge. O **hub do IoT Edge** gere as comunicações entre os módulos no dispositivo do IoT Edge e entre o dispositivo e o Hub IoT.

Este artigo lista os passos para instalar o runtime do Azure IoT Edge no seu Windows x64 (AMD/Intel) system. 

Suporte do Windows está atualmente em pré-visualização.

## <a name="supported-windows-versions"></a>Versões suportadas do Windows
O Azure IoT Edge com contentores do Windows pode ser utilizado com:
  * Windows 10/IoT Enterprise/atualizar o IoT Core com Abril de 2018 (17134 criar).
  * Windows Server versão 1803

## <a name="install-the-container-runtime"></a>Instalar o runtime de contentor 

>[!NOTE]
>Para a instalação do motor de contentor no Windows IoT Core, siga os passos da [aprovisionar um artigo de dispositivo IoT Core] [ lnk-iot-core] e, em seguida, prossiga com as instruções abaixo.

O Azure IoT Edge se baseia numa [compatível com o OCI] [ lnk-oci] tempo de execução do contentor (por exemplo, o Docker). Pode usar [Docker para Windows] [ lnk-docker-for-windows] para desenvolvimento e teste. 

Configurar Docker para Windows [utilizar contentores do Windows][lnk-docker-config].

## <a name="install-the-azure-iot-edge-security-daemon"></a>Instale o Daemon de segurança do Azure IoT Edge

>[!NOTE]
>Pacotes de software do Azure IoT Edge são sujeita aos termos de licença localizados nos pacotes (no diretório de licença). Leia os termos de licença antes de utilizar o pacote. A instalação e a utilização do pacote constitui a sua aceitação destes termos. Se não concordar com os termos de licenciamento, não utilize o pacote.

Um único dispositivo IoT Edge pode ser aprovisionado manualmente através de uma cadeia de ligações de dispositivo fornecida pelo IoT Hub. Em alternativa, pode utilizar o serviço de aprovisionamento de dispositivos para aprovisionar automaticamente os dispositivos, que é útil quando tem o número de dispositivos para aprovisionar. Dependendo de sua escolha de aprovisionamento, escolha o script de instalação apropriado. 

### <a name="install-and-manually-provision"></a>Instalação e manualmente a aprovisionar

1. Siga os passos em [registar um novo dispositivo Azure IoT Edge] [ lnk-dcs] para registar o seu dispositivo e obter a cadeia de ligação do dispositivo. 

2. No seu dispositivo IoT Edge, execute o PowerShell como administrador. 

3. Baixe e instale o runtime do IoT Edge. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Install-SecurityDaemon -Manual -ContainerOs Windows
   ```

4. Quando lhe for pedido para um **DeviceConnectionString**, fornecem a cadeia de ligação que obteve a partir do IoT Hub. Não inclua a cadeia de ligação entre aspas. 

### <a name="install-and-automatically-provision"></a>Instalação automática e aprovisionamento

1. Siga os passos em [criar e aprovisionar um dispositivo de periferia de TPM simulado no Windows] [ lnk-dps] para configurar o serviço de aprovisionamento de dispositivos e obter seu **ID de âmbito**, simular um TPM dispositivos e obter seus **ID de registo**, em seguida, criar uma inscrição individual. Depois do dispositivo estiver registado no IoT Hub, continue com a instalação.  

   >[!TIP]
   >Mantenha a janela que está a executar o simulador de TPM aberta durante a instalação e teste. 

2. No seu dispositivo IoT Edge, execute o PowerShell como administrador. 

3. Baixe e instale o runtime do IoT Edge. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Install-SecurityDaemon -Dps -ContainerOs Windows
   ```

4. Quando solicitado, forneça o **ScopeID** e **RegistrationID** para o seu serviço de aprovisionamento e o dispositivo. 

## <a name="verify-successful-installation"></a>Certifique-se a instalação com êxito

Pode verificar o estado do serviço IoT Edge por: 

```powershell
Get-Service iotedge
```

Examine os registos de serviço para os últimos 5 minutos, com:

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

## <a name="next-steps"></a>Passos Seguintes

Agora que tiver um dispositivo IoT Edge aprovisionado com o tempo de execução instalado, pode [implementar módulos IoT Edge][lnk-modules].

Se estiver a ter problemas com o runtime do Edge corretamente a instalação, consulte a [resolução de problemas] [ lnk-trouble] página.


<!-- Images -->
[img-nat]: ./media/how-to-install-iot-edge-windows-with-windows/nat.png

<!-- Links -->
[lnk-docker-config]: https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers
[lnk-dcs]: how-to-register-device-portal.md
[lnk-dps]: how-to-auto-provision-simulated-device-windows.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md
[lnk-docker-for-windows]: https://www.docker.com/docker-windows
[lnk-iot-core]: how-to-install-iot-core.md
[lnk-modules]: how-to-deploy-modules-portal.md
