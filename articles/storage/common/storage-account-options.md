---
title: Opções de contas de Armazenamento do Azure | Microsoft Docs
description: Compreenda as opções para utilizar o Armazenamento do Azure.
services: storage
author: jirwin
manager: jwillis
ms.service: storage
ms.workload: storage
ms.topic: get-started-article
ms.date: 01/17/2018
ms.author: jirwin
ms.openlocfilehash: 75d1580df5e36b2c88939fde9077c5a1948f6348
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="azure-storage-account-options"></a>Opções de contas do Armazenamento do Azure

## <a name="overview"></a>Descrição geral
O Armazenamento do Azure oferece três opções de contas diferentes, com preços e funcionalidades distintas suportadas. Considere estas diferenças antes de criar uma conta de armazenamento para determinar a opção mais adequada às suas aplicações. As três opções de conta de armazenamento distintas são:

* Contas para **Fins gerais v2 (GPv2)** 
* Contas para **Fins gerais v1 (GPv1)**
* Contas de **armazenamento de blobs**

Cada tipo de conta é descrito mais detalhadamente na secção seguinte:

## <a name="storage-account-options"></a>Opções da conta de armazenamento

### <a name="general-purpose-v2"></a>Fins gerais v2

As contas para Fins gerais v2 (GPv2) são contas de armazenamento que suportam todas as funcionalidades mais recentes para blobs, ficheiros, filas e tabelas. As contas GPv2 suportam todas as APIs e funcionalidades suportadas nas contas GPv1 e nas contas de armazenamento de blobs. Também suportam as mesmas funcionalidades de durabilidade, disponibilidade, escalabilidade e desempenho nesses tipos de conta. Os preços das contas GPv2 foram estabelecidos para oferecerem os preços mais baixos por gigabyte e os preços mais competitivos da indústria para transações.

Pode atualizar a conta GPv1 para uma conta GPv2 com o PowerShell ou a CLI do Azure. 

Para blobs de blocos numa conta de armazenamento GPv2, pode optar entre as camadas de armazenamento frequente e esporádico ao nível da conta ou as camadas frequente, esporádica e de arquivo ao nível do blob com base nos padrões de acesso. Armazene dados acedidos com muita frequência, com pouca frequência e raramente nas camadas de armazenamento frequente, esporádico e de arquivo, respetivamente, para otimizar os custos. 

As contas de armazenamento GPv2 expõem o atributo de **Camada de acesso** ao nível da conta, o qual especifica a camada da conta de armazenamento predefinida como **Frequente** ou **Esporádica**. A camada da conta de armazenamento predefinida é aplicada a qualquer blob que não tenha uma camada explícita definida ao nível do blob. Se o padrão de utilização dos dados sofrer uma alteração, pode também alternar entre estas camadas de armazenamento em qualquer altura. A **camada de arquivo** só pode ser aplicada ao nível do blob.

> [!NOTE]
> A alteração da camada de armazenamento poderá resultar em encargos adicionais. Para obter mais informações, veja a secção [Preços e faturação](#pricing-and-billing).
>
> A Microsoft recomenda a utilização de contas de armazenamento para fins gerais v2 em detrimento de contas de armazenamento de blobs para a maioria dos cenários.

### <a name="upgrade-a-storage-account-to-gpv2"></a>Atualizar uma conta de armazenamento para GPv2

Os utilizadores podem atualizar uma conta GPv1 para uma conta GPv2 em qualquer altura com o PowerShell ou a CLI do Azure. Esta alteração não pode ser revertida e não são permitidas quaisquer outras alterações.

#### <a name="upgrade-with-powershell"></a>Atualizar com o PowerShell

Para atualizar uma conta GPv1 para uma conta GPv2 com o PowerShell, primeiro atualize o PowerShell para utilizar a versão mais recente do módulo **AzureRm.Storage**. Veja [Instalar e configurar o Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) para obter informações sobre a instalação do PowerShell. Em seguida, chame o comando seguinte para atualizar a conta, substituindo o nome do grupo de recursos e da conta de armazenamento:

```powershell
Set-AzureRmStorageAccount -ResourceGroupName <resource-group> -AccountName <storage-account> -UpgradeToStorageV2
```

#### <a name="upgrade-with-azure-cli"></a>Atualizar com a CLI do Azure

Para atualizar uma conta GPv1 para uma conta GPv2 com a CLI do Azure, instale primeiro a versão mais recente da CLI do Azure. Veja [Install the Azure CLI 2.0 ](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) (Instalar a CLI 2.0 do Azure) para obter informações sobre a instalação da CLI. Em seguida, chame o comando seguinte para atualizar a conta, substituindo o nome do grupo de recursos e da conta de armazenamento:

```cli
az storage account update -g <resource-group> -n <storage-account> --set kind=StorageV2
```` 

### <a name="general-purpose-v1"></a>Fins gerais v1

As contas para Fins gerais v1 (GPv1) proporcionam acesso a todos os serviços do Armazenamento do Azure, mas podem não ter as funcionalidades mais recentes ou os preços mais baixos por gigabyte. Por exemplo, o armazenamento esporádico e o armazenamento de arquivo não são suportados nas contas GPv1. O preço é mais baixo para as transações GPv1, pelo que as cargas de trabalho com elevado número de alterações ou velocidades de leitura altas podem tirar partido deste tipo de conta.

As contas de armazenamento para Fins gerais v1 (GPv1) são o tipo mais antigo de contas de armazenamento e as únicas que podem ser utilizadas com o modelo de implementação clássica. 

### <a name="blob-storage-accounts"></a>Contas do Blob Storage

As contas de armazenamento de blobs suportam as mesmas funcionalidades de blob de blocos que as contas GPv2, mas têm a limitação de suportar apenas blobs de blocos. Os preços são essencialmente semelhantes aos preços das contas para fins gerais v2. Os clientes devem analisar as diferenças nos preços entre as contas de Armazenamento de blobs e GPv2 e considerar a atualização para GPv2. Esta atualização não pode ser anulada.

A capacidade de atualizar as Contas de armazenamento de blobs para GPv2 estará disponível em breve.

> [!NOTE]
> As contas do Blob Storage suportam apenas blobs de blocos e de acréscimo e não suportam blobs de páginas.

## <a name="recommendations"></a>Recomendações

Para obter mais informações sobre contas de armazenamento, veja [Acerca das contas de armazenamento do Azure](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

Para as aplicações que requerem apenas o armazenamento de blobs de blocos ou de acréscimo, recomendamos que utilize contas de armazenamento GPv2, de modo a tirar partido do modelo de preços diferenciado do armazenamento em camadas. No entanto, pode querer utilizar as contas GPv1 em determinados cenários, tais como:

* Ainda tem de utilizar o modelo de implementação clássica. As contas do Blob Storage estão disponíveis apenas através do modelo de implementação Azure Resource Manager.

* Se utilizar elevados volumes de transações ou de largura de rede de georreplicação, os quais são ambos mais caros nas contas GPv2 e nas contas de Armazenamento de blobs do que nas contas GPv1, e não tem armazenamento suficiente que lhe permita tirar partido dos custos mais baixos de armazenamento por GB.

* Utiliza uma versão da [API REST dos Serviços do Storage](https://msdn.microsoft.com/library/azure/dd894041.aspx) anterior a 14/02/2014 ou uma biblioteca de cliente com uma versão inferior à 4.x e não pode atualizar a sua aplicação.

> [!NOTE]
> Atualmente, as contas do armazenamento de Blobs são suportadas em todas as regiões do Azure.

## <a name="pricing-and-billing"></a>Preços e faturação
Todas as contas de armazenamento utilizam um modelo de preços para o armazenamento de blobs com base na camada de cada blob. Ao utilizar uma conta de armazenamento, aplicam-se as seguintes considerações de faturação:

* **Custos de armazenamento**: para além da quantidade de dados armazenados, o custo do armazenamento de dados varia consoante a camada de armazenamento. O custo por gigabyte diminui conforme a camada se torna mais esporádica.

* **Custos de acesso a dados**: os custos de acesso a dados aumenta conforme a camada se torna mais esporádica. Para os dados na camada frequente e de armazenamento de arquivo, é cobrada uma taxa de acesso a dados por gigabyte pelas operações de leitura.

* **Custos de transação**: há um encargo por transação para todas as camadas que aumenta cada vez que a camada é mais esporádica.

* **Custos de transferência de dados de georreplicação**: este custo aplica-se apenas às contas que têm a georreplicação configurada, incluindo GRS e RA-GRS. A transferência de dados de georreplicação está sujeita a uma taxa por gigabyte.

* **Custos de transferência de dados de saída**: as transferências de dados de saída (dados que são transferidos para fora de uma região do Azure) estão sujeitas a uma cobrança pela utilização de largura de banda por gigabyte, tal como as contas do Storage para fins gerais.

* **Alteração da camada de armazenamento**: a alteração da camada de armazenamento de conta esporádica para armazenamento frequente está sujeita à cobrança de uma taxa igual à leitura de todos os dados existentes na conta do armazenamento. No entanto, a alteração da camada de armazenamento de conta de frequente para esporádica incorre um encargo igual à escrita de todos os dados na camada esporádica (contas GPv2 apenas).

> [!NOTE]
> Para obter mais informações sobre o modelo de preços das contas do Armazenamento de blobs, veja a página [Preços do Armazenamento do Azure](https://azure.microsoft.com/pricing/details/storage/). Para obter mais informações sobre as taxas aplicáveis às transferências de dados de saída, veja a página [Detalhes de Preços das Transferências de Dados](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="quickstart-scenarios"></a>Cenários de início rápido

Nesta secção, são demonstrados os seguintes cenários através do portal do Azure:

* Como criar uma conta de armazenamento GPv2.
* Como converter uma conta de Armazenamento de blobs ou GPv1 numa conta de armazenamento GPv2.
* Como definir a camada da conta e dos blobs em contas de armazenamento GPv2.

Nos exemplos seguintes, não pode definir a camada de acesso como arquivo porque esta definição aplica-se a toda a conta de armazenamento. O armazenamento de arquivo só pode ser definido em blobs específicos.

### <a name="create-a-gpv2-storage-account-using-the-azure-portal"></a>Criar uma conta de armazenamento GPv2 através do portal do Azure

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

2. No menu Hub, selecione **Criar um recurso** > **Dados + Armazenamento** > **Conta de armazenamento**.

3. Introduza um nome para a conta do Storage.

    Este nome tem de ser globalmente exclusivo; é utilizado como parte do URL utilizado para aceder aos objetos na conta de armazenamento.  

4. Selecione **Resource Manager** como o modelo de implementação.

    O armazenamento em camadas só pode ser utilizado com contas de armazenamento do Resource Manager; o Resource Manager é o modelo de implementação recomendado para novos recursos. Para obter mais informações, veja a [Descrição geral do Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).  

5. Na lista pendente **Tipo de conta**, selecione **Fins gerais v2**.

    Quando seleciona GPv2, a camada de desempenho é definida como Standard. O armazenamento em camadas não está disponível com a camada de desempenho Premium.

6. Selecione a opção de replicação para a conta de armazenamento: **LRS**, **ZRS**, **GRS** ou **RA-GRS**. A predefinição é **RA-GRS**.

    LRS = armazenamento localmente redundante; ZRS = armazenamento com redundância de zona; GRS = armazenamento georredundante (duas regiões); RA-GRS = armazenamento georredundante com acesso de leitura (duas regiões com acesso de leitura para a segunda).

    Consulte [Replicação do Storage do Azure](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) para obter detalhes adicionais sobre as opções de replicação do Storage do Azure.

7. Selecione a camada de armazenamento correta para as suas necessidades: defina a **Camada de acesso** como **Esporádica** ou **Frequente**. A predefinição é **Frequente**.

8. Selecione a subscrição na qual pretende criar a nova conta do Storage.

9. Especifique um novo grupo de recursos ou selecione um grupo de recursos existente. Para obter mais informações sobre os grupos de recursos, veja [Descrição geral do Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

10. Selecione a região para a sua conta de armazenamento.

11. Clique em **Criar** para criar a conta do Storage.

### <a name="convert-a-gpv1-account-to-a-gpv2-storage-account-using-the-azure-portal"></a>Converter uma conta GPv1 numa conta de armazenamento GPv2 com o portal do Azure

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

2. Para navegar para a conta de armazenamento, selecione **Todos os recursos** e, em seguida, selecione a conta de armazenamento.

3. Na secção Definições, clique em **Configuração**.

4. Em **Tipo de conta**, clique em **Atualizar**.

5. Em **Confirmar atualização**, introduza o nome da sua conta. 

5. Clique em Atualizar, na parte inferior do painel.

### <a name="change-the-storage-tier-of-a-gpv2-storage-account-using-the-azure-portal"></a>Alterar a camada de armazenamento de uma conta de armazenamento GPv2 com o portal do Azure

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

2. Para navegar para a conta de armazenamento, selecione **Todos os recursos** e, em seguida, selecione a conta de armazenamento.

3. No painel Definições, clique em**Configuração** para ver e/ou alterar a configuração da conta.

4. Selecione a camada de armazenamento correta para as suas necessidades: defina a **Camada de acesso** como **Esporádica** ou **Frequente**.

5. Clique em Guardar na parte superior do painel.

### <a name="change-the-storage-tier-of-a-blob-using-the-azure-portal"></a>Alterar a camada de armazenamento de um blob através do portal do Azure

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

2. Para navegar para o blob na conta de armazenamento, selecione **Todos os recursos**, selecione a conta de armazenamento, selecione o contentor e, em seguida, selecione o blob.

3. No painel de propriedades do Blob, clique no menu pendente **Camada de Acesso** para selecionar a camada de armazenamento **Frequente**, **Esporádica** ou **Arquivo**.

5. Clique em Guardar na parte superior do painel.

> [!NOTE]
> A alteração da camada de armazenamento poderá resultar em encargos adicionais. Para obter mais informações, veja a secção [Preços e Faturação](#pricing-and-billing).


## <a name="evaluating-and-migrating-to-gpv2-storage-accounts"></a>Avaliar e migrar para contas de armazenamento GPv2
O objetivo desta secção consiste em ajudar os utilizadores a fazerem uma transição tranquila para a utilização de contas de armazenamento GPv2 (por oposição a GPv1). Há dois cenários de utilizador:

* Tem uma conta de armazenamento GPv1 já existente e quer avaliar uma alteração para uma conta de armazenamento GPv2 com a camada de armazenamento mais adequada.
* Decidiu utilizar uma conta de armazenamento GPv2 ou já tem uma e pretende avaliar se deve utilizar a camada de armazenamento frequente ou esporádico.

Em ambos os casos, a primeira prioridade é estimar os custos de armazenar e aceder aos seus dados armazenados numa conta de armazenamento GPv2 e compará-los com os custos que tem atualmente.

## <a name="evaluating-gpv2-storage-account-tiers"></a>Avaliar as camadas das conta de armazenamento GPv2

Para estimar os custos de armazenar e aceder aos dados armazenados numa conta de armazenamento GPv2, tem de avaliar o seu padrão de utilização existente ou fazer uma aproximação do padrão de utilização esperado. Em geral, precisa de saber o seguinte:

* O seu consumo de armazenamento: que quantidade de dados está a ser armazenada e como é que isso muda mensalmente?

* O padrão de acesso ao armazenamento: que quantidade de dados é lida da conta e escrita na mesma (incluindo dados novos)? Quantas transações são utilizadas para acesso a dados e de que tipo são?

## <a name="monitoring-existing-storage-accounts"></a>Monitorizar contas de armazenamento existentes

Para monitorizar as suas contas de armazenamento existentes e recolher estes dados, pode utilizar a Análise do Armazenamento do Azure, que efetua o registo e fornece dados de métricas para uma conta de armazenamento. A Análise de Armazenamento pode armazenar métricas que incluem estatísticas agregadas de transações e dados de capacidade sobre pedidos enviados para o serviço de armazenamento relativamente aos tipos de contas de Armazenamento de blobs, GPv1 e GPv2. Estes dados são armazenados em tabelas bem conhecidas na mesma conta de armazenamento.

Para obter mais informações, veja [About Storage Analytics Metrics (Sobre Métricas da Análise de Armazenamento)](https://msdn.microsoft.com/library/azure/hh343258.aspx) e [Storage Analytics Metrics Table Schema (Esquema da Tabela de Métricas da Análise de Armazenamento)](https://msdn.microsoft.com/library/azure/hh343264.aspx)

> [!NOTE]
> As contas de Armazenamento de blobs expõem o ponto final do serviço Tabela apenas para armazenar e aceder aos dados de métricas relativos a essa conta. 

Para monitorizar o consumo de armazenamento do Armazenamento de blobs, tem de ativar as métricas de capacidade.
Com esta opção ativada, os dados de capacidade são registados diariamente para um serviço Blob de uma conta de armazenamento e registados como uma entrada que é escrita na tabela *$MetricsCapacityBlob* dentro da mesma conta de armazenamento.

Para monitorizar os padrões de acesso a dados do Armazenamento de blobs, tem de ativar as métricas de transação horária a partir da API. Com as métricas de transação horária ativadas, as transações por API são agregadas ao fim de cada hora e registadas como uma entrada que é escrita na tabela *$MetricsHourPrimaryTransactionsBlob* dentro da mesma conta de armazenamento. A tabela *$MetricsHourSecondaryTransactionsBlob* regista as transações para o ponto final secundário quando são utilizadas contas de armazenamento RA-GRS.

> [!NOTE]
> Se tiver uma conta de armazenamento de fins gerais na qual armazenou blobs de páginas e discos de máquinas virtuais ou filas, ficheiros ou tabelas, juntamente com dados de blobs de blocos e de acréscimo, este processo de estimativa não é aplicável. Os dados de capacidade não distinguem os blobs de blocos dos outros tipos e, por isso, não disponibilizam dados de capacidade para outros tipos de dados. Se utilizar estes tipos, uma metodologia alternativa é ver as quantidades na sua fatura mais recente.

Para obter uma boa aproximação do seu consumo de dados e padrão de acesso, recomendamos que escolha um período de retenção para as métricas que seja representativo da sua utilização normal e o utilize para tirar conclusões. Uma opção é manter os dados das métricas durante sete dias e recolher os dados todas as semanas, para análise no fim do mês. Outra opção é manter os dados das métricas para os últimos 30 dias e recolher e analisar os dados no final desse período de 30 dias.

Para obter detalhes sobre como ativar, recolher e ver dados de métricas, veja [Enabling Azure Storage metrics and viewing metrics data)](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) (Ativar as métricas do Armazenamento do Azure e visualizar os dados das mesmas).

> [!NOTE]
> O armazenamento, o acesso e a transferência de dados de análise são cobrados também como dados normais do utilizador.

### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Utilizar métricas de utilização para estimar os custos

#### <a name="storage-costs"></a>Custos de armazenamento

A entrada mais recente na tabela de métricas de capacidade *$MetricsCapacityBlob* com a chave de linha *'data'* mostra a capacidade de armazenamento consumida pelos dados do utilizador. A entrada mais recente na tabela de métricas de capacidade *$MetricsCapacityBlob* com a chave de linha *'analytics'* mostra a capacidade de armazenamento consumida pelos registos de análise.

Esta capacidade total consumida pelos dados de utilizador e pelos registos de análise (se ativados) pode depois ser utilizada para estimar o custo do armazenamento dos dados na conta de armazenamento. O mesmo método também pode ser utilizado para estimar os custos de armazenamento nas contas GPv1.

#### <a name="transaction-costs"></a>Custos de transação

A soma dos *'TotalBillableRequests'*, em todas as entradas de uma API na tabela de métricas de transação indica o número total de transações para essa API específica. *Por exemplo,*, o número total de transações *'GetBlob'* num determinado período pode ser calculado pela soma do total de pedidos faturáveis para todas as entradas com a chave de linha *'user;GetBlob'*.

Para que possa estimar os custos de transação para as contas do Armazenamento de blobs, tem de dividir as transações em três grupos, uma vez que têm um preço diferente.

* Transações de escrita como *'PutBlob'*, *'PutBlock'*, *'PutBlockList'*, *'AppendBlock'*, *'ListBlobs'*, *'ListContainers'*, *'CreateContainer'*, *'SnapshotBlob'* e *'CopyBlob'*.
* Transações de eliminação como *'DeleteBlob'* e *'DeleteContainer'*.
* Todas as outras transações.

Para estimar os custos de transação nas contas de armazenamento GPv1, tem de agregar todas as transações, independentemente da operação/API.

### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Custos de acesso a dados e custos de transferência de dados de georreplicação

Embora a análise de armazenamento não forneça a quantidade de dados lidos e escritos numa conta de armazenamento, esta pode ser aproximadamente estimada ao consultar a tabela de métricas de transação. A soma de *'TotalIngress'* de todas as entradas de uma API na tabela de métricas de transação indica a quantidade total de dados de entrada em bytes para essa API específica. Do mesmo modo, a soma de *'TotalEgress'* indica a quantidade total de dados de saída, em bytes.

Para que possa estimar os custos de acesso a dados para as contas do Armazenamento de blobs, precisa de dividir as transações em dois grupos.

* A quantidade de dados obtidos a partir da conta de armazenamento pode ser estimada ao ver a soma de *'TotalEgress'* para, principalmente, as operações *'GetBlob'* e *'CopyBlob'*.

* A quantidade de dados escritos na conta de armazenamento pode ser estimada ao ver a soma de *'TotalIngress'* para, principalmente, as operações *'PutBlob'*, *'PutBlock'*, *'CopyBlob'* e *'AppendBlock'*.

Também pode ser calculado o custo de transferência de dados de georreplicação para contas do Armazenamento de blobs mediante a utilização da estimativa da quantidade de dados escritos se for utilizada uma conta de armazenamento GRS ou RA-GRS.

> [!NOTE]
> Para obter um exemplo mais detalhado sobre o cálculo dos custos de utilização da camada de armazenamento frequente ou esporádico, veja as perguntas mais frequentes com o nome *“O que são as camadas de acesso Frequente e Esporádico e como posso determinar qual delas utilizar?”* na [Página de Preços do Armazenamento do Azure](https://azure.microsoft.com/pricing/details/storage/).

## <a name="migrating-existing-data"></a>Migração de dados existentes

Uma conta GPv1 pode ser facilmente atualizada para GPv2 sem tempo de inatividade ou alterações à API e sem ser necessário migrar dados. Por este motivo, recomenda-se a migração das contas GPv1 para contas GPv2, em vez da migração para contas de Armazenamento de blobs.

No entanto, se precisar de migrar para uma conta de Armazenamento de blobs, pode utilizar as instruções seguintes.

Uma conta do Blob Storage é especializada para o armazenamento apenas de blobs de blocos e de acréscimo. Não é possível converter as contas de armazenamento para fins gerais existentes, que lhe permitem armazenar tabelas, filas, ficheiros, discos e também blobs, em contas do Armazenamento de blobs. Para utilizar as camadas de armazenamento, tem de criar contas do Armazenamento de blobs novas e migrar os dados existentes para as mesmas.

Pode utilizar os métodos seguintes para migrar os dados existentes para contas do Armazenamento de Blobs a partir de dispositivos de armazenamento no local, fornecedores de armazenamento na cloud externos ou das suas contas do armazenamento para fins gerais existentes no Azure:

### <a name="azcopy"></a>AzCopy

O AzCopy é um utilitário de linha de comandos do Windows concebido para copiar dados de e para o Storage do Azure com um elevado desempenho. Pode utilizar o AzCopy para copiar dados para a sua conta do Blob Storage a partir das suas contas do Storage para fins gerais existentes ou para carregar dados dos dispositivos de armazenamento no local para a conta do Blob Storage.

Para obter mais informações, veja [Transferir dados com o Utilitário de Linha de Comandos AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="data-movement-library"></a>Biblioteca de Movimento de Dados

A biblioteca de movimento de dados do Armazenamento do Azure para .NET baseia-se na arquitetura de movimento de dados central do AzCopy. A biblioteca foi concebida para operações de transferência de dados de elevado desempenho, fiáveis e simples, semelhantes às de AzCopy. Pode utilizá-la para tirar partido das funcionalidades fornecidas pelo AzCopy na sua aplicação de forma nativa, sem ter de executar e monitorizar instâncias externas do AzCopy.

Para obter mais informações, veja [Biblioteca de Movimento de Dados do Armazenamento do Azure para .Net](https://github.com/Azure/azure-storage-net-data-movement)

### <a name="rest-api-or-client-library"></a>API REST ou biblioteca de cliente

Pode criar uma aplicação personalizada para migrar os dados para uma conta do Blob Storage utilizando uma das bibliotecas de cliente do Azure ou a API REST dos serviços do Storage do Azure. O Storage do Azure fornece bibliotecas de cliente avançadas para várias linguagens e plataformas como .NET, Java, C++, Node.JS, PHP, Ruby e Python. As bibliotecas de cliente oferecem funcionalidades avançadas, tais como lógica de repetição, registo e carregamentos paralelos. Também pode desenvolver diretamente na API REST, que pode ser chamada por qualquer linguagem que efetue pedidos HTTP/HTTPS.

Para obter mais informações, veja [Introdução ao Armazenamento de Blobs do Azure](../blobs/storage-dotnet-how-to-use-blobs.md).

> [!IMPORTANT]
> Os blobs encriptados através de encriptação do lado do cliente armazenam os metadados relacionados com a encriptação com o blob. Se copiar um blob com encriptação do lado do cliente, certifique-se de que a operação de cópia preserva os metadados do blob e, especialmente, os metadados relacionados com a encriptação. Se copiar um blob sem os metadados de encriptação, não poderá obter novamente o conteúdo do mesmo. Para obter mais informações acerca dos metadados relacionados com a encriptação, veja [Encriptação do Lado do Cliente do Armazenamento do Azure](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="faq"></a>FAQ

**As contas de armazenamento existentes continuam disponíveis?**

Sim, as contas de armazenamento (GPv1) existentes continuam disponíveis e permanecem inalteradas em termos de preços e funcionalidade. As contas GPv1 não têm a capacidade de escolher uma camada de armazenamento e não terão capacidades de camadas no futuro.

**Por que razão devo começar a utilizar as contas de armazenamento GPv2 e quando o devo fazer?**

As contas de armazenamento GPv2 são especializadas para proporcionar os custos de armazenamento por GB mais baixos, oferecendo ao mesmo tempo os custos de transação e acesso a dados mais competitivos da indústria. Doravante, as contas de armazenamento GPv2 são a forma recomendada de armazenar blobs, uma vez que serão introduzidas capacidades futuras, como, por exemplo, notificações de alterações, baseadas neste tipo de conta. No entanto, o momento da migração fica ao seu critério, consoante os seus requisitos empresariais. Por exemplo, pode optar por otimizar os seus padrões de otimização antes de atualizar.

Não é suportada a mudança para versão anterior do GPv2, por isso considere todas as implicações de preços antes de atualizar as suas contas para GPv2.

**Posso atualizar a minha conta de armazenamento existente numa conta de armazenamento GPv2?**

Sim. As contas GPv1 podem ser facilmente atualizadas para GPv2 no portal ou utilizando o PowerShell ou a CLI. As contas do Armazenamento de blobs podem ser atualizadas para GPv2 com o PowerShell ou a CLI. A capacidade de atualizar uma conta de Armazenamento de blobs para GPv2 no portal estará disponível em breve.

Não é suportada a mudança para versão anterior do GPv2, por isso considere todas as implicações de preços antes de atualizar as suas contas para GPv2.

**Posso armazenar objetos em ambas as camadas de armazenamento na mesma conta?**

Sim. O atributo **Access Tier** definido ao nível de uma conta é a camada predefinida que se aplica a todos os objetos nessa conta sem uma camada de conjunto explícita. No entanto, a criação de camadas ao nível do blob permite-lhe definir a camada de acesso ao nível do objeto, independentemente de qual seja a definição de camada de acesso da conta. Os blobs em qualquer uma das três camadas de armazenamento (frequente, esporádico ou arquivo) podem existir na mesma conta.

**Posso alterar a camada de armazenamento da minha conta de armazenamento GPv2?**

Sim, pode definir o atributo **Access Tier** na conta de armazenamento para alterar a camada de armazenamento da conta. A alteração da camada de armazenamento da conta aplica-se a todos os objetos armazenados na conta que não têm uma camada explícita definida. A alteração da camada de armazenamento de frequente para esporádica acarreta custos com operações de escrita (por 10 000) (contas GPv2 apenas), enquanto a alteração de esporádica para frequente acarreta custos com operações de leitura (por 10 000) e obtenção de dados (por GB) pela leitura de todos os dados na conta.

**Com que frequência posso alterar a camada de armazenamento da minha conta de armazenamento de Blobs?**

Apesar de não impormos uma limitação à frequência com que é possível alterar a camada de armazenamento, tenha em atenção que a alteração da camada de esporádica para frequente resulta em encargos significativos. Não é recomendada uma alteração da camada de armazenamento frequentemente.

**O comportamento dos blobs na camada de armazenamento esporádico é diferente dos blobs na camada de armazenamento frequente?**

Os blobs na camada de armazenamento frequente das contas de Armazenamento de blobs e GPv2 têm a mesma latência dos blobs nas contas de armazenamento GPv1. Os blobs na camada de armazenamento esporádico têm uma latência semelhante (em milissegundos) aos blobs na camada frequente. Os blobs na camada de armazenamento de arquivo têm várias horas de latência.

Os blobs na camada de armazenamento esporádico têm um nível de serviço (SLA) de disponibilidade ligeiramente inferior ao dos blobs armazenados na camada de armazenamento frequente. Para obter mais informações, veja [SLA para Armazenamento](https://azure.microsoft.com/support/legal/sla/storage).

**Posso armazenar blobs de páginas e discos de máquinas virtuais em contas de armazenamento de Blobs?**

Não. As contas do Blob Storage suportam apenas blobs de blocos e de acréscimo e não suportam blobs de páginas. Os discos de Virtual Machines do Azure são apoiados por blobs de páginas e, como consequência, não é possível utilizar contas do Blob Storage para armazenar discos de máquinas virtuais. No entanto, é possível armazenar cópias de segurança dos discos de máquinas virtuais como blobs de blocos numa conta do Blob Storage. Esta é uma das razões para considerar a utilização de GPv2 em vez de contas de Armazenamento de blobs.

**Tenho de alterar as minhas aplicações existentes para utilizar as contas de armazenamento GPv2?**

As contas de armazenamento GPv2 são 100% consistentes em termos de API com as contas GPv1 e de Armazenamento de blobs. Desde que a aplicação utilize blobs de blocos ou blobs de acréscimo e esteja a utilizar a versão de 14/02/2014 da [API REST dos Serviços do Storage](https://msdn.microsoft.com/library/azure/dd894041.aspx) ou superior, a aplicação deverá funcionar normalmente. Se estiver a utilizar uma versão mais antiga do protocolo, tem de atualizar a sua aplicação para utilizar a versão nova, de modo a trabalhar de forma totalmente integrada com ambos os tipos de contas de armazenamento. Em geral, recomendamos sempre que utilize a versão mais recente, independentemente da conta do Storage utilizada.

Os preços de GPv2 são, geralmente, superiores aos de GPv1 para transações e largura de banda. Por conseguinte, poderá ser necessário otimizar os padrões de transação antes de atualizar para que a fatura total não aumente.

**Existem alterações relativas à experiência do utilizador?**

As contas de armazenamento GPv2 são muito semelhantes às GPv1 e suportam todas as principais funcionalidades do Armazenamento do Azure, incluindo elevada durabilidade e disponibilidade, escalabilidade, desempenho e segurança. Com exceção das funcionalidades e restrições específicas das contas do Armazenamento de blobs e das respetivas camadas de armazenamento que foram descritas acima, tudo o resto permanece igual após a atualização para a conta GPv2 ou de Armazenamento de blobs.

## <a name="next-steps"></a>Passos seguintes

### <a name="evaluate-blob-storage-accounts"></a>Avaliar as contas do Blob Storage

[Verificar a disponibilidade das contas do armazenamento de Blobs por região](https://azure.microsoft.com/regions/#services)

[Avaliar a utilização das suas contas de armazenamento atuais ao ativar as métricas do Armazenamento do Azure](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Verificar os preços do armazenamento de Blobs por região](https://azure.microsoft.com/pricing/details/storage/)

[Verificar os preços das transferências de dados](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-gpv2-storage-accounts"></a>Começar a utilizar as contas de armazenamento GPv2

[Introdução ao armazenamento de Blobs do Azure](../blobs/storage-dotnet-how-to-use-blobs.md)

[Mover dados de e para o Armazenamento do Azure](../common/storage-moving-data.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Transferir dados com o Utilitário de Linha de Comandos AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Procurar e explorar as suas contas de armazenamento](http://storageexplorer.com/)
