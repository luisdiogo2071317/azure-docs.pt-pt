---
title: incluir ficheiro
description: incluir ficheiro
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 10/05/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 9246dea7fa12e5ac9378203e96352e917679525b
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/11/2018
ms.locfileid: "49312413"
---
### <a name="general-requirements"></a>Requisitos gerais

* A VNet tem de estar na mesma subscrição e região da conta do Batch utilizada para criar o conjunto.

* O conjunto que utiliza a VNet pode ter um máximo de 4096 nós.

* A sub-rede especificada para o conjunto deve ter endereços IP não atribuídos suficientes para acomodar o número de VMs direcionadas para o conjunto; ou seja, a soma de propriedades `targetDedicatedNodes` e `targetLowPriorityNodes` do conjunto. Se a sub-rede não tiver endereços IP não atribuídos suficientes, o conjunto atribui parcialmente os nós de computação e ocorre um erro de redimensionamento. 

* O ponto final do Armazenamento do Azure tem de ser resolvido por qualquer servidor DNS personalizado que sirva a sua VNet. Mais concretamente, devem ser resolvíveis os URLs no formato `<account>.table.core.windows.net`, `<account>.queue.core.windows.net` e `<account>.blob.core.windows.net`. 

Os requisitos adicionais de VNet diferem, consoante o conjunto do Batch está na configuração da Máquina Virtual ou na configuração dos Serviços Cloud. Para novas implementações de conjuntos numa VNet, é recomendada a configuração de Máquina Virtual.

### <a name="pools-in-the-virtual-machine-configuration"></a>Conjuntos na configuração de Máquina Virtual

**VNets suportadas** - apenas VNets baseadas no Azure Resource Manager

**ID de sub-rede** - quando especificar a sub-rede com as APIs do Batch, utilize o *identificador de recurso* da sub-rede. O identificador da sub-rede tem o formato:

  ```
  /subscriptions/{subscription}/resourceGroups/{group}/providers/Microsoft.Network/virtualNetworks/{network}/subnets/{subnet}
  ```

**Permissões** - verifique se as suas políticas de segurança ou bloqueios na subscrição ou no grupo de recursos da VNet restringem as permissões de um utilizador para gerir a VNet.

**Recursos de rede adicionais** - o Batch aloca automaticamente recursos de rede adicionais no grupo de recursos que contém a VNet. Para cada 50 nós dedicados (ou cada 20 nós de baixa prioridade), o Batch aloca: um grupo de segurança de rede (NSG), um endereço IP público e um balanceador de carga. Estes recursos estão limitados pelas [quotas de recursos](../articles/azure-subscription-service-limits.md) da subscrição. Para conjuntos grandes pode ter de pedir um aumento de quota para um ou mais destes recursos.

#### <a name="network-security-groups"></a>Grupos de segurança de rede

A sub-rede tem de permitir comunicação de entrada a partir do serviço Batch para conseguir agendar tarefas nos nós de computação e comunicação de saída para comunicar com o Armazenamento do Azure ou outros recursos. Para conjuntos na configuração de Máquina Virtual, o Batch adiciona NSGs ao nível das interfaces de rede (NICs) ligadas às VMs. Estes NSGs configuram automaticamente regras de entrada e saída para permitir o tráfego seguinte:

* Tráfego TCP de entrada nas portas 29876 e 29877 a partir de endereços IP de função do serviço Batch. 
* Tráfego TCP de entrada nas portas 22 (nós do Linux) ou 3389 (nós do Windows) para permitir acesso remoto.
* Tráfego de saída em qualquer porta para a rede virtual.
* Tráfego de saída em qualquer porta para a Internet.

> [!IMPORTANT]
> Tenha cuidado se modificar ou adicionar regras de entrada ou saída em NSGs configurados pelo Batch. Se a comunicação com os nós de computação na sub-rede especificada for recusada por um NSG, o serviço Batch define o estado dos nós de computação como **inutilizável**.

