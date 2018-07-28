---
title: Como instalar o Azure IoT Edge no Windows com contentores do Linux | Documentos da Microsoft
description: O Azure IoT Edge as instruções de instalação no Windows com contentores do Linux
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: kgremban
ms.openlocfilehash: a56b2b12143a29637196d2239f648b78f1f8e763
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39307870"
---
# <a name="install-azure-iot-edge-runtime-on-windows-to-use-with-linux-containers"></a>Instalar o runtime do Azure IoT Edge no Windows para utilização com contentores do Linux

O tempo de execução do Azure IoT Edge é implementado em todos os dispositivos do IoT Edge. Tem três componentes. O **daemon de segurança de IoT Edge** fornece e mantém as normas de segurança no dispositivo Edge. O daemon começa em cada inicialização e inicializa o dispositivo ao iniciar o agente do IoT Edge. O **agente do IoT Edge** facilita a implementação e monitorização de módulos no dispositivo do Edge, incluindo o hub IoT Edge. O **hub do IoT Edge** gere as comunicações entre os módulos no dispositivo do IoT Edge e entre o dispositivo e o Hub IoT.

Este artigo lista os passos para instalar o runtime do Azure IoT Edge no seu Windows x64 (AMD/Intel) system. Suporte do Windows está atualmente em pré-visualização.

>[!NOTE]
Através de contentores de Linux no Windows sytems não é uma configuração de produção recomendada ou suportado para o Azure IoT Edge. No entanto, ele pode ser usado para fins de testes e de desenvolvimento.

## <a name="supported-windows-versions"></a>Versões suportadas do Windows
O Azure IoT Edge pode ser utilizada para o desenvolvimento e teste nas seguintes versões do Windows, ao utilizar contentores do Linux:
  * Windows 10 ou sistemas operativos de ambiente de trabalho mais recentes.
  * Windows Server 2016 ou novos sistemas operacionais de servidor.

## <a name="install-the-container-runtime"></a>Instalar o runtime de contentor 

O Azure IoT Edge se baseia numa [compatível com o OCI] [ lnk-oci] tempo de execução do contentor (por exemplo, o Docker). 

Pode usar [Docker para Windows] [ lnk-docker-for-windows] para desenvolvimento e teste. Certifique-se de que é o Docker para Windows [configurado para utilizar contentores do Linux][lnk-docker-config]

## <a name="install-the-azure-iot-edge-security-daemon"></a>Instale o Daemon de segurança do Azure IoT Edge

>[!NOTE]
>Pacotes de software do Azure IoT Edge são sujeita aos termos de licença localizados nos pacotes (no diretório de licença). Leia os termos de licença antes de utilizar o pacote. A instalação e a utilização do pacote constitui a sua aceitação destes termos. Se não concordar com os termos de licenciamento, não utilize o pacote.

### <a name="download-the-edge-daemon-package-and-install"></a>Transferir o pacote de daemon de borda e instalar

Numa janela do PowerShell de administrador, execute os seguintes comandos:

```powershell
Invoke-WebRequest https://aka.ms/iotedged-windows-latest -o .\iotedged-windows.zip
Expand-Archive .\iotedged-windows.zip C:\ProgramData\iotedge -f
Move-Item c:\ProgramData\iotedge\iotedged-windows\* C:\ProgramData\iotedge\ -Force
rmdir C:\ProgramData\iotedge\iotedged-windows
$sysenv = "HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment"
$path = (Get-ItemProperty -Path $sysenv -Name Path).Path + ";C:\ProgramData\iotedge"
Set-ItemProperty -Path $sysenv -Name Path -Value $path
```

Instale o vcruntime usando:

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

Criar uma **iotedge.reg** com o seguinte conteúdo de ficheiros e importar para o registo do Windows ao fazer duplo clique ou utilizar o `reg import iotedge.reg` comando:

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\iotedged]
"CustomSource"=dword:00000001
"EventMessageFile"="C:\\ProgramData\\iotedge\\iotedged.exe"
"TypesSupported"=dword:00000007
```

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Configurar o Daemon de segurança do Azure IoT Edge

O daemon de pode ser configurado com o ficheiro de configuração em `C:\ProgramData\iotedge\config.yaml`.

O dispositivo de limite pode ser configurado manualmente usando um [cadeia de ligação do dispositivo] [ lnk-dcs] ou [automaticamente através do serviço aprovisionamento de dispositivos] [ lnk-dps].

* Para a configuração manual, anule os comentários da **manual** modo de aprovisionamento. Atualize o valor de **device_connection_string** com a cadeia de ligação do seu dispositivo IoT Edge.

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

* Para a configuração automática, anule os comentários da **dps** modo de aprovisionamento. Atualize os valores **scope_id** e **registration_id** com os valores da sua instância de pontos de distribuição do IoT Hub e o seu dispositivo IoT Edge com o TPM. 

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

Obter o nome da utilização de dispositivo do edge `hostname` comando no PowerShell e defina-o como o valor de **nome de anfitrião:** no yaml de configuração. Por exemplo:

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

Em seguida, forneça o endereço IP e porta para **workload_uri** e **management_uri** no **ligar:** e **escutar:** secções das configuração.

Para obter seu endereço IP, introduza `ipconfig` na janela do PowerShell. Copie o endereço IP dos **vEthernet (DockerNAT)**"interface conforme mostrado no exemplo seguinte (o endereço ip no seu sistema pode ser diferente):

![DockerNat][img-docker-nat]

Atualização do **workload_uri** e **management_uri** no **ligar:** secção do ficheiro de configuração. Substitua **\<GATEWAY_ADDRESS\>** pelo endereço IP de DockerNAT que copiou. 

```yaml
connect:
  management_uri: "http://<GATEWAY_ADDRESS>:15580"
  workload_uri: "http://<GATEWAY_ADDRESS>:15581"
```

Introduza os endereços do mesmo o **escutar:** secção.

```yaml
listen:
  management_uri: "http://<GATEWAY_ADDRESS>:15580"
  workload_uri: "http://<GATEWAY_ADDRESS>:15581"
```

Na janela do PowerShell, crie uma variável de ambiente **IOTEDGE_HOST** com o **management_uri** endereço.

```powershell
[Environment]::SetEnvironmentVariable("IOTEDGE_HOST", "http://<GATEWAY_ADDRESS>:15580")
```

Mantenha a variável de ambiente entre reinícios.

```powershell
SETX /M IOTEDGE_HOST "http://<GATEWAY_ADDRESS>:15580"
```

Por fim, certifique-se a **rede:** em **moby_runtime:** uncommented e definido **azure-iot-edge**

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

Se tiver utilizado o **configuração manual** passos na secção anterior, o runtime do IoT Edge deve estar em execução no seu dispositivo e aprovisionada com êxito. Se tiver utilizado o **a configuração automática** passos, tem de concluir alguns passos adicionais para que o tempo de execução pode registar o seu dispositivo com o seu hub IoT em seu nome. Para os passos seguintes, veja [criar e aprovisionar um dispositivo de periferia de TPM simulado no Windows](how-to-auto-provision-simulated-device-windows.md#create-a-tpm-environment-variable).


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

## <a name="next-steps"></a>Passos Seguintes

Se tiver problemas com o runtime do Edge Check-out a corretamente, instalar o [resolução de problemas] [ lnk-trouble] página.


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

