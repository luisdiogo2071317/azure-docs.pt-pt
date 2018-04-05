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
ms.openlocfilehash: 6381f8f0e68853183fc3e17e76b4ab93b152b48b
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
| Recurso | Limite Predefinido |
| --- | --- |
| Número de contas do storage por região | 200<sup>1</sup> |
| Capacidade de conta de armazenamento máximo | 500 TiB<sup>2</sup> |
| Número máx. de contentores de BLOBs, blobs, partilhas de ficheiros, tabelas, filas, entidades ou mensagens por conta de armazenamento | Sem limite |
| Taxa de pedidos máximo por conta de armazenamento | 20.000 pedidos por segundo<sup>2</sup> |
| Entrada máximo<sup>3</sup> por conta de armazenamento (regiões EUA) | 10 Gbps, se estiver ativada RA-GRS/GRS, 20 Gbps para LRS/ZRS<sup>4</sup> |
| Saída máximo<sup>3</sup> por conta de armazenamento (regiões EUA) | 20 Gbps se RA-GRS/GRS ativada, 30 Gbps para LRS/ZRS<sup>4</sup> |
| Entrada máximo<sup>3</sup> por conta de armazenamento (regiões de não-US) | 5 Gbps se RA-GRS/GRS ativada, 10 Gbps para LRS/ZRS<sup>4</sup> |
| Saída máximo<sup>3</sup> por conta de armazenamento (regiões de não-US) | 10 Gbps, se estiver ativada RA-GRS/GRS, 15 Gbps para LRS/ZRS<sup>4</sup> |

<sup>1</sup>inclui as contas do storage Standard e Premium. Se necessitar de mais de 200 contas de armazenamento, efetue um pedido através do [Suporte do Azure](https://azure.microsoft.com/support/faq/). A equipa do Armazenamento do Azure analisará o seu caso comercial e pode aprovar até 250 contas de armazenamento. 

<sup>2</sup> se precisar de limites expandidos para a sua conta de armazenamento, contacte o [suporte do Azure](https://azure.microsoft.com/support/faq/). A equipa de armazenamento do Azure irá examinar o pedido e pode aprovar os limites superiores numa base de maiúsculas e minúsculas, maiúsculas e minúsculas. Ambos para fins gerais e de contas do Blob storage suportam maior capacidade, a entrada/saída e a taxa de pedidos por pedido. Para os novos valores máximos para contas do Blob storage, consulte [anunciar as contas de armazenamento de escala maiores, superior](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/).

<sup>3</sup> limitado apenas por limites de entrada/saída da conta. *Entrada* refere-se a todos os dados (pedidos) que está a ser enviados para uma conta de armazenamento. *Entrada* refere-se a todos os dados (respostas) recebidas de uma conta de armazenamento.  

<sup>4</sup>as opções de redundância do armazenamento do azure incluem:
* **RA-GRS**: armazenamento georredundante com acesso de leitura. Se estiver ativada RA-GRS, destinos de saída para a localização secundária são idênticos para a localização primária.
* **GRS**: armazenamento georredundante. 
* **O ZRS**: armazenamento com redundância de zona.
* **LRS**: armazenamento localmente redundante. 
