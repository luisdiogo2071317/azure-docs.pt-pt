---
title: " Gerir servidores VMware vcenter Server no Azure Site Recovery | Microsoft Docs"
description: Este artigo descreve como adicionar e gerir o VMware vCenter no Azure Site Recovery.
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 03/05/2018
ms.author: anoopkv
ms.openlocfilehash: be415340da09043eccd361b0168bb304d8904bef
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="manage-vmware-vcenter-servers"></a>Gerir servidores VMware vcenter Server 

Este artigo aborda as várias operações de recuperação de sites que podem ser executadas no VMware vCenter. Certifique-se a [pré-requisitos](vmware-physical-azure-support-matrix.md#replicated-machines) antes de começar.


## <a name="set-up-an-account-for-automatic-discovery"></a>Configure uma conta para a deteção automática

Recuperação de sites precisa de aceder ao VMware para o servidor de processos detetar automaticamente máquinas virtuais e para ativação pós-falha e a reativação pós-falha de máquinas virtuais. Crie uma conta de acesso da seguinte forma:

1. Inicie sessão no computador do servidor de configuração.
2. Abra a iniciação cspsconfigtool.exe utilizando o atalho do ambiente de trabalho.
3. Clique em **adicionar conta** no **gerir conta** separador.

  ![add-account](./media/vmware-azure-manage-vcenter/addaccount.png)
1. Forneça os detalhes da conta e clique em **OK** adicioná-lo.  A conta deve ter os privilégios resumidos na tabela seguinte. 

Demora cerca de 15 minutos para as informações de conta sincronizado com o serviço de recuperação de sites.

### <a name="account-permissions"></a>Permissões de conta

|**Tarefa** | **Conta** | **Permissões** | **Detalhes**|
|--- | --- | --- | ---|
|**Deteção automática/migrar (sem a reativação pós-falha)** | Precisa de, pelo menos, um utilizador só de leitura | Objeto Data Center –> Propagar ao Objeto Subordinado, função=Só de Leitura | Utilizador atribuído ao nível do datacenter, com acesso a todos os objetos no datacenter.<br/><br/> Para restringir o acesso, atribua o **sem acesso** função com o **Propagate para subordinado** objeto, para os objetos subordinados (anfitriões vSphere, datastores, máquinas virtuais e redes).|
|**Replicação/ativação pós-falha** | Precisa de, pelo menos, um utilizador só de leitura| Objeto Data Center –> Propagar ao Objeto Subordinado, função=Só de Leitura | Utilizador atribuído ao nível do datacenter, com acesso a todos os objetos no datacenter.<br/><br/> Para restringir o acesso, atribua o **sem acesso** função com o **Propagate para subordinado** objeto para os objetos subordinados (anfitriões vSphere, datastores, máquinas virtuais e redes).<br/><br/> Útil para fins de migração, mas não completa replicação, ativação pós-falha, reativação pós-falha.|
|**Replicação/ativação pós-falha/reativação pós-falha** | Sugerimos que crie uma função (AzureSiteRecoveryRole) com as permissões necessárias e, em seguida, atribua a função a um grupo ou utilizador de VMware | Objeto de centro de dados –> Propagate ao objeto do subordinado função = AzureSiteRecoveryRole<br/><br/> Arquivo de Dados -> Alocar espaço, navegar no arquivo de dados, operações de ficheiro de baixo nível, remover ficheiros, atualizar ficheiros de máquinas virtuais<br/><br/> Rede -> Atribuição de rede<br/><br/> Recursos -> Atribuir VM a agrupamento de recursos, migrar VMs desligadas, migrar VMs ligadas<br/><br/> Tarefas -> Criar tarefa, atualizar tarefa<br/><br/> Máquina virtual -> Configuração<br/><br/> Máquina virtual -> Interagir -> responder a perguntas, ligação de dispositivos, configurar suportes de dados em CD, configurar suportes de dados em disquete, desligar, ligar, instalação de ferramentas de VMware<br/><br/> Máquina virtual -> Inventário -> Criar, registar, anular o registo<br/><br/> Máquina virtual -> Aprovisionamento -> Permitir transferência de máquinas virtuais, permitir carregamento de ficheiros de máquinas virtuais<br/><br/> Máquina virtual -> Instantâneos -> Remover instantâneos | Utilizador atribuído ao nível do datacenter, com acesso a todos os objetos no datacenter.<br/><br/> Para restringir o acesso, atribua o **sem acesso** função com o **Propagate para subordinado** objeto, para os objetos subordinados (anfitriões vSphere, datastores, máquinas virtuais e redes).|


## <a name="add-vmware-server-to-the-vault"></a>Adicionar o servidor VMware para o Cofre de

1. No portal do Azure, abra o seu Cofre > **infraestrutura de recuperação de Site** > **servidores de configuração**e abra o servidor de configuração.
2. No **detalhes** página, clique em **+ vCenter**.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]

## <a name="modify-credentials"></a>Modificar as credenciais

Modificar as credenciais utilizadas para ligar ao servidor vCenter ou anfitrião ESXi da seguinte forma:

1. Inicie sessão no servidor de configuração e, inicie o cspsconfigtool.exe no ambiente de trabalho.
2. Clique em **adicionar conta** no **gerir conta** separador.

  ![add-account](./media/vmware-azure-manage-vcenter/addaccount.png)
3. Forneça os detalhes da conta nova e clique em **OK** adicioná-lo. A conta deve ter os privilégios listados [acima](#account-permissions).
4. No portal do Azure, abra o Cofre > **infraestrutura de recuperação de Site** > **servidores de configuração**e abra o servidor de configuração.
5. No **detalhes** página, clique em **atualização do servidor**.
6. Depois de concluída a tarefa de atualização do servidor, selecione o vCenter Server, para abrir o vCenter **resumo** página.
7. Selecione a conta recentemente adicionada no **conta de anfitriões vSphere/servidor vCenter** campo e clique em **guardar**.

    ![modify-account](./media/vmware-azure-manage-vcenter/modify-vcente-creds.png)

## <a name="delete-a-vcenter-server"></a>Eliminar um vCenter server

1. No portal do Azure, abra o seu Cofre > **infraestrutura de recuperação de Site** > **servidores de configuração**e abra o servidor de configuração.
2. No **detalhes** página, selecione o servidor vCenter.
3. Clique em de **eliminar** botão.

  ![delete-account](./media/vmware-azure-manage-vcenter/delete-vcenter.png)

> [!NOTE]
Se necessitar de modificar o endereço IP do vCenter, o FQDN ou a porta, terá de eliminar o servidor vCenter e adicione-a novamente para o portal.
