---
title: Suporte SSH para o serviço de aplicações do Azure no Linux | Microsoft Docs
description: Saiba como utilizar o SSH com o serviço de aplicações do Azure no Linux.
keywords: serviço de aplicações do Azure, aplicação web, linux, oss
services: app-service
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 66f9988f-8ffa-414a-9137-3a9b15a5573c
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: wesmc
ms.openlocfilehash: c2beb67a27b667d31402b903f38dbf116e9425d0
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
ms.locfileid: "34301080"
---
# <a name="ssh-support-for-azure-app-service-on-linux"></a>Suporte SSH para o serviço de aplicações do Azure no Linux

[Secure Shell (SSH)](https://wikipedia.org/wiki/Secure_Shell) costuma ser utilizada para executar comandos administrativos remotamente a partir de um terminal da linha de comandos. Serviço de aplicações no Linux fornece suporte SSH para o contentor de aplicação com cada uma das imagens de Docker incorporadas utilizadas para a pilha de tempo de execução de aplicações web novas. 

![Pilhas de tempo de execução](./media/app-service-linux-ssh-support/app-service-linux-runtime-stack.png)

Para imagens personalizadas dos Docker, através da configuração SSH servidor na sua imagem personalizada.

Também pode ligar ao contentor diretamente a partir do seu computador de desenvolvimento local através de SSH e SFTP.

## <a name="open-ssh-session-in-browser"></a>Abrir sessão SSH no browser

Para efetuar uma ligação de cliente SSH com o contentor, a aplicação deve estar em execução.

Cole o seguinte URL no browser e substituir \<APP_NAME>.azurewebsites.NET > com o nome da aplicação:

```
https://<app_name>.scm.azurewebsites.net/webssh/host
```

Se não está já autenticado, é necessário para autenticar com a sua subscrição do Azure para ligar. Uma vez autenticado, verá uma shell no browser, onde pode executar os comandos dentro do contentor.

![Ligação de SSH](./media/app-service-linux-ssh-support/app-service-linux-ssh-connection.png)

## <a name="use-ssh-support-with-custom-docker-images"></a>Utilizar o suporte SSH com imagens personalizadas do Docker

Ordem de uma imagem personalizada do Docker suportar a comunicação de SSH entre o contentor e o cliente no portal do Azure, execute os seguintes passos para a imagem de Docker.

Estes passos são apresentados no repositório de App Service do Azure como [um exemplo](https://github.com/Azure-App-Service/node/blob/master/6.9.3/).

1. Incluir o `openssh-server` instalação no [ `RUN` instrução](https://docs.docker.com/engine/reference/builder/#run) no Dockerfile para a imagem e defina a palavra-passe para a raiz da conta para `"Docker!"`.

    > [!NOTE]
    > Esta configuração não permite ligações externas ao contentor. SSH só pode ser acedido através do Kudu / SCM Site, o que é autenticado utilizando as credenciais de publicação.

    ```docker
    # ------------------------
    # SSH Server support
    # ------------------------
    RUN apt-get update \
        && apt-get install -y --no-install-recommends openssh-server \
        && echo "root:Docker!" | chpasswd
    ```

1. Adicionar um [ `COPY` instrução](https://docs.docker.com/engine/reference/builder/#copy) para Dockerfile para copiar uma [sshd_config](http://man.openbsd.org/sshd_config) do ficheiro para o */etc/ssh/* diretório. O ficheiro de configuração deve ser baseado no ficheiro sshd_config no repositório de GitHub do serviço de aplicações do Azure [aqui](https://github.com/Azure-App-Service/node/blob/master/8.2.1/sshd_config).

    > [!NOTE]
    > O *sshd_config* ficheiros têm de incluir o seguinte procedimento ou a ligação falhar: 
    > * `Ciphers` tem de incluir, pelo menos, um dos seguintes: `aes128-cbc,3des-cbc,aes256-cbc`.
    > * `MACs` tem de incluir, pelo menos, um dos seguintes: `hmac-sha1,hmac-sha1-96`.

    ```docker
    COPY sshd_config /etc/ssh/
    ```

1. Incluir porta 2222 no [ `EXPOSE` instrução](https://docs.docker.com/engine/reference/builder/#expose) para o Dockerfile. Embora a palavra-passe de raiz seja conhecida, não é possível aceder à porta 2222 a partir da Internet. É uma porta única interna acessível apenas por contentores dentro da rede de bridge de uma rede privada virtual.

    ```docker
    EXPOSE 2222 80
    ```

1. Certifique-se iniciar o serviço SSH utilizando um script de shell (consulte exemplo em [init_container.sh](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh)).

    ```bash
    #!/bin/bash
    service ssh start
    ```

O Dockerfile utiliza o [ `ENTRYPOINT` instrução](https://docs.docker.com/engine/reference/builder/#entrypoint) para executar o script.

    ```docker
    COPY startup /opt/startup
    ...
    RUN chmod 755 /opt/startup/init_container.sh
    ...
    ENTRYPOINT ["/opt/startup/init_container.sh"]
    ```

## <a name="open-ssh-session-from-remote-shell"></a>Abrir sessão SSH a partir da shell remota

> [!NOTE]
> Esta funcionalidade está atualmente em pré-visualização.
>

Através de TCP de túnel, podem criar uma ligação de rede entre o computador de desenvolvimento e a aplicação Web de contentores através de uma ligação de WebSocket autenticada. Permite-lhe abrir uma sessão SSH com o contentor em execução no App Service do cliente à sua escolha.

Para começar, tem de instalar [CLI do Azure](/cli/azure/install-azure-cli?view=azure-cli-latest). Para ver como funciona sem instalar a CLI do Azure, abra [Shell de nuvem do Azure](../../cloud-shell/overview.md). 

Adicionar a extensão de serviço de aplicações mais recente através da execução [Adicionar extensão az](/cli/azure/extension?view=azure-cli-latest#az-extension-add):

```azurecli-interactive
az extension add -–name webapp
```

Se já tiver executado `az extension add` anteriormente, execute [atualização az da extensão](/cli/azure/extension?view=azure-cli-latest#az-extension-update) em vez disso:

```azurecli-interactive
az extension update -–name webapp
```

Abrir uma ligação remota a sua aplicação ao utilizar o [az webapp remoto-ligação criar](/cli/azure/ext/webapp/webapp/remote-connection?view=azure-cli-latest#ext-webapp-az-webapp-remote-connection-create) comando. Especifique  _\<grupo\_nome >_ e \_< app\_nome > _ para a aplicação e substitua \<porta > com um número de porta local.

```azurecli-interactive
az webapp remote-connection create --resource-group <group_name> -n <app_name> -p <port> &
```

> [!TIP]
> `&` no final do comando é apenas para conveniência se estiver a utilizar a Shell de nuvem. Executa o processo em segundo plano, para que possa executar o seguinte comando na shell do mesmo.

A saída do comando dá-lhe as informações necessárias abrir uma sessão SSH.

```
Port 21382 is open
SSH is available { username: root, password: Docker! }
Start your favorite client and connect to port 21382
```

Abra uma sessão SSH com o contentor com o cliente à sua escolha, utilizando a porta local. O exemplo seguinte utiliza a predefinição [ssh](https://ss64.com/bash/ssh.html) comando:

```azurecli-interactive
ssh root@127.0.0.1 -p <port>
```

Quando lhe for pedido que está a ser, escreva `yes` para continuar a ligar. Em seguida, é-lhe pedido para a palavra-passe. Utilize `Docker!`, que foi apresentada anteriormente para si.

```
Warning: Permanently added '[127.0.0.1]:21382' (ECDSA) to the list of known hosts.
root@127.0.0.1's password:
```

Uma vez que está a ser autenticado, deverá ver o ecrã de boas-vindas da sessão.

```
  _____
  /  _  \ __________ _________   ____
 /  /_\  \___   /  |  \_  __ \_/ __ \
/    |    \/    /|  |  /|  | \/\  ___/
\____|__  /_____ \____/ |__|    \___  >
        \/      \/                  \/
A P P   S E R V I C E   O N   L I N U X

0e690efa93e2:~#
```

Agora está ligado ao seu conector. 

Tente executar a [superior](https://ss64.com/bash/top.html) comando. Deve conseguir ver o processo da sua aplicação na lista de processo. O resultado de exemplo abaixo, é a `PID 263`.

```
Mem: 1578756K used, 127032K free, 8744K shrd, 201592K buff, 341348K cached
CPU:   3% usr   3% sys   0% nic  92% idle   0% io   0% irq   0% sirq
Load average: 0.07 0.04 0.08 4/765 45738
  PID  PPID USER     STAT   VSZ %VSZ CPU %CPU COMMAND
    1     0 root     S     1528   0%   0   0% /sbin/init
  235     1 root     S     632m  38%   0   0% PM2 v2.10.3: God Daemon (/root/.pm2)
  263   235 root     S     630m  38%   0   0% node /home/site/wwwroot/app.js
  482   291 root     S     7368   0%   0   0% sshd: root@pts/0
45513   291 root     S     7356   0%   0   0% sshd: root@pts/1
  291     1 root     S     7324   0%   0   0% /usr/sbin/sshd
  490   482 root     S     1540   0%   0   0% -ash
45539 45513 root     S     1540   0%   0   0% -ash
45678 45539 root     R     1536   0%   0   0% top
45733     1 root     Z        0   0%   0   0% [init]
45734     1 root     Z        0   0%   0   0% [init]
45735     1 root     Z        0   0%   0   0% [init]
45736     1 root     Z        0   0%   0   0% [init]
45737     1 root     Z        0   0%   0   0% [init]
45738     1 root     Z        0   0%   0   0% [init]
```

## <a name="next-steps"></a>Passos Seguintes

Pode publicar perguntas e problemas no [fórum do Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

Para obter mais informações sobre a aplicação Web para contentores, consulte:

* [Introdução ao depuração remota de aplicações Node.js no Azure App Service a partir do código de VS](https://medium.com/@auchenberg/introducing-remote-debugging-of-node-js-apps-on-azure-app-service-from-vs-code-in-public-preview-9b8d83a6e1f0)
* [Como utilizar uma imagem personalizada do Docker para as Aplicações Web para Contentores](quickstart-docker-go.md)
* [Utilizar o .NET Core no Serviço de Aplicações do Azure no Linux](quickstart-dotnetcore.md)
* [Utilizar o Ruby no Serviço de Aplicações do Azure no Linux](quickstart-ruby.md)
* [FAQ sobre a Aplicação Web do Serviço de Aplicações do Azure para Contentores](app-service-linux-faq.md)