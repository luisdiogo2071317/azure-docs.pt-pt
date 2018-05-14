---
title: incluir ficheiro
description: incluir ficheiro
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/26/2018
ms.author: jeking
ms.custom: include file
ms.openlocfilehash: 0347d6ca951b75c385b138487f58b85a809c6805
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
---
Armazenamento redundante de zona (ZRS) em sincronia replica os dados em clusters de armazenamento 3 (três) numa única região. Cada cluster de armazenamento é fisicamente separado de outros e reside na sua própria zona de disponibilidade (AZ). Cada zona de disponibilidade e o cluster ZRS dentro da mesma, é autónomo, com diferentes utilitários e capacidades de rede.

Armazenar os dados numa conta ZRS garante que irá ser capaz de acesso e gerir os seus dados no caso de uma zona fica indisponível. O ZRS fornece excelente desempenho e a latência extremamente baixa. Na verdade, o ZRS tem o mesmo [metas de escalabilidade](../articles/storage/common/storage-scalability-targets.md) como LRS.

Considere o ZRS para cenários que requerem a consistência forte, forte durabilidade e elevada disponibilidade, mesmo se uma falha ou desastre natural composições um centro de dados zonal indisponível. O ZRS oferece durabilidade para objetos de armazenamento de, pelo menos, 99.9999999999% (12 do 9) ao longo de um ano específico.

