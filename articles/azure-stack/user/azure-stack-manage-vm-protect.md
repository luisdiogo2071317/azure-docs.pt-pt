---
title: Proteger VMs implementadas na pilha do Azure | Microsoft Docs
description: Orientações sobre como proteger máquinas virtuais implementadas na pilha do Azure.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 4e5833cf-4790-4146-82d6-737975fb06ba
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/14/2018
ms.author: jeffgilb
ms.reviewer: hector.linares
ms.openlocfilehash: 734ee0e6ffb0dab660a2b63b431780208e0e0484
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
ms.locfileid: "34165269"
---
# <a name="protect-virtual-machines-deployed-on-azure-stack"></a>Proteger máquinas virtuais implementadas na pilha do Azure

Utilize este artigo como guia para desenvolver um plano para proteger as máquinas virtuais (VMs) que os utilizadores implementam na pilha do Azure.

Para proteger contra a perda de dados e o período de indisponibilidade não planeado, terá de implementar um plano de recuperação de cópia de segurança ou recuperação de desastre para aplicações de utilizador e os respetivos dados. Este plano poderá ser exclusivo para cada aplicação mas segue uma estrutura estabelecida pelo continuidade do negócio abrangente da sua organização e a estratégia de recuperação (BC/DR) de desastre. É um ponto de partida [conceber aplicações resilientes para o Azure](https://docs.microsoft.com/azure/architecture/resiliency), proporcionando gerais padrões e práticas de disponibilidade de aplicações e resiliência.

>[!IMPORTANT]
> Teste os planos de recuperação de cópia de segurança e recuperação após desastre numa base contínua. Tem de esta opção para garantir que:
> * Os planos de trabalho
> * Os planos ainda necessidades que foram concebidos para.

## <a name="azure-stack-infrastructure-recovery"></a>Recuperação de infraestrutura de pilha do Azure

Os utilizadores são responsáveis por proteger as respetivas VMs separadamente a partir dos serviços de infraestrutura da pilha do Azure.

O plano de recuperação para os serviços de infraestrutura do Azure pilha **não** incluem a recuperação de VMs do utilizador, contas de armazenamento ou bases de dados. Como o proprietário da aplicação, é responsável por implementar um plano de recuperação para as suas aplicações e dados.

Se a nuvem de pilha do Azure está offline para um período de tempo alargado ou permanentemente irrecuperável, tem de ter um plano de recuperação colocar que:

* Garante o período de indisponibilidade mínimo
* Mantém VMs crítico, tais como servidores de base de dados, em execução
* Permite que as aplicações a manter a servir pedidos do utilizador

O operador da nuvem pilha do Azure é responsável pela criação de um plano de recuperação para os serviços e infraestrutura subjacente de pilha do Azure. Para obter mais informações, leia o artigo [recuperar a partir de perda catastrófica de dados](https://docs.microsoft.com/azure/azure-stack/azure-stack-backup-recover-data).

## <a name="sourcetarget-combinations"></a>Origem/destino combinações

Cada nuvem de pilha do Azure é implementada para um centro de dados. Num ambiente separado é necessário, pelo que pode recuperar as suas aplicações. O ambiente de recuperação pode ser outra nuvem de pilha do Azure num centro de dados diferente ou de nuvem pública do Azure. O soberania de dados e os requisitos de privacidade de dados irão determinar o ambiente de recuperação para a sua aplicação. Como ativar a proteção para cada aplicação, tem a flexibilidade para escolher uma opção de recuperação específico para cada um deles. Pode ter aplicações na subscrição de uma cópia de segurança de dados para outro datacenter. Na outra subscrição, pode replicar os dados na nuvem pública do Azure.

Planeie a estratégia de recuperação de cópia de segurança e recuperação de desastres para cada aplicação para determinar o destino para cada aplicação. Um plano de recuperação irá ajudar a sua organização corretamente o tamanho do armazenamento capacidade necessária no local e projeto consumo na nuvem pública.

|  | Global Azure | A pilha do Azure implementado no datacenter CSP e operada por CSP | Pilha do Azure implementadas no Centro de dados do cliente e que operava pelo cliente |
|------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| **A pilha do Azure implementado no datacenter CSP e operada por CSP** | VMs do utilizador são implementadas com a pilha do Azure do CSP operado. VMs do utilizador são restauradas a partir de cópia de segurança ou a ativação pós-falha diretamente para o Azure. | CSP funciona as instâncias principais e secundários da pilha do Azure nos respetivos centros de dados. VMs do utilizador são restauradas ou a ativação pós-falha entre as duas instâncias de pilha do Azure. | CSP funciona pilha do Azure no site primário. Centro de dados do cliente é o destino de restauro ou a ativação pós-falha. |
| **Pilha do Azure implementadas no Centro de dados do cliente e que operava pelo cliente** | VMs do utilizador são implementadas para o cliente operado pilha do Azure. VMs do utilizador são restauradas a partir de cópia de segurança ou a ativação pós-falha diretamente para o Azure. | Cliente funciona as instâncias principais e secundários da pilha do Azure nos respetivos centros de dados. VMs do utilizador são restauradas ou a ativação pós-falha entre as duas instâncias de pilha do Azure. | Cliente funciona pilha do Azure no site primário. Datacenter do CSP é o destino de restauro ou a ativação pós-falha. |

![Combinações de destino de origem](media\azure-stack-manage-vm-backup\vm_backupdataflow_01.png)

## <a name="application-recovery-objectives"></a>Objetivos de recuperação de aplicações

Terá de determinar a quantidade de perda de dados e de período de indisponibilidade que organização pode tolerar para cada aplicação. Por quantifying perda de dados e o período de indisponibilidade pode criar um plano de recuperação que minimiza o impacto de um desastre na sua organização. Para cada aplicação, tenha em consideração:

 - **Objetivo de tempo de recuperação (RTO)**  
RTO é o tempo de aceitável máximo que uma aplicação pode não estar disponível depois de um incidente. Por exemplo, um RTO de 90 minutos significa que tem de ser capaz de restaurar a aplicação num Estado em execução dentro de 90 minutos desde o início de um desastre. Se tiver um RTO baixa, poderá manter uma segunda implementação continuamente em execução no modo de espera para proteção contra uma falha regional.
 - **Objetivo de ponto de recuperação (RPO)**  
O RPO é a duração máxima de perda de dados que é aceitável durante um desastre. Por exemplo, se armazenar os dados numa única base de dados, sem qualquer replicação para outras bases de dados, e fizer cópias de segurança de hora a hora, poderá perder uma hora de dados.

O RTO e RPO são requisitos comerciais. Realize uma avaliação de risco definir RTO e RPO da aplicação.

Outra métrica é **média de tempo para recuperar** (MTTR), que é o tempo médio necessário para restaurar a aplicação após uma falha. MTTR é um valor empirical para um sistema. Se o MTTR exceder o RTO, então uma falha no sistema irá causar uma interrupção inaceitável do negócio, porque não será possível restaurar o sistema no RTO definido.

### <a name="backup-restore"></a>Restauro da cópia de segurança

O esquema de proteção mais comuns para aplicações baseadas na VM consiste em utilizar o software de cópia de segurança. Fazer uma cópia de segurança de uma VM normalmente inclui o sistema operativo, configuração do sistema operativo, os binários da aplicação e dados da aplicação. As cópias de segurança são criadas por um instantâneo de volumes, discos ou toda a VM. Com a pilha do Azure, tem a flexibilidade de cópia de segurança de dentro do contexto do SO convidado ou do armazenamento do Azure pilha e APIs de computação. Pilha do Azure não suporta a criar cópias de segurança ao nível do hipervisor.
 
![Cópia de segurança restor](media\azure-stack-manage-vm-backup\vm_backupdataflow_03.png)

Recuperar a aplicação requer o restauro de uma ou mais VMs na mesma nuvem ou para uma nuvem de novo. Pode visar uma nuvem no seu centro de dados ou na nuvem pública. A nuvem que escolher completamente dentro do seu controlo e baseia-se nos seus requisitos de privacidade e soberania dos dados.
 
 - RTO: Período de indisponibilidade medido em horas
 - RPO: Perda de dados da variável (dependendo da frequência de cópia de segurança)
 - Topologia de implementação: ativo/passivo

#### <a name="planning-your-backup-strategy"></a>Planear a estratégia de cópia de segurança

Planear a estratégia de cópia de segurança e definir os requisitos de escala começa com quantifying o número de instâncias VM que necessitam de proteção. Cópia de segurança de todas as VMs em todos os servidores num ambiente é uma estratégia de comuns. No entanto, com a pilha do Azure, existem algumas VMs que necessitam de uma cópia de segurança. Por exemplo, VMs num conjunto de dimensionamento são consideradas efémeras recursos que podem ficar e aceda, por vezes, sem aviso prévio. Quaisquer dados duráveis que tem de ser protegida são armazenados num repositório separado, como um arquivo de base de dados ou o objeto.

Considerações importantes sobre para cópia de segurança das VMs na pilha do Azure:

 - **Categorização**
    - Considere um modelo em que os utilizadores optar ativamente por participar na cópia de segurança VM.
    - Defina um recuperação ao nível contrato de serviço (SLA) com base na prioridade de aplicações ou o impacto no negócio.
 - **Dimensionamento**
    - Quando um grande número de novas VMs Dependency (se a cópia de segurança é necessária), considere escalonadas cópias de segurança.
    - Avalie produtos de cópia de segurança que podem capturar e transmitir dados de cópia de segurança para minimizar o conteúdo de recursos na solução de forma eficiente.
    - Avalie produtos de cópia de segurança de forma eficiente armazenam cópias de segurança utilizando cópias de segurança incrementais ou diferenciais para minimizar a necessidade de cópias de segurança completas em todas as VMs no ambiente.
 - **Restaurar**
    - Podem restaurar a cópia de segurança produtos discos virtuais, dados de aplicações dentro de uma VM existente, ou o recurso VM todo e discos virtuais associados. O esquema de restauro que precisa depende de como pretende restaurar a aplicação e irá afetar o tempo de aplicação para recuperação. Por exemplo, poderá ser mais fácil de Reimplementar o SQL server a partir de um modelo e, em seguida, restaurar as bases de dados em vez de restaurar a toda a VM ou um conjunto de VMs.

### <a name="replicationmanual-failover"></a>Ativação pós-falha de replicação/manual

Uma abordagem alternativa para suportar a elevada disponibilidade está a replicar as VMs de aplicação para outra nuvem e dependem de uma ativação pós-falha manual. A replicação do sistema operativo, os binários da aplicação e dados da aplicação pode ser executada no nível VM ou nível de SO convidado. A ativação pós-falha é gerida a utilização de software adicional que não faz parte da aplicação.

Com esta abordagem, a aplicação é implementada na nuvem de uma e respetivos VM é replicado para outra nuvem. Se uma ativação pós-falha é acionada, secundário VMs tem de ser ligado na nuvem segundo. Em alguns cenários, a ativação pós-falha cria os discos de VMs e anexa aos mesmos. Este processo pode demorar muito tempo a concluir, especialmente com uma aplicação de várias camadas de mensagens em fila que necessita de uma sequência de arranque específico. Também pode ser passos que tem de ser executados antes da aplicação está pronta para começar a pedidos de serviço.

![Ativação pós-falha do manual de replicação](media\azure-stack-manage-vm-backup\vm_backupdataflow_02.png)

 - RTO: Período de indisponibilidade medido em minutos
 - RPO: Perda de dados da variável (dependendo da frequência de replicação)
 - Topologia de implementação: modo de espera ativo/passivo por
 
### <a name="high-availabilityautomatic-failover"></a>Ativação pós-falha de elevada disponibilidade/automático

Para aplicações em que a sua empresa pode tolerar alguns segundos ou minutos de inatividade e perda mínima de dados, terá de considerar uma configuração de elevada disponibilidade. Aplicações de elevada disponibilidade foram concebidas para rapidamente e automaticamente recuperar de falhas. Para falhas de local hardware, infraestrutura de pilha do Azure implementa elevada disponibilidade da rede física com duas principais de comutadores de bastidor. Para falhas de nível de computação, a pilha do Azure utiliza vários nós na unidade de escala. Ao nível da VM, pode utilizar os conjuntos de dimensionamento em combinação com domínios de falhas para garantir a falhas de nó não tirá a sua aplicação.

Em combinação com conjuntos de dimensionamento, a aplicação será necessário suportar a elevada disponibilidade nativamente ou suportar a utilização de clustering de software. Por exemplo, Microsoft SQL Server suporta nativamente elevada disponibilidade para bases de dados utilizando o modo de consolidação síncrona. No entanto, se pode apenas suportar a replicação assíncrona, em seguida, haverá alguma perda de dados. Também é possível implementar aplicações num cluster de ativação pós-falha em que o software de clustering processa a ativação pós-falha automática da aplicação.

Através desta abordagem, a aplicação só está ativa uma nuvem, mas o software é implementado em várias nuvens. Outras nuvens estejam no modo de espera modo pronto para iniciar a aplicação quando a ativação pós-falha é activada.

 - RTO: Período de indisponibilidade medido em segundos
 - RPO: Perda mínima de dados
 - Topologia de implementação: modo de espera por ativo/ativo

### <a name="fault-tolerance"></a>Tolerância a falhas

Pilha do Azure de disponibilidade do serviço de redundância e a infraestrutura física só proteger face ao hardware de nível de falhas/falhas esse disco, fonte de alimentação, porta de rede ou nó. No entanto, se a aplicação tem de estar sempre disponível e nunca pode perder os dados, terá de implementar a tolerância a falhas nativamente na sua aplicação ou utilize o software adicional para ativar a tolerância a falhas.

Primeiro, precisa de Certifique-se de que a aplicação que VMS implementadas utilizando escala define para proteger contra falhas de nível de nó. Para proteger contra a nuvem ficar offline, a mesma aplicação tem já de ser implementada numa nuvem diferente, pelo que pode continuar a manutenção pedidos sem interrupção Este modelo é normalmente designado por uma implementação de ativo-ativo.

Tenha em atenção que cada nuvem de pilha do Azure está independente entre si, para que as nuvens são sempre consideradas ativas de uma perspetiva de infraestrutura. Neste caso, várias instâncias de Active Directory da aplicação são implementadas numa ou mais nuvens Active Directory.

 - RTO: Sem períodos de indisponibilidade
 - O RPO: Qualquer perda de dados
 - Topologia de implementação: ativo/ativo

### <a name="no-recovery"></a>Sem recuperação

Algumas aplicações no seu ambiente podem não necessitar de proteção contra períodos de indisponibilidade não planeado ou perda de dados. Por exemplo, VMs utilizado para desenvolvimento e teste, normalmente, não precisa de ser recuperados. É a decisão de fazer sem proteção de uma aplicação ou uma VM específica. Pilha do Azure oferece cópia de segurança ou a replicação de VMs de infraestrutura subjacente. Semelhante para o Azure, terá de optar ativamente por participar proteção para cada VM em cada uma das suas subscrições.

 - RTO: irrecuperável
 - RPO: Perda de dados completa

## <a name="recommended-topologies"></a>Topologias recomendadas

Considerações importantes sobre a implementação de pilha do Azure:

|     | Recomendação | Comentários |
|-------------------------------------------------------------------------------------------------|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cópia de segurança/restauro VMs a um destino de cópia de segurança externo já implementada no seu centro de dados | Recomendado | Tire partido da infraestrutura de cópia de segurança existente e competências operacionais. Certifique-se para o tamanho da infraestrutura de cópia de segurança para que fique pronta para proteger as instâncias de VM adicionais. Certifique-se a infraestrutura de cópia de segurança não está a ser próximos para a origem. Pode restaurar VMs para a origem de pilha do Azure, para uma instância de pilha do Azure secundária ou Azure. |
| Cópia de segurança/restauro VMs a um destino de cópia de segurança externo dedicado a pilha do Azure | Recomendado | Pode comprar nova infraestrutura de cópia de segurança ou de infraestrutura de cópia de segurança dedicado de aprovisionamento para a pilha do Azure. Certifique-se a infraestrutura de cópia de segurança não está a ser próximos para a origem. Pode restaurar VMs para a origem de pilha do Azure, para uma instância de pilha do Azure secundária ou Azure. |
| Cópia de segurança/restauro VMs diretamente ao global Azure ou de um fornecedor de serviço fidedigno | Recomendado | Desde que pode satisfazer os seus requisitos de regulamentação e privacidade dos dados, pode armazenar as cópias de segurança no global Azure ou de um fornecedor de serviço fidedigno. Idealmente, o fornecedor de serviços também está em execução pilha do Azure para obter consistência na experiência operacional quando restaurar. |
| Replicar/ativação pós-falha VMs a uma instância separada do pilha do Azure | Recomendado | No caso de ativação pós-falha, terá de ter uma segundo pilha do Azure na nuvem totalmente operacional, por isso pode evitar períodos de indisponibilidade de aplicações expandida. |
| Replicar/ativação pós-falha VMs diretamente para o Azure ou um fornecedor de serviço fidedigno | Recomendado | Desde que pode satisfazer os seus requisitos de regulamentação e privacidade dos dados, pode replicar os dados global do Azure ou um fornecedor de serviço fidedigno. Idealmente, o fornecedor de serviços também está em execução pilha do Azure para obter consistência na experiência operacional após a ativação pós-falha. |
| Implementar o destino de cópia de segurança na mesma nuvem pilha do Azure com os seus dados de aplicação | Não recomendado | Evite armazenar cópias de segurança na mesma nuvem pilha do Azure. Período de indisponibilidade não planeado da nuvem pode que tenha dos dados de cópia de segurança e de dados principal. Se optar por implementar um destino de cópia de segurança como uma aplicação virtual (para fins de otimização para cópia de segurança e restauro), certifique-se de todos os dados continuamente são copiados para uma localização de cópia de segurança externa. |
| Implementar o dispositivo de cópia de segurança físico para o mesmo bastidor onde a solução de pilha do Azure está instalada | Não suportado | Neste ponto no tempo, não é possível ligar a todos os outros dispositivos na parte superior do comutadores bastidor que não fazem parte da solução original. |

## <a name="next-steps"></a>Passos Seguintes

Este artigo fornecido Diretrizes gerais para proteger as VMs do utilizador implementadas na pilha do Azure. Para obter informações sobre como utilizar os serviços do Azure para proteger VMs do utilizador, consulte:

 - [Utilizar o Backup do Azure para cópia de segurança de ficheiros e aplicações na pilha do Azure](https://docs.microsoft.com/azure/backup/backup-mabs-files-applications-azure-stack)
 - [Suporte de servidor do Backup do Azure para a pilha do Azure](https://docs.microsoft.com/azure/backup/ ) 
 - [Suporte do Azure Site Recovery para a pilha do Azure](https://docs.microsoft.com/azure/site-recovery/)  

Para saber mais sobre os produtos de parceiro que oferecem proteção de VM na pilha do Azure, consulte "[proteger aplicações e os dados na pilha do Azure](https://azure.microsoft.com/blog/protecting-applications-and-data-on-azure-stack/)."
