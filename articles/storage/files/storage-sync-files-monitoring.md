---
title: Monitorizar a sincronização de ficheiros do Azure | Documentos da Microsoft
description: Como monitorizar a sincronização de ficheiros do Azure.
services: storage
author: jeffpatt24
ms.service: storage
ms.topic: article
ms.date: 01/31/2019
ms.author: jeffpatt
ms.subservice: files
ms.openlocfilehash: a14b0f2b01a0566a47cbcb02ee4315adcba9a90f
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56200807"
---
# <a name="monitor-azure-file-sync"></a>Monitorizar o Azure File Sync

Utilize o Azure File Sync para centralizar as partilhas de ficheiros da sua organização nos ficheiros do Azure, mantendo a flexibilidade, desempenho e compatibilidade de um servidor de ficheiros no local. O Azure File Sync transforma o Windows Server numa cache rápida da sua partilha de ficheiros do Azure. Pode usar qualquer protocolo disponível no Windows Server para aceder aos seus dados localmente, incluindo SMB, NFS e FTPS. Pode ter o número de caches que precisar em todo o mundo.

Este artigo descreve como monitorizar a implementação do Azure File Sync com o portal do Azure e o Windows Server.

As seguintes opções de monitorização estão disponíveis atualmente:

## <a name="azure-portal"></a>Portal do Azure

No portal do Azure, pode ver o estado de funcionamento do servidor registado e métricas de estado de funcionamento do ponto final de servidor (estado de funcionamento de sincronização).

### <a name="storage-sync-service"></a>Serviço de Sincronização de Armazenamento

Para ver o estado de funcionamento do servidor registado, o estado de funcionamento de ponto final de servidor e métricas, vá para o serviço de sincronização de armazenamento no portal do Azure. Estado de funcionamento do servidor registado é visível no painel de servidores registada. Estado de funcionamento de ponto final de servidor é visualizável em Painel de grupos de sincronização.

