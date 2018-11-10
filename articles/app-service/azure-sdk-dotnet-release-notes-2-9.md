---
title: Notas de versão do Azure SDK para .NET 2.9
description: Notas de versão do Azure SDK para .NET 2.9
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: ''
ms.assetid: c83d815b-fc19-4260-821e-7d2a7206dffc
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 04ee2daaf7b06f8e7bdd8de144a039474551ea11
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51227047"
---
# <a name="azure-sdk-for-net-29-release-notes"></a>Notas de versão do SDK do Azure para .NET 2.9

Este tópico inclui as notas de versão para versões 2.9 e 2.9.6 do Azure SDK para .NET.

## <a name="azure-sdk-for-net-296-release-summary"></a>Resumo de versão do SDK do Azure para .NET 2.9.6

Data de lançamento: 11/16/2016
 
Sem alterações de última hora para o Azure SDK 2.9 foram introduzidas nesta versão. Também não é nenhum processo de atualização necessário para aproveitar este SDK com os projetos existentes do serviço em nuvem.

### <a name="visual-studio-2017-release-candidate"></a>Visual Studio 2017 versão Release Candidate

- No Visual Studio 2017 RC, esta versão do SDK do Azure para .NET é interno para a carga de trabalho do Azure. Todas as ferramentas que precisa fazer o desenvolvimento do Azure vão fazer parte do Visual Studio 2017 RC daqui em diante. Para Visual Studio 2015 e o Visual Studio 2013, o SDK continuará disponível através de WebPI. Podemos irá ser descontinuar SDK do Azure para versões do .NET para Visual Studio 2013, quando o Visual Studio 2017 versões como um produto final. Siga esta ligação para transferir o Visual Studio 2017 RC: https://www.visualstudio.com/vs/visual-studio-2017-rc/

### <a name="azure-diagnostics"></a>Diagnóstico do Azure

- Alterar o comportamento para armazenar apenas uma cadeia de ligação parcial com a chave substituída por um token de cadeia de ligação de armazenamento de diagnósticos de serviços Cloud. A chave de armazenamento real está agora armazenada na pasta de perfil do usuário para que o acesso pode ser controlado. Visual Studio irá ler a chave de armazenamento da pasta de perfil do usuário para depuração local e o processo de publicação. 
- Em resposta à alteração descrita acima, a equipe Visual Studio Online avançado o modelo de tarefa de implementação de serviços Cloud do Azure para que os utilizadores podem especificar a chave de armazenamento para a configuração de extensão de diagnóstico durante a publicação para o Azure na integração contínua e Implementação.
- Foram efetuadas possível armazenar a cadeia de ligação segura e atomização de frases para Azure Diagnostics (WAD), para ajudar a resolver problemas de configuração em environements.
 
### <a name="windows-server-2016-virtual-machines"></a>Máquinas virtuais do Windows Server 2016

- Visual Studio agora suporta a implementação de serviços em nuvem de máquinas de virtuais de 5 de família do SO (Windows Server 2016). Para serviços cloud existentes, pode alterar as definições para a nova família de SO de destino. Ao criar novos serviços em nuvem, se optar por criar o serviço usando o .net 4.6 ou superior, a predefinição será o serviço para utilizar a 5 de família de SO.  Para obter mais informações, pode rever o [tabela de suportar a família de SO convidado](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/).

#### <a name="known-issues"></a>Problemas conhecidos

- SDK de .NET do Azure 2.9.6 introduziu uma restrição que bloqueia a implementação de projetos usando estruturas de .NET não suportadas (por exemplo, o .NET 4.6) qualquer membro da família SO < 5. Uma solução alternativa é fornecida [aqui](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9).

 
### <a name="azure-in-role-cache"></a>Cache na função do Azure 

- Suporte para a Cache na função do Azure termina em 30 de Novembro de 2016. Para obter mais detalhes, clique em [aqui](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).

### <a name="azure-resource-manager-templates-for-azure-stack"></a>Modelos do Azure Resource Manager para o Azure Stack

- Introduzimos Azure Stack como destino de implantação para modelos do Azure Resource Manager.


## <a name="azure-sdk-for-net-29-summary"></a>Azure SDK para .NET 2.9 resumo

## <a name="overview"></a>Descrição geral
Este documento contém as notas de versão para o Azure SDK para .NET 2.9 versão. 

Para obter informações detalhadas sobre as atualizações nesta versão, consulte a [mensagem de anúncio do Azure SDK 2.9](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).

## <a name="azure-sdk-29-for-visual-studio-2015-update-2-and-visual-studio-15-preview"></a>Azure SDK 2.9 para Visual Studio 2015 Update 2 e o Visual Studio "15" pré-visualização
Esta atualização inclui as correções seguintes:

* Problema relacionado com a geração de cliente de API de REST em que a cadeia de caracteres "Tipo desconhecido" aparece como o nome da pasta de geração de código e/ou o nome do espaço de nomes largadas no código gerado.
* Problema relacionado com WebJobs agendados em que as informações de autenticação não conseguiu ser passada para o agendador de processo de aprovisionamento.

Esta atualização inclui a nova funcionalidade seguinte:

* Suporte para serviços de aplicação secundário no separador "Serviços" da caixa de diálogo de aprovisionamento de serviço de aplicações. 

## <a name="azure-data-lake-tools-for-visual-studio-2015-update-2"></a>O Azure Data Lake Tools para Visual Studio 2015 Update 2
Esta atualização inclui o seguinte:

* **O Azure Data Lake Tools** para Visual Studio agora é intercalado com o Azure SDK para a versão do .NET. A ferramenta é instalada automaticamente ao instalar o SDK do Azure. 
  
    A ferramenta é atualizada frequentemente, aceda [aqui](https://aka.ms/datalaketool) para obter as atualizações.
* **Explorador de servidores** agora permite-lhe ver tudo e criar algumas entidades de metadados de U-SQL. Para obter mais informações, consulte [isso](https://azure.microsoft.com/documentation/services/data-lake-analytics/) blog.

## <a name="hdinsight-tools"></a>Ferramentas do HDInsight
**Ferramentas do HDInsight** para o Visual Studio agora suporta correções de HDInsight versão 3.3, incluindo a exibição de gráficos Tez e outra linguagem.

## <a name="azure-resource-manager"></a>Azure Resource Manager
Esta versão adiciona [KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) suporte para modelos do Resource Manager.

## <a name="see-also"></a>Consulte também
[Mensagem de anúncio de SDK 2.9 do Azure](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)

