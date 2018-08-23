---
title: Depurar um publicado um Azure serviço em nuvem com o Visual Studio e IntelliTrace | Documentos da Microsoft
description: Saiba como depurar um serviço cloud com o Visual Studio e IntelliTrace
services: visual-studio-online
documentationcenter: n/a
author: mikejo
manager: douge
editor: ''
ms.assetid: 5e6662fc-b917-43ea-bf2b-4f2fc3d213dc
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.custom: vs-azure
ms.workload: azure-vs
ms.date: 03/21/2017
ms.author: mikejo
ms.openlocfilehash: 1deec539d3dfe65a2ac02a9f4cd2b94d9a78274a
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/22/2018
ms.locfileid: "42444347"
---
# <a name="debugging-a-published-azure-cloud-service-with-visual-studio-and-intellitrace"></a>Depurar um serviço cloud do Azure publicada no Visual Studio e IntelliTrace
Com o IntelliTrace, pode iniciar sessão extensa informações de depuração para uma instância de função quando é executada no Azure. Se precisar de descobrir a causa de um problema, pode utilizar os registos do IntelliTrace para percorrer o código do Visual Studio como se estivesse em execução no Azure. Na verdade, registos do IntelliTrace principais execução de código e dados de ambiente, quando a aplicação do Azure está a ser executado como um serviço cloud no Azure e permite-lhe reproduzir os dados a partir do Visual Studio. 

Pode usar o IntelliTrace, se tiver o Visual Studio Enterprise, instalado e seus destinos de aplicação do Azure .NET Framework 4 ou uma versão posterior. IntelliTrace recolhe informações para as funções do Azure. As máquinas virtuais para estas funções sempre executadas sistemas operativos de 64 bits.

Como alternativa, pode utilizar [depuração remota](http://go.microsoft.com/fwlink/p/?LinkId=623041) anexar diretamente a um serviço cloud que está em execução no Azure.

> [!IMPORTANT]
> IntelliTrace destina-se apenas a cenários de depuração e não deve ser utilizado para uma implementação de produção.
> 

## <a name="configure-an-azure-application-for-intellitrace"></a>Configurar uma aplicação do Azure para o IntelliTrace
Para ativar o IntelliTrace para uma aplicação do Azure, tem de criar e publicar a aplicação a partir de um projeto do Visual Studio do Azure. Tem de configurar o IntelliTrace para a sua aplicação do Azure antes de o publicar no Azure. Se publicar seu aplicativo sem configurar o IntelliTrace, terá de voltar a publicar o projeto. Para obter mais informações, consulte [cloud do Azure de publicação dos serviços de projetos com o Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=623012).

1. Quando estiver pronto para implementar a aplicação do Azure, certifique-se de que os seus destinos de compilação do projeto estão definidos **depurar**.

1. Na **Explorador de soluções**, clique no projeto e, no menu de contexto, selecione **Publish**.
   
1. Na **publicar a aplicação do Azure** caixa de diálogo, selecione a subscrição do Azure e selecione **próxima**.

1. Na **configurações** página, selecione a **definições avançadas** separador.

1. Ativar a **ativar o IntelliTrace** opção para recolher registos do IntelliTrace para a sua aplicação quando é publicado na nuvem.
   
1. Para personalizar a configuração básica do IntelliTrace, selecione **configurações** junto a **ativar o IntelliTrace**.

    ![Ligação de definições de IntelliTrace](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/intellitrace-settings-link.png)
   
1. Na **Nastavení IntelliTrace** caixa de diálogo, pode especificar os eventos de registo, se a recolher informações sobre a chamada, quais módulos e processos para recolher registos para e quanto espaço para alocar para a gravação. Para obter mais informações sobre IntelliTrace, consulte [depuração com o IntelliTrace](http://go.microsoft.com/fwlink/?LinkId=214468).
   
    ![Nastavení IntelliTrace](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

O registo de IntelliTrace é um ficheiro de registo circular do tamanho máximo especificado nas definições de IntelliTrace (o tamanho predefinido é 250 MB). Registos do IntelliTrace são recolhidos para um ficheiro no sistema de ficheiros da máquina virtual. Quando solicita os registos, um instantâneo é direcionado nesse ponto no tempo e transferido para o computador local.

Depois do serviço cloud do Azure foi publicado para o Azure, pode determinar se IntelliTrace tenha sido ativado a partir do nó do Azure no **Explorador de servidores**, conforme mostrado na imagem seguinte:

![Explorador de servidores - IntelliTrace habilitado](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## <a name="download-intellitrace-logs-for-a-role-instance"></a>Transferir registos do IntelliTrace para uma instância de função
Com o Visual Studio, pode transferir registos do IntelliTrace para uma instância de função, seguindo estes passos:

1. Na **Explorador de servidores**, expanda o **serviços Cloud** nó e localize a instância de função cujos registos que pretende transferir. 

1. A instância de função com o botão direito e, no menu de contexto s, selecione **ver registos de IntelliTrace**. 

    ![Ver a opção de menu de registos do IntelliTrace](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/view-intellitrace-logs.png)

1. Os registos do IntelliTrace são transferidos para um ficheiro num diretório no seu computador local. Registos de cada vez que solicite o IntelliTrace, é criado um novo instantâneo. Embora os registos estão a ser transferidos, o Visual Studio exibe o progresso da operação no **registo de atividades do Azure** janela. Conforme mostrado na figura a seguir, pode expandir o item de linha para a operação ver mais detalhes.

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

Pode continuar a trabalhar no Visual Studio, enquanto estiver a transferir os registos do IntelliTrace. Quando o registo de concluir a transferência, é aberto no Visual Studio.

> [!NOTE]
> Os registos do IntelliTrace podem conter exceções que a estrutura gera e trata-se, posteriormente. Código do framework interno gera essas exceções como uma parte normal de iniciar uma função de, pelo que pode ignorar com segurança.
> 
> 

## <a name="next-steps"></a>Passos Seguintes
- [Opções de depuração de serviços cloud do Azure](vs-azure-tools-debugging-cloud-services-overview.md)
- [Publicação de um serviço cloud do Azure com o Visual Studio](vs-azure-tools-publishing-a-cloud-service.md)