Estado de funcionamento do servidor registado
- Se o estado do servidor registado estiver Online, o servidor está a comunicar corretamente com o serviço.
- Se o estado do servidor registado é aparece Offline, certifique-se de que o processo de Monitor de sincronização de armazenamento (AzureStorageSyncMonitor.exe) no servidor está em execução. Se o servidor estiver protegido por uma firewall ou proxy, configure o firewall e proxy por [documentação](https://docs.microsoft.com/azure/storage/files/storage-sync-files-firewall-and-proxy).

Server Endpoint Health
- O estado de funcionamento do ponto final do servidor no portal do baseia-se sobre os eventos de sincronização que são registados no registo de eventos de telemetria no servidor (ID 9102 e 9302). Se uma sessão de sincronização falhar devido a um erro transitório (por exemplo, o erro foi cancelado), sincronização pode ainda mostrar bom estado de funcionamento no portal, desde que a sessão de sincronização atual está fazendo progresso (9302 de ID de evento é usada para determinar se os ficheiros estão a ser aplicados). Consulte a seguinte documentação para obter mais informações: [Sincronizar o estado de funcionamento](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) & [sincronizar progresso](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-monitor-the-progress-of-a-current-sync-session).
- Se o portal mostra um erro de sincronização devido à não fazendo progresso de sincronização, consulte a [documentação de resolução de problemas](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#common-sync-errors) para obter orientações.

Métricas
- As seguintes métricas estão visíveis no portal do serviço de sincronização de armazenamento:

  | Nome da métrica | Descrição | Portal blade(s) | 
  |-|-|-|
  | Bytes sincronizados | Tamanho dos dados transferidos (carregamento e transferência) | Grupo de sincronização, o ponto final do servidor |
  | Lembre-se camadas da cloud | Tamanho dos dados recuperados | Servidores Registados |
  | Os ficheiros não estão a sincronizar | Contagem de ficheiros que estão a falhar para sincronizar | Ponto final do servidor |
  | Ficheiros sincronizados | Contagem de ficheiros transferidos (carregamento e transferência) | Grupo de sincronização, o ponto final do servidor |
  | Heartbeat do servidor | Contagem de heartbeats recebido do servidor | Servidores Registados |

- Para obter mais informações, consulte [do Azure Monitor](https://docs.microsoft.com/azure/storage/files/storage-sync-files-monitoring#azure-monitor) secção. 

  > [!Note]  
  > Os gráficos no portal do serviço de sincronização de armazenamento tem um intervalo de tempo de 24 horas. Para ver os intervalos de tempo diferente ou dimensões, utilize o Azure Monitor.


### <a name="azure-monitor"></a>Azure Monitor

Uso [do Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) para monitorizar a sincronização de camada de cloud e conetividade do servidor. Métricas para o Azure File Sync estão ativadas por predefinição e são enviadas para o Azure Monitor a cada 15 minutos.

Para ver métricas de sincronização de ficheiros do Azure no Azure Monitor, selecione o tipo de recurso de serviços de sincronização de armazenamento.

As métricas seguintes para o Azure File Sync estão disponíveis no Azure Monitor:

| Nome da métrica | Descrição |
|-|-|
| Bytes sincronizados | Tamanho dos dados transferidos (carregamento e transferência).<br><br>Unidade: Bytes<br>Tipo de agregação: Soma<br>Dimensões aplicável: Nome de servidor Endpoint nome, sincronização direção, grupo de sincronização |
| Lembre-se camadas da cloud | Tamanho dos dados recuperados.<br><br>Unidade: Bytes<br>Tipo de agregação: Soma<br>Dimensão aplicável: Nome do Servidor |
| Os ficheiros não estão a sincronizar | Contagem de ficheiros que estão a falhar para sincronizar.<br><br>Unidade: Contagem<br>Tipo de agregação: Soma<br>Dimensões aplicável: Nome de servidor Endpoint nome, sincronização direção, grupo de sincronização |
| Ficheiros sincronizados | Contagem de ficheiros transferidos (carregamento e transferência).<br><br>Unidade: Contagem<br>Tipo de agregação: Soma<br>Dimensões aplicável: Nome de servidor Endpoint nome, sincronização direção, grupo de sincronização |
| Heartbeat do servidor | Número de heartbeats recebido do servidor.<br><br>Unidade: Contagem<br>Tipo de agregação: Máximo<br>Dimensão aplicável: Nome do Servidor |
| Resultado da sessão de sincronização | Resultado da sessão de sincronização (1 = a sincronização com êxito sessão; 0 = a sessão de sincronização com falha)<br><br>Unidade: Contagem<br>Tipos de agregação: Máximo<br>Dimensões aplicável: Nome de servidor Endpoint nome, sincronização direção, grupo de sincronização |

## <a name="windows-server"></a>Windows Server

No Windows Server, pode ver na cloud em camadas, servidor registado e sincronizar o estado de funcionamento.

### <a name="event-logs"></a>Registos de eventos

Utilize o registo de eventos de telemetria no servidor para monitorizar o servidor registado, sincronização e estado de funcionamento de camada de cloud. O registo de eventos de telemetria está localizado em Applications and Services\Microsoft\FileSync\Agent no Visualizador de eventos.

Sync Health
- ID de evento 9102 é registado quando uma sessão de sincronização é concluída. Este evento deve ser utilizado para determinar se sessões de sincronização são concluídos com êxito (HResult = 0) e se existem por item erros de sincronização. Consulte a seguinte documentação para obter mais informações: [Sincronizar o estado de funcionamento](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) & [erros por Item](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing).

  > [!Note]  
  > Às vezes, as sessões de sincronização falhar em geral ou ter um PerItemErrorCount diferente de zero, mas ainda manter o andamento, com alguns arquivos a sincronização com êxito. Isso pode ser visto nos aplicado campos (AppliedFileCount, AppliedDirCount, AppliedTombstoneCount e AppliedSizeBytes), que lhe dizer quanto da sessão é que os sucedem. Se vir várias sessões de sincronização numa linha que estão a falhar, mas têm uma contagem de aplicados de cada vez maior, em seguida, deve dar tempo de sincronização para tentar novamente antes de abrir um pedido de suporte.

- ID de evento 9302 é registado a cada 5 a 10 minutos, se existe uma sessão de sincronização do Active Directory. Este evento deve ser utilizado para determinar se a sessão de sincronização atual está fazendo progresso (AppliedItemCount > 0). Se a sincronização não está fazendo progressos, a sessão de sincronização deve, eventualmente, falhar e uma 9102 de ID de evento será registado com o erro. Consulte a seguinte documentação para obter mais informações: [Progresso da sincronização](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-monitor-the-progress-of-a-current-sync-session)

Estado de funcionamento do servidor registado
- ID de evento 9301 é registado quando um servidor de consulta o serviço para as tarefas a cada 30 segundos. Se GetNextJob for concluída com o estado = 0, o servidor é capaz de comunicar com o serviço. Se GetNextJob for concluída com um erro, consulte a [documentação de resolução de problemas](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#common-sync-errors) para obter orientações.

Cloud Tiering Health
- Para monitorizar a atividade de disposição em camadas num servidor, utilize 9003 de ID de evento, 9016 e 9029 no registo de eventos de telemetria (localizado em Applications and Services\Microsoft\FileSync\Agent no Visualizador de eventos).

  - ID de evento 9003 fornece distribuição de erros para um ponto final do servidor. Por exemplo, Contagem Total de erros, código de erro, etc. Tenha em atenção de que um evento é registado por código de erro.
  - ID de evento 9016 fornece resultados à conversão em fantasma para um volume. Por exemplo, o espaço livre é de %, número de ficheiros fantasma na sessão, o número de ficheiros não conseguiu ghost, etc.
  - ID de evento 9029 fornece informações de sessão à conversão em fantasma para um ponto final do servidor. Por exemplo, o número de ficheiros tentada a sessão, o número de ficheiros em camadas na sessão, número de ficheiros já em camadas, etc.
  
- Para monitorizar a atividade de recolhimento num servidor, utilize 9005 de ID de evento, 9006, 9009 e 9059 no registo de eventos de telemetria (localizado em Applications and Services\Microsoft\FileSync\Agent no Visualizador de eventos).

  - ID de evento 9005 fornece confiabilidade de recolhimento de um ponto de final do servidor. Por exemplo, Total de ficheiros exclusivos acedidos, Total de ficheiros exclusivo com acesso falhadas, etc.
  - ID de evento 9006 fornece distribuição de erros de recolhimento de um ponto de final do servidor. Por exemplo, Total de pedidos falhados, código de erro, etc. Tenha em atenção de que um evento é registado por código de erro.
  - ID de evento 9009 fornece informações de sessão de recolhimento de um ponto de final do servidor. Por exemplo, DurationSeconds, CountFilesRecallSucceeded, CountFilesRecallFailed, etc.
  - ID de evento 9059 fornece distribuição de solicitação de recolhimento de aplicativo para um ponto final do servidor. Por exemplo, ShareId, nome da aplicação e TotalEgressNetworkBytes.

### <a name="performance-counters"></a>Contadores de desempenho

Utilize os contadores de desempenho de sincronização de ficheiros do Azure no servidor para monitorizar a atividade de sincronização.

Para ver o desempenho do Azure File Sync contadores no servidor, iniciar o Monitor de desempenho (Perfmon.exe) e os contadores podem ser encontrados em objetos de transferidos Bytes a AFS e AFS operações de sincronização.

Os seguintes contadores de desempenho para o Azure File Sync estão disponíveis no Monitor de desempenho:

| Nome de Object\Counter de desempenho | Descrição |
|-|-|
| AFS Bytes Transferred\Downloaded Bytes/seg | Número de bytes transferidos por segundo. |
| AFS Bytes Transferred\Uploaded Bytes/seg | Número de bytes carregados por segundo. |
| AFS Bytes Transferred\Total Bytes/seg | Total de bytes por segundo (carregamento e transferência). |
| AFS sincronização Operations\Downloaded sincronização ficheiros/seg | Número de ficheiros transferidos por segundo. |
| AFS sincronização Operations\Uploaded sincronização ficheiros/seg | Número de ficheiros carregados por segundo. |
| AFS sincronização Operations\Total sincronização ficheiro operações/seg | Número total de ficheiros sincronizados (carregamento e transferência). |

## <a name="next-steps"></a>Passos Seguintes
- [Planear uma implementação do Azure File Sync](storage-sync-files-planning.md)
- [Considere as definições de proxy e firewall](storage-sync-files-firewall-and-proxy.md)
- [Implementar Azure File Sync](storage-sync-files-deployment-guide.md)
- [Resolver problemas da sincronização de ficheiros do Azure](storage-sync-files-troubleshoot.md)
- [Perguntas mais frequentes sobre os ficheiros do Azure](storage-files-faq.md)
