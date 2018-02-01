---
title: "Replicação de dados no armazenamento do Azure | Microsoft Docs"
description: "Os dados na sua conta de armazenamento do Microsoft Azure são replicados para durabilidade e elevada disponibilidade. As opções de replicação incluem o armazenamento localmente redundante (LRS), o armazenamento com redundância de zona (ZRS), o armazenamento georredundante (GRS) e o armazenamento georredundante com acesso de leitura (RA-GRS)."
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.workload: storage
ms.topic: article
ms.date: 01/21/2018
ms.author: tamram
ms.openlocfilehash: 18d0e8bc6cc1559f9ae1a1a4457aa85d2a206597
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2018
---
# <a name="azure-storage-replication"></a>Replicação do Storage do Azure

Os dados na sua conta de armazenamento do Microsoft Azure são sempre replicados para garantir a durabilidade e a elevada disponibilidade. Replicação copia os dados para que este se encontra protegido contra falhas de hardware transitórias, preservando a sua aplicação de tempo. 

Pode optar por replicar os dados no mesmo centro de dados, em todos os centros de dados na mesma região ou em regiões. Se os dados são replicados em múltiplos centros de dados ou em regiões, também estão protegido contra uma falha catastrófica numa única localização.

A replicação garante que a sua conta de armazenamento cumpre o [Contrato de Nível de Serviço (SLA) para o Armazenamento](https://azure.microsoft.com/support/legal/sla/storage/) mesmo em caso de falhas. Leia o SLA para obter mais informações sobre as garantias do Armazenamento do Azure quanto à durabilidade e à disponibilidade.

Quando cria uma conta de armazenamento, tem de selecionar uma das seguintes opções de replicação:

* [Armazenamento localmente redundante (LRS)](#locally-redundant-storage)
* [Armazenamento com redundância de zona (ZRS)](#zone-redundant-storage)
* [Armazenamento georredundante (GRS)](#geo-redundant-storage)
* [Armazenamento georredundante com acesso de leitura (RA-GRS)](#read-access-geo-redundant-storage)

Armazenamento georredundante com acesso de leitura (RA-GRS) é a opção predefinida quando cria uma conta de armazenamento.

A tabela seguinte fornece uma descrição geral rápida das diferenças entre LRS, ZRS, GRS e RA-GRS. As seções subsequentes deste artigo abordar cada tipo de replicação mais detalhadamente.

| Estratégia de replicação | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| Os dados são replicados em vários datacenters. |Não |Sim |Sim |Sim |
| Podem ler dados a partir de uma localização secundária, bem como a localização primária. |Não |Não |Não |Sim |
| Concebida para fornecer ___ durabilidade dos objetos através de um ano específico. |pelo menos 99.999999999% (11 da 9)|pelo menos 99.9999999999% (12 do 9)|pelo menos 99.99999999999999% (16 do 9)|pelo menos 99.99999999999999% (16 do 9)|

Consulte [preços do Storage do Azure](https://azure.microsoft.com/pricing/details/storage/) para informações sobre preços para as opções de redundância diferentes.

> [!NOTE]
> Armazenamento Premium suporta armazenamento apenas localmente redundante (LRS). Para obter informações sobre o Premium Storage, consulte [Premium Storage: armazenamento de elevado desempenho para cargas de trabalho de Máquina Virtual de Azure](../../virtual-machines/windows/premium-storage.md).
>

## <a name="locally-redundant-storage"></a>Armazenamento localmente redundante
[!INCLUDE [storage-common-redundancy-LRS](../../../includes/storage-common-redundancy-LRS.md)]

## <a name="zone-redundant-storage"></a>Armazenamento com redundância de zona
[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-ZRS.md)]

### <a name="zrs-classic-accounts"></a>Contas ZRS clássico

A capacidade ZRS existente é agora é denominada ZRS clássico. O ZRS clássico contas estão disponíveis apenas para blobs de blocos em contas de armazenamento para fins gerais V1. 

O ZRS clássico replica os dados no modo assíncrono entre centros de dados dentro de uma ou duas regiões. Uma réplica poderão não estar disponível, a menos que a Microsoft inicia a ativação pós-falha para o secundário. 

Não não possível converter o ZRS clássico contas para ou do LRS, GRS ou RA-GRS. Contas ZRS clássico também não suportam as métricas ou de registo.   

Assim que o ZRS estiver geralmente disponível numa região, já não será capaz de criar uma conta clássica ZRS a partir do portal nessa região, mas é possível criar um através de outros meios.  
Um processo de migração automática do ZRS clássico para o ZRS será fornecido no futuro.

Pode migrar manualmente os dados de conta de ZRS para ou a partir de uma conta LRS, ZRS clássico, GRS ou RAGRS. Pode efetuar esta migração manual utilizando o AzCopy, o Explorador de armazenamento do Azure, Azure PowerShell, CLI do Azure ou uma das bibliotecas de cliente do Storage do Azure.

> [!NOTE]
> Contas ZRS clássico estejam planeadas para preterição e migração necessária no 31 de Março de 2021. A Microsoft enviará mais detalhes para os clientes ZRS clássico antes de preterição.

Perguntas adicionais são tratadas o [perguntas mais frequentes](#frequently-asked-questions) secção. 

## <a name="geo-redundant-storage"></a>Armazenamento georredundante
[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-GRS.md)]

## <a name="read-access-geo-redundant-storage"></a>Armazenamento georredundante com acesso de leitura
Armazenamento georredundante com acesso de leitura (RA-GRS) maximiza a disponibilidade para a sua conta de armazenamento. RA-GRS fornece acesso só de leitura aos dados na localização secundária, além de georreplicação em duas regiões.

Quando ativar o acesso só de leitura aos seus dados na região secundária, os dados estão disponíveis um ponto final secundário, bem como o ponto final principal para a sua conta de armazenamento. O ponto final secundário é semelhante para o ponto final principal, mas acrescenta o sufixo `–secondary` ao nome da conta. Por exemplo, se o ponto final principal para o serviço Blob é `myaccount.blob.core.windows.net`, em seguida, o ponto final secundário é `myaccount-secondary.blob.core.windows.net`. As chaves de acesso para a sua conta de armazenamento são as mesmas para os pontos de finais primários e secundários.

Algumas considerações a lembrar ao utilizar o RA-GRS:

* A aplicação tem de gerir o ponto final de interagir com ao utilizar o RA-GRS.
* Uma vez que a replicação assíncrona envolve um atraso, as alterações que ainda não tem sido replicadas para a região secundária podem ser perdidas se não não possível recuperar os dados da região primária, por exemplo na eventualidade de ocorrer um desastre regional.
* Se o Microsoft inicia a ativação pós-falha para a região secundária, será leu e acesso de escrita para que os dados após a ativação pós-falha foi concluída. Para obter mais informações, consulte [orientações de recuperação após desastre](../storage-disaster-recovery-guidance.md).
* RA-GRS destina-se a fins de elevada disponibilidade. Para obter orientações de escalabilidade, reveja o [lista de verificação de desempenho](../storage-performance-checklist.md).

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

<a id="howtochange"></a>
#### <a name="1-how-can-i-change-the-geo-replication-type-of-my-storage-account"></a>1. Como posso alterar o tipo de replicação geográfica da minha conta de armazenamento?

   Pode alterar o tipo de replicação geográfica da conta de armazenamento utilizando o [portal do Azure](https://portal.azure.com/), [Azure Powershell](storage-powershell-guide-full.md), ou uma das bibliotecas de cliente do Storage do Azure.

   > [!NOTE]
   > Contas ZRS não podem ser convertida LRS ou GRS. Da mesma forma, uma conta existente do LRS ou GRS não pode ser convertida para uma conta ZRS.
    
<a id="changedowntime"></a>
#### <a name="2-does-changing-the-replication-type-of-my-storage-account-result-in-down-time"></a>2. Funciona para alterar o tipo de replicação do meu resultado de conta de armazenamento no tempo?

   Não, alterar o tipo de replicação da conta de armazenamento não resulta num tempo.

<a id="changecost"></a>
#### <a name="3-are-there-additional-costs-to-changing-the-replication-type-of-my-storage-account"></a>3. Existem custos adicionais para alterar o tipo de replicação da minha conta de armazenamento?

   Sim. Se mudar do LRS para o GRS (ou RA-GRS) para a sua conta de armazenamento, cobrado um custo adicional de saída ao copiar dados existentes de localização primária para a localização secundária. Depois dos dados iniciais são copiados, não existem nenhum encargos associados à mais saída adicionais para a georreplicação do primário para a localização secundária. Para obter mais detalhes em custos de largura de banda, consulte [página de preços do Storage do Azure](https://azure.microsoft.com/pricing/details/storage/blobs/).

   Se alterar o GRS para LRS, há sem custos adicionais, mas os dados são eliminados da localização secundária.

<a id="ragrsbenefits"></a>
#### <a name="4-how-can-ra-grs-help-me"></a>4. Como é que o RA-GRS-me pode ajudar?

   Armazenamento GRS fornece replicação dos seus dados de um site primário para uma região secundária que é centenas de quilómetros região primária. Com a GRS, os seus dados são duráveis, mesmo em caso de uma falha regional completa ou de um desastre no qual a região primária não é recuperável. Armazenamento RA-GRS oferece replicação GRS e adiciona a capacidade de ler os dados da localização secundária. Para sugestões sobre como conceber para elevada disponibilidade, consulte [conceber altamente disponível aplicações utilizando o armazenamento RA-GRS](../storage-designing-ha-apps-with-ragrs.md).

<a id="lastsynctime"></a>
#### <a name="5-is-there-a-way-to-figure-out-how-long-it-takes-to-replicate-my-data-from-the-primary-to-the-secondary-region"></a>5. Existe alguma forma para perceber quanto tempo demora para replicar os meus dados a partir de principal para a região secundária?

   Se estiver a utilizar o armazenamento RA-GRS, pode verificar a hora da última sincronização da sua conta de armazenamento. Hora da última sincronização é um valor de data/hora GMT. Todas as escritas primárias antes da hora da última sincronização tem sido escrita com êxito na localização secundária, o que significa que estão disponíveis para ler a partir da localização secundária. Escreve principal após a hora da última sincronização pode ou não estar disponível para leituras ainda. Pode consultar este valor a utilizar o [portal do Azure](https://portal.azure.com/), [Azure PowerShell](storage-powershell-guide-full.md), ou uma das bibliotecas de cliente do Storage do Azure.

<a id="outage"></a>
#### <a name="6-if-there-is-an-outage-in-the-primary-region-how-do-i-switch-to-the-secondary-region"></a>6. Se existir uma falha na região primária, como posso alternar para a região secundária?

   Para obter mais informações, consulte [o que fazer se ocorrer uma falha de armazenamento do Azure](../storage-disaster-recovery-guidance.md).

<a id="rpo-rto"></a>
#### <a name="7-what-is-the-rpo-and-rto-with-grs"></a>7. Qual é o RPO e RTO com a GRS?

   **Objetivo de ponto de recuperação (RPO):** no GRS e RA-GRS, o armazenamento no modo assíncrono service georreplicação-são replicados os dados a partir de principal para a localização secundária. Em caso de desastre grave regional na região primária, a Microsoft efetua uma ativação pós-falha para a região secundária. Se uma ativação pós-falha acontecer, as alterações recentes à que não ainda foram georreplicação poderão perder-se. O número de minutos de potenciais dados perdidos é referido como o RPO e indica o ponto no tempo para que os dados podem ser recuperados. Normalmente, o Storage do Azure tem um RPO de menos de 15 minutos, apesar de não existe atualmente nenhum SLA no georreplicação quanto tempo demora.

   **Objetivo de tempo de recuperação (RTO):** o RTO é uma medida do tempo que demora para efetuar a ativação pós-falha e obter a conta de armazenamento online. O tempo para efetuar a ativação pós-falha inclui as seguintes ações:

   * A hora exige o Microsoft para determinar se os dados podem ser recuperados na localização principal ou se uma ativação pós-falha é necessária.
   * O tempo para efetuar a ativação pós-falha da conta do storage alterando as entradas DNS principais para apontar para a localização secundária.

   Microsoft assume a responsabilidade de preservar os seus dados muito a sério. Se não houver qualquer possibilidade de recuperar os dados na região primária, o Microsoft atrasar a ativação pós-falha e concentrar-se na recuperar os dados. Versão futura do serviço permitirá acionar uma ativação pós-falha ao nível de uma conta para que pode controlar o RTO por si.

#### <a name="8-what-azure-storage-objects-does-zrs-support"></a>8. Os objetos de armazenamento do Azure suporta ZRS? 
Os blobs de blocos, blobs de páginas (exceto para os discos VM de cópia de segurança), tabelas, ficheiros e filas. 

#### <a name="9-does-zrs-also-include-geo-replication"></a>9. O ZRS também incluir georreplicação? 
O ZRS não suporta atualmente a georreplicação. Se o seu cenário necessitar de replicação por várias regiões para fins de recuperação após desastre, em vez disso, a utilizar uma conta de armazenamento GRS ou RA-GRS.   

#### <a name="10-what-happens-when-one-or-more-zrs-zones-go-down"></a>10. O que acontece quando um ou mais zonas ZRS ficarem Inativos? 
Quando a primeira zona fica inativo, o ZRS continua a escrever réplicas dos dados entre as duas zonas restantes na região. Se uma segunda zona ficar inativo, acesso de escrita de leitura e não está disponível até, pelo menos, duas zonas estão disponíveis novamente. 

## <a name="next-steps"></a>Passos Seguintes
* [Ao conceber aplicações altamente disponíveis a utilizar o armazenamento RA-GRS](../storage-designing-ha-apps-with-ragrs.md)
* [Preços do Armazenamento do Azure](https://azure.microsoft.com/pricing/details/storage/)
* [Sobre as contas de armazenamento do Azure](../storage-create-storage-account.md)
* [Azure metas de desempenho e escalabilidade de armazenamento](storage-scalability-targets.md)
* [Armazenamento do Microsoft Azure redundância opções e acesso de leitura georreplicação armazenamento redundantes](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
* [Sosp - Storage do Azure: Um elevada serviço em nuvem armazenamento com consistência forte](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)
