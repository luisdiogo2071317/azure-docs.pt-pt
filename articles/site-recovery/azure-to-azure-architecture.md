---
title: Arquitetura de replicação do Azure para o Azure no Azure Site Recovery | Documentos da Microsoft
description: Este artigo fornece uma visão geral dos componentes e da arquitetura utilizada ao replicar VMs do Azure entre regiões do Azure com o serviço Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 33ab90f958e5033c0c563e4fd8921ee1f7d57c47
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/09/2018
ms.locfileid: "37915678"
---
# <a name="azure-to-azure-replication-architecture"></a>Arquitetura da replicação do Azure para o Azure


Este artigo descreve a arquitetura utilizada ao replicar, efetuar a ativação pós-falha e recuperar máquinas virtuais do Azure (VMs) entre regiões do Azure, utilizando o [do Azure Site Recovery](site-recovery-overview.md) serviço.




## <a name="architectural-components"></a>Componentes da arquitetura

O gráfico a seguir fornece uma visão geral de um ambiente de VM do Azure numa região específica (neste exemplo, a localização E.U.A. Leste). Num ambiente de VM do Azure:
- Aplicações podem ser executado em VMs com discos geridos ou discos não geridos espalhadas em contas de armazenamento.
- As VMs podem ser incluídas numa ou mais sub-redes numa rede virtual.


**Replicação do Azure para o Azure**

![customer-environment](./media/concepts-azure-to-azure-architecture/source-environment.png)

## <a name="replication-process"></a>Processo de replicação

### <a name="step-1"></a>Passo 1

Quando ativa a replicação de VM do Azure, os recursos a seguir são criados automaticamente na região de destino, com base nas definições de região de origem. Pode personalizar as definições de recursos de destino conforme necessário.

![Ativar o processo de replicação, passo 1](./media/concepts-azure-to-azure-architecture/enable-replication-step-1.png)

**Recurso** | **Detalhes**
--- | ---
**Grupo de recursos de destino** | O grupo de recursos ao qual as VMs replicadas pertencem após a ativação pós-falha. A localização deste grupo de recursos pode estar em qualquer região do Azure, exceto a região do Azure na qual as máquinas de virtuais de origem estão alojadas.
**Rede virtual de destino** | A rede virtual na qual replicadas estão localizadas as VMs após a ativação pós-falha. É criado um mapeamento de rede entre redes virtuais de origem e destino e vice-versa.
**Contas de armazenamento em cache** | Antes das alterações VM de origem são replicadas para uma conta de armazenamento de destino, sejam rastreadas e enviados para a conta de armazenamento de cache na localização de origem. Este passo garante um impacto mínimo sobre aplicações de produção em execução na VM.
**Contas de armazenamento de destino (se a origem de que VM não utiliza discos geridos)**  | Contas de armazenamento na localização de destino ao qual os dados são replicados.
* * Réplica discos geridos (se a origem de VM está em discos geridos) * *  | Os discos na localização de destino ao qual os dados são replicados geridos.
**Conjuntos de disponibilidade de destino**  | Conjuntos de disponibilidade na qual as VMs replicadas estão localizadas após a ativação pós-falha.

### <a name="step-2"></a>Passo 2

Como a replicação estiver ativada, o serviço de mobilidade de extensão do Site Recovery é automaticamente instalado na VM:

1. A VM está registada com o Site Recovery.

2. A replicação contínua está configurada para a VM. Escritas de dados nos discos de VM continuamente são transferidas para a conta de armazenamento de cache, no local de origem.

   ![Ativar o processo de replicação, o passo 2](./media/concepts-azure-to-azure-architecture/enable-replication-step-2.png)


 Recuperação de site não tem necessidade conectividade de entrada para a VM. Conectividade de saída só é necessária para o seguinte.

 - Endereços URLs/IP do serviço de recuperação de sites
 - Endereços URLs/IP de autenticação do Office 365
 - Endereços IP da conta de armazenamento de cache

Se ativar a consistência multi-VM, as máquinas no grupo de replicação comunicam entre si pela porta 20004. Certifique-se de que não há nenhum dispositivo de firewall a bloquear a comunicação interna entre as VMs através da porta 20004.

> [!IMPORTANT]
Se pretender que as VMs do Linux pertençam a um grupo de replicação, certifique-se de que o tráfego de saída na porta 20004 é aberto manualmente de acordo com as orientações da versão específica do Linux.

### <a name="step-3"></a>Passo 3

Após a replicação contínua em curso, escritas em disco imediatamente são transferidas para a conta de armazenamento de cache. Site Recovery processa os dados e envia-os para o destino de conta de armazenamento ou a réplica de discos geridos. Depois dos dados são processados, pontos de recuperação são gerados na conta de armazenamento de destino intervalos de poucos minutos.

## <a name="failover-process"></a>Processo de ativação pós-falha

Quando iniciar uma ativação pós-falha, as VMs são criadas no grupo de recursos de destino, a rede virtual de destino, a sub-rede de destino e no conjunto de disponibilidade de destino. Durante uma ativação pós-falha, pode utilizar qualquer ponto de recuperação.

![Processo de ativação pós-falha](./media/concepts-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a>Passos Seguintes

[Replicar rapidamente](azure-to-azure-quickstart.md) uma VM do Azure para uma região secundária.
