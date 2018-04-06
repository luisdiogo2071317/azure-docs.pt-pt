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
ms.openlocfilehash: f6d437e4ad0e05d55c408ad8f9defe5385b52050
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
Armazenamento redundante de zona (ZRS) em sincronia replica os dados em três clusters de armazenamento numa única região. Cada cluster de armazenamento é fisicamente separado de outros e reside na sua própria zona de disponibilidade (AZ). Cada zona de disponibilidade e o cluster ZRS dentro da mesma, é autónomo, com diferentes utilitários e capacidades de rede.

Armazenar os dados numa conta ZRS garante que irá ser capaz de acesso e gerir os seus dados no caso de uma zona fica indisponível. O ZRS fornece excelente desempenho e a latência extremamente baixa.

Para obter mais informações sobre as zonas de disponibilidade, consulte [descrição geral da disponibilidade zonas](https://docs.microsoft.com/azure/availability-zones/az-overview).

## <a name="zrs-for-high-availability"></a>ZRS para elevada disponibilidade 

Considere o ZRS para cenários que requerem a consistência forte, forte durabilidade e elevada disponibilidade, mesmo se uma falha ou desastre natural composições um centro de dados disponível. O ZRS oferece durabilidade para objetos de armazenamento de, pelo menos, 99.9999999999% (12 do 9) ao longo de um ano específico.

Atualmente, o ZRS suporta v2 padrão, para fins gerais tipos de conta (GPv2). O ZRS está disponível para blobs de blocos, blobs de páginas de disco não, ficheiros, tabelas e filas. 

O ZRS está normalmente disponível em regiões do seguintes:

- Este dos EUA 2
- Centro dos EUA
- Europa do Norte
- Europa Ocidental
- Centro de França
- Sudeste Asiático

Microsoft continua a ativar o ZRS noutras regiões do Azure.

### <a name="what-happens-when-a-zone-becomes-unavailable"></a>O que acontece quando uma zona fica indisponível?

Os dados permanecerão resilientes se uma zona ficar indisponível. A Microsoft recomenda que continue a seguir práticas para falhas transitórias processar, tais como implementar as políticas de repetição com término exponencial. Quando uma zona não estiver disponível, o Azure undertakes atualizações redes, tais como repointing de DNS. Estas atualizações podem afetar a aplicação se estão a aceder aos seus dados antes de poderem tem concluído.

O ZRS não pode proteger os seus dados contra desastres regional em várias zonas permanentemente são afetadas. Em vez disso, o ZRS oferece resiliência para os seus dados no caso de indisponibilidade temporal. Para proteção contra desastres inesperados regionais, a Microsoft recomenda a utilização [armazenamento georredundante (GRS): a replicação entre regionais para armazenamento do Azure](../articles/storage/common/storage-redundancy-grs.md).

## <a name="zrs-classic-a-legacy-option-for-block-blobs-redundancy"></a>O ZRS clássico: Uma opção legada para a redundância de blobs de blocos
> [!NOTE]
> Contas ZRS clássico estejam planeadas para preterição e migração necessária no 31 de Março de 2021. A Microsoft enviará mais detalhes para os clientes ZRS clássico antes de preterição. Microsoft planos fornecer um processo de migração automática do ZRS clássico para ZRS no futuro.

O ZRS clássico só está disponível para blobs de blocos numa V1 para fins gerais contas de armazenamento (GPv1). O ZRS clássico replica de forma assíncrona dados em todos os centros de dados dentro de uma ou duas regiões. Uma réplica pode não estar disponível, a menos que a Microsoft inicie a ativação pós-falha para o secundário. Uma conta ZRS Classic não pode ser convertida para ou do LRS ou GRS e não tem as métricas de capacidade de inicio de sessão.

Não não possível converter o ZRS clássico contas para ou do LRS, GRS ou RA-GRS. Contas ZRS clássico também não suportam as métricas ou de registo.

Assim que o ZRS estiver geralmente disponível numa região, já não será capaz de criar uma conta clássica ZRS a partir do portal nessa região, mas é possível criar um através de outros meios, como o PowerShell, CLI e assim sucessivamente.

Para migrar manualmente os dados de conta de ZRS para ou a partir de uma conta LRS, ZRS clássico, GRS ou RA-GRS, utilize o AzCopy, o Explorador de armazenamento do Azure, o Azure PowerShell ou o CLI do Azure. Também pode criar a sua própria solução de migração com uma das bibliotecas de cliente do Storage do Azure.