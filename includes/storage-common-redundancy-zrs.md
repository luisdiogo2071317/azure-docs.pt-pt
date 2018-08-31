---
title: incluir ficheiro
description: incluir ficheiro
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/11/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 5f7477671e7be4e6e8fd736ab94fa7e7b1e99a6e
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43184644"
---
Armazenamento com redundância de zona (ZRS) replica os dados de forma síncrona em três clusters de armazenamento numa única região. Cada cluster de armazenamento é fisicamente separado dos outros e reside na sua própria zona de disponibilidade (AZ). Cada zona de disponibilidade e o cluster ZRS dentro da mesma, é autónomo, com utilitários separados e capacidades de rede.

Armazenar os seus dados numa conta ZRS garante que será capaz de aceder e gerir os seus dados no caso de uma zona fica indisponível. O ZRS fornece excelente desempenho e baixa latência. O ZRS oferece o mesmo [destinos de escalabilidade](../articles/storage/common/storage-scalability-targets.md) como [armazenamento localmente redundante (LRS)](../articles/storage/common/storage-redundancy-lrs.md).

Considere o ZRS para cenários que exigem uma consistência forte, forte durabilidade e elevada disponibilidade, mesmo que uma indisponibilidade ou desastre natural processa um centro de dados zonal indisponível. O ZRS oferece durabilidade para objetos de armazenamento de, pelo menos, 99.9999999999% (12 9 s) ao longo de um ano.

