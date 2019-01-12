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
ms.date: 01/05/2019
ms.author: sethm
ms.reviewer: jiahan
ms.openlocfilehash: 78cb969aa96378dd84243545be1678ae4eaf0e0e
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/11/2019
ms.locfileid: "54232537"
---
# <a name="azure-stack-managed-disks-differences-and-considerations"></a>Pilha Managed Disks do Azure: diferenças e considerações

Este artigo resume as diferenças conhecidas entre [Managed Disks do Azure Stack](azure-stack-manage-vm-disks.md) e [Managed Disks do Azure](../../virtual-machines/windows/managed-disks-overview.md). Para saber mais sobre das principais diferenças entre o Azure Stack e o Azure, consulte a [considerações da chave](azure-stack-considerations.md) artigo.

Discos geridos simplifica a gestão de discos para IaaS VMs ao gerir o [contas de armazenamento](../azure-stack-manage-storage-accounts.md) associadas aos discos VM.

> [!Note]  
> Discos geridos no Azure Stack está disponível no 1808 update. Ele é habilitado por padrão durante a criação de máquinas virtuais através do portal do Azure Stack do 1811 update.
  

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
|Migração      |Fornecer ferramenta para migrar de não geridos do Azure Resource Manager VMs existentes sem a necessidade de recriar a VM  |Ainda não é suportado |

> [!NOTE]  
> IOPs de discos geridos e o débito no Azure Stack é um número de limite, em vez de um número aprovisionado, que pode ser afetado pelo hardware e cargas de trabalho em execução no Azure Stack.

## <a name="metrics"></a>Métricas

Também existem diferenças com métricas de armazenamento:

- Com o Azure Stack, os dados de transação nas métricas de armazenamento não distinguem largura de banda de rede internos ou externos.
- Dados de transação de pilha do Azure nas métricas de armazenamento não incluem o acesso à máquina virtual para os discos montados.

## <a name="api-versions"></a>Versões da API

Pilha de Managed Disks do Azure suporta as seguintes versões de API:

- 2017-03-30

## <a name="configuration"></a>Configuração

Depois de aplicar o 1808 atualizar ou posterior, tem de efetuar a seguinte configuração antes de utilizar discos geridos:

- Se uma subscrição tiver sido criada antes da atualização de 1808, siga os passos para atualizar a subscrição abaixo. Caso contrário, a implementação de VMs nesta subscrição poderá falhar com uma mensagem de erro "Erro interno no Gestor de discos."
   1. No portal do inquilino, aceda a **subscrições** e encontrar a subscrição. Clique em **fornecedores de recursos**, em seguida, clique em **Microsoft. Compute**e, em seguida, clique em **voltar a registar**.
   2. Na mesma subscrição, aceda a **controlo de acesso (IAM)** e certifique-se de que **disco gerido do Azure Stack –** está listado.
- Se utilizar um ambiente multi-inquilino, peça ao seu operador da nuvem (Maio na sua organização ou do fornecedor de serviços) para reconfigurar cada um dos seus diretórios de convidado, siga estes passos [este artigo](../azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory). Caso contrário, a implementação de VMs numa assinatura associada um diretório de convidado poderá falhar com uma mensagem de erro "Erro interno no Gestor de discos."


## <a name="next-steps"></a>Passos Seguintes

- [Saiba mais sobre as máquinas virtuais do Azure Stack](azure-stack-compute-overview.md)
