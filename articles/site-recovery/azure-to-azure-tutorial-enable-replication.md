---
title: Configurar a recuperação após desastre de VMs do Azure para uma região secundária do Azure com o Azure Site Recovery
description: Saiba como configurar a recuperação após desastre de VMs do Azure para uma região diferente do Azure, utilizando o serviço Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 10/10/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 0404774f1cb347ceead8b78d1a9a6506712dea5c
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "49069102"
---
# <a name="set-up-disaster-recovery-for-azure-vms-to-a-secondary-azure-region"></a>Configurar a recuperação após desastre de VMs do Azure para uma região secundária do Azure

O serviço [Azure Site Recovery](site-recovery-overview.md) contribui para a estratégia de recuperação após desastre, através da gestão e orquestração de replicação, ativação pós-falha e reativação pós-falha de máquinas no local e máquinas virtuais (VMs) do Azure.

Este tutorial mostra como configurar a recuperação após desastre para uma região secundária do Azure para as VMs do Azure. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar um cofre dos Serviços de Recuperação 
> * Verificar as definições dos recursos de destino
> * Configurar o acesso de saída das VMs
> * Ativar replicação para uma VM


## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial:

- Certifique-se de que compreende a [arquitetura e os componentes do cenário](concepts-azure-to-azure-architecture.md).
- Reveja os [requisitos de suporte](site-recovery-support-matrix-azure-to-azure.md) de todos os componentes.

## <a name="create-a-vault"></a>Criar um cofre

Crie o cofre em qualquer região, exceto na região de origem.

