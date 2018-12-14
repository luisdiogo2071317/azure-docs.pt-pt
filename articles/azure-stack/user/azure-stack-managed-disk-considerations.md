---
title: Diferenças e considerações para o Managed Disks no Azure Stack | Documentos da Microsoft
description: Saiba mais sobre as diferenças e considerações ao trabalhar com os Managed Disks no Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: sethm
ms.reviewer: jiahan
ms.openlocfilehash: 9eed4c4bd8cd6290bd2126c91bcf4e37c1b0fa0b
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2018
ms.locfileid: "53341954"
---
# <a name="azure-stack-managed-disks-differences-and-considerations"></a>Discos geridos do Azure Stack: Diferenças e considerações
Este artigo resume as diferenças conhecidas entre Managed Disks do Azure Stack e Managed Disks do Azure. Para saber mais sobre das principais diferenças entre o Azure Stack e o Azure, consulte a [considerações da chave](azure-stack-considerations.md) artigo.

Discos geridos simplifica a gestão de discos para IaaS VMs ao gerir o [contas de armazenamento](/azure/azure-stack/azure-stack-manage-storage-accounts) associadas aos discos VM.

> [!Note]  
> Discos geridos no Azure Stack está disponível a partir 1808.
  

## <a name="cheat-sheet-managed-disk-differences"></a>Referência rápida: Gerido diferenças de disco

| Funcionalidade | (Global) do Azure | Azure Stack |
| --- | --- | --- |
|Encriptação para dados Inativos |Encriptação do serviço de armazenamento do Azure (SSE), Azure Disk Encryption (ADE)     |Encriptação de AES de 128 bits do BitLocker      |
|Imagem          | Suporte de imagem personalizada gerida |Ainda não é suportado|
|Opções de cópia de segurança |Suporta o serviço de cópia de segurança do Azure |Ainda não é suportado |
|Opções de recuperação após desastre |Suporte do Azure Site Recovery |Ainda não é suportado|
|Tipos de disco     |Premium SSD e Standard HDD SSD Standard (pré-visualização) |Premium SSD, HDD padrão |
|Discos Premium  |Totalmente suportado |Pode ser aprovisionado, mas sem limite de desempenho ou garantir  |
|IOPs de discos Premium  |Depende do tamanho do disco  |2300 IOPs por disco |
|Débito de discos Premium |Depende do tamanho do disco |145 MB por segundo por disco |
|Tamanho do disco  |Disco Premium do Azure: P4 (32 GiB) para P80 (32 TiB)<br>Disco SSD Standard do Azure: E10 (128 GiB) para E80 (32 TiB)<br>Disco HDD padrão do Azure: S4 (32 GiB) para S80 (32 TiB) |M4: 32 GiB<br>M6: 64 GiB<br>M10: 128 GiB<br>M15: 256 GiB<br>M20: 512 GiB<br>M30: 1024 giB |
|Cópia do instantâneo de discos|Tire um instantâneo do Azure discos geridos anexados a uma VM em execução suportada|Ainda não é suportado |
|Análise de desempenho de discos |Agregar as métricas e por métrica de disco suportada |Ainda não é suportado |
|Migração      |Fornecer ferramenta para migrar de não gerido do Azure Resource Manager VMs existentes sem a necessidade de recriar a VM  |Ainda não é suportado |

> [!Note]  
> IOPs de discos geridos e o débito no Azure Stack é um número de limite, em vez de um número aprovisionado, que pode ser afetado pelo hardware e cargas de trabalho em execução no Azure Stack.


## <a name="metrics"></a>Métricas
Também existem diferenças com métricas de armazenamento:
- Com o Azure Stack, os dados de transação nas métricas de armazenamento não distingue a largura de banda de rede internos ou externos.
- Dados de transação de pilha do Azure nas métricas de armazenamento não incluem o acesso à máquina virtual para os discos montados.


## <a name="api-versions"></a>Versões da API
Pilha de Managed Disks do Azure suporta as seguintes versões de API:
- 2017-03-30


## <a name="next-steps"></a>Passos Seguintes
[Saiba mais sobre as máquinas virtuais do Azure Stack](azure-stack-compute-overview.md)
