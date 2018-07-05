---
title: Criar cópias de segurança e restaurar VMs encriptadas com a cópia de segurança do Azure
description: Este artigo fala sobre a experiência de cópia de segurança e restauro para VMs encriptadas com o Azure Disk Encryption.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 10/13/2017
ms.author: sogup
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 49361aef774e9eb5a0995bc106e73b236a71b0bb
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37441136"
---
# <a name="back-up-and-restore-encrypted-virtual-machines-with-azure-backup"></a>Criar cópias de segurança e restaurar máquinas virtuais encriptadas com o Azure Backup
Este artigo fala sobre os passos para criar cópias de segurança e restaurar máquinas virtuais (VMs) com o Azure Backup. Também fornece detalhes sobre os cenários suportados, pré-requisitos e passos de resolução de problemas para casos de erro.

## <a name="supported-scenarios"></a>Cenários suportados

 * Cópia de segurança e restauro de VMs encriptadas só é suportada para VMs que utilizam o modelo de implementação Azure Resource Manager. Não é suportada para VMs que utilizam o modelo de implementação clássica. <br>
 * Cópia de segurança e restauro de VMs encriptadas é suportada para VMs de Linux que utilizam o Azure Disk Encryption e Windows. Encriptação de disco utiliza a funcionalidade de BitLocker padrão da indústria do Windows e a funcionalidade de dm-crypt do Linux para fornecer encriptação de discos. <br>
 
 A tabela seguinte mostra os cenários suportados para a chave de encriptação do BitLocker (BEK) - chave de encriptação apenas e chaves (KEK) - encriptados VMs:
 
 
   |  | BEK + KEK VMs | VMs só BEK |
   | --- | --- | --- |
   | **Em VMs**  | Sim | Sim  |
   | **VMs geridas**  | Sim | Sim  |

## <a name="prerequisites"></a>Pré-requisitos
* A VM foi encriptada utilizando [do Azure Disk Encryption](../security/azure-security-disk-encryption.md).

* Foi criado um cofre dos serviços de recuperação e a replicação de armazenamento foi definida ao seguir os passos em [preparar o ambiente para cópia de segurança](backup-azure-arm-vms-prepare.md).

