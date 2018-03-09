---
title: "Descrição geral do suporte de multi-inquilino para a replicação de VM de VMware para Azure (CSP) utilizando o Azure Site Recovery | Microsoft Docs"
description: "Fornece uma descrição geral do suporte do Azure Site Recovery para o inquilino subscrições num ambiente multi-inquilino, através do programa de CSP."
services: site-recovery
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 03/05/2018
ms.author: manayar
ms.openlocfilehash: 9b4fbb34686a12f992b344ac61420c9ba99ee405
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="overview-of-multi-tenant-support-for-vmware-replication-to-azure-with-csp"></a>Descrição geral do suporte de multi-inquilino para replicação de VMware no Azure com o CSP

[O Azure Site Recovery](site-recovery-overview.md) suporta ambientes de multi-inquilinos para subscrições de inquilino. Também suporta vários inquilinos para subscrições de inquilino que são criadas e geridas através do programa de fornecedor de solução em nuvem (CSP) da Microsoft. 

Este artigo fornece uma descrição geral da implementação e gestão de multi-inquilino VMware para a replicação do Azure. 

## <a name="multi-tenant-environments"></a>Ambientes de multi-inquilinos

Existem três modelos de multi-inquilinos principais:

* **Partilhado do fornecedor de serviços de alojamento (HSP)**: O parceiro é proprietário a infraestrutura física e utiliza partilhado recursos (vCenter, centros de dados, armazenamento físico e assim sucessivamente) para alojar vários inquilinos VMs na mesma infraestrutura. O parceiro pode fornecer gestão de recuperação após desastre como um serviço gerido ou o inquilino pode ter recuperação após desastre como uma solução de self-service.

* **Dedicado que aloja fornecedor de serviços**: O parceiro é proprietário a infraestrutura física, mas utiliza recursos dedicados (vários vCenters, datastores físico e assim sucessivamente) para alojar VMs cada inquilino numa infraestrutura separada. O parceiro pode fornecer gestão de recuperação após desastre como um serviço gerido ou o inquilino pode tê-la como uma solução de self-service.

* **Gerido fornecedor de serviços (\\<ServerName>\SMS_<SITECODE>\HOTFIX\<KB)**: O cliente possui a infraestrutura física que aloja as VMs e o parceiro fornece gestão e de ativação de recuperação após desastre.

## <a name="shared-hosting-services-provider-hsp"></a>Fornecedor de serviços de alojamento partilhados (HSP)

 Os outros dois cenários são subconjuntos do cenário de alojamento partilhados e utilizarem princípios da mesmos. As diferenças são descritas no final de orientação alojamento partilhados.

O requisito básico num cenário de multi-inquilino é que os inquilinos tem de ser isolados. Um inquilino não deve ser capaz de observar o que tenha hosted outro inquilino. Num ambiente gerido por um parceiro, este requisito não é tão importante como faz parte de um ambiente de self-service, onde pode ser crítico. Este artigo pressupõe que o isolamento de inquilino é necessário.

A arquitetura é mostrada no diagrama seguinte.

![HSP partilhado com um vCenter](./media/vmware-azure-multi-tenant-overview/shared-hosting-scenario.png)  

**Partilhado-aloja com um servidor vCenter**

No diagrama, cada cliente possui um servidor de gestão separados. Esta configuração limita o acesso de inquilino para VMs de inquilino específico e permite isolamento de inquilinos. A replicação de VM de VMware utiliza o servidor de configuração para detetar VMs e instalar agentes. Princípios da mesmos aplicam-se aos ambientes multi-inquilino, com a adição de restringir a deteção VM utilizando o controlo de acesso do vCenter.

O requisito de isolamento de dados significa que todas as informações de infraestrutura confidenciais (por exemplo, as credenciais de acesso) permanecem undisclosed aos inquilinos. Por este motivo, recomendamos que todos os componentes do servidor de gestão permanecem sob o controlo do parceiro de exclusivo. Os componentes de servidor de gestão são:

* Servidor de configuração)
* Servidor de processos
* Servidor de destino mestre

