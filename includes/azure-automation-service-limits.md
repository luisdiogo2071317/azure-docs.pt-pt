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
ms.openlocfilehash: abe7668ed16ddd5cc1b247207d3e80f7c1118137
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50964612"
---
| Recurso | Limite Máximo |Notas|
| --- | --- |---|
| Número máximo de novas tarefas que podem ser enviadas a cada 30 segundos por Conta de Automatização (tarefas que não sejam agendadas) |100 |Quando esse limite é alcançado, os pedidos subsequentes para criar uma tarefa falham. O cliente recebe uma resposta de erro.|
| Número máximo de tarefas em execução em simultâneo na mesma instância de tempo por Conta de Automatização (tarefas não agendadas) |200 |Quando esse limite é alcançado, os pedidos subsequentes para criar uma tarefa falham. O cliente recebe uma resposta de erro.|
| Número máximo de módulos que podem ser importados ac ada 30 segundos por Conta de Automatização |5 ||
| Tamanho máximo de um Módulo |100 MB ||
| Tempo de Execução de Tarefa - escalão Gratuito |500 minutos por subscrição por mês ||
| Quantidade máxima de espaço em disco permitida por sandbox**<sup>1</sup>** |1 GB |Aplica-se apenas às sandboxes do Azure|
| Quantidade máxima de memória dada a uma sandbox**<sup>1</sup>** |400 MB |Aplica-se apenas às sandboxes do Azure|
| Número máximo de sockets de rede permitidos por sandbox**<sup>1</sup>** |1000 |Aplica-se apenas às sandboxes do Azure|
| Tempo de execução máximo permitido por runbook **<sup>1</sup>** |3 horas |Aplica-se apenas às sandboxes do Azure|
| Número máximo de Contas de Automatização numa subscrição |Sem Limite ||
|Número máximo de tarefas em simultâneo a executar numa única Função de Trabalho de Runbook Híbrida|50 ||
| Tamanho máximo de parâmetros de Tarefa do Runbook   | 512 kb||
| Máximo de parâmetros do Runbook   | 50|Pode passar uma cadeia de carateres em JSON ou XML para um parâmetro e analisá-lo com o runbook se alcançar o limite de 50 parâmetros|
| Tamanho máximo de payload de webhook |  512 kb|
| Máximo de dias que os dados da tarefa são retidos|30 dias|

**<sup>1</sup>** Uma sandbox é um ambiente partilhado que pode ser utilizado por várias tarefas, as tarefas que utilizem a mesma sandbox são vinculadas pelas limitações de recurso da sandbox.
