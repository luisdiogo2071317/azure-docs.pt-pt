---
title: Atualizar para a V2 de pilha de cópia de segurança de VM do Azure
description: Atualizar o processo e FAQs para a pilha de cópia de segurança da VM, o modelo de implementação do Resource Manager
services: backup
author: trinadhk
manager: vijayts
tags: azure-resource-manager, virtual-machine-backup
ms.service: backup
ms.topic: conceptual
ms.date: 10/3/2018
ms.author: trinadhk
ms.openlocfilehash: adb6898d9b7f4c0272085828778096816489a18d
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2019
ms.locfileid: "54201797"
---
# <a name="get-improved-backup-and-restore-performance-with-azure-backup-instant-restore-capability"></a>Obter o melhor de cópia de segurança e restaurar o desempenho com capacidade de Azure cópia de segurança instantâneas restaurar

> [!NOTE]
> Com base nos comentários recebidos de utilizadores que o **pilha de cópia de segurança da VM V2** é confuso com o Azure stack, ter renomeamos para **restaurar instantâneas** fornecendo uma experiência atualizada e melhor.

> [!IMPORTANT]
> Depois de receber a resposta positiva de nossos usuários já tendo o desempenho atualizado, decidimos atualizar todos os nossos utilizadores a funcionalidade de restauro imediato. Para atualizar, não existe nenhuma ação necessária da sua parte. **A partir de Fevereiro de 2019**, começaremos a implementar esta funcionalidade região a região.
>
>

O novo modelo para restaurar instantâneas fornece os seguintes aprimoramentos de recursos:

* Capacidade de utilizar instantâneos tirados como parte de uma tarefa de cópia de segurança que está disponível para recuperação sem aguardar a transferência de dados para o cofre para concluir. Reduz o tempo de espera para instantâneos copiar para o cofre, antes de acionar o restauro.
* Reduz os tempos de backup e restauração, guardam instantâneos localmente, por dois dias por predefinição. Este cofre de padrão é configurável para qualquer valor entre 1 a 5 dias.
* Disco suporta tamanhos até 4 TB.
* Suporta discos de Standard SSD.
* Capacidade de utilizar contas de armazenamento originais uma VM não gerida (por disco), ao restaurar. Esta capacidade existe mesmo quando a VM tem discos que são distribuídos em contas de armazenamento. Ela acelera as operações de restauro para uma grande variedade de configurações de VM.


## <a name="whats-new-in-this-feature"></a>O que há de novo nesta funcionalidade

Atualmente, a tarefa de cópia de segurança consiste em duas fases:

1.  Criar um instantâneo VM.
2.  A transferência de um instantâneo VM para o Cofre dos serviços de recuperação do Azure.

Um ponto de recuperação é considerado criado apenas depois de fases 1 e 2 são concluídas. Como parte desta atualização, é criado um ponto de recuperação assim que o instantâneo estiver concluído e que este ponto de recuperação do tipo de instantâneo pode ser utilizado para efetuar um restauro com o mesmo fluxo de restauro. Pode identificar este ponto de recuperação no portal do Azure ao utilizar "instantâneo" como o tipo de ponto de recuperação e, depois do instantâneo é transferido para o cofre, o tipo de ponto de recuperação é alterado para "instantâneo e cofre."

![Tarefa de cópia de segurança no VM pilha de cópia de segurança do Resource Manager modelo de implementação, armazenamento e o Cofre](./media/backup-azure-vms/instant-rp-flow.png)

Por predefinição, os instantâneos são retidos durante dois dias. Esta funcionalidade permite restauros da operação estes instantâneos lá por diminuir os tempos de restauro. Reduz o tempo necessário para transformar e copiar dados do cofre para a conta de armazenamento do usuário para cenários de disco não gerido enquanto para utilizadores de disco gerido, cria discos geridos fora os dados de serviços de recuperação.

## <a name="feature-considerations"></a>Considerações sobre funcionalidades

