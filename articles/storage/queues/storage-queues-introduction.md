---
title: Introdução às filas do Azure | Documentos da Microsoft
description: Introdução às filas do Azure
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 02/06/2019
ms.author: tamram
ms.subservice: queues
ms.openlocfilehash: 2ae0d3993df54e1c9e5a9bf93619e8f9faa8a917
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55873602"
---
# <a name="what-are-azure-queues"></a>Quais são as filas do Azure?

O Armazenamento de Filas do Azure é um serviço para armazenar um grande número de mensagens que podem ser acedidas a partir de qualquer local no mundo através de chamadas autenticadas com HTTP ou HTTPS. Uma mensagem de fila única pode ter até 64 KB e uma fila pode conter milhões de mensagens, até ao limite da capacidade total de uma conta de armazenamento.

## <a name="common-uses"></a>Utilizações comuns

Utilizações comuns do Armazenamento de filas:

* Criação de um registo de tarefas pendentes do trabalho para processamento de forma assíncrona
* Transmissão de mensagens de uma função da Web do Azure para uma função de trabalho do Azure

## <a name="queue-service-concepts"></a>Conceitos do serviço fila

O serviço Fila contém os seguintes componentes:

![Conceitos de fila](./media/storage-queues-introduction/queue1.png)

* **Formato de URL:** Ficheiros são endereçáveis através do formato de URL seguinte:   
    https://`<storage account>`.queue.core.windows.net/`<queue>` 
  
    O seguinte URL endereça uma fila no diagrama:  
  
    `https://myaccount.queue.core.windows.net/images-to-download`

* **Conta de armazenamento:** Todos os acessos ao armazenamento do Azure é feito através de uma conta de armazenamento. Veja [Metas de Desempenho e Escalabilidade do Storage do Azure](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) para obter detalhes acerca da capacidade das contas de armazenamento.

* **Fila:** Uma fila contém um conjunto de mensagens. Todas as mensagens têm de estar numa fila. Tenha em atenção que o nome da fila tem de estar todo em minúsculas. Para obter informações sobre a nomenclatura de filas, veja [Nomenclatura de Filas e Metadados](https://msdn.microsoft.com/library/azure/dd179349.aspx).

* **mensagem:** Uma mensagem, em qualquer formato, de até 64 KB. O tempo máximo que uma mensagem pode permanecer na fila é de sete dias.

## <a name="next-steps"></a>Passos Seguintes

* [Criar uma conta de armazenamento](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [Introdução às filas com .NET](storage-dotnet-how-to-use-queues.md)
