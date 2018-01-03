---
title: "Ver a tendência do custo estimado do laboratório mensal no Azure DevTest Labs | Microsoft Docs"
description: "Saiba mais sobre o gráfico de tendências de custo estimado mensal do Azure DevTest Labs."
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 1f46fdc5-d917-46e3-a1ea-f6dd41212ba4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/22/2017
ms.author: v-craic
ms.openlocfilehash: 660180fac4f4743bc543b1babf7dbe921bf8c26b
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2018
---
# <a name="view-the-monthly-estimated-lab-cost-trend-in-azure-devtest-labs"></a>Ver a tendência do custo estimado do laboratório mensal no Azure DevTest Labs
A funcionalidade de gestão de custo de DevTest Labs ajuda-o a controlar o custo de laboratório. Este artigo ilustra como utilizar o **tendência do custo estimado mensalmente** gráfico para ver custos-para-data estimada o calendário do mês atual dos e o custo de fim do mês previsto para o mês de calendário atual. Este artigo mostra também como para melhor gerir os custos de laboratório, definindo destinos de gastos e os limiares que, quando atingido, o acionador DevTest Labs para reportar os resultados para si.

## <a name="viewing-the-monthly-estimated-cost-trend-chart"></a>Visualizar o gráfico de tendência do custo estimado mensalmente
Para ver o gráfico de tendência do custo estimado mensalmente, siga estes passos: 

