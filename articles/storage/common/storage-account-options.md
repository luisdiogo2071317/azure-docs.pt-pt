---
title: "Opções de contas de Armazenamento do Azure | Microsoft Docs"
description: "Compreenda as opções para utilizar o Armazenamento do Azure."
services: storage
documentationcenter: 
author: jirwin
manager: jwillis
editor: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/11/2017
ms.author: jirwin
ms.openlocfilehash: 7f07734433694999d38429ca264c58c5f3c619e1
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/19/2017
---
# <a name="azure-storage-account-options"></a>Opções de contas do Armazenamento do Azure

## <a name="overview"></a>Descrição geral
O Armazenamento do Azure oferece três opções de contas diferentes, com preços e funcionalidades distintas suportadas. É importante que os utilizadores tenham em conta estas diferenças para saberem qual das opções melhor se adequa às respetivas aplicações.  As três opções são as seguintes:

* Contas de **Fins Gerais v2 (GPv2)**, que disponibilizam todas as funcionalidades mais recentes e suportam Blobs, Ficheiros, Filas e Tabelas. Hoje em dia, estas funcionalidades mais recentes incluem a atribuição de camadas ao nível do blob, o armazenamento de arquivo, limites de contas com maior dimensionamento e eventos de armazenamento. Os preços foram estabelecidos para serem os mais baixos por GB e os mais competitivos da indústria para transações.

* Contas de **Armazenamento de Blobs**, que oferecem todas as últimas funcionalidades para blobs de bloco, mas que só suportam este tipo de blobs.  Os preços são essencialmente semelhantes aos das contas de Fins Gerais v2. Encorajamos a maioria dos utilizadores a utilizar contas de Fins Gerais v2 em vez de Armazenamento de Blobs.

* Contas de **Fins Gerais v1 (GPv1)**, que permitem utilizar todos os Serviços do Armazenamento do Azure, mas podem não ter as mais recentes funcionalidades ou os preços mais baixos por GB. Por exemplo, o armazenamento esporádico e de arquivo não são suportados nas contas GPv1.  O preço é mais baixo para as transações, pelo que as cargas de trabalho com elevado número de alterações ou velocidades de leitura altas podem tirar partido deste tipo de conta.

### <a name="changing-account-kind"></a>Alterar o tipo de conta
Os utilizadores podem atualizar as contas GPv1 ou de Armazenamento de Blobs para uma conta GPv2 em qualquer altura através do portal, da CLI ou do PowerShell. Esta alteração não pode ser revertida e não são permitidas quaisquer outras alterações.

## <a name="general-purpose-v2"></a>Fins Gerais v2
As contas de **Fins Gerais v2 (GPv2)** são contas de armazenamento que suportam todas as funcionalidades de todos os serviços de armazenamento, incluindo Blobs, Ficheiros, Filas e Tabelas. Para os Blobs de Blocos, pode escolher as camadas de armazenamento frequente e esporádico ao nível da conta ou as camadas frequente, esporádica e de arquivo ao nível do blob com base nos padrões de acesso. Armazene dados acedidos com muita frequência, com pouca frequência e raramente nas camadas de armazenamento frequente, esporádico e de arquivo, respetivamente, para otimizar os custos. Mais importante ainda, qualquer conta GPv1 pode ser atualizada para uma GPv2 no portal, na CLI ou no PowerShell. As contas GPv2 suportam as mesmas APIs e funcionalidades das contas de Armazenamento de Blobs e GPv1 e partilham as mesmas excelentes características de durabilidade, disponibilidade, escalabilidade e desempenho daqueles tipos de contas.

As contas de Armazenamento de blobs expõem o atributo **Access Tier** ao nível da conta, o qual especifica a camada da conta de armazenamento predefinida como **Frequente** ou **Esporádica**. A camada da conta de armazenamento predefinida é aplicada a qualquer blob que não tenha uma camada explícita definida ao nível do blob. Se o padrão de utilização dos dados sofrer uma alteração, pode também alternar entre estas camadas de armazenamento em qualquer altura. A **camada de arquivo** só pode ser aplicada ao nível do blob.

