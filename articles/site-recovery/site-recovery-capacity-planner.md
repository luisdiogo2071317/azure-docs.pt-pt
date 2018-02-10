---
title: "Estimar a capacidade de replicação no Azure | Microsoft Docs"
description: "Utilize este artigo para estimar a capacidade quando se replica através da utilização do Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 0a1cd8eb-a8f7-4228-ab84-9449e0b2887b
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/28/2017
ms.author: nisoneji
ms.openlocfilehash: bfeefde53aa2b3645934f068d580c0714714dd69
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="plan-capacity-for-protecting-hyper-v-vms-with-site-recovery"></a>Planear a capacidade para proteger as VMs de Hyper-V com a recuperação de Site

Uma nova versão avançada do [Planeador de implementação do Azure Site Recovery para o Hyper-V para implementação do Azure](site-recovery-hyper-v-deployment-planner.md) está agora disponível. Substitui a ferramenta antiga. Utilize a ferramenta de novo para o planeamento da implementação. A ferramenta fornece as seguintes diretrizes:

* Avaliação de elegibilidade da VM, com base no número de discos, tamanho do disco, IOPS, volume de alterações e alguns características VM
* Tem de largura de banda de rede versus avaliação RPO
* Requisitos de infraestrutura do Azure
* Requisitos de infraestrutura no local
* Replicação inicial orientações de criação de batches
* Estimar o custo de recuperação de desastre total para o Azure


Planeador de capacidade de recuperação de Site do Azure ajuda-o a determinar os requisitos de capacidade quando replicar VMs de Hyper-V com o Azure Site Recovery.

Utilize o planeador de capacidade de recuperação de Site para analisar o seu ambiente de origem e cargas de trabalho. Ajuda a calcular as necessidades de largura de banda, os recursos de servidor que necessita para a localização de origem e os recursos (por exemplo, VMs e armazenamento) tem de na localização de destino.

Pode executar a ferramenta em dois modos:

* **Planear rápida**: fornece projeções de rede e servidor com base num número médio de VMs, discos, armazenamento e a taxa de alteração.
* **Planeamento de detalhado**: fornece detalhes de cada carga de trabalho ao nível da VM. Analisar a compatibilidade da VM e obter projeções de rede e servidor.

## <a name="before-you-start"></a>Antes de começar