Para obter mais informações sobre as zonas de disponibilidade, consulte [descrição geral das zonas de disponibilidade](https://docs.microsoft.com/azure/availability-zones/az-overview).

## <a name="support-coverage-and-regional-availability"></a>Cobertura de suporte e a disponibilidade regional
Standard suporta atualmente o ZRS [para fins gerais v2 (GPv2)](../articles/storage/common/storage-account-options.md#general-purpose-v2-accounts) tipos de conta. O ZRS está disponível para blobs de blocos, blobs de páginas sem disco, ficheiros, tabelas e filas. Além disso, todos os seus [a análise de armazenamento](../articles/storage/common/storage-analytics.md) registos e [métricas de armazenamento](../articles/storage/common/storage-enable-and-view-metrics.md)

O ZRS está disponível em geral nas seguintes regiões:

- E.U.A Leste
- Este dos EUA 2
- E.U.A. Oeste 2
- Centro dos EUA
- Europa do Norte
- Europa Ocidental
- França Central
- Sudeste Asiático

A Microsoft continua a ativar o ZRS em mais regiões do Azure. Verifique os [atualizações de serviço do Azure](https://azure.microsoft.com/updates/) página regularmente para obter informações sobre novas regiões.

## <a name="what-happens-when-a-zone-becomes-unavailable"></a>O que acontece quando uma zona não estiver disponível?

Os seus dados permanecem resilientes se uma zona não estiver disponível. A Microsoft recomenda que continue a seguir práticas recomendadas para processamento, como a implementação de políticas de repetição com término exponencial de falhas transitórias. Quando uma zona não estiver disponível, o Azure realiza a atualizações de sistema de rede, como repointing de DNS. Estas atualizações podem afetar seu aplicativo se estão a aceder aos seus dados antes de eles foram concluídas.

O ZRS não pode proteger os seus dados contra um desastre regional em que várias zonas de forma permanente afetadas. Em vez disso, o ZRS oferece resiliência para os seus dados se torna-se temporariamente indisponível. Para proteção contra desastres regionais, a Microsoft recomenda a utilização de armazenamento georredundante (GRS). Para obter mais informações sobre o GRS, consulte [armazenamento georredundante (GRS): replicação de inter-regional do armazenamento do Azure](../articles/storage/common/storage-redundancy-grs.md).

## <a name="converting-to-zrs-replication"></a>Converter para replicação ZRS
Hoje em dia, pode utilizar o portal do Azure ou a API de fornecedor de recursos de armazenamento para alterar o tipo de redundância da sua conta, enquanto estiver a migrar para ou do LRS, GRS e RA-GRS. Com o ZRS, no entanto, migração não é como simples porque envolve o movimento de dados físico de um carimbo de armazenamento única para vários carimbos de data / dentro de uma região. 

Tem duas opções principais para a migração para ou a partir de ZRS. Manualmente, pode copiar ou mover dados para uma nova conta ZRS da sua conta existente. Também pode pedir uma migração em direto. A Microsoft recomenda vivamente que efetue uma migração manual porque não existe nenhuma garantia em relação a quando uma migração em direto será concluída. Uma rota de migração manual proporciona mais flexibilidade do que faz de uma migração em direto, e está no controle do período de tempo da migração.

Para efetuar uma migração manual, terá opções uma variedade:
- Utilize as ferramentas existentes, como o AzCopy, o armazenamento do SDK, ferramentas de terceiros confiáveis, etc.
- Se estiver familiarizado com o Hadoop ou o HDInsight, pode anexar tanto source e conta para o cluster de destino (ZRS) e usar algo como DistCp para paralelizar em massa o processo de cópia de dados
- Criar suas próprias ferramentas tirar partido de um tipo do SDK de armazenamento

Se, no entanto, uma migração manual resultará num período de indisponibilidade de aplicação e não consegue absorver que no final, a Microsoft fornece uma opção de migração em direto. Uma migração em direto é uma migração no local que permite-lhe continuar a utilizar a conta de armazenamento existente, enquanto os dados são migrados entre carimbos de armazenamento de origem e de destino. Durante a migração, ainda terá o mesmo nível de durabilidade e SLA de disponibilidade como faz normalmente.

Migração em direto são fornecidos com determinadas restrições, no entanto. Eles estão listados abaixo.

- Embora a Microsoft irá resolver o seu pedido de migração em direto imediatamente, não há nenhuma garantia em relação a quando a migração será concluída. Se precisar de seus dados no ZRS através de um determinado período de tempo, em seguida, deve fazer uma migração manual. Em geral, quanto mais dados tiver na sua conta, quanto mais tempo demorará a migrar esses dados. 
- Só pode executar uma migração em direto de uma conta com replicação LRS ou GRS. Se a sua conta utilizar RA-GRS, em seguida, terá de migrar primeiro para um dos seguintes tipos de replicação antes de continuar. Este passo intermediário garante que só de leitura ponto final secundário que RA-GRS fornece é removido antes da migração.
- Sua conta tem de conter dados.
- Migrações de intra-região apenas são suportadas. Se pretender migrar os dados para uma conta ZRS localizada numa região diferente da conta de origem, tem de efetuar uma migração manual.
- Tipos de conta de armazenamento standard só são suportados. Não é possível migrar a partir de uma conta de armazenamento premium.

Pedidos enviados através do portal de suporte do Azure de migração em direto. No portal, selecione a conta de armazenamento que pretende converter em ZRS.
1. Clique em **novo pedido de suporte**
2. Verifique se os fundamentos básicos. Clique em **Seguinte**. 
3. Sobre o **problema** secção, 
    - Deixe a gravidade como-é.
    - Tipo de problema = **migração de dados**
    - Categoria = **migre para o ZRS dentro de uma região**
    - Title = **migração de contas ZRS** (ou algo descritivo)
    - Detalhes = gostava de migrar para o ZRS do [LRS, GRS] na região _. 
4. Clique em **Seguinte**.
5. Certifique-se de que as informações de contacto está correta no painel de informações de contacto.
6. Clique em **submeter**.

Um técnico de suporte, em seguida, irá entrar em contacto consigo. Essa pessoa estarão disponível para fornecer qualquer, poderá precisar de assistência. 

## <a name="zrs-classic-a-legacy-option-for-block-blobs-redundancy"></a>O ZRS clássico: Uma opção herdada para redundância de blobs de bloco
> [!NOTE]
> As contas ZRS Classic estão planeadas para preterição e migração necessária no dia 31 de Março de 2021. Microsoft irá enviar mais detalhes aos clientes ZRS Classic antes de preterição. A Microsoft planeja fornecer um processo de migração automatizada de ZRS Classic para ZRS no futuro.

>[!NOTE]
> Assim que estiver ZRS [em disponibilidade geral](#support-coverage-and-regional-availability) numa região, já não será capaz de criar uma conta ZRS clássico a partir do portal nessa mesma região. No entanto, pode ainda criar uma por outros meios, como Microsoft PowerShell e CLI do Azure, ou seja, até que o ZRS Classic foi preterido.

ZRS Classic assincronamente replica os dados em centros de dados dentro de uma ou duas regiões. Uma réplica pode não estar disponível, a menos que a Microsoft inicie a ativação pós-falha para o secundário. O ZRS clássico só está disponível para **blobs de blocos** na [para fins gerais V1 (GPv1)](../articles/storage/common/storage-account-options.md#general-purpose-v1-accounts) contas de armazenamento. Uma conta ZRS Classic não pode ser convertida para ou do LRS ou GRS e não tem as métricas de capacidade de inicio de sessão.

As contas ZRS Classic não podem ser convertidas para ou do LRS, GRS ou RA-GRS. As contas ZRS Classic também não suportam as métricas ou de registo.

Para migrar manualmente os dados da conta ZRS para ou a partir de uma conta LRS, ZRS Classic, GRS ou RA-GRS, utilize o AzCopy, o Explorador de armazenamento do Azure, o Azure PowerShell ou o CLI do Azure. Também pode criar sua própria solução de migração com uma das bibliotecas de cliente do armazenamento do Azure.
