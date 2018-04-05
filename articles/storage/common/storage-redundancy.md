---
title: Replicação de dados no armazenamento do Azure | Microsoft Docs
description: Os dados na sua conta de armazenamento do Microsoft Azure são replicados para durabilidade e elevada disponibilidade. As opções de replicação incluem o armazenamento localmente redundante (LRS), o armazenamento com redundância de zona (ZRS), o armazenamento georredundante (GRS) e o armazenamento georredundante com acesso de leitura (RA-GRS).
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 01/21/2018
ms.author: tamram
ms.openlocfilehash: bdb9bfaa85f526af0c5e42294a75664fa7137849
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="azure-storage-replication"></a>Replicação do Storage do Azure

Os dados na sua conta de armazenamento do Microsoft Azure são sempre replicados para garantir a durabilidade e a elevada disponibilidade. Replicação do Storage do Azure copia os dados para que este se encontra protegido contra planeados e imprevistos eventos de falhas de hardware transitórias, rede ou falhas de energia, perante desastres naturais enormes e assim sucessivamente. Pode optar por replicar os dados no mesmo centro de dados, em todos os centros de dados zonal na mesma região e até mesmo em regiões.

A replicação garante que a sua conta de armazenamento cumpre o [Contrato de Nível de Serviço (SLA) para o Armazenamento](https://azure.microsoft.com/support/legal/sla/storage/) mesmo em caso de falhas. Leia o SLA para obter mais informações sobre as garantias do Armazenamento do Azure quanto à durabilidade e à disponibilidade.

## <a name="choosing-a-replication-option"></a>Escolher uma opção de replicação

Quando cria uma conta de armazenamento, tem de selecionar uma das seguintes opções de replicação:

* [Armazenamento localmente redundante (LRS)](storage-redundancy-lrs.md)
* [Armazenamento com redundância de zona (ZRS)](storage-redundancy-zrs.md)
* [Armazenamento georredundante (GRS)](storage-redundancy-grs.md)
* [Armazenamento georredundante com acesso de leitura (RA-GRS)](storage-redundancy-grs.md#read-access-geo-redundant-storage)

A tabela seguinte fornece uma rápida descrição geral do âmbito de durabilidade e disponibilidade que cada estratégia de replicação irá fornecer-lhe para um determinado tipo de evento (ou evento de impacto semelhante).

| Cenário | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| Indisponibilidade de nó dentro de um centro de dados |Sim |Sim |Sim |Sim
| Um centro de dados completo (zonal ou não zonal) fica indisponível |Não |Sim |Sim |Sim |
| Uma falha de toda a região |Não |Não |Sim |Sim |
| Acesso de leitura aos seus dados (numa região remoto, georreplicação) em caso de indisponibilidade de toda a região |Não |Não |Não |Sim |
| Concebida para fornecer ___ durabilidade dos objetos através de um ano específico |pelo menos 99.999999999% (11 da 9)|pelo menos 99.9999999999% (12 do 9)|pelo menos 99.99999999999999% (16 do 9)|pelo menos 99.99999999999999% (16 do 9)|
| Disponível em tipos de conta de armazenamento ___ |GPv1, GPv2, Blob |GPv2 |GPv1, GPv2, Blob |GPv1, GPv2, Blob

Consulte [preços do Storage do Azure](https://azure.microsoft.com/pricing/details/storage/) para informações sobre as opções de redundância diferentes de preços.

> [!NOTE]
> Armazenamento Premium suporta armazenamento apenas localmente redundante (LRS). Para obter informações sobre o Premium Storage, consulte [Premium Storage: armazenamento de elevado desempenho para cargas de trabalho de Máquina Virtual de Azure](../../virtual-machines/windows/premium-storage.md).

## <a name="changing-replication-strategy"></a>Alterar a estratégia de replicação
Iremos permitem-lhe alterar estratégia de replicação da sua conta de armazenamento utilizando o [portal do Azure](https://portal.azure.com/), [Azure Powershell](storage-powershell-guide-full.md), [CLI do Azure](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest), ou um dos muitos [ Bibliotecas de cliente do Azure](https://docs.microsoft.com/en-us/azure/index?view=azure-dotnet#pivot=sdkstools). Alterar o tipo de replicação da conta de armazenamento não resulta num tempo.

   > [!NOTE]
   > Atualmente, não é possível utilizar o Portal ou a API para converter a sua conta para o ZRS. No entanto, planeamos suportar a migrar para o ZRS do LRS, GRS e RA-GRS depois do ZRS estiver geralmente disponível. Consulte [armazenamentocomredundânciadezona (ZRS)](storage-redundancy-zrs.md) para obter mais detalhes.
    
### <a name="are-there-any-costs-to-changing-my-accounts-replication-strategy"></a>Existem quaisquer custos para alterar a estratégia de replicação da minha conta?
Depende do seu caminho de conversão. Ordenação de cheapest a oferta de redundância mais dispendioso temos LRS, ZRS, GRS e RA-GRS. Por exemplo, vai *de* LRS para qualquer coisa resultará em encargos adicionais, porque, se pretender um nível de redundância mais sofisticado. Vai *para* GRS ou RA-GRS será cobrado um custo de largura de banda de saída porque os dados (na sua região principal) está a ser replicados para a região secundária remoto. Este é cobrada uma taxa única na configuração inicial. Depois dos dados são copiados, não existem sem custos de conversão mais. Só será cobrada para replicar qualquer novo ou atualizações aos dados existentes. Para obter mais detalhes em custos de largura de banda, consulte [página de preços do Storage do Azure](https://azure.microsoft.com/pricing/details/storage/blobs/).

Se alterar o GRS para LRS, há sem custos adicionais, mas os dados replicados são eliminados da localização secundária.

## <a name="see-also"></a>Consulte também

- [Armazenamento localmente redundante (LRS): redundância de dados de baixo custo de armazenamento do Azure](storage-redundancy-lrs.md)
- [Armazenamento com redundância de zona (ZRS): aplicações de elevada disponibilidade de armazenamento do Azure](storage-redundancy-zrs.md)
- [Armazenamento georredundante (GRS): a replicação entre regionais para armazenamento do Azure](storage-redundancy-grs.md)
- [Azure metas de desempenho e escalabilidade de armazenamento](storage-scalability-targets.md)
- [Ao conceber aplicações altamente disponíveis a utilizar o armazenamento RA-GRS](../storage-designing-ha-apps-with-ragrs.md)
- [Armazenamento do Microsoft Azure redundância opções e acesso de leitura georreplicação armazenamento redundantes ](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
- [Sosp - Storage do Azure: Um elevada serviço em nuvem armazenamento com consistência forte](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)
