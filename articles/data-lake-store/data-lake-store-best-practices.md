---
title: Melhores práticas para utilizar o Azure Data Lake Store | Microsoft Docs
description: Saiba mais sobre a ingestão de dados, a segurança de data e o desempenho relacionadas com a utilização do Azure Data Lake Store, as melhores práticas
services: data-lake-store
documentationcenter: ''
author: sachinsbigdata
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.date: 03/02/2018
ms.author: sachins
ms.openlocfilehash: ac0a01ed7a067688732aa54eb1b76e0e299e4263
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="best-practices-for-using-azure-data-lake-store"></a>Melhores práticas para utilizar o Azure Data Lake Store
Neste artigo, pode saber mais sobre as melhores práticas e considerações para trabalhar com o Azure Data Lake Store. Este artigo fornece informações em torno da segurança, desempenho, resiliência e monitorização para o Data Lake Store. Antes de Data Lake Store, como trabalhar com macrodados verdadeiramente nos serviços como o Azure HDInsight era demasiado complexo. Era necessário dividir os dados em várias contas do Blob storage para que podem ser alcançados petabyte armazenamento e um desempenho ideal que escala. Com o Data Lake Store, a maioria dos limites de tamanho e o desempenho do disco rígidos é removida. No entanto, ainda existem algumas considerações que este artigo abrange, de modo a que possa obter o melhor desempenho com o Data Lake Store. 

## <a name="security-considerations"></a>Considerações de segurança

O Azure Data Lake Store oferece acesso POSIX controla e detalhadas de auditoria para o Azure Active Directory (Azure AD) utilizadores, grupos e principais de serviço. Estes controlos de acesso podem ser definidos como pastas e ficheiros existentes. Os controlos de acesso também podem ser utilizados para criar as predefinições que podem ser aplicadas a novos ficheiros ou pastas. Quando as permissões estão definidas como pastas existentes e os objetos subordinados, as permissões têm de ser propagado em modo recursivo em cada objeto. Se existir grande número de ficheiros, propagar as permissões pode demorar muito tempo. O tempo que pode variar entre objetos de 30 a 50 processados por segundo. Por conseguinte, planear adequadamente os grupos de utilizador e a estrutura da pasta. Caso contrário, pode causar atrasos inesperados e problemas quando trabalha com os seus dados. 