1. Inicie sessão no [portal do Azure](https://portal.azure.com) > **Serviços de Recuperação**.
2. Clique em **Criar um recurso** > **Monitorização e Gestão** > **Backup e Site Recovery**.
3. Em **Nome**, especifique um nome amigável para identificar o cofre. Se tiver mais do que uma subscrição, selecione a que for adequada.
4. Crie um grupo de recursos ou selecione um existente. Selecione uma região do Azure. Para verificar as regiões suportadas, veja a disponibilidade geográfica em [Detalhes dos Preços do Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
5. Para aceder rapidamente ao cofre a partir do dashboard, clique em **Afixar ao dashboard** e clique em **Criar**.

   ![Novo cofre](./media/azure-to-azure-tutorial-enable-replication/new-vault-settings.png)

   O cofre novo é adicionado ao **Dashboard** em **Todos os recursos** e na página principal **Cofres dos Serviços de Recuperação**.

## <a name="verify-target-resources"></a>Verificar recursos de destino

1. Certifique-se de que a sua subscrição do Azure permite criar VMs na região de destino utilizada para recuperação após desastre. Contacte o suporte para ativar a quota necessária.

2. Certifique-se de que a sua subscrição tem recursos suficientes para suportar as VMs com tamanhos que correspondem às VMs de origem. O Site Recovery escolhe o mesmo tamanho ou o tamanho mais próximo possível da VM de destino.

## <a name="configure-outbound-network-connectivity"></a>Configurar a conectividade de rede de saída

Para o Site Recovery funcionar conforme esperado, terá de efetuar algumas alterações na conectividade de rede de saída, das VMs que pretende replicar.

- O Site Recovery não suporta a utilização de um proxy de autenticação para controlar a conectividade de rede.
- Se tiver um proxy de autenticação, não é possível ativar a replicação.

### <a name="outbound-connectivity-for-urls"></a>Conectividade de saída para URLs

Se estiver a utilizar um proxy de firewall baseado em URL para controlar a conectividade de saída, permita o acesso aos seguintes URLs utilizados pelo Site Recovery.

| **URL** | **Detalhes** |
| ------- | ----------- |
| *.blob.core.windows.net | Permite que os dados sejam escritos da VM para a conta de armazenamento em cache na região de origem. |
| login.microsoftonline.com | Fornece autorização e autenticação para os URLs do serviço Site Recovery. |
| *.hypervrecoverymanager.windowsazure.com | Permite que a VM comunique com o serviço Site Recovery. |
| *.servicebus.windows.net | Permite que a VM escreva dados de monitorização e diagnóstico do Site Recovery. |

### <a name="outbound-connectivity-for-ip-address-ranges"></a>Conectividade de saída para intervalos de endereços IP

Se quiser controlar a conectividade de saída com endereços IP em vez de URLs, adicione os intervalos do datacenter adequados, os endereços do Office 365 e os endereços de ponto final de serviço à lista de permissões para as firewalls baseadas em IP, o proxy ou as regras do NSG.

  - [Intervalos IP do Datacenter do Microsoft Azure](http://www.microsoft.com/en-us/download/details.aspx?id=41653)
  - [Intervalos de IP do Datacenter do Windows Azure na Alemanha](http://www.microsoft.com/en-us/download/details.aspx?id=54770)
  - [Intervalos de IP do Datacenter do Windows Azure na China](http://www.microsoft.com/en-us/download/details.aspx?id=42064)
  - [Intervalos de endereços IP e URLs do Office 365](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity)
  - [Endereços IP do ponto final do serviço Site Recovery](https://aka.ms/site-recovery-public-ips)

Pode utilizar este [script](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702) para criar as regras do NSG necessárias.

## <a name="verify-azure-vm-certificates"></a>Verificar os certificados de VM do Azure

Verifique se todos os certificados de raiz mais recentes estão presentes nas VMs do Windows ou Linux que pretende replicar. Se os certificados de raiz mais recentes não estiverem presentes, não será possível registar a VM no Site Recovery, devido a restrições de segurança.

- Para VMs do Windows, instale todas as atualizações mais recentes do Windows na VM, para que todos os certificados de raiz fidedigna fiquem na máquina. Num ambiente desligado, siga o padrão do Windows Update e os processos de atualização de certificados da sua organização.

- Para VMs do Linux, siga as orientações fornecidas pelo distribuidor do Linux, para obter os certificados de raiz fidedigna mais recentes e a lista de revogação de certificados na VM.

## <a name="set-permissions-on-the-account"></a>Definir permissões na conta

O Azure Site Recovery fornece três funções incorporadas para controlar as operações de gestão do Site Recovery.

- **Contribuinte do Site Recovery** - esta função tem todas as permissões necessárias para gerir as operações do Azure Site Recovery num cofre dos Serviços de Recuperação. No entanto, um utilizador com esta função não consegue criar nem eliminar um cofre dos Serviços de Recuperação, nem atribuir direitos de acesso a outros utilizadores. Esta função é mais adequada para os administradores de recuperação após desastre que podem ativar e gerir a recuperação após desastre para aplicações ou organizações completas.

- **Operador do Site Recovery** - esta função tem permissões para executar e gerir operações de Ativação Pós-falha e Reativação Pós-falha. Um utilizador com esta função não consegue ativar nem desativar a replicação, criar ou eliminar cofres, registar uma nova infraestrutura ou atribuir direitos de acesso a outros utilizadores. Esta função é mais adequada para um operador de recuperação após desastre que pode efetuar a ativação pós-falha de máquinas virtuais ou aplicações quando indicado pelos proprietários da aplicação e administradores de TI. Após a resolução do desastre, o operador de DR pode voltar a proteger e efetuar a reativação pós-falha das máquinas virtuais.

- **Leitor do Site Recovery** - esta função tem permissões para ver todas as operações de gestão do Site Recovery. Esta função é mais adequada para um executivo de monitorização de TI que pode monitorizar o estado atual da proteção e emitir pedidos de suporte.

Obter mais informações sobre [Funções incorporadas do RBAC do Azure](../role-based-access-control/built-in-roles.md)

## <a name="enable-replication"></a>Ativar a replicação

### <a name="select-the-source"></a>Selecionar a origem

1. Nos cofres dos Serviços de Recuperação, clique no nome do cofre > **+Replicar**.
2. Em **Origem**, selecione **Azure**.
3. Em **Localização de origem**, selecione a região do Azure de origem onde as VMs estão atualmente a ser executadas.
4. Selecione o **modelo de implementação da máquina virtual do Azure** para VMs: **Resource Manager** ou **Clássico**.
5. Selecione a **Subscrição de origem** onde as máquinas virtuais são executadas. Isto pode ser qualquer subscrição com o mesmo inquilino do Azure Active Directory onde existe o cofre de serviços de recuperação.
6. Selecione o **Grupo de recursos de origem** para as VMs do Resource Manager ou o **serviço cloud** para as VMs clássicas.
7. Clique em **OK** para guardar as definições.

### <a name="select-the-vms"></a>Selecionar as VMs

O Site Recovery obtém uma lista das VMs associadas à subscrição e o grupo de recursos/serviço cloud.

1. Em **Máquinas Virtuais**, selecione as VMs que pretende replicar.
2. Clique em **OK**.

### <a name="configure-replication-settings"></a>Configurar as definições de replicação

O Site Recovery cria as predefinições e a política de replicação para a região de destino. Pode alterar as definições com base nos seus requisitos.

1. Clique em **Definições** para ver as definições de destino e replicação.
2. Para substituir as predefinições de destino, clique em **Personalizar** junto a **Grupo de recursos, Rede, Armazenamento e Conjuntos de Disponibilidade**.

  ![Configurar definições](./media/azure-to-azure-tutorial-enable-replication/settings.png)


- **Subscrição de destino**: A subscrição de destino utilizada para recuperação após desastre. Por predefinição, a subscrição de destino será igual à subscrição de origem. Clique em "Personalizar" para selecionar uma subscrição de destino diferente no mesmo inquilino do Azure Active Directory.

- **Localização de destino**: a região de destino utilizada para recuperação após desastre. Recomendamos que a localização de destino corresponda à localização do cofre do Site Recovery.

- **Grupo de recursos de destino**: o grupo de recursos na região de destino que contém as VMs do Azure após a ativação pós-falha. Por predefinição, o Site Recovery cria um novo grupo de recursos na região de destino com um sufixo "asr". A localização do grupo de recursos do grupo de recursos de destino pode ser qualquer região, exceto a região onde estão alojadas as máquinas virtuais de origem.

- **Rede virtual de destino**: a rede na região de destino onde estão localizadas as VMs após a ativação pós-falha.
  Por predefinição, o Site Recovery cria uma nova rede virtual (e sub-redes) na região de destino com um sufixo "asr".

- **Contas de armazenamento em cache**: o Site Recovery utiliza uma conta de armazenamento na região de origem. As alterações às VMs de origem são enviadas para esta conta antes da replicação para a localização de destino.

- **Contas de armazenamento de destino (se a VM de origem não utilizar discos geridos)**: por predefinição, o Site Recovery cria uma nova conta de armazenamento na região de destino para espelhar a conta de armazenamento das VMs de origem.

- **Discos geridos de réplica (se a VM de origem utilizar discos geridos)**: por predefinição, o Site Recovery cria discos geridos de réplica na região de destino para espelhar os discos geridos da VM de origem com o mesmo tipo de armazenamento (Standard ou Premium) do disco gerido da VM de origem.

- **Conjuntos de disponibilidade de destino**: por predefinição, o Site Recovery cria um novo conjunto de disponibilidade na região de destino com o sufixo "asr". Só é possível adicionar conjuntos de disponibilidade se as VMs fizerem parte de um conjunto na região de origem.

Para substituir as predefinições da política de replicação, clique em **Personalizar** junto a **Política de replicação**.  

- **Nome da política de replicação**: o nome da política.

- **Retenção de pontos de recuperação**: por predefinição, o Site Recovery mantém os pontos de recuperação durante 24 horas. Pode configurar um valor entre 1 e 72 horas.

- **Frequência de instantâneos consistentes com a aplicação**: por predefinição, o Site Recovery tira um instantâneo consistente da aplicação a cada 4 horas. Pode configurar qualquer valor entre 1 e 12 horas. Um instantâneo consistente com a aplicação é um instantâneo de ponto no tempo dos dados de aplicação dentro da VM. O Serviço de Cópia Sombra de Volumes (VSS) garante que a aplicação está num estado consistente quando se obtém o instantâneo.

- **Grupo de replicação**: se a sua aplicação precisar de consistência multi-VM entre VMs, pode criar um grupo de replicação para essas VMs. Por predefinição, as VMs selecionadas não fazem parte de qualquer grupo de replicação.

  Clique em **Personalizar** junto a **Política de replicação** e, em seguida, selecione **Sim** para que a consistência multi-VM faça parte de um grupo de replicação. Pode criar um novo grupo de replicação ou utilizar um grupo de replicação existente. Selecione as VMs para pertencerem ao grupo de replicação e clique em **OK**.

> [!IMPORTANT]
  Todas as máquinas num grupo de replicação irão partilhar pontos de recuperação consistentes com a aplicação e com falhas aquando da ativação pós-falha. Ativar a consistência multi-VM pode afetar o desempenho da carga de trabalho, e deve ser utilizado apenas se as máquinas estiverem a executar a mesma carga de trabalho e precisar de consistência entre várias máquinas.

> [!IMPORTANT]
  Se ativar a consistência multi-VM, as máquinas no grupo de replicação comunicam entre si pela porta 20004. Certifique-se de que não há nenhum dispositivo de firewall a bloquear a comunicação interna entre as VMs através da porta 20004. Se pretender que as VMs do Linux pertençam a um grupo de replicação, certifique-se de que o tráfego de saída na porta 20004 é aberto manualmente de acordo com as orientações da versão específica do Linux.

### <a name="configure-encryption-settings"></a>Configurar as definições de encriptação

Se a máquina virtual de origem tiver o Azure Disk Encryption (ADE) ativado, será apresentada a secção de definições de encriptação abaixo.

- **Cofres de chaves da encriptação do disco**: por predefinição, o Azure Site Recovery cria um novo cofre de chaves na região de destino com o sufixo “asr” no nome, com base nas chaves de encriptação do disco da VM de origem. Caso o cofre de chaves criado pelo Azure Site Recovery já exista, será reutilizado.
- **Cofres de chaves da encriptação de chaves**: por predefinição, o Azure Site Recovery cria um novo cofre de chaves na região de destino com o sufixo “asr” no nome, com base nas chaves de encriptação da chave da VM de origem. Caso o cofre de chaves criado pelo Azure Site Recovery já exista, será reutilizado.

Clique em “Personalizar”, junto às definições de encriptação, para substituir as predefinições e selecionar os cofres de chaves personalizados.

>[!NOTE]
>Apenas as VMs do Azure com o SO Windows e [ativadas para a encriptação com a aplicação do Azure AD](https://aka.ms/ade-aad-app) são atualmente suportadas pelo Azure Site Recovery.
>

### <a name="track-replication-status"></a>Controlar o estado de replicação

1. Em **Definições**, clique em **Atualizar** para obter o estado mais recente.

2. Pode controlar o progresso da tarefa **Ativar proteção** em **Definições** > **Tarefas** > **Tarefas do Site Recovery**.

3. Em **Definições** > **Itens Replicados**, pode ver o estado das VMs e o progresso da replicação inicial. Clique na VM para desagregar as respetivas definições.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, configurou a recuperação após desastre para uma VM do Azure. O passo seguinte consiste em testar a configuração.

> [!div class="nextstepaction"]
> [Executar um teste de recuperação após desastre](azure-to-azure-tutorial-dr-drill.md)
