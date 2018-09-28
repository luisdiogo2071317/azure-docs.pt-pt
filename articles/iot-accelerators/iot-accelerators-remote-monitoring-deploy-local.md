---
title: Implementar a solução de monitorização remota localmente - Azure | Documentos da Microsoft
description: Este guia de procedimentos mostra-lhe como implementar o acelerador de solução de monitorização remota no seu computador local para teste e desenvolvimento.
author: asdonald
manager: timlt
ms.author: asdonald
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 09/26/2018
ms.topic: conceptual
ms.openlocfilehash: 7007b1406dbcfab3af4700418ac2ce07b9e521c0
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47407438"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally"></a>Implementar localmente a acelerador de soluções de monitorização remota

Este artigo mostra-lhe como implementar o acelerador de solução de monitorização remota no seu computador local para teste e desenvolvimento. A abordagem descrita neste artigo implementa os microsserviços para um contentor do Docker local e utiliza o IoT Hub, o Cosmos DB e o Azure Time Series Insights serviços na cloud. Para saber como executar o acelerador de solução de monitorização remota num IDE no seu computador local, veja [a partir de Microsserviços no ambiente local](https://github.com/Azure/remote-monitoring-services-java/blob/master/docs/LOCAL_DEPLOYMENT.md) no GitHub.

## <a name="prerequisites"></a>Pré-requisitos

Para implementar os serviços do Azure utilizados pelo acelerador de solução de monitorização remota, precisa de uma subscrição do Azure Active Directory.

Se não tiver uma conta, pode criar uma de avaliação gratuita em apenas alguns minutos. Para obter mais detalhes, consulte [Avaliação gratuita do Azure](http://azure.microsoft.com/pricing/free-trial/).

Para concluir a implementação de local, terá das seguintes ferramentas instaladas no seu computador de desenvolvimento local:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [O docker compose](https://docs.docker.com/compose/install/)
* [NODE. js v8](https://nodejs.org/) -este software é um pré-requisito para a CLI de PCS que os scripts usam para criar recursos do Azure. Não utilize v10 de node. js.

> [!NOTE]
> Estas ferramentas estão disponíveis em muitas plataformas, incluindo Windows, Linux e iOS.

### <a name="download-the-source-code"></a>Baixe o código-fonte

O repositório GitHub monitorização remota de código de origem inclui os ficheiros de configuração do Docker que tem de transferir, configurar e executar as imagens do Docker que contêm os microsserviços. Clonar e criar uma versão local do repositório, utilize o seu ambiente de linha de comandos para navegar para uma pasta adequada no seu computador local e, em seguida, execute um dos seguintes comandos:

Para transferir a versão mais recente das implementações de microsserviços Java, execute:

```cmd/sh
git clone https://github.com/Azure/remote-monitoring-services-java.git
```

Para transferir a versão mais recente das implementações de microsserviços .NET, execute:

```cmd\sh
git clone https://github.com/Azure/remote-monitoring-services-dotnet.git
```

> [!NOTE]
> Estes comandos Baixe o código-fonte para todos os microsserviços, além dos scripts que utiliza para executar os microsserviços localmente. Apesar de não precisar do código-fonte para executar os microsserviços no Docker, o código-fonte é útil se pretender mais tarde modificar o acelerador de solução e testar as suas alterações localmente.

## <a name="deploy-the-azure-services"></a>Implementar os serviços do Azure

Embora este artigo mostra como executar os microsserviços localmente, dependem de serviços do Azure em execução na cloud. Pode implementar estes serviços do Azure [manualmente através do portal do Azure](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Manual-steps-to-create-azure-resources-for-local-setup), ou utilize o script fornecido. Os exemplos de script seguintes partem do princípio de que estiver a utilizar o repositório .NET num computador Windows. Se estiver trabalhando em outro ambiente, ajuste adequadamente os caminhos, extensões de ficheiro e os separadores de caminho. Para utilizar os scripts fornecidos para:

### <a name="create-new-azure-resources"></a>Criar novos recursos do Azure

Se ainda não criou os recursos do Azure necessários, siga estes passos:

1. No seu ambiente de linha de comandos, navegue para o **remote-monitoring-services-dotnet\scripts\local\launch** pasta na sua cópia clonada do repositório.

2. Executar o **start.cmd** do script e siga as instruções. O script pede-lhe para iniciar sessão sua conta do Azure e reinicie o script. O script, em seguida, pede-lhe as seguintes informações:
    * Um nome de solução.
    * A subscrição do Azure que deve utilizar.
    * A localização do datacenter do Azure para utilizar.

    O script cria o grupo de recursos no Azure com o nome da sua solução. Este grupo de recursos contém os recursos do Azure que utiliza o solution accelerator.

3. Quando o script tiver concluído, ele exibe uma lista de variáveis de ambiente. Siga as instruções na saída do comando para guardar estas variáveis para o **remote-monitoring-services-dotnet\\scripts\\local\\. env** ficheiro.

### <a name="use-existing-azure-resources"></a>Utilizar recursos do Azure existentes

Se já tiver criado os recursos do Azure necessários editar as definições de variável de ambiente no **remote-monitoring-services-dotnet\\scripts\\local\\. env** de ficheiros com o valores necessários. O **. env** arquivo contém informações detalhadas sobre onde encontrar os valores necessários.

## <a name="run-the-microservices-in-docker"></a>Execute os microsserviços no Docker

Os microsserviços em execução nos contentores do Docker locais tem de aceder aos serviços em execução no Azure. Pode testar a conectividade de internet do seu ambiente do Docker com o seguinte comando que inicia um contentor de pequeno e tenta fazer ping um endereço na internet:

```cmd/sh
docker run --rm -ti library/alpine ping google.com
```

Para executar o solution accelerator, navegue para o **remote-monitoring-services-dotnet\\scripts\\local** pasta no seu ambiente de linha de comandos e execute o seguinte comando:

```cmd\sh
docker-compose up
```

Na primeira vez que executar este comando, o Docker transfere as imagens de microsserviços do hub do Docker para criar os contentores localmente. As execuções posteriores, Docker é executado os contentores imediatamente.

Pode utilizar um shell separado para ver os registos do contentor. Primeiro, encontrar o ID de contentor com o `docker ps -a` comando. Em seguida, utilizar `docker logs {container-id} --tail 1000` para ver as últimas entradas de registo de 1000 para o contentor especificado.

Para aceder ao dashboard de solução de monitorização remota, navegue até [ http://localhost:8080 ](http://localhost:8080) no seu browser.

## <a name="clean-up"></a>Limpeza

Para evitar desnecessários custos, quando tiver terminado de seu teste remover os serviços em nuvem da sua subscrição do Azure. A maneira mais fácil para remover os serviços é navegar para o [portal do Azure](https://ms.portal.azure.com) e elimine o grupo de recursos que foi criado quando executou o **start.cmd** script.

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
