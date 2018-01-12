---
title: 'Depurar o Hadoop no HDInsight: ver registos e interpretar as mensagens de erro - Azure | Microsoft Docs'
description: "Saiba mais sobre as mensagens de erro que poderá receber ao administrar HDInsight com o PowerShell e passos que pode efetuar para recuperar."
services: hdinsight
tags: azure-portal
editor: cgronlun
manager: jhubbard
author: ashishthaps
documentationcenter: 
ms.assetid: 7e6ceb0e-8be8-4911-bc80-20714030a3ad
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: ashish
ms.openlocfilehash: 682b73aefff2ac20cbd38f6780b73cde859378ed
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="analyze-hadoop-logs"></a>Analisar os registos de Hadoop

Cada cluster de Hadoop no HDInsight do Azure tem uma conta de armazenamento do Azure utilizada como sistema de ficheiros predefinido. A conta de armazenamento é referida como a conta do Storage predefinida. Cluster utilizar o Table storage do Azure e o armazenamento de BLOBs na conta de armazenamento predefinida para armazenar os seus registos.  Para localizar a conta de armazenamento predefinido para o cluster, consulte [clusters do Hadoop gerir no HDInsight](../hdinsight-administer-use-management-portal.md#find-the-default-storage-account). Os registos de manter na conta de armazenamento, mesmo depois do cluster é eliminado.

## <a name="logs-written-to-azure-tables"></a>Registos de escrita de tabelas do Azure

Os registos de escrita de tabelas do Azure fornecem um nível de informações sobre o que acontece com um cluster do HDInsight.

Quando cria um cluster do HDInsight, seis tabelas são criadas automaticamente para clusters baseados em Linux no armazenamento de tabelas predefinidas:

* hdinsightagentlog
* syslog
* daemonlog
* hadoopservicelog
* ambariserverlog
* ambariagentlog

Os nomes de ficheiro de tabela são **u<ClusterName>DDMonYYYYatHHMMSSsss<TableName>**.

Estas tabelas conter os seguintes campos:

* ClusterDnsName
* ComponentName
* eventTimestamp
* Anfitrião
* MALoggingHash
* Mensagem
* N
* PreciseTimeStamp
* Função
* RowIndex
* Inquilino
* TIMESTAMP
* TraceLevel

### <a name="tools-for-accessing-the-logs"></a>Ferramentas para aceder os registos
Existem várias ferramentas disponíveis para aceder aos dados nestas tabelas:

* Visual Studio
* Explorador do Storage do Azure
* Power Query para Excel

#### <a name="use-power-query-for-excel"></a>Utilize o Power Query para Excel
Power Query pode ser instalado na [Microsoft Power Query para Excel](http://www.microsoft.com/en-us/download/details.aspx?id=39379). Consulte a página de transferência para os requisitos de sistema.

**Utilize o Power Query para abrir e analise o registo de serviço**

1. Abra **Microsoft Excel**.
2. Do **Power Query** menu, clique em **do Azure**e, em seguida, clique em **da Microsoft Table storage do Azure**.
   
    ![HDInsight Hadoop Excel PowerQuery abrir Table storage do Azure](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-using-excel-power-query-open.png)
3. Introduza o nome de conta de armazenamento, o nome abreviado ou o FQDN.
4. Introduza a chave de conta de armazenamento. Deverá ver uma lista de tabelas:
   
    ![Registos de Hadoop do HDInsight armazenados no Table storage do Azure](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-table-names.png)
5. Clique com o botão direito a tabela hadoopservicelog o **navegador** painel e selecione **editar**. Deverá ver quatro colunas. Opcionalmente, elimine o **chave de partição**, **chave de linha**, e **Timestamp** colunas por selecioná-los, em seguida, clicar em **remover colunas** do as opções no Friso.
6. Clique no ícone de expansão na coluna conteúdo para escolher as colunas que pretende importar para a folha de cálculo do Excel. Para esta demonstração, posso escolheu TraceLevel e ComponentName: pode fornecer-me algumas informações básicas no qual os componentes tinham problemas.
   
    ![Registos de Hadoop do HDInsight escolher colunas](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-using-excel-power-query-filter.png)
7. Clique em **OK** para importar os dados.
8. Selecione o **TraceLevel**, função, e **ComponentName** colunas e, em seguida, clique em **Group By** controlo no Friso.
9. Clique em **OK** na caixa de diálogo Group By
10. Clique em * * Aplicar & Fechar * *.

Agora, pode utilizar o Excel para filtrar e ordenar conforme necessário. Poderá querer incluir outras colunas (por exemplo, a mensagem) para desagregar problemas quando ocorrem, mas a seleção e agrupar as colunas descritas acima fornecem uma imagem decent da que está a acontecer com serviços do Hadoop. A mesma ideia pode ser aplicada para as tabelas setuplog e hadoopinstalllog.

#### <a name="use-visual-studio"></a>Utilizar o Visual Studio
**Utilizar o Visual Studio**

1. Abra o Visual Studio.
2. Do **vista** menu, clique em **Cloud Explorer**. Ou clique simplesmente **CTRL +\, CTRL + X**.
3. De **Cloud Explorer**, selecione **tipos de recursos**.  A outra opção disponível é **grupos de recursos**.
4. Expanda **contas do Storage**, a conta de armazenamento predefinido para o cluster e, em seguida, **tabelas**.
5. Faça duplo clique em **hadoopservicelog**.
6. Adicione um filtro. Por exemplo:
   
        TraceLevel eq 'ERROR'
   
    ![Registos de Hadoop do HDInsight escolher colunas](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-visual-studio-filter.png)
   
    Para obter mais informações sobre a construção de filtros, consulte [construir cadeias de filtro para o estruturador de tabela](../../vs-azure-tools-table-designer-construct-filter-strings.md).

## <a name="logs-written-to-azure-blob-storage"></a>Registos de escrita para o armazenamento de Blobs do Azure
[Os registos de escrita de tabelas do Azure](#log-written-to-azure-tables) fornecer um nível de informações sobre o que acontece com um cluster do HDInsight. No entanto, estas tabelas não fornecem ao nível da tarefa de registos, que poderá ser útil desagregação adicional para problemas quando ocorrem. Para fornecer esta próximo nível de detalhe, clusters do HDInsight são configurados para escrever os registos de tarefas à sua conta de armazenamento de BLOBs para qualquer tarefa que é submetida através de Templeton. Praticamente, isto significa que as tarefas submetidas utilizando cmdlets do Microsoft Azure PowerShell ou APIs de submissão de tarefa .NET, não as tarefas submetidas através do acesso RDP/comandos-linha para o cluster. 

Para ver os registos, consulte o artigo [nos registos de aplicação YARN de acesso no HDInsight baseado em Linux](../hdinsight-hadoop-access-yarn-app-logs-linux.md).

Para obter mais informações sobre os registos de aplicações, consulte [simplificar a gestão de registos de utilizador e de acesso no YARN](http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/).

## <a name="view-cluster-health-and-job-logs"></a>Ver registos de estado de funcionamento e a tarefa de cluster
### <a name="access-the-ambari-ui"></a>Aceder à IU do Ambari
No portal do Azure, clique num nome de cluster do HDInsight para abrir o painel do cluster. No painel de cluster, clique em **Dashboard**.

![Iniciar o dashboard de cluster](./media/apache-hadoop-debug-jobs/hdi-debug-launch-dashboard.png)


### <a name="access-the-yarn-ui"></a>Acesso o IU do Yarn
No portal do Azure, clique num nome de cluster do HDInsight para abrir o painel do cluster. No painel de cluster, clique em **Dashboard**. Quando lhe for pedido, introduza as credenciais de administrador do cluster. No Ambari, selecione **YARN** da lista de serviços no lado esquerdo. Na página que aparece, selecione **ligações rápidas**, em seguida, selecione a entrada de nó principal Active Directory e a IU do Resource Manager.

Pode utilizar a IU do YARN para fazer o seguinte:

* **Obter o estado do cluster**. No painel esquerdo, expanda **Cluster**e clique em **sobre**. Este presente cluster detalhes de estado, como total atribuída memória, núcleos utilizados, o estado do Gestor de recursos do cluster, versão do cluster e assim sucessivamente.
  
    ![Iniciar o dashboard de cluster](./media/apache-hadoop-debug-jobs/hdi-debug-yarn-cluster-state.png)
* **Obter o estado do nó**. No painel esquerdo, expanda **Cluster**e clique em **nós**. Isto apresenta uma lista de todos os nós do cluster, o endereço HTTP de cada nó, recursos atribuídos a cada nó, etc.
* **Monitorizar o estado da tarefa**. No painel esquerdo, expanda **Cluster**e, em seguida, clique em **aplicações** para listar todas as tarefas do cluster. Se pretender ver tarefas num estado específico (por exemplo, executar de novo, submetido, etc.), clique na hiperligação adequada em **aplicações**. Ainda pode clicar no nome da tarefa para obter mais informações sobre a tarefa essas, incluindo o resultado, os registos, etc.

### <a name="access-the-hbase-ui"></a>Aceder à IU do HBase
No portal do Azure, clique num nome de cluster HBase do HDInsight para abrir o painel do cluster. No painel de cluster, clique em **Dashboard**. Quando lhe for pedido, introduza as credenciais de administrador do cluster. No Ambari, selecione o HBase da lista de serviços. Selecione **ligações rápidas** no topo da página, apontar para a ligação de nós de Zookeeper Active Directory e, em seguida, clique em mestre HBase IU.

## <a name="hdinsight-error-codes"></a>Códigos de erro do HDInsight
As mensagens de erro descritas nesta secção são fornecidas para ajudar os utilizadores do Hadoop no Azure HDInsight compreender condições de erro possíveis podem ocorrer ao administrar o serviço com o Azure PowerShell e aconselhe-los processo os passos que podem ser executadas Para recuperar do erro.

Algumas destas mensagens de erro foi também são apresentadas no portal do Azure quando é utilizado para gerir clusters do HDInsight. Mas as outras mensagens de erro poderá encontrar existem menos granular devido a restrições nas toma ações possíveis neste contexto. Outras mensagens de erro são fornecidas nos contextos de onde está óbvias a mitigação. 

### <a id="AtleastOneSqlMetastoreMustBeProvided"></a>AtleastOneSqlMetastoreMustBeProvided
* **Descrição**: forneça os detalhes da base de dados do SQL do Azure para, pelo menos, um componente para poder utilizar as definições personalizadas para metastores do Hive e Oozie.
* **Mitigação**: O utilizador tem de fornecer um metastore do SQL Azure válido e repita o pedido.  

### <a id="AzureRegionNotSupported"></a>AzureRegionNotSupported
* **Descrição**: não foi possível criar o cluster na região *nameOfYourRegion*. Utilizam uma região HDInsight válida e repita o pedido.
* **Mitigação**: cliente deve criar a região de cluster suporta atualmente: Sudeste asiático, Europa Ocidental, Norte da Europa, EUA Leste ou EUA oeste.  

### <a id="ClusterContainerRecordNotFound"></a>ClusterContainerRecordNotFound
* **Descrição**: O servidor não foi possível localizar o registo de pedido do cluster.  
* **Mitigação**: Repita a operação.

### <a id="ClusterDnsNameInvalidReservedWord"></a>ClusterDnsNameInvalidReservedWord
* **Descrição**: nome de DNS de Cluster *yourDnsName* é inválido. Certifique-se nome inicia e termina com alfanumérico e só pode conter '-' caráter especial  
* **Mitigação**: Certifique-se de que utiliza um nome DNS válido para o cluster que inicia e termina com alfanuméricos e contém não especial carateres exceto o travessão '-' e, em seguida, repita a operação.

### <a id="ClusterNameUnavailable"></a>ClusterNameUnavailable
* **Descrição**: nome do Cluster *yourClusterName* não está disponível. . Escolha outro nome.  
* **Mitigação**: O utilizador deve especificar um clustername que seja exclusivo e não existe e repita. Se o utilizador estiver a utilizar o Portal, a IU irá notificá-los de se já está a ser utilizado um nome de cluster durante os passos de criação.

### <a id="ClusterPasswordInvalid"></a>ClusterPasswordInvalid
* **Descrição**: palavra-passe do Cluster é inválido. Palavra-passe tem de ser no mínimo 10 carateres e tem de conter, pelo menos, um número, letra maiúscula, letra minúscula e um caráter especial sem espaços e não deve conter o nome de utilizador como parte do mesmo.  
* **Mitigação**: forneça uma palavra-passe de cluster válido e repita a operação.

### <a id="ClusterUserNameInvalid"></a>ClusterUserNameInvalid
* **Descrição**: nome de utilizador do Cluster é inválido. Certifique-se de que nome de utilizador não contém carateres especiais ou espaços.  
* **Mitigação**: forneça um nome de utilizador de cluster válido e repita a operação.

### <a id="ClusterUserNameInvalidReservedWord"></a>ClusterUserNameInvalidReservedWord
* **Descrição**: nome de DNS de Cluster *yourDnsClusterName* é inválido. Certifique-se nome inicia e termina com alfanumérico e só pode conter '-' caráter especial  
* **Mitigação**: forneça um nome de utilizador de cluster DNS válido e repita a operação.

### <a id="ContainerNameMisMatchWithDnsName"></a>ContainerNameMisMatchWithDnsName
* **Descrição**: nome do contentor no URI *yourcontainerURI* e o nome DNS *yourDnsName* num pedido de corpo têm de ser iguais.  
* **Mitigação**: Certifique-se de que o contentor do nome e o nome DNS são os mesmos e repita a operação.

### <a id="DataNodeDefinitionNotFound"></a>DataNodeDefinitionNotFound
* **Descrição**: configuração de cluster é inválido. Não é possível localizar quaisquer definições de nó de dados de tamanho de nó.  
* **Mitigação**: Repita a operação.

### <a id="DeploymentDeletionFailure"></a>DeploymentDeletionFailure
* **Descrição**: Ocorreu uma falha na eliminação da implementação para o Cluster  
* **Mitigação**: Repita a operação de eliminação.

### <a id="DnsMappingNotFound"></a>DnsMappingNotFound
* **Descrição**: erro de configuração do serviço. Informações de mapeamento de DNS necessárias não foi encontradas.  
* **Mitigação**: eliminar o cluster e criar um novo cluster.

### <a id="DuplicateClusterContainerRequest"></a>DuplicateClusterContainerRequest
* **Descrição**: duplicado tentativa de criação do contentor de cluster. Existe registo para *nameOfYourContainer* mas Etags não correspondem.
* **Mitigação**: forneça um nome exclusivo para o contentor e repita a operação de criação.

### <a id="DuplicateClusterInHostedService"></a>DuplicateClusterInHostedService
* **Descrição**: serviço alojado *nameOfYourHostedService* já contém um cluster. Um serviço alojado não pode conter vários clusters  
* **Mitigação**: alojar o cluster no outro serviço alojado.

### <a id="FailureToUpdateDeploymentStatus"></a>FailureToUpdateDeploymentStatus
* **Descrição**: O servidor não foi possível atualizar o estado da implementação do cluster.  
* **Mitigação**: Repita a operação. Se isto acontecer várias vezes, contacte o CSS.

### <a id="HdiRestoreClusterAltered"></a>HdiRestoreClusterAltered
* **Descrição**: Cluster *yourClusterName* foi eliminado como parte da manutenção. Volte a criar o cluster.
* **Mitigação**: recrie o cluster.

### <a id="HeadNodeConfigNotFound"></a>HeadNodeConfigNotFound
* **Descrição**: configuração de cluster é inválido. Necessário não foi encontrada em tamanhos de nó de configuração do nó principal.
* **Mitigação**: Repita a operação.

### <a id="HostedServiceCreationFailure"></a>HostedServiceCreationFailure
* **Descrição**: não é possível criar o serviço alojado *nameOfYourHostedService*. Repita o pedido.  
* **Mitigação**: repetir o pedido.

### <a id="HostedServiceHasProductionDeployment"></a>HostedServiceHasProductionDeployment
* **Descrição**: serviço alojado *nameOfYourHostedService* já tem uma implementação de produção. Um serviço alojado não pode conter várias implementações de produção. Repita o pedido com um nome de outro cluster.
* **Mitigação**: Utilize outro nome de cluster e repita o pedido.

### <a id="HostedServiceNotFound"></a>HostedServiceNotFound
* **Descrição**: serviço alojado *nameOfYourHostedService* para não foi possível encontrar o cluster.  
* **Mitigação**: se o cluster está em estado de erro, elimine-o e, em seguida, tente novamente.

### <a id="HostedServiceWithNoDeployment"></a>HostedServiceWithNoDeployment
* **Descrição**: serviço alojado *nameOfYourHostedService* não tem nenhuma implementação associada.  
* **Mitigação**: se o cluster está em estado de erro, elimine-o e, em seguida, tente novamente.

### <a id="InsufficientResourcesCores"></a>InsufficientResourcesCores
* **Descrição**: O SubscriptionId *yourSubscriptionId* não tem núcleos à esquerda para criar cluster *yourClusterName*. Obrigatório: *resourcesRequired*, disponíveis: *resourcesAvailable*.  
* **Mitigação**: libertar recursos na sua subscrição ou aumentar os recursos disponíveis para a subscrição e tente criar o cluster novamente.

### <a id="InsufficientResourcesHostedServices"></a>InsufficientResourcesHostedServices
* **Descrição**: ID de subscrição *yourSubscriptionId* não tem quota para um novo HostedService Criar cluster *yourClusterName*.  
* **Mitigação**: libertar recursos na sua subscrição ou aumentar os recursos disponíveis para a subscrição e tente criar o cluster novamente.

### <a id="InternalErrorRetryRequest"></a>InternalErrorRetryRequest
* **Descrição**: O servidor encontrou um erro interno. Repita o pedido.  
* **Mitigação**: repetir o pedido.

### <a id="InvalidAzureStorageLocation"></a>InvalidAzureStorageLocation
* **Descrição**: localização de armazenamento do Azure *dataRegionName* não é uma localização válida. Certifique-se que a região está correta e repita o pedido.
* **Mitigação**: selecione uma localização de armazenamento que suporta o HDInsight, verifique se o cluster se encontra localizado conjuntamente e repita a operação.

### <a id="InvalidNodeSizeForDataNode"></a>InvalidNodeSizeForDataNode
* **Descrição**: tamanho da VM inválido para nós de dados. Tamanho de 'VM grande' só é suportado para todos os nós de dados.  
* **Mitigação**: Especifique o tamanho de nó suportados para o nó de dados e repita a operação.

### <a id="InvalidNodeSizeForHeadNode"></a>InvalidNodeSizeForHeadNode
* **Descrição**: tamanho da VM inválido para o nó principal. Tamanho de 'ExtraLarge VM' só é suportado para o nó principal.  
* **Mitigação**: Especifique o tamanho de nó suportados para o nó principal e repita a operação

### <a id="InvalidRightsForDeploymentDeletion"></a>InvalidRightsForDeploymentDeletion
* **Descrição**: ID de subscrição *yourSubscriptionId* a ser utilizada não possui permissões suficientes para executar a operação de eliminação para o cluster *yourClusterName*.  
* **Mitigação**: se o cluster está em estado de erro, removê-la e, em seguida, tente novamente.  

### <a id="InvalidStorageAccountBlobContainerName"></a>InvalidStorageAccountBlobContainerName
* **Descrição**: nome de contentor do blob de conta do storage externo *yourContainerName* é inválido. Certifique-se do nome começa com uma letra e contém apenas letras minúsculas, números e travessão.  
* **Mitigação**: Especifique um nome de contentor de blob de conta de armazenamento válida e repita a operação.

### <a id="InvalidStorageAccountConfigurationSecretKey"></a>InvalidStorageAccountConfigurationSecretKey
* **Descrição**: configuração para a conta do storage externo *yourStorageAccountName* é necessário ter os detalhes da chave secretos para ser definido.  
* **Mitigação**: Especifique uma chave secreta válida para a conta de armazenamento e repita a operação.

### <a id="InvalidVersionHeaderFormat"></a>InvalidVersionHeaderFormat
* **Descrição**: cabeçalho de versão *yourVersionHeader* não está num formato válido de aaaa-mm-dd.  
* **Mitigação**: Especifique um formato válido para o cabeçalho de versão e repita o pedido.

### <a id="MoreThanOneHeadNode"></a>MoreThanOneHeadNode
* **Descrição**: configuração de cluster é inválido. Foi encontrado mais de uma configuração de nó principal.  
* **Mitigação**: Editar a configuração, para que apenas um nó principal especificado.

### <a id="OperationTimedOutRetryRequest"></a>OperationTimedOutRetryRequest
* **Descrição**: não foi possível concluir a operação no tempo permitido ou o máximo de tentativas tenta possíveis. Repita o pedido.  
* **Mitigação**: repetir o pedido.

### <a id="ParameterNullOrEmpty"></a>ParameterNullOrEmpty
* **Descrição**: parâmetro *yourParameterName* não pode ser nulo nem estar vazio.  
* **Mitigação**: Especifique um valor válido para o parâmetro.

### <a id="PreClusterCreationValidationFailure"></a>PreClusterCreationValidationFailure
* **Descrição**: um ou mais entradas de pedido de criação de cluster não são válido. Certifique-se os valores de entrada estão corretos e repita o pedido.  
* **Mitigação**: Certifique-se os valores de entrada estão corretos e repita o pedido.

### <a id="RegionCapabilityNotAvailable"></a>RegionCapabilityNotAvailable
* **Descrição**: capacidade de região não está disponível para a região *yourRegionName* e ID de subscrição *yourSubscriptionId*.  
* **Mitigação**: Especifique uma região que suporta clusters do HDInsight. Regiões suportadas publicamente são: Sudeste asiático, Europa Ocidental, Norte da Europa, EUA Leste ou EUA oeste.

### <a id="StorageAccountNotColocated"></a>StorageAccountNotColocated
* **Descrição**: conta de armazenamento *yourStorageAccountName* está numa região *currentRegionName*. Deve ser igual a região de cluster *yourClusterRegionName*.  
* **Mitigação**: Especifique uma conta do storage na mesma região que o cluster está a ser ou se os dados já estão a ser a conta de armazenamento, crie um novo cluster na mesma região que a conta de armazenamento existente. Se estiver a utilizar o Portal, a IU irá notificá-los deste problema antecipadamente.

### <a id="SubscriptionIdNotActive"></a>SubscriptionIdNotActive
* **Descrição**: ID de subscrição fornecido *yourSubscriptionId* não está ativa.  
* **Mitigação**: reativar a sua subscrição ou obtenha uma nova subscrição válida.

### <a id="SubscriptionIdNotFound"></a>SubscriptionIdNotFound
* **Descrição**: ID de subscrição *yourSubscriptionId* não foi possível encontrar.  
* **Mitigação**: verificar se o seu ID de subscrição é válido e repita a operação.

### <a id="UnableToResolveDNS"></a>UnableToResolveDNS
* **Descrição**: não é possível resolver DNS *yourDnsUrl*. Certifique-se de que o URL completamente qualificado para o ponto final do blob é fornecido.  
* **Mitigação**: forneça um URL de blob válido. O URL tem de ser totalmente válidos, incluindo começadas *http://* e termina numa *empresa>.com*.

### <a id="UnableToVerifyLocationOfResource"></a>UnableToVerifyLocationOfResource
* **Descrição**: não é possível verificar a localização do recurso *yourDnsUrl*. Certifique-se de que o URL completamente qualificado para o ponto final do blob é fornecido.  
* **Mitigação**: forneça um URL de blob válido. O URL tem de ser totalmente válidos, incluindo começadas *http://* e termina numa *empresa>.com*.

### <a id="VersionCapabilityNotAvailable"></a>VersionCapabilityNotAvailable
* **Descrição**: capacidade de versão não está disponível para a versão *specifiedVersion* e ID de subscrição *yourSubscriptionId*.  
* **Mitigação**: escolher uma versão que está disponível e repita a operação.

### <a id="VersionNotSupported"></a>VersionNotSupported
* **Descrição**: versão *specifiedVersion* não suportado.
* **Mitigação**: escolher uma versão suportada e repita a operação.

### <a id="VersionNotSupportedInRegion"></a>VersionNotSupportedInRegion
* **Descrição**: versão *specifiedVersion* não está disponível na região do Azure *specifiedRegion*.  
* **Mitigação**: escolher uma versão que é suportada na região especificada e repita a operação.

### <a id="WasbAccountConfigNotFound"></a>WasbAccountConfigNotFound
* **Descrição**: configuração de cluster é inválido. Necessária WASB configuração da conta não encontrada em contas externas.  
* **Mitigação**: Certifique-se de que a conta existe e está corretamente especificado na configuração e repita a operação.

## <a name="next-steps"></a>Passos Seguintes

* [Utilizar vistas do Ambari para depurar tarefas Tez no HDInsight](../hdinsight-debug-ambari-tez-view.md)
* [Ativar capturas de área dinâmica para dados de serviços do Hadoop no HDInsight baseado em Linux](../hdinsight-hadoop-collect-debug-heap-dump-linux.md)

<!--
TODO  * [Manage HDInsight clusters by using the Ambari Web UI](hdinsight-hadoop-manage-ambari.md)
-->
