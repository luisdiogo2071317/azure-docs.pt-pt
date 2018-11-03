---
title: Com o Visual Studio publicar Assistente da aplicação do Azure | Documentos da Microsoft
description: Saiba como configurar as várias configurações no Visual Studio publicar Assistente da aplicação Azure
services: visual-studio-online
author: ghogen
manager: douge
assetId: 7d8f1ac9-e439-47e0-a183-0642c4ea1920
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/21/2017
ms.author: ghogen
ms.openlocfilehash: 19932446193c6f36aedc8907fd59c2377a2663eb
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969378"
---
# <a name="using-the-visual-studio-publish-azure-application-wizard"></a>Com o Visual Studio publicar Assistente da aplicação do Azure

Depois de desenvolver uma aplicação web no Visual Studio, pode publicar essa aplicação para um serviço cloud do Azure utilizando o **publicar a aplicação do Azure** assistente.

> [!Note]
> Este artigo é sobre a implementação de serviços cloud, não a sites da web. Para obter informações sobre a implementação em web sites, consulte [como implementar um Web Site do Azure](https://social.msdn.microsoft.com/Search/windowsazure?query=How%20to%20Deploy%20an%20Azure%20Web%20Site&Refinement=138&ac=4#refinementChanges=117&pageNumber=1&showMore=false).

## <a name="accessing-the-publish-azure-application-wizard"></a>Acessando o Assistente para publicar a aplicação do Azure

Pode acessar o Assistente de publicar a aplicação do Azure de duas formas, consoante o tipo de projeto do Visual Studio que tem.

**Se tiver um projeto do serviço cloud do Azure:**

1. Crie ou abra um projeto do serviço cloud do Azure no Visual Studio.

1. Na **Explorador de soluções**, clique com o botão direito no projeto e, no menu de contexto, selecione **Publish**.

**Se tiver um projeto de aplicativo web que não está ativado para o Azure:**

1. Crie ou abra um projeto do serviço cloud do Azure no Visual Studio.

1. Na **Explorador de soluções**, clique com o botão direito no projeto e, no menu de contexto, selecione **converter** > **converter para o projeto de serviço de nuvem do Azure**. 

1. Na **Explorador de soluções**, com o botão direito no projeto do Azure recém-criado e, no menu de contexto, selecione **Publish**.

## <a name="sign-in-page"></a>Página de início de sessão

![Página de início de sessão](./media/vs-azure-tools-publish-azure-application-wizard/sign-in.png)

**Conta** - Selecione uma conta ou selecione **adicionar uma conta** na lista pendente.

**Escolha a sua subscrição** -escolher a subscrição a utilizar para a sua implementação.

## <a name="settings-page---common-settings-tab"></a>Página de definições - guia configurações comuns

![Definições Comuns](./media/vs-azure-tools-publish-azure-application-wizard/settings-common-settings.png)

**Serviço em nuvem** -com a lista pendente, selecione uma cloud existente, serviço ou selecione  **&lt;criar nova >** e criar um serviço cloud. Apresenta o Centro de dados entre parêntesis, para cada serviço em nuvem. Recomenda-se que o Centro de dados local para o serviço cloud sejam os mesmos que a localização do Datacenter para a conta de armazenamento (definições avançadas).

**Ambiente** -selecione **produção** ou **teste**. Escolha o ambiente de teste, se pretender implementar a sua aplicação num ambiente de teste. 

**Konfigurace buildu** -selecione **depurar** ou **versão**.

**Configuração do serviço** -selecione **Cloud** ou **Local**.

**Ativar o ambiente de trabalho remoto para todas as funções** -Selecione esta opção se pretender ser capaz de ligar remotamente ao serviço. Esta opção é usada principalmente para resolução de problemas. Para obter mais informações, consulte [ativar a ligação de ambiente de trabalho remoto para uma função nos serviços Cloud do Azure com o Visual Studio](cloud-services/cloud-services-role-enable-remote-desktop-visual-studio.md).

**Ativar o Web Deploy para todas as funções de web** -Selecione esta opção para ativar a implementação do serviço web. Também tem de selecionar o **ativar o ambiente de trabalho remoto para todas as funções** opção para utilizar esta funcionalidade. Para obter mais informações, consulte [publicação de um serviço cloud com o Visual Studio](vs-azure-tools-publishing-a-cloud-service.md).

## <a name="settings-page---advanced-settings-tab"></a>Página de definições - definições guia avançada

![Definições avançadas](./media/vs-azure-tools-publish-azure-application-wizard/settings-advanced-settings.png)

**Etiqueta de implementação** -aceite o nome predefinido ou introduza um nome à sua escolha. Para acrescentar a data para a etiqueta de implementação, deixe a caixa de verificação selecionada. 

**Conta de armazenamento** -selecione a conta de armazenamento a utilizar para esta implementação, * *&lt;criar novo > para criar uma conta de armazenamento. Apresenta o Centro de dados entre parêntesis, para cada conta de armazenamento. Recomenda-se que a localização do Datacenter para a conta de armazenamento é o mesmo que a localização do Datacenter para o serviço cloud (configurações comuns).

A conta de armazenamento do Azure armazena o pacote para a implementação da aplicação. Depois da aplicação é implementada, o pacote for removido da conta de armazenamento.

**Eliminar implementação em caso de falha** -Selecione esta opção para que a implementação eliminada se for encontrado qualquer erro durante a publicação. Deve estar desmarcada se pretender manter um endereço IP virtual constante para o seu serviço cloud.

**Atualização da implementação** -Selecione esta opção se pretender implementar apenas componentes atualizados. Este tipo de implementação pode ser mais rápido do que uma implementação completa. Isso deve ser verificado se pretender manter um endereço IP virtual constante para o seu serviço cloud. 

**Atualização da implementação - definições** -esta caixa de diálogo é utilizada para especificar a forma como pretende que as funções para ser atualizado. Se escolher **atualização Incremental**, cada instância da sua aplicação é atualizada, para que o aplicativo estará sempre disponível. Se escolher **atualização em simultâneo**, todas as instâncias da sua aplicação são atualizadas ao mesmo tempo. A atualização simultânea é mais rápido, mas o seu serviço pode não estar disponível durante o processo de atualização.

![Definições de implementação](./media/vs-azure-tools-publish-azure-application-wizard/deployment-settings.png)

**Ativar o IntelliTrace** -Especifique se pretende ativar o IntelliTrace. Com o IntelliTrace, pode iniciar sessão extensa informações de depuração para uma instância de função quando é executada no Azure. Se precisar de descobrir a causa de um problema, pode utilizar os registos do IntelliTrace para percorrer o código do Visual Studio como se estivesse em execução no Azure. Para obter mais informações sobre como utilizar o IntelliTrace, consulte [depurar um serviço cloud do Azure publicada no Visual Studio e IntelliTrace](./vs-azure-tools-intellitrace-debug-published-cloud-services.md).

**Ativar a criação de perfis** -Especifique se pretende ativar a criação de perfis de desempenho. O criador de perfil do Visual Studio permite-lhe obter uma análise aprofundada dos aspectos de computacionais do como o seu serviço cloud é executado. Para obter mais informações sobre como usar o criador de perfil do Visual Studio, consulte [testar o desempenho de um serviço cloud do Azure](./vs-azure-tools-performance-profiling-cloud-services.md).

**Ativar o depurador remoto para todas as funções** -Especifique se pretende ativar a depuração remota. Para obter mais informações sobre a depuração de serviços em nuvem com o Visual Studio, consulte [depurar um serviço cloud do Azure ou numa máquina virtual no Visual Studio](./vs-azure-tools-debug-cloud-services-virtual-machines.md).

## <a name="diagnostics-settings-page"></a>Página de definições de diagnóstico

![Definições de diagnósticos](./media/vs-azure-tools-publish-azure-application-wizard/diagnostic-settings.png)

Diagnóstico permite-lhe resolver problemas de um serviço cloud do Azure (ou máquina virtual do Azure). Para obter informações sobre o diagnóstico, consulte [configurar diagnósticos para serviços Cloud do Azure e máquinas virtuais](./vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md). Para obter informações sobre o Application Insights, veja [o que é o Application Insights?](./application-insights/app-insights-overview.md).

## <a name="summary-page"></a>Página de resumo

![Resumo](./media/vs-azure-tools-publish-azure-application-wizard/summary.png)

**Perfil de destino** -pode optar por criar um perfil de publicação a partir das definições que escolheu. Por exemplo, poderá criar um perfil para um ambiente de teste e outra para produção. Para guardar este perfil, escolha o **guardar** ícone. O assistente cria o perfil e guarda-o no projeto do Visual Studio. Para modificar o nome do perfil, abra a **perfil de destino** lista e, em seguida, escolha  **&lt;gerir... &gt;**.

   > [!Note]
   > O perfil de publicação é apresentada no Explorador de soluções no Visual Studio e as definições de perfil são escritas num ficheiro com uma extensão de .azurePubxml. As definições são guardadas como atributos de marcas XML.

## <a name="publishing-your-application"></a>Publicação da sua aplicação

Depois de configurar todas as definições de implementação do seu projeto, selecione **publicar** na parte inferior da caixa de diálogo. Pode monitorizar o estado do processo no **saída** janela no Visual Studio.

## <a name="next-steps"></a>Passos Seguintes

- [Migrar e publicar uma aplicação Web num serviço cloud do Azure a partir do Visual Studio](./vs-azure-tools-migrate-publish-web-app-to-cloud-service.md)

- [Saiba como utilizar o Visual Studio para publicar um serviço cloud do Azure](./vs-azure-tools-publishing-a-cloud-service.md)

- [Depurar um serviço cloud do Azure publicada no Visual Studio e IntelliTrace](./vs-azure-tools-intellitrace-debug-published-cloud-services.md)

- [Testar o desempenho de um serviço cloud do Azure](./vs-azure-tools-performance-profiling-cloud-services.md)

- [Configurar os diagnósticos para serviços Cloud do Azure e máquinas virtuais](./vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).

- [O que é o Application Insights?](./application-insights/app-insights-overview.md)