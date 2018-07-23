---
title: Implementar a solução de monitorização remota localmente - Azure | Documentos da Microsoft
description: Este tutorial mostra-lhe como implementar o acelerador de solução de monitorização remota no seu computador local para teste e desenvolvimento.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/07/2018
ms.topic: conceptual
ms.openlocfilehash: 21bc8c27a44c940279b0c5bdcdbe04e579dc4bfa
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39188793"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally"></a>Implementar localmente a acelerador de soluções de monitorização remota

Este artigo mostra-lhe como implementar o acelerador de solução de monitorização remota no seu computador local para teste e desenvolvimento. Essa abordagem implementa os microsserviços para um contentor do Docker local e utiliza o IoT Hub e dos serviços de armazenamento do Azure Cosmos DB na cloud. Utilize os Aceleradores de solução (PCS) da CLI para implementar os serviços cloud do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Para implementar os serviços do Azure utilizados pelo acelerador de solução de monitorização remota, precisa de uma subscrição do Azure Active Directory.

Se não tiver uma conta, pode criar uma de avaliação gratuita em apenas alguns minutos. Para obter mais detalhes, consulte [Avaliação gratuita do Azure](http://azure.microsoft.com/pricing/free-trial/).

Para concluir a implementação de local, terá das seguintes ferramentas instaladas no seu computador de desenvolvimento local:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [O docker compose](https://docs.docker.com/compose/install/)
* [NODE. js](https://nodejs.org/) -este software é um pré-requisito para a CLI de PCS.
* CLI DE PCS
* Repositório local do código-fonte

> [!NOTE]
> Estas ferramentas estão disponíveis em muitas plataformas, incluindo Windows, Linux e iOS.

### <a name="install-the-pcs-cli"></a>Instalar a CLI de PCS

Para instalar a CLI de PCS através do npm, execute o seguinte comando no seu ambiente de linha de comandos:

```cmd/sh
npm install iot-solutions -g
```

Para obter mais informações sobre a CLI, veja [como utilizar a CLI](https://github.com/Azure/pcs-cli/blob/master/README.md).

### <a name="download-the-source-code"></a>Baixe o código-fonte

 O repositório de código do código-fonte de monitorização remota inclui os ficheiros de configuração do Docker que tem de transferir, configurar e executar as imagens do Docker que contêm os microsserviços. Para clonar e criar uma versão local do repositório, navegue para uma pasta adequada no seu computador local por meio de seus favorita de linha de comandos ou terminal e execute um dos seguintes comandos:

Para instalar as implementações de Java dos microsserviços, execute:

```cmd/sh
git clone --recursive https://github.com/Azure/azure-iot-pcs-remote-monitoring-java
```

Para instalar as implementações do .net dos microsserviços, execute:

```cmd\sh
git clone --recursive https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet
```

Repositório de solução pré-configurada de Monioring remoto & submódulos [ [Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java) | [.Net](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet) ]

> [!NOTE]
> Estes comandos Baixe o código-fonte para todos os microsserviços são. Apesar de não precisar do código-fonte para executar os microsserviços no Docker, o código-fonte é útil se pretender mais tarde modificar a solução pré-configurada e testar as suas alterações localmente.

## <a name="deploy-the-azure-services"></a>Implementar os serviços do Azure

Embora este artigo mostra como executar os microsserviços localmente, dependem de três serviços do Azure em execução na cloud. Pode implementar estes serviços do Azure [manualmente através do portal do Azure](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Manual-steps-to-create-azure-resources-for-local-setup), ou utilizar a CLI de PCS. Este artigo mostra-lhe como utilizar o `pcs` ferramenta.

### <a name="sign-in-to-the-cli"></a>Entrar para a CLI

Antes de poder implementar o solution accelerator, tem de iniciar sessão sua subscrição do Azure com a CLI da seguinte forma:

```cmd/sh
pcs login
```

Siga na tela instruções para concluir o processo de início de sessão. Certifique-se de que não clica em qualquer lugar dentro que da CLI ou o início de sessão pode falhar. Verá uma mensagem de início de sessão com êxito na CLI se tiver concluído o início de sessão. 

### <a name="run-a-local-deployment"></a>Executar uma implantação local

Utilize o seguinte comando para iniciar a implementação local. Isso irá criar os recursos do azure necessários e imprimir as variáveis de ambiente para a consola. 

```cmd/pcs
pcs -s local
```

O script pede-lhe as seguintes informações:

* Um nome de solução.
* A subscrição do Azure que deve utilizar.
* A localização do datacenter do Azure para utilizar.

> [!NOTE]
> O script cria um IoT Hub instância, uma instância de Cosmos DB e uma conta de armazenamento do Azure no grupo de recursos na sua subscrição do Azure. O nome do grupo de recursos é o nome da solução que escolheu quando executou o `pcs` ferramenta acima. 

> [!IMPORTANT]
> O script demora vários minutos para ser executado. Quando tiver concluído, verá uma mensagem `Copy the following environment variables to /scripts/local/.env file:`. Cópia para baixo o ambiente de seguir a mensagem, as definições de variável utilizará-los num passo posterior.

## <a name="run-the-microservices-in-docker"></a>Execute os microsserviços no Docker

Para executar os microsserviços no Docker, editar a **scripts\\locais\\. env** ficheiro na sua cópia local do repositório clonado no passo anterior acima. Substitua todo o conteúdo do ficheiro com as definições de variável de ambiente que efetuou uma nota de quando executou o `pcs` comando no último passo. Os microsserviços no contentor do Docker para ligar aos serviços do Azure criados ao ativar a estas variáveis de ambiente a `pcs` ferramenta.

Para executar o solution accelerator, navegue para o **scripts\local** pasta no seu ambiente de linha de comandos e execute o seguinte comando:

```cmd\sh
docker-compose up
```

Na primeira vez que executar este comando, o Docker transfere as imagens de microsserviços do hub do Docker para criar os contentores localmente. As execuções posteriores, Docker é executado os contentores imediatamente.

Pode utilizar um shell separado para ver os registos do contentor. Primeiro, encontrar o ID de contentor com o `docker ps -a` comando. Em seguida, utilizar `docker logs {container-id} --tail 1000` para ver as últimas entradas de registo de 1000 para o contentor especificado.

Para aceder ao dashboard de solução de monitorização remota, navegue até [ http://localhost:8080 ](http://localhost:8080) no seu browser.

## <a name="clean-up"></a>Limpeza

Para evitar cobranças desnecessárias, quando tiver concluído os testes, remova os serviços em nuvem da sua subscrição do Azure. A maneira mais fácil para remover os serviços é navegar para o [portal do Azure](https://ms.portal.azure.com) e elimine o grupo de recursos que criou através do `pcs` ferramenta.

Utilize o `docker-compose down --rmi all` comando para remover as imagens do Docker e liberte espaço no seu computador local. Também pode eliminar a cópia local do repositório de monitorização remota criado quando clonou o código-fonte do GitHub.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Configurar um ambiente de desenvolvimento local
> * Configurar o acelerador de soluções
> * Implementar o acelerador de solução
> * Inicie sessão no solution accelerator

Agora que tenha implantado a solução de monitorização remota, a próxima etapa é [explore os recursos do dashboard da solução](quickstart-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->