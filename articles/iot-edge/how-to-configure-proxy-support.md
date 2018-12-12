---
title: Configurar dispositivos para os proxies de rede - Azure IoT Edge | Documentos da Microsoft
description: Como configurar o tempo de execução do Azure IoT Edge e quaisquer módulos do IoT Edge de acesso à internet para comunicar através de um servidor proxy.
author: kgremban
manager: ''
ms.author: kgremban
ms.date: 11/01/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: a7d32c98b77568e02fa14b70e969eeb254989062
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53100485"
---
# <a name="configure-an-iot-edge-device-to-communicate-through-a-proxy-server"></a>Configurar um dispositivo IoT Edge para comunicar através de um servidor proxy

Os dispositivos de IoT Edge enviam pedidos HTTPS para comunicar com IoT Hub. Se o dispositivo estiver ligado a uma rede que utiliza um servidor proxy, tem de configurar o runtime do IoT Edge para comunicar através do servidor. Servidores proxy também podem afetar os módulos do IoT Edge individuais se efetuam os pedidos HTTP ou HTTPS que não são encaminhados através do hub do Edge. 

Configurar um dispositivo IoT Edge para trabalhar com um servidor proxy, segue estas etapas básicas: 

1. Instale o runtime do IoT Edge no dispositivo. 
2. Configure o daemon do Docker e o daemon de IoT Edge no seu dispositivo para utilizar o servidor proxy.
3. Configure as propriedades de edgeAgent no arquivo config.yaml no seu dispositivo.
4. Defina variáveis de ambiente para o runtime do IoT Edge e outros do IoT Edge módulos no manifesto de implantação. 

## <a name="know-your-proxy-url"></a>Saber o URL de proxy

Para configurar o daemon do Docker e o IoT Edge no seu dispositivo, precisa saber o URL de proxy. 

URLs de proxy efetuar o seguinte formato: **protocolo**://**proxy_host**:**porta_proxy**. 

* O **protocolo** é HTTP ou HTTPS. O daemon do Docker pode ser configurado com qualquer protocolo, dependendo das suas definições de registo de contentor, mas os contentores de daemon e o runtime do IoT Edge devem sempre utilizar HTTPS.

* O **proxy_host** é um endereço do servidor proxy. Se o servidor proxy requer autenticação, pode fornecer as suas credenciais como parte da proxy_host no formato **usuário**:**palavra-passe**@**proxy_host**. 

* O **porta_proxy** é a porta de rede em que o proxy responde ao tráfego de rede. 

## <a name="install-the-runtime"></a>Instalar o tempo de execução

