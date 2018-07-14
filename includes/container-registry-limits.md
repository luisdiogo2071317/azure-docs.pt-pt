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
ms.openlocfilehash: 92a5d162e7a0b2c752a2f8e9c5941edf43e539e3
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38991084"
---
| Recurso | Básica | Standard | Premium |
|---|---|---|---|---|
| Armazenamento | 10 giB | 100 GiB| 500 GiB |
| Tamanho máximo da camada de imagem | 20 giB | 20 giB | 50 giB |
| ReadOps por minuto<sup>1, 2</sup> | 1,000 | 3000 | 10,000 |
| WriteOps por minuto<sup>1, 3</sup> | 100 | 500 | 2,000 |
| Transferir MBps da largura de banda<sup>1</sup> | 30 | 60 | 100 |
| Carregar MBps da largura de banda<sup>1</sup> | 10 | 20 | 50 |
| Webhooks | 2 | 10 | 100 |
| Georreplicação | N/A | N/A | [Suportado](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication) |

<sup>1</sup> *ReadOps*, *WriteOps*, e *largura de banda* são estimativas mínimo. ACR está empenhada em melhorar o desempenho à medida que necessita de utilização.

<sup>2</sup> [pull do docker](https://docs.docker.com/registry/spec/api/#pulling-an-image) se traduz em várias operações de leitura, com base no número de camadas na imagem, bem como a obtenção de manifesto.

<sup>3</sup> [push do docker](https://docs.docker.com/registry/spec/api/#pushing-an-image) se traduz em várias operações de escrita, com base no número de camadas que devem ser colocadas. R `docker push` inclui *ReadOps* para recuperar um manifesto para uma imagem existente.
