---
title: Utilize a análise de armazenamento do Azure para recolher dados de registos e métricas | Documentos da Microsoft
description: Análise de armazenamento permite-lhe para controlar os dados de métricas para todos os serviços de armazenamento e para recolher registos para o armazenamento de BLOBs, filas e tabelas.
services: storage
author: roygara
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 03/03/2017
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: 1b27bbaa3d8e570c8431708934edee564e994487
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/05/2019
ms.locfileid: "55745661"
---
# <a name="storage-analytics"></a>Análise de Armazenamento

A Análise de Armazenamento do Azure leva a cabo o registo e fornece dados métricos relativamente a uma conta de armazenamento. Pode utilizar estes dados para rastrear pedidos, analisar tendências de utilização e diagnosticar problemas relacionados com a sua conta de armazenamento.

Para utilizar a análise de armazenamento, terá de a ativar individualmente para cada serviço que pretende monitorizar. Pode ativá-lo a partir da [Portal do Azure](https://portal.azure.com). Para obter detalhes, consulte [monitorizar uma conta de armazenamento no Portal do Azure](storage-monitor-storage-account.md). Também pode ativar a análise de armazenamento através de programação com a API REST ou a biblioteca de cliente. Utilize o [obter as propriedades do serviço Blob](https://msdn.microsoft.com/library/hh452239.aspx), [obter as propriedades do serviço de fila](https://msdn.microsoft.com/library/hh452243.aspx), [obter as propriedades do serviço de tabela](https://msdn.microsoft.com/library/hh452238.aspx), e [obter as propriedades do serviço de arquivo](https://msdn.microsoft.com/library/mt427369.aspx)operações para ativar a análise de armazenamento para cada serviço.

Os dados agregados são armazenados num blob bem conhecido (para o registo) e em tabelas bem conhecidas (para métricas), que podem ser acessadas utilizando o serviço de BLOBs e as APIs de serviço de tabela.

Análise de armazenamento tem um limite de 20 TB na quantidade de dados armazenados, que é independentes do limite total para a sua conta de armazenamento. Para obter mais informações sobre faturação e políticas de retenção de dados, consulte [a análise de armazenamento e de faturação](https://msdn.microsoft.com/library/hh360997.aspx). Para obter mais informações sobre os limites de conta de armazenamento, consulte [metas de desempenho e escalabilidade do armazenamento do Azure](storage-scalability-targets.md).

Para obter um guia detalhado sobre como utilizar a análise de armazenamento e outras ferramentas para identificar, diagnosticar e resolver problemas relacionados com o armazenamento do Azure, consulte [monitorizar, diagnosticar e resolver problemas de armazenamento do Microsoft Azure](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="about-storage-analytics-logging"></a>Sobre o registo de análise de armazenamento
Análise de armazenamento regista informações detalhadas sobre pedidos com êxito ou falhadas para um serviço de armazenamento. Estas informações podem ser utilizadas para monitorizar pedidos individuais e para diagnosticar problemas com um serviço de armazenamento. Pedidos são registados numa base de melhor esforço.

Entradas de registo são criadas apenas se existe atividade do serviço de armazenamento. Por exemplo, se uma conta de armazenamento tiver uma atividade no seu serviço de Blob, mas não no respetivos serviços de tabelas ou filas, serão criados apenas os registos relativos ao serviço de Blobs.

O registo de análise de armazenamento não está disponível para ficheiros do Azure.

### <a name="logging-authenticated-requests"></a>Pedidos de registo autenticado
Os seguintes tipos de pedidos autenticados são registados:

* Pedidos com êxito.
* Pedidos falhados, incluindo o tempo limite, limitação, rede, autorização e outros erros.
* Pedidos com um acesso assinatura partilhado (SAS), incluindo pedidos de falhadas e bem-sucedidas.
* Pedidos de dados de análise.

Pedidos efetuados por análise de armazenamento em si, como o registo é criada ou eliminada, não tem sessão iniciados. Uma lista completa dos dados com sessão iniciada está documentada no [operações com sessão iniciada da análise de armazenamento e as mensagens de estado](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) e [formato de registo de análise de armazenamento](/rest/api/storageservices/storage-analytics-log-format) tópicos.

### <a name="logging-anonymous-requests"></a>Registo de pedidos anónimos
Os seguintes tipos de solicitações anônimas são registados:

* Pedidos com êxito.
* Erros de servidor.
* Erros de tempo limite de cliente e servidor.
* OBTER pedidos falhados com código de erro 304 (não é modificada).

Todos os outros pedidos anónimos com falhas não são registados. Uma lista completa dos dados com sessão iniciada está documentada no [operações com sessão iniciada da análise de armazenamento e as mensagens de estado](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) e [formato de registo de análise de armazenamento](/rest/api/storageservices/storage-analytics-log-format) tópicos.

### <a name="how-logs-are-stored"></a>Como os registos são armazenados
Todos os registos são armazenados em blobs de blocos num contentor com o nome $logs, que é criado automaticamente quando a análise de armazenamento é ativada para uma conta de armazenamento. O contentor de $logs está localizado no espaço de nomes de BLOBs da conta de armazenamento, por exemplo: `http://<accountname>.blob.core.windows.net/$logs`. Não é possível eliminar este contentor assim que tiver sido ativada a análise de armazenamento, embora seu conteúdo pode ser eliminado.

> [!NOTE]
> O contentor de $logs não é apresentado quando é executado um operação de listagem de contentor, como o [ListContainers](https://msdn.microsoft.com/library/azure/dd179352.aspx) método. Tem de ser acedido diretamente. Por exemplo, pode utilizar o [ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx) método para aceder aos blobs a `$logs` contentor.
> Que as solicitações são registadas, análise de armazenamento irá carregar os resultados intermediários como blocos. Periodicamente, análise de armazenamento irá consolidar esses blocos e disponibilizá-los como um blob.
>
>

Registos duplicados podem existir para logs criados na mesma hora. Pode determinar se um registo é um duplicado, verificando a **RequestId** e **operação** número.

### <a name="log-naming-conventions"></a>Registo de convenções de nomenclatura
Cada registo será gravado no seguinte formato.

    <service-name>/YYYY/MM/DD/hhmm/<counter>.log

A tabela seguinte descreve cada atributo no nome do registo.

| Atributo | Descrição |
| --- | --- |
| <service-name> |O nome do serviço de armazenamento. Por exemplo: BLOBs, tabelas ou filas. |
| AAAA |O ano de quatro dígitos para o log. Por exemplo: 2011. |
| MM |O mês de dois dígitos para o log. Por exemplo: 07. |
| DD |O mês de dois dígitos para o log. Por exemplo: 07. |
| hh |A hora de dois dígitos que indica a hora de partida para os registos, no formato UTC de 24 horas. Por exemplo: 18. |
| mm |O número de dois dígitos que indica o minuto de partida para os registos. Este valor não é suportado na versão atual da análise de armazenamento e seu valor será sempre 00. |
| <counter> |Um contador baseado em zero com seis dígitos que indica o número de blobs de registo gerados para o serviço de armazenamento numa hora de período de tempo. Este contador é US 000000. Por exemplo: 000001. |

Segue-se um nome de registo de exemplo completo que combina os exemplos anteriores.

    blob/2011/07/31/1800/000001.log

Segue-se um exemplo de URI que pode ser utilizado para aceder ao registo anterior.

    https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log

Quando um pedido de armazenamento tem sessão iniciado, o nome do registo resultante está correlacionada com a hora quando concluir a operação pedida. Por exemplo, se um pedido de GetBlob foi concluído às 18h00: 30. no 7/31/2011, seria escrito o registo com o seguinte prefixo: `blob/2011/07/31/1800/`

### <a name="log-metadata"></a>Metadados de registo
Todos os blobs de registo são armazenados com metadados que podem ser utilizados para identificar os dados de registo que contém o blob. A tabela seguinte descreve cada atributo de metadados.

| Atributo | Descrição |
| --- | --- |
| LogType |Descreve se o registo contém informações relativas à leitura, gravação ou operações de eliminação. Este valor pode incluir um tipo ou uma combinação de todos os três, separados por vírgulas. Exemplo 1: escrever; Exemplo 2: ler, escrever; Exemplo 3: leitura, escrita e eliminação. |
| StartTime |A primeira hora de uma entrada no registo, o formato AAAA-MM-: ssZ. Por exemplo: 2011-07-31T18:21:46Z. |
| endTime |O tempo mais recente de uma entrada no registo, o formato AAAA-MM-: ssZ. Por exemplo: 2011-07-31T18:22:09Z. |
| LogVersion |A versão do formato de registo. Atualmente, o único valor suportado é 1.0. |

A lista seguinte apresenta os metadados de exemplo completo com os exemplos anteriores.

* LogType=write
* StartTime=2011-07-31T18:21:46Z
* EndTime=2011-07-31T18:22:09Z
* LogVersion=1.0

### <a name="accessing-logging-data"></a>Aceder aos dados de registo
Todos os dados no `$logs` contentor podem ser acedidos ao utilizar as APIs do serviço de BLOBs, incluindo as APIs de .NET fornecido pelo Azure biblioteca gerida. O administrador de conta de armazenamento pode ler e eliminar registos, mas não é possível criar ou atualizá-los. Metadados do registo e o nome do registo podem ser utilizados ao consultar para um registo. É possível para os registos de uma determinada hora a aparecer fora de ordem, mas os metadados sempre Especifica o período de tempo de entradas de registo num registo. Por conseguinte, pode utilizar uma combinação de nomes de registo e de metadados ao pesquisar um log específico.

## <a name="about-storage-analytics-metrics"></a>Sobre as métricas de análise de armazenamento
Análise de armazenamento pode armazenar métricas que incluem dados de estatísticas e a capacidade de agregadas de transações sobre pedidos para um serviço de armazenamento. Transações são comunicadas ao nível da operação ambas API, bem como o nível de serviço de armazenamento e capacidade é comunicada o nível de serviço de armazenamento. Dados de métricas podem ser utilizados para analisar a utilização do serviço de armazenamento, diagnóstico de problemas com pedidos efetuados para o serviço de armazenamento e para melhorar o desempenho das aplicações que utilizam um serviço.

Para utilizar a análise de armazenamento, terá de a ativar individualmente para cada serviço que pretende monitorizar. Pode ativá-lo a partir da [Portal do Azure](https://portal.azure.com). Para obter detalhes, consulte [monitorizar uma conta de armazenamento no Portal do Azure](storage-monitor-storage-account.md). Também pode ativar a análise de armazenamento através de programação com a API REST ou a biblioteca de cliente. Utilize o **obter as propriedades do serviço** operações para ativar a análise de armazenamento para cada serviço.

### <a name="transaction-metrics"></a>Métricas de transação
Um robusto conjunto de dados é registrado em intervalos por hora ou minutos para cada serviço de armazenamento e a operação de API, incluindo a entrada/saída, disponibilidade, erros, pedida e categorizadas percentagens de pedido. Pode ver uma lista completa dos detalhes da transação no [esquema de tabela de métricas de análise de armazenamento](https://msdn.microsoft.com/library/hh343264.aspx) tópico.

Dados de transações são registrados em dois níveis – o nível de serviço e o nível de operação de API. No nível de serviço, estatísticas que resume todas pediu a operações de API são escritas para uma entidade de tabela por hora, mesmo que não existem pedidos foram feitos para o serviço. No nível de operação da API, as estatísticas só são escritas para uma entidade se a operação foi pedida nessa hora.

Por exemplo, se efetuar uma **GetBlob** operação no seu serviço de BLOBs, métricas da análise de armazenamento irá registrar a solicitação e incluí-lo nos dados agregados para o serviço Blob, bem como a **GetBlob** operação. No entanto, se nenhum **GetBlob** operação é solicitada durante a hora, uma entidade será não ser escrita para o `$MetricsTransactionsBlob` para essa operação.

Métricas de transação são registradas para pedidos de utilizadores e pedidos efetuados por análise de armazenamento em si. Por exemplo, são registados pedidos pela análise de armazenamento para escrever registos e as entidades da tabela. Para obter mais informações sobre como estes pedidos são cobrados, consulte [a análise de armazenamento e de faturação](https://msdn.microsoft.com/library/hh360997.aspx).

### <a name="capacity-metrics"></a>Métricas de capacidade
> [!NOTE]
> Atualmente, as métricas de capacidade só estão disponíveis para o serviço de Blobs.
>
>

Dados de capacidade é registados diariamente para serviço de uma conta de armazenamento de BLOBs e duas entidades de tabela são escritas. Uma entidade fornece estatísticas dos dados do usuário e o outro fornece estatísticas sobre o `$logs` utilizado pela análise de armazenamento de contentor de Blobs. O `$MetricsCapacityBlob` tabela inclui as estatísticas seguintes:

* **Capacidade**: A quantidade de armazenamento utilizada pelo serviço de Blob da conta de armazenamento, em bytes.
* **ContainerCount**: O número de contentores de BLOBs no serviço de Blob da conta de armazenamento.
* **ObjectCount**: O número de consolidada e não consolidados bloco ou página blobs no serviço de Blob da conta de armazenamento.

Para obter mais informações sobre as métricas de capacidade, consulte [esquema de tabela de métricas de análise de armazenamento](/rest/api/storageservices/storage-analytics-metrics-table-schema).

### <a name="how-metrics-are-stored"></a>Como as métricas são armazenadas
Todos os dados de métricas para cada um dos serviços de armazenamento são armazenados em três tabelas reservadas para esse serviço: uma tabela para obter informações de transação, uma tabela para informações sobre transações minuto e outra tabela para obter informações de capacidade. Informações de transação de transação e o minuto consistem em dados de solicitação e resposta e informações de capacidade consiste em armazenamento dados de utilização. Métrica de hora, a métrica de minuto e a capacidade para o serviço de Blob de uma conta de armazenamento podem ser acedidos em tabelas com o nome, tal como descrito na tabela seguinte.

| Nível de métricas | Nomes de tabelas | Versões suportadas |
| --- | --- | --- |
| Métricas de hora a hora, localização primária |$MetricsTransactionsBlob <br/>$MetricsTransactionsTable <br/> $MetricsTransactionsQueue |Versões anteriores a 2013-08-15 apenas. Embora esses nomes ainda são suportados, recomenda-se que mude para utilizar as tabelas listadas abaixo. |
| Métricas de hora a hora, localização primária |$MetricsHourPrimaryTransactionsBlob <br/>$MetricsHourPrimaryTransactionsTable <br/>$MetricsHourPrimaryTransactionsQueue |Todas as versões, incluindo 08-2013-15. |
| Métrica de minuto, localização primária |$MetricsMinutePrimaryTransactionsBlob <br/>$MetricsMinutePrimaryTransactionsTable <br/>$MetricsMinutePrimaryTransactionsQueue |Todas as versões, incluindo 08-2013-15. |
| Métricas de hora a hora, localização secundária |$MetricsHourSecondaryTransactionsBlob <br/>$MetricsHourSecondaryTransactionsTable <br/>$MetricsHourSecondaryTransactionsQueue |Todas as versões, incluindo 08-2013-15. Replicação georredundante de acesso de leitura tem de estar ativada. |
| Métrica de minuto, localização secundária |$MetricsMinuteSecondaryTransactionsBlob <br/>$MetricsMinuteSecondaryTransactionsTable <br/>$MetricsMinuteSecondaryTransactionsQueue |Todas as versões, incluindo 08-2013-15. Replicação georredundante de acesso de leitura tem de estar ativada. |
| Capacidade (serviço de BLOBs apenas) |$MetricsCapacityBlob |Todas as versões, incluindo 08-2013-15. |

Essas tabelas são criadas automaticamente quando a análise de armazenamento é ativada para uma conta de armazenamento. Estes são acedidos através do espaço de nomes da conta de armazenamento, por exemplo: `https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")`

### <a name="accessing-metrics-data"></a>Aceder aos dados de métricas
Todos os dados nas tabelas métricas podem ser acedidos ao utilizar as APIs do serviço de tabela, incluindo as APIs de .NET fornecido pelo Azure biblioteca gerida. O administrador de conta de armazenamento pode ler e eliminar entidades de tabelas, mas não é possível criar ou atualizá-los.

## <a name="billing-for-storage-analytics"></a>Faturação de análise de armazenamento
Todos os dados de métricas é escrito pelos serviços de uma conta de armazenamento. Como resultado, cada operação de escrita efetuada por análise de armazenamento está sujeitos a faturação. Além disso, a quantidade de armazenamento utilizada pelo dados das métricas também está sujeitos a faturação.

As seguintes ações realizadas por análise de armazenamento são a cobrar:

* Pedidos para criar blobs para o registo.
* Pedidos para criar entidades da tabela de métricas.

Se tiver configurado uma política de retenção de dados, não é cobradas para a eliminação de transações quando a análise de armazenamento elimina dados antigos de registo e as métricas. No entanto, a eliminação de transações de um cliente é a cobrar. Para obter mais informações sobre as políticas de retenção, consulte [definir uma política de retenção de dados de análise de armazenamento](https://msdn.microsoft.com/library/azure/hh343263.aspx).

### <a name="understanding-billable-requests"></a>Noções básicas sobre pedidos faturáveis
Cada solicitação feita ao serviço de uma conta de armazenamento está sujeito a faturação ou não faturável. Análise de armazenamento de registos cada pedidos individuais efetuados para um serviço, incluindo uma mensagem de estado que indica como o pedido foi processado. Da mesma forma, a análise de armazenamento armazena as métricas para um serviço e as operações de API do serviço, incluindo as percentagens e a contagem de determinadas mensagens de estado. Juntos, esses recursos podem ajudar a analisar seus pedidos faturáveis, fazer melhorias na sua aplicação e diagnosticar problemas com os pedidos aos seus serviços. Para obter mais informações sobre a faturação, consulte [Noções básicas sobre faturação do Azure Storage - largura de banda, transações e capacidade](https://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

Ao analisar os dados de análise de armazenamento, pode utilizar as tabelas a [operações com sessão iniciada da análise de armazenamento e as mensagens de estado](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) tópico para determinar que pedidos são a cobrar. Em seguida, pode comparar os dados de métricas para as mensagens de estado para ver se foram bem cobrados por uma solicitação específica e registos. Também pode utilizar as tabelas no tópico anterior para investigar a disponibilidade para um serviço de armazenamento ou a operação de API individual.

## <a name="next-steps"></a>Passos Seguintes
* [Monitorizar uma conta de armazenamento no Portal do Azure](storage-monitor-storage-account.md)
* [Registo de análise de armazenamento](https://msdn.microsoft.com/library/hh343262.aspx)
* [Métricas da análise de armazenamento](https://msdn.microsoft.com/library/hh343258.aspx)
