---
title: Diferenças e considerações para o Managed Disks no Azure Stack | Documentos da Microsoft
description: Saiba mais sobre as diferenças e considerações ao trabalhar com os Managed Disks no Azure Stack.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: brenduns
ms.reviewer: jiahan
ms.openlocfilehash: 4fd2a26d9119e52fc75918abc1ca97e6f9888169
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/06/2018
ms.locfileid: "44028600"
---
# <a name="azure-stack-managed-disks-differences-and-considerations"></a>Pilha Managed Disks do Azure: Diferenças e considerações
Este artigo resume as diferenças conhecidas entre Managed Disks do Azure Stack e Managed Disks do Azure. Para saber mais sobre das principais diferenças entre o Azure Stack e o Azure, consulte a [considerações da chave](azure-stack-considerations.md) artigo.

Discos geridos simplifica a gestão de discos para IaaS VMs ao gerir o [contas de armazenamento](/azure/azure-stack/azure-stack-manage-storage-accounts) associadas aos discos VM.
  

## <a name="cheat-sheet-virtual-machine-differences"></a>Referência rápida: diferenças de Máquina Virtual

| Funcionalidade | (Global) do Azure | Azure Stack |
| --- | --- | --- |
|Encriptação para dados Inativos |Encriptação do serviço de armazenamento do Azure (SSE), Azure Disk Encryption (ADE)     |Encriptação de AES de 128 bits do BitLocker      |
|Imagem          | Suporte de imagem personalizada gerida |Ainda não é suportado|
|Opções de cópia de segurança |Suporta o serviço de cópia de segurança do Azure |Ainda não é suportado |
|Opções de recuperação após desastre |Suporte do Azure Site Recovery |Ainda não é suportado|
|Tipos de disco     |Premium SSD e Standard HDD SSD Standard (pré-visualização) |Premium SSD, HDD padrão |
|Discos Premium  |Totalmente suportado |Pode ser aprovisionado, mas sem limite de desempenho ou garantir  |
|Discos Premium  |IOPs  |Depende do tamanho 2300 IOPs por disco de disco |
|Débito de discos Premium |Depende do tamanho do disco |145 MB por segundo por disco |
|Tamanho máximo do disco  |4 TB       |1 TB       |
|Análise de desempenho de discos |Agregar as métricas e por métrica de disco suportada |Ainda não é suportado |
|Migração      |Fornecer ferramenta para migrar de não gerido do Azure Resource Manager VMs existentes sem a necessidade de recriar a VM  |Ainda não é suportado |


## <a name="metrics"></a>Métricas
Também existem diferenças com métricas de armazenamento:
- Com o Azure Stack, os dados de transação nas métricas de armazenamento não distingue a largura de banda de rede internos ou externos.
- Dados de transação de pilha do Azure nas métricas de armazenamento não incluem o acesso à máquina virtual para os discos montados.


## <a name="api-versions"></a>Versões da API
Pilha de Managed Disks do Azure suporta as seguintes versões de API:
- 2017-03-30


## <a name="next-steps"></a>Passos Seguintes
[Saiba mais sobre as máquinas virtuais do Azure Stack](azure-stack-compute-overview.md)