Não é necessário especificar NSGs ao nível da sub-rede porque o Batch configura o seus próprios NSGs. No entanto, se a sub-rede especificada tiver Grupos de Segurança de Rede (NSGs) associados e/ou uma firewall, configure as regras de segurança de entrada e saída conforme mostrado nas tabelas seguintes. Configure o tráfego de entrada na porta 3389 (Windows) ou 22 (Linux), apenas se tiver de permitir acesso remoto às VMs do conjunto. Não é necessário para as VMs do conjunto serem utilizáveis.

**Regras de segurança de entrada**

| Endereços IP de origem | Portas de origem | Destino | Portas de destino | Protocolo | Ação |
| --- | --- | --- | --- | --- | --- |
Qualquer <br /><br />Apesar de isto necessitar efetivamente de "permitir todos", o serviço Batch aplica um NSG ao nível da interface de rede em cada VM criada na configuração de Máquina Virtual que filtra todos os endereços IP de serviço não pertencentes ao Batch. | * | Qualquer | 29876-29877 | TCP | Permitir |
| Máquinas de utilizador, utilizadas para fins de depuração para aceder remotamente às VMs do conjunto. | * | Qualquer |  3389 (Windows), 22 (Linux) | TCP | Permitir |

**Regras de segurança de saída**

| Origem | Portas de origem | Destino | Etiqueta do serviço de destino | Protocolo | Ação |
| --- | --- | --- | --- | --- | --- |
| Qualquer | 443 | [Etiqueta de serviço](../articles/virtual-network/security-overview.md#service-tags) | Armazenamento (na mesma região da conta do Batch e da VNet)  | Qualquer | Permitir |

### <a name="pools-in-the-cloud-services-configuration"></a>Conjuntos na configuração dos Serviços Cloud

**VNets suportadas** - Apenas VNets clássicas

**ID de sub-rede** - quando especificar a sub-rede com as APIs do Batch, utilize o *identificador de recurso* da sub-rede. O identificador da sub-rede tem o formato:

  ```
  /subscriptions/{subscription}/resourceGroups/{group}/providers/Microsoft.ClassicVirtualNetwork /virtualNetworks/{network}/subnets/{subnet}
  ```

**Permissões**  - O principal de serviço `MicrosoftAzureBatch` tem de ter a função de Controlo de Acesso Baseado em Funções (RBAC) `Classic Virtual Machine Contributor` na VNet especificada.

#### <a name="network-security-groups"></a>Grupos de segurança de rede

A sub-rede tem de permitir comunicação de entrada a partir do serviço Batch para conseguir agendar tarefas nos nós de computação e comunicação de saída para comunicar com o Armazenamento do Azure ou outros recursos.

Não é necessário especificar um NSG, porque o Batch configura a comunicação de entrada apenas a partir dos endereços IP do Batch para os nós do conjunto. No entanto, se a sub-rede especificada tiver NSGs associados e/ou uma firewall, configure as regras de segurança de entrada e saída conforme mostrado nas tabelas seguintes. Se a comunicação com os nós de computação na sub-rede especificada for recusada por um NSG, o serviço Batch define o estado dos nós de computação como **inutilizável**.

 Configure o tráfego de entrada na porta 3389 (Windows) ou 22 (Linux), apenas se tiver de permitir acesso remoto aos nós do conjunto. Não é necessário para os nós do conjunto serem utilizáveis.

**Regras de segurança de entrada**

| Endereços IP de origem | Portas de origem | Destino | Portas de destino | Protocolo | Ação |
| --- | --- | --- | --- | --- | --- |
Qualquer <br /><br />Apesar de isto necessitar efetivamente de "permitir todos", o serviço Batch aplica uma regra ACL ao nível de cada nó que filtra todos os endereços IP de serviço não pertencentes ao Batch. | * | Qualquer | 10100, 20100, 30100 | TCP | Permitir |
| Máquinas de utilizador, utilizadas para fins de depuração para aceder remotamente às VMs do conjunto. | * | Qualquer |  3389 (Windows), 22 (Linux) | TCP | Permitir |

**Regras de segurança de saída**

| Origem | Portas de origem | Destino | Portas de destino | Protocolo | Ação |
| --- | --- | --- | --- | --- | --- |
| Qualquer | * | Qualquer | 443  | Qualquer | Permitir |