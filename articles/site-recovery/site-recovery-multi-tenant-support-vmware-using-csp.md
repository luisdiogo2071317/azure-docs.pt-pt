---
title: "Suporte de multi-inquilino para replicação de VM de VMware no Azure (programa CSP) | Microsoft Docs"
description: "Descreve como implementar o Azure Site Recovery num ambiente multi-inquilino para orquestrar a replicação, ativação pós-falha e recuperação de máquinas de virtuais de VMware no local (VMs) para o Azure através do programa de CSP utilizando o portal do Azure"
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2017
ms.author: manayar
ms.openlocfilehash: 273efe0bdef421d753ea51e01060d48351cbe6fc
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/15/2017
---
# <a name="multi-tenant-support-in-azure-site-recovery-for-replicating-vmware-virtual-machines-to-azure-through-csp"></a>Suporte de multi-inquilino no Azure Site Recovery para replicar máquinas virtuais VMware para o Azure através de CSP

O Azure Site Recovery suporta ambientes de multi-inquilinos para subscrições de inquilino. Também suporta vários inquilinos para subscrições de inquilino que são criadas e geridas através do programa de fornecedor de solução em nuvem (CSP) da Microsoft. Este artigo descreve em detalhe as orientações para implementar e gerir a cenários de VMware para o Azure multi-inquilinos. Para obter detalhes sobre como criar e gerir subscrições de inquilino, consulte [gerir vários inquilinos com CSP](site-recovery-manage-multi-tenancy-with-csp.md) .

Esta orientação descontos elevados desenha na documentação do existente para replicar máquinas virtuais VMware para o Azure. Para obter mais informações, consulte [em máquinas virtuais VMware replicar para o Azure com a recuperação de Site](site-recovery-vmware-to-azure.md).

## <a name="multi-tenant-environments"></a>Ambientes de multi-inquilinos
Existem três modelos de multi-inquilinos principais:

* **Partilhado do fornecedor de serviços de alojamento (HSP)**: O parceiro é proprietário a infraestrutura física e utiliza partilhado recursos (vCenter, centros de dados, armazenamento físico e assim sucessivamente) para alojar VMs vários inquilinos na mesma infraestrutura. O parceiro pode fornecer gestão de recuperação após desastre como um serviço gerido ou o inquilino pode ter recuperação após desastre como uma solução de self-service.

* **Dedicado que aloja fornecedor de serviços**: O parceiro é proprietário a infraestrutura física, mas utiliza recursos dedicados (vários vCenters, datastores físico e assim sucessivamente) para alojar VMs cada inquilino numa infraestrutura separada. O parceiro pode fornecer gestão de recuperação após desastre como um serviço gerido ou o inquilino pode tê-la como uma solução de self-service.

* **Gerido fornecedor de serviços (\\<ServerName>\SMS_<SITECODE>\HOTFIX\<KB)**: O cliente possui a infraestrutura física que aloja as VMs e o parceiro fornece gestão e de ativação de recuperação após desastre.

## <a name="shared-hosting-multi-tenant-guidance"></a>Orientações de multi-inquilino de alojamento partilhados
Esta secção abrange o cenário de alojamento partilhados em detalhe. Os outros dois cenários são subconjuntos do cenário de alojamento partilhados e utilizarem princípios da mesmos. As diferenças são descritas no final de orientação alojamento partilhados.

É o requisito básico num cenário de multi-inquilino para isolar os vários inquilinos. Um inquilino não deve ser capaz de observar o que tenha hosted outro inquilino. Num ambiente gerido por um parceiro, este requisito não é tão importante como faz parte de um ambiente de self-service, onde pode ser crítico. Esta orientação assume que o isolamento de inquilino é necessário.

A arquitetura é apresentada no diagrama seguinte:

![HSP partilhado com um vCenter](./media/site-recovery-multi-tenant-support-vmware-using-csp/shared-hosting-scenario.png)  
**Cenário de alojamento partilhados com um vCenter**

Como mostrado no diagrama anterior, cada cliente possui um servidor de gestão separados. Esta configuração limita o acesso de inquilino para VMs de inquilino específico e permite isolamento de inquilinos. Um cenário de replicação da máquina virtual VMware utiliza o servidor de configuração para gerir contas de VMs de detetar e instalar agentes. Princípios da mesmos aplicam-se aos ambientes multi-inquilino, com a adição de restringir a deteção VM através do controlo de acesso do vCenter.

O requisito de isolamento de dados exige que todas as informações de infraestrutura confidenciais (por exemplo, as credenciais de acesso) permanecem undisclosed aos inquilinos. Por este motivo, recomendamos que todos os componentes do servidor de gestão permanecem sob o controlo do parceiro de exclusivo. Os componentes de servidor de gestão são:
* Servidor de configuração (CS)
* Servidor de processos (PS)
* Servidor de destino mestre (MT)

Um PS de escalamento horizontal também está sob o controlo do parceiro.

### <a name="every-cs-in-the-multi-tenant-scenario-uses-two-accounts"></a>Cada CS o cenário de multi-inquilino utiliza duas contas

- **conta de acesso do vCenter**: Utilize esta conta para detetar VMs inquilinas. Tem as permissões de acesso do vCenter atribuídas (conforme descrito na secção seguinte). Para ajudar a evitar fugas acidentais acesso, recomendamos que parceiros introduzir estas credenciais próprios na ferramenta de configuração.

- **Conta de acesso à máquina virtual**: Utilize esta conta para instalar o agente de mobilidade nas VMs de inquilino através de um push automática. Normalmente, é uma conta de domínio que pode fornecer um inquilino para um parceiro, ou que o parceiro poderá gerir diretamente. Se não pretender que um inquilino partilhar os detalhes com o parceiro diretamente, poderem ser permitidas para introduzir as credenciais através de acesso de tempo limitado para o CS ou, com a assistência o parceiro, instalar agentes de mobilidade manualmente.

