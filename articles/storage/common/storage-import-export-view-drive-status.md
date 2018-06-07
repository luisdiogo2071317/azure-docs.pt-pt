---
title: Ver o estado das tarefas de importação/exportação do Azure | Microsoft Docs
description: Saiba como visualizar o estado das tarefas de importação/exportação e as unidades utilizadas.
author: alkohli
manager: jeconnoc
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/17/2018
ms.author: alkohli
ms.openlocfilehash: 176cbf190b6442682a222eb4f24b6583fb87a46b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661020"
---
# <a name="view-the-status-of-azure-importexport-jobs"></a>Ver o estado das tarefas de importação/exportação do Azure

Este artigo fornece informações sobre como visualizar o estado da tarefa e de unidade para tarefas de importação/exportação do Azure. Serviço de importação/exportação do Azure é utilizado para transferir, de forma segura, grandes quantidades de dados de Blobs do Azure e ficheiros do Azure. O serviço também é utilizado para exportar dados do Blob storage do Azure.  

## <a name="view-job-and-drive-status"></a>Ver estado de tarefa e unidade
Pode controlar o estado da sua importação ou exportação tarefas do portal do Azure. Clique em de **importar/exportar** separador. É apresentada uma lista das suas tarefas na página.

![Vista de estado da tarefa](./media/storage-import-export-service/jobstate.png)


## <a name="view-job-status"></a>Ver o estado de tarefa

Vê um dos seguintes Estados de tarefas, dependendo de onde a unidade está no processo.

| Estado da Tarefa | Descrição |
|:--- |:--- |
| Criação | Depois de uma tarefa é criada, o estado estiver definido como **criar**. Enquanto a tarefa está no **criar** Estado, o serviço de importação/exportação pressupõe que as unidades não foram fornecidas para o Centro de dados. Uma tarefa pode permanecer neste estado para até duas semanas, após o qual é automaticamente eliminado pelo serviço. |
| Envio | Depois de enviar o pacote, deve atualizar as informações de controlo no portal do Azure.  Isto ativa a tarefa para **envio** estado. A tarefa continuará a ser o **envio** estado para duas semanas. 
| Recebido | Depois de todas as unidades forem recebidas Centro de dados, o estado da tarefa está definido como **recebidos**. |
| A transferir | Depois de pelo menos uma unidade começou a processar, o estado da tarefa está definido como **Transferring**. Para obter mais informações, aceda a [Estados de unidade](#view-drive-status). |
| Empacotamento | Depois de todas as unidades concluiu o processamento, a tarefa é colocada na **empacotamento** estado até que as unidades são fornecidas para lhe. |
| Concluída | Depois de todas as unidades são fornecidas para que, se a tarefa tiver sido concluída sem erros, a tarefa com a em seguida, está definida como **concluído**. A tarefa é eliminada automaticamente após 90 dias no **concluído** estado. |
| Fechado | Depois de todas as unidades são fornecidas para que, se ocorreram erros durante o processamento da tarefa, a tarefa está definida como **fechado**. A tarefa é eliminada automaticamente após 90 dias no **fechado** estado. |

## <a name="view-drive-status"></a>Ver o estado de unidade

A tabela abaixo descreve o ciclo de vida de uma unidade individuais, que passa através de uma tarefa de importação ou exportação. O estado atual de cada unidade numa tarefa é apresentado no portal do Azure.

A tabela seguinte descreve cada Estado de cada unidade de uma tarefa pode pass-through.

| Unidade de estado | Descrição |
|:--- |:--- |
| Especificado | Para uma tarefa de importação, quando a tarefa é criada a partir do portal do Azure, o estado inicial para uma unidade é **especificado**. Para uma tarefa de exportação, uma vez que não existem unidade é especificada quando a tarefa é criada, o estado do disco inicial é **recebidos**. |
| Recebido | A unidade passa para o **recebidos** quando o serviço de importação/exportação processou as unidades que foram recebidas a partir da empresa de envio para uma tarefa de importação de estado. Para uma tarefa de exportação, o estado da unidade inicial é o **recebidos** estado. |
| NeverReceived | Move a unidade para o **NeverReceived** quando chega o pacote para uma tarefa, mas o pacote não contém a unidade de estado. Uma unidade também desloca para neste estado se duas semanas, uma vez que o serviço recebeu as informações de envio, mas o pacote ainda não chegaram no Centro de dados. |
| A transferir | Move de uma unidade para o **Transferring** estado quando o serviço começa a transferência de dados da unidade ao Storage do Azure. |
| Concluída | Move de uma unidade para o **concluído** estado quando o serviço foi transferido com êxito todos os dados sem erros.
| CompletedMoreInfo | Move de uma unidade para o **CompletedMoreInfo** quando o serviço detetou alguns problemas ao copiar dados a partir de ou para a unidade de estado. As informações podem incluir erros, avisos e mensagens informativas sobre substituir blobs.
| ShippedBack | Move de uma unidade para o **ShippedBack** estado quando foi enviada do Centro de dados para o endereço do remetente. |

Esta imagem do portal do Azure apresenta o estado de unidade de uma tarefa de exemplo:

![Vista de estado de unidade](./media/storage-import-export-service/drivestate.png)

A tabela seguinte descreve os Estados de falha de unidade e as ações executadas para cada Estado.

| Unidade de estado | Evento | Resolução / passo seguinte |
|:--- |:--- |:--- |
| NeverReceived | Uma unidade que está marcada como **NeverReceived** (porque não foi recebida como parte da shipment da tarefa), que são recebidos na shipment outro. | A equipa de operações move a unidade para **recebidos**. |
| N/A | Uma unidade que não faz parte de qualquer tarefa chega ao centro de dados como parte de outra tarefa. | A unidade está marcada como uma unidade adicional e é devolvida para si quando a tarefa associada ao pacote original estiver concluída. |

## <a name="time-to-process-job"></a>Tempo até ao processo de trabalho
A quantidade de tempo que demora a processar uma tarefa de importação/exportação varia com base num número de fatores, tais como:

-  Hora de envio
-  Carregar no Centro de dados
-  Tipo de tarefa e tamanho dos dados que está a ser copiados
-  Número de discos numa tarefa. 

Serviço de importação/exportação não tem um SLA, mas o serviço strives concluir a cópia dentro de 7 a 10 dias após os discos são recebidos. Para além do estado publicado no Portal do Azure, as REST APIs pode ser utilizadas para controlar o progresso da tarefa. A percentagem de conclusão parâmetro o [tarefas lista]() chamada de operação API fornece o progresso de cópia de percentagem.


## <a name="next-steps"></a>Passos Seguintes

* [Configurar a ferramenta de WAImportExport](storage-import-export-tool-how-to.md)
* [Transferência de dados com o utilitário de linha de comandos AzCopy](storage-use-azcopy.md)
* [Exemplo de API de REST de exportação de importação do Azure](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)

