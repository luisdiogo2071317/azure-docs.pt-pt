---
title: Gerir servidores VMware vCenter para recuperação após desastre de VMs de VMware para o Azure com o Azure Site Recovery | Documentos da Microsoft
description: Este artigo descreve como adicionar e gerir o VMware vCenter para recuperação após desastre de VMs de VMware para o Azure com o Azure Site Recovery.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: ramamill
ms.openlocfilehash: 854d7724aebd882414287b19c015ef1425bfa6db
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50210300"
---
# <a name="manage-vmware-vcenter-server"></a>Gerir o servidor VMware vCenter

Este artigo aborda as várias operações de recuperação de sites que podem ser realizadas num VMware vCenter. Verifique se o [pré-requisitos](vmware-physical-azure-support-matrix.md#replicated-machines) antes de começar.


## <a name="set-up-an-account-for-automatic-discovery"></a>Configurar uma conta para a deteção automática

Recuperação de sites precisa de aceder ao VMware para o servidor de processos detetar automaticamente as máquinas virtuais e para a ativação pós-falha e reativação pós-falha de máquinas virtuais. Crie uma conta de acesso da seguinte forma:

1. Inicie sessão no computador do servidor de configuração.
2. Abra o lançamento cspsconfigtool.exe usando o atalho de ambiente de trabalho.
3. Clique em **adicionar conta** sobre o **gerir conta** separador.

  ![add-account](./media/vmware-azure-manage-vcenter/addaccount.png)
1. Forneça os detalhes da conta e clique em **OK** para adicioná-lo.  A conta deve ter os privilégios resumidos na tabela seguinte. 

Demora cerca de 15 minutos para as informações da conta a ser sincronizada a cópia de segurança com o serviço Site Recovery.

### <a name="account-permissions"></a>Permissões de conta

|**Tarefa** | **Conta** | **Permissões** | **Detalhes**|
|--- | --- | --- | ---|
|**Deteção automática/migrar (sem ativação pós-falha)** | Tem de, pelo menos, um utilizador só de leitura | Objeto Data Center –> Propagar ao Objeto Subordinado, função=Só de Leitura | Utilizador atribuído ao nível do datacenter, com acesso a todos os objetos no datacenter.<br/><br/> Para restringir o acesso, atribua a **sem acesso** função com o **propagar ao filho** objeto, aos objetos subordinados (anfitriões vSphere, arquivos de dados, máquinas virtuais e redes).|
|**A replicação/ativação pós-falha** | Tem de, pelo menos, um utilizador só de leitura| Objeto Data Center –> Propagar ao Objeto Subordinado, função=Só de Leitura | Utilizador atribuído ao nível do datacenter, com acesso a todos os objetos no datacenter.<br/><br/> Para restringir o acesso, atribua a **sem acesso** função com o **propagar ao filho** objeto aos objetos subordinados (anfitriões vSphere, arquivos de dados, máquinas virtuais e redes).<br/><br/> Útil para fins de migração, mas a replicação completa não, a ativação pós-falha e reativação pós-falha.|
|**A replicação/ativação pós-falha/reativação pós-falha** | Sugerimos que cria uma função (AzureSiteRecoveryRole) com as permissões necessárias e, em seguida, atribua a função a um grupo ou utilizador de VMware | Objeto Data Center –> propagar ao objeto subordinado, função = AzureSiteRecoveryRole<br/><br/> Arquivo de Dados -> Alocar espaço, navegar no arquivo de dados, operações de ficheiro de baixo nível, remover ficheiros, atualizar ficheiros de máquinas virtuais<br/><br/> Rede -> Atribuição de rede<br/><br/> Recursos -> Atribuir VM a agrupamento de recursos, migrar VMs desligadas, migrar VMs ligadas<br/><br/> Tarefas -> Criar tarefa, atualizar tarefa<br/><br/> Máquina virtual -> Configuração<br/><br/> Máquina virtual -> Interagir -> responder a perguntas, ligação de dispositivos, configurar suportes de dados em CD, configurar suportes de dados em disquete, desligar, ligar, instalação de ferramentas de VMware<br/><br/> Máquina virtual -> Inventário -> Criar, registar, anular o registo<br/><br/> Máquina virtual -> Aprovisionamento -> Permitir transferência de máquinas virtuais, permitir carregamento de ficheiros de máquinas virtuais<br/><br/> Máquina virtual -> Instantâneos -> Remover instantâneos | Utilizador atribuído ao nível do datacenter, com acesso a todos os objetos no datacenter.<br/><br/> Para restringir o acesso, atribua a **sem acesso** função com o **propagar ao filho** objeto, aos objetos subordinados (anfitriões vSphere, arquivos de dados, máquinas virtuais e redes).|


## <a name="add-vmware-server-to-the-vault"></a>Adicionar servidor VMware no Cofre

1. No portal do Azure, abra o Cofre > **infraestrutura do Site Recovery** > **servidores de configuração**e abra o servidor de configuração.
2. Sobre o **detalhes** página, clique em **+ vCenter**.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]

## <a name="modify-credentials"></a>Modificar as credenciais

Modificar as credenciais utilizadas para ligar ao servidor vCenter ou anfitrião ESXi da seguinte forma:

1. Inicie sessão no servidor de configuração e inicie o cspsconfigtool.exe da área de trabalho.
2. Clique em **adicionar conta** sobre o **gerir conta** separador.

  ![add-account](./media/vmware-azure-manage-vcenter/addaccount.png)
3. Forneça os detalhes da conta nova e clique em **OK** para adicioná-lo. A conta deve ter os privilégios listados [acima](#account-permissions).
4. No portal do Azure, abra o Cofre > **infraestrutura do Site Recovery** > **servidores de configuração**e abra o servidor de configuração.
5. Na **detalhes** página, clique em **atualizar servidor**.
6. Depois de concluída a tarefa de atualização do servidor, selecione o vCenter Server, para abrir o vCenter **resumo** página.
7. Selecione a conta recentemente adicionada no **conta de anfitrião do vCenter server/vSphere** campo e clique em **guardar**.

    ![modify-account](./media/vmware-azure-manage-vcenter/modify-vcente-creds.png)

## <a name="delete-a-vcenter-server"></a>Eliminar um servidor vCenter

1. No portal do Azure, abra o Cofre > **infraestrutura do Site Recovery** > **servidores de configuração**e abra o servidor de configuração.
2. Sobre o **detalhes** , selecione o servidor vCenter.
3. Clique nas **eliminar** botão.

  ![eliminar conta](./media/vmware-azure-manage-vcenter/delete-vcenter.png)

> [!NOTE]
Se precisar de modificar o endereço IP do vCenter, FQDN ou porta, em seguida, terá de eliminar o servidor vCenter e adicioná-lo novamente para o portal.
