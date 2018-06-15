---
title: Recuperação após desastre e distribuição georreplicação nas funções durável - Azure
description: Saiba mais sobre a recuperação após desastre e distribuição georreplicação nas funções durável.
services: functions
author: MS-Santi
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/25/2018
ms.author: azfuncdf
ms.openlocfilehash: 8eb42a60045304416ec6aa1099a84b1e264c692d
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
ms.locfileid: "33207105"
---
# <a name="disaster-recovery-and-geo-distribution"></a>Recuperação após desastre e a distribuição de georreplicação

## <a name="overview"></a>Descrição geral
Nas funções de Azure durável, todos os Estados é continuado no armazenamento do Azure. A [hub tarefas](durable-functions-task-hubs.md) é um contentor lógico para os recursos de armazenamento do Azure que são utilizados para orchestrations. As funções do Orchestrator e atividade só podem interagir com entre si quando pertencem ao mesmo hub de tarefas.
Os cenários descritos propor opções de implementação para aumentar a disponibilidade e minimizar o período de indisponibilidade durante a atividades de recuperação após desastre.
É importante notar que estes cenários se baseiam em configurações de ativo-passivo, uma vez que estes guiado através da utilização do Storage do Azure. Este padrão consiste na implementação de uma aplicação de função (passivo) de cópia de segurança noutra região. Gestor de tráfego irá monitorizar a aplicação de função (ativo) principal para disponibilidade. -Serão ativadas pós-falha para a aplicação de função de cópia de segurança se o principal falhar. Para obter mais informações, consulte [Gestor de tráfego](https://azure.microsoft.com/services/traffic-manager/)do [método de encaminhamento de tráfego de prioridade.](../traffic-manager/traffic-manager-routing-methods.md#a-name--priorityapriority-traffic-routing-method)


>[!NOTE]
>- A configuração de ativo-passivo proposta garante que um cliente é sempre possível acionar a nova orchestrations através de HTTP. No entanto, como consequência ter duas aplicações de função partilha o mesmo armazenamento, processamento em segundo plano será distribuído entre ambos os parâmetros, competir pela mensagens de filas das mesmas. Esta configuração incorreu custos da saída foi adicionada para a aplicação de função secundária.
>- O hub de conta e a tarefa de armazenamento subjacente são criados na região primária e são partilhados por ambas as aplicações de função.
>- Todas as aplicações de função redundantly implementadas, têm de partilhar as mesmas chaves de acesso de função no caso de a ser ativado através de HTTP. O tempo de execução de funções expõe um [API de gestão](https://github.com/Azure/azure-functions-host/wiki/Key-management-API) que permite que os consumidores adicionar, eliminar e atualizar as teclas de função através de programação.

## <a name="scenario-1---load-balanced-compute-with-shared-storage"></a>Cenário 1 - com balanceamento de carga computação com armazenamento partilhado
Se falhar a infraestrutura de computação do Azure, a aplicação de função poderão ficar indisponível. Para minimizar a possibilidade desse período de indisponibilidade, este cenário utiliza dois de função das aplicações implementadas em regiões diferentes. Gestor de tráfego está configurado para detetar problemas na aplicação de função principal e redirecionar o tráfego automaticamente para a aplicação de função na região secundária. Esta aplicação de função partilha a mesma conta de armazenamento do Azure e o Hub de tarefas. Por conseguinte, o estado das aplicações de função não é perdido e trabalho possa retomar normalmente. Depois do Estado de funcionamento é restaurado para a região primária, o Gestor de tráfego do Azure começará encaminhamento de pedidos para essa aplicação de função automaticamente.


![Diagrama que mostra cenário 1.](media/durable-functions-disaster-recovery-geo-distribution/durable-functions-geo-scenario01.png)

Existem várias vantagens em utilizar este cenário de implementação:
- Se falhar a infraestrutura de computação, o trabalho possa retomar na ativação através de região sem perda de estado.
- Gestor de tráfego encarrega-se de que a ativação automática pós-falha para a aplicação de função em bom estado automaticamente.
- Gestor de tráfego restabelece automaticamente o tráfego para a aplicação de função principal após a interrupção foi resolvida.

No entanto, considere a utilização deste cenário:
- Se a aplicação de função é implementada através de um plano de serviço aplicacional dedicado, replicar a infraestrutura de computação a ativação através de centros de dados aumenta os custos.
- Este cenário abrange falhas na infraestrutura de computação, mas a conta de armazenamento continua a ser o ponto único de falha para a função de aplicação. Se existir uma falha de armazenamento, a aplicação sofrerá de um tempo de inatividade.
- Se a aplicação de função é a ativação pós-falha, haverá uma maior latência, uma vez que acedem a conta do storage em regiões.
- Acesso ao serviço armazenamento de noutra região onde está localizado implica um custo mais elevado devido ao tráfego de rede de saída.
- Neste cenário depende do Traffic Manager. Considerar [como funciona o Gestor de tráfego](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), poderá ser algum tempo até que uma aplicação de cliente que consome uma função durável tem de consultar novamente o endereço de aplicação de função do Gestor de tráfego. 


## <a name="scenario-2---load-balanced-compute-with-regional-storage"></a>Cenário 2 - com balanceamento de carga computação com armazenamento regional
O cenário anterior abrange apenas o caso de falha na infraestrutura de computação. Se o serviço de armazenamento falhar, irá resultar numa falha de aplicação de função.
Para garantir o funcionamento contínuo das funções duráveis, este cenário utiliza uma conta de armazenamento local em cada região em que as aplicações de função são implementadas.

![Diagrama que mostra cenário 2.](media/durable-functions-disaster-recovery-geo-distribution/durable-functions-geo-scenario02.png)

Esta abordagem adiciona melhorias no cenário anterior:
- Se a aplicação de função falhar, Gestor de tráfego encarrega-se de falhas em ativação pós-falha para a região secundária. No entanto, uma vez se baseia a aplicação de função na sua própria conta de armazenamento, as funções duráveis continuam a trabalhar.
- Durante uma ativação pós-falha, não há nenhum latência adicional com a ativação através de região, uma vez que a aplicação de função e a conta de armazenamento estão localizadas conjuntamente.
- Falha da camada de armazenamento irá causar falhas nas funções da duráveis, que, por sua vez, serão acionado um redirecionamento para a ativação através de região. Novamente, uma vez que a aplicação de função e armazenamento isolados por região, as funções duráveis continuarão a funcionar.
 
Considerações importantes sobre para este cenário:
- Se a aplicação de função é implementada através de um plano de serviço aplicacional dedicado, replicar a infraestrutura de computação a ativação através de centros de dados aumenta os custos.
- Estado atual não está a ativação pós-falha, que indica que a executar e funções checkpointed falhará. Está a funcionar para a aplicação de cliente para o trabalho de tentativas/reinício.

## <a name="scenario-3---load-balanced-compute-with-grs-shared-storage"></a>Cenário 3 - com balanceamento de carga computação com armazenamento GRS partilhado
Este cenário é uma modificação através do primeiro cenário, a implementação de uma conta de armazenamento partilhado. A principal diferença a que a conta de armazenamento é criada com georreplicação ativada.
Funcionalmente, este cenário fornece as mesmas vantagens como cenário 1, mas permite que as vantagens de recuperação de dados adicionais:
- Armazenamento georredundante (GRS) e GRS de acesso de leitura (RA-GRS) maximização da disponibilidade para a sua conta de armazenamento.
- Se existir uma falha de região do serviço de armazenamento, um das possibilidades é que as operações do Centro de dados determinam que armazenamento podem ser ativado pós-falha para a região secundária. Neste caso, o acesso de conta de armazenamento será redirecionado transparente para a cópia georreplicação da conta do storage, sem intervenção do utilizador.
- Neste caso, o estado das funções duráveis será mantido até a última replicação da conta de armazenamento, ocorre a cada alguns minutos.
Tal como acontece com outros cenários, existem considerações importantes sobre:
- Ativação pós-falha para a réplica é feita ao operadores de centro de dados e pode demorar algum tempo. Até essa hora, a aplicação de função será afetado uma falha.
- Não há um aumento custo de utilização de contas de armazenamento de georreplicação.
- GRS ocorre no modo assíncrono. Algumas das transações mais recentes poderão perder-se devido a latência do processo de replicação.

![Diagrama que mostra cenário 3.](media/durable-functions-disaster-recovery-geo-distribution/durable-functions-geo-scenario03.png)


## <a name="next-steps"></a>Passos Seguintes

Pode ler mais sobre [conceber altamente disponível aplicações utilizando RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md)