Se estiver a instalar o runtime do IoT Edge num dispositivo Linux, configure o Gestor de pacotes para passar pelo seu servidor de proxy para aceder ao pacote de instalação. Por exemplo, [definir apt-get para utilizar um proxy http](https://help.ubuntu.com/community/AptGet/Howto/#Setting_up_apt-get_to_use_a_http-proxy). Depois de configurar o seu Gestor de pacote, siga as instruções em [tempo de execução de instalar o Azure IoT Edge no Linux (ARM32v7/armhf)](how-to-install-iot-edge-linux-arm.md) ou [instalar o runtime do Azure IoT Edge no Linux (x64)](how-to-install-iot-edge-linux.md) como de costume. 

Se estiver a instalar o runtime do IoT Edge num dispositivo Windows, terá de percorrer o servidor de proxy para aceder ao pacote de instalação. Pode configurar as informações de proxy nas definições do Windows, ou incluir as informações de proxy diretamente no script de instalação. O seguinte script do powershell é um exemplo de uma instalação do windows usando o `-proxy` argumento:

```powershell
. {Invoke-WebRequest -proxy <proxy URL> -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Manual -ContainerOs Windows
```

Para obter mais opções de instalação e informações, consulte [runtime de instalar o Azure IoT Edge no Windows para utilização com contentores do Windows](how-to-install-iot-edge-windows-with-windows.md) ou [runtime de instalar o Azure IoT Edge no Windows para utilização com contentores de Linux](how-to-install-iot-edge-windows-with-linux.md).

Depois de instalar o runtime do IoT Edge, utilize a secção seguinte para configurá-lo com as suas informações de proxy. 

## <a name="configure-the-daemons"></a>Configurar os daemons

Os daemons do Docker e do IoT Edge em execução no seu dispositivo IoT Edge tem de ser configurado para utilizar o servidor proxy. O daemon do Docker faz solicitações da web para extrair imagens de contentores de registos de contentores. O daemon de IoT Edge faz solicitações da web para comunicar com IoT Hub.

### <a name="docker-daemon"></a>Daemon do docker

Consulte a documentação do Docker para configurar o daemon do Docker com variáveis de ambiente. A maioria dos registos de contentores (incluindo DockerHub e registos de contentores do Azure) suportam pedidos HTTPS, pelo que é o parâmetro que deve definir **HTTPS_PROXY**. Se está obtendo imagens a partir de um registo que não suporta a segurança de camada de transporte (TLS), então deve definir os **HTTP_PROXY** parâmetro. 

Escolha o artigo que aplica-se a sua versão do Docker: 

* [Docker](https://docs.docker.com/config/daemon/systemd/#httphttps-proxy)
* [Docker para Windows](https://docs.docker.com/docker-for-windows/#proxies)

### <a name="iot-edge-daemon"></a>Daemon de IoT Edge

O daemon de IoT Edge é configurado de forma semelhante para o daemon do Docker. Todos os pedidos do IoT Edge envia para o IoT Hub utilizam HTTPS. Utilize os seguintes passos para definir uma variável de ambiente para o serviço, com base no seu sistema operativo. 

#### <a name="linux"></a>Linux

Abra um editor no terminal para configurar o daemon de IoT Edge. 

```bash
sudo systemctl edit iotedge
```

Introduza o texto seguinte, substituindo  **\<URL de proxy >** com o seu endereço do servidor proxy e porta. Em seguida, guarde e saia. 

```text
[Service]
Environment="https_proxy=<proxy URL>"
```

Atualize o service manager para recolher a nova configuração para iotedge.

```bash
sudo systemctl daemon-reload
```

Reinicie o IoT Edge, para que as alterações entrem em vigor.

```bash
sudo systemctl restart iotedge
```

Certifique-se de que a variável de ambiente foi criada e a nova configuração foi carregada. 

```bash
systemctl show --property=Environment iotedge
```

#### <a name="windows"></a>Windows

Abra uma janela do PowerShell como administrador e execute o seguinte comando para editar o registo com a nova variável de ambiente. Substitua  **\<url de proxy >** com o seu endereço do servidor proxy e porta. 

```powershell
reg add HKLM\SYSTEM\CurrentControlSet\Services\iotedge /v Environment /t REG_MULTI_SZ /d https_proxy=<proxy URL>
```

Reinicie o IoT Edge, para que as alterações entrem em vigor.

```powershell
Restart-Service iotedge
```

## <a name="configure-the-edge-agent"></a>Configurar o agente do Edge

O agente do Edge é o módulo de primeira para começar em qualquer dispositivo IoT Edge. Ele é iniciado pela primeira vez com base nas informações no ficheiro de config.yaml do IoT Edge. O agente do Edge, em seguida, liga ao IoT Hub para obter os manifestos de implantação, declarar o que outros módulos devem ser implementado no dispositivo.

Abra o ficheiro de config.yaml no seu dispositivo IoT Edge. Em sistemas Linux, este ficheiro está localizado em **/etc/iotedge/config.yaml**. Em sistemas Windows, este ficheiro está localizado em **C:\ProgramData\iotedge\config.yaml**. O ficheiro de configuração está protegido, pelo que necessita de privilégios administrativos para aceder ao mesmo. Em sistemas Linux, isso significa usar o `sudo` comando antes de abrir o ficheiro no seu editor de texto preferido. No Windows, isso significa que abrir o editor de texto, como o bloco de notas para executar como administrador e, em seguida, abrir o ficheiro. 

No ficheiro config.yaml, localize a **especificação do módulo de agente do Edge** secção. A definição de agente do Edge inclui uma **env** parâmetro onde pode adicionar variáveis de ambiente. 

<!--
![edgeAgent definition](./media/how-to-configure-proxy-support/edgeagent-unedited.png)
-->

Remova as chavetas que marcadores de posição para o parâmetro env e adicionar a nova variável numa nova linha. Lembre-se de que recua no YAML é dois espaços. 

```yaml
https_proxy: "<proxy URL>"
```

O runtime do IoT Edge utiliza o AMQP por predefinição para comunicar com o IoT Hub. Alguns servidores de proxy bloqueiam portas AMQP. Se for esse o caso, em seguida, terá também de configurar edgeAgent para utilizar o AMQP sobre WebSocket. Adicione uma segunda variável de ambiente.

```yaml
UpstreamProtocol: "AmqpWs"
```

![definição de edgeAgent com variáveis de ambiente](./media/how-to-configure-proxy-support/edgeagent-edited.png)

Guardar as alterações à config.yaml e feche o editor. Reinicie o IoT Edge, para que as alterações entrem em vigor. 

* Linux: 

   ```bash
   sudo systemctl restart iotedge
   ```

* Windows:

   ```powershell
   Restart-Service iotedge
   ```

## <a name="configure-deployment-manifests"></a>Configurar manifestos de implantação  

Assim que o seu dispositivo IoT Edge está configurado para trabalhar com o servidor proxy, precisa também declarar as variáveis de ambiente em todos os manifestos de implantação futuras. Os dois módulos de tempo de execução, edgeAgent e edgeHub, deve ter sempre o servidor proxy configurado para manter a comunicação com o IoT Hub. Pode configurar qualquer módulo do IoT Edge para comunicar através de um servidor proxy, mas não é necessário para módulos que rotear suas mensagens por meio de edgeHub ou que só comunicam com outros módulos no dispositivo. 

Pode criar os manifestos de implantação com o portal do Azure ou manualmente, editar um ficheiro JSON. 

### <a name="azure-portal"></a>Portal do Azure

Quando utiliza a **definir módulos** Assistente para criar implementações para o IoT Edge dispositivos, cada módulo é um **variáveis de ambiente** secção que pode utilizar para configurar ligações de servidor proxy. 

Para configurar o agente do Edge e os módulos de hub do Edge, selecione **configurar definições de Runtime do Edge avançadas** no primeiro passo do assistente. 

![Configurar as definições avançadas de Runtime do Edge](./media/how-to-configure-proxy-support/configure-runtime.png)

Adicionar a **https_proxy** variável de ambiente para o agente do Edge e definições do módulo do Edge hub. Se tiver incluído os **UpstreamProtocol** variável de ambiente no arquivo config.yaml no seu dispositivo IoT Edge, adicioná-la para a definição de módulo de agente do Edge demasiado. 

![Variável de ambiente de https_proxy de conjunto](./media/how-to-configure-proxy-support/edgehub-environmentvar.png)

Todos os outros módulos que adicionar a um manifesto de implantação seguem o mesmo padrão. Na página em que definir o nome do módulo e a imagem, há uma secção de variáveis de ambiente.

### <a name="json-deployment-manifest-files"></a>Ficheiros de manifesto de implantação de JSON

Se criar implementações para o IoT Edge em dispositivos com os modelos no Visual Studio Code ou criando manualmente os ficheiros JSON, pode adicionar as variáveis de ambiente diretamente para cada definição de módulo. 

Utilize o seguinte formato JSON: 

```json
"env": {
    "https_proxy": {
        "value": "<proxy URL>"
    }
}
```

Com as variáveis de ambiente incluídas, a definição de módulo deve ter um aspeto semelhante ao seguinte exemplo de edgeHub:

```json
"edgeHub": {
    "type": "docker",
    "settings": {
        "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
        "createOptions": ""
    },
    "env": {
        "https_proxy": {
            "value": "https://proxy.example.com:3128"
        }
    },
    "status": "running",
    "restartPolicy": "always"
}
```

Se tiver incluído os **UpstreamProtocol** variável de ambiente no arquivo confige.yaml no seu dispositivo IoT Edge, adicioná-la para a definição de módulo de agente do Edge demasiado. 

```json
"env": {
    "https_proxy": {
        "value": "<proxy URL>"
    },
    "UpstreamProtocol": {
        "value": "AmqpWs"
    }
}
```

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as funções do [runtime do IoT Edge](iot-edge-runtime.md).

Resolver erros de instalação e configuração com [problemas comuns e resoluções para o Azure IoT Edge](troubleshoot.md)

