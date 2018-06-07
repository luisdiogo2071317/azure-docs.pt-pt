---
title: incluir ficheiro
description: incluir ficheiro
services: container-registry
author: mmacy
ms.service: container-registry
ms.topic: include
ms.date: 05/29/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: 942b9bdf0201acaefe3333bcf928772899b9bdc2
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34665068"
---
| Recurso | Básica | Standard | Premium |
|---|---|---|---|---|
| Armazenamento | 10 giB | 100 GiB| 500 GiB |
| Tamanho máximo da camada de imagem | 20 giB | 20 giB | 50 giB |
| ReadOps por minuto<sup>1, 2</sup> | 1000 | 3000 | 10000 |
| WriteOps por minuto<sup>1, 3</sup> | 100 | 500 | 2000 |
| Transferir a largura de banda MBps<sup>1</sup> | 30 | 60 | 100 |
| Carregar MBps da largura de banda<sup>1</sup> | 10 | 20 | 50 |
| Webhooks | 2 | 10 | 100 |
| Georreplicação | N/A | N/A | [Suportado](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication) |

<sup>1</sup> *ReadOps*, *WriteOps*, e *largura de banda* são estimativas mínimo. ACR strives melhorar o desempenho porque requer a utilização.

<sup>2</sup> [solicitação docker](https://docs.docker.com/registry/spec/api/#pulling-an-image) traduz-se várias operações de leitura, com base no número de camadas na imagem, mais a obtenção de manifesto.

<sup>3</sup> [docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image) traduz-se várias operações de escrita, com base no número de camadas tem de ser instalada. A `docker push` inclui *ReadOps* para obter um manifesto para uma imagem existente.