Partem do princípio de que tem uma pasta com 100.000 objetos subordinados. Se não efetuar o limite inferior de 30 objetos processados por segundo, ao atualizar a permissão para a pasta completa pode demorar uma hora. Estão disponíveis mais detalhes no Data Lake Store ACLs em [controlo de acesso no Azure Data Lake Store](data-lake-store-access-control.md). Para um melhor desempenho na atribuição recursivamente ACLs, pode utilizar a ferramenta de linha de comandos do Azure Data Lake. A ferramenta cria vários threads e lógica de navegação recursiva aplicarem rapidamente as ACLs para milhões de ficheiros. A ferramenta está disponível para Linux e Windows e o [documentação](https://github.com/Azure/data-lake-adlstool) e [transfere](http://aka.ms/adlstool-download) para esta ferramenta pode ser encontrada no GitHub. Estas melhorias de desempenho mesmo podem ser ativadas utilizando as suas próprias ferramentas escritas com o Data Lake Store [.NET](data-lake-store-data-operations-net-sdk.md) e [Java](data-lake-store-get-started-java-sdk.md) SDKs.

### <a name="use-security-groups-versus-individual-users"></a>Utilizar grupos de segurança por oposição aos utilizadores individuais 

Ao trabalhar com macrodados no Data Lake Store, provavelmente um principal de serviço é utilizado para permitir que os serviços como o Azure HDInsight para trabalhar com os dados. No entanto, poderão existir casos onde os utilizadores individuais têm acesso aos dados, bem como. Nestes casos, tem de utilizar o Azure Active Directory [grupos de segurança](data-lake-store-secure-data.md#create-security-groups-in-azure-active-directory) em vez de atribuir utilizadores individuais para pastas e ficheiros. 

Depois de um grupo de segurança é atribuído permissões, adição ou remoção de utilizadores do grupo não requer quaisquer atualizações para o Data Lake Store. Isto também ajuda a garantir que não excede o limite de [acesso 32 e predefinido ACLs](../azure-subscription-service-limits.md#data-lake-store-limits) (Isto inclui as quatro ACLs do estilo POSIX associados sempre todos os ficheiros e pastas: [o utilizador proprietário](data-lake-store-access-control.md#the-owning-user), [o grupo de proprietário](data-lake-store-access-control.md#the-owning-group), [a máscara](data-lake-store-access-control.md#the-mask-and-effective-permissions)e outros).

### <a name="security-for-groups"></a>Segurança para grupos 

Tal como abordado, quando os utilizadores precisam de aceder ao Data Lake Store, é melhor utilizar grupos de segurança do Azure Active Director. Algumas recomendado grupos começar com poderão ser **ReadOnlyUsers**, **WriteAccessUsers**, e **FullAccessUsers** para a raiz da conta e até mesmo separar aqueles para a chave subpastas. Se existirem quaisquer outros prevista dos grupos de utilizadores que podem ser adicionados mais tarde, mas que não tenham sido identificados ainda, podem considerar a criar grupos de segurança fictício que têm acesso de determinadas pastas. Utilizar o grupo de segurança garante que mais tarde não necessita de algum tempo de processamento para atribuir permissões de novo a milhares de ficheiros. 

### <a name="security-for-service-principals"></a>Segurança de principais de serviço 

Principais de serviço do Azure Active Directory são normalmente utilizadas por serviços como o Azure HDInsight para aceder a dados no Data Lake Store. Consoante os requisitos de acesso através de várias cargas de trabalho, poderão existir algumas considerações para garantir a segurança dentro e fora da organização. Para muitos clientes, um principal de serviço do Azure Active Directory única pode ser adequado e pode ter permissões totais na raiz do Data Lake Store. Outros clientes poderão necessitar de vários clusters com os principais de serviço diferentes em que um cluster tem acesso total aos dados e o outro cluster com acesso só de leitura. Tal como acontece com os grupos de segurança, poderá considerar efetuar um principal de serviço para cada antecipado cenário (leitura, escrita, completa) quando é criada uma conta de Data Lake Store. 

### <a name="enable-the-data-lake-store-firewall-with-azure-service-access"></a>Ativar a firewall do Data Lake Store com acesso de serviço do Azure 

Data Lake Store suporta a opção de ativar uma firewall e limitar o acesso apenas aos serviços do Azure, que é recomendado para um vetor de ataque mais pequeno de intrusions fora. Firewall pode ser ativada na conta de Data Lake Store no portal do Azure através de **Firewall** > **ativar Firewall (ON)** > **permitir o acesso aos serviços do Azure**  opções.  

![Firewall de definições no Data Lake Store](./media/data-lake-store-best-practices/data-lake-store-firewall-setting.png "Firewall definições no Data Lake Store")

Assim que a firewall estiver ativados, apenas Azure serviços como o HDInsight, o Data Factory, o SQL Data Warehouse, etc. têm acesso ao Data Lake Store. Devido a tradução de endereços de rede interna utilizada pelo Azure, o Data Lake Store não suporta a restringir a serviços específicos por IP e firewall destina-se apenas de restrições de pontos finais fora do Azure, tal como no local. 

## <a name="performance-and-scale-considerations"></a>Considerações de desempenho e dimensionamento

Uma das funcionalidades mais poderosas do Data Lake Store é que remove os limites de débito de dados no disco rígidos. Remover os limites permite que os clientes aumentar o tamanho de dados e acompanhado requisitos de desempenho sem necessitar de partições horizontais os dados. Um das considerações mais importantes para otimizar o desempenho do Data Lake Store é que executa o melhor quando fornecido paralelismo.

### <a name="improve-throughput-with-parallelism"></a>Melhorar o débito com paralelismo 

Considere atribuir às 8 12 threads por núcleo para o débito de leitura/escrita ideal. Isto acontece porque está a bloquear leituras/escritas num único thread e threads mais podem permitir simultaneidade superior na VM. Para garantir que os níveis de estão em bom estado de funcionamento e paralelismo pode ser aumentado, lembre-se de que monitorizar a utilização da CPU da VM.   

### <a name="avoid-small-file-sizes"></a>Evitar tamanhos de ficheiro pequeno

Permissões de POSIX e auditoria no Data Lake Store é fornecido com uma sobrecarga que torna-se aparente ao trabalhar com vários ficheiros pequenos. Como melhor prática, tem de lote os dados em ficheiros maiores versus escrever milhares ou milhões de pequenos ficheiros para o Data Lake Store. Evitar tamanhos de ficheiro pequeno pode ter várias vantagens, tais como:

* Reduzir as verificações de autenticação através de vários ficheiros
* Reduzido ligações ficheiro aberto
* Copiar mais rápida/replicação.
* Menos ficheiros para processar ao atualizar as permissões de POSIX do Data Lake Store 

Dependendo de que os serviços e as cargas de trabalho utilizar os dados, um intervalo de boa a considerar para tamanhos de ficheiro é de 256 MB para 1 GB, idealmente não vai abaixo de 100 MB ou superior a 2 GB. Se os tamanhos de ficheiro não não possível criar batch quando o destino no Data Lake Store, poderá ter uma tarefa de compactação separado que combine estes ficheiros aqueles maior. Para obter mais informações e recomendação em tamanhos de ficheiro e organizar os dados no Data Lake Store, consulte [estrutura do conjunto de dados](data-lake-store-performance-tuning-guidance.md#structure-your-data-set). 

### <a name="large-file-sizes-and-potential-performance-impact"></a>Tamanhos de ficheiros grandes e o potencial impacto do desempenho 

Embora o Data Lake Store suporta ficheiros grandes até petabytes de tamanho, para um desempenho ideal e consoante o processo de leitura de dados, poderá não ser ideal ir acima 2 GB em média. Por exemplo, quando utilizar **Distcp** para copiar dados entre localizações ou contas de armazenamento diferentes, os ficheiros são o finest nível de granularidade utilizado para determinar as tarefas de mapa. Por isso, se estiver a copiar 10 ficheiros que estão a 1 TB, no máximo 10 mappers são alocados. Além disso, se tiver muitos ficheiros com mappers atribuídos, inicialmente os mappers funcionam em paralelo para mover ficheiros grandes. No entanto, como a tarefa começa a vento apenas alguns mappers permanecem alocados e pode estar bloqueada com um único mapeador atribuído a um ficheiro grande. Microsoft apresentou melhoramentos ao Distcp para resolver este problema em versões futuras do Hadoop.  

Outro exemplo a ter em consideração é quando utiliza o Azure Data Lake Analytics com o Data Lake Store. Consoante o processamento efetuado o extrator, alguns ficheiros que não é possível dividir (por exemplo, XML, JSON) foi afetado no desempenho quando é superior a 2 GB. Em casos onde os ficheiros podem ser divididos por um extrator (por exemplo, CSV), ficheiros grandes são preferenciais.

### <a name="capacity-plan-for-your-workload"></a>Planear a capacidade da carga de trabalho 

O Azure Data Lake Store remove e/s de disco rígido os limites que são colocadas em contas do Blob storage. No entanto, existem limites ainda recuperável que precisam de ser considerados. Os limites de limitação de entrada/saída predefinida satisfazem as necessidades da maioria dos cenários. Se a sua carga de trabalho tem de ter os limites aumentados, trabalhar com o suporte da Microsoft. Além disso, observe os limites durante a fase de prova de conceito, para que os limites de limitação de e/s não são de acessos durante produção. Se isto acontecer, exijam espera um aumento manual da equipa de engenharia da Microsoft. Se ocorrer a limitação de e/s, o Azure Data Lake Store devolve um código de erro de 429 e Idealmente, deve ser repetido com uma política de término exponencial adequado. 

### <a name="optimize-writes-with-the-data-lake-store-driver-buffer"></a>Otimizar "escreve" com a memória intermédia de controlador de Data Lake Store 

Para otimizar o desempenho e reduzir o IOPS ao escrever para o Data Lake Store do Hadoop, execute operações de escrita tão próximos o tamanho de memória intermédia de controlador do Data Lake Store quanto possível. Tente não exceder o tamanho da memória intermédia antes de libertar, tal como quando utiliza o Apache Storm de transmissão em fluxo ou cargas de trabalho de transmissão em fluxo do Spark. Quando são escritos para o Data Lake Store do HDInsight/Hadoop, é importante saber que o Data Lake Store tem um controlador com uma memória intermédia de 4 MB. Como muitas controladores do sistema de ficheiros, desta memória intermédia possam ser esvaziada manualmente antes de atingir o tamanho de 4 MB. Caso contrário, imediatamente for descarregado para o armazenamento se a escrita seguinte excede o tamanho máximo da memória intermédia. Sempre que possível, deve evitar um overrun ou um underrun significativa da memória intermédia quando a sincronização/limpar política através da janela de contagem ou hora.

## <a name="resiliency-considerations"></a>Considerações de resiliência 

Quando arquitetar um sistema com o Data Lake Store ou de qualquer serviço de nuvem, tem de considerar os requisitos de disponibilidade e como reagir a potenciais interrupções no serviço. Um problema pode ser localizado para a instância específica ou mesmo nível região, por isso, ter um plano para ambas é importante. Consoante o **objetivo de tempo de recuperação** e **objetivo de ponto de recuperação** SLA para a carga de trabalho, pode escolher uma estratégia a mais ou menos agressiva de elevada disponibilidade e recuperação após desastre.

### <a name="high-availability-and-disaster-recovery"></a>Elevada disponibilidade e recuperação após desastre 

Elevada disponibilidade (HA) e recuperação após desastre (DR) podem, por vezes, ser combinados, apesar de cada um tem uma estratégia de ligeiramente diferente, especialmente quando se trata de dados. Arquivo data Lake já processa a replicação x 3 sob definições avançadas para proteger contra falhas de hardware localizada. No entanto, uma vez que a replicação em regiões não está incorporada, tem de gerir este por si. Ao criar um plano para HA, na eventualidade de ocorrer uma interrupção do serviço a carga de trabalho precisa de acesso para os dados mais recentes mais rapidamente possível pelo mudar para uma instância separadamente replicada localmente ou numa região de novo.  

Na estratégia de DR, para se preparar para improvável eventualidade de uma falha catastrófica de uma região, também é importante que tenha dados replicados noutra região. Estes dados inicialmente poderão ser o mesmo que os dados replicados do HA. No entanto, também tem de considerar os requisitos para casos edge como danos nos dados onde poderá pretender criar instantâneos periódicos para reverter para. Consoante a importância e o tamanho dos dados, considere a anular instantâneos delta de 1-, 6 e 24 horas períodos no arquivo local e/ou secundário, de acordo com as tolerâncias de risco. 

Para resiliência de dados com o Data Lake Store, recomenda-georreplicação replicar os dados para uma região separado com uma frequência que satisfaça os requisitos de HA/DR, idealmente, a cada hora. Frequência de replicação minimiza movimentos de dados em grande escala que podem ter precisa de débito concorrente com o sistema principal e um objetivo de ponto de recuperação (RPO) melhor. Além disso, deve considerar formas para a aplicação com o Data Lake Store para efetuar automaticamente a ativação pós-falha para a conta secundária com a monitorização de acionadores ou o comprimento de tentativas falhadas, enviar, pelo menos, uma notificação para administradores de intervenção manual. Tenha em atenção que não há compromisso de falha através do versus à espera de um serviço fique novamente online. Se os dados não foi concluída a replicar, uma ativação pós-falha pode causar potencial perda de dados, inconsistência ou intercalação complexas dos dados. 

Seguem-se as principais três opções recomendadas para a replicação da orquestração entre contas de Data Lake Store e principais diferenças entre cada um deles.


|  |Distcp  |Azure Data Factory  |AdlCopy  |
|---------|---------|---------|---------|
|**Limites de escala**     | Tem um vínculo por nós de trabalho        | Limitado pelas unidades de movimento de dados de nuvem máx.        | Vinculada por unidades de análise        |
|**Suporta copiar deltas**     |   Sim      | Não         | Não         |
|**Orquestração incorporada**     |  Não (utilizar o Oozie Airflow ou cron tarefas)       | Sim        | Não (utilizar a automatização do Azure ou o Programador de tarefas do Windows)         |
|**Sistemas de ficheiros suportados**     | ADL, HDFS, WASB, S3, GS, CFS        |Várias, consulte [conectores](../data-factory/connector-azure-blob-storage.md).         | ADL para ADL, WASB para ADL (mesma região apenas)        |
|**Suporte de SO**     |Qualquer SO em execução de Hadoop         | N/A          | Windows 10         |
   

### <a name="use-distcp-for-data-movement-between-two-locations"></a>Utilizar o Distcp para movimento de dados entre duas localizações 

Curto para cópia distribuída, o Distcp é uma ferramenta de linha de comandos do Linux que vem com o Hadoop e fornece o movimento de dados distribuída entre duas localizações. As duas localizações podem ser Data Lake Store, HDFS, WASB ou S3. Esta ferramenta utiliza as tarefas do MapReduce num cluster de Hadoop (por exemplo, o HDInsight) para aumentar horizontalmente em todos os nós. Distcp é considerado a forma mais rápida para mover os dados de grandes sem aparelhos de compressão de rede especiais. Distcp também fornece uma opção para atualizar apenas deltas entre duas localizações, tentativas automáticas de identificadores, bem como dimensionamento dinâmico de computação. Esta abordagem é incredibly eficiente quando se trata de replicar coisas como tabelas do Hive/Spark que podem ter muitos ficheiros grandes num único diretório e apenas pretende copiar através de dados modificados. Por esta razão, Distcp é a ferramenta mais recomendada para copiar dados entre os arquivos de macrodados. 

Tarefas de cópia podem ser acionadas por fluxos de trabalho do Apache Oozie com frequência ou acionadores de dados, bem como as tarefas de cron do Linux. Para tarefas de replicação que consomem muita, se recomenda a rotação de segurança de um cluster do HDInsight Hadoop separados que pode ser otimizado e ampliado especificamente para as tarefas de cópia. Isto garante que as tarefas de cópia não interferem com tarefas críticas. Se executar o replicação uma frequência suficientemente grande, o cluster pode mesmo ser desativado entre cada tarefa. Se efetuar a ativação pós-falha para a região secundária, certifique-se de que outro cluster é também puserem em marcha na região secundária para replicar dados novos para a conta de Data Lake Store primária assim que volta a funcionar. Para obter exemplos de utilizar o Distcp, consulte [Distcp de utilização para copiar dados entre o Blobs Storage do Azure e a Data Lake Store](data-lake-store-copy-data-wasb-distcp.md).

### <a name="use-azure-data-factory-to-schedule-copy-jobs"></a>Utilizar o Azure Data Factory para agendar tarefas de cópia 

Também pode ser utilizado o Azure Data Factory para agendar tarefas de cópia utilizando um **atividade de cópia**e até mesmo pode ser configurado numa frequência através de **Assistente para copiar**. Tenha em atenção que o Azure Data Factory tem um limite de unidades de movimento de dados de nuvem (DMUs) e, eventualmente, caps a débito de computação para cargas de trabalho de dados de grandes dimensões. Além disso, do Azure Data Factory atualmente não oferecem as atualizações de diferenças entre contas de Data Lake Store, pelo que pastas, como as tabelas do Hive iriam exigir uma cópia completa para replicar. Consulte o [guia de otimização de atividade de cópia](../data-factory/v1/data-factory-copy-activity-performance.md) para obter mais informações sobre a cópia com o Data Factory. 

### <a name="adlcopy"></a>AdlCopy

AdlCopy é uma ferramenta de linha de comandos do Windows que permite copiar dados entre duas contas de Data Lake Store apenas na mesma região. A ferramenta de AdlCopy fornece uma opção autónoma ou a opção para utilizar uma conta do Azure Data Lake Analytics para executar a tarefa de cópia. Embora foi originalmente criada para cópias da pedido, por oposição a uma replicação robusta, proporciona outra opção para efetuar a cópia distribuída em contas de Data Lake Store na mesma região. Para fiabilidade, é recomendado que utilize a opção de Data Lake Analytics premium para qualquer carga de trabalho de produção. A versão autónoma pode devolver respostas ocupadas e limitou a monitorização e dimensionamento. 

Como o Distcp, o AdlCopy tem de ser orquestrada por algo como o automatização do Azure ou o Programador de tarefas do Windows. Tal como acontece com a fábrica de dados, AdlCopy não suporta a cópia apenas os ficheiros atualizados, mas voltará a copiar e substituir ficheiros existentes. Para obter mais informações e exemplos de como utilizar AdlCopy, consulte [copiar dados de Blobs de armazenamento do Azure para o Data Lake Store](data-lake-store-copy-data-azure-storage-blob.md).

## <a name="monitoring-considerations"></a>Considerações sobre a monitorização 

Data Lake Store fornece registos de diagnóstico detalhados e auditoria. Data Lake Store fornece algumas básicas métricas no portal do Azure com a conta do Data Lake Store e no Monitor do Azure. Disponibilidade do Data Lake Store é apresentada no portal do Azure. No entanto, esta métrica é atualizada a cada sete minutos e não pode ser consultada através de uma API publicamente exposta. Para obter a mais atualizadas à sua disponibilidade de uma conta de Data Lake Store, tem de executar os seus próprios testes sintéticos para validar a disponibilidade. Outras métricas, tais como a utilização de armazenamento total, a pedidos de leitura/escrita e a entrada/saída podem demorar até 24 horas para atualizar. Por isso, métricas atualizadas mais devem ser calculadas manualmente através de ferramentas de linha de comandos do Hadoop ou agregar informações de registo. A forma mais rápida para obter a utilização de armazenamento mais recente está a executar este comando HDFS de um nó de cluster do Hadoop (por exemplo, nó principal):   

    hdfs dfs -du -s -h adl://<adls_account_name>.azuredatalakestore.net:443/

### <a name="export-data-lake-store-diagnostics"></a>Diagnóstico de exportação de Data Lake Store 

Uma das formas mais rápidas e aceder aos registos pesquisáveis do Data Lake Store é permitir o envio de registos para **Log Analytics** sob o **diagnóstico** painel para a conta de Data Lake Store. Isto fornece acesso imediato para os registos de entrada com o tempo e os filtros de conteúdo, juntamente com as opções (e-mail/webhook) de alerta acionado dentro de intervalos de 15 minutos. Para obter instruções, consulte [aceder a registos de diagnóstico para o Azure Data Lake Store](data-lake-store-diagnostic-logs.md). 

Para mais de alertas em tempo real e mais controlo sobre onde encaminhado para os registos, considere a exportar os registos para EventHub do Azure onde conteúdo pode ser analisado individualmente ou através de uma janela de tempo para envio de notificações em tempo real a uma fila. Uma aplicação separada, como um [aplicação lógica](../connectors/connectors-create-api-azure-event-hubs.md) pode, em seguida, consumir e comunicar os alertas para o canal adequado, bem como submeter as métricas de ferramentas, como da NewRelic, Datadog ou AppDynamics de monitorização. Em alternativa, se estiver a utilizar uma ferramenta de terceiros, tais como ElasticSearch, pode exportar os registos para o Blob Storage e utilizar o [Plug-in do Azure Logstash](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob) para consumir dados para a pilha Elasticsearch, Kibana e Logstash (ELK).

### <a name="turn-on-debug-level-logging-in-hdinsight"></a>Ative o registo de nível de depuração no HDInsight 

Se Data Lake Store o envio de registo não está ativado, Azure HDInsight também fornece uma forma de ativar [do lado do cliente registo para o Data Lake Store](data-lake-store-performance-tuning-mapreduce.md) através de log4j. Tem de definir a propriedade seguinte **Ambari** > **YARN** > **configuração** > **avançadas yarn log4j configurações**: 

    log4j.logger.com.microsoft.azure.datalake.store=DEBUG 

Assim que a propriedade está definida e os nós são reiniciados, diagnóstico de Data Lake Store é escrito para os registos YARN em nós (/tmp/<user>/yarn.log) e detalhes importantes, como erros ou de limitação (código de erro HTTP 429) pode ser monitorizado. Esta mesma informação também pode ser monitorizada na análise de registos ou onde quer que os registos são fornecidos no [diagnóstico](data-lake-store-diagnostic-logs.md) painel da conta do Data Lake Store. Recomenda-se para, pelo menos, tem de ativar o registo do lado do cliente ou utilizar o opção de Data Lake Store para operacional visibilidade e mais fácil a depuração do envio do registo.

### <a name="run-synthetic-transactions"></a>Executar transações sintéticas 

A métrica de disponibilidade do serviço do Data Lake Store no portal do Azure tem atualmente, a janela de atualização de 7 minutos. Além disso, não pode ser consultado através de uma API publicamente exposta. Por conseguinte, é recomendado para criar uma aplicação básica que suporta transações sintéticas ao Data Lake Store pode fornecer até a disponibilidade de minuto. Um exemplo poderá criar um trabalho Web, aplicação lógica ou aplicação de função do Azure para efetuar uma leitura, criar, atualizar contra Data Lake Store e enviar os resultados para a sua solução de monitorização. As operações podem ser efetuadas numa pasta temporária e, em seguida, eliminadas após o teste, o que poderá ser executado a cada 30-60 segundos, dependendo dos requisitos.

## <a name="directory-layout-considerations"></a>Considerações de esquema de diretório

Quando o destino dados no data lake, é importante planear previamente a estrutura dos dados para que a segurança, criação de partições e o processamento podem ser utilizados de forma eficaz. Muitas das recomendações seguintes podem ser utilizadas se se trata de com o Azure Data Lake Store, o armazenamento de BLOBs ou o HDFS. Cada carga de trabalho tem requisitos diferentes em como os dados são consumidos, mas seguem-se alguns esquemas a considerar quando trabalhar com IoT comuns e cenários do batch.

### <a name="iot-structure"></a>Estrutura de IoT 

Em cargas de trabalho do IoT, pode existir uma grande quantidade de dados que está a ser landed no arquivo de dados que abrange inúmeras produtos, dispositivos, as organizações e clientes. É importante planear previamente o esquema de diretório para a organização, segurança e processamento eficiente dos dados para os consumidores de fluxo de baixo. Um modelo geral a considerar poderá ser o esquema do seguinte: 

    {Region}/{SubjectMatter(s)}/{yyyy}/{mm}/{dd}/{hh}/ 

Por exemplo, destino telemetria de um motor de avião dentro de RU aspeto que poderá ter a estrutura seguinte: 

    UK/Planes/BA1293/Engine1/2017/08/11/12/ 

Não há um motivo importante para colocar a data no fim da estrutura de pastas. Se pretender bloquear determinadas regiões ou é importante de assunto para os utilizadores/grupos, em seguida, pode facilmente fazê-com as permissões de POSIX. Caso contrário, se tiver ocorrido uma necessidade de restringi um determinado grupo de segurança ao visualizar apenas os dados de RU ou determinados planos, com a estrutura de data à frente uma permissão separada seria necessária para várias pastas na pasta cada hora. Além disso, ter a estrutura de data à frente seria aumentar exponencialmente o número de pastas como tempo correu.

### <a name="batch-jobs-structure"></a>Estrutura de tarefas do batch 

De um nível elevado, uma abordagem frequentemente utilizada no processamento de batches é encaminhado para dados numa pasta "em". Em seguida, depois dos dados são processados, colocar os novos dados para uma pasta "out" para a jusante processos a consumir. Esta estrutura de diretório é utilizada, por vezes, para tarefas que necessitam de processamento em ficheiros individuais e poderão não requerer processamento paralelo em massa através de grandes conjuntos de dados. Como a estrutura de IoT recomendada acima, uma estrutura de diretórios boa tem as pastas de nível de principal para coisas como região e o requerente é importante (por exemplo, organização, produto/produtor). Esta estrutura ajuda com a proteger os dados na sua organização e uma melhor gestão de dados nas cargas de trabalho. Além disso, considere a data e hora na estrutura para permitir uma melhor organização, de pesquisas filtradas, de segurança e de automatização no processamento. O nível de granularidade da estrutura de data é determinado pelo intervalo no qual os dados são carregados ou processados, tais como a hora a hora, diária ou até mesmo mensal. 

O ficheiro, por vezes, o processamento é concluído com êxito devido a danos nos dados ou formatos inesperados. Nestes casos, poderá beneficiar a estrutura de diretórios um **/incorreto** pasta para mover os ficheiros para aproveitarem inspeção. A tarefa de lote também poderá processe o Reporting Services ou notificação destes *incorreto* ficheiros de intervenção manual. Considere a seguinte estrutura do modelo: 

    {Region}/{SubjectMatter(s)}/In/{yyyy}/{mm}/{dd}/{hh}/ 
    {Region}/{SubjectMatter(s)}/Out/{yyyy}/{mm}/{dd}/{hh}/ 
    {Region}/{SubjectMatter(s)}/Bad/{yyyy}/{mm}/{dd}/{hh}/ 

Por exemplo, um firm marketing recebe extrai dados diários de atualizações de cliente dos respetivos clientes na América do Norte. Este aspeto que poderá ter o seguinte fragmento antes e depois a ser processado: 

    NA/Extracts/ACMEPaperCo/In/2017/08/14/updates_08142017.csv 
    NA/Extracts/ACMEPaperCo/Out/2017/08/14/processed_updates_08142017.csv 
 
No caso de comuns dos dados de batch que está a ser processados diretamente para bases de dados como o Hive ou tradicionais bases de dados do SQL Server, não é necessário um **/in** ou **/out** pasta, uma vez que o resultado já entra um Separe pasta para a tabela do Hive ou bases de dados externas. Por exemplo, extrai diárias de clientes seria encaminhado para os seus respetivas pastas e orquestração por algo semelhante do Azure Data Factory, o Apache Oozie, ou Apache Airflow seria acionar uma tarefa do Hive ou Spark diária para processar e escrever os dados para uma tabela do Hive.

## <a name="next-steps"></a>Passos Seguintes     

* [Descrição geral do Azure Data Lake Store](data-lake-store-overview.md) 
* [Controlo de acesso no Azure Data Lake Store](data-lake-store-access-control.md) 
* [Segurança no Azure Data Lake Store](data-lake-store-security-overview.md)
* [Otimização do Azure Data Lake Store para desempenho](data-lake-store-performance-tuning-guidance.md)
* [Orientações para a utilização do HDInsight Spark com o Azure Data Lake Store de otimização do desempenho](data-lake-store-performance-tuning-spark.md)
* [Orientações para utilizar o Hive do HDInsight com o Azure Data Lake Store de otimização do desempenho](data-lake-store-performance-tuning-hive.md)
* [Orquestração de Dados com o Azure Data Factory para o Azure Data Lake Store](https://mix.office.com/watch/1oa7le7t2u4ka)
* [Criar clusters do HDInsight com o Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md) 