Também é um servidor de processos de escalamento horizontal separada sob o controlo do parceiro.

## <a name="configuration-server-accounts"></a>Contas de servidor de configuração

Cada servidor de configuração no cenário de multi-inquilino utiliza duas contas:

- **conta de acesso do vCenter**: esta conta é utilizada para detetar VMs inquilinas. Tem as permissões de acesso do vCenter atribuídas. Para ajudar a evitar fugas de acesso, recomendamos que parceiros introduzir estas credenciais próprios na ferramenta de configuração.

- **Conta de acesso à máquina virtual**: esta conta é utilizada para instalar o agente do serviço de mobilidade no inquilino VMs, com um push automática. Normalmente, é uma conta de domínio que um inquilino pode fornecer a um parceiro ou uma conta que o parceiro poderá gerir diretamente. Se não pretender que um inquilino partilhar os detalhes com o parceiro diretamente, ele podem introduzir as credenciais através de acesso de tempo limitado ao servidor de configuração. Em alternativa, com a assistência o parceiro, podem instalar o agente do serviço de mobilidade manualmente.

## <a name="vcenter-account-requirements"></a>requisitos de conta do vCenter

Tem de configurar o servidor de configuração com uma conta que tenha uma função especial atribuída. 

- A atribuição de função tem de ser aplicada para a conta de acesso de vCenter para cada objeto vCenter e não é propagada para os objetos subordinados. Esta configuração garante o isolamento de inquilinos, porque a propagação de acesso pode resultar num acesso acidental outros objetos.

    ![Propagate à opção de objetos subordinados](./media/vmware-azure-multi-tenant-overview/assign-permissions-without-propagation.png)

- A abordagem alternativa é para atribuir a função do objecto de centro de dados e conta de utilizador e propagá-las para os objetos subordinados. Em seguida, atribua à conta de um **sem acesso** função para todos os objetos (por exemplo, as VMs que pertencem aos outros inquilinos) que deve ser acessível a um determinado inquilino. Esta configuração é complexa. Expõe controlos de acesso acidentais, porque todos os objetos subordinados novo também automaticamente é concedido acesso é herdado do principal. Por conseguinte, recomendamos que utilize a primeira abordagem.

### <a name="create-a-vcenter-account"></a>Criar uma conta do vCenter

1. Criar uma nova função através da clonagem predefinidos *só de leitura* função e, em seguida, atribua um nome conveniente (por exemplo, Azure_Site_Recovery, conforme mostrado neste exemplo).
2. Atribua as seguintes permissões a esta função:

    * **Arquivo de dados**: alocar espaço, arquivo de dados de procura, operações de baixo nível de ficheiro, remova ficheiros, ficheiros de atualização de máquina virtual
    * **Rede**: atribuir de rede
    * **Recurso**: VM atribuir ao agrupamento de recursos, migrar desligada VM, com tecnologia migrar na VM
    * **Tarefas**: criar a tarefa, a tarefa de atualização
    * **VM - configuração**: todos os
    - **VM - interação** > responder pergunta, ligação do dispositivo, CD configurar suporte de dados, configurar disquetes suportes de dados, desligar, ligar, instalação de ferramentas do VMware
    - **VM - inventário** > criar a partir de existentes, criar um novo, registar, anular o registo
    - **VM - aprovisionamento** > Permitir transferências de máquina virtual, o carregamento de ficheiros de máquina virtual de permitir
    - **VM - gestão de instantâneos** > Remover instantâneos

        ![A caixa de diálogo Editar função](./media/vmware-azure-multi-tenant-overview/edit-role-permissions.png)

3. Atribua níveis de acesso à conta do vCenter (utilizado no servidor de configuração de inquilino) para vários objetos, da seguinte forma:

