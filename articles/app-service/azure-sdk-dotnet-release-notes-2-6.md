---
title: Notas de versão do Azure SDK para .NET 2.6
description: Notas de versão do Azure SDK para .NET 2.6
services: app-service/web
documentationcenter: .net
author: chrissfanos
editor: ''
ms.assetid: b45853d5-a2b8-4962-a22d-579cb36ae14c
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 040eea1efd3abdbbfa65c24b2950ebece9d4acbc
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51258396"
---
# <a name="azure-sdk-for-net-26-release-notes"></a>Notas de versão do Azure SDK para .NET 2.6
Este documento contém as notas de versão para o Azure SDK para .NET 2.6 versão. 

Com o Azure SDK 2.6, pode desenvolver aplicativos de serviço cloud (PaaS) destinado a .NET 4.5.2 ou .NET 4.6, desde que instale manualmente o .NET Framework de destino na função do serviço de nuvem. Ver [instalar o .NET numa função de serviço Cloud](https://go.microsoft.com/fwlink/?LinkID=309796).

## <a name="service-bus-updates"></a>Atualizações do Service Bus
* Hubs de eventos: 
  
  * Agora permite que o controlo de acesso de destino quando o envio de eventos, expondo o ponto final de publicador adicionais dos Hubs de eventos.
  * Estabilidade adicional e o aprimoramento adicionada a funcionalidade de Hubs de eventos.
  * Adição de suporte do protocolo Amqp sobre WebSocket para mensagens e os Hubs de eventos.

## <a name="hdinsight-tools-for-visual-studio-updates"></a>Atualizações de ferramentas do HDInsight para Visual Studio
* **Aprimoramento do IntelliSense**: sugestões de metadados remotos
  
    Ferramentas do HDInsight para Visual Studio suporta agora a obter metadados remotos durante a edição do script do Hive. Por exemplo, pode digitar **SELECIONAR * FROM** e serão apresentados todos os nomes de tabela. Além disso, os nomes das colunas serão apresentadas depois de especificar uma tabela.
* **Suporte de emulador do HDInsight**
  
    Ferramentas do HDInsight para Visual Studio suportam agora a ligar ao HDInsight emulator, portanto, pode desenvolver seus scripts de Hive localmente sem introduzir quaisquer custos, em seguida, executar esses scripts em relação a seus clusters do HDInsight. 
  
    Para obter mais informações, consulte [deste manual](https://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).
* **Ferramentas do HDInsight para Visual Studio de suporte para clusters do Hadoop genéricos** (pré-visualização)
  
    Ferramentas do HDInsight para Visual Studio suportam agora clusters do Hadoop genéricos, pelo que pode utilizar ferramentas do HDInsight para Visual Studio para fazer o seguinte:
  
  * ligar ao cluster, 
  * escrever a consulta do Hive com o suporte de IntelliSense/autocompletar avançado, 
  * ver todas as tarefas no seu cluster com uma interface do Usuário intuitiva. 
    
    Para obter mais informações, consulte [deste manual](https://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).

## <a name="in-role-cache-updates"></a>Atualizações da Cache na função
* **Cache na função** foi atualizado para usar **SDK de armazenamento do Microsoft Azure** versão 4.3. Até agora, o **Cache na função** estava a utilizar o SDK de armazenamento do Azure versão 1.7.
  
    Os clientes a utilizar o Azure SDK 2.5 ou abaixo deve atualizar para o Azure SDK 2.6 e mover para a nova versão do SDK de armazenamento do Azure. 
  
    Neste momento o armazenamento do Azure versão 2011-08-18 está agendado para ser removido a 1 de Agosto de 2016. Qualquer migrações de Cache na função do Azure SDK 2.5 ou abaixo para 2.6 tem de ser concluídas neste momento. Para obter mais informações sobre a desativação de armazenamento do Azure versão 2011-08-18, consulte [os atualização do remoção de versão do serviço de armazenamento do Microsoft Azure: extensão para o 2016](https://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx).

> [!IMPORTANT]
> Estamos a anunciar a desativação de 30 de Novembro de 2016, para o serviço de Cache gerida do Azure e Cache na função do Azure. Recomendamos que migre para a Cache de Redis do Azure em preparação para esta extinção. Para obter mais informações sobre as datas e orientações de migração, consulte [ofertas da Cache de Azure que é mais adequada para mim?](../redis-cache/cache-faq.md#which-azure-cache-offering-is-right-for-me)
> 
> 

## <a name="azure-app-service-tools"></a>Ferramentas de serviço de aplicações do Azure
Os seguintes itens foram atualizados na versão do Azure SDK 2.6.

* Publicação no Azure melhorada para incluir aplicações de API do Azure como destino de implantação.
* Aplicações API funcionalidade de aprovisionamento para permitir que os utilizadores com a funcionalidade de criação e da configuração da aplicação API.
* Explorador de servidores foi alterado para refletir o novo nó de serviço de aplicações, com as aplicações Web, móveis e API agrupadas por grupo de recursos.
* Adicionar o gesto de cliente de aplicação de API do Azure adicionado à maioria C# projetos que permitirão que a geração automática de aplicações API ativada no Swagger em execução na subscrição do Azure de um utilizador.
* As ferramentas de aplicações API e nós de serviço de aplicações no Explorador de servidores só estão disponíveis no Visual Studio 2013. 

## <a name="azure-resource-manager-tools-updates"></a>Atualizações de ferramentas do Gerenciador de recursos do Azure
As ferramentas do Gerenciador de recursos do Azure foram atualizadas para incluir os modelos de máquinas virtuais, rede e armazenamento. O JSON de experiência de edição foi atualizado para incluir uma nova vista de estrutura de tópicos para modelos e a capacidade de editar os modelos com fragmentos JSON. Modelos implantados a partir do Visual Studio, utilize um script do PowerShell fornecido com o projeto, para que todas as alterações efetuadas ao script serão utilizadas pelo Visual Studio.

## <a name="diagnostics-improvements-for-cloud-services"></a>Melhorias de diagnóstico para os serviços Cloud
Azure SDK 2.6 recupera o suporte para recolher registos de diagnóstico no emulador de computação do Azure e transferir para o armazenamento de desenvolvimento. Registos do qualquer diagnóstico (incluindo registos, rastreamento de eventos para os registos do Windows (ETW), contadores de desempenho, infraestrutura registos de eventos do windows e registos de rastreio de aplicação) gerado quando a aplicação está em execução no emulador pode ser transferido para armazenamento de desenvolvimento Para verificar se o registo de diagnóstico está a funcionar no seu computador local. 

A conta de armazenamento de diagnóstico agora pode ser especificada no ficheiro de configuração (. cscfg) do serviço, tornando mais fácil de usar contas de armazenamento de diagnóstico diferentes para diferentes ambientes. Existem algumas diferenças notáveis entre como a cadeia de ligação trabalhou no Azure SDK 2.4 e o Azure SDK 2.6. Para obter mais informações sobre como utilizar a cadeia de ligação de armazenamento de diagnóstico e como ele afeta seus projetos, consulte [diagnóstico de configuração de serviços Cloud do Azure](https://go.microsoft.com/fwlink/?LinkID=532784).

## <a name="breaking-changes"></a>Alterações interruptivas
### <a name="azure-resource-manager-tools"></a>Ferramentas do Gestor de recursos do Azure
* O **projetos de implantação de nuvem** foi alterado para tipo de projeto disponível no Azure SDK 2.5 **grupo de recursos do Azure**.
* **Projetos de implantação de nuvem** tipo de projetos criados no Azure SDK 2.5 pode ser usado em 2.6, mas a implementar o modelo do Visual Studio irá falhar. No entanto, a implantação com o script do PowerShell ainda funcionará como fazia anteriormente.  Para obter informações sobre como usar **projetos de implantação de nuvem** no 2.6 leia esta [publicar](https://go.microsoft.com/fwlink/?LinkID=534086).

## <a name="known-issues"></a>Problemas conhecidos
* Recolher registos de diagnóstico no emulador requer um sistema operativo de 64 bits. Os registos de diagnóstico não serão recolhidos quando em execução num sistema operativo de 32 bits. Isto não afeta quaisquer outras funcionalidades do emulador. 
* Azure SDK 2.6, lançada em 4/29/2015 tinha dois problemas: 
  
  * Não foi possível carregar a aplicação universal no Visual Studio 2015 quando o Azure SDK 2.6 foi instalado na máquina.
  * Depuração de um projeto de serviço em nuvem falharia no Visual Studio 2013 e Visual Studio 2015, em que o Visual Studio deixar de responder e falhas ao apresentar uma caixa de diálogo com a mensagem "Configurar o diagnóstico para o emulador".
    
    Uma atualização para o Azure SDK 2.6 foi lançada em 18/5/2015. A versão atualizada é 2.6.30508.1601; ela contém correções para problemas de duas descritos acima. Pode identificar a compilação do SDK no painel de controlo -> programas e funcionalidades -> Ferramentas do Microsoft Azure para Microsoft Visual Studio 2013 – v 2.6. A coluna de versão irá apresentar o número de compilação.
    
    Se ainda está a enfrentar problemas acima, instale a versão mais recente do SDK 2.6 do Azure, para [VS 2012](https://go.microsoft.com/fwlink/p/?linkid=323511&clcid=0x409), [VS 2013](https://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) ou [VS 2015](https://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409).

## <a name="see-also"></a>Consultar Também
[Suporte e informações de reforma para o Azure SDK para .NET e APIs](https://msdn.microsoft.com/library/azure/dn479282.aspx/)