1. Inicie sessão no [Portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Se necessário, selecione **todos os serviços**e, em seguida, selecione **DevTest Labs** da lista. (O laboratório já pode ser apresentado no Dashboard em **todos os recursos**).
1. Na lista de laboratórios, selecione o laboratório pretendido.  
1. No laboratório de **descrição geral** área selecione **políticas de configuração e**.   
1. No lado esquerdo em **custo controlo**, selecione **tendência do custo**.

   A seguinte captura de ecrã mostra um exemplo de um gráfico de custo. 
   
    ![Gráfico de custo](./media/devtest-lab-configure-cost-management/graph.png)

O **estimado custo** valor é custos-para-data estimada o calendário do mês atual dos. O **projetado custo** é o custo estimado para o mês de calendário atual todo, calculado com o custo de laboratório de cinco dias anteriores.

As quantidades de custo são arredondadas para o número inteiro seguinte. Por exemplo: 

* 5.01 Arredonda por excesso até 6 
* 5.50 Arredonda por excesso até 6
* 5.99 Arredonda por excesso até 6

Como Estados acima do gráfico, são os custos vir por predefinição no gráfico *estimado* custos de utilização [pay as you go](https://azure.microsoft.com/offers/ms-azr-0003p/) oferecem taxas. Também pode definir seus gastos destinos que são apresentados nos gráficos por [gerir os destinos de custos para o laboratório.](#managing-cost-targets-for-your-lab)

Além disso, seguem-se *não* incluídas no cálculo custo:

* As subscrições Dreamspark e CSP atualmente não são suportadas como Azure DevTest Labs utiliza o [APIs de faturação do Azure](../billing/billing-usage-rate-card-overview.md) para calcular o custo, que não suporta CSP ou Dreamspark subscrições de laboratório.
* As taxas de oferta. Atualmente, não é possível utilizar as taxas de oferta (mostradas na sua subscrição) que pode ter negociados com Microsoft ou Microsoft parceiros. São utilizadas apenas as taxas de pay as you go.
* As taxas
* Os descontos
* Moeda de faturação. Atualmente, o custo de laboratório é apresentado apenas na moeda EUR.

## <a name="managing-cost-targets-for-your-lab"></a>Gestão de destinos de custos para o laboratório
DevTest Labs permite-lhe melhor gerir os custos no laboratório, definindo um destino de gastos, em seguida, pode ver no gráfico de tendência do custo estimado mensalmente. DevTest Labs pode também enviar-lhe uma notificação quando os gastos de destino especificado ou o limiar for atingido. 

1. No seu laboratório **descrição geral** painel, selecione **políticas de configuração e**.
1. No lado esquerdo em **custo controlo**, selecione **tendência do custo**.

    ![Gerir o botão de destino](./media/devtest-lab-configure-cost-management/cost-trend.png)

1. No **tendência do custo** painel, selecione **gerir destino**.

    ![Gerir o botão de destino](./media/devtest-lab-configure-cost-management/cost-trend-manage-target.png)

1. No painel de destino de gerir, especifique o destino pretendido de gastos e limiares. Também pode definir se cada limite selecionado é reportado no gráfico de tendência do custo ou através de uma notificação de webhook.

    ![Gerir o painel de destino](./media/devtest-lab-configure-cost-management/cost-trend-manage-target-pane.png)

   - Selecione um período de tempo durante os quais pretende que os destinos de custo controlados.
      - **Mensal**: destinos de custo são controlados por mês.
      - **Fixo**: destinos de custo são registados para o intervalo de datas que especificar na data de início e campos de data de fim. Normalmente, isto poderá corresponder com quanto o projeto está agendado para execução.
   - Especifique um **custo de destino**. Por exemplo, pode ser quanto pretende gastam a este laboratório no período de tempo que definiu.
   - Selecione para ativar ou desativar qualquer limiar que pretende comunicadas – em incrementos de 25% – até 125% do seu especificado **custo de destino**.
      - **Notificar**: quando deste limiar ser cumprido, será notificado por um URL do webhook que especificar.
      - **Desenhar no gráfico**: quando deste limiar ser cumprido, os resultados são desenhados no gráfico de tendência do custo que pode ver, conforme descrito em [visualizar o gráfico de tendência do custo estimado mensalmente](#viewing-the-monthly-estimated-cost-trend-chart).
   - Se optar por **notificar** quando o limiar for cumprido, tem de especificar um URL do webhook. Na área de integrações de custo, selecione **clique aqui para adicionar uma integração**.

      Introduza um URL do Webhook no painel de notificação de configurar e, em seguida, selecione **OK**.

       ![Configurar o painel de notificação](./media/devtest-lab-configure-cost-management/configure-notification.png)

      - Se especificar **notificar**, tem de definir um URL do webhook.
      - Da mesma forma, se definir um URL do webhook, tem de definir **notificação** para **no** no painel de limiar de custo.
      - Tem de criar um webhook antes de introduzi-lo aqui.  

      Para obter mais informações sobre webhooks, consulte [criar um webhook ou uma função Azure API](../azure-functions/functions-create-a-web-hook-or-api-function.md). 
 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Mensagens de blogue relacionados
* [Duas coisas mais para manter os seus custos rumo no DevTest Labs](https://blogs.msdn.microsoft.com/devtestlab/2016/06/21/keep-your-cost-on-track/)
* [Limiares de custo por que motivo?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/11/why-cost-thresholds/)

## <a name="next-steps"></a>Passos Seguintes
Eis algumas coisas a experimentar seguinte:

* [Definir políticas de laboratório](devtest-lab-set-lab-policy.md) -Saiba como definir várias políticas utilizadas para governar a forma como são utilizados o laboratório e respetivas VMs. 
* [Criar a imagem personalizada](devtest-lab-create-template.md) - quando criar uma VM, especifique uma base que pode ser uma imagem personalizada ou uma imagem do Marketplace. Este artigo ilustra como criar uma imagem personalizada de um ficheiro VHD.
* [Configurar imagens do Marketplace](devtest-lab-configure-marketplace-images.md) - DevTest Labs suporta a criação de VMs baseadas nas imagens do Azure Marketplace. Este artigo ilustra como especificar que, se existirem, as imagens do Azure Marketplace podem ser utilizado ao criar as VMs num laboratório.
* [Criar uma VM num laboratório](devtest-lab-add-vm.md) -ilustra como criar uma VM a partir de uma imagem de base (ou personalizado ou Marketplace) e como trabalhar com artefactos em VM.

