---
title: Como instalar o Azure IoT Edge no Linux | Documentos da Microsoft
description: Instruções de instalação do Azure IoT Edge no Linux no ARM32
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 08/14/2018
ms.author: kgremban
ms.openlocfilehash: 7720e0471c6d8f2ba20f28753773829a28f93c7a
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/16/2018
ms.locfileid: "42054795"
---
# <a name="install-azure-iot-edge-runtime-on-linux-arm32v7armhf"></a>Instalar o runtime do Azure IoT Edge no Linux (ARM32v7/armhf)

O tempo de execução do Azure IoT Edge é implementado em todos os dispositivos do IoT Edge. Tem três componentes. O **daemon de segurança de IoT Edge** fornece e mantém as normas de segurança no dispositivo Edge. O daemon começa em cada inicialização e inicializa o dispositivo ao iniciar o agente do IoT Edge. O **agente do IoT Edge** facilita a implementação e monitorização de módulos no dispositivo do Edge, incluindo o hub IoT Edge. O **hub do IoT Edge** gere as comunicações entre os módulos no dispositivo do IoT Edge e entre o dispositivo e o Hub IoT.

Este artigo lista os passos para instalar o runtime do Azure IoT Edge num dispositivo Edge Linux ARM32v7/armhf (por exemplo, o Raspberry Pi).

>[!NOTE]
>Pacotes em repositórios de software do Linux estão sujeita aos termos de licença localizados em cada pacote (/ usr/partilhar/doc/*nome do pacote*). Leia os termos de licença antes de utilizar o pacote. A instalação e a utilização do pacote constitui a sua aceitação destes termos. Se não concordar com os termos de licenciamento, não utilize o pacote.

## <a name="install-the-container-runtime"></a>Instalar o runtime de contentor

O Azure IoT Edge se baseia numa [compatível com o OCI] [ lnk-oci] tempo de execução do contentor. Para cenários de produção, é altamente recomendado que utilize o [com base em Moby] [ lnk-moby] motor fornecido abaixo. É o mecanismo de contentor único oficialmente suportado com o Azure IoT Edge. Imagens de contentor do docker CE/EE são compatíveis com o tempo de execução com base em Moby.

Comandos abaixo instala o mecanismo baseado em Moby e interface de linha de comandos (CLI). A CLI é útil para desenvolvimento mas opcional para implementações de produção.

```cmd/sh

# You can copy the entire text from this code block and 
# paste in terminal. The comment lines will be ignored.

# Download and install the moby-engine
curl -L https://aka.ms/moby-engine-armhf-latest -o moby_engine.deb && sudo dpkg -i ./moby_engine.deb

# Download and install the moby-cli
curl -L https://aka.ms/moby-cli-armhf-latest -o moby_cli.deb && sudo dpkg -i ./moby_cli.deb

# Run apt-get fix
sudo apt-get install -f

```

## <a name="install-the-iot-edge-security-daemon"></a>Instale o Daemon de segurança de IoT Edge

```cmd/sh
# You can copy the entire text from this code block and 
# paste in terminal. The comment lines will be ignored.

# Download and install the standard libiothsm implementation
curl -L https://aka.ms/libiothsm-std-linux-armhf-latest -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb

# Download and install the IoT Edge Security Daemon
curl -L https://aka.ms/iotedged-linux-armhf-latest -o iotedge.deb && sudo dpkg -i ./iotedge.deb

# Run apt-get fix
sudo apt-get install -f
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

Guarde e feche o ficheiro. 

   `CTRL + X`, `Y`, `Enter`

Após introduzir as informações de aprovisionamento na configuração, reinicie o daemon:

```cmd/sh
sudo systemctl restart iotedge
```

>[!TIP]
>São necessários privilégios elevados para executar `iotedge` comandos. Depois de terminar sessão do seu computador e inicie sessão novamente na primeira vez depois de instalar o runtime do IoT Edge, as suas permissões são atualizadas automaticamente. Até lá, utilize **sudo** à frente os comandos. 

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
>[!NOTE]
>No recurso de dispositivos como RaspberryPi restrita, é altamente recomendável que *OptimizeForPerformance* variável de ambiente está definida como *false* de acordo com as instruções no [ Guia de resolução de problemas.][lnk-trouble]


## <a name="next-steps"></a>Passos Seguintes

Se tiver problemas com o runtime do Edge Check-out a corretamente, instalar o [resolução de problemas] [ lnk-trouble] página.

<!-- Links -->
[lnk-dcs]: how-to-register-device-portal.md
[lnk-dps]: how-to-auto-provision-simulated-device-linux.md
[lnk-trouble]: https://docs.microsoft.com/azure/iot-edge/troubleshoot#stability-issues-on-resource-constrained-devices
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
