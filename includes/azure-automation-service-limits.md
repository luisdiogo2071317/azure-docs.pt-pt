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
ms.openlocfilehash: 34cae9172d9b024bd6866742d39d82ad496bfc52
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45570444"
---
| Recurso | Limite Máximo |Notas|
| --- | --- |---|
| Número máx. de novas tarefas que podem ser submetidos a cada 30 segundos por conta de automatização (agendada não tarefas) |100 |Quando este limite tive, os pedidos subsequentes para criar uma tarefa falharem. O cliente recebe uma resposta de erro.|
| Número máximo de tarefas em execução em simultâneo na mesma instância de tempo por conta de automatização (agendada não tarefas) |200 |Quando este limite tive, os pedidos subsequentes para criar uma tarefa falharem. O cliente recebe uma resposta de erro.|
| Número máximo de módulos que podem ser importados a cada 30 segundos por conta de automatização |5 ||
| Tamanho máx. de um módulo |100 MB ||
| Tempo de execução de tarefa - escalão gratuito |500 minutos por subscrição por mês de calendário ||
| Quantidade máxima de espaço em disco permitida por sandbox  **<sup>1</sup>** |1 GB |Aplica-se a apenas uma áreas de segurança do Azure|
| Quantidade máxima de memória para uma área de segurança  **<sup>1</sup>** |400 MB |Aplica-se a apenas uma áreas de segurança do Azure|
| Número máx. de soquetes de rede, permitidos por sandbox  **<sup>1</sup>** |1000 |Aplica-se a apenas uma áreas de segurança do Azure|
| Tempo de execução máximo permitido por runbook  **<sup>1</sup>** |3 horas |Aplica-se a apenas uma áreas de segurança do Azure|
| Número máx. de contas de automatização numa subscrição |Sem Limite ||
|Número máx. de simultâneas tarefas que ser executados num único Runbook Worker híbrido|50 ||
| Tamanho de parâmetros da tarefa de Runbook máx.   | 512 kb||
| Parâmetros do Runbook máx.   | 50|Pode passar uma cadeia de caracteres do JSON ou XML para um parâmetro e analisá-lo com o runbook se atingir o limite de parâmetro de 50|
| Tamanho máximo payload de webhook |  512 kb|

**<sup>1</sup>**  uma área de segurança é um ambiente compartilhado que pode ser utilizado por várias tarefas, tarefas com a mesma área de segurança são ligadas pelas limitações de recursos da área de segurança.
