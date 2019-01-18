---
title: incluir ficheiro
description: incluir ficheiro
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 01/11/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: e4fa42b6c32c3eb383eea4489ea109c0d496bdb9
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2019
ms.locfileid: "54392740"
---
A tabela seguinte descreve os limites predefinidos para o armazenamento do Azure. O *entrada* limite refere-se a todos os dados (pedidos) a ser enviados para uma conta de armazenamento. O *saída* limite refere-se a todos os dados (respostas) recebidos de uma conta de armazenamento.

| Recurso | Limite predefinido |
| --- | --- |
| Número de contas de armazenamento por região por subscrição, incluindo contas standard e premium | 250 |
| Capacidade de conta de armazenamento máximo | 2 PB para E.U.A. e Europa, 500 TB para todas as outras regiões, incluindo do Reino Unido |
| Número máx. de contentores de BLOBs, blobs, partilhas de ficheiros, tabelas, filas, entidades ou mensagens por conta de armazenamento | Sem limite |
| Taxa máxima de pedidos<sup>1</sup> por conta de armazenamento | 20 000 pedidos por segundo |
| Entrada de Max<sup>1</sup> por conta de armazenamento (regiões E.u.a.) | 10 Gbps, se o RA-GRS/GRS ativado, 20 Gbps para LRS/ZRS<sup>2</sup> |
| Entrada de Max<sup>1</sup> por conta de armazenamento (regiões de não-US) | 5 Gbps, se o RA-GRS/GRS ativado, 10 Gbps para LRS/ZRS<sup>2</sup> |
| Saída de Max para fins gerais v2 e contas de armazenamento de BLOBs (todas as regiões) | 50 Gbps |
| Saída de máx. de contas de armazenamento para fins gerais v1 (regiões dos E.U.A..) | 20 Gbps se o RA-GRS/GRS ativado, 30 Gbps para LRS/ZRS <sup>2</sup> |
| Saída de máx. de contas de armazenamento para fins gerais v1 (regiões de não-US) | 10 Gbps, se o RA-GRS/GRS ativado, 15 Gbps para LRS/ZRS <sup>2</sup> |

<sup>1</sup> contas de armazenamento standard do azure suportam limites mais elevados para entrada por pedido. Para pedir um aumento de limites de conta para entrada, contacte [suporte do Azure](https://azure.microsoft.com/support/faq/).

<sup>2</sup> [replicação de armazenamento do azure](https://docs.microsoft.com/azure/storage/common/storage-redundancy) as opções incluem:
* **RA-GRS**: Armazenamento georredundante com acesso de leitura. Se o RA-GRS for ativado, os destinos de saída para a localização secundária são idênticos para a localização principal.
* **GRS**: Armazenamento georredundante. 
* **ZRS**: Armazenamento com redundância de zona.
* **LRS**: Armazenamento localmente redundante. 

> [!NOTE]
> A Microsoft recomenda utilizar uma conta de armazenamento para fins gerais v2 na maioria dos cenários. Pode facilmente atualizar uma para fins gerais v1 ou a conta de armazenamento de BLOBs para uma conta para fins gerais v2, sem períodos de indisponibilidade e sem a necessidade de copiar dados.
>
> Para obter mais informações sobre contas de armazenamento do Azure, consulte [descrição geral da conta de armazenamento](../articles/storage/common/storage-account-overview.md). 

Se as necessidades da sua aplicação excederem os destinos de escalabilidade de uma conta de armazenamento única, pode criar a sua aplicação para utilizar várias contas de armazenamento. Em seguida, pode particionar seus objetos de dados entre essas contas de armazenamento. Ver [preços de armazenamento do Azure](https://azure.microsoft.com/pricing/details/storage/) para obter informações sobre os preços de volumes.

Todas as contas de armazenamento executam numa topologia de rede simples e suportam os destinos de escalabilidade e desempenho descritos neste artigo, independentemente de quando foram criadas. Para obter mais informações sobre a arquitetura de rede simples de armazenamento do Azure e na escalabilidade, consulte [armazenamento do Microsoft Azure: Um serviço de armazenamento de Cloud de elevada disponibilidade com consistência forte](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).

