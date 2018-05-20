---
title: Implementar o Java solução - Azure de monitorização remota | Microsoft Docs
description: Este tutorial mostra como aprovisionar o acelerador de solução de monitorização remota, utilizando a CLI.
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 01/29/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 3178d51cd2c04f3be8d4a6284a4f1635845def8c
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-using-the-cli"></a>Implementar o acelerador de solução de monitorização remota, utilizando a CLI

Este tutorial mostra como aprovisionar o acelerador de solução de monitorização remota. Implementar a solução utilizando a CLI. Também pode implementar a solução utilizando a IU baseada na web em azureiotsuite.com, para saber mais sobre esta opção, consulte [implementar o acelerador de solução de monitorização remota](iot-accelerators-remote-monitoring-deploy.md).

## <a name="prerequisites"></a>Pré-requisitos

Para implementar o acelerador de solução de monitorização remota, terá de uma subscrição do Azure Active Directory.

Se não tiver uma conta, pode criar uma de avaliação gratuita em apenas alguns minutos. Para obter mais detalhes, consulte [Avaliação gratuita do Azure](http://azure.microsoft.com/pricing/free-trial/).

Para executar o CLI, terá de [Node.js](https://nodejs.org/) instalado no seu computador local.

## <a name="install-the-cli"></a>Instalar a CLI

Para instalar a CLI, execute o seguinte comando no seu ambiente de linha de comandos:

```cmd/sh
npm install iot-solutions -g
```

## <a name="sign-in-to-the-cli"></a>Iniciar sessão para a CLI

Antes de poder implementar o acelerador de solução, tem de iniciar sessão sua subscrição do Azure utilizando a CLI do seguinte modo:

```cmd/sh
pcs login
```

Siga o ecrã instruções para concluir o processo de início de sessão.

## <a name="deployment-options"></a>Opções de Implementação

Quando implementa o acelerador de solução, existem várias opções que configuram o processo de implementação:

| Opção | Valores | Descrição |
| ------ | ------ | ----------- |
| SKU    | `basic`, `standard`, `local` | A _básico_ implementação destina-se de teste e demonstrações, implementa todos os micro-serviços uma única máquina virtual. A _padrão_ implementação destina-se para produção, implementa os micro-serviços várias máquinas virtuais. A _local_ implementação configura um contentor de Docker para executar os micro-serviços no seu computador local e utiliza os serviços do Azure, tais como a BD do Cosmos e de armazenamento na nuvem. |
| Runtime | `dotnet`, `java` | Seleciona a implementação de idioma dos micro-serviços. |

Para saber mais sobre como utilizar a implementação local, consulte [executar localmente a solução de monitorização remota](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Running-the-Remote-Monitoring-Solution-Locally#deploy-azure-services-and-set-environment-variables).

## <a name="basic-vs-standard-deployments"></a>Vs básicas. Implementações Standard

### <a name="basic"></a>Básica
Implementação básica é adaptada na direção de mostrar a solução. Para reduzir o custo desta demonstração, todos os micro-serviços são implementados numa única máquina virtual; Isto não é considerado uma arquitetura de prontos para produção.

A nossa opção de implementação padrão deve ser utilizada quando estiver pronto para personalizar uma arquitetura de prontos para produção, criada para dimensionamento e extensibilidade.

Criar uma solução básica de resultará nos seguintes serviços do Azure que está a ser aprovisionados na sua subscrição do Azure, custo: 

| Contagem | Recurso                       | Tipo         | Utilizado para |
|-------|--------------------------------|--------------|----------|
| 1     | [Máquina Virtual com Linux](https://azure.microsoft.com/services/virtual-machines/) | D1 padrão V2  | Alojamento micro-serviços |
| 1     | [Hub IoT do Azure](https://azure.microsoft.com/services/iot-hub/)                  | S1 – o escalão básico | Gestão de dispositivos e comunicação |
| 1     | [BD do Cosmos para o Azure](https://azure.microsoft.com/services/cosmos-db/)              | Standard        | Armazenar dados de configuração e a telemetria do dispositivo, como regras, alarmes e mensagens |  
| 1     | [Conta de armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)  | Standard        | Armazenamento de VM e os pontos de verificação de transmissão em fluxo |
| 1     | [Aplicação Web](https://azure.microsoft.com/services/app-service/web/)        |                 | Aplicação web front-end de alojamento |

### <a name="standard"></a>Standard
A implementação padrão é uma implementação de prontos para produção, um programador pode personalizar e expandir para satisfazer as suas necessidades. Para a fiabilidade e o dimensionamento, micro-serviços de aplicação são criados como contentores de Docker e implementadas através de um orchestrator ([Kubernetes](https://kubernetes.io/) por predefinição). O orchestrator é responsável pela implementação, dimensionamento e gestão da aplicação.

Criar uma solução padrão resultará nos seguintes serviços do Azure que está a ser aprovisionados na sua subscrição do Azure, custo:

| Contagem | Recurso                                     | SKU / tamanho      | Utilizado para |
|-------|----------------------------------------------|-----------------|----------|
| 4     | [Máquinas Virtuais do Linux](https://azure.microsoft.com/services/virtual-machines/)   | D2 padrão V2  | 1 mestre e 3 agentes para o alojamento micro-serviços com redundância |
| 1     | [Serviço de contentor do Azure](https://azure.microsoft.com/services/container-service/) |                 | [Kubernetes](https://kubernetes.io) orchestrator |
| 1     | [IoT Hub do azure] [https://azure.microsoft.com/services/iot-hub/]                     | S1 – o escalão básico | Gestão de dispositivos, o comando e controlo |
| 1     | [BD do Cosmos para o Azure](https://azure.microsoft.com/services/cosmos-db/)                 | Standard        | Armazenar dados de configuração e a telemetria do dispositivo, como regras, alarmes e mensagens |
| 5     | [Contas do Storage do Azure](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)    | Standard        | 4 para armazenamento VM e 1 para os pontos de verificação de transmissão em fluxo |
| 1     | [Serviço de Aplicações](https://azure.microsoft.com/services/app-service/web/)             | S1 Standard     | Gateway de aplicação através de SSL |

> Informações sobre os preços para estes serviços podem ser encontradas [aqui](https://azure.microsoft.com/pricing). Os detalhes de faturação da sua subscrição e as quantidades de utilização podem ser encontrados no [Portal do Azure](https://portal.azure.com/).

## <a name="deploy-the-solution-accelerator"></a>Implementar o acelerador de solução

### <a name="example-deploy-net-version"></a>Exemplo: implementar a versão do .NET

O exemplo seguinte mostra como implementar o basic, versão de .NET do acelerador de solução de monitorização remota:

```cmd/sh
pcs -t remotemonitoring -s basic -r dotnet
```

### <a name="example-deploy-java-version"></a>Exemplo: implementar a versão de Java

O exemplo seguinte mostra como implementar a norma, a versão de Java do acelerador de solução de monitorização remota:

```cmd/sh
pcs -t remotemonitoring -s standard -r java
```

### <a name="pcs-command-options"></a>Opções de comando de PCs

Quando executa o `pcs` comando para implementar uma solução, é-lhe pedido para:

- Um nome para a sua solução. Este nome tem de ser exclusivo.
- A subscrição do Azure que deve utilizar.
- Uma localização.
- As credenciais para as máquinas virtuais que alojam os micro-serviços. Pode utilizar estas credenciais para aceder as máquinas virtuais para resolução de problemas.

Quando o `pcs` comando estiver concluído, apresenta o URL da sua nova implementação de solução de acelerador. O `pcs` comando também cria um ficheiro `{deployment-name}-output.json` com informações adicionais, como o nome do IoT Hub que tenha sido aprovisionado para si.

Para obter mais informações sobre os parâmetros da linha de comandos, execute:

```cmd/sh
pcs -h
```

Para obter mais informações sobre a CLI, consulte [como utilizar a CLI](https://github.com/Azure/pcs-cli/blob/master/README.md).

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Configurar o acelerador de solução
> * Implementar o acelerador de solução
> * Iniciar sessão para o acelerador de solução

Agora que implementou a solução de monitorização remota, o passo seguinte consiste em [explorar as funcionalidades do dashboard de solução](./iot-accelerators-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->