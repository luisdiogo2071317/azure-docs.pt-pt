---
title: Anotações do Application Insights da versão | Documentos da Microsoft
description: Adicionar implementação ou criar marcadores para seus gráficos do Explorador de métricas no Application Insights.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 23173e33-d4f2-4528-a730-913a8fd5f02e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 11/16/2016
ms.author: mbullwin
ms.openlocfilehash: 660080a629e00884dd61a49bc0950ebe25b6a0c5
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/16/2018
ms.locfileid: "42058496"
---
# <a name="annotations-on-metric-charts-in-application-insights"></a>Anotações em gráficos de métricas no Application Insights
Anotações no [Explorador de métricas](app-insights-metrics-explorer.md) gráficos mostram onde implementou uma nova compilação ou outro evento significativo. Eles facilitam ver se as suas alterações tinham qualquer impacto no desempenho da sua aplicação. Eles podem ser criados automaticamente pelos [sistema de compilação do Visual Studio Team Services](https://docs.microsoft.com/vsts/pipelines/tasks/). Também pode criar anotações para sinalizar a qualquer evento desejar [criá-los a partir do PowerShell](#create-annotations-from-powershell).

![Exemplo de anotações com visível correlação com o tempo de resposta do servidor](./media/app-insights-annotations/00.png)



## <a name="release-annotations-with-vsts-build"></a>Notas de versão com compilação VSTS

Notas de versão são uma funcionalidade da compilação com base na cloud e versão de serviço do Visual Studio Team Services. 

### <a name="install-the-annotations-extension-one-time"></a>Instalar a extensão de anotações (uma vez)
Para poder criar notas de versão, terá de instalar uma das extensões de serviço do Team muitos disponíveis no Visual Studio Marketplace.

1. Inicie sessão no seu [Visual Studio Team Services](https://visualstudio.microsoft.com/vso/) projeto.
2. No Visual Studio Marketplace [obter a extensão de notas de versão](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations)e adicione-o à sua conta do Team Services.

![AT principais direita da página da web do Team Services, Marketplace aberto. Selecione Visual Team Services e, em seguida, na criação e versão, escolha ver mais.](./media/app-insights-annotations/10.png)

Apenas terá de fazer isto vez para a sua conta do Visual Studio Team Services. Notas de versão agora podem ser configuradas para qualquer projeto na sua conta. 

### <a name="configure-release-annotations"></a>Configurar notas de versão

Tem de obter uma chave de API separada para cada modelo de versão do VSTS.

1. Inicie sessão para o [Portal do Microsoft Azure](https://portal.azure.com) e abra o recurso do Application Insights monitoriza a aplicação. (Ou [crie uma agora](app-insights-overview.md), se ainda não tiver feito isso ainda.)
2. Open **acesso à API**, **Id de informações da aplicação**.
   
    ![Em portal.azure.com, abra o recurso do Application Insights e escolha definições. Abra o acesso à API. Copie o ID da aplicação](./media/app-insights-annotations/20.png)

4. Numa janela do browser separado, abra (ou crie) o modelo de liberação que gere as suas implementações do Visual Studio Team Services. 
   
    Adicionar uma tarefa e selecione a tarefa de anotação de versão do Application Insights a partir do menu.
   
    Colar o **Id da aplicação** que copiou a partir do painel de acesso à API.
   
    ![No Visual Studio Team Services, abra o lançamento, em seguida, selecione uma definição de versão e selecione Edit. Clique em Adicionar tarefas e selecione a anotação de versão do Application Insights. Cole o ID de informações da aplicação.](./media/app-insights-annotations/30.png)
4. Definir o **APIKey** campo a uma variável `$(ApiKey)`.

5. Na janela do Azure, crie uma nova chave de API e fazer uma cópia do mesmo.
   
    ![No painel de acesso à API a janela do Azure, clique em Criar chave de API. Fornecer um comentário, verifique as anotações de escrita e clique em Gerar chave. Copie a nova chave.](./media/app-insights-annotations/40.png)

6. Abra o separador de configuração do modelo de liberação.
   
    Criar uma definição de variável para `ApiKey`.
   
    Cole a chave de API para a definição de variável ApiKey.
   
    ![Na janela do Team Services, selecione o separador de configuração e clique em Adicionar variável. Defina o nome para ApiKey e no valor, cole a chave que acabou de gerar e clique no ícone de bloqueio.](./media/app-insights-annotations/50.png)
7. Por fim, **guardar** a definição de versão.


## <a name="view-annotations"></a>Ver anotações
Agora, sempre que utilizar o modelo de liberação para implementar uma nova versão, uma anotação será enviada para o Application Insights. As anotações aparecerá em gráficos no Explorador de métricas.

Clique em qualquer marcador de anotação para abrir os detalhes sobre a versão, incluindo o requerente, ramo de controle de origem, versão de definição, o ambiente e muito mais.

![Clique em qualquer marcador de anotação de versão.](./media/app-insights-annotations/60.png)

## <a name="create-custom-annotations-from-powershell"></a>Criar anotações personalizadas do PowerShell
Também pode criar anotações a partir de qualquer processo que quiser (sem utilizar o VS Team System). 


1. Faça uma cópia local do [script do Powershell do GitHub](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).

2. Obtenha o ID da aplicação e criar uma chave de API a partir do painel de acesso à API.

3. Chame o script como este:

```PS

     .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }
```

É fácil modificar o script, por exemplo, para criar anotações para o passado.

## <a name="next-steps"></a>Passos Seguintes

* [Criar itens de trabalho](app-insights-diagnostic-search.md#create-work-item)
* [Automação com PowerShell](app-insights-powershell.md)
