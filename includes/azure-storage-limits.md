---
title: incluir ficheiro
description: incluir ficheiro
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 04/03/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 6572adb0d8d629910492603a17988b89acce2f17
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38756038"
---
| Recurso | Limite Predefinido |
| --- | --- |
| Número de contas de armazenamento por região por subscrição | 200<sup>1</sup> |
| Capacidade de conta de armazenamento máximo | 500 TiB<sup>2</sup> |
| Número máx. de contentores de BLOBs, blobs, partilhas de ficheiros, tabelas, filas, entidades ou mensagens por conta de armazenamento | Sem limite |
| Taxa de pedidos máxima por conta de armazenamento | 20 000 pedidos por segundo<sup>2</sup> |
| Entrada de Max<sup>3</sup> por conta de armazenamento (regiões E.u.a.) | 10 Gbps, se o RA-GRS/GRS ativado, 20 Gbps para LRS/ZRS<sup>4</sup> |
| Saída de Max<sup>3</sup> por conta de armazenamento (regiões E.u.a.) | 20 Gbps se o RA-GRS/GRS ativado, 30 Gbps para LRS/ZRS<sup>4</sup> |
| Entrada de Max<sup>3</sup> por conta de armazenamento (regiões de não-US) | 5 Gbps, se o RA-GRS/GRS ativado, 10 Gbps para LRS/ZRS<sup>4</sup> |
| Saída de Max<sup>3</sup> por conta de armazenamento (regiões de não-US) | 10 Gbps, se o RA-GRS/GRS ativado, 15 Gbps para LRS/ZRS<sup>4</sup> |

<sup>1</sup>inclui contas de armazenamento Standard e Premium. Se necessitar de mais de 200 contas de armazenamento numa determinada região, efetue um pedido através de [suporte do Azure](https://azure.microsoft.com/support/faq/). A equipa do armazenamento do Azure analisará o seu caso comercial e pode aprovar até 250 contas de armazenamento para uma determinada região. 

<sup>2</sup> se precisar de limites expandidos para a sua conta de armazenamento, entre em contato com [suporte do Azure](https://azure.microsoft.com/support/faq/). A equipa do armazenamento do Azure analisará o pedido e aprovar limites mais elevados, caso a caso. Ambos para fins gerais e as contas de armazenamento de BLOBs suportam maior capacidade e taxa de pedidos de entrada/saída por pedido. Para os novos valores máximos para contas de armazenamento de BLOBs, veja [contas de armazenamento maiores e maior escala a anunciar](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/).

<sup>3</sup> colocado apenas por limites de entrada/saída da conta. *Entrada* refere-se a todos os dados (pedidos) a ser enviados para uma conta de armazenamento. *Entrada* refere-se a todos os dados (respostas) recebidas de uma conta de armazenamento.  

<sup>4</sup>as opções de redundância de armazenamento do azure incluem:
* **RA-GRS**: armazenamento geograficamente redundante de acesso de leitura. Se o RA-GRS for ativado, os destinos de saída para a localização secundária são idênticos para a localização principal.
* **GRS**: armazenamento georredundante. 
* **O ZRS**: armazenamento com redundância de zona.
* **LRS**: armazenamento localmente redundante. 