Para obter mais informações sobre as zonas de disponibilidade, consulte [descrição geral da disponibilidade zonas](https://docs.microsoft.com/azure/availability-zones/az-overview).

## <a name="support-coverage-and-regional-availability"></a>Cobertura de suporte e disponibilidade regional
Atualmente, suporta o ZRS [ **padrão, para fins gerais v2 (GPv2)** ](../articles/storage/common/storage-account-options.md#general-purpose-v2) tipos de conta. O ZRS está disponível para blobs de blocos, blobs de páginas de disco não, ficheiros, tabelas e filas. Além disso, todos os seus [análise de armazenamento](../articles/storage/common/storage-analytics.md) registos e [as métricas do Storage](../articles/storage/common/storage-enable-and-view-metrics.md)

O ZRS é **geralmente disponível** nas seguintes regiões:

- Este dos EUA 2
- Centro dos EUA
- Europa do Norte
- Europa Ocidental
- Centro de França
- Sudeste Asiático

A Microsoft continua a ativar o ZRS noutras regiões do Azure e, atualizaremos esta lista quando isso ocorrer. Também iremos essas anúncios através dos canais padrão, tais como o [atualizações de serviço do Azure](https://azure.microsoft.com/updates/) notificações de e-mail e de página para proprietários de subscrição do Azure e administradores.

## <a name="what-happens-when-a-zone-becomes-unavailable"></a>O que acontece quando uma zona fica indisponível?

Os dados permanecerão resilientes se uma zona ficar indisponível. A Microsoft recomenda que continue a seguir práticas para falhas transitórias processar, tais como implementar as políticas de repetição com término exponencial. Quando uma zona não estiver disponível, o Azure undertakes atualizações redes, tais como repointing de DNS. Estas atualizações podem afetar a aplicação se estão a aceder aos seus dados antes de poderem tem concluído.

O ZRS não pode proteger os seus dados contra desastres regional em várias zonas permanentemente são afetadas. Em vez disso, o ZRS oferece resiliência para os seus dados no caso de indisponibilidade temporal. Para proteção contra desastres inesperados regionais, a Microsoft recomenda a utilização [armazenamento georredundante (GRS): a replicação entre regionais para armazenamento do Azure](../articles/storage/common/storage-redundancy-grs.md).

## <a name="converting-to-zrs-replication"></a>Converter para replicação ZRS
Hoje, é pretty simples alternar entre LRS, GRS e RA-GRS - tirar partido do portal ou a API. Com o ZRS, no entanto, não é como straightforward porque envolve o movimento de dados físico de um carimbo de armazenamento única para vários carimbos dentro de uma região. Como tal, tem duas opções principais - manualmente copiar/mover dados para uma nova conta ZRS da sua conta existente ou pedir uma migração em direto. Recomendamos vivamente que efetue uma migração manual porque não nos é possível garantir quando uma migração em direto será concluída; Existem muitos fatores que diretamente e indiretamente afetam a conclusão de uma tarefa de migração. 

Para efetuar uma migração manual, tem opções uma variedade:
- Utilize ferramentas existentes, como o AzCopy, o armazenamento SDK, as ferramentas de terceiros fiáveis, etc.
- Se estiver familiarizado com o HDInsight ou do Hadoop, pode anexar ambas origem e destino (ZRS) conta para o cluster e utilizar algo semelhante DistCp para parallelize em massa o processo de cópia de dados
- Criar as suas próprias ferramentas tirar partido de uma versão do SDK de armazenamento

Conforme mencionado anteriormente, recomendamos vivamente que aceda a rota de migração manual porque dá-lhe uma maior flexibilidade do que uma migração em direto. Também são no controlo total sobre quando ocorre a migração.

Se, no entanto, uma migração manual resultará em algum período de indisponibilidade de aplicações e conseguir absorver que na sua extremidade, fornecemos uma opção de migração em direto. Uma migração em direto é uma migração no local que permite-lhe continuar a utilizar a conta de armazenamento existente, enquanto os dados são migrados entre carimbos de armazenamento de origem e de destino. Durante a migração, ainda terá o mesmo nível de durabilidade e disponibilidade SLA tal como é normalmente.

Migração em direto vêm com determinadas restrições, no entanto. Estes são apresentados abaixo.

- Enquanto o pedido de migração em direto irão abordar retomadas rapidamente, não é possível garantimos quando a migração, na verdade, irá concluir. Se necessitar dos dados no ZRS através de um determinado período de tempo, em seguida, deve efetuar uma migração manual. Geralmente, os dados mais tem na sua conta, mais tempo demorará para migrar os dados. 
- Pode migrar apenas em direto a uma conta com replicação LRS e GRS. Se tiver o RA-GRS, em seguida, terá primeiro altere para um dos seguintes tipos de replicação antes de continuar. Este passo intermediário assegura que o modo só de leitura ponto final secundário que fornece de RA-GRS é removido quando estiver preparado.
- A conta tem de ser não vazio.
- Migrações de região intra só são suportadas. Se pretender migrar os dados para uma conta ZRS localizada numa região diferente da conta de origem, tem de efetuar uma migração manual.
- Armazenamento Standard tipos de conta apenas. Não é possível migrar a partir de uma conta de armazenamento Premium.

Pedidos de aceder através do portal de suporte do Azure de migração em direto. No portal, selecione a conta de armazenamento que pretende converter ZRS.
1. Clique em **novo pedido de suporte**
2. Certifique-se as noções básicas. Clique em **Seguinte**. 
3. No **problema** secção, 
    - Deixe a gravidade como-é.
    - Tipo de problema = **migração de dados**
    - Categoria = **migrar para o ZRS numa região**
    - Título = **ZRS conta migração** (ou algo descritivo)
    - Detalhes = gostaria de migrar para o ZRS do [LRS, GRS] na região ___. 
4. Clique em **Seguinte**.
5. Certifique-se de que as informações de contacto está correta no painel de informações de contacto.
6. Clique em **submeter**.

Uma pessoa de suporte, em seguida, será in contact with lhe. Essa pessoa estará disponível para fornecer qualquer pode necessitar de assistência. 

## <a name="zrs-classic-a-legacy-option-for-block-blobs-redundancy"></a>O ZRS clássico: Uma opção legada para a redundância de blobs de blocos
> [!NOTE]
> Contas ZRS clássico estejam planeadas para preterição e migração necessária no 31 de Março de 2021. A Microsoft enviará mais detalhes para os clientes ZRS clássico antes de preterição. Microsoft planos fornecer um processo de migração automática do ZRS clássico para ZRS no futuro.

>[!NOTE]
> Depois do ZRS [geralmente disponível](#support-coverage-and-regional-availability) numa região, já não será capaz de criar uma conta clássica ZRS a partir do portal nessa mesma região. No entanto, pode ainda criar uma através de outros meios, como o Microsoft PowerShell e a CLI do Azure, ou seja, até que o ZRS clássico foi preterido.

O ZRS clássico replica de forma assíncrona dados em todos os centros de dados dentro de uma ou duas regiões. Uma réplica pode não estar disponível, a menos que a Microsoft inicie a ativação pós-falha para o secundário. O ZRS clássico só está disponível para **blobs de blocos** no [V1 para fins gerais (GPv1)](../articles/storage/common/storage-account-options.md#general-purpose-v1) contas de armazenamento. Uma conta ZRS Classic não pode ser convertida para ou do LRS ou GRS e não tem as métricas de capacidade de inicio de sessão.

Não não possível converter o ZRS clássico contas para ou do LRS, GRS ou RA-GRS. Contas ZRS clássico também não suportam as métricas ou de registo.

Para migrar manualmente os dados de conta de ZRS para ou a partir de uma conta LRS, ZRS clássico, GRS ou RA-GRS, utilize o AzCopy, o Explorador de armazenamento do Azure, o Azure PowerShell ou o CLI do Azure. Também pode criar a sua própria solução de migração com uma das bibliotecas de cliente do Storage do Azure.
