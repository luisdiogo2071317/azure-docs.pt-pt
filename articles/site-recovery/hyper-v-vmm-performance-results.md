---
title: "Resultados do teste para replicação de VMs de Hyper-V em nuvens VMM para um site secundário com o Azure Site Recovery | Microsoft Docs"
description: "Este artigo fornece informações sobre testes de desempenho para replicação de VMs de Hyper-V em nuvens VMM para um site secundário utilizando o Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 02/13/2018
ms.author: raynew
ms.openlocfilehash: e15f435a3f32b8908b5b93bccc6c57710ab589bc
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="test-results-for-hyper-v-replication-to-a-secondary-site"></a>Resultados do teste para replicação de Hyper-V para um site secundário

Este artigo fornece os resultados de testes quando replicar VMs de Hyper-V em nuvens do System Center Virtual Machine Manager (VMM), para um datacenter secundário de desempenho.

## <a name="test-goals"></a>Objetivos de teste

Era o objetivo de testar examinar como efetua a recuperação de sites durante a replicação de estado de repouso.

- Estado de repouso replicação ocorre quando VMs concluíram a replicação inicial e estiver a sincronizar as alterações de delta.
- É importante medir o desempenho através de estado de repouso, porque é o estado em que a maioria das VMs permanecem, exceto se ocorrerem falhas inesperadas.
- A implementação de teste consistiu em dois sites no local, com um servidor do VMM em cada site. Esta implementação de teste é típica de implementação de sucursal/office head, com sede a agir como o site primário e o escritório de sucursal como o site secundário ou de recuperação.

## <a name="what-we-did"></a>O que fizemos

Eis o que podemos no teste passou:

1. VMs criadas utilizando modelos do VMM.
2. Iniciado VMs e capturados métricas de desempenho da linha de base superior a 12 horas.
3. Nuvens criadas no principal e servidores do VMM de recuperação.
4. Replicação configurada no Site Recovery, incluindo o mapeamento entre a origem e de recuperação de nuvens.
5. Ativar a proteção para VMs e permitido-los a replicação inicial.
6. Aguardaram duas horas para estabilização de sistema.
7. Capturar as métricas de desempenho superior a 12 horas, em que todas as VMs permanecer num Estado de replicação esperado para esses 12 horas.
8. A medida as diferenças entre as métricas de desempenho da linha de base e as métricas de desempenho de replicação.


## <a name="primary-server-performance"></a>Desempenho do servidor primário

* Réplica Hyper-V (utilizado pelo Site Recovery) no modo assíncrono regista as alterações para um ficheiro de registo, com armazenamento mínima sobrecarga no servidor primário.
* Réplica do Hyper-V utiliza a cache de memória personalizada manter para minimizar o IOPS sobrecarga para o controlo. Armazena escreve o VHDX na memória e os remove da cache para o ficheiro de registo antes do tempo que o registo é enviado para o site de recuperação. Um disco libertação ocorre também se as escritas atingiu um limite predeterminado.
* O gráfico abaixo mostra a sobrecarga IOPS de estado de repouso para replicação. É possível ver que o IOPS sobrecarga devido a replicação é de aproximadamente 5%, que é bastante baixa.

  ![Resultados primários](./media/hyper-v-vmm-performance-results/IC744913.png)

Réplica do Hyper-V utiliza a memória no servidor primário, para otimizar o desempenho de disco. Como é mostrado no seguinte gráfico, memória sobrecarga em todos os servidores no cluster primário é marginal. A memória sobrecarga apresentada é a percentagem de memória utilizada pela replicação, em comparação comparada a memória total instalada no servidor de Hyper-V.

![Resultados primários](./media/hyper-v-vmm-performance-results/IC744914.png)

Réplica do Hyper-V tem mínimo sobrecarga da CPU. Conforme apresentado no gráfico, overhead da replicação está no intervalo de % 2 a 3.

![Resultados primários](./media/hyper-v-vmm-performance-results/IC744915.png)

## <a name="secondary-server-performance"></a>Desempenho do servidor secundário

Réplica do Hyper-V utiliza uma pequena quantidade de memória no servidor de recuperação, para otimizar o número de operações de armazenamento. O gráfico resume a utilização da memória no servidor de recuperação. A memória sobrecarga apresentada é a percentagem de memória utilizada pela replicação, em comparação comparada a memória total instalada no servidor de Hyper-V.

![Resultados secundários](./media/hyper-v-vmm-performance-results/IC744916.png)

A quantidade de operações de e/s no site de recuperação é uma função do número de operações de escrita no site primário. Vamos observar as operações de e/s totais no site de recuperação in comparison with as operações de e/s totais exibidas as operações no site primário. Os gráficos mostram que o total de IOPS no site de recuperação