* Os instantâneos são armazenados juntamente com os discos para aumentar a criação do ponto de recuperação e para acelerar as operações de restauro. Como resultado, verá os custos de armazenamento que correspondem aos instantâneos tirados durante este período.
* Instantâneos incrementais são armazenados como blobs de páginas. Todos os utilizadores com discos não geridos são cobrados para instantâneos armazenados na sua conta de armazenamento local. Uma vez que as coleções de ponto de restauro utilizadas pelas cópias de segurança de VM geridos utilizam instantâneos de blob ao nível do armazenamento subjacente, para discos geridos, verá os custos correspondentes aos preços de instantâneo de blob e são incrementais.
* Para contas de armazenamento premium, os instantâneos tirados para contagem de pontos de recuperação imediata para o limite de 10 TB de espaço em atribuído.
* Tem uma capacidade para configurar a retenção de instantâneo com base nas necessidades de restauro. Dependendo do requisito, pode definir o período de retenção do instantâneo para um mínimo de um dia no painel da política de cópia de segurança, conforme explicado abaixo. Isto pode ajudar a poupar no custo para a retenção de instantâneo.

> [!NOTE]
>
Com esta instantâneas de atualização, o período de retenção de instantâneo de todos os clientes de restauro **(novos e existentes incluídos)** será definido como um valor predefinido de dois dias. No entanto, pode definir a duração de acordo com seus requisitos para qualquer valor entre 1 a 5 dias.
>
>

## <a name="cost-impact"></a>Impacto de custo

