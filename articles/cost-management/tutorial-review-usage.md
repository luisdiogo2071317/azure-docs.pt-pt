---
title: Tutorial - rever a utilização e os custos do Azure Cost Management | Microsoft Docs
description: Neste tutorial, reveja a utilização e os custos para ver tendências, detetar ineficiências e criar alertas.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 02/27/2018
ms.topic: tutorial
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 558dcd65051c0134a87205dcd8bbf432d7763fd2
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
<!-- Intent: As a cloud-consuming user, I need to view usage and costs for my cloud resources and services.
-->

# <a name="tutorial-review-usage-and-costs"></a>Tutorial: rever a utilização e os custos

O Azure Cost Management mostra-lhe a utilização e os custos, de modo a que possa ver tendências, detetar ineficiências e criar alertas. Todos os dados de utilização e de custos são apresentados nos relatórios e nos dashboards da Cloudyn. Os exemplos neste tutorial orientam-no ao longo da revisão da utilização e dos custos através dos dashboards e dos relatórios. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Ver as tendências de utilização e de custos
> * Detetar ineficiências na utilização
> * Criar alertas para despesas ou gastos excessivos invulgares

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

- Tem de ter uma conta do Azure.
- Tem de ter um registo de avaliação ou uma subscrição paga do Azure Cost Management.

## <a name="open-the-cloudyn-portal"></a>Abrir o portal da Cloudyn

A utilização e os custos são revistos no portal da Cloudyn. Abra o portal da Cloudyn a partir do portal do Azure ou navegue para https://azure.cloudyn.com e inicie sessão.

## <a name="track-usage-and-cost-trends"></a>Ver as tendências de utilização e de custos

O dinheiro efetivamente gasto para utilização e custos é controlado com os relatórios Ao Longo do Tempo, de modo a identificar tendências. Para começar a procurar tendências, utilize o relatório Custo Efetivo ao Longo do Tempo. No menu de relatórios na parte superior do portal, clique em **Custos** > **Análise de Custos** > **Custo Efetivo ao Longo do Tempo**. Da primeira vez que abrir o relatório, não estão aplicados grupos ou filtros ao mesmo.

Segue-se um exemplo de relatório:

![relatório de exemplo](./media/tutorial-review-usage/actual-cost01.png)

O relatório mostra todos os gastos ao longo dos últimos 30 dias. Para ver os gastos apenas para serviços do Azure, aplique o grupo Serviços e filtre por todos os serviços do Azure. A imagem seguinte mostra os serviços filtrados.

![serviços filtrados](./media/tutorial-review-usage/actual-cost02.png)

No exemplo anterior, foi gasto menos dinheiro a partir de 31-08-2017 do que antes. Essa tendência de custos continua para os vários serviços durante nove dias. Depois, as despesas adicionais prosseguem como antes. No entanto, haver demasiadas colunas pode ocultar uma tendência óbvia. Pode alterar a vista do relatório para uma linha ou área do gráfico para ver os dados apresentados noutras vistas. A imagem seguinte mostra a tendência com mais clareza.

![tendência no relatório](./media/tutorial-review-usage/actual-cost03.png)

No exemplo, pode ver facilmente que os custos com o Armazenamento do Azure desceram a partir de 31-08-2017, ao passo que as despesas com outros serviços do Azure permaneceram equilibradas. Então, o que é que originou a redução nas despesas? Neste exemplo, alguns colaboradores estavam de férias e não utilizaram o serviço de Armazenamento.

