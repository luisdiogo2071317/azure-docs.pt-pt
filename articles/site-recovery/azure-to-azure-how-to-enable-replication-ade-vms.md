---
title: Configurar a replicação para a encriptação de disco do Azure (ADE) ativada VMs no Azure Site Recovery | Documentos da Microsoft
description: Este artigo descreve como configurar a replicação de VMs de ADE, de uma região do Azure para outra com o Site Recovery.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 09/28/2018
ms.author: sutalasi
ms.openlocfilehash: 5d1beb124bbb857d13aecad7bf0cef493d42dac5
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/02/2018
ms.locfileid: "48043282"
---
# <a name="replicate-azure-disk-encryption-ade-enabled-virtual-machines-to-another-azure-region"></a>Replicar máquinas de virtuais do Azure disk encryption (ADE) ativada para outra região do Azure

Este artigo descreve como ativar VMs de replicação de encriptação de disco do Azure (ADE) ativada, de uma região do Azure para outra.

>[!NOTE]
>Apenas as VMs do Azure que executa o SO Windows e [ativada para a encriptação com a aplicação do Azure AD](https://aka.ms/ade-aad-app) são atualmente suportadas pelo Azure Site Recovery.
>

## <a name="required-user-permissions"></a>Permissões de utilizador necessário

Para ativar a replicação de VMs de ADE a partir do portal, o utilizador deve ter o abaixo permissões.
- Permissões do Cofre de chaves
    - lista
    - Criar
    - Get

-   Permissões de segredos do Cofre de chaves
    - Lista
    - Criar
    - Get

- Permissões de chave de Cofre de chaves (necessárias apenas se as VMs utilizam a chave de encriptação de chave para criptografar chaves de encriptação de disco)
    - Lista
    - Get
    - Criar
    - Encriptar
    - Desencriptar

Pode gerir as permissões ao navegar para o recurso do Cofre de chaves no portal e adicionar as permissões necessárias para o usuário.

![keyvaultpermissions](./media/azure-to-azure-how-to-enable-replication-ade-vms/keyvaultpermissions.png)

Se o utilizador ativar a recuperação após desastre (DR) não tem as permissões necessárias para copiar as chaves, o script abaixo puder ser dado ao administrador de segurança com as permissões adequadas para copiar as chaves e segredos de encriptação para a região de destino.

>[!NOTE]
>Para ativar a replicação de VM de ADE partir do portal, tem, pelo menos, permissões de "List" em cofres de chaves, segredos e chaves
>

## <a name="copy-ade-keys-to-dr-region-using-powershell-script"></a>Copiar chaves de ADE a região de DR com o Script do PowerShell

1. Abra o código de script não processados "CopyKeys" numa janela do browser ao clicar em [esta ligação](https://aka.ms/ade-asr-copy-keys-code).
2. Copie o script para um ficheiro e chamá-lo de "cópia-keys.ps1'.
2. Abra a aplicação do Windows PowerShell e vá para a localização da pasta onde o ficheiro existe.
3. Iniciar ' cópia-keys.ps1'
4. Forneça as credenciais de início de sessão do Azure.
5. Selecione o **subscrição do Azure** das suas VMs.
6. Aguarde que os grupos de recursos carregar e, em seguida, selecione o **grupo de recursos** das suas VMs.
7. Selecione as VMs da lista de VMs apresentado. Apenas as VMs ativadas com encriptação de disco do Azure são apresentadas na lista.
8. Selecione o **localização de destino**.
9. **Cofres de chaves de encriptação de disco**: por predefinição, do Azure Site Recovery cria um novo cofre de chaves na região de destino com um nome que o sufixo "asr" com base nas chaves de encriptação de disco da VM de origem. No caso de Cofre de chaves criado pelo Azure Site Recovery já existe, este é reutilizado. Pode selecionar um cofre de chaves diferente na lista, se necessário.
10. **Cofres de chaves de encriptação da chave**: por predefinição, do Azure Site Recovery cria um novo cofre de chaves na região de destino com um nome que o sufixo "asr" com base nas chaves de encriptação de chave de VM de origem. No caso de Cofre de chaves criado pelo Azure Site Recovery já existe, este é reutilizado. Pode selecionar um cofre de chaves diferente na lista, se necessário.

## <a name="enable-replication"></a>Ativar a replicação

Este procedimento assume que a região primária do Azure é a Ásia Oriental e a região secundária é Sudeste asiático.

1. No cofre, clique em **+ replicar**.
2. Tenha em atenção os seguintes campos:
    - **Origem**: O ponto de origem das VMs, que neste caso, é **Azure**.
    - **Localização de origem**: A região do Azure a partir de onde pretende proteger as suas máquinas virtuais. Nesta ilustração, a localização de origem é 'Ásia Oriental'
    - **Modelo de implementação**: modelo de implementação do Azure, máquinas de origem.
    - **Subscrição de origem**: A subscrição ao qual pertencem a suas máquinas de virtuais de origem. Isto pode ser qualquer subscrição com o mesmo inquilino do Azure Active Directory onde existe o cofre de serviços de recuperação.
    - **Grupo de recursos**: O grupo de recursos ao qual pertencem a suas máquinas de virtuais de origem. Todas as VMs no grupo de recursos selecionada são listadas para proteção no próximo passo.

3. Na **máquinas virtuais > Selecione as máquinas virtuais**, clique e selecione cada VM que pretende replicar. Só pode selecionar máquinas para as quais a replicação pode ser ativada. Em seguida, clique em **OK**.

4. Na **definições**, opcionalmente, pode configurar as definições de site de destino:

    - **Localização de destino**: A localização onde os dados de máquina virtual de origem serão replicados. Consoante a localização de máquinas selecionadas, Site Recovery fornece a lista de regiões de destino adequado. Recomendamos que mantenha a localização de destino a mesma da localização do Cofre de serviços de recuperação.
    - **Subscrição de destino**: A subscrição de destino utilizada para recuperação após desastre. Por predefinição, a subscrição de destino será igual à subscrição de origem.
    - **Grupo de recursos de destino**: O grupo de recursos para que todos os seus máquinas virtuais replicadas pertence. Por predefinição o Azure Site Recovery cria um novo grupo de recursos na região de destino com um nome que o sufixo "asr". No caso de grupo de recursos criado pelo Azure Site Recovery já existe, este é reutilizado. Também é possível personalizá-lo, conforme mostrado na secção abaixo. A localização do grupo de recursos de destino pode ser qualquer região do Azure, exceto a região na qual as máquinas de virtuais de origem estão alojadas.
    - **Rede Virtual de destino**: por predefinição, Site Recovery cria uma nova rede virtual na região de destino com um nome que o sufixo "asr". Este é mapeado para a sua rede de origem e utilizado para qualquer proteção futura. [Saiba mais](site-recovery-network-mapping-azure-to-azure.md) sobre mapeamento de rede.
    - **Contas de armazenamento de destino (se a sua origem de VM não utiliza discos geridos)**: por predefinição, o Site Recovery cria uma nova conta de armazenamento de destino imitando a configuração de armazenamento VM de origem. No caso de já existe uma conta de armazenamento, este é reutilizado.
    - **Discos de geridos de réplica (se a VM de origem utilizar discos geridos)**: o Site Recovery cria novos discos geridos de réplica na região de destino para espelhar os discos geridos da VM de origem com o mesmo tipo de armazenamento (Standard ou premium), como a VM de origem de disco gerido.
    - **Contas de armazenamento em cache**: Site Recovery precisa da conta de armazenamento extra chamada de armazenamento de cache na região de origem. Todas as alterações acontecendo nas VMs de origem são controladas e enviadas para a conta de armazenamento de cache antes de replicar os para a localização de destino.
    - **Conjunto de disponibilidade**: por predefinição, o Azure Site Recovery cria uma novo conjunto de disponibilidade na região de destino com um nome que o sufixo "asr". No caso de conjunto de disponibilidade criado pelo Azure Site Recovery já existe, este é reutilizado.
    - **Cofres de chaves de encriptação de disco**: por predefinição, do Azure Site Recovery cria um novo cofre de chaves na região de destino com um nome que o sufixo "asr" com base nas chaves de encriptação de disco da VM de origem. No caso de Cofre de chaves criado pelo Azure Site Recovery já existe, este é reutilizado.
    - **Cofres de chaves de encriptação da chave**: por predefinição, do Azure Site Recovery cria um novo cofre de chaves na região de destino com um nome que o sufixo "asr" com base nas chaves de encriptação de chave de VM de origem. No caso de Cofre de chaves criado pelo Azure Site Recovery já existe, este é reutilizado.
    - **Política de replicação**: define as definições de recuperação ponto retenção histórico e aplicação de frequência de instantâneo consistente. Por predefinição, o Azure Site Recovery cria uma nova política de replicação com as predefinições dos ' 24 horas para retenção do ponto de recuperação e "60 minutos para a frequência de instantâneo consistente da aplicação.



## <a name="customize-target-resources"></a>Personalizar os recursos de destino

Pode modificar as predefinições de destino utilizadas pelo Site Recovery.


1. Clique em **personalizar:** junto a "Subscrição de destino" para modificar a subscrição de destino predefinido. Selecione a subscrição da lista de todas as subscrições disponíveis no mesmo inquilino do Azure Active Directory (AAD).

2. Clique em **personalizar:** junto a "grupo de recursos, armazenamento, rede e disponibilidade define como para modificar o abaixo as predefinições:
    - Na **grupo de recursos de destino**, selecione o grupo de recursos na lista de todos os grupos de recursos na localização de destino da subscrição.
    - Na **rede virtual de destino**, selecione a rede de uma lista de todos os a rede virtual na localização de destino.
    - Na **conjunto de disponibilidade**, pode adicionar as definições de conjunto de disponibilidade para a VM, se eles são parte de um conjunto de disponibilidade na região de origem.
    - Na **contas de armazenamento de destino**, selecione a conta que pretende utilizar.


2. Clique em **personalizar:** junto a "Definições de encriptação" para modificar o abaixo as predefinições:
    - Na **Cofre de chaves de encriptação de disco de destino**, selecione o Cofre de chaves de encriptação de disco destino na lista de todos os cofres de chaves na localização de destino da subscrição.
  - Na **Cofre de chaves de encriptação de chave de destino**, selecione o Cofre de chaves de encriptação de chave de destino na lista de todos os cofres de chaves na localização de destino da subscrição.

3. Clique em **criar o recurso de destino** > **ativar replicação**.
4. Depois das VMs estiverem ativadas para replicação, pode verificar o estado do Estado de funcionamento da VM em **itens replicados**

>[!NOTE]
>Durante a replicação inicial, o estado pode demorar algum tempo para atualizar, sem o progresso. Clique nas **atualizar** botão, para obter o estado mais recente.
>

## <a name="update-target-vm-encryption-settings"></a>Atualizar definições de encriptação de VM de destino
Na abaixo cenários, será necessário para atualizar as definições de encriptação da VM de destino.
  - Ativar a replicação de recuperação de Site na VM e ativada a encriptação de disco do Azure (ADE) na VM de origem numa data posterior
  - Ativar a replicação de recuperação de Site na VM e alterar a chave de encriptação de disco e/ou a chave de encriptação de chave na VM de origem numa data posterior

Pode usar [o script](#copy-ade-keys-to-dr-region-using-powershell-script) para copiar as chaves de encriptação para a região de destino e, em seguida, Atualize as definições de encriptação de destino no **-> de Cofre de serviços de recuperação de item replicado -> propriedades -> computação e rede.**

![ade-definições de atualização](./media/azure-to-azure-how-to-enable-replication-ade-vms/update-ade-settings.png)

## <a name="next-steps"></a>Passos Seguintes

[Saiba mais](site-recovery-test-failover-to-azure.md) sobre como executar uma ativação pós-falha de teste.
