---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 533fa1a8491a701571011f407b338e04fb6a7e8b
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/16/2018
ms.locfileid: "40182594"
---
A família VM de série B permite-lhe escolher o tamanho VM fornece-lhe o desempenho de nível de base necessárias para a sua carga de trabalho, a capacidade de desempenho de CPU de rajada de até 100% de um v4 Intel® Broadwell E5-2673 2.3 GHz ou um processador de v3 Intel® Haswell 2.4 GHz E5-2673 vCPU.

As VMs de série B são ideais para cargas de trabalho que não necessita de um desempenho total da CPU continuamente, como servidores web, desenvolvimento e pequenas bases de dados e os ambientes de teste. Estas cargas de trabalho normalmente têm requisitos de desempenho burstable. A série B fornece-lhe a possibilidade de comprar um tamanho de VM com o desempenho de linha de base e a instância VM cria créditos quando está a utilizar menos do que sua linha de base. Quando a VM acumular crédito, a VM pode ultrapassar os limites acima a linha de base com até 100% da vCPU quando a sua aplicação necessitar de desempenho de CPU superior.

A série B possui os seguintes seis tamanhos VM:

| Tamanho          | da vCPU | Memória: GiB | Armazenamento (SSD) temporário GiB | Base de desempenho de CPU da VM | Desempenho de CPU máxima da VM | Créditos Banked / hora | Máx. Banked créditos |
|---------------|--------|-------------|----------------|--------------------------------|---------------------------|-----------------------|--------------------|
| Standard_B1s  | 1      | 1           | 4              | 10%                            | 100%                      | 6                     | 144                |
| Standard_B1ms | 1      | 2           | 4              | 20%                            | 100%                      | 12                    | 288                |
| Standard_B2s  | 2      | 4           | 8              | 40%                            | 200%                      | 24                    | 576                |
| Standard_B2ms | 2      | 8           | 16             | 60%                            | 200%                      | 36                    | 864                |
| Standard_B4ms | 4      | 16          | 32             | 90%                            | 400%                      | 54                    | 1296               |
| Standard_B8ms | 8      | 32          | 64             | 135%                           | 800%                      | 81                    | 1944               |




## <a name="q--a"></a>P&R 

### <a name="q-how-do-you-get-135-baseline-performance-from-a-vm"></a>P: como obtém 135% de desempenho de linha de base de uma VM?
**A**: % a 135 é partilhado entre os 8 Vcpus que compõem o tamanho da VM. Por exemplo, se a sua aplicação utiliza 4 de 8 núcleos a trabalhar no processamento em lotes e cada um desses 4 vCPU executem a 30% de utilização a quantidade total de desempenho de VM CPU seria igual a % de 120.  O que significa que a VM seria criar tempo de crédito com base no delta de 15% do desempenho da linha de base.  Mas isso também significa que quando tem os créditos disponíveis que a mesma VM pode utilizar 100% da vCPU de 8 todas as estamos a dar essa VM um desempenho de CPU máxima de % de 800.


### <a name="q-how-can-i-monitor-my-credit-balance-and-consumption"></a>P: como posso monitorizar o meu saldo de crédito e o consumo
**Uma**: podemos irá apresentar o 2 novas métricas nas próximas semanas, a **crédito** métrica permite-lhe ver quantos créditos a VM tem banked e o **ConsumedCredit** métrica mostrará o número de CPU créditos que sua VM consumiu do banco.    Poderá ver estas métricas a partir do painel de métricas no portal ou através de programação através das APIs de Monitor do Azure.

Para obter mais informações sobre como acessar os dados das métricas para o Azure, consulte [descrição geral das métricas no Microsoft Azure](../articles/monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="q-how-are-credits-accumulated"></a>P: como os créditos são acumulados?
**A**: taxas de acumulação e o consumo da VM são definidas, de modo a que uma VM em execução em exatamente seu nível de desempenho de base terá nem uma acumulação de rede ou o consumo de extrapolação créditos.  Uma VM terá um aumento de net em créditos sempre que estiver em execução abaixo seu nível de desempenho de base e terá uma diminuição líquida créditos sempre que a VM estiver a utilizar a CPU mais do que seu nível de desempenho de base.

**Exemplo**: eu implantar uma VM com o tamanho de B1ms para meu aplicativo de banco de dados de tempo de pequeno e participação. Este tamanho permite que a minha aplicação utilizar até 20% de um vCPU como minha linha de base, o que é.2 créditos por minuto, que posso usar ou banco. 

Meu aplicativo está ocupado no início e fim do dia de trabalho meus funcionários, entre 7:00 - 9:00 e 4: - 6 das 12:00:00. Durante as outras 20 horas do dia, meu aplicativo é, normalmente, em inatividade, usando apenas 10% da vCPU. Para as horas de pico, ganhe créditos 0,2 por minuto, mas só consumir 0.l créditos por minuto, para que a minha VM será bancária.1 x 60 = 6 créditos por hora.  Para 20 horas que estou fora de pico, eu será bancária 120 créditos.  

Durante o horário de pico meu aplicativo calcula a média 60% de utilização de vCPU, eu ainda Ganhe créditos 0,2 por minuto, mas eu consumir 0,6 créditos por minuto, um custo líquido de.4 créditos um minuto ou.4 x 60 = 24 créditos, por hora. Tenho 4 horas por dia de pico do uso de, pelo que custa 24 x 4 = 96 créditos quanto ao meu uso de pico.

Se eu tirar os 120 créditos que obter fora de pico e subtrair os 96 créditos que usei para meu as horas de ponta, eu bancária um créditos adicionais 24 por dia em que posso usar para outros absorver picos de atividade.


### <a name="q-does-the-b-series-support-premium-storage-data-disks"></a>P: a série B suporta discos de dados do armazenamento Premium?
**A**: Sim, todos os tamanhos de série B suportam discos de dados do armazenamento Premium.   
    
### <a name="q-why-is-my-remaining-credit-set-to-0-after-a-redeploy-or-a-stopstart"></a>P: por que motivo é meu crédito restante definido como 0 após uma voltar a implementar ou iniciar/parar?
**A** : quando uma VM é "REDPLOYED" e a VM passa para outro nó, o crédito acumulado é perdido. Se a VM é iniciada/interrompida, mas permanece no mesmo nó, a VM mantém o crédito acumulado. Sempre que a VM entrar atualizada num nó, obtém um crédito inicial, para Standard_B8ms é 240 minutos.

    

    
