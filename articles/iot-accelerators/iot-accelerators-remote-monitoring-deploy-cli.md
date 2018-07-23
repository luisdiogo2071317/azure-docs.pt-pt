---
title: Implementar a solução de monitorização remota do Java - Azure | Documentos da Microsoft
description: Este tutorial mostra-lhe como aprovisionar o acelerador de solução de monitorização remota com a CLI.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: dd696330c9ee78ef84ac9fcf85946c837ad5b824
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39188025"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-using-the-cli"></a>Implementar o acelerador de solução de monitorização remota com a CLI

Este tutorial mostra-lhe como aprovisionar o acelerador de solução de monitorização remota. Implementar a solução com a CLI. Também pode implementar a solução usando a interface do Usuário baseada na web em azureiotsuite.com, para saber mais sobre esta opção, consulte [implementar o acelerador de solução de monitorização remota](quickstart-remote-monitoring-deploy.md).

## <a name="prerequisites"></a>Pré-requisitos

Para implementar o acelerador de solução de monitorização remota, terá de uma subscrição do Azure Active Directory.

Se não tiver uma conta, pode criar uma de avaliação gratuita em apenas alguns minutos. Para obter mais detalhes, consulte [Avaliação gratuita do Azure](http://azure.microsoft.com/pricing/free-trial/).

Para executar a CLI, precisa [node. js](https://nodejs.org/) instalado no seu computador local.

## <a name="install-the-cli"></a>Instalar a CLI

Para instalar a CLI, execute o seguinte comando no seu ambiente de linha de comandos:

```cmd/sh
npm install iot-solutions -g
```

## <a name="sign-in-to-the-cli"></a>Entrar para a CLI

Antes de poder implementar o solution accelerator, tem de iniciar sessão sua subscrição do Azure com a CLI da seguinte forma:

```cmd/sh
pcs login
```

Siga na tela instruções para concluir o processo de início de sessão.

## <a name="deployment-options"></a>Opções de implementação

Ao implementar o solution accelerator, existem várias opções que configuram o processo de implantação:

| Opção | Valores | Descrição |
| ------ | ------ | ----------- |
| SKU    | `basic`, `standard`, `local` | R _básica_ implementação destina-se para teste e demonstrações, implementa todos os microsserviços são uma única máquina virtual. R _padrão_ implementação destina-se para a produção, ele implementa os microsserviços em várias máquinas virtuais. R _local_ implementação configura um contentor de Docker para executar os microsserviços no seu computador local e utiliza os serviços do Azure, como o armazenamento e o Cosmos DB, na cloud. |
| Runtime | `dotnet`, `java` | Seleciona a implementação de idioma dos microsserviços. |

Para saber mais sobre como utilizar a implementação local, veja [a executar localmente a solução de monitorização remota](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Running-the-Remote-Monitoring-Solution-Locally#deploy-azure-services-and-set-environment-variables).

## <a name="basic-vs-standard-deployments"></a>Vs básicas. Implementações padrão

### <a name="basic"></a>Básica
Implementação básica destina-se se utilizar que apresenta a solução. Para reduzir o custo desta demonstração, todos os microsserviços são implementados numa única máquina virtual; isso não é considerado uma arquitetura de prontos para produção.

Nossa opção de implementação padrão deve ser utilizada quando estiver pronto para personalizar uma arquitetura de prontos para produção, criada para dimensionamento e extensibilidade.

Criar uma solução básica de resultará nos seguintes serviços do Azure a ser aprovisionados na sua subscrição do Azure sem custo: 

| Contagem | Recurso                       | Tipo         | Utilizado para |
|-------|--------------------------------|--------------|----------|
| 1     | [Máquina Virtual do Linux](https://azure.microsoft.com/services/virtual-machines/) | Standard D1 V2  | Alojamento de microsserviços |
| 1     | [Hub IoT do Azure](https://azure.microsoft.com/services/iot-hub/)                  | S1 – escalão Standard | Gestão de dispositivos e a comunicação |
| 1     | [BD do Cosmos para o Azure](https://azure.microsoft.com/services/cosmos-db/)              | Standard        | Armazenar dados de configuração e a telemetria do dispositivo, como regras, alarmes e mensagens |  
| 1     | [Conta de armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)  | Standard        | Armazenamento de VM e os pontos de verificação de transmissão em fluxo |
| 1     | [Aplicação Web](https://azure.microsoft.com/services/app-service/web/)        |                 | Aplicativo web de front-end de hospedagem |

### <a name="standard"></a>Standard
A implementação padrão é uma implementação de prontos para produção, um desenvolvedor pode personalizar e expandir para satisfazer as suas necessidades. Para fiabilidade e dimensionamento, os microsserviços de aplicação são criados como contentores do Docker e implementados com um orquestrador ([Kubernetes](https://kubernetes.io/) por predefinição). O orquestrador é responsável pela implementação, dimensionamento e gestão da aplicação.

Criação de uma solução padrão resultará nos seguintes serviços do Azure a ser aprovisionados na sua subscrição do Azure sem custo:

| Contagem | Recurso                                     | SKU / tamanho      | Utilizado para |
|-------|----------------------------------------------|-----------------|----------|
| 4     | [Máquinas Virtuais do Linux](https://azure.microsoft.com/services/virtual-machines/)   | Standard D2 V2  | mestre de 1 e 3 agentes para o alojamento de microsserviços com redundância |
| 1     | [Serviço de contentor do Azure](https://azure.microsoft.com/services/container-service/) |                 | [Kubernetes](https://kubernetes.io) orchestrator |
| 1     | [Hub IoT do azure] [https://azure.microsoft.com/services/iot-hub/]                     | S2 – escalão Standard | Gestão de dispositivos, comando e controlo |
| 1     | [BD do Cosmos para o Azure](https://azure.microsoft.com/services/cosmos-db/)                 | Standard        | Armazenar dados de configuração e a telemetria do dispositivo, como regras, alarmes e mensagens |
| 5     | [Contas de armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)    | Standard        | 4 para armazenamento de VM e 1 para os pontos de verificação de transmissão em fluxo |
| 1     | [Serviço de Aplicações](https://azure.microsoft.com/services/app-service/web/)             | S1 Standard     | Gateway de aplicação através de SSL |

> Informações sobre os preços para estes serviços podem ser encontrados [aqui](https://azure.microsoft.com/pricing). As quantidades de utilização e os detalhes de faturação para a sua subscrição pode ser encontrado na [Portal do Azure](https://portal.azure.com/).

## <a name="deploy-the-solution-accelerator"></a>Implementar o acelerador de solução

### <a name="example-deploy-net-version"></a>Exemplo: implementar a versão do .NET

O exemplo seguinte mostra como implementar a versão do .NET basic, do solution accelerator monitorização remota:

```cmd/sh
pcs -t remotemonitoring -s basic -r dotnet
```

### <a name="example-deploy-java-version"></a>Exemplo: implementar a versão de Java

O exemplo seguinte mostra como implementar a versão de Java padrão, do solution accelerator monitorização remota:

```cmd/sh
pcs -t remotemonitoring -s standard -r java
```

### <a name="pcs-command-options"></a>as opções do comando de PCs

Quando executa o `pcs` comando para implementar uma solução, é-lhe perguntado para:

- Um nome para a sua solução. Este nome tem de ser exclusivo.
- A subscrição do Azure que deve utilizar.
- Uma localização.
- Credenciais para as máquinas virtuais que alojam os microsserviços. Pode utilizar estas credenciais para aceder as máquinas virtuais de resolução de problemas.

Quando o `pcs` comando estiver concluído, ele apresenta o URL da sua nova implementação de acelerador de solução. O `pcs` comando também cria um ficheiro `{deployment-name}-output.json` com informações adicionais, como o nome do IoT Hub que foi provisionado para.

Para obter mais informações sobre os parâmetros da linha de comandos, execute:

```cmd/sh
pcs -h
```

Para obter mais informações sobre a CLI, veja [como utilizar a CLI](https://github.com/Azure/pcs-cli/blob/master/README.md).

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Configurar o acelerador de soluções
> * Implementar o acelerador de solução
> * Inicie sessão no solution accelerator

Agora que tenha implantado a solução de monitorização remota, a próxima etapa é [explore os recursos do dashboard da solução](./quickstart-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->