* Cerca de 1,5 vezes a operação de escrita IOPS principal.
* Cerca de 37% do IOPS total no site primário.

![Resultados secundários](./media/hyper-v-vmm-performance-results/IC744917.png)

![Resultados secundários](./media/hyper-v-vmm-performance-results/IC744918.png)

## <a name="effect-on-network-utilization"></a>Efeito na utilização de rede

Uma média de 275 Mb por segundo de largura de banda de rede foi utilizada entre os nós primário e de recuperação (com compressão ativado), com uma largura de banda existente de 5 Gb por segundo.

![Utilização de rede de resultados](./media/hyper-v-vmm-performance-results/IC744919.png)

## <a name="effect-on-vm-performance"></a>Efeito no desempenho da VM

Uma consideração importante é o impacto da replicação em cargas de trabalho de produção em execução nas máquinas virtuais. Se o site primário tem é aprovisionado para replicação, não deve existir qualquer impacto nas cargas de trabalho. Mecanismo de controlo de lightweight de réplica de Hyper-V assegura que as cargas de trabalho em execução nas máquinas virtuais não são afetadas durante a replicação de estado de repouso. Esta situação é ilustrada no seguintes gráficos.

Este gráfico mostra o que IOPS realizadas por máquinas virtuais em execução diferentes cargas de trabalho, antes e após a replicação foi ativada. Pode observar o que não há qualquer diferença entre os dois.

![Resultados do efeito de réplica](./media/hyper-v-vmm-performance-results/IC744920.png)

O gráfico seguinte mostra o débito de máquinas virtuais em execução diferentes cargas de trabalho, antes e após a replicação foi ativada. Pode observar a que a replicação não tem nenhum impacto significativo.

![Efeitos de réplica de resultados](./media/hyper-v-vmm-performance-results/IC744921.png)

## <a name="conclusion"></a>Conclusão

Os resultados mostram claramente que a recuperação de sites, conjugada com a réplica do Hyper-V, dimensiona bem com o mínimo de sobrecarga de um cluster de grandes dimensões. Recuperação de sites fornece implementação simples, a replicação, a gestão e monitorização. Réplica do Hyper-V fornece a infraestrutura necessária para o dimensionamento de replicação com êxito. 

## <a name="test-environment-details"></a>Detalhes de ambiente de teste

### <a name="primary-site"></a>Site primário

* O site primário tem um cluster de cinco servidores de Hyper-V, 470 máquinas virtuais em execução.
* As VMs executam cargas de trabalho diferentes e todos os têm a proteção de recuperação de sites ativada.
* Armazenamento para o nó de cluster é fornecido por uma SAN iSCSI. Modelo – Hitachi HUS130.
* Cada servidor de cluster tem quatro placas de rede (NICs) de um Gbps.
* Duas das placas de rede estão ligadas a uma rede privada iSCSI e dois estão ligados a uma rede de enterprise externo. Uma das redes externas está reservada para comunicações de cluster apenas.

![Requisitos de hardware principal](./media/hyper-v-vmm-performance-results/IC744922.png)

| Servidor | RAM | Modelo | Processador | Número de processadores | NIC | Software |
| --- | --- | --- | --- | --- | --- | --- |
| Servidores de Hyper-V num cluster: <br />ESTLAB-HOST11<br />ESTLAB-HOST12<br />ESTLAB-HOST13<br />ESTLAB-HOST14<br />ESTLAB-HOST25 |128ESTLAB-HOST25 has 256 |Dell ™ PowerEdge ™ R820 |Intel CPUs CPU E5-4620 0 @ 2.20GHz |4 |I Gbps x 4 |Windows Server R2 de 2012 Datacenter (x64) + a função Hyper-V |
| Servidor do VMM |2 | | |2 |1 Gbps |Windows Server da base de dados 2012 R2 (x64) + VMM 2012 R2 |

### <a name="secondary-site"></a>Site secundário

* O site secundário tem um cluster de ativação pós-falha de seis nós.
* Armazenamento para o nó de cluster é fornecido por uma SAN iSCSI. Modelo – Hitachi HUS130.

![Especificação de hardware principal](./media/hyper-v-vmm-performance-results/IC744923.png)

