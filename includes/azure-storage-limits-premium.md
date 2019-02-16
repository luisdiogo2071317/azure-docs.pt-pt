---
title: incluir ficheiro
description: incluir ficheiro
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 01/22/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 3d100ec08b7b6d70366e605daf9c67edc6ab3bf3
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/16/2019
ms.locfileid: "56331073"
---
Contas de armazenamento Premium tem os seguintes destinos de escalabilidade:

| Capacidade de conta total | Largura de banda total de uma conta de armazenamento localmente redundante |
| --- | --- | 
| Capacidade do disco: 35 TB <br>Capacidade do instantâneo: 10 TB | Cópia de segurança para 50 gigabits por segundo de entrada<sup>1</sup> + saída<sup>2</sup> |

<sup>1</sup> todos os dados (pedidos), que são enviados para uma conta de armazenamento

<sup>2</sup> todos os dados (respostas) que são recebidos a partir de uma conta de armazenamento

Se estiver a utilizar contas de armazenamento premium para discos não geridos e seu aplicativo excede os destinos de escalabilidade de uma única conta de armazenamento, pode querer migrar para discos geridos. Se não quiser migrar para discos geridos, crie a sua aplicação para utilizar várias contas de armazenamento. Em seguida, particione os dados entre essas contas de armazenamento. Por exemplo, se quiser anexar discos de 51-TB em várias VMs, espalhá-los entre duas contas de armazenamento. 35 TB é o limite para uma conta de armazenamento premium única. Certifique-se de que uma conta de armazenamento premium única nunca tem mais de 35 TB de discos aprovisionados.