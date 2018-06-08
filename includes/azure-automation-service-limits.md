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
ms.openlocfilehash: 6b6e4afa7c8b18c8ce9af8c6abd371b4321e3343
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "34852046"
---
| Recurso | Limite Máximo |Notas|
| --- | --- |---|
| Número máx. de novas tarefas que podem ser submetidos a cada 30 segundos por conta de automatização (tarefas agendadas não) |100 |Quando este limite-atingido, os pedidos subsequentes para criar uma tarefa falharem. O cliente recebe uma resposta de erro.|
| Número máximo de tarefas em execução em simultâneo na mesma instância do tempo por conta de automatização (tarefas agendadas não) |200 |Quando este limite-atingido, os pedidos subsequentes para criar uma tarefa falharem. O cliente recebe uma resposta de erro.|
| Número máx. de módulos que podem ser importados a cada 30 segundos por conta de automatização |5 ||
| Tamanho máx. de um módulo |100 MB ||
| Tempo de execução de tarefa - escalão gratuito |500 minutos por subscrição por mês de calendário ||
| Quantidade máxima de espaço em disco permitido por sandbox**<sup>1</sup>** |1 GB |Aplica-se a apenas sandboxes do Azure|
| Quantidade máxima de memória especificada para um sandbox**<sup>1</sup>** |400 MB |Aplica-se a apenas sandboxes do Azure|
| Número máx. de sockets rede permitido por sandbox**<sup>1</sup>** |1000 |Aplica-se a apenas sandboxes do Azure|
| Número máx. de contas de automatização numa subscrição |Sem Limite ||
|Número máx. de em simultâneo no cancelamento das tarefas que ser executadas num único Runbook Worker híbrido|50 ||

**<sup>1</sup>**  uma sandbox é um ambiente partilhado que pode ser utilizado por várias tarefas, as tarefas que utilizam a mesma sandbox estão vinculadas pelas limitações de recursos de sandbox.
