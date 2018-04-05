---
title: incluir ficheiro
description: incluir ficheiro
services: container-registry
author: mmacy
ms.service: container-registry
ms.topic: include
ms.date: 03/23/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: 575483192954f4e05db50e701e223829e041cffc
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
| Recurso | Básica | Standard | Premium |
|---|---|---|---|---|
| Armazenamento | 10 GiB | 100 GiB| 500 GiB |
| ReadOps por minuto<sup>1, 2</sup> | 1000 | 3000 | 10000 |
| WriteOps por minuto<sup>1, 3</sup> | 100 | 500 | 2000 |
| Transferir a largura de banda MBps<sup>1</sup> | 30 | 60 | 100 |
| Carregar MBps da largura de banda<sup>1</sup> | 10 | 20 | 50 |
| Webhooks | 2 | 10 | 100 |
| Georreplicação | N/A | N/A | [Suportado *(pré-visualização)*](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication) |

<sup>1</sup> *ReadOps*, *WriteOps*, e *largura de banda* são estimativas mínimo. ACR strives melhorar o desempenho porque requer a utilização.

<sup>2</sup> [solicitação docker](https://docs.docker.com/registry/spec/api/#pulling-an-image) traduz-se várias operações de leitura, com base no número de camadas na imagem, mais a obtenção de manifesto.

<sup>3</sup> [docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image) traduz-se várias operações de escrita, com base no número de camadas tem de ser instalada. A `docker push` inclui *ReadOps* para obter um manifesto para uma imagem existente.