* Cópia de segurança foi indicada [permissões para aceder um cofre de chaves](#provide-permissions-to-backup) que contém as chaves e segredos para VMs encriptadas.

## <a name="backup-encrypted-vm"></a>VM encriptada de cópia de segurança
Utilize os seguintes passos para definir um objetivo de cópia de segurança, definir uma política, configurar os itens e acionar uma cópia de segurança.

### <a name="configure-backup"></a>Configurar a cópia de segurança
1. Se já tiver um cofre de serviços de recuperação aberto, avance para o passo seguinte. Se não tiver um cofre de serviços de recuperação aberto, mas está no portal do Azure, selecione **todos os serviços**.

   a. Na lista de recursos, escreva **Serviços de Recuperação**.

   b. À medida que começa a escrever, a lista filtra com base na sua entrada. Quando vir **cofres dos serviços de recuperação**, selecioná-lo.

      ![Cofre dos Serviços de Recuperação](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

    c. É apresentada a lista dos cofres dos Serviços de Recuperação. Selecione um cofre da lista.

     O dashboard do cofre selecionado é aberto.
2. Na lista de itens que é apresentado no cofre, selecione **cópia de segurança** para começar a fazer backup da VM encriptada.

      ![Painel cópia de segurança](./media/backup-azure-vms-encryption/select-backup.png)
3. Sobre o **cópia de segurança** mosaico, selecione **objetivo de cópia de segurança**.

      ![Painel cenário](./media/backup-azure-vms-encryption/select-backup-goal-one.png)
4. Sob **em que a sua carga de trabalho é executado?**, selecione **Azure**. Sob **o que fazer quiser a cópia de segurança?**, selecione **Máquina Virtual**. Em seguida, selecione **OK**.

   ![Abrir o painel Cenário](./media/backup-azure-vms-encryption/select-backup-goal-two.png)
5. Sob **escolher política de cópia de segurança**, selecione a política de cópia de segurança que pretende aplicar ao cofre. Em seguida, selecione **OK**.

      ![Selecionar política de cópia de segurança](./media/backup-azure-vms-encryption/setting-rs-backup-policy-new.png)

    Os detalhes da política predefinida são listados. Se quiser criar uma política, selecione **criar novo** na lista pendente. Depois de selecionar **OK**, a política de cópia de segurança é associada ao cofre.

6. Escolha as VMs encriptadas a associar à política especificada e selecione **OK**.

      ![Selecione as VMs encriptadas](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)
7. Esta página mostra uma mensagem sobre cofres de chaves associado para as VMs encriptadas que selecionou. Cópia de segurança requer acesso só de leitura para as chaves e segredos no Cofre de chaves. Utiliza estas permissões para criar cópias de segurança as chaves e segredos, juntamente com as VMs associadas.<br>
Se for um **utilizador de membro**, processo de ativar cópia de segurança será perfeitamente adquirir acesso ao Cofre de chaves para cópia de segurança VMs encriptadas sem exigir qualquer intervenção do utilizador.

   ![Mensagem de VMs encriptada](./media/backup-azure-vms-encryption/member-user-encrypted-vm-warning-message.png)

   Para uma **utilizador convidado**, tem de fornecer permissões para o serviço de cópia de segurança para aceder ao Cofre de chaves para cópias de segurança trabalhar. Pode fornecer estas permissões ao seguir o [passos mencionados na secção seguinte](#provide-permissions-to-backup)

   ![Mensagem de VMs encriptada](./media/backup-azure-vms-encryption/guest-user-encrypted-vm-warning-message.png)
 
    Agora que definiu todas as definições do cofre, selecione **ativar cópia de segurança** na parte inferior da página. **Ativar cópia de segurança** implementa a política no cofre e as VMs.
  
8. A próxima fase no processo de preparação está a instalar o agente da VM ou certificar-se de que o agente da VM está instalado. Para fazer o mesmo, siga os passos em [preparar o ambiente para cópia de segurança](backup-azure-arm-vms-prepare.md).

### <a name="trigger-a-backup-job"></a>Acionar uma tarefa de cópia de segurança
Siga os passos em [VMs de cópia de segurança do Azure para um cofre dos serviços de recuperação](backup-azure-arm-vms.md) para acionar uma tarefa de cópia de segurança.

### <a name="continue-backups-of-already-backed-up-vms-with-encryption-enabled"></a>Continuar as cópias de segurança de VMs de cópia de segurança já com encriptação ativada  
Se tiver VMs já a cópia de segurança num cofre dos serviços de recuperação que estão ativadas para a encriptação mais tarde, tem de dar permissões para cópia de segurança para aceder ao Cofre de chave para cópias de segurança continuar. Pode fornecer estas permissões ao seguir a [os passos na secção seguinte](#provide-permissions-to-azure-backup). Ou pode seguir os passos de PowerShell na secção "Ativar cópia de segurança" a [documentação do PowerShell](backup-azure-vms-automation.md). 

## <a name="provide-permissions-to-backup"></a>Fornecer permissões para cópia de segurança
Utilize os seguintes passos para fornecer permissões relevantes à cópia de segurança para aceder ao Cofre de chaves e executar a cópia de segurança de VMs encriptadas.
1. Selecione **todos os serviços**e procure **cofres de chaves**.

    ![Cofres de chaves](./media/backup-azure-vms-encryption/search-key-vault.png)
    
2. Na lista de cofres de chaves, selecione o Cofre de chaves associado à VM encriptada que tem de ser efetuada a cópia de segurança.

     ![Seleção de Cofre de chaves](./media/backup-azure-vms-encryption/select-key-vault.png)
     
3. Selecione **políticas de acesso**e, em seguida, selecione **adicionar novo**.

    ![Adicionar nova](./media/backup-azure-vms-encryption/select-key-vault-access-policy.png)
    
4. Selecione **selecionar principal**e, em seguida, escreva **serviço de gestão de cópia de segurança** na caixa de pesquisa. 

    ![Pesquisa de serviço de cópia de segurança](./media/backup-azure-vms-encryption/search-backup-service.png)
    
5. Selecione **serviço de gestão de cópia de segurança**e, em seguida, selecione **selecione**.

    ![Seleção de serviço de cópia de segurança](./media/backup-azure-vms-encryption/select-backup-service.png)
    
6. Sob **configurar a partir de modelo (opcional)**, selecione **cópia de segurança do Azure**. As permissões necessárias são prefilled para **permissões da chave** e **permissões secretas**. Se a VM é encriptada utilizando **BEK só**, apenas para segredos são necessárias permissões de, pelo que tem de remover a seleção para **permissões da chave**.

    ![Seleção de cópia de segurança do Azure](./media/backup-azure-vms-encryption/select-backup-template.png)
    
7. Selecione **OK**. Tenha em atenção que **serviço de gestão de cópia de segurança** é adicionado na **políticas de acesso**. 

    ![Políticas de acesso](./media/backup-azure-vms-encryption/backup-service-access-policy.png)
    
8. Selecione **guardar** para conceder as permissões necessárias para a cópia de segurança.

    ![Política de cópia de segurança de acesso](./media/backup-azure-vms-encryption/save-access-policy.png)

Depois de permissões com êxito são fornecidas, pode avançar com a ativação de cópia de segurança para VMs encriptadas.

## <a name="restore-an-encrypted-vm"></a>Restaurar uma VM encriptada
Para restaurar uma VM encriptada, restaurar discos ao seguir os passos na secção "Restaurar discos de cópia de segurança" na [escolha uma configuração de restauro de VM](backup-azure-arm-restore-vms.md#choose-a-vm-restore-configuration). Depois disso, pode utilizar uma das seguintes opções:

* Siga os passos no PowerShell [criar uma VM a partir de discos restaurados](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) para criar uma VM completa a partir de discos restaurados.
* Ou, [usar modelos para personalizar uma VM restaurada](backup-azure-arm-restore-vms.md#use-templates-to-customize-a-restored-vm) para criar VMs a partir de discos restaurados. Os modelos podem ser utilizados apenas para pontos de recuperação criados depois de 26 de Abril de 2017.

## <a name="troubleshooting-errors"></a>Resolução de erros
| Operação | Detalhes do erro | Resolução |
| --- | --- | --- |
|Cópia de segurança | Cópia de segurança não tem permissões suficientes para o Cofre de chaves para cópia de segurança de VMs encriptadas. | Cópia de segurança deve ser fornecida estas permissões ao seguir a [os passos na secção anterior](#provide-permissions-to-azure-backup). Ou pode seguir os passos de PowerShell na secção "Ativar a proteção" da documentação do PowerShell no [Recoveryservices de utilização de cmdlets para fazer uma cópia de segurança de máquinas virtuais](backup-azure-vms-automation.md#back-up-azure-vms). |  
| Restauro |Não é possível restaurar esta VM encriptada porque não existe o Cofre de chaves associado a esta VM. |Criar um cofre de chaves utilizando [introdução ao Azure Key Vault](../key-vault/key-vault-get-started.md). Ver [restaurar uma chave de Cofre de chaves e um segredo com o Azure Backup](backup-azure-restore-key-secret.md) para restaurar uma chave e um segredo, se eles não estão presentes. |
| Restauro |Não é possível restaurar esta VM encriptada porque a chave e o segredo associado esta VM não existem. |Ver [restaurar uma chave de Cofre de chaves e um segredo com o Azure Backup](backup-azure-restore-key-secret.md) para restaurar uma chave e um segredo, se eles não estão presentes. |
| Restauro |Cópia de segurança não tem autorização para aceder aos recursos na sua subscrição. |Restaurar, conforme mencionado anteriormente, discos primeiro ao seguir os passos na secção "Restaurar discos de cópia de segurança" na [escolha uma configuração de restauro de VM](backup-azure-arm-restore-vms.md#choose-a-vm-restore-configuration). Depois disso, utilize o PowerShell para [criar uma VM a partir de discos restaurados](backup-azure-vms-automation.md#create-a-vm-from-restored-disks). |
