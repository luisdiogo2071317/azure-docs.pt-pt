---
title: incluir ficheiro
description: incluir ficheiro
services: batch
author: dlepow
ms.service: batch
ms.topic: include
ms.date: 04/06/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 0dcb608553d4455403c073e34e83bccb81cc64db
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2018
---
Uma tarefa em execução no Azure Batch pode produzir dados de saída quando é executada. Tarefa dados de saída, muitas vezes, necessita de ser armazenados durante obtenção por outras tarefas na tarefa, a aplicação de cliente que executou a tarefa, ou ambos. Tarefas escrever dados de saída para o sistema de ficheiros de um nó de computação do Batch, mas todos os dados no nó são perdidas quando é recriada ou quando o nó deixa o conjunto. As tarefas podem também ter um período de retenção de ficheiros, após o qual são eliminados os ficheiros criados pela tarefa. Por esta razão, é importante manter o resultado da tarefa que terá mais tarde para um arquivo de dados, tais como [Storage do Azure](https://docs.microsoft.com/azure/storage/).

Para opções de conta de armazenamento no Batch, consulte o [descrição geral da funcionalidade do Batch](../articles/batch/batch-api-basics.md#azure-storage-account).
