---
title: incluir ficheiro
description: incluir ficheiro
services: automation
author: georgewallace
ms.service: automation
ms.topic: include
ms.date: 04/05/2018
ms.author: gwallace
ms.custom: include file
ms.openlocfilehash: 8242f2bb16b52e8c319027788d5b937d7f79ad3d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34665067"
---
| Recurso | Limite Máximo |
| --- | --- |
| Número máx. de novas tarefas que podem ser submetidos a cada 30 segundos por conta de automatização (tarefas agendadas não) |100 |
| Número máximo de tarefas em execução em simultâneo na mesma instância do tempo por conta de automatização (tarefas agendadas não) |200 |
| Número máx. de módulos que podem ser importados a cada 30 segundos por conta de automatização |5 |
| Tamanho máx. de um módulo |100 MB |
| Tempo de execução de tarefa - escalão gratuito |500 minutos por subscrição por mês de calendário |
| Quantidade máxima de memória especificada para uma tarefa  **<sup>1</sup>** |400 MB |
| Número máx. de sockets rede permitido por tarefa  **<sup>1</sup>** |1000 |
| Número máx. de contas de automatização numa subscrição |Sem Limite |

**<sup>1</sup>**  estes limites aplicam apenas aos sandboxes no Azure, para híbridos estas são só de estar limitadas pelas capacidades da máquina onde está localizado o worker híbrido.