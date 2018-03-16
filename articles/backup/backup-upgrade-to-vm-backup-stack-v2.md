---
title: "Atualizar para a pilha de cópia de segurança de VM do Azure V2 | Microsoft Docs"
description: "Atualizar o processo e perguntas mais frequentes para a pilha de cópia de segurança de VM V2"
services: backup, virtual-machines
documentationcenter: 
author: trinadhk
manager: vijayts
tags: azure-resource-manager, virtual-machine-backup
ms.assetid: 
ms.service: backup, virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 03/08/2018
ms.author: trinadhk
ms.openlocfilehash: b7e9f45c61d2af1940be50a368b87cd35c85b1dd
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="upgrade-to-vm-backup-stack-v2"></a>Atualizar para a pilha de cópia de segurança de VM V2
A atualização de cópia de segurança pilha V2 de máquina virtual (VM) fornece as seguintes melhorias de funcionalidade:
* Capacidade para ver o instantâneo tirado como parte da tarefa de cópia de segurança para estar disponível para recuperação sem aguardar a transferência de dados concluir.
Reduzirá a espera instantâneo para ser copiado para o Cofre antes de acionar o restauro. Além disso, este irá eliminar os requisitos de armazenamento adicional para a cópia de segurança de VMs do premium, exceto a primeira cópia de segurança.  

* Redução no tempo de cópia de segurança e restauro mantendo instantâneos localmente durante 7 dias. 

* Suporte para o disco tamanhos até 4 TB.  

* Capacidade de utilizar as contas do storage original (mesmo quando a VM tem discos que estão distribuídos contas de armazenamento) quando efetuar um restauro de uma VM não gerido. Isto tornará restauros mais rápida para uma grande variedade de configurações de VM. 
    > [!NOTE] 
    > Não é igual ao substituir a VM original. 
    > 
    >

## <a name="what-is-changing-in-the-new-stack"></a>O que está a alterar na pilha de novo?
Atualmente, a tarefa de cópia de segurança consiste em duas fases:
1.  Criar o instantâneo VM. 
2.  Transferência de instantâneos VM para o Cofre de cópia de segurança do Azure. 

Um ponto de recuperação é considerado criados apenas depois de terminado fases 1 e 2. Como parte da pilha de novo, é criado um ponto de recuperação, assim que o instantâneo é concluído. Também pode restaurar a partir deste ponto de recuperação utilizando o mesmo fluxo de restauro. Pode identificar este ponto de recuperação no portal do Azure utilizando "instantâneo" como tipo de ponto de recuperação. Após o instantâneo é transferido para o cofre, tipo de ponto de recuperação é alterado para "Instantâneo e cofre". 

![Tarefa de cópia de segurança na pilha de cópia de segurança de VM V2](./media/backup-azure-vms/instant-rp-flow.jpg) 

Por predefinição, instantâneos serão mantidos durante sete dias. Isto permite que o restauro para ser efetuada mais rapidamente nestes instantâneos, ao reduzir o tempo necessário para copiar dados novamente a partir do cofre para a conta de armazenamento do cliente. 

## <a name="considerations-before-upgrade"></a>Considerações sobre antes da atualização
* Esta é uma atualização unidirecional da pilha de cópia de segurança de VM. Por isso, todas as cópias de segurança futuras entra neste fluxo. Uma vez que **estiver ativada a um nível de subscrição, todas as VMs que serão enviadas para este fluxo**. Todas as novas adições de funcionalidade serão com base na pilha mesma. Capacidade para controlar que este nível de política futuras nas futuras versões. 
* Para VMs com discos premium, durante a primeira cópia de segurança, certifique-se de que o espaço de armazenamento equivalente ao tamanho da VM está disponível na conta de armazenamento até concluir a primeira cópia de segurança. 
* Uma vez que os instantâneos são armazenados localmente para melhorar a criação do ponto de recuperação e também para acelerar o restauro, verá os custos de armazenamento correspondente a instantâneos durante o período de sete dias.
Para discos geridos, não há *não aumento no preço* porque restorePointCollections são gratuitas. 
* Se estão a fazer um restauro do ponto de recuperação de instantâneos para uma VM do Premium, verá uma localização de armazenamento temporário que está a ser utilizada enquanto a VM está a ser criada como parte do restauro. 

## <a name="how-to-upgrade"></a>Como atualizar?
### <a name="the-azure-portal"></a>O portal do Azure
Se estiver a utilizar no portal do Azure, verá uma notificação no dashboard do cofre relacionados com o suporte de disco grande e cópia de segurança e restaurar os melhoramentos de velocidade.

![Tarefa de cópia de segurança na pilha de cópia de segurança de VM V2](./media/backup-azure-vms/instant-rp-banner.png) 

Para abrir um ecrã para atualizar para a nova pilha, selecione de faixa. 

![Tarefa de cópia de segurança na pilha de cópia de segurança de VM V2](./media/backup-azure-vms/instant-rp.png) 

### <a name="powershell"></a>PowerShell
Execute os seguintes cmdlets a partir de um elevada do PowerShell terminal:
1.  Inicie sessão sua conta do Azure. 

```
PS C:> Login-AzureRmAccount
```

2.  Selecione a subscrição que pretende registar para pré-visualização:

```
PS C:>  Get-AzureRmSubscription –SubscriptionName "Subscription Name" | Select-AzureRmSubscription
```

3.  Registe esta subscrição para pré-visualização privada:

```
PS C:>  Register-AzureRmProviderFeature -FeatureName “InstantBackupandRecovery” –ProviderNamespace Microsoft.RecoveryServices
```

## <a name="verify-whether-the-upgrade-is-complete"></a>Certifique-se a atualização é concluída
A partir de um terminal PowerShell elevada, execute o seguinte cmdlet:

```
Get-AzureRmProviderFeature -FeatureName “InstantBackupandRecovery” –ProviderNamespace Microsoft.RecoveryServices
```

Se diz registada, a subscrição está atualizada à pilha de cópia de segurança de VM V2. 