* Recolha informações sobre o ambiente, incluindo VMs, discos por VM, de armazenamento por disco.
* Identifica a taxa de alteração (renovação) diária para os dados replicados. Transferir o [ferramenta o planeamento de capacidade do Hyper-V](https://www.microsoft.com/download/details.aspx?id=39057) para obter a taxa de alteração. [Saiba mais](site-recovery-capacity-planning-for-hyper-v-replication.md) sobre esta ferramenta. Recomendamos que esta ferramenta é executada através de uma semana para capturar médias.
   

## <a name="run-the-quick-planner"></a>Executar o planeador rápido
1. Transfira e abra [Planeador de capacidade de recuperação de Site](http://aka.ms/asr-capacity-planner-excel). Terá de executar macros. Quando lhe for pedido, efetue seleções ativar editar e o conteúdo.

2. No **selecione um tipo de planner** caixa de lista, selecione **Planner rápida**.

   ![Introdução](./media/site-recovery-capacity-planner/getting-started.png)

3. No **Capacity Planner** folha de cálculo, introduza as informações necessárias. Preencha todos os campos dentro de um círculo vermelho na captura de ecrã seguinte:

   a. No **selecione o seu cenário**, escolha **Hyper-V para o Azure** ou **VMware/físico para o Azure**.

   b. No **alterações dos dados diários médios taxa (%)**, introduza as informações que recolher utilizando o [ferramenta o planeamento de capacidade do Hyper-V](site-recovery-capacity-planning-for-hyper-v-replication.md) ou [Planeador de implementação de recuperação de Site](./site-recovery-deployment-planner.md). 

   c. O **compressão** definição não é utilizada quando replicar VMs Hyper-V para o Azure. Para compressão, utilize uma aplicação de terceiros, tais como Riverbed.

   d. No **retenção em dias**, especifique em dias quanto tempo pretende manter as réplicas.

   e. No **número de horas em que a replicação inicial para o lote de máquinas virtuais deve concluir** e **número de máquinas virtuais por cada lote de replicação inicial**, introduza as definições que são utilizadas para computação requisitos de replicação inicial. Quando a recuperação de sites é implementada, o conjunto completo de dados inicial é carregado.

   ![Entradas](./media/site-recovery-capacity-planner/inputs.png)

4. Depois de introduzir os valores para o ambiente de origem, a saída apresentada inclui:

   * **Largura de banda necessária para a replicação delta (em Megabits por segundo)**: largura de banda de rede para a replicação de diferenças é calculada sobre a taxa média de alteração de dados diária.
   * **Largura de banda necessária para a replicação inicial (em Megabits por segundo)**: largura de banda de rede para a replicação inicial é calculada nos valores de replicação inicial que introduzir.
   * **Armazenamento necessário (em GB somando)**: O armazenamento do Azure total necessário.
   * **Total de IOPS no armazenamento Standard**: O número é calculado com base no tamanho de unidade IOPS 8 K em contas de armazenamento standard total. Para o planeador rápido, o número é calculado com base em todos os discos VM de origem e a taxa de alteração de dados diárias. Para o planeador de detalhado, o número é calculado com base no número total de VMs que estão mapeadas para standard VMs do Azure e as alterações dos dados taxa dessas VMs.
   * **Número de contas do Standard storage necessário**: O número total de contas do standard storage necessário para proteger as VMs. Uma conta de armazenamento standard pode conter até 20.000 IOPS em todas as VMs no armazenamento standard. É suportado um máximo de 500 IOPS por disco.
   * **Número de discos de Blob necessários**: O número de discos que são criadas no armazenamento do Azure.
   * **Número de contas premium necessário**: O número total de contas de armazenamento premium necessário para proteger as VMs. Uma origem de VM com o IOPS elevado (maior do que 20 000) tem uma conta de armazenamento premium. Uma conta do premium storage pode conter até 80.000 IOPS.
   * **Total de IOPS no armazenamento Premium**: O número é calculado com base no tamanho de unidade IOPS 256 K em contas de armazenamento total premium. Para o planeador rápido, o número é calculado com base em todos os discos VM de origem e a taxa de alteração de dados diárias. Para o planeador de detalhado, o número é calculado com base no número total de VMs que estão mapeadas para as VMs do Azure premium (séries DS e GS) e as alterações dos dados taxa dessas VMs.
   * **Número de servidores de configuração necessários**: mostra o número de servidores de configuração é necessário para a implementação.
   * **Número de servidores de processos adicionais necessários**: mostra se os servidores de processos adicional são necessárias, além do servidor de processo está em execução no servidor de configuração por predefinição.
   * **armazenamento adicional de 100% na origem**: mostra se armazenamento adicional é necessária a localização de origem.

      ![Saída](./media/site-recovery-capacity-planner/output.png)

## <a name="run-the-detailed-planner"></a>Executar o planeador de detalhado

1. Transfira e abra [Planeador de capacidade de recuperação de Site](http://aka.ms/asr-capacity-planner-excel). Terá de executar macros. Quando lhe for pedido, efetue seleções ativar editar e o conteúdo.

2. No **selecione um tipo de planner**, selecione **Planeador de detalhado** da caixa de listagem.

   ![Guia de introdução](./media/site-recovery-capacity-planner/getting-started-2.png)

3. No **qualificação da carga de trabalho** folha de cálculo, introduza as informações necessárias. Terá de preencher os campos marcados.

   a. No **núcleos do processador**, especifique o número total de núcleos num servidor de origem.

   b. No **a atribuição de memória (em MB)**, especifique o tamanho da RAM de um servidor de origem.

   c. No **número de NICs**, especifique o número de adaptadores de rede num servidor de origem.

   d. No **armazenamento Total (em GB)**, especifique o tamanho total do armazenamento VM. Por exemplo, se o servidor de origem tiver três discos com 500 GB, o tamanho de armazenamento total é 1.500 GB.

   e. No **número de discos anexados**, especifique o número total de discos de um servidor de origem.

   f. No **disco a utilização da capacidade (%)**, especifique a utilização média.

   g. No **alterações dos dados diários taxa (%)**, especifique a alteração de dados diárias taxa de um servidor de origem.

   h. No **tamanho da VM do Azure de mapeamento**, introduza o tamanho da VM do Azure que pretende mapear. Se não quiser fazê-lo manualmente, selecione **computação VMs de IaaS**. Se uma definição manual de entrada e, em seguida, selecione **computação VMs de IaaS**, a definição manual pode ser substituída. O processo de computação identifica automaticamente a melhor correspondência no tamanho da VM do Azure.

   ![Folha de cálculo de qualificação de carga de trabalho](./media/site-recovery-capacity-planner/workload-qualification.png)

4. Se selecionar **computação VMs de IaaS**, eis o que faz:

   * Valida as entradas obrigatórias.
   * Calcula o IOPS e sugere a melhor correspondência de tamanho de VM do Azure para cada VM que seja elegível para replicação no Azure. Se uma VM do Azure não é possível detetar um tamanho adequado, apresenta um erro. Por exemplo, se o número de discos anexados 65, apresenta um erro porque o tamanho maior de uma VM do Azure é 64.
   * Sugere uma conta de armazenamento que pode ser utilizada para uma VM do Azure.
   * Calcula o número total de contas do standard storage e contas de armazenamento premium necessárias para a carga de trabalho. Desloque para baixo para ver o tipo de armazenamento do Azure e a conta de armazenamento que pode ser utilizada para um servidor de origem.
   * Realiza e ordena o resto da tabela consoante o tipo de armazenamento necessária (standard ou premium) atribuído para uma VM e o número de discos anexados. Para todas as VMs que cumprem os requisitos do Azure, a coluna **VM é qualificado?** mostra **Sim**. Se uma VM não pode ser feita para o Azure, apresenta um erro.

Colunas AA para AE são produzidas e fornecem informações para cada VM.

![Colunas de saída AA para AE](./media/site-recovery-capacity-planner/workload-qualification-2.png)

### <a name="example"></a>Exemplo
Por exemplo, para seis VMs com os valores apresentados na tabela, a ferramenta calcula e atribui a melhor correspondência de VM do Azure e os requisitos de armazenamento do Azure.

![Atribuições de qualificação da carga de trabalho](./media/site-recovery-capacity-planner/workload-qualification-3.png)

* No exemplo de saída, tenha em atenção o seguinte:

  * A primeira coluna é uma coluna de validação para as VMs, discos e volume de alterações.
  * Duas contas do standard storage e conta de armazenamento um premium são necessárias para as cinco VMs.
  * VM3 não se qualificar para a proteção porque um ou mais discos são mais de 1 TB.
  * VM1 e VM2 podem utilizar a primeira conta de armazenamento standard
  * VM4 pode utilizar a segunda conta de armazenamento standard.
  * VM5 e VM6 precisam de uma conta de armazenamento premium, e ambos podem utilizar uma única conta.

    > [!NOTE]
    > IOPS no storage standard e premium são calculadas ao nível da VM e não ao nível do disco. Um padrão VM pode processar até 500 IOPS por disco. Se o IOPS para um disco for maiores do que 500, precisa de armazenamento premium. Se o IOPS para um disco estiver mais do que 500, mas IOPS para os discos da VM totais respeitam os limites do suporte padrão VM do Azure, escolhe o planeador de uma VM padrão e não a série DS ou GS. (Os limites de VM do Azure estão tamanho da VM, número de discos, número de adaptadores, CPU e memória). Terá de atualizar manualmente a célula de tamanho do Azure de mapeamento com o adequado série DS ou GS VM.


Depois de todas as informações são introduzidas, selecione **submeter dados à ferramenta planner** para abrir o planeador de capacidade. Cargas de trabalho são realçadas para mostrar se estiverem elegíveis para proteção.

### <a name="submit-data-in-capacity-planner"></a>Submissão de dados no Planeador de capacidade
1. Ao abrir o **Capacity Planner** folha de cálculo, é preenchido com base nas definições que especificou. A palavra "Carga de trabalho" aparece no **origem de entradas de Infra** célula para mostrar que a entrada é o **qualificação da carga de trabalho** folha de cálculo.

2. Se pretender efetuar alterações, tem de modificar o **qualificação da carga de trabalho** folha de cálculo. Em seguida, selecione **submeter dados à ferramenta planner** novamente. 

   ![Capacity Planner](./media/site-recovery-capacity-planner/capacity-planner.png)

## <a name="next-steps"></a>Passos Seguintes
[Saiba como executar](site-recovery-capacity-planning-for-hyper-v-replication.md) a ferramenta de planeamento da capacidade.
