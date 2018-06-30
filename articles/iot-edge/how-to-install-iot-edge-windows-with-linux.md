---
title: Como instalar o Azure IoT Edge no Windows com contentores do Linux | Microsoft Docs
description: Azure IoT Edge as instruções de instalação no Windows com o Linux contentores
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: kgremban
ms.openlocfilehash: 039ca304633eafa8211daffe1a4241b326eda6fb
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114106"
---
# <a name="install-azure-iot-edge-runtime-on-windows-to-use-with-linux-containers"></a>Instalar o runtime de limite de IoT do Azure no Windows para utilizar com o Linux contentores

O tempo de execução do Azure IoT Edge é implementado em todos os dispositivos de limite de IoT. Tem três componentes. O **daemon de segurança de IoT Edge** fornece e mantém as normas de segurança do dispositivo de limite. O daemon de inicia a cada arranque e bootstraps o dispositivo ao iniciar o agente de limite de IoT. O **agente IoT Edge** facilita a implementação e monitorização dos módulos do dispositivo de limite, incluindo o hub IoT Edge. O **hub do IoT Edge** gere as comunicações entre os módulos no dispositivo do IoT Edge e entre o dispositivo e o Hub IoT.

Este artigo apresenta os passos para instalar o runtime de limite de IoT do Azure no seu Windows x64 (AMD/Intel) sistema. Suporte do Windows está atualmente em pré-visualização.

>[!NOTE]
Utilizar contentores do Linux no Windows sytems não é uma configuração de produção recomendada ou suportado para o limite de IoT do Azure. No entanto, pode ser utilizado para desenvolvimento e testes.

## <a name="supported-windows-versions"></a>Versões suportadas do Windows
Limite de IoT do Azure pode ser utilizado para o desenvolvimento e teste nas seguintes versões do Windows, ao utilizar os contentores de Linux:
  * Windows 10 ou sistemas operativos de ambiente de trabalho mais recentes.
  * Windows Server 2016 ou novos sistemas operativos de servidor.

## <a name="install-the-container-runtime"></a>Instalar o runtime do contentor 

Limite de IoT do Azure baseia-se num [OCI compatível] [ lnk-oci] tempo de execução do contentor (por exemplo, Docker). 

Pode utilizar [Docker para Windows] [ lnk-docker-for-windows] para desenvolvimento e testes. Certifique-se de Docker para Windows [configurado para utilizar os contentores do Linux][lnk-docker-config]

## <a name="install-the-azure-iot-edge-security-daemon"></a>Instalar o Daemon de segurança do Azure IoT Edge

>[!NOTE]
>Os pacotes de software do limite do IoT do Azure estão sujeitas os termos de Licenciamento localizados nos pacotes (no diretório de licença). Leia os termos de licenciamento antes de utilizar o pacote. A instalação e utilização do pacote constitui a aceitação destes termos. Se não concordar com os termos de licenciamento, não utilize o pacote.

### <a name="download-the-edge-daemon-package-and-install"></a>Transferir o pacote de daemon de limite e instalar

Numa janela do PowerShell de administrador, execute os seguintes comandos:

```powershell
Invoke-WebRequest https://aka.ms/iotedged-windows-latest -o .\iotedged-windows.zip
Expand-Archive .\iotedged-windows.zip C:\ProgramData\iotedge -f
Move-Item c:\ProgramData\iotedge\iotedged-windows\* C:\ProgramData\iotedge\ -Force
rmdir C:\ProgramData\iotedge\iotedged-windows
$env:Path += ";C:\ProgramData\iotedge"
SETX /M PATH "$env:Path"
```

Instale o vcruntime utilizando:

```powershell
Invoke-WebRequest -useb https://download.microsoft.com/download/0/6/4/064F84EA-D1DB-4EAA-9A5C-CC2F0FF6A638/vc_redist.x64.exe -o vc_redist.exe
.\vc_redist.exe /quiet /norestart
 ```

Crie e inicie *iotedge* serviço:

```powershell
New-Service -Name "iotedge" -BinaryPathName "C:\ProgramData\iotedge\iotedged.exe -c C:\ProgramData\iotedge\config.yaml"
Start-Service iotedge
```

Adicione exceções de Firewall para as portas utilizadas pelo serviço:

```powershell
New-NetFirewallRule -DisplayName "iotedged allow inbound 15580,15581" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 15580-15581 -Program "C:\programdata\iotedge\iotedged.exe" -InterfaceType Any
```

