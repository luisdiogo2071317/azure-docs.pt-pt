---
title: Atualizar para o modelo de implementação Azure Resource Manager para a pilha de cópia de segurança de VM do Azure | Microsoft Docs
description: Atualizar o processo e perguntas mais frequentes para a pilha de cópia de segurança de VM, modelo de implementação do Resource Manager
services: backup, virtual-machines
documentationcenter: ''
author: trinadhk
manager: vijayts
tags: azure-resource-manager, virtual-machine-backup
ms.assetid: ''
ms.service: backup, virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 03/08/2018
ms.author: trinadhk, sogup
ms.openlocfilehash: 1e5515486afac5a6d84a35bca33f55ae98e287d3
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="upgrade-to-the-azure-resource-manager-deployment-model-for-azure-vm-backup-stack"></a>Atualizar para o modelo de implementação Azure Resource Manager para a pilha de cópia de segurança de VM do Azure
O modelo de implementação Resource Manager para a atualização para a pilha de cópia de segurança de máquina virtual (VM) fornece as seguintes melhorias de funcionalidade:
* Capacidade de ver instantâneos tirados como parte de uma tarefa de cópia de segurança que está disponível para recuperação sem aguardar a transferência de dados para concluir. Reduz o tempo de espera de instantâneos copiar para o Cofre antes de acionar o restauro. Além disso, esta capacidade elimina o requisito de armazenamento adicional para cópia de segurança de VMs do premium, exceto a primeira cópia de segurança.  

* Redução no tempo de cópia de segurança e restauro mantendo instantâneos localmente durante sete dias.

* Suporte para o disco tamanhos até 4 TB.

* Capacidade para utilizar as contas do storage originais da VM não geridas ao restaurar. Esta capacidade existe mesmo quando a VM possui discos que estão distribuídos contas de armazenamento. Faz restauros mais rápida para uma grande variedade de configurações de VM.
    > [!NOTE] 
    > Esta capacidade não é igual a substituição da VM original. 
    >

## <a name="whats-changing-in-the-new-stack"></a>O que está a alterar na pilha de novo?
Atualmente, a tarefa de cópia de segurança consiste em duas fases:
1.  A criar um instantâneo VM. 
2.  Transferência de um instantâneo VM para o Cofre de cópia de segurança do Azure. 

Um ponto de recuperação é considerado criados apenas depois de terminado fases 1 e 2. Como parte da pilha de novo, é criado um ponto de recuperação, assim que o instantâneo estiver concluído. Também pode restaurar a partir deste ponto de recuperação utilizando o mesmo fluxo de restauro. Pode identificar este ponto de recuperação no portal do Azure através da utilização de "instantâneo" como o tipo de ponto de recuperação. Depois do instantâneo é transferido para o cofre, o tipo de ponto de recuperação é alterado para "instantâneo e cofre." 

![Tarefa de cópia de segurança no VM cópia de segurança de pilha do Resource Manager modelo de implementação – cofre e de armazenamento](./media/backup-azure-vms/instant-rp-flow.jpg) 

Por predefinição, os instantâneos são mantidos durante sete dias. Esta funcionalidade permite o restauro concluir mais rápida destes instantâneos. Reduz o tempo necessária para copiar dados novamente a partir do cofre para a conta de armazenamento do cliente. 

## <a name="considerations-before-upgrade"></a>Considerações sobre antes da atualização
* A atualização da pilha de cópia de segurança de VM é um direcional. Por isso, todas cópias de segurança enviadas para este fluxo. Porque se encontra ativada ao nível da subscrição, todas as VMs entrar neste fluxo. Todas as novas adições funcionalidade baseiam-se a mesma pilha. Capacidade para controlar que este nível de política futuras nas futuras versões.

* Instantâneos são armazenados localmente para melhorar a criação do ponto de recuperação e também para acelerar o restauro. Por conseguinte, pode ver os custos de armazenamento que correspondem aos instantâneos durante o período de sete dias.

* Instantâneos incrementais são armazenados como blobs de páginas. Todos os clientes que utilizam discos não geridos são-lhe cobrados durante sete dias que os instantâneos são armazenados na conta de armazenamento local do cliente. De acordo com o modelo de preços atual, há sem qualquer custo para clientes em discos geridos.

* Se efetuar um restauro a partir de um ponto de recuperação de instantâneos para uma VM do premium, verá uma localização de armazenamento temporário que é utilizada durante a VM é criada como parte do restauro.

* Para contas de armazenamento premium, os instantâneos efetuadas para recuperação imediata ocupam 10 TB de espaço alocado.

## <a name="upgrade"></a>Atualizar
### <a name="the-azure-portal"></a>O portal do Azure
Se utilizar o portal do Azure, verá uma notificação no dashboard do cofre. Esta notificação se relaciona com suporte de disco grande e melhorias de velocidade de cópia de segurança e restauro.

![Tarefa de cópia de segurança no modelo de implementação de Gestor de recursos de pilha de cópia de segurança de VM – notificação de suporte](./media/backup-azure-vms/instant-rp-banner.png) 

Para abrir um ecrã para atualizar para a nova pilha, selecione de faixa. 

![Tarefa de cópia de segurança na pilha de cópia de segurança de VM modelo de implementação do Resource Manager – atualizar](./media/backup-azure-vms/instant-rp.png) 

### <a name="powershell"></a>PowerShell
Execute os seguintes cmdlets a partir de um elevada do PowerShell terminal:
1.  Inicie sessão sua conta do Azure: 

    ```
    PS C:> Connect-AzureRmAccount
    ```

2.  Selecione a subscrição que pretende registar para pré-visualização:

    ```
    PS C:>  Get-AzureRmSubscription –SubscriptionName "Subscription Name" | Select-AzureRmSubscription
    ```

3.  Registe esta subscrição para pré-visualização privada:

    ```
    PS C:>  Register-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" –ProviderNamespace Microsoft.RecoveryServices
    ```

## <a name="verify-that-the-upgrade-is-finished"></a>Certifique-se de que a atualização estiver concluída
A partir de um terminal PowerShell elevada, execute o seguinte cmdlet:

```
Get-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" –ProviderNamespace Microsoft.RecoveryServices
```

Se diz "Registada", a sua subscrição está atualizada ao modelo de implementação de Gestor de recursos de pilha de cópia de segurança de VM.
