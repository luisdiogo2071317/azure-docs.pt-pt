---
title: Como instalar o Azure IoT Edge no Linux | Documentos da Microsoft
description: Instruções de instalação do Azure IoT Edge no Linux
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: kgremban
ms.openlocfilehash: 0174aa2288bbb95cc5cfc796446893fde00a8964
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/02/2018
ms.locfileid: "37344356"
---
# <a name="install-the-azure-iot-edge-runtime-on-linux-x64"></a>Instalar o runtime do Azure IoT Edge no Linux (x64)

O tempo de execução do Azure IoT Edge é implementado em todos os dispositivos do IoT Edge. Ele tem três componentes. O **daemon de segurança de IoT Edge** fornece e mantém as normas de segurança no dispositivo Edge. O daemon começa em cada inicialização e inicializa o dispositivo ao iniciar o agente do IoT Edge. O **agente do IoT Edge** facilita a implementação e monitorização de módulos no dispositivo do Edge, incluindo o hub IoT Edge. O **hub do IoT Edge** gere as comunicações entre os módulos no dispositivo do IoT Edge e entre o dispositivo e o Hub IoT.

Este artigo lista os passos para instalar o runtime do Azure IoT Edge no seu Linux x64 (Intel/AMD) dispositivo Edge.

>[!NOTE]
>Pacotes em repositórios de software do Linux estão sujeita aos termos de licença localizados em cada pacote (/ usr/partilhar/doc/*nome do pacote*). Leia os termos de licença antes de utilizar o pacote. A instalação e a utilização do pacote constitui a sua aceitação destes termos. Se não concordar com os termos de licenciamento, não utilize o pacote.

## <a name="register-microsoft-key-and-software-repository-feed"></a>Registar o feed de repositório de chave e o software Microsoft

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```cmd/sh
# Install repository configuration
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/

# Install Microsoft GPG public key
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

### <a name="ubuntu-1804"></a>Ubuntu 18.04

```cmd/sh
# Install repository configuration
curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/

# Install Microsoft GPG public key
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/

# Perform apt upgrade
sudo apt-get upgrade
```

## <a name="install-the-container-runtime"></a>Instalar o runtime de contentor 

O Azure IoT Edge se baseia numa [compatível com o OCI] [ lnk-oci] tempo de execução do contentor (por exemplo, o Docker). Se já tiver o Docker CE/EE instalada no seu dispositivo do Edge, pode continuar a utilizá-lo para desenvolvimento e teste com o Azure IoT Edge. 

Para cenários de produção, é altamente recomendado que utilize o [com base em Moby] [ lnk-moby] motor fornecido abaixo. É o mecanismo de contentor único oficialmente suportado com o Azure IoT Edge. Imagens de contentor do docker CE/EE são totalmente compatíveis com o tempo de execução Moby.

Atualize apt-get.

```bash
sudo apt-get update
```

Instale o motor de Moby e a interface de linha de comandos (CLI). A CLI é útil para desenvolvimento mas opcional para deployments.* de produção

```bash
sudo apt-get install moby-engine
sudo apt-get install moby-cli
```

## <a name="install-the-azure-iot-edge-security-daemon"></a>Instale o Daemon de segurança do Azure IoT Edge

Comandos abaixo irão também instalar a versão padrão dos **iothsmlib** se ainda não estiver presente.

```bash
sudo apt-get update
sudo apt-get install iotedge
```

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Configurar o Daemon de segurança do Azure IoT Edge

O daemon de pode ser configurado com o ficheiro de configuração em `/etc/iotedge/config.yaml`. O ficheiro é protegida contra escrita por padrão, poderá ter permissões elevadas para editá-lo.

```bash
sudo nano /etc/iotedge/config.yaml
```

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

Após introduzir as informações de aprovisionamento na configuração, reinicie o daemon:

```cmd/sh
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Certifique-se a instalação com êxito

Se tiver utilizado o **configuração manual** passos na secção anterior, o runtime do IoT Edge deve estar em execução no seu dispositivo e aprovisionada com êxito. Se tiver utilizado o **a configuração automática** passos, tem de concluir alguns passos adicionais para que o tempo de execução pode registar o seu dispositivo com o seu hub IoT em seu nome. Para os passos seguintes, veja [criar e aprovisionar um dispositivo de periferia de TPM simulado numa máquina virtual Linux](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm).

Pode verificar o estado do IoT Edge Daemon:

```cmd/sh
systemctl status iotedge
```

Examine os registos de daemon com:

```cmd/sh
journalctl -u iotedge --no-pager --no-full
```

E lista de módulos com a executar:

```cmd/sh
sudo iotedge list
```

## <a name="next-steps"></a>Passos Seguintes

Se tiver problemas com o runtime do Edge Check-out a corretamente, instalar o [resolução de problemas] [ lnk-trouble] página.

<!-- Links -->
[lnk-dcs]: how-to-register-device-portal.md
[lnk-dps]: how-to-auto-provision-simulated-device-linux.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md
