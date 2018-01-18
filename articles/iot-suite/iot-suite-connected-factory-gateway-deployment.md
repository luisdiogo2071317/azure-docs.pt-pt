---
title: "Implementar o gateway de fábrica ligado - Azure | Microsoft Docs"
description: "Como implementar um gateway no Windows ou Linux para ativar a conetividade à fábrica de ligado a solução pré-configurada."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/17/2018
ms.author: dobett
ms.openlocfilehash: 4606cb676c3ab7c8c8511579f43d251ff7d2ae8a
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2018
---
# <a name="deploy-an-edge-gateway-for-the-connected-factory-preconfigured-solution-on-windows-or-linux"></a>Implementar um gateway do limite para a solução de fábrica ligado pré-configurada no Windows ou Linux

Tem dois componentes de software para implementar um gateway do limite para o *fábrica ligada* solução pré-configurada:

- O *OPC Proxy* estabelece uma ligação a uma fábrica ligada. O Proxy de OPC, em seguida, aguarda para mensagens de comando e controlo de Browser OPC integrada que é executado no portal de solução de fábrica ligado.

- O *OPC publicador* estabelece ligação a servidores OPC UA no local existentes e reencaminha as mensagens de telemetria dos mesmos à fábrica ligada. Pode ligar um dispositivo clássico OPC a utilizar o [adaptador clássico OPC OPC UA](https://github.com/OPCFoundation/UA-.NETStandard/blob/master/ComIOP/README.md).

Ambos os componentes são open source e estão disponíveis como origem no GitHub e como contentores de Docker no DockerHub:

| GitHub | DockerHub |
| ------ | --------- |
| [Publicador OPC](https://github.com/Azure/iot-edge-opc-publisher) | [Publicador OPC](https://hub.docker.com/r/microsoft/iot-edge-opc-publisher/)   |
| [OPC Proxy](https://github.com/Azure/iot-edge-opc-proxy)         | [OPC Proxy](https://hub.docker.com/r/microsoft/iot-edge-opc-proxy/) |

Não é necessário um endereço IP destinado ao público ou portas de entrada abertas na firewall do gateway para o componente. Os componentes de OPC Proxy e OPC publicador utilizam apenas a porta 443 de saída.

Os passos neste artigo mostram como implementar um gateway de limite utilizando o Docker no Windows ou Linux. O gateway permite a conectividade com a solução de fábrica ligado pré-configurada. Também pode utilizar os componentes sem fábrica ligada.

> [!NOTE]
> Ambos os componentes podem ser utilizados como módulos no [Azure IoT Edge](https://github.com/Azure/iot-edge).

## <a name="choose-a-gateway-device"></a>Escolha um dispositivo de gateway

Se ainda não tiver um dispositivo de gateway, a Microsoft recomenda que comprar um gateway comercial a partir de um dos respetivos parceiros. Para obter uma lista de dispositivos de gateway compatíveis com a solução de fábrica ligado, visite o [catálogo de dispositivos do IoT do Azure](https://catalog.azureiotsuite.com/?q=opc). Siga as instruções que vêm com o dispositivo para configurar o gateway.

Em alternativa, utilize as seguintes instruções para configurar manualmente um dispositivo de gateway existente.

## <a name="install-and-configure-docker"></a>Instalar e configurar o Docker

Instalar [Docker para Windows](https://www.docker.com/docker-windows) no seu dispositivo de gateway baseado no Windows ou utilizar um Gestor de pacote para instalar o docker no seu dispositivo de gateway baseado em Linux.

Durante a configuração de Docker para Windows, selecione uma unidade no computador anfitrião para partilhar com Docker. A seguinte captura de ecrã mostra a partilha a **D** unidade no seu sistema Windows para permitir o acesso para a unidade de anfitrião a partir de dentro de um contentor de docker:

![Instalar Docker para Windows](./media/iot-suite-connected-factory-gateway-deployment/image1.png)

> [!NOTE]
> Também pode efetuar este passo depois de instalar o docker do **definições** caixa de diálogo. Clique com o botão direito no **Docker** ícone no tabuleiro de sistema do Windows e escolha **definições**. Se tiverem sido implementadas principais atualizações do Windows para o sistema, tais como os criadores de reversão do Windows update, unshare as unidades e partilhá-los novamente para atualizar os direitos de acesso.

Se estiver a utilizar o Linux, não é necessária nenhuma configuração adicional para ativar o acesso ao sistema de ficheiros.

No Windows, crie uma pasta no disco partilhado com o Docker, no Linux, crie uma pasta com o sistema de ficheiros de raiz. Estas instruções refere-se a esta pasta como `<SharedFolder>`.

Quando fizer referência ao `<SharedFolder>` num comando Docker, lembre-se de que utilize a sintaxe correta para o seu sistema operativo. Seguem-se dois exemplos, um para o Windows e outro para Linux:

- Se o tem com a pasta `D:\shared` no Windows como o `<SharedFolder>`, a sintaxe de comando de Docker é `d:/shared`.

- Se o tem com a pasta `/shared` no Linux como o `<SharedFolder>`, a sintaxe de comando de Docker é `/shared`.

Para obter mais informações, consulte o [utilizar volumes](https://docs.docker.com/engine/admin/volumes/volumes/) referência do motor de docker.

## <a name="configure-the-opc-components"></a>Configurar as componentes OPC

Antes de instalar os componentes OPC, conclua os seguintes passos para preparar o seu ambiente:

1. Para concluir a implementação do gateway, terá do **iothubowner** cadeia de ligação do IoT Hub na sua implementação de fábrica ligado. No [portal do Azure](http://portal.azure.com/), navegue até ao seu IoT Hub no grupo de recursos que criou quando implementou a solução de fábrica ligado. Clique em **políticas de acesso partilhado** para acesso a **iothubowner** cadeia de ligação:

    ![Localize a cadeia de ligação do IoT Hub](./media/iot-suite-connected-factory-gateway-deployment/image2.png)

    Copiar o **chave primária de cadeia de ligação** valor.

1. Para permitir a comunicação entre os contentores de docker, precisa de uma rede de bridge definido pelo utilizador. Para criar uma rede de bridge para os contentores, execute os seguintes comandos numa linha de comandos:

    ```cmd/sh
    docker network create -d bridge iot_edge
    ```

    Para verificar o **iot_edge** rede bridge foi criado, execute o seguinte comando:

    ```cmd/sh
    docker network ls
    ```

    O **iot_edge** rede bridge está incluído na lista de redes.

Para executar o publicador OPC, execute o seguinte comando numa linha de comandos:

```cmd/sh
docker run --rm -it -v <SharedFolder>:/docker -v x509certstores:/root/.dotnet/corefx/cryptography/x509stores --network iot_edge --name publisher -h publisher -p 62222:62222 --add-host <OpcServerHostname>:<IpAddressOfOpcServerHostname> microsoft/iot-edge-opc-publisher:2.1.3 publisher "<IoTHubOwnerConnectionString>" --lf /docker/publisher.log.txt --as true --si 1 --ms 0 --tm true --vc true --di 30
```

- O [OPC publicador GitHub](https://github.com/Azure/iot-edge-opc-publisher) e [docker run referência](https://docs.docker.com/engine/reference/run/) fornecem mais informações sobre:

  - As opções de linha de comandos do docker especificadas antes do nome do contentor (`microsoft/iot-edge-opc-publisher:2.1.3`).
  - O significado dos parâmetros de linha de comandos OPC publicador especificado após o nome do contentor (`microsoft/iot-edge-opc-publisher:2.1.3`).

- O `<IoTHubOwnerConnectionString>` é o **iothubowner** partilhado a cadeia de ligação de política de acesso do portal do Azure. Esta cadeia de ligação que copiou no passo anterior. Basta esta cadeia de ligação para a primeira execução da OPC publicador. No execuções subsequentes deve omiti-la porque esta opção origina um risco de segurança.

- O `<SharedFolder>` utilizar e a sintaxe é descrito na secção [instalar e configurar Docker](#install-and-configure-docker). Publicador de OPC utiliza o `<SharedFolder>` para ler e escrever no ficheiro de configuração do publicador OPC, escrever no ficheiro de registo e disponibilize ambos estes ficheiros fora do contentor.

- Publicador OPC lê a respetiva configuração a partir do **publishednodes.json** ficheiro, o que é lida e escrito para o `<SharedFolder>/docker` pasta. Este ficheiro de configuração define os dados de nó de OPC UA num servidor OPC UA determinado que publicador OPC deve subscrever. A sintaxe completa do **publishednodes.json** descrito no ficheiro de [OPC publicador](https://github.com/Azure/iot-edge-opc-publisher) página no GitHub. Quando adicionar um gateway, colocar vazio **publishednodes.json** para a pasta:

    ```json
    [
    ]
    ```

- Sempre que o servidor de OPC UA notifica OPC publicador uma alteração de dados, o novo valor é enviado ao IoT Hub. Consoante as definições de lotes, o publicador de OPC primeiro de Maio acumular os dados antes de enviar os dados ao IoT Hub num batch.

- Docker não suporta a resolução do nome NetBIOS, apenas resolução de nomes DNS. Se não tiver um servidor DNS na rede, pode utilizar a solução mostrada no exemplo anterior de linha de comandos. O anterior exemplo de linha de comandos utiliza o `--add-host` parâmetro para adicionar uma entrada para o ficheiro de anfitriões de contentores. Esta entrada permite a pesquisa de nome de anfitrião para a determinada `<OpcServerHostname>`, ao resolver para o endereço IP indicado `<IpAddressOfOpcServerHostname>`.

- OPC UA utiliza certificados x. 509 para autenticação e encriptação. Tem de colocar o certificado de publicador OPC no servidor de OPC UA que está a ligar, certifique-se de que confia OPC publicador. O arquivo de certificados do publicador OPC está localizado no `<SharedFolder>/CertificateStores` pasta. Pode encontrar o certificado de publicador OPC no `trusted/certs` pasta o `CertificateStores` pasta.

  Os passos para configurar o servidor de OPC UA dependem do dispositivo que estiver a utilizar. estes passos são normalmente documentados no manual de utilizador do servidor de OPC UA.

Para instalar o Proxy de OPC, execute o seguinte comando numa linha de comandos:

```cmd/sh
docker run -it --rm -v <SharedFolder>:/mapped --network iot_edge --name proxy --add-host <OpcServerHostname>:<IpAddressOfOpcServerHostname> microsoft/iot-edge-opc-proxy:1.0.2 -i -c "<IoTHubOwnerConnectionString>" -D /mapped/cs.db
```

Basta executar a instalação de uma vez num sistema.

Utilize o seguinte comando para executar o Proxy de OPC:

```cmd/sh
docker run -it --rm -v <SharedFolder>:/mapped --network iot_edge --name proxy --add-host <OpcServerHostname>:<IpAddressOfOpcServerHostname> microsoft/iot-edge-opc-proxy:1.0.2 -D /mapped/cs.db
```

Proxy de OPC guarda a cadeia de ligação durante a instalação. No execuções subsequentes deve omitir a cadeia de ligação porque esta opção origina um risco de segurança.

## <a name="enable-your-gateway"></a>Ativar o gateway

Conclua os seguintes passos para ativar o gateway na solução de fábrica ligado pré-configurada:

1. Quando ambos os componentes estão em execução, navegue para o **ligar o seu próprio servidor de UA OPC** página no portal de solução de fábrica ligado. Esta página apenas está disponível para administradores na solução. Introduza o URL de ponto final do fabricante (opc.tcp://publisher: 62222) e clique em **Connect**.

1. Estabelece uma relação de confiança entre o portal de fábrica ligado e OPC publicador. Quando vir um aviso de certificado, clique em **continuar**. Em seguida, verá um erro que o publicador OPC não confiar ao cliente UA Web. Para resolver este erro, copie o **UA Web cliente** certificado do `<SharedFolder>/CertificateStores/rejected/certs` pasta para o `<SharedFolder>/CertificateStores/trusted/certs` pasta no gateway. Não é necessário reiniciar o gateway.

Agora pode ligar ao gateway de nuvem e está pronto para adicionar servidores de OPC UA à solução.

## <a name="add-your-own-opc-ua-servers"></a>Adicionar os seus próprios servidores OPC UA

Para adicionar os seus próprios servidores OPC UA à fábrica de ligado solução pré-configurada:

1. Navegue para o **ligar o seu próprio servidor OPC UA** página no portal de solução de fábrica ligado.

    1. Inicie o servidor de OPC UA que pretende ligar. Certifique-se de que o servidor de OPC UA pode ser contactado de OPC publicador e o Proxy de OPC no contentor (consulte os comentários sobre a resolução de nome anteriores).
    1. Introduza o URL de ponto final do seu servidor de OPC UA (`opc.tcp://<host>:<port>`) e clique em **Connect**.
    1. Como parte da configuração de ligação, é estabelecida uma relação de confiança entre o portal de fábrica ligado (cliente OPC UA) e o servidor de OPC UA que está a tentar ligar. No dashboard do factory ligado obter um **não é possível verificar o certificado do servidor que pretende ligar** aviso. Quando vir um aviso de certificado, clique em **continuar**.
    1. Mais difícil de configuração é a configuração do certificado do servidor que está a tentar ligar ao OPC UA. Para servidores de OPC UA baseados no PC, apenas poderá receber uma caixa de diálogo de aviso no dashboard que pode confirmar. Para sistemas de servidor de OPC UA incorporados, consulte a documentação do seu servidor de OPC UA procurar como esta tarefa é efetuada. Para concluir esta tarefa, poderá ter o certificado de cliente de OPC UA o portal de fábrica ligado. Um administrador pode transferir este certificado no **ligar o seu próprio servidor OPC UA** página:

        ![Portal de solução](./media/iot-suite-connected-factory-gateway-deployment/image4.png)

1. Procurar a árvore de nós de OPC UA do seu servidor de OPC UA, faça duplo clique nós OPC que pretende enviar valores à fábrica ligada e selecione **publicar**.

1. Agora flui de telemetria do dispositivo de gateway. Pode ver a telemetria no **fábrica localizações** vista do portal fábrica ligados em **nova fábrica**.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre a arquitetura da solução fábrica ligado pré-configuradas, consulte [instruções sobre a solução pré-configurada de fábrica ligada](https://docs.microsoft.com/azure/iot-suite/iot-suite-connected-factory-sample-walkthrough).

Saiba mais sobre o [implementação de referência do publicador OPC](https://docs.microsoft.com/azure/iot-suite/iot-suite-connected-factory-publisher).