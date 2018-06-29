---
title: Como instalar o Azure IoT Edge no Linux | Microsoft Docs
description: Instruções de instalação do limite do IoT do Azure no Linux em ARM32
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: kgremban
ms.openlocfilehash: 2f25faa6957f9a8a42e64829e62506ae2add13ae
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37096163"
---
# <a name="install-azure-iot-edge-runtime-on-linux-arm32v7armhf"></a>Instalar o runtime do Azure IoT Edge no Linux (ARM32v7/armhf)

O tempo de execução do Azure IoT Edge é implementado em todos os dispositivos de limite de IoT. Tem três componentes. O **daemon de segurança de IoT Edge** fornece e mantém as normas de segurança do dispositivo de limite. O daemon de inicia a cada arranque e bootstraps o dispositivo ao iniciar o agente de limite de IoT. O **agente IoT Edge** facilita a implementação e monitorização dos módulos do dispositivo de limite, incluindo o hub IoT Edge. O **hub do IoT Edge** gere as comunicações entre os módulos no dispositivo do IoT Edge e entre o dispositivo e o Hub IoT.

Este artigo apresenta os passos para instalar o runtime do Azure IoT Edge num dispositivo de limite de Linux ARM32v7/armhf (por exemplo, Raspberry Pi).

>[!NOTE]
>Pacotes em repositórios de software do Linux estão sujeitos os termos de Licenciamento localizados em cada pacote (usr/partilhar documentos/*nome do pacote*). Leia os termos de licenciamento antes de utilizar o pacote. A instalação e utilização do pacote constitui a aceitação destes termos. Se não concordar com os termos de licenciamento, não utilize o pacote.

## <a name="install-the-container-runtime"></a>Instalar o runtime do contentor

Limite de IoT do Azure baseia-se num [OCI compatível] [ lnk-oci] tempo de execução do contentor (por exemplo, Docker). Se já tiver Docker CE/EE instalada no seu dispositivo de limite, pode continuar a utilizá-lo para desenvolvimento e testes com o Azure IoT Edge. 

Para cenários de produção, recomenda-se vivamente que utilize o [baseado em Moby] [ lnk-moby] motor fornecida abaixo. É o motor de contentor só oficialmente suportado com limite de IoT do Azure. As imagens de contentor do docker CE/EE são totalmente compatíveis com o tempo de execução Moby.

Comandos abaixo instalar moby motor e a interface de linha de comandos (CLI). A CLI é útil para desenvolvimento, mas é opcional para implementações de produção.

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

## <a name="install-the-iot-edge-security-daemon"></a>Instalar o Daemon de segurança de IoT Edge

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

O daemon de pode ser configurado utilizando o ficheiro de configuração em `/etc/iotedge/config.yaml` o dispositivo de limite pode ser configurado <!--[automatically via Device Provisioning Service][lnk-dps] or--> manualmente utilizando um [a cadeia de ligação do dispositivo][lnk-dcs].

Para a configuração manual, introduza a cadeia de ligação do dispositivo no **aprovisionamento** secção **config.yaml**

```yaml
provisioning:
  source: "manual"
  device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
```

*O ficheiro está protegida contra escrita por predefinição, poderá ter de utilizar `sudo` editá-lo. Por exemplo `sudo nano /etc/iotedge/config.yaml`*

Depois de introduzir as informações de aprovisionamento na configuração, reinicie o daemon de:

```cmd/sh
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Certifique-se a instalação com êxito

Pode verificar o estado da utilização de Daemon de limite de IoT:

```cmd/sh
systemctl status iotedge
```

Examine os registos de daemon utilizando:

```cmd/sh
journalctl -u iotedge --no-pager --no-full
```

E lista de módulos com a executar:

```cmd/sh
sudo iotedge list
```

## <a name="next-steps"></a>Passos Seguintes

Se estiver a ter problemas com o tempo de execução de limite instalar corretamente, consulte o [resolução de problemas] [ lnk-trouble] página.

<!-- Links -->
[lnk-dcs]: ../iot-hub/quickstart-send-telemetry-dotnet.md#register-a-device
[lnk-dps]: how-to-simulate-dps-tpm.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md