### <a name="requirements-for-a-vcenter-access-account"></a>Requisitos para uma conta de acesso do vCenter

Tal como mencionado na secção anterior, tem de configurar o CS com uma conta que tenha uma função especial atribuída. A atribuição de função tem de ser aplicada para a conta de acesso de vCenter para cada objeto vCenter e não é propagada para os objetos subordinados. Esta configuração garante o isolamento de inquilinos, porque a propagação de acesso pode resultar num acesso acidental outros objetos.

![Propagate à opção de objetos subordinados](./media/site-recovery-multi-tenant-support-vmware-using-csp/assign-permissions-without-propagation.png)

A alternativa é para atribuir a função do objecto de centro de dados e conta de utilizador e a propagação-las para os objetos subordinados. Em seguida, atribua à conta de um *sem acesso* função para todos os objetos (por exemplo, VMs dos outros inquilinos) que devem ser acessível a um determinado inquilino. Esta configuração é complexa e expõe controlos de acesso acidentais, porque todos os objetos subordinados novo também automaticamente é concedido acesso é herdado do principal. Por conseguinte, recomendamos que utilize a primeira abordagem.

O procedimento de acesso de conta do vCenter é o seguinte:

1. Criar uma nova função através da clonagem previamente definido *só de leitura* função e, em seguida, atribua um nome conveniente (por exemplo, Azure_Site_Recovery, conforme mostrado neste exemplo).

2. Atribua as seguintes permissões a esta função:

    * **Arquivo de dados**: alocar espaço, arquivo de dados de procura, operações de baixo nível de ficheiro, remova ficheiros, ficheiros de atualização de máquina virtual

    * **Rede**: atribuir de rede

    * **Recurso**: VM atribuir ao agrupamento de recursos, migrar desligada VM, com tecnologia migrar na VM

    * **Tarefas**: criar a tarefa, a tarefa de atualização

    * **Máquina virtual**:
        * Configuração > todos os
        * Interação > responder pergunta, ligação do dispositivo, CD configurar suporte de dados, configurar disquetes suportes de dados, desligar, ligar, instalação de ferramentas do VMware
        * Inventário > criar a partir de existentes, criar um novo, registar, anular o registo
        * Aprovisionamento > Permitir transferências de máquina virtual, o carregamento de ficheiros de máquina virtual de permitir
        * Gestão de instantâneos > Remover instantâneos

    ![A caixa de diálogo Editar função](./media/site-recovery-multi-tenant-support-vmware-using-csp/edit-role-permissions.png)

3. Atribua níveis de acesso à conta de vCenter (utilizada no inquilino CS) para vários objetos, da seguinte forma:

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

Para restringir operações de recuperação após desastre até o estado de ativação pós-falha (ou seja, sem capacidades de reativação pós-falha), siga o procedimento anterior, com uma exceção: em vez de atribuir o *Azure_Site_Recovery* função ao vCenter aceder à conta, atribua apenas um *só de leitura* função a essa conta. Este conjunto de permissões permite VM replicação e ativação pós-falha e não permite a reativação pós-falha. Tudo o resto do processo anterior permanece como está. Para garantir o isolamento de inquilinos e restringir a deteção VM, cada permissão é ainda atribuído apenas ao nível do objeto e não propagada aos objetos subordinados.

## <a name="other-multi-tenant-environments"></a>Outros ambientes de multi-inquilinos

A secção anterior descrito como configurar um ambiente de multi-inquilino para uma solução de alojamento partilhado. As outras duas principais soluções estão dedicadas de alojamento e de serviço geridas. A arquitetura destas soluções é descrita nas secções seguintes.

### <a name="dedicated-hosting-solution"></a>Dedicado a solução de alojamento

Como é mostrado no diagrama seguinte, a diferença da arquitetura numa solução de alojamento dedicada é que a infraestrutura de cada inquilino é definida para esse inquilino apenas. Uma vez que os inquilinos estão isolados através de vCenters separada, o fornecedor de alojamento ainda tem de seguir os passos CSP fornecidos para o alojamento partilhados e não precisa de preocupar com isolamento de inquilino. Configuração CSP permanece inalterada.

![hsp arquitetura partilhado](./media/site-recovery-multi-tenant-support-vmware-using-csp/dedicated-hosting-scenario.png)  
**Cenário de alojamento dedicado com vários vCenters**

### <a name="managed-service-solution"></a>Solução de serviço geridas

Como é mostrado no diagrama seguinte, a diferença da arquitetura numa solução de serviço geridas é que a infraestrutura de cada inquilino também está separada fisicamente da infraestrutura dos outros inquilinos. Este cenário existe, normalmente, quando o inquilino é proprietário da infraestrutura e pretender que um fornecedor de solução para gerir a recuperação após desastre. Novamente, porque os inquilinos estão fisicamente isolados através de infraestruturas diferentes, as necessidades de parceiro a seguir os passos CSP fornecido para o alojamento partilhados, mas não precisa de preocupar com isolamento de inquilino. Aprovisionamento de CSP permanece inalterado.

![hsp arquitetura partilhado](./media/site-recovery-multi-tenant-support-vmware-using-csp/managed-service-scenario.png)  
**Cenário de serviço com vários vCenters de geridos**

## <a name="next-steps"></a>Passos seguintes
[Saiba mais](site-recovery-role-based-linked-access-control.md) sobre o controlo de acesso baseado em funções para gerir implementações do Azure Site Recovery.

[Gerir vários inquilinos com CSP](site-recovery-manage-multi-tenancy-with-csp.md)
