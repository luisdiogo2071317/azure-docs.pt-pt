---
title: "Implementar a solução de monitorização remota localmente - Azure | Microsoft Docs"
description: "Este tutorial mostra como implementar a solução pré-configurada de monitorização remota no seu computador local para desenvolvimento e teste."
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 03/07/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 77f40e2f10cbdb9930a22a4248e19bb3356af7af
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/12/2018
---
# <a name="deploy-the-remote-monitoring-preconfigured-solution-locally"></a>Implementar a solução monitorização remota pré-configurada localmente

Este artigo mostra como implementar a solução pré-configurada de monitorização remota no seu computador local para desenvolvimento e teste. Esta abordagem implementa os micro-serviços para um contentor de Docker local e utiliza o IoT Hub, base de dados do Cosmos e serviços de armazenamento do Azure na nuvem. Utilize as soluções pré-configuradas (PCS) CLI para implementar os serviços em nuvem do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Para implementar os serviços do Azure utilizados pela solução pré-configurada de monitorização remota, terá de uma subscrição do Azure Active Directory.

Se não tiver uma conta, pode criar uma de avaliação gratuita em apenas alguns minutos. Para obter mais detalhes, consulte [Avaliação gratuita do Azure](http://azure.microsoft.com/pricing/free-trial/).

Para concluir a implementação local, terá das ferramentas seguintes instaladas no seu computador de desenvolvimento local:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Docker compose](https://docs.docker.com/compose/install/)
* [NODE.js](https://nodejs.org/) -este software é um pré-requisito para a CLI de PCS.
* PCS CLI

> [!NOTE]
> Estas ferramentas estão disponíveis em muitas plataformas, incluindo Windows, Linux e iOS.

### <a name="install-the-pcs-cli"></a>Instalar a CLI de PCS

Para instalar a CLI, execute o seguinte comando no seu ambiente de linha de comandos:

```cmd/sh
npm install iot-solutions -g
```

Para obter mais informações sobre a CLI, consulte [como utilizar a CLI](https://github.com/Azure/pcs-cli/blob/master/README.md).

## <a name="deploy-the-azure-services"></a>Implementar os serviços do Azure

Embora este artigo mostra como executar os micro-serviços localmente, dependem de três serviços do Azure em execução na nuvem. Pode implementar estes serviços do Azure manualmente através do portal do Azure ou utilizar a CLI de PCS. Este artigo mostra como utilizar o `pcs` ferramenta.

### <a name="sign-in-to-the-cli"></a>Iniciar sessão para a CLI

Antes de poder implementar a solução pré-configurada, tem de iniciar sessão sua subscrição do Azure utilizando a CLI do seguinte modo:

```cmd/sh
pcs login
```

Siga o ecrã instruções para concluir o processo de início de sessão.

### <a name="run-a-local-deployment"></a>Executar uma implementação de local

Utilize o seguinte comando para iniciar a implementação local:

```cmd/pcs
pcs -s local
```

O script pede-lhe as seguintes informações:

* Um nome de solução.
* A subscrição do Azure que deve utilizar.
* A localização do datacenter do Azure a utilizar.

O script cria um IoT Hub instância, uma instância de base de dados do Cosmos e uma conta de armazenamento do Azure num grupo de recursos na sua subscrição do Azure. O nome do grupo de recursos é o nome da solução que escolheu quando executou o `pcs` ferramenta.

O script demora vários minutos a executar. Quando terminar, verá uma mensagem `Copy the following environment variables to /scripts/local/.env file:`. Copiar as definições de variável de ambiente a mensagem a seguir, utilizá-los num passo posterior.

## <a name="download-the-source-code"></a>Transferir o código de origem

O repositório de código de origem monitorização remoto inclui os ficheiros de configuração de Docker que terá de transferir, configurar e executar as imagens de Docker que contêm os micro-serviços. Para clonar o repositório, navegue para uma pasta adequada no seu computador local e execute um dos seguintes comandos:

Para instalar as implementações de Java dos micro-serviços, execute:

```cmd/sh
git clone --recursive https://github.com/Azure/azure-iot-pcs-remote-monitoring-java
```

Para instalar as implementações de .net dos micro-serviços, execute:

```cmd\sh
git clone --recursive https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet
```

Estes comandos transferem o código de origem para todos os micro-serviços. Apesar de não precisa do código de origem para executar os micro-serviços no Docker, o código de origem é útil se pretender mais tarde modificar a solução pré-configurada e testar as suas alterações localmente.

## <a name="run-the-microservices-in-docker"></a>Execute os micro-serviços no Docker

Para executar os micro-serviços Docker, edite o **scripts\\local\\.env** ficheiros na sua cópia local do repositório. Substitua os conteúdos integrais do ficheiro com as definições de variável de ambiente apontou de quando executou o `pcs` comando anteriormente. Ativar a estas variáveis de ambiente micro-serviços no contentor do Docker para ligar aos serviços do Azure criados pelo `pcs` ferramenta.

Para executar a solução pré-configurada, navegue para o **scripts\local** pasta no seu ambiente de linha de comandos e execute o seguinte comando:

```cmd\sh
docker-compose up
```

Na primeira vez que executar este comando, Docker transfere as imagens de microsserviço hub Docker para criar os contentores localmente. No execuções subsequentes, Docker executa os contentores imediatamente.

Pode utilizar um shell separada para ver os registos do contentor. Primeiro localizar o ID de contentor utilizando o `docker ps -a` comando. Em seguida, utilize `docker logs {container-id} --tail 1000` para ver as últimas entradas de registo de 1000 para o contentor especificado.

Para aceder ao dashboard da solução de monitorização remota, navegue para [http://localhost:8080](http://localhost:8080) no seu browser.

## <a name="tidy-up"></a>Tidy cópias de segurança

Para evitar custos desnecessários, quando tiver concluído os testes, remova os serviços em nuvem da sua subscrição do Azure. A forma mais fácil para remover os serviços consiste em utilizar o portal do Azure para eliminar o grupo de recursos criado pelo `pcs` ferramenta.

Utilize o `docker-compose down --rmi all` comando para remover as imagens de Docker e libertar espaço no seu computador local. Também pode eliminar a cópia local do repositório de monitorização remota criado quando clonar o código de origem a partir do GitHub.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Configurar a solução pré-configurada
> * Implementar a solução pré-configurada
> * Iniciar sessão para a solução pré-configurada

Agora que implementou a solução de monitorização remota, o passo seguinte consiste em [explorar as funcionalidades do dashboard de solução](./iot-suite-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->