> [!NOTE]
> A alteração da camada de armazenamento poderá resultar em encargos adicionais. Veja a secção [Preços e faturação](#pricing-and-billing) para obter mais detalhes.

## <a name="blob-storage-accounts"></a>Contas de Armazenamento de blobs

As **contas de Armazenamento de Blobs** suportam as mesmas funcionalidades de Blobs de Blocos de GPv2, mas têm a limitação de só suportar este tipo de blocos. Os clientes devem analisar as diferenças nos preços entre as contas de Armazenamento de Blobs e GPv2 e considerar atualizar para esta última. Tenha em atenção que esta atualização não pode ser anulada.

> [!NOTE]
> As contas de Armazenamento de Blobs suportam apenas blobs de blocos e blobs de acréscimo e não suportam blobs de páginas.

## <a name="general-purpose-v1"></a>Fins Gerais v1
As contas de **Fins Gerais v1 (GPv1)** são as contas de armazenamento mais antigas e as únicas que podem ser utilizadas no modelo de implementação clássica. Funcionalidades como armazenamento esporádico e de arquivo não estão disponíveis em GPv1. Geralmente, estas contas têm custos de armazenamento por GB superiores, mas custos de transação inferiores do que as contas GPv2 ou de Armazenamento de Blobs.

## <a name="recommendations"></a>Recomendações

Consulte [Acerca das contas do Storage do Azure](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) para obter mais informações sobre contas do Storage.

Para as aplicações que requerem apenas o armazenamento de blobs de blocos ou de acréscimo, recomendamos que utilize contas de armazenamento GPv2, de modo a tirar partido do modelo de preços diferenciado do armazenamento em camadas. No entanto, compreendemos que tal pode não ser possível em determinadas circunstâncias, nas quais a utilização de contas de armazenamento GPv1 seria mais adequada, tais como:

* Ainda tem de utilizar o modelo de implementação clássica. As contas do Armazenamento de Blobs só estão disponíveis através do modelo de implementação Azure Resource Manager.

* Se utilizar elevados volumes de transações ou de largura de rede de georreplicação, os quais são ambos mais caros nas contas GPv2 e de Armazenamento de Blos do que em GPv1, e não tem armazenamento suficiente que lhe permita tirar partido dos custos mais baixos de armazenamento por GB.

* Utiliza uma versão da [API REST dos Serviços do Storage](https://msdn.microsoft.com/library/azure/dd894041.aspx) anterior a 14/02/2014 ou uma biblioteca de cliente com uma versão inferior à 4.x e não pode atualizar a sua aplicação.

> [!NOTE]
> As contas de Armazenamento de Blobs são suportadas em todas as regiões do Azure.

## <a name="pricing-and-billing"></a>Preços e faturação
Todas as contas de armazenamento utilizam um modelo de preços para o armazenamento de blobs com base na camada de cada blob. Ao utilizar uma conta de armazenamento, aplicam-se as seguintes considerações de faturação:

* **Custos de armazenamento**: para além da quantidade de dados armazenados, o custo do armazenamento de dados varia consoante a camada de armazenamento. O custo por gigabyte diminui conforme a camada se torna mais esporádica.

* **Custos de acesso a dados**: os custos de acesso a dados aumenta conforme a camada se torna mais esporádica. Para os dados na camada frequente e de armazenamento de arquivo, é cobrada uma taxa de acesso a dados por gigabyte pelas operações de leitura.

* **Custos de transação**: há um encargo por transação para todas as camadas que aumenta cada vez que a camada é mais esporádica.

* **Custos de transferência de dados de georreplicação**: aplica-se apenas às contas que têm a georreplicação configurada, incluindo GRS e RA-GRS. A transferência de dados de georreplicação está sujeita a uma taxa por gigabyte.

* **Custos de transferência de dados de saída**: as transferências de dados de saída (dados que são transferidos para fora de uma região do Azure) estão sujeitas a uma cobrança pela utilização de largura de banda por gigabyte, tal como as contas do Storage para fins gerais.

* **Alteração da camada de armazenamento**: a alteração da camada de armazenamento de conta esporádica para armazenamento frequente está sujeita à cobrança de uma taxa igual à leitura de todos os dados existentes na conta do armazenamento. No entanto, a alteração da camada de armazenamento de conta de frequente para esporádica incorre um encargo igual à escrita de todos os dados na camada esporádica (contas GPv2 apenas).

> [!NOTE]
> Para obter mais detalhes sobre o modelo de preços das contas do Armazenamento de Blobs, veja a página [Preços do Armazenamento do Azure](https://azure.microsoft.com/pricing/details/storage/). Para obter mais detalhes sobre as taxas aplicáveis às transferências de dados de saída, veja a página [Detalhes de Preços das Transferências de Dados](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="quickstart-scenarios"></a>Cenários de início rápido

Nesta secção os seguintes cenários são demonstrados através do portal do Azure:

* Como criar uma conta de armazenamento GPv2.
* Como converter uma conta de Armazenamento de Blobs ou GPv1 numa conta de armazenamento GPv2.
* Como definir a camada da conta e dos blobs em contas de armazenamento GPv2.

Nos exemplos seguintes, não pode definir a camada de acesso como arquivo porque esta definição aplica-se a toda a conta de armazenamento. O armazenamento de arquivo só pode ser definido em blobs específicos.

### <a name="create-a-gpv2-storage-account-using-the-azure-portal"></a>Criar uma conta de armazenamento GPv2 através do portal do Azure

1. Inicie sessão no [Portal do Azure](https://portal.azure.com).

2. No menu Hub, selecione **Novo** > **Dados + Armazenamento** > **Conta do Storage**.

3. Introduza um nome para a conta do Storage.

    Este nome tem de ser globalmente exclusivo; é utilizado como parte do URL utilizado para aceder aos objetos na conta de armazenamento.  

4. Selecione **Resource Manager** como o modelo de implementação.

    O armazenamento em camadas só pode ser utilizado com contas de armazenamento do Resource Manager; este é o modelo de implementação recomendado para novos recursos. Para obter mais informações, veja [Descrição geral do Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).  

5. Na lista pendente Tipo de Conta, selecione **Fins Gerais v2**.

    É aqui que pode seleciona o tipo de conta de armazenamento. O armazenamento em camadas não está disponível no armazenamento para fins gerais; só está disponível na conta de tipo de Armazenamento de Blobs.     

    Quando seleciona esta opção, a camada de desempenho fica definida como Standard. O armazenamento em camadas não está disponível com a camada de desempenho Premium.

6. Selecione a opção de replicação para a conta do Storage: **LRS**, **GRS** ou **RA-GRS**. A predefinição é **RA-GRS**.

    LRS = armazenamento localmente redundante; GRS = armazenamento georredundante (duas regiões); RA-GRS é armazenamento georredundante com acesso de leitura (2 regiões com acesso de leitura para a segunda).

    Consulte [Azure Storage replication (Replicação do Armazenamento do Azure)](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) para obter detalhes adicionais sobre as opções de replicação do Armazenamento do Azure.

7. Selecione a camada de armazenamento correta para as suas necessidades: defina a **Camada de acesso** como **Esporádica** ou **Frequente**. A predefinição é **Frequente**.

8. Selecione a subscrição na qual pretende criar a nova conta do Storage.

9. Especifique um novo grupo de recursos ou selecione um grupo de recursos existente. Para obter mais informações sobre os grupos de recursos, veja [Descrição geral do Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

10. Selecione a região para a sua conta de armazenamento.

11. Clique em **Criar** para criar a conta do Storage.

### <a name="convert-a-gpv1-or-blob-storage-account-to-a-gpv2-storage-account-using-the-azure-portal"></a>Converter uma conta de Armazenamento de Blobs ou GPv1 numa conta de armazenamento GPv2 com o portal do Azure

1. Inicie sessão no [Portal do Azure](https://portal.azure.com).

2. Para navegar para a sua conta de armazenamento, selecione Todos os Recursos, em seguida, selecione a sua conta de armazenamento.

3. No painel Definições, clique em **Configuração**.

4. Em Tipo de Conta, clique em **Atualizar**.

5. É apresentado um painel novo à direita, para confirmação. Em Confirmar atualização, escreva o nome da sua conta. 

5. Clique em Atualizar, na parte inferior do painel.

### <a name="change-the-storage-tier-of-a-gpv2-storage-account-using-the-azure-portal"></a>Alterar a camada de armazenamento de uma conta de armazenamento GPv2 com o portal do Azure

1. Inicie sessão no [Portal do Azure](https://portal.azure.com).

2. Para navegar para a sua conta de armazenamento, selecione Todos os Recursos, em seguida, selecione a sua conta de armazenamento.

3. No painel Definições, clique em**Configuração** para ver e/ou alterar a configuração da conta.

4. Selecione a camada de armazenamento correta para as suas necessidades: defina a **Camada de acesso** como **Esporádica** ou **Frequente**.

5. Clique em Guardar na parte superior do painel.

### <a name="change-the-storage-tier-of-a-blob-using-the-azure-portal"></a>Alterar a camada de armazenamento de um blob através do portal do Azure

1. Inicie sessão no [Portal do Azure](https://portal.azure.com).

2. Para navegar para o blob na conta de armazenamento, selecione Todos os Recursos, selecione a conta de armazenamento, selecione o contentor e, em seguida, selecione o blob.

3. No painel de propriedades do Blob, clique no menu pendente **Camada de Acesso** para selecionar a camada de armazenamento **Frequente**, **Esporádica** ou **Arquivo**.

5. Clique em Guardar na parte superior do painel.

> [!NOTE]
> A alteração da camada de armazenamento poderá resultar em encargos adicionais. Veja a secção [Preços e faturação](#pricing-and-billing) para obter mais detalhes.


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

Para monitorizar as suas contas de armazenamento existentes e recolher estes dados, pode utilizar a Análise do Armazenamento do Azure, que efetua o registo e fornece dados de métricas para uma conta de armazenamento. A Análise do Armazenamento pode armazenar métricas que incluem estatísticas agregadas de transações e dados de capacidade sobre pedidos enviados ao serviço de armazenamento relativamente aos tipos de contas de Armazenamento de Blobs, GPv1 e GPv2. Estes dados são armazenados em tabelas bem conhecidas na mesma conta de armazenamento.

Para obter mais detalhes, veja [About Storage Analytics Metric (Sobre Métricas da Análise do Armazenamento)](https://msdn.microsoft.com/library/azure/hh343258.aspx) e [Storage Analytics Metrics Table Schema (Esquema da Tabela de Métricas da Análise do Armazenamento)](https://msdn.microsoft.com/library/azure/hh343264.aspx)

> [!NOTE]
> As contas do Armazenamento de Blobs expõem o ponto final de serviço de tabelas apenas para armazenar e aceder aos dados de métricas para essa conta. As contas de armazenamento GPv1 ZRS não suportam os dados de métricas.

Para monitorizar o consumo de armazenamento do serviço de Armazenamento de Blobs, tem de ativar as métricas de capacidade.
Com esta opção ativada, os dados de capacidade são registados diariamente para um serviço Blob de uma conta de armazenamento e registados como uma entrada que é escrita na tabela *$MetricsCapacityBlob* dentro da mesma conta de armazenamento.

Para monitorizar o padrão de acesso a dados do serviço de Armazenamento de Blobs, tem de ativar as métricas de transação horária ao nível da API. Com esta opção ativada, as transações por API são agregadas ao fim de cada hora e registadas como uma entrada que é escrita na tabela *$MetricsHourPrimaryTransactionsBlob* dentro da mesma conta de armazenamento. A tabela *$MetricsHourSecondaryTransactionsBlob* regista as transações para o ponto final secundário quando são utilizadas contas de armazenamento RA-GRS.

> [!NOTE]
> No caso de ter uma conta de armazenamento de fins gerais em que tem armazenados blobs de páginas e discos de máquinas virtuais ou filas, ficheiros ou tabelas em conjunto com dados de blobs de blocos e de acréscimo, este processo de estimativa não é aplicável. Isto acontece porque os dados de capacidade não distinguem os blobs de blocos dos outros tipos e não disponibiliza dados de capacidade aos outros tipos de dados. Se utilizar estes tipos, uma metodologia alternativa é ver as quantidades na sua fatura mais recente.

Para obter uma boa aproximação do seu consumo de dados e padrão de acesso, recomendamos que escolha um período de retenção para as métricas que seja representativo da sua utilização normal e o utilize para tirar conclusões. Uma opção é manter os dados das métricas durante sete dias e recolher os dados todas as semanas, para análise no fim do mês. Outra opção é manter os dados das métricas para os últimos 30 dias e recolher e analisar os dados no final desse período de 30 dias.

Para obter detalhes sobre como ativar, recolher e ver dados de métricas, veja [Enabling Azure Storage metrics and viewing metrics data)](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) (Ativar as métricas do Armazenamento do Azure e visualizar os dados das mesmas).

> [!NOTE]
> O armazenamento, o acesso e a transferência de dados de análise são cobrados também como dados normais do utilizador.

### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Utilizar métricas de utilização para estimar os custos

### <a name="storage-costs"></a>Custos de armazenamento

A entrada mais recente na tabela de métricas de capacidade *$MetricsCapacityBlob* com a chave de linha *'data'* mostra a capacidade de armazenamento consumida pelos dados do utilizador. A entrada mais recente na tabela de métricas de capacidade *$MetricsCapacityBlob* com a chave de linha *'analytics'* mostra a capacidade de armazenamento consumida pelos registos de análise.

Esta capacidade total consumida pelos dados de utilizador e pelos registos de análise (se ativados) pode depois ser utilizada para estimar o custo do armazenamento dos dados na conta de armazenamento. O mesmo método também pode ser utilizado para estimar os custos de armazenamento nas contas GPv1.

### <a name="transaction-costs"></a>Custos de transação

A soma dos *'TotalBillableRequests'*, em todas as entradas de uma API na tabela de métricas de transação indica o número total de transações para essa API específica. *Por exemplo,*, o número total de transações *'GetBlob'* num determinado período pode ser calculado pela soma do total de pedidos faturáveis para todas as entradas com a chave de linha *'user;GetBlob'*.

Para que possa estimar os custos de transação para as contas do Armazenamento de Blobs, tem de dividir as transações em três grupos, uma vez que têm um preço diferente.

* Transações de escrita como *'PutBlob'*, *'PutBlock'*, *'PutBlockList'*, *'AppendBlock'*, *'ListBlobs'*, *'ListContainers'*, *'CreateContainer'*, *'SnapshotBlob'* e *'CopyBlob'*.
* Transações de eliminação como *'DeleteBlob'* e *'DeleteContainer'*.
* Todas as outras transações.

Para estimar os custos de transação nas contas de armazenamento GPv1, tem de agregar todas as transações, independentemente da operação/API.

### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Custos de acesso a dados e custos de transferência de dados de georreplicação

Embora a análise de armazenamento não forneça a quantidade de dados lidos e escritos numa conta de armazenamento, esta pode ser aproximadamente estimada ao consultar a tabela de métricas de transação. A soma de *'TotalIngress'* de todas as entradas de uma API na tabela de métricas de transação indica a quantidade total de dados de entrada em bytes para essa API específica. Do mesmo modo, a soma de *'TotalEgress'* indica a quantidade total de dados de saída, em bytes.

Para poder estimar os custos de acesso a dados para as contas do Armazenamento de Blobs, tem de dividir as transações em dois grupos.

* A quantidade de dados obtidos a partir da conta de armazenamento pode ser estimada ao ver a soma de *'TotalEgress'* para, principalmente, as operações *'GetBlob'* e *'CopyBlob'*.

* A quantidade de dados escritos na conta de armazenamento pode ser estimada ao ver a soma de *'TotalIngress'* para, principalmente, as operações *'PutBlob'*, *'PutBlock'*, *'CopyBlob'* e *'AppendBlock'*.

Também pode ser calculado o custo de transferência de dados de georreplicação para contas do Armazenamento de Blobs mediante a utilização da estimativa da quantidade de dados escritos, se for utilizada uma conta de armazenamento GRS ou RA-GRS.

> [!NOTE]
> Para obter um exemplo mais detalhado sobre o cálculo dos custos de utilização da camada de armazenamento frequente ou esporádico, veja as perguntas mais frequentes com o nome *“O que são as camadas de acesso Frequente e Esporádico e como posso determinar qual delas utilizar?”* na [Página de Preços do Armazenamento do Azure](https://azure.microsoft.com/pricing/details/storage/).

## <a name="migrating-existing-data"></a>Migração de dados existentes

As contas GPv1 ou de Armazenamento de Blobs podem ser facilmente atualizadas para GPv2 sem tempo de inatividade ou alterações à API e sem ser necessário movimentar dados. Este é um dos principais benefícios das contas GPv2 face às contas de Armazenamento de Blobs.

No entanto, se tiver de migrar para uma conta de Armazenamento de Blobs, pode utilizar as instruções abaixo.

As contas de Armazenamento de Blobs foram criadas especificamente para armazenar apenas blobs de blocos e de acréscimo. Não é possível converter as contas de armazenamento de fins gerais existentes, que lhe permitem armazenar tabelas, filas, ficheiros, discos e também blobs, em contas de Armazenamento de Blobs. Para utilizar as camadas de armazenamento, tem de criar contas de Armazenamento de Blobs novas e migrar os dados existentes para as mesmas.

Pode utilizar os métodos seguintes para migrar os dados existentes para contas de Armazenamento de Blobs a partir de dispositivos de armazenamento no local, fornecedores de armazenamento na cloud externos ou das suas contas de armazenamento de fins gerais existentes no Azure:

### <a name="azcopy"></a>AzCopy

O AzCopy é um utilitário de linha de comandos do Windows concebido para copiar dados de e para o Storage do Azure com um elevado desempenho. Pode utilizar AzCopy para copiar dados para a sua conta de Armazenamento de Blobs a partir das suas contas de Armazenamento de fins gerais existentes ou para carregar dados de dispositivos de armazenamento no local para a sua conta de Armazenamento de Blobs.

Para obter mais detalhes, consulte [Transferir dados com o utilitário de linha de comandos AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="data-movement-library"></a>Biblioteca de Movimento de Dados

A biblioteca de movimento de dados do Storage do Azure para .NET baseia-se na arquitetura de movimento de dados central do AzCopy. A biblioteca foi concebida para operações de transferência de dados de elevado desempenho, fiáveis e simples, semelhantes às de AzCopy. Isto permite-lhe tirar o máximo partido das funcionalidades fornecidas pelo AzCopy na sua aplicação de forma nativa, sem ter de executar e monitorizar instâncias externas do AzCopy.

Para obter mais detalhes, consulte [Biblioteca de Movimento de Dados do Storage do Azure para .Net](https://github.com/Azure/azure-storage-net-data-movement)

### <a name="rest-api-or-client-library"></a>API REST ou biblioteca de cliente

Pode criar uma aplicação personalizada para migrar os dados para uma conta de Armazenamento de Blobs com uma das bibliotecas de cliente do Azure ou a API REST dos serviços de Armazenamento do Azure. O Storage do Azure fornece bibliotecas de cliente avançadas para várias linguagens e plataformas como .NET, Java, C++, Node.JS, PHP, Ruby e Python. As bibliotecas de cliente oferecem funcionalidades avançadas, tais como lógica de repetição, registo e carregamentos paralelos. Também pode desenvolver diretamente na API REST, que pode ser chamada por qualquer linguagem que efetue pedidos HTTP/HTTPS.

Para obter mais detalhes, veja [Introdução ao Armazenamento de Blobs do Azure](../blobs/storage-dotnet-how-to-use-blobs.md).

> [!NOTE]
> Os blobs encriptados através de encriptação do lado do cliente armazenam os metadados relacionados com a encriptação armazenados com o blob. É fundamental que os mecanismos de cópia utilizados assegurem que os metadados do blob, e, em especial, os metadados relacionados com a encriptação, são preservados. Se copiar os blobs sem estes metadados, não será possível obter novamente o conteúdo do mesmo. Para obter mais detalhes acerca dos metadados relacionados com a encriptação, consulte [Encriptação do Lado do Cliente do Armazenamento do Azure](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="faq"></a>FAQ

**As contas de armazenamento existentes continuam disponíveis?**

Sim, as contas do Storage existentes continuam disponíveis e permanecem inalteradas em termos de preços e funcionalidade.  Não têm a capacidade de escolher uma camada de armazenamento e não terão capacidades de camadas no futuro.

**Por que razão devo começar a utilizar as contas de armazenamento GPv2 e quando o devo fazer?**

As contas de armazenamento GPv2 são especializadas para proporcionar os custos de armazenamento por GB mais baixos, oferecendo ao mesmo tempo os custos de transação e acesso a dados mais competitivos da indústria. Doravante, as contas de armazenamento GPv2 são a forma recomendada de armazenar blobs, uma vez que serão introduzidas capacidades futuras, como, por exemplo, notificações de alterações, baseadas neste tipo de conta. No entanto, o momento da migração fica ao seu critério, consoante os seus requisitos empresariais.  Por exemplo, pode optar por otimizar os seus padrões de otimização antes de atualizar.

**Posso atualizar a minha conta de armazenamento existente numa conta de armazenamento GPv2?**

Sim. As contas de Armazenamento de Blobs ou GPv1 podem ser facilmente atualizadas para GPv2 no portal.

**Posso armazenar objetos em ambas as camadas de armazenamento na mesma conta?**

Sim. O atributo **Access Tier** definido ao nível de uma conta é a camada predefinida que se aplica a todos os objetos nessa conta sem uma camada de conjunto explícita. No entanto, a criação de camadas ao nível do blob permite-lhe definir a camada de acesso ao nível do objeto, independentemente de qual seja a definição de camada de acesso da conta. Os blobs em qualquer uma das três camadas de armazenamento (frequente, esporádico ou arquivo) podem existir na mesma conta.

**Posso alterar a camada de armazenamento da minha conta de armazenamento GPv2?**

Sim, pode definir o atributo **Access Tier** na conta de armazenamento para alterar a camada de armazenamento da conta. A alteração da camada de armazenamento da conta aplica-se a todos os objetos armazenados na conta que não têm uma camada explícita definida. A alteração da camada de armazenamento de frequente para esporádica acarreta custos com operações de escrita (por 10 000) (contas GPv2 apenas), enquanto a alteração de esporádica para frequente acarreta custos com operações de leitura (por 10 000) e obtenção de dados (por GB) pela leitura de todos os dados na conta.

**Com que frequência posso alterar a camada de armazenamento da minha conta de Armazenamento de Blobs?**

Apesar de não impormos uma limitação à frequência com que é possível alterar a camada de armazenamento, tenha em atenção que a alteração da camada de esporádica para frequente resulta em encargos significativos. Não é recomendada uma alteração da camada de armazenamento frequentemente.

**O comportamento dos blobs na camada de armazenamento esporádico é diferente dos blobs na camada de armazenamento frequente?**

Os blobs na camada de armazenamento frequente das contas de Armazenamento de Blobs e GPv2 têm a mesma latência dos blobs nas contas GPv1. Os blobs na camada de armazenamento esporádico têm uma latência semelhante (em milissegundos) aos blobs na camada frequente. Os blobs na camada de armazenamento de arquivo têm várias horas de latência.

Os blobs na camada de armazenamento esporádico têm um nível de serviço (SLA) de disponibilidade ligeiramente inferior ao dos blobs armazenados na camada de armazenamento frequente. Para obter mais detalhes, consulte [SLA para o armazenamento](https://azure.microsoft.com/support/legal/sla/storage).

**Posso armazenar blobs de páginas e discos de máquinas virtuais em contas de Armazenamento de Blobs?**

Não. As contas de Armazenamento de Blobs suportam apenas blobs de blocos e blobs de acréscimo e não suportam blobs de páginas. Os discos de máquinas virtuais do Azure são apoiados por blobs de páginas e, como consequência, não é possível utilizar contas do Armazenamento de Blobs para armazená-los. No entanto, é possível armazenar cópias de segurança dos discos de máquinas virtuais como blobs de blocos numa conta de Armazenamento de Blobs. Esta é uma das razões para considerar a utilização de GPv2 em vez de contas de Armazenamento de Blobs.

**Tenho de alterar as minhas aplicações existentes para utilizar as contas de armazenamento GPv2?**

As contas de armazenamento GPv2 são 100% consistentes com as contas GPv1 e de Armazenamento de Blobs. Desde que a aplicação utilize blobs de blocos ou blobs de acréscimo e esteja a utilizar a versão de 14/02/2014 da [API REST dos Serviços do Storage](https://msdn.microsoft.com/library/azure/dd894041.aspx) ou superior, a aplicação deverá funcionar normalmente. Se estiver a utilizar uma versão mais antiga do protocolo, tem de atualizar a sua aplicação para utilizar a versão nova, de modo a trabalhar de forma totalmente integrada com ambos os tipos de contas de armazenamento. Em geral, recomendamos sempre que utilize a versão mais recente, independentemente da conta do Storage utilizada.

**Existem alterações relativas à experiência do utilizador?**

As contas de armazenamento GPv2 são muito semelhantes às GPv1 e suportam todas as principais funcionalidades do Armazenamento do Azure, incluindo elevada durabilidade e disponibilidade, escalabilidade, desempenho e segurança. Com exceção das funcionalidades e restrições específicas das contas do Armazenamento de Blobs e das respetivas camadas de armazenamento que foram descritas acima, tudo o resto permanece igual após a atualização para a conta GPv2 ou de Armazenamento do Azure.

## <a name="next-steps"></a>Passos seguintes

### <a name="evaluate-blob-storage-accounts"></a>Avaliar as contas de Armazenamento do Azure

[Verificar a disponibilidade das contas do Armazenamento de Blobs por região](https://azure.microsoft.com/regions/#services)

[Avaliar a utilização das suas contas de armazenamento atuais ao ativar as métricas do Armazenamento do Azure](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Verificar os preços do Armazenamento de Blobs por região](https://azure.microsoft.com/pricing/details/storage/)

[Verificar os preços das transferências de dados](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-gpv2-storage-accounts"></a>Começar a utilizar as contas de armazenamento GPv2

[Introdução ao Armazenamento de Blobs do Azure](../blobs/storage-dotnet-how-to-use-blobs.md)

[Mover dados de e para o Armazenamento do Azure](../common/storage-moving-data.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Transferir dados com o Utilitário de Linha de Comandos AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Procurar e explorar as suas contas de armazenamento](http://storageexplorer.com/)