| Servidor | RAM | Modelo | Processador | Número de processadores | NIC | Software |
| --- | --- | --- | --- | --- | --- | --- |
| Servidores de Hyper-V num cluster: <br />ESTLAB-HOST07<br />ESTLAB-HOST08<br />ESTLAB-HOST09<br />ESTLAB-HOST10 |96 |Dell ™ PowerEdge ™ R720 |Intel CPUs CPU E5-2630 0 @ 2.30GHz |2 |I Gbps x 4 |Windows Server R2 de 2012 Datacenter (x64) + a função Hyper-V |
| ESTLAB-HOST17 |128 |Dell ™ PowerEdge ™ R820 |Intel CPUs CPU E5-4620 0 @ 2.20GHz |4 | |Windows Server R2 de 2012 Datacenter (x64) + a função Hyper-V |
| ESTLAB-HOST24 |256 |Dell ™ PowerEdge ™ R820 |Intel CPUs CPU E5-4620 0 @ 2.20GHz |2 | |Windows Server R2 de 2012 Datacenter (x64) + a função Hyper-V |
| Servidor do VMM |2 | | |2 |1 Gbps |Windows Server da base de dados 2012 R2 (x64) + VMM 2012 R2 |

### <a name="server-workloads"></a>Cargas de trabalho do servidor

* Para fins de teste, iremos selecionado cargas de trabalho normalmente utilizadas em cenários de cliente da empresa.
* Utilizamos [IOMeter](http://www.iometer.org) com característica a carga de trabalho resumida na tabela de simulação.
* Todos os perfis de IOMeter são definidos para escrita aleatórias bytes para simular worst-case escrever padrões para cargas de trabalho.

| Carga de trabalho | Tamanho de e/s (KB) | % Acesso | % De leitura | E/s pendentes | Padrão de e/s |
| --- | --- | --- | --- | --- | --- |
| Servidor de Ficheiros |48163264 |60%20%5%5%10% |80%80%80%80%80% |88888 |Todos os 100% aleatório |
| SQL Server (volume 1) SQL Server (volume 2) |864 |100%100% |70%0% |88 |% de 100% random100 sequencial |
| Troca |32 |100% |67% |8 |100% aleatório |
| Estação de trabalho/VDI |464 |66%34% |70%95% |11 |Ambos os aleatório 100% |
| Servidor de ficheiros do Web |4864 |33%34%33% |95%95%95% |888 |Todos os 75% aleatório |

### <a name="vm-configuration"></a>Configuração de VM

* 470 VMs no cluster primário.
* Todas as VMs com o disco do VHDX.
* VMs com cargas de trabalho resumidas na tabela. Todos os foram criados com modelos do VMM.

| Carga de trabalho | # VMs | RAM mínima (GB) | RAM máxima (GB) | Tamanho de disco lógico (GB) por VM | IOPS máximo |
| --- | --- | --- | --- | --- | --- |
| SQL Server |51 |1 |4 |167 |10 |
| Servidor do Exchange |71 |1 |4 |552 |10 |
| Servidor de Ficheiros |50 |1 |2 |552 |22 |
| VDI |149 |.5 |1 |80 |6 |
| Servidor Web |149 |.5 |1 |80 |6 |
| TOTAL |470 | | |96.83 TB |4108 |

### <a name="site-recovery-settings"></a>Definições de recuperação do site

* Recuperação de sites foi configurada para no local para a proteção no local
* O servidor VMM tem quatro nuvens configuradas, que contém os servidores de cluster do Hyper-V e as respetivas VMs.

| Nuvem VMM principal | VMs protegidas | Frequência de replicação | Pontos de recuperação adicionais |
| --- | --- | --- | --- |
| PrimaryCloudRpo15m |142 |15 min |Nenhum |
| PrimaryCloudRpo30s |47 |seg 30 |Nenhuma |
| PrimaryCloudRpo30sArp1 |47 |seg 30 |1 |
| PrimaryCloudRpo5m |235 |5 minutos |Nenhum |

### <a name="performance-metrics"></a>Métricas de desempenho

A tabela resume as métricas de desempenho e os contadores que foram avaliados na implementação.

| Métrica | Contador |
| --- | --- |
| CPU |\Processor(_Total)\% Processor Time |
| Memória disponível |MBytes de \Memory\Available |
| IOPS |\PhysicalDisk(_Total)\Disk Transfers/sec |
| Segundo (IOPS) de operações de leitura de VM |Dispositivo de armazenamento virtual \Hyper-V (<VHD>) \Read operações por segundo |
| Operações de escrita (IOPS) de VM/seg |Dispositivo de armazenamento virtual \Hyper-V (<VHD>) \Write operações/S |
| Débito de leitura de VM |Dispositivo de armazenamento virtual \Hyper-V (<VHD>) \Read Bytes/seg |
| Débito de escrita VM |Dispositivo de armazenamento virtual \Hyper-V (<VHD>) \Write Bytes/seg |

## <a name="next-steps"></a>Passos Seguintes

[Configurar a replicação](hyper-v-vmm-disaster-recovery.md)