Os instantâneos incrementais são armazenados na conta de armazenamento da VM, que são utilizados para a recuperação instantânea. Instantâneo incremental significa que o espaço ocupado por um instantâneo é igual para o espaço ocupado por páginas que são escritas depois do instantâneo foi criado. A faturação é ainda para o por GB ocupado por instantâneo e o preço por GB de espaço utilizado é a mesmo, conforme mencionado na [página de preços](https://azure.microsoft.com/pricing/details/managed-disks/).


## <a name="upgrading-to-instant-restore"></a>Atualizar para o restauro imediato
Se utilizar o portal do Azure, verá uma notificação no dashboard do cofre. Esta notificação se relaciona com suporte de disco grande e melhorias de velocidade de cópia de segurança e restauro. Como alternativa, pode aceder à página de propriedades do cofre para obter a opção de atualização.

![Tarefa de cópia de segurança no modelo de implementação de Gestor de recursos de pilha de cópia de segurança de VM – notificação de suporte](./media/backup-azure-vms/instant-rp-banner.png)

Para abrir uma tela para atualizar para o restaurar instantânea, selecione a faixa.

![Tarefa de cópia de segurança na pilha de cópia de segurança de VM modelo de implementação do Resource Manager – atualizar](./media/backup-azure-vms/instant-rp.png)

## <a name="upcoming-changes"></a>Alterações futuras

### <a name="configure-snapshot-retention-using-azure-portal"></a>Configurar a retenção de instantâneo com o portal do Azure

Para os usuários atualizados, no portal do Azure pode ver um campo adicionado no **política de cópia de segurança de VM** painel sob o **restaurar instantâneas** secção. Pode alterar o período de retenção de instantâneo do **política de cópia de segurança de VM** painel para todas as VMs associadas à política de cópia de segurança específica.


![Tarefa de cópia de segurança na implementação de restaurar instantânea](./media/backup-azure-vms/instant-rp-banner1.png)


## <a name="upgrade-to-instant-restore-using-powershell"></a>Atualize para restaurar instantâneas com o PowerShell

Se ainda não é automaticamente atualizados ainda e quiser Self-Service, em seguida, execute os seguintes cmdlets de um terminal do PowerShell elevada:

1.  Inicie sessão sua conta do Azure:

    ```
    PS C:> Connect-AzureRmAccount
    ```

2.  Selecione a subscrição que pretende registar:

    ```
    PS C:>  Get-AzureRmSubscription –SubscriptionName "Subscription Name" | Select-AzureRmSubscription
    ```

3.  Registe esta subscrição:

    ```
    PS C:>  Register-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" –ProviderNamespace Microsoft.RecoveryServices
    ```

## <a name="upgrade-to-instant-restore-using-cli"></a>Atualize para restaurar instantâneas com a CLI

Execute os seguintes comandos a partir de uma shell:

1.  Inicie sessão sua conta do Azure:

    ```
    az login
    ```

2.  Selecione a subscrição que pretende registar:

    ```
    az account set --subscription "Subscription Name"
    ```

3.  Registe esta subscrição:

    ```
    az feature register --namespace Microsoft.RecoveryServices --name InstantBackupandRecovery
    ```

## <a name="verify-that-the-upgrade-is-successful"></a>Certifique-se de que a atualização é efetuada com êxito

### <a name="powershell"></a>PowerShell
A partir de um terminal do PowerShell elevada, execute o seguinte cmdlet:

```
Get-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" -ProviderNamespace Microsoft.RecoveryServices
```

### <a name="cli"></a>CLI
A partir de um shell, execute o seguinte comando:

```
az feature show --namespace Microsoft.RecoveryServices --name InstantBackupandRecovery
```

Se disser "Registado", a sua subscrição está atualizada para restaurar instantânea.

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

### <a name="what-are-the-cost-implications-of-instant-restore"></a>Quais são as implicações de custo de restauro imediato?
Os instantâneos são armazenados juntamente com os discos para acelerar a criação do ponto de recuperação e restaurar as operações. Como resultado, verá os custos de armazenamento que correspondem para o período de retenção de instantâneo selecionado como parte da política de cópia de segurança de VM.

### <a name="does-snapshot-retention-increase-the-premium-storage-account-snapshot-limit-by-10-tb"></a>Retenção de instantâneo aumentar o limite de instantâneo de conta de armazenamento premium, 10 TB?
Não, o limite de instantâneo total por conta de armazenamento permanece em 10 TB.

### <a name="in-premium-storage-accounts-do-the-snapshots-taken-for-instant-recovery-point-occupy-the-10-tb-snapshot-limit"></a>Nas contas de armazenamento Premium, os instantâneos tirados de ponto de recuperação instantânea ocupam o limite de 10 TB de instantâneo?
Sim, para contas de armazenamento premium, os instantâneos tirados de ponto de recuperação instantânea, ocupam alocado 10-TB de espaço.

### <a name="how-does-the-snapshot-retention-work-during-the-five-day-period"></a>Como funciona a retenção de instantâneo durante o período de cinco dias?
Todos os dias um novo instantâneo, em seguida, existem cinco instantâneos incrementais individuais. Depende do tamanho do instantâneo no fluxo de dados, que é, na maioria dos casos aproximadamente 2% 5%.

### <a name="is-an-instant-restore-snapshot-an-incremental-snapshot-or-full-snapshot"></a>É um instantâneo de restauro imediato de um instantâneo incremental ou um instantâneo completo?
Instantâneos tirados como parte da capacidade de restauro imediato são instantâneos incrementais.

### <a name="how-can-i-calculate-the-approximate-cost-increase-due-to-instant-restore-feature"></a>Como posso calcular o aumento de custo aproximado devido a funcionalidade de restauro imediato?
Depende do volume de alterações da VM. Num estado estável, pode assumir o aumento no custo é = instantâneo período de retenção * diariamente de alterações por VM * custo de armazenamento por GB.

### <a name="if-the-recovery-type-for-a-restore-point-is-snapshot-and-vault-and-i-perform-a-restore-operation-which-recovery-type-will-be-used"></a>Se o tipo de recuperação para um ponto de restauro é "Instantâneo e cofre" e posso executar uma operação de restauro, será utilizado o tipo de recuperação?
Se o tipo de recuperação for "instantâneo e cofre", será possível automaticamente efetuar um restauro do instantâneo local, o que irá ser muito mais rápido em comparação com o restauro feito a partir do cofre.

### <a name="what-happens-if-i-select-retention-period-of-restore-point-tier-2-less-than-the-snapshot-tier1-retention-period"></a>O que acontece se eu selecionar o período de retenção do ponto de restauro (camada 2) menor que o período de retenção do instantâneo (nível 1)?
O novo modelo não permite a eliminar o ponto de restauro (administração de nível 2), a menos que o instantâneo (nível 1) é eliminado. Atualmente, suportamos o período de retenção de sete dias para eliminação de instantâneos (nível 1), para que o restauro do ponto de (administração de nível 2) o período de retenção para menos de sete dias não é honrado. Recomendamos que o serviço de agendamento do período de retenção de (escalão 2) do ponto de restauro mais de sete dias.

### <a name="why-is-my-snapshot-existing-even-after-the-set-retention-period-in-backup-policy"></a>Por que minha instantâneo existentes, mesmo após o período de retenção do conjunto de período na política de cópia de segurança?
Se o ponto de recuperação tem de instantâneo e é que a RP mais recente disponível, são mantido até à vez que existe uma cópia de segurança seguinte. Trata-se de acordo com a projetado GC política hoje que estipula a, pelo menos, uma RP mais recente sempre esteja presente no caso de todas as cópias de segurança ainda mais no falharem devido a um problema na VM. Em cenários normais, RPs são limpos no máximo de 48 horas após a respetiva expiração.
