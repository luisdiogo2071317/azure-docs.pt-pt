---
title: O Azure Site Recovery de resolução de problemas para erros e problemas de replicação do Azure para o Azure | Documentos da Microsoft
description: Erros e problemas de resolução de problemas ao replicar máquinas virtuais do Azure para recuperação após desastre
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/30/2018
ms.author: asgang
ms.openlocfilehash: c1cc8769617e4c9cd823b1e44d60c49b73f4bdb9
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51289716"
---
# <a name="troubleshoot-azure-to-azure-vm-ongoing-replication-issues"></a>Resolver problemas de replicação em curso de VMS do Azure para o Azure

Este artigo descreve os problemas comuns no Azure Site Recovery quando replicar e recuperar máquinas virtuais do Azure a partir de uma região para outra região e explica como solucionar esses problemas. Para obter mais informações sobre configurações suportadas, consulte a [matriz de suporte para replicar VMs do Azure](site-recovery-support-matrix-azure-to-azure.md).


## <a name="recovery-points-not-getting-generated"></a>Pontos de recuperação não ser gerados

MENSAGEM de erro: Não existem falhas consistente ponto de recuperação disponível para a VM nos últimos 60 minutos.</br>
ID DO ERRO: 153007 </br>

O Azure Site Recovery replica os dados da região de origem para a região de recuperação após desastre de forma consistente e cria ponto consistente com a falha a cada 5 minutos. Se o Site Recovery não consegue criar pontos de recuperação durante 60 minutos, em seguida, alerta utilizador. Seguem-se as causas que poderá resultar neste erro:

**Causa 1: [taxa na máquina de virtual de origem de alteração de dados elevado](#high-data-change-rate-on-the-source-virtal-machine)**    
**Causa 2: [problema de conectividade de rede ](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**

## <a name="causes-and-solutions"></a>Causas e soluções

### <a name="high-data-change-rate-on-the-source-virtal-machine"></a>Taxa na origem da Máquina Virtual de alteração de dados elevado
O Azure Site Recovery dispara um evento se a taxa de alteração de dados na máquina de virtual de origem for maior do que os limites suportados. Para verificar se o problema é devido ao elevado número de alterações, aceda a itens de replicado > VM > clique em "eventos-últimas 72 horas".
Deverá ver a evento "dados taxa de alteração para além dos limites suportados", conforme mostrado na captura de ecrã abaixo

![data_change_rate_high](./media/site-recovery-azure-to-azure-troubleshoot/data_change_event.png)

Se clicar no evento, deverá ver as informações do disco exato conforme mostrado na captura de ecrã abaixo

![data_change_rate_event](./media/site-recovery-azure-to-azure-troubleshoot/data_change_event2.png)


#### <a name="azure-site-recovery-limits"></a>Limites do Azure Site Recovery
A tabela seguinte fornece os limites do Azure Site Recovery. Estes limites baseiam-se nos nossos testes, mas não abrangem todas as combinações de E/S de aplicações possíveis. Os resultados reais podem variar consoante a combinação de E/S da sua aplicação. Observe também que existem limites de duas a serem considerados, por disco de dados de alterações e por dados da máquina virtual de abandono.
Por exemplo, se observarmos o disco Premium P20 na abaixo da tabela, Site Recovery pode lidar com alterações a 5 MB/s por disco com a máxima de cinco esses discos por VM devido ao limite de 25 MB/s total de alterações por VM.

**Destino do armazenamento da replicação** | **Tamanho médio de E/S do disco de origem** |**Média de alterações a dados do disco de origem** | **Total de alterações a dados do disco de origem por dia**
---|---|---|---
Armazenamento Standard | 8 KB | 2 MB/s | 168 GB por disco
Disco Premium P10 ou P15 | 8 KB  | 2 MB/s | 168 GB por disco
Disco Premium P10 ou P15 | 16 KB | 4 MB/s |  336 GB por disco
Disco Premium P10 ou P15 | 32 KB ou superior | 8 MB/s | 672 GB por disco
Disco Premium P20 ou P30 ou P40 ou P50 | 8 KB    | 5 MB/s | 421 GB por disco
Disco Premium P20 ou P30 ou P40 ou P50 | 16 KB ou superior |10 MB/s | 842 GB por disco


**Alterações a dados de origem** | **Limite máximo**
---|---
Média de alterações a dados por VM| 25 MB/s
Alterações a dados de pico em todos os discos numa VM | 54 MB/s

### <a name="solution"></a>Solução
É necessário compreendermos que o Azure Site Recovery tem limites de velocidade, com base no tipo de disco de alteração de dados. Para saber se este problema seja recorrente ou momentaneamente, é importante localizar os dados alterados padrão da taxa da máquina virtual afetada.
Para localizar os dados de taxa de alteração da máquina virtual afetada. Aceda a máquina virtual de origem > métricas em monitorização e adicione as métricas, como mostrado abaixo.

![high_data_change_rate](./media/site-recovery-azure-to-azure-troubleshoot/churn.png)

1. Clique em "Adicionar métrica" e adicione "Bytes/seg de escrita de disco do SO" e "Bytes/seg de escrita de disco de dados".
2. Monitorize o pico, conforme mostrado na captura de ecrã.
3. Esta operação irá mostrar que o total escreve operação a acontecer no disco do SO e todos os discos de dados combinados. Agora estas métricas podem não informará por informações do nível de disco, mas são um bom indicador do padrão de alterações a total de dados.

Em casos como acima se é um fluxo de dados ocasionais e a taxa de alteração de dados for superior a 10 (para Premium) e 2 MBps (por padrão) há algum tempo e resume, replicação será acompanhar. No entanto se o volume de alterações está muito além do limite suportado na maioria das vezes, em seguida, deve considerar uma do abaixo opção se for possível:

**Opção 1:** excluir o disco, o que está causando a taxa de alteração de dados de alta: </br>
Atualmente pode excluir o disco com [Powershell da recuperação de Site](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#replicate-azure-virtual-machine)

**Opção 2:** alterar a camada de disco de armazenamento de recuperação de desastres: </br>
Esta opção só é possível se o volume de alterações de dados do disco é inferior a 10 MB/s. Permitir que dizer que uma VM com P10 disco está a ter uma alterações a dados da maior que 8 MB/s, mas inferior a 10 MB/s. Se o cliente pode utilizar P30 disco para armazenamento de destino durante a proteção, pode ser resolvido o problema.

### <a name="Network-connectivity-issue"></a>Problema de conectividade de rede

Para replicação do Site Recovery para o trabalho, a conectividade de saída para URLs ou IP específicos a intervalos é necessária da VM. Se a VM estiver protegido por uma firewall ou utiliza regras de grupo (NSG) de segurança de rede para controlar a conectividade de saída, poderá deparar-se com um desses problemas.</br>
Consulte [conectividade de saída para URLs do Site Recovery](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-troubleshoot-errors?#outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072)
