---
title: Notas de versão do Azure SDK para .NET 2.7 e o .NET 2.7.1
description: Notas de versão do Azure SDK para .NET 2.7 e o .NET 2.7.1
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: ''
ms.assetid: 877d070a-9bd5-49b3-8fac-6bb5f65c3554
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 4c5a4dfcdde91d1bd0c2728ff9d071d4c2f73f0e
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52969775"
---
# <a name="azure-sdk-for-net-27-and-net-271-release-notes"></a>Notas de versão do Azure SDK para .NET 2.7 e o .NET 2.7.1
## <a name="overview"></a>Descrição geral
Este documento contém as notas de versão para o Azure SDK para .NET 2.7 versão. 

O documento também contêm as notas de versão do SDK do Azure para .NET 2.7.1 de versão.

Azure SDK 2.7 só é suportado no Visual Studio 2015 e o Visual Studio 2013. [Azure SDK 2.6](https://azure.microsoft.com/downloads/) é a última suportada SDK para Visual Studio 2012.

Para obter informações detalhadas sobre esta versão, consulte [mensagem de anúncio do Azure SDK 2.7](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) e [mensagem de anúncio do Azure SDK 2.7.1](https://go.microsoft.com/fwlink/?LinkId=623850).

## <a name="azure-sdk-for-net-27"></a>Azure SDK para .NET 2.7
### <a name="sign-in-improvements-for-visual-studio-2015"></a>Inicie sessão em aprimoramentos para o Visual Studio 2015
Azure SDK 2.7 para Visual Studio 2015 oferece suporte as novas funcionalidades de gestão de identidade no Visual Studio 2015.  Isto inclui suporte para contas de aceder ao Azure através do controlo de acesso baseado em funções, fornecedores de soluções Cloud, DreamSpark e outros tipos de conta e subscrição.

Os aprimoramentos de início de sessão incluídos com o Azure SDK 2.7 só estão disponíveis no Visual Studio 2015. Suporte para Visual Studio 2013 está incluído no Azure SDK 2.7.1.

### <a name="mobile-sdk"></a>SDK móvel
Atualizado **Mobile Apps** modelos para refletir a mais recente [pacote NuGet](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) e o processo de configuração.

### <a name="service-bus"></a>Service Bus
Gerais de correções de erros e melhorias. Para obter detalhes sobre atualizações e funcionalidades, consulte as notas de versão da versão mais recente [NuGet do Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/).

### <a name="hdinsight-tools"></a>Ferramentas do HDInsight
Nesta versão, foram efetuadas as seguintes atualizações. Estas atualizações estão em pré-visualização. Para obter mais informações, consulte [este blog](https://go.microsoft.com/fwlink/?LinkId=619108).

* Gráficos de ramo de registo para o Hive no tarefas do Tez
* Suporte total do IntelliSense de DML do Hive
* Suporte de modelo do PIg
* Modelos de Storm para serviços do Azure

#### <a name="breaking-changes"></a>Alterações interruptivas
* Antigo **Storm** projeto tem de ser atualizado quando utilizar esta versão das ferramentas. Para obter mais informações, consulte [este blog](https://go.microsoft.com/fwlink/?LinkId=619108).
* Já não é suportado para o Visual Studio Web Express. Para obter mais informações, consulte [este blog](https://go.microsoft.com/fwlink/?LinkId=619108).

### <a name="azure-app-service-tools"></a>Ferramentas de serviço de aplicações do Azure
Nesta versão, as seguintes atualizações foram feitas para extensões de ferramentas da Web. Para obter mais informações, consulte [isso](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) blog. 

* Suporte para contas do DreamSpark adicionado
* Alteração completa para o Azure Tools feitas para suportar as novas APIs de gestão de recursos do Azure
* Adicionado suporte para o serviço de aplicações do Azure para [Cloud Explorer](#cloud_explorer)

#### <a name="known-issues"></a>Problemas conhecidos
Nós de ranhura de implementação de aplicação Web não são apresentados sob o nó de ranhuras no Explorador de servidores, e nós de subordinados de ranhura de implementação de aplicação Web não carregam no Cloud Explorer. Este problema foi resolvido e preparado para a próxima versão do SDK. 

### <a name="cloud_explorer"></a>Cloud Explorer para Visual Studio 2015
Azure SDK 2.7 inclui o Cloud Explorer para Visual Studio 2015, que permite-lhe ver os recursos do Azure, Inspecione as respetivas propriedades e executar ações de chave do desenvolvedor do Visual Studio. 

Explorador da cloud suporta o seguinte:

* Vistas de grupo de recursos e o tipo de recurso dos seus recursos do Azure 
* Procurar recursos por nome (disponível na vista de tipo de recurso)
* Suporte para as subscrições e recursos que tenham a função de acesso baseado em controle (RBAC) aplicada 
* Painel de ação integrada que mostra as ações orientada para programadores específicas para recursos selecionados. Por exemplo: anexar o depurador remoto para máquinas virtuais criadas com a pilha do Resource Manager, ver os dados de diagnóstico para máquinas virtuais etc.
* Painel de propriedades integrada, que mostra as propriedades de voltada para desenvolvedores de mensagens em fila normalmente necessárias durante o desenvolvimento e teste 
* A mudança rápida da conta a utilizar ao enumerar recursos (utilizar o comando de definições na barra de ferramentas) 
* Filtragem de subscrições para utilizar ao enumerar recursos (utilizar o comando de definições na barra de ferramentas) 
* Ligações avançadas para o Portal do Azure para a gestão de recursos e grupos de recursos 

### <a name="azure-resource-manager-tools"></a>Ferramentas do Gestor de recursos do Azure
O Azure Resource Manager Tools foram atualizados para funcionar com o controlo de acesso baseado em ' (RBAC) da função e novos tipos de subscrição.  A capacidade de utilizar novas contas de armazenamento, além do armazenamento clássico, para armazenar artefactos durante a implementação está incluído com essas alterações.  

Se estiver usando um projeto do grupo de recursos do Azure de uma versão anterior do SDK com o SDK 2.7, é necessário um novo script de implementação para implementar uma nova conta de armazenamento a utilizar em vez de armazenamento clássicas.  Será solicitado antes de serem efetuadas alterações ao seu projeto para adicionar o novo script.  O script antigo será renomeado e terá de fazer manualmente quaisquer modificações para o novo script.

### <a name="storage-explorer-tools"></a>Ferramentas do Explorador de armazenamento
* Suporte para visualizar os Blobs de acréscimo. Obter mais informações em [nesta mensagem de blogue](https://blogs.msdn.com/b/windowsazurestorage/archive/2015/04/13/introducing-azure-storage-append-blob.aspx). 
* Suporte para a visualização de contas de armazenamento Premium através do Explorador de servidores. Explorador de servidores apresentará apenas blobs de páginas para contas de armazenamento premium, pois são o único tipo de suporte para contas de armazenamento premium.

### <a name="azure-data-factory-tools-for-visual-studio"></a>Ferramentas do Azure Data Factory para Visual Studio
Apresentando **ferramentas do Azure Data Factory** para o Visual Studio. Seguem-se as funcionalidades ativadas. Ver [este blog](https://go.microsoft.com/fwlink/?LinkId=617530) para obter mais informações.

* **Modelo com base em criação**: Select Case para utilização com base em modelos, os modelos de movimento de dados ou modelos de processamento de dados para implementar uma solução de integração de dados ponto a ponto e começar a experimentar rapidamente o Data Factory. 
* **Integração com o Explorador de soluções para criar e implementar entidades do Data Factory**: criar e implementar pipelines e as entidades relacionadas como projetos do Visual Studio. 
* **Integração com o Vista de diagrama para interação visual criação**: criar visualmente pipelines e conjuntos de dados com a ajuda da vista de diagrama. 
* **Integração com o Explorador de servidores para navegação e interação com entidades já implementadas**: tirar partido do Server Explorer para procurar já implementado fábricas de dados e entidades correspondentes. Importe uma fábrica de dados implementados ou qualquer entidade (Pipeline, o serviço ligado, conjuntos de dados) para o seu projeto. 
* **Edição de JSON com o intellisense avançados e validação de esquema**: configurar e editar documentos JSON das entidades do Data Factory com a validação de esquema e intellisense avançada com eficiência 
* **Publicação de ambiente multi**: publicar criados pipelines para desenvolvimento, teste ou ambiente de produção através da criação de ficheiros de configuração separadas para cada ambiente.
* **PIg, Hive e do .net com base em suporte ao processamento de dados**: suporte para o Pig e Hive Scripts no projeto do Data Factory. Suporte para a referenciar C# projeto para o gerenciamento de .net atividade.

## <a name="azure-sdk-for-net-271"></a>Azure SDK para .NET 2.7.1
A secção seguinte contém as atualizações que foram introduzidas com o Azure SDK para .NET 2.7.1 de versão.

### <a name="hdinsight-tools"></a>Ferramentas do HDInsight
Para obter mais explicações sobre atualizações de ferramentas do HDInsight, consulte [este blog](https://go.microsoft.com/fwlink/?LinkId=623831).

* Vista de operador de tarefas do Hive (um novo recurso)
  
    Melhor, para ajudar a compreender a sua consulta do Hive foi adicionada a funcionalidade de vista de operador do Hive. Para ver todos os operadores dentro de um vértice, faça duplo clique nos vértices do gráfico da tarefa. Para ver mais detalhes de um operador específico, passe o rato sobre esse operador.
* Marcador de erro do Hive (um novo recurso)
  
    De forma instantânea, para que possa ver os erros de gramática foi adicionada a funcionalidade de marcador de erro do Hive. Além disso, as mensagens de erro, foram aprimoradas e agora pode ver os erros de gramática detalhadas instantaneamente (até nesta versão, era necessário que submeter um script de ramo de registo para o cluster e aguarde algum tempo antes de obter detalhes sobre a sua mensagem de erro).  
* Gráfico de topologia do Storm (um novo recurso)
  
    Visualizar é muito importante quando deseja ver se a topologia está a funcionar conforme esperado. Nesta versão, adicionámos visualização para gráficos de Storm. É possível visualizar as métricas importantes para a sua topologia (por exemplo, uma cor indica Meteorologia um determinado Bolt é "ocupada" ou não). Pode também faça duplo clique Spout/Bolt para ver mais detalhes.
* Suporte para clusters do HDInsight que foram criadas no Portal do Azure (uma correção de erro)
  
    Agora, pode utilizar o Visual Studio para ver e submeter tarefas para todos os seus clusters do HDInsight, não importa em que foram criados no cluster.
* Mais suporte do IntelliSense e metadados do Hive mais rápido carregar o (uma melhoria)
  
    Melhorámos o IntelliSense, adicionando mais sugestões de amigável de utilizador. Por exemplo, o alias de tabela pode agora também sugerido no IntelliSense para que pode escrever a consulta mais facilmente. Além disso, melhorámos os metadados do Hive a carregar para que apenas irá demorar vários segundos para listar todas as bases de dados, tabelas e colunas de sua metastore de Hive.

Para obter mais explicações sobre atualizações de ferramentas do HDInsight, consulte [este blog](https://go.microsoft.com/fwlink/?LinkId=623831).

### <a name="improvements-in-visual-studio-2013"></a>Aprimoramentos no Visual Studio 2013
* Azure SDK 2.7.1 permite que o Visual Studio 2013 para aceder a contas do Azure e subscrições através do controlo de acesso baseado em funções, fornecedores de soluções Cloud e do Dreamspark.
* Com o Azure SDK 2.7.1, a nova janela de ferramenta do Cloud Explorer agora também está disponível no Visual Studio 2013.

### <a name="known-issues"></a>Problemas conhecidos
Instalar o Azure SDK 2.6 ou 2.7.1 para Visual Studio Community 2013 num não - em inglês SO, será apresentado um aviso de que os recursos em inglês e não o inglês do Visual Studio podem ser sem correspondência. Este aviso pode ser dispensado em segurança. Só ocorrerá se a máquina não continha uma instalação anterior do Visual Studio Community 2013 e estiver a instalar o SDK num não - em inglês sistema operacional. Depois do pacote de idiomas aplica-se os recursos do RTM para o Visual Studio, mas antes de aplicar a atualização 4, é apresentado o aviso. A dispensar o aviso permitirá que o pacote de idiomas continuar e concluir a aplicar a versão de atualização 4 do conteúdo do pacote de idioma.

Projetos do LightSwitch não são compatibile com esta versão. Este problema será resolvido com a próxima versão do SDK.

## <a name="also-see"></a>Consulte também
[Azure SDK 2.7.1 mensagem de anúncio](https://go.microsoft.com/fwlink/?LinkId=623850)

[Mensagem de anúncio do Azure SDK 2.7](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/)

[Suporte e informações de reforma para o Azure SDK para .NET e APIs](https://msdn.microsoft.com/library/azure/dn479282.aspx/)

