---
title: Replicação de dados no armazenamento do Azure | Documentos da Microsoft
description: Dados da sua conta de armazenamento do Microsoft Azure são replicados para durabilidade e elevada disponibilidade. Opções de replicação incluem armazenamento localmente redundante (LRS), o armazenamento com redundância de zona (ZRS), o armazenamento georredundante (GRS) e o armazenamento georredundante com acesso de leitura (RA-GRS).
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 10/08/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: 618e1f5249f2e05c26e91231b4283d82546a880b
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49954492"
---
# <a name="azure-storage-replication"></a>Replicação do Storage do Azure

Os dados na sua conta de armazenamento do Microsoft Azure são sempre replicados para garantir a durabilidade e a elevada disponibilidade. Replicação de armazenamento do Azure copia os dados para que ele está protegido contra eventos planeados e imprevistos, desde a falhas transitórias de hardware, rede ou falhas de energia, enormes desastres naturais e assim por diante. Pode optar por replicar os seus dados no mesmo centro de dados, em centros de dados zonal dentro da mesma região e até mesmo entre regiões.

A replicação garante que a sua conta de armazenamento cumpre o [Contrato de Nível de Serviço (SLA) para o Armazenamento](https://azure.microsoft.com/support/legal/sla/storage/) mesmo em caso de falhas. Leia o SLA para obter mais informações sobre as garantias do Armazenamento do Azure quanto à durabilidade e à disponibilidade.

## <a name="choosing-a-replication-option"></a>Escolher uma opção de replicação

Quando cria uma conta de armazenamento, tem de selecionar uma das seguintes opções de replicação:

* [Armazenamento localmente redundante (LRS)](storage-redundancy-lrs.md)
* [Armazenamento com redundância de zona (ZRS)](storage-redundancy-zrs.md)
* [Armazenamento georredundante (GRS)](storage-redundancy-grs.md)
* [Armazenamento georredundante com acesso de leitura (RA-GRS)](storage-redundancy-grs.md#read-access-geo-redundant-storage)

A tabela seguinte fornece uma rápida visão geral do escopo da durabilidade e disponibilidade cada estratégia de replicação irá fornecer-lhe para um determinado tipo de evento (ou um evento de impacto semelhante).

| Cenário                                                                                                 | LRS                             | ZRS                              | GRS                                  | RA-GRS                               |
| :------------------------------------------------------------------------------------------------------- | :------------------------------ | :------------------------------- | :----------------------------------- | :----------------------------------- |
| Indisponibilidade de nó dentro de um centro de dados                                                                 | Sim                             | Sim                              | Sim                                  | Sim                                  |
| Um Datacenter inteiro (zonal ou não zonal) fica indisponível                                           | Não                              | Sim                              | Sim                                  | Sim                                  |
| Uma interrupção de toda a região                                                                                     | Não                              | Não                               | Sim                                  | Sim                                  |
| Acesso de leitura aos seus dados (numa região remota, georreplicado) em caso de indisponibilidade de toda a região | Não                              | Não                               | Não                                   | Sim                                  |
| Concebido para fornecer \_ \_ durabilidade dos objetos ao longo de um determinado ano                                          | pelo menos, 99,999999999% (11 9 s) | pelo menos, 99.9999999999% (12 9 s) | pelo menos 99,99999999999999% (16 9 s) | pelo menos 99,99999999999999% (16 9 s) |
| Tipos de conta de armazenamento suportadas                                                                   | Blob de GPv2, GPv1,                | GPv2, GPv1 (através do PowerShell, CLI do Azure ou recurso de API do fornecedor)                             | Blob de GPv2, GPv1,                     | Blob de GPv2, GPv1,                     |
| SLA de disponibilidade para solicitações de leitura | Pelo menos, 99,9% (99% para o escalão de acesso esporádico) | Pelo menos, 99,9% (99% para o escalão de acesso esporádico) | Pelo menos, 99,9% (99% para o escalão de acesso esporádico) | Pelo menos, 99,99% (99,9% para o escalão de acesso esporádico) |
| SLA de disponibilidade para pedidos de escrita | Pelo menos, 99,9% (99% para o escalão de acesso esporádico) | Pelo menos, 99,9% (99% para o escalão de acesso esporádico) | Pelo menos, 99,9% (99% para o escalão de acesso esporádico) | Pelo menos, 99,9% (99% para o escalão de acesso esporádico) |

Para obter informações sobre cada opção de redundância de preços, consulte [preços de armazenamento do Azure](https://azure.microsoft.com/pricing/details/storage/). 

Para obter informações sobre o armazenamento do Azure garante-se para durabilidade e disponibilidade, consulte a [SLA de armazenamento do Azure](https://azure.microsoft.com/support/legal/sla/storage/).

> [!NOTE]
> O armazenamento Premium suporta o armazenamento apenas localmente redundante (LRS). Para obter informações sobre o armazenamento Premium, consulte [o armazenamento Premium: armazenamento de elevado desempenho para cargas de trabalho de Máquina Virtual de Azure](../../virtual-machines/windows/premium-storage.md).

## <a name="changing-replication-strategy"></a>Alterar a estratégia de replicação
Permitimos que mudam a estratégia de replicação de sua conta de armazenamento utilizando o [portal do Azure](https://portal.azure.com/), [Azure Powershell](storage-powershell-guide-full.md), [da CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), ou um dos muitos [ Bibliotecas de cliente do Azure](https://docs.microsoft.com/azure/index?view=azure-dotnet#pivot=sdkstools). Alterar o tipo de replicação da conta de armazenamento não resulta num período de indisponibilidade.

   > [!NOTE]
   > Atualmente, não é possível utilizar o Portal ou a API para converter a sua conta ZRS. Se pretender converter a replicação da sua conta ZRS, veja [armazenamentoredundância de zona (ZRS)](storage-redundancy-zrs.md) para obter detalhes.
    
### <a name="are-there-any-costs-to-changing-my-accounts-replication-strategy"></a>Existem custos para alterar a estratégia de replicação da minha conta?
Depende de seu caminho de conversão. Ordenação de mais barato da oferta de redundância mais caras temos LRS, ZRS, GRS e RA-GRS. Por exemplo, irá *partir* LRS a nada serão aplicados custos adicionais, uma vez que vai para um nível de redundância mais sofisticado. Vai *para* GRS ou RA-GRS será cobrado um custo de largura de banda de saída porque os dados (em sua região primária) está a ser replicados para a região secundária remoto. Este é um custo único na configuração inicial. Depois dos dados são copiados, não existem mais conversão custos. Apenas serão cobrados para replicar qualquer novo ou de atualizações nos dados existentes. Para obter detalhes sobre os custos de largura de banda, consulte [página de preços de armazenamento do Azure](https://azure.microsoft.com/pricing/details/storage/blobs/).

Se alterar o GRS para LRS, não existe nenhum custo adicional, mas os dados replicados são eliminados da localização secundária.

## <a name="see-also"></a>Consulte também

- [Armazenamento localmente redundante (LRS): redundância de dados de baixo custo do armazenamento do Azure](storage-redundancy-lrs.md)
- [Armazenamento com redundância de zona (ZRS): aplicações de armazenamento do Azure de elevada disponibilidade](storage-redundancy-zrs.md)
- [Armazenamento georredundante (GRS): replicação de inter-regional do armazenamento do Azure](storage-redundancy-grs.md)
- [Azure destinos de escalabilidade e desempenho de armazenamento](storage-scalability-targets.md)
- [Conceber aplicações de elevada disponibilidade com o armazenamento RA-GRS](../storage-designing-ha-apps-with-ragrs.md)
- [Armazenamento do Microsoft Azure redundância acesso de leitura e de opções de armazenamento georredundante ](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
- [Artigo SOSP – Storage do Azure: Um cloud altamente disponível, serviço de armazenamento com consistência forte](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)