Criar um **iotedge.reg** com o seguinte conteúdo de ficheiros e importe no registo do Windows, fazer duplo clique ou utilizando o `reg import iotedge.reg` comando:

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\iotedged]
"CustomSource"=dword:00000001
"EventMessageFile"="C:\\ProgramData\\iotedge\\iotedged.exe"
"TypesSupported"=dword:00000007
```

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Configurar o Daemon de segurança do Azure IoT Edge

O daemon de pode ser configurado utilizando o ficheiro de configuração em `C:\ProgramData\iotedge\config.yaml`.

O dispositivo de limite pode ser configurado manualmente utilizando um [a cadeia de ligação do dispositivo] [ lnk-dcs] ou [automaticamente através do serviço de aprovisionamento de dispositivos] [ lnk-dps].

* Para a configuração manual, anule os comentários do **manual** modo de aprovisionamento. Atualize o valor de **device_connection_string** com a cadeia de ligação do seu dispositivo de limite de IoT.

   ```yaml
   provisioning:
     source: "manual"
     device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   # provisioning: 
   #   source: "dps"
   #   global_endpoint: "https://global.azure-devices-provisioning.net"
   #   scope_id: "{scope_id}"
   #   registration_id: "{registration_id}"
   ```

* Para a configuração automática, anule os comentários do **dps** modo de aprovisionamento. Atualize os valores de **scope_id** e **registration_id** com os valores da sua instância de DPS do IoT Hub e o dispositivo de limite de IoT com TPM. 

   ```yaml
   # provisioning:
   #   source: "manual"
   #   device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   provisioning: 
     source: "dps"
     global_endpoint: "https://global.azure-devices-provisioning.net"
     scope_id: "{scope_id}"
     registration_id: "{registration_id}"
   ```

Obter o nome da utilização do dispositivo de limite `hostname` comando do PowerShell e defina-o como o valor de **hostname:** no yaml a configuração. Por exemplo:

```yaml
  ###############################################################################
  # Edge device hostname
  ###############################################################################
  #
  # Configures the environment variable 'IOTEDGE_GATEWAYHOSTNAME' injected into
  # modules.
  #
  ###############################################################################

  hostname: "edgedevice-1"
```

Em seguida, vamos precisar de fornecer o endereço ip e porta do **workload_uri** e **management_uri** no **ligar:** secção de configuração.

Para o endereço ip, introduza `ipconfig` na janela do PowerShell e selecione o endereço ip do **vEthernet (DockerNAT)**' interface conforme mostrado no exemplo abaixo (o endereço ip no seu sistema pode ser diferente):

![DockerNat][img-docker-nat]

```yaml
connect:
  management_uri: "http://10.0.75.1:15580"
  workload_uri: "http://10.0.75.1:15581"
```

Introduza os endereços do mesmos a **escutar:** secção de configuração. Por exemplo:

```yaml
listen:
  management_uri: "http://10.0.75.1:15580"
  workload_uri: "http://10.0.75.1:15581"
```

Na janela do PowerShell, criar uma variável de ambiente **IOTEDGE_HOST** com o **management_uri** endereço, exemplo:

```powershell
[Environment]::SetEnvironmentVariable("IOTEDGE_HOST", "http://10.0.75.1:15580")
```

Por fim, certifique-se do **rede:** em **moby_runtime:** uncommented e definido **azure-iot-edge**

```yaml
moby_runtime:
  docker_uri: "npipe://./pipe/docker_engine"
  network: "azure-iot-edge"
```

Guarde o ficheiro de configuração e reiniciar o serviço:

```powershell
Stop-Service iotedge -NoWait
sleep 5
Start-Service iotedge
```

## <a name="verify-successful-installation"></a>Certifique-se a instalação com êxito

Se utilizou o **configuração manual** passos na secção anterior, o tempo de execução do IoT limite deve ser aprovisionado e em execução no seu dispositivo com êxito. Se utilizou o **configuração automática** passos, em seguida, o que é necessário executar alguns passos adicionais para que o tempo de execução pode registar o seu dispositivo com o seu IoT hub em seu nome. Para os passos seguintes, consulte [criar e aprovisionar um dispositivo simulado do limite do TPM no Windows](how-to-auto-provision-simulated-device-windows.md#create-a-tpm-environment-variable).


Pode verificar o estado do serviço IoT limite por: 

```powershell
Get-Service iotedge
```

Examine os registos de serviço de últimos cinco minutos a utilizar:

```powershell

# Displays logs from last 5 min, newest at the bottom.

Get-WinEvent -ea SilentlyContinue `
  -FilterHashtable @{ProviderName= "iotedged";
    LogName = "application"; StartTime = [datetime]::Now.AddMinutes(-5)} |
  select TimeCreated, Message |
  sort-object @{Expression="TimeCreated";Descending=$false}
```

E lista de módulos com a executar:

```powershell
iotedge list
```

## <a name="next-steps"></a>Passos Seguintes

Se estiver a ter problemas com o tempo de execução de limite instalar corretamente, consulte o [resolução de problemas] [ lnk-trouble] página.


<!-- Images -->
[img-docker-nat]: ./media/how-to-install-iot-edge-windows-with-linux/dockernat.png

<!-- Links -->
[lnk-docker-config]: https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers
[lnk-dcs]: how-to-register-device-portal.md
[lnk-dps]: how-to-auto-provision-simulated-device-windows.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md
[lnk-docker-for-windows]: https://www.docker.com/docker-windows