Para assistir a um vídeo tutorial sobre como ver as tendências de utilização e de custos, veja [Analyzing your cloud billing data vs. time with Azure Cost Management (Analisar os dados de faturação na cloud vs. tempo com o Azure Cost Management)](https://youtu.be/7LsVPHglM0g).

## <a name="detect-usage-inefficiencies"></a>Detetar ineficiências na utilização

Os relatórios de otimizador melhoram a eficiência, otimizam a utilização e identificam formas de poupar dinheiro nos seus recursos da cloud. São especialmente úteis para recomendações de dimensionamento económico, com o objetivo de ajudar a reduzir o número de VMs inativas ou dispendiosas.

Um problema comum que afeta as organizações quando movem inicialmente os recursos para a cloud é a estratégia de virtualização. Muitas vezes, utilizam uma abordagem semelhante à que utilizaram para criar as máquinas virtuais para o ambiente de virtualização no local. E partem do princípio de que basta mover as VMs no local para a cloud tal como estão para que os custos baixem. No entanto, é pouco provável que esta abordagem tenha esse efeito.

O problema é que as respetivas infraestruturas já estavam pagas. Os utilizadores podiam criar e manter em execução VMs grandes se quisessem, inativas ou não, e as consequências eram poucas. Mover VMs grandes ou inativas para a cloud leva a um provável *aumento* dos custos. É importante ter em conta a alocação de custos para recursos quando celebra contratos com fornecedores de serviços cloud. Tem de pagar aquilo com que se comprometeu, quer utilize os recursos na totalidade ou não.

O relatório Recomendações de Dimensionamento Económico identifica potenciais poupanças anuais, ao comparar a capacidade do tipo de instância de VM com os dados históricos de utilização de CPU e memória.  

No menu de relatórios na parte superior do portal, clique em **Otimizador** > **Otimização de Preços** > **Recomendações de Dimensionamento Económico**. Filtre o fornecedor para o Azure para ver apenas as VMs do Azure. Segue-se uma imagem de exemplo.

![VMs do Azure](./media/tutorial-review-usage/sizing01.png)

Neste exemplo, seguir as recomendações para alterar os tipos de instância de VM levaria a uma poupança de 3114 $. Clique no sinal de adição (+) em **Detalhes** na primeira recomendação. Seguem-se os detalhes da primeira recomendação.

![detalhes da recomendação](./media/tutorial-review-usage/sizing02.png)

Clique no sinal de mais junto a **Lista de Candidatos** para ver os IDs das instâncias de VMs.

![Lista de Candidatos](./media/tutorial-review-usage/sizing03.png)

Para ver um tutorial em vídeo sobre como detetar ineficiências de utilização, veja [Optimizing VM Size in Azure Cost Management (Otimizar o Tamanho das VMs no Azure Cost Management)](https://youtu.be/1xaZBNmV704).

## <a name="create-alerts-for-unusual-spending"></a>Criar alertas para despesas invulgares

Pode alertar os outros intervenientes automaticamente sobre despesas anómalas e riscos de incorrer em gastos excessivos. Pode criar rápida e facilmente os alertas através de relatórios que os suportem com base em limiares de orçamentos e de custos.

Pode utilizar qualquer relatório Custos para criar um alerta para qualquer despesa. Neste exemplo, utilize o relatório Custo Efetivo ao Longo do Tempo para ser notificado quando os gastos com VMs do Azure estiverem prestes a atingir o seu orçamento total. No menu de relatórios na parte superior do portal, clique em **Custos** > **Análise de Custos** > **Custo Efetivo ao Longo do Tempo**. Defina **Grupos** como **Serviços** e defina **Filtrar por serviço** como **Azure/VM**. No canto superior direito do relatório, clique em **Ações** e selecione **Agendar relatório**.

Utilize o separador **Agendamento** para enviar um e-mail para si próprio com o relatório de acordo com a frequência que pretenda. Todos os grupos, etiquetas e filtragem que utilizou são incluídos no e-mail com o relatório. Clique no separador **Limiar** e selecione **Custo Efetivo vs. Limiar**. Se tiver um orçamento total de 500 000 $ e quiser ser notificado quando os custos estiverem quase a atingir metade do orçamento, pode criar um **Alerta vermelho** nos 250 000 $ e um **Alerta amarelo** nos 240 000 $. Depois, escolha o número de alertas consecutivos. Quando receber o número total de alertas que especificou, não são enviados alertas adicionais. Guarde o relatório agendado.

![relatório de exemplo](./media/tutorial-review-usage/schedule-alert01.png)

Também pode escolher a métrica Percentagem de Custos vs. Limiar de orçamento para criar os alertas. Com esta métrica, pode utilizar percentagens do orçamento em vez de valores de moeda.


## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Ver as tendências de utilização e de custos
> * Detetar ineficiências na utilização
> * Criar alertas para despesas ou gastos excessivos invulgares


Avance para o tutorial seguinte para aprender a prever despesas através de dados históricos.

> [!div class="nextstepaction"]
> [Prever despesas futuras](tutorial-forecast-spending.md)
