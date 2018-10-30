---
title: Modelos do Azure Resource Manager para o Azure Backup
description: Exemplos do PowerShell para o Azure Backup
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: sample
ms.date: 04/18/2018
ms.author: markgal
ms.custom: mvc
ms.openlocfilehash: 941357d0a8b7fb46a5c2dcebb70681452118f23f
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/18/2018
ms.locfileid: "49406386"
---
# <a name="azure-resource-manager-templates-for-azure-backup"></a>Modelos do Azure Resource Manager para o Azure Backup

A tabela seguinte inclui ligações para modelos do Azure Resource Manager para utilização com os cofres dos Serviços de Recuperação e as funcionalidades do Azure Backup.

|   |   |
|---|---|
|**Cofre dos Serviços de Recuperação** | |
| [Criar um cofre dos Serviços de Recuperação](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-vault-create)| Crie um cofre dos Serviços de Recuperação. O cofre pode ser utilizado no Azure Backup e no Azure Site Recovery. |
|**Fazer uma cópia de segurança das máquinas virtuais**| |
| [Fazer uma cópia de segurança das VMs do Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-backup-vms) | Utilize o cofre dos Serviços de Recuperação e a política de Cópia de Segurança existentes para fazer uma cópia de segurança das máquinas virtuais do Resource Manager no mesmo grupo de recursos.|
| [Fazer uma cópia de segurança das VMs IaaS para o cofre dos Serviços de Recuperação](https://github.com/Azure/azure-quickstart-templates/tree/master/201-recovery-services-backup-classic-resource-manager-vms) | Modelo para fazer cópias de segurança de máquinas virtuais clássicas e do Resource Manager. |
| [Criar uma política de Cópia de Segurança Semanal para VMs IaaS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-weekly-backup-policy-create) | O modelo cria um cofre dos Serviços de Recuperação e uma política de cópia de segurança semanal que será utilizada para fazer uma cópia de segurança de máquinas virtuais clássicas e do Resource Manager.|
| [Criar uma política de Cópia de Segurança Diária para VMs IaaS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-daily-backup-policy-create) | O modelo cria um cofre dos Serviços de Recuperação e uma política de cópia de segurança diária que será utilizada para fazer uma cópia de segurança de máquinas virtuais clássicas e do Resource Manager.|
| [Implementar a VM do Windows Server com cópia de segurança ativada](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-create-vm-and-configure-backup) | O modelo cria uma VM do Windows Server e um cofre dos Serviços de Recuperação com a política de cópia de segurança predefinida ativada.|
|**Monitorizar tarefas de cópia de segurança** |  |
| [Utilizar o Log Analytics para monitorizar o Azure Backup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-backup-oms-monitoring) | O modelo implementa a Monitorização do Log Analytics para o Azure Backup, que lhe permite monitorizar as tarefas de cópia de segurança e restauro, os alertas de cópia de segurança e o armazenamento na Cloud utilizados nos seus cofres dos Serviços de Recuperação.|  
|   |   |