>| Objeto | Função | Observações |
>| --- | --- | --- |
>| vCenter | Só de Leitura | É necessário apenas para permitir o acesso de vCenter para gerir diferentes objetos. Pode remover esta permissão, se a conta nunca vai ser fornecida para um inquilino ou utilizada para quaisquer operações de gestão no vCenter. |
>| Datacenter | Azure_Site_Recovery |  |
>| Anfitriões e cluster de anfitriões | Azure_Site_Recovery | Novamente garante que é o acesso ao nível do objeto, para que apenas os anfitriões acessíveis tenham VMs inquilinas antes da ativação pós-falha e depois de reativação pós-falha. |
>| Cluster de arquivo de dados e o arquivo de dados | Azure_Site_Recovery | Igual ao anterior. |
>| Rede | Azure_Site_Recovery |  |
>| Servidor de gestão | Azure_Site_Recovery | Inclui o acesso a todos os componentes (CS, PS e MT) fora da máquina de CS. |
>| VMs de inquilino | Azure_Site_Recovery | Assegura que nenhum inquilino novo VMs de um determinado inquilino também obter este acesso ou não estarão disponíveis Detetáveis através do portal do Azure. |

O acesso de conta do vCenter está agora concluído. Este passo satisfaz o requisito de permissões mínimas para concluir as operações de reativação pós-falha. Também pode utilizar estas permissões de acesso com as políticas existentes. Apenas modificar as permissões existentes definidas para incluir permissões de função do passo 2, detalhadas anteriormente.

### <a name="failover-only"></a>Ativação pós-falha apenas
Para restringir operações de recuperação após desastre até apenas ativação pós-falha (ou seja, sem capacidades de reativação pós-falha), utilize o procedimento anterior, com as seguintes exceções:

- Em vez de atribuir o *Azure_Site_Recovery* função para a conta de acesso do vCenter, atribuir apenas um *só de leitura* função a essa conta. Este conjunto de permissões permite VM replicação e ativação pós-falha e não permite a reativação pós-falha.
- Tudo o resto do processo anterior permanece como está. Para garantir o isolamento de inquilinos e restringir a deteção VM, cada permissão é ainda atribuído apenas ao nível do objeto e não propagada aos objetos subordinados.


## <a name="dedicated-hosting-solution"></a>Dedicado a solução de alojamento

Como é mostrado no diagrama seguinte, a diferença da arquitetura numa solução de alojamento dedicada é que a infraestrutura de cada inquilino é definida para esse inquilino apenas. Uma vez que os inquilinos estão isolados através de vCenters separada, o fornecedor de alojamento ainda tem de seguir os passos CSP fornecidos para o alojamento partilhados e não precisa de preocupar com isolamento de inquilino. Configuração CSP permanece inalterada.

![architecture-shared-hsp](./media/vmware-azure-multi-tenant-overview/dedicated-hosting-scenario.png)  
**Cenário de alojamento dedicado com vários vCenters**

## <a name="managed-service-solution"></a>Solução de serviço geridas

Como é mostrado no diagrama seguinte, a diferença da arquitetura numa solução de serviço geridas é que a infraestrutura de cada inquilino também está separada fisicamente da infraestrutura dos outros inquilinos. Este cenário existe, normalmente, quando o inquilino é proprietário da infraestrutura e pretender que um fornecedor de solução para gerir a recuperação após desastre. Novamente, porque os inquilinos estão fisicamente isolados através de infraestruturas diferentes, as necessidades de parceiro a seguir os passos CSP fornecido para o alojamento partilhados, mas não precisa de preocupar com isolamento de inquilino. Aprovisionamento de CSP permanece inalterado.

![architecture-shared-hsp](./media/vmware-azure-multi-tenant-overview/managed-service-scenario.png)  
**Cenário de serviço com vários vCenters de geridos**

## <a name="next-steps"></a>Passos Seguintes
[Saiba mais](site-recovery-role-based-linked-access-control.md) sobre o controlo de acesso baseado em funções na recuperação de sites.
Saiba como [configurar a recuperação após desastre de VMs de VMware para Azure](vmware-azure-tutorial.md)
[configurar a recuperação após desastre para as VMs de VMWare com vários inquilinos com CSP](vmware-azure-multi-tenant-csp-disaster-recovery.md)
