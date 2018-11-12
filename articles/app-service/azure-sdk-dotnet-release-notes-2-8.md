---
title: Notas de versão do Azure SDK para .NET 2.8
description: Notas de versão do Azure SDK para .NET 2.8
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: ''
ms.assetid: de7207ff-ba4f-4008-9141-8742fcaa3254
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 6aa2684a900dffecd481d51b8876b0e674c1a6ea
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51235535"
---
# <a name="azure-sdk-for-net-28-281-and-282"></a>Azure SDK para .NET 2.8, 2.8.1 e 2.8.2
## <a name="overview"></a>Descrição geral
Este artigo contém as notas de versão (que inclui os problemas conhecidos e alterações de última hora) para o Azure SDK para .NET 2.8, 2.8.1 e 2.8.2 versões. 

Para obter a lista completa dos novos recursos e atualizações feitas nesta versão, consulte a [do Azure SDK 2.8 para Visual Studio 2013 e Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/) anúncio. 

## <a name="azure-sdk-for-net-28"></a>Azure SDK para NET 2.8
### <a name="download-azure-sdk-for-net-28"></a>Transfira o Azure SDK para .NET 2.8
[Azure SDK para .NET 2.8 para Visual Studio 2015](https://go.microsoft.com/fwlink/?LinkId=699285) 

[Azure SDK para .NET 2.8 para Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=699287)

### <a name="net-452-support"></a>Suporte a .NET 4.5.2
#### <a name="known-issues"></a>Problemas conhecidos
O Azure .NET SDK 2.8 permite-lhe criar a .NET 4.5.2 pacotes de serviço em nuvem. No entanto 4.5.2 do .NET framework não será instalado no padrão de versão imagens de SO convidado até Janeiro de 2016 SO convidado. Antes dessa, 4.5.2 do .NET framework estará disponível por meio de uma versão de lançamento de SO convidado separada – Novembro de 2015-02. Consulte a [versões do SO convidado do Azure e matriz de compatibilidade de SDK](../cloud-services/cloud-services-guestos-update-matrix.md) página para controlar quando a imagem será lançada.  Quando a imagem do Novembro de 2015-02 é lançada pode optar por utilizar essa imagem ao atualizar o ficheiro de arquivo (. cscfg) de configuração do serviço em nuvem. Na configuração do serviço ficheiro definir o atributo osVersion do elemento ServiceConfiguration para a cadeia "WA-convidado-SO-4.26_201511-02". Se optar por opte por utilizar esta imagem, em seguida, já não irá obter as atualizações automáticas para o SO convidado. Para obter as atualizações automáticas a versão do SO tem de ser definido "*" e a .NET 4.5.2 só estará disponível por meio de atualizações automáticas em Janeiro de 2016.

### <a name="azure-data-factory"></a>Azure Data Factory
#### <a name="known-issues"></a>Problemas conhecidos
Durante uma **modelo de fábrica de dados** projeto criação que envolvem o dados de exemplo, o script do azure power shell poderão falhar se do azure power shell versão instalado na máquina é após a 0.9.8.

Para este tipo de projeto foi criada com êxito, tem de instalar [versão do azure power shell 0.9.8](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi).

### <a name="azure-resource-manager-tools"></a>Ferramentas do Gestor de recursos do Azure
#### <a name="breaking-changes"></a>Alterações interruptivas
O script do PowerShell fornecido pelo projeto do grupo de recursos do Azure foi atualizado nesta versão para trabalhar com os novos cmdlets do Azure PowerShell versão 1.0.  Este novo script não funcionará de com o Visual Studio quando utiliza uma versão do SDK antes de 2.8.  

Scripts de projetos criados em versões anteriores do SDK não serão executado de dentro do Visual Studio ao utilizar o SDK 2.8.  Todos os scripts irão continuar a trabalhar fora do Visual Studio com a versão adequada de cmdlets do Azure PowerShell.  

O SDK 2.8 requer a versão 1.0 dos cmdlets do PowerShell do Azure.  Todas as outras versões do SDK é necessária a versão 0.9.8 dos cmdlets do PowerShell do Azure.  Para obter mais informações, consulte [isso](https://go.microsoft.com/fwlink/?LinkID=623011) blog.

### <a name="web-tools-extensions"></a>Extensões de ferramentas de Web
#### <a name="known-issues"></a>Problemas conhecidos
Os seguintes problemas conhecidos serão resolvidos na versão seguinte.

* Serviço de aplicações relacionadas com a nuvem e do Explorador de servidores de gesto para ambientes de não produção (como o Azure China ou os clientes do Azure Stack) não funcionam. Para os clientes nessas áreas afetadas, transferir o perfil de publicação do portal do Azure irá permitir a capacidade de publicação. Uma versão futura irá reparar gestos, como "Anexar o depurador" e "Ver registos de transmissão em fluxo" para o Azure China e clientes de pilha. 
* Os clientes poderão ver erros durante a criação, quando o App Insights instância para que eles estão a implementar está numa região diferente dos EUA leste de serviço de aplicações. Nestes cenários, a criação de um serviço de aplicações no portal e transferir o perfil de publicação, irão ativar cenários de publicação. 

### <a name="azure-hdinsight-tools"></a>Ferramentas do Azure HDInsight
#### <a name="new-updates"></a>Novas atualizações
* Pode executar sua consulta do Hive no cluster através de HiveServer2 quase sem sobrecarga e ver que a tarefa de registos em tempo real.
* Usando o novo ramo de registo Vista execução de tarefas pode aprofundar seu trabalho mais aprofundado, veja mais detalhes e identificar potenciais problemas.

Para obter informações, consulte [do Azure SDK 2.8 para Visual Studio 2013 e Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/). 

## <a name="azure-sdk-for-net-281"></a>Azure SDK para .NET 2.8.1
### <a name="known-issues-for-visual-studio-2013-and-visual-studio-2015"></a>Problemas conhecidos do Visual Studio 2013 e Visual Studio 2015
1. WebJob acionada publica para ranhuras irão mostrar e o erro e não uma agenda do conjunto, mas ele envia por push o WebJob para o Azure. Os clientes que precisam de uma tarefa agendada, em seguida, podem utilizar o Portal do Azure para configurar a agenda para o trabalho Web. 
2. Os clientes de Python podem ter problemas de depurador. Equipa do serviço está a implementar uma correção para isso, mas se os clientes estão afetados, entre em contacto Microsoft saber nos fóruns ou no blog do anúncio ou secção de comentários de notas de versão. 
3. Os clientes em determinadas regiões (por exemplo, Sul da Índia) irão ocorrer erros de aprovisionamento de serviço de aplicações. Este procedimento é consistente com o portal e os clientes que ter este problema podem utilizar o portal do Azure para pedir acesso para publicar a estas regiões da geo. Assim que eles solicitar acesso a estas regiões através do aprovisionamento portal do Azure deve funcionar. 

## <a name="azure-sdk-for-net-282"></a>Azure SDK para .NET 2.8.2
Após a instalação do 2.8.2 ferramentas, os clientes podem ter o seguinte problema.         

* Se estiver a utilizar o Windows 10 e não tiver instalado o Internet Explorer, poderá receber um erro de "Internet Explorer não foi possível encontrar".
  Para resolver o problema, instale o Internet Explorer usando a caixa de diálogo Adicionar/remover componentes do Windows.

Se observar este problema, utilize a funcionalidade de enviar um sorriso para relatá-lo.

Para obter mais informações, consulte [isso](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-2-for-net/) publicar.

## <a name="other-updates"></a>Outras atualizações
Para outras atualizações, consulte [mensagem de anúncio do Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

## <a name="also-see"></a>Consulte também
[Mensagem de anúncio do Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)

[Suporte e informações de reforma para o Azure SDK para .NET e APIs](https://msdn.microsoft.com/library/azure/dn479282.aspx)

