---
title: Arquitetura do Azure para o Azure replicação no Azure Site Recovery | Microsoft Docs
description: Este artigo fornece uma descrição geral dos componentes e arquitetura utilizada quando replicar VMs do Azure entre regiões do Azure utilizando o serviço do Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 05/15/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: f6ade346305ee6f18fb41b93bc8a1dc8543fcdb7
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="azure-to-azure-replication-architecture"></a>Arquitetura de replicação do Azure para o Azure


Este artigo descreve a arquitetura utilizada quando replicar, efetuar a ativação pós-falha e recuperar máquinas virtuais do Azure (VMs) entre regiões do Azure, utilizando o [do Azure Site Recovery](site-recovery-overview.md) serviço.

>[!NOTE]
>Replicação de VM do Azure com o serviço de recuperação de sites está atualmente em pré-visualização.



## <a name="architectural-components"></a>Componentes da arquitetura

O seguinte gráfico fornece uma vista de alto nível de um ambiente de VM do Azure numa região específica (neste exemplo, a localização EUA Leste). Num ambiente VM do Azure:
- As aplicações podem estar a executar em VMs com discos geridos ou não geridos discos distribuídos por contas de armazenamento.
- As VMs podem ser incluídas numa ou mais sub-redes dentro de uma rede virtual.


**Replicação do Azure para o Azure**

![customer-environment](./media/concepts-azure-to-azure-architecture/source-environment.png)

## <a name="replication-process"></a>Processo de replicação

### <a name="step-1"></a>Passo 1

Ao ativar a replicação de VM do Azure, os seguintes recursos são criados automaticamente na região de destino, com base nas definições de região de origem. Pode personalizar as definições de recursos de destino conforme necessário.

![Ativar o processo de replicação, o passo 1](./media/concepts-azure-to-azure-architecture/enable-replication-step-1.png)

**Recurso** | **Detalhes**
--- | ---
**Grupo de recursos de destino** | O grupo de recursos aos quais as VMs replicadas pertencem após a ativação pós-falha. A localização deste grupo de recursos pode estar em qualquer região do Azure, exceto a região do Azure na qual as máquinas virtuais de origem estão alojadas.
**Rede virtual de destino** | A rede virtual no qual as VMs replicadas estão localizadas após a ativação pós-falha. Um mapeamento da rede é criado entre redes virtuais de origem e de destino e vice-versa.
**Contas de armazenamento de cache** | Replicar as alterações VM de origem para uma conta de armazenamento de destino, são controladas e enviados para a conta de armazenamento de cache numa localização de origem. Este passo garante um impacto mínimo no aplicações de produção em execução na VM.
**As contas de armazenamento de destino (se não utilizar a VM de origem gerido discos)**  | Contas de armazenamento na localização de destino para o qual os dados são replicados.
* * Réplica discos geridos pelo (se a origem a VM está em discos geridos pelo) * *  | Gerido discos na localização de destino para o qual os dados são replicados.
**Conjuntos de disponibilidade de destino**  | Conjuntos de disponibilidade na qual as VMs replicadas estão localizadas após a ativação pós-falha.

### <a name="step-2"></a>Passo 2

Como a replicação está ativada, o serviço de mobilidade de extensão de recuperação de Site é automaticamente instalado na VM:

1. A VM está registada com a recuperação de Site.

2. Replicação contínua está configurada para a VM. Operações de escrita de dados nos discos de VM continuamente são transferidas para a conta de armazenamento de cache, na localização de origem.

   ![Ativar o processo de replicação, o passo 2](./media/concepts-azure-to-azure-architecture/enable-replication-step-2.png)


 Recuperação de site nunca necessita de conectividade de entrada para a VM. Conectividade de saída só é necessário para o seguinte.

 - Endereços IP/os URLs do serviço de recuperação de sites
 - Endereços de IP/os URLs de autenticação do Office 365
 - Endereços IP de conta de armazenamento de cache

Se ativar a consistência multi-VM, as máquinas no grupo de replicação comunicam entre si pela porta 20004. Certifique-se de que não há nenhum dispositivo de firewall a bloquear a comunicação interna entre as VMs através da porta 20004.

> [!IMPORTANT]
Se pretender que as VMs do Linux pertençam a um grupo de replicação, certifique-se de que o tráfego de saída na porta 20004 é aberto manualmente de acordo com as orientações da versão específica do Linux.

### <a name="step-3"></a>Passo 3

Após a conclusão da replicação contínua em curso, escritas em disco imediatamente são transferidas para a conta de armazenamento de cache. Recuperação do site processa os dados e envia-a para o destino da conta de armazenamento ou réplica discos geridos pelo. Depois dos dados são processados, pontos de recuperação são gerados na conta de armazenamento de destino em alguns minutos.

## <a name="failover-process"></a>Processo de ativação pós-falha

Quando seleciona uma ativação pós-falha, as VMs são criadas no grupo de recursos de destino, a rede virtual de destino, a sub-rede de destino e, no conjunto de disponibilidade de destino. Durante uma ativação pós-falha, pode utilizar qualquer ponto de recuperação.

![Processo de ativação pós-falha](./media/concepts-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a>Passos Seguintes

[Replicar rapidamente](azure-to-azure-quickstart.md) uma VM do Azure para uma região secundária.
