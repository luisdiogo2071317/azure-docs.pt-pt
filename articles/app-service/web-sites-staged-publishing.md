---
title: Configurar ambientes de teste para aplicações web no App Service do Azure | Documentos da Microsoft
description: Saiba como utilizar a publicação faseada para web apps no App Service do Azure.
services: app-service
documentationcenter: ''
author: cephalin
writer: cephalin
manager: erikre
editor: mollybos
ms.assetid: e224fc4f-800d-469a-8d6a-72bcde612450
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: cephalin
ms.openlocfilehash: c6e28554f86a6a7de5d50027ceb552e15856bc96
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52970863"
---
# <a name="set-up-staging-environments-in-azure-app-service"></a>Configurar ambientes de teste no serviço de aplicações do Azure
<a name="Overview"></a>

Ao implementar a aplicação web, aplicação web no Linux, back-end móvel e aplicação de API para [serviço de aplicações](https://go.microsoft.com/fwlink/?LinkId=529714), pode implementar num bloco de implementação separados em vez do bloco de produção predefinido ao executar o **padrão**, **Premium**, ou **Isolated** escalão do plano de serviço de aplicações. Blocos de implementação são aplicações, na verdade, em direto com os seus próprios nomes de anfitrião. Elementos de conteúdo e de configuração da aplicação podem ser trocados entre duas ranhuras de implementação, incluindo o bloco de produção. Implementar a aplicação para um bloco de implementação tem as seguintes vantagens:

* Pode validar alterações da aplicação num bloco de implementação de teste antes de a colocar com o bloco de produção.
* Implementar uma aplicação para um bloco de primeiro e colocar em produção assegura que todas as instâncias do slot são preparadas antes de serem colocadas em produção. Esta ação elimina o tempo de inatividade, quando implementar a sua aplicação. O redirecionamento de tráfego é totalmente integrado, e não existem pedidos são ignorados como resultado de operações de troca. Este fluxo de trabalho completo pode ser automatizado através da configuração [comutação automática](#Auto-Swap) quando a validação de pré-troca de não é necessária.
* Após uma alternância de, a ranhura com a aplicação anteriormente faseada tem agora a aplicação de produção anterior. Se as alterações colocadas em bloco de produção não conforme esperado, pode executar a mesma troca imediatamente para obter sua "última boa site conhecido" novamente.

Cada escalão de plano de serviço de aplicações suporta um número diferente de blocos de implementação. Para obter informações sobre o número de ranhuras suporta a camada da sua aplicação, consulte [limites de serviço de aplicações](https://docs.microsoft.com/azure/azure-subscription-service-limits#app-service-limits). Para dimensionar a sua aplicação para um escalão diferente, a camada de destino tem de suportar o número de ranhuras a que sua aplicação já utiliza. Por exemplo, se a sua aplicação tiver mais de 5 ranhuras, não é possível dimensionar até **padrão** tier, porque **padrão** escalão suporta apenas 5 blocos de implementação.

<a name="Add"></a>

## <a name="add-a-deployment-slot"></a>Adicionar um bloco de implementação
A aplicação tem de executar o **padrão**, **Premium**, ou **Isolated* escalão para que a ativação de vários blocos de implementação.

1. Na [Portal do Azure](https://portal.azure.com/), abra a sua aplicação [painel de recursos](../azure-resource-manager/resource-group-portal.md#manage-resources).
2. Escolha o **blocos de implementação** opção, em seguida, clique em **adicionar ranhura**.
   
    ![Adicionar um novo bloco de implementação][QGAddNewDeploymentSlot]
   
   > [!NOTE]
   > Se a aplicação já não está no **padrão**, **Premium**, ou **Isolated* camada, receberá uma mensagem a indicar as camadas de suporte para ativar a publicação faseada. Neste ponto, tem a opção de selecionar **atualizar** e navegue para o **dimensionamento** separador da sua aplicação antes de continuar.
   > 
   > 
3. Na **adicionar uma ranhura** painel, dê um nome de ranhura de e selecione se pretende clonar a configuração de aplicações do outro bloco de implementação existente. Clique na marca de verificação para continuar.
   
    ![Origem da Configuração][ConfigurationSource1]
   
    Na primeira vez, adicionar uma ranhura, só tem duas opções: configuração de clone da ranhura padrão na produção ou nada.
    Depois de criar várias ranhuras, poderá clonar a configuração a partir de uma ranhura diferente em produção:
   
    ![Origens de configuração][MultipleConfigurationSources]
4. No painel de recursos da sua aplicação, clique em **blocos de implementação**, em seguida, clique num bloco de implementação para abrir o painel de recursos desse bloco, com um conjunto de métricas e configuração assim como qualquer outra aplicação. O nome do slot é mostrado na parte superior do painel para lembrá-lo de que está a ver o bloco de implementação.
   
    ![Título do bloco de implementação][StagingTitle]
5. Clique no URL da aplicação no painel da ranhura. Observe que o bloco de implementação tem seu próprio nome de anfitrião e também é uma aplicação em direto. Para limitar o acesso público a ranhura de implementação, consulte [aplicação Web do App Service – bloquear o acesso web a blocos de implementação de não produção](https://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).

Não existe conteúdo após a criação de ranhura de implementação. Pode implementar para o bloco de um ramo do repositório diferente ou um repositório completamente diferente. Também pode alterar a configuração da ranhura. Utilize as credenciais de implementação ou perfil de publicar associadas com o bloco de implementação para atualizações de conteúdo.  Por exemplo, pode [publicar esta ranhura com o git](app-service-deploy-local-git.md).

<a name="AboutConfiguration"></a>

## <a name="which-settings-are-swapped"></a>As definições que são ativadas?
Quando clona configuração a partir de outro bloco de implementação, a configuração clonada é editável. Além disso, alguns elementos de configuração irão seguir o conteúdo numa troca (não ranhura específica), enquanto outros elementos de configuração irão permanecer na ranhura de mesmo após uma alternância de (bloco específico). As listas seguintes mostram as definições que alteram quando trocar as ranhuras.

**As definições que são ativadas**:

* Definições gerais - como sockets Web do framework versão, 32/64 bits,
* Definições da aplicação (pode ser configurado para utilizar um bloco de)
* Cadeias de ligação (pode ser configurado para utilizar um bloco de)
* Mapeamentos de processador
* Definições de monitorização e diagnósticos
* Conteúdo de WebJobs
* Ligações híbridas

**As definições que não são ativadas**:

* Pontos finais de publicação
* Nomes de domínio personalizados
* Certificados SSL e enlaces
* Definições de dimensionamento
* Agendadores de WebJobs

Para configurar uma cadeia de ligação ou definição de aplicação de adotar uma ranhura (não trocada), aceda a **as definições da aplicação** painel para um bloco específico, em seguida, selecione a **definição de ranhura** caixa para a configuração elementos que devem adotar o bloco. Marcando um elemento de configuração como ranhura específica tem o efeito de estabelecer esse elemento como não-swap em todas as ranhuras de implementação associadas à aplicação.

![Definições de ranhura][SlotSettings]

<a name="Swap"></a>

## <a name="swap-deployment-slots"></a>Trocar as ranhuras de implementação 
Pode trocar as ranhuras de implementação no **descrição geral** ou **blocos de implementação** vista do painel de recursos da sua aplicação.

> [!IMPORTANT]
> Antes de mudar uma aplicação a partir de um bloco de implementação para produção, certifique-se de que todas as definições específicas do bloco de não estão configuradas exatamente como pretende que o destino de troca.
> 
> 

1. Para trocar as ranhuras de implementação, clique nas **troca** botão na barra de comandos da aplicação ou na barra de comandos de um bloco de implementação.
   
    ![Botão de alternância][SwapButtonBar]

2. Certifique-se de que a origem de troca e o destino de comutação estão definidas corretamente. Normalmente, o destino de comutação é o bloco de produção. Clique em **OK** para concluir a operação. Quando a operação estiver concluída, os blocos de implementação tiveram sido alternados.

    ![Concluir troca](./media/web-sites-staged-publishing/SwapImmediately.png)

    Para o **troca com pré-visualização** tipo de troca, consulte [troca com pré-visualização (troca de múltiplas fase)](#Multi-Phase).  

<a name="Multi-Phase"></a>

## <a name="swap-with-preview-multi-phase-swap"></a>Troca com pré-visualização (múltiplas fase swap)

Troca com pré-visualização ou troca de múltiplas fase, simplificar a validação de elementos de configuração específicas de bloco, como cadeias de ligação.
Para cargas de trabalho de missão crítica, deseja validar que a aplicação se comporta conforme o esperado quando é aplicada a configuração da ranhura de produção, e tem de efetuar essa validação *antes de* a aplicação é colocada em produção. Troca com pré-visualização é o que precisa.

> [!NOTE]
> Troca com pré-visualização não é suportada nas aplicações web no Linux.

Quando utiliza a **troca com pré-visualização** opção (consulte [trocar as ranhuras de implementação](#Swap)), o serviço de aplicações faz o seguinte:

- Mantém o bloco de destino inalterado para que a carga de trabalho existente em que a ranhura (por exemplo, produção) não é afetada.
- Aplica-se os elementos de configuração da ranhura de destino para a ranhura de origem, incluindo as cadeias de caracteres de conexão específica do bloco e as definições da aplicação.
- Reinicia os processos de trabalho na ranhura de origem com estes elementos de configuração mencionados anteriormente.
- Quando concluir a troca: move a ranhura de origem do pre-warmed-up para o bloco de destino. A ranhura de destino é movida para a ranhura de origem como numa troca manual.
- Quando cancelar a troca: volta a aplicar os elementos de configuração da ranhura de origem para a ranhura de origem.

Pode visualizar exatamente como a aplicação se irá comportar com a configuração da ranhura de destino. Depois de concluir a validação, concluir a troca num passo separado. Este passo tem a vantagem adicional de que a ranhura de origem já está preparada com a configuração pretendida e os clientes não experienciar qualquer período de inatividade.  

Exemplos de cmdlets do Azure PowerShell disponíveis para troca de múltiplas fase são incluídos em cmdlets do Azure PowerShell para a secção de ranhuras de implementação.

<a name="Auto-Swap"></a>

## <a name="configure-auto-swap"></a>Configurar a troca automática
Troca automática simplifica cenários DevOps onde pretende implementar continuamente a sua aplicação com zero a frio e nenhum tempo de inatividade para os clientes do fim da aplicação. Quando um bloco de implementação está configurado para troca automática em produção, sempre que a atualização de código para que a ranhura, o serviço de aplicações será automaticamente a mudar a aplicação para produção, depois de já tem aquecido na ranhura de.

> [!IMPORTANT]
> Quando ativa a troca automática de mensagens em fila para um bloco, certifique-se de que a configuração da ranhura é exatamente a configuração que se destina a ranhura de destino (normalmente, o bloco de produção).
> 
> 

> [!NOTE]
> Troca automática não é suportada nas aplicações web no Linux.

Configurar a troca automática de mensagens em fila para um bloco é fácil. Siga estes passos.

1. Na **blocos de implementação**, selecione uma ranhura de não produção e escolha **configurações de aplicativo** no painel de recursos desse bloco.  
   
    ![][Autoswap1]
2. Selecione **nos** para **comutação automática**, selecione a ranhura de destino pretendida na **ranhura de troca automática**e clique em **guardar** na barra de comandos. Certifique-se de configuração para o bloco é exatamente a configuração que se destina a ranhura de destino.
   
    O **notificações** separador pisca um verde **êxito** depois de concluída a operação.
   
    ![][Autoswap2]
   
   > [!NOTE]
   > Para testar a troca automática para a sua aplicação, pode selecionar primeiro uma ranhura de destino de não produção na **ranhura de troca automática** para se familiarizar com a funcionalidade.  
   > 
   > 
3. Execute um push de código para esse bloco de implementação. Acontece de troca automática após um curto período de tempo e a atualização é refletida no URL da ranhura de destino.

<a name="Rollback"></a>

## <a name="roll-back-a-production-app-after-swap"></a>Reverter uma aplicação de produção após a troca
Se todos os erros são identificados em produção após uma alternância de bloco, reverta ranhuras para seus Estados de pré-comutação de ao trocar as ranhuras de dois mesmo imediatamente.

<a name="Warm-up"></a>

## <a name="custom-warm-up-before-swap"></a>Personalizado aquecimento antes de troca
Ao usar [troca automática](#Auto-Swap), algumas aplicações podem exigir ações de aquecimento personalizado. O `applicationInitialization` elemento de configuração em Web. config permite-lhe especificar ações de inicialização personalizada a ser executada antes de uma solicitação é recebida. A operação de troca aguarda que este aquecimento personalizado concluir. Aqui está um fragmento de Web. config de exemplo.

    <system.webServer>
        <applicationInitialization>
            <add initializationPage="/" hostName="[app hostname]" />
            <add initializationPage="/Home/About" hostName="[app hostname]" />
        </applicationInitialization>
    </system.webServer>

## <a name="monitor-swap-progress"></a>Monitorize o progresso de troca

Às vezes, a operação de troca demora algum tempo a concluir, tais como quando a aplicação que é trocada tem um tempo de aquecimento longo. Pode obter mais informações sobre as operações de troca no [registo de atividades](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) no [portal do Azure](https://portal.azure.com).

Na página da aplicação do portal, no painel de navegação esquerdo, selecione **registo de atividades**.

Uma operação de troca é apresentado na consulta de registo como `Slotsswap`. Pode expandi-lo e selecione um dos suboperations ou erros para ver os detalhes.

![Registo de atividades para troca de ranhura](media/web-sites-staged-publishing/activity-log.png)

<a name="Delete"></a>

## <a name="delete-a-deployment-slot"></a>Eliminar um bloco de implementação
No painel de um bloco de implementação, abra o painel do bloco de implementação, clique em **descrição geral** (a página padrão) e clique em **eliminar** na barra de comandos.  

![Eliminar um bloco de implementação][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>

## <a name="automate-with-azure-powershell"></a>Automatizar com o Azure PowerShell

O Azure PowerShell é um módulo que oferece cmdlets para gerir o Azure através do Windows PowerShell, incluindo suporte para o gerenciamento de blocos de implementação no serviço de aplicações do Azure.

* Para obter informações sobre como instalar e configurar o Azure PowerShell e sobre a autenticação do Azure PowerShell com a sua subscrição do Azure, consulte [como instalar e configurar o Microsoft Azure PowerShell](/powershell/azure/overview).  

- - -
### <a name="create-a-web-app"></a>Criar uma aplicação Web
```PowerShell
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [app name] -Location [location] -AppServicePlan [app service plan name]
```

- - -
### <a name="create-a-deployment-slot"></a>Criar um bloco de implementação
```PowerShell
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

- - -
### <a name="initiate-a-swap-with-preview-multi-phase-swap-and-apply-destination-slot-configuration-to-source-slot"></a>Iniciar uma troca com pré-visualização (troca de múltiplas fase) e aplicar a configuração da ranhura de destino para a ranhura de origem
```PowerShell
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="cancel-a-pending-swap-swap-with-review-and-restore-source-slot-configuration"></a>Cancelar uma troca pendente (troca com a revisão) e restaurar a configuração da ranhura de origem
```PowerShell
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

- - -
### <a name="swap-deployment-slots"></a>Trocar as ranhuras de implementação
```PowerShell
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

### <a name="monitor-swap-events-in-the-activity-log"></a>Eventos de troca de monitor no registo de atividades
```PowerShell
Get-AzureRmLog -ResourceGroup [resource group name] -StartTime 2018-03-07 -Caller SlotSwapJobProcessor  
```

- - -
### <a name="delete-deployment-slot"></a>Eliminar o bloco de implementação
```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [app name]/[slot name] -ApiVersion 2015-07-01
```

- - -
<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>

## <a name="automate-with-azure-cli"></a>Automatizar com a CLI do Azure

Para [CLI do Azure](https://github.com/Azure/azure-cli) comandos para blocos de implementação, consulte [bloco de implementação do az webapp](/cli/azure/webapp/deployment/slot).

## <a name="next-steps"></a>Passos Seguintes
[A aplicação – bloquear o acesso web a blocos de implementação de não produção de Web de serviço de aplicações do Azure](https://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)  
[Introdução ao serviço de aplicações no Linux](../app-service/containers/app-service-linux-intro.md)  
[Versão de avaliação gratuita do Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)

<!-- IMAGES -->
[QGAddNewDeploymentSlot]:  ./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png
[AddNewDeploymentSlotDialog]: ./media/web-sites-staged-publishing/AddNewDeploymentSlotDialog.png
[ConfigurationSource1]: ./media/web-sites-staged-publishing/ConfigurationSource1.png
[MultipleConfigurationSources]: ./media/web-sites-staged-publishing/MultipleConfigurationSources.png
[SiteListWithStagedSite]: ./media/web-sites-staged-publishing/SiteListWithStagedSite.png
[StagingTitle]: ./media/web-sites-staged-publishing/StagingTitle.png
[SwapButtonBar]: ./media/web-sites-staged-publishing/SwapButtonBar.png
[SwapConfirmationDialog]:  ./media/web-sites-staged-publishing/SwapConfirmationDialog.png
[DeleteStagingSiteButton]: ./media/web-sites-staged-publishing/DeleteStagingSiteButton.png
[SwapDeploymentsDialog]: ./media/web-sites-staged-publishing/SwapDeploymentsDialog.png
[Autoswap1]: ./media/web-sites-staged-publishing/AutoSwap01.png
[Autoswap2]: ./media/web-sites-staged-publishing/AutoSwap02.png
[SlotSettings]: ./media/web-sites-staged-publishing/SlotSetting.png

