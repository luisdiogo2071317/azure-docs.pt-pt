---
title: Reportar dados de utilização do Azure Stack para o Azure | Documentos da Microsoft
description: Saiba como configurar relatórios no Azure Stack de dados de utilização.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2019
ms.author: sethm
ms.reviewer: alfredop
ms.openlocfilehash: 147242e7d5d62204198360f580848cd997850d09
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/16/2019
ms.locfileid: "54332528"
---
# <a name="report-azure-stack-usage-data-to-azure"></a>Reportar dados de utilização do Azure Stack para o Azure

Dados de utilização, também chamados de dados de consumo, representam a quantidade de recursos utilizados.

Sistemas de vários nós de pilha do Azure que utilizam o modelo de faturação baseado no consumo devem reportar dados de utilização do Azure para efeitos de faturação. Operadores do Azure Stack devem configurar a sua instância do Azure Stack para reportar dados de utilização para o Azure.

> [!IMPORTANT]
> Todas as cargas de trabalho [tem de ser implementada nas subscrições do inquilino](#are-users-charged-for-the-infrastructure-vms) para estar em conformidade com os termos de licenciamento do Azure Stack.

Relatórios de dados de utilização é necessário para os utilizadores com vários nós do Azure Stack portadores de licenças de acordo com o modelo de Pay-as que use. É opcional para os clientes que sob o modelo de capacidade de licença (consulte a [como comprar página](https://azure.microsoft.com/overview/azure-stack/how-to-buy/). Para usuários do Kit de desenvolvimento do Azure Stack, operadores do Azure Stack podem reportar dados de utilização e a funcionalidade de teste. No entanto, os utilizadores não são cobrados de qualquer utilização em que incorrem.

![fluxo de faturação](media/azure-stack-usage-reporting/billing-flow.png)

Dados de utilização são enviados do Azure Stack para o Azure através de Bridge do Azure. No Azure, o sistema de comércio processa os dados de utilização e gera a fatura. Depois de fatura é gerada, o proprietário da subscrição do Azure pode ver e transferi-lo a partir da [Centro de contas do Azure](https://account.windowsazure.com/Subscriptions). Para saber mais sobre como é licenciado do Azure Stack, veja a [do Azure Stack pacotes e preços documento](https://go.microsoft.com/fwlink/?LinkId=842847).

## <a name="set-up-usage-data-reporting"></a>Configurar relatórios de dados de utilização

Para configurar os relatórios de dados de utilização, terá [registe a sua instância do Azure Stack com o Azure](azure-stack-register.md). Como parte do processo de registo, o componente de ponte de Azure do Azure Stack, que liga o Azure Stack para o Azure e envia os dados de utilização, está configurado. Os seguintes dados de utilização são enviados do Azure Stack para Azure:

- **ID do medidor** – ID exclusivo para o recurso que foi consumido.
- **Quantidade** – quantidade de utilização de recursos.
- **Localização** – localização onde o recurso atual do Azure Stack é implementado.
- **URI do recurso** – completamente qualificado do URI do recurso para o qual está a ser comunicada a utilização.
- **ID de subscrição** – ID de subscrição do utilizador do Azure Stack, que é a subscrição (Azure Stack) local.
- **Tempo** – hora de início e fim dos dados de utilização. Existe algum atraso entre a hora quando esses recursos são consumidos no Azure Stack e quando os dados de utilização são reportados para comércio. Azure Stack de dados de utilização de agregações para a cada 24 horas e os dados de utilização de relatórios para o pipeline de comércio no Azure leva a outro algumas horas. Então, utilização, que ocorre um pouco antes da meia-noite pode aparecer no Azure do dia seguinte.

## <a name="generate-usage-data-reporting"></a>Gerar relatórios de dados de utilização

- Para testar os relatórios de dados de utilização, crie alguns recursos no Azure Stack. Por exemplo, pode criar uma [conta de armazenamento](azure-stack-provision-storage-account.md), [VM do Windows Server](azure-stack-provision-vm.md) e uma VM do Linux com os SKUs básico e Standard para ver como é comunicada a utilização de núcleos. Os dados de utilização para diferentes tipos de recursos são reportados a diferentes indicadores.

- Deixe os seus recursos em execução para algumas horas. Informações de utilização são recolhidas aproximadamente uma vez por hora. Depois de coletar, estes dados são transmitidos para o Azure e processados para o sistema de comércio do Azure. Este processo pode demorar algumas horas.

## <a name="view-usage---csp-subscriptions"></a>Ver utilização - subscrições de CSP

Se tiver registado a sua pilha do Azure com uma subscrição do CSP, pode ver a utilização e custos da mesma forma na qual ver o consumo do Azure. Utilização de pilha do Azure estará incluída na sua fatura e no ficheiro de reconciliação, disponível através da [Centro de parceiros](https://partnercenter.microsoft.com/partner/home). O ficheiro de reconciliação é atualizado mensalmente. Se precisar de aceder às informações de utilização recentes do Azure Stack, pode utilizar as APIs do Centro de parceiros.

![Centro de parceiros](media/azure-stack-usage-reporting/partner-center.png)

## <a name="view-usage--enterprise-agreement-subscriptions"></a>Ver utilização – subscrições de contrato Enterprise

Se tiver registado a sua utilização de uma subscrição do Enterprise Agreement do Azure Stack, pode ver a utilização e custos no [portal EA](https://ea.azure.com/). Utilização do Azure Stack está incluída nos downloads avançados juntamente com a utilização do Azure na secção relatórios neste portal. 

## <a name="view-usage--other-subscriptions"></a>Ver utilização – outras subscrições

Se tiver registado a sua pilha do Azure através de qualquer tipo de outra subscrição; Por exemplo, uma subscrição Pay As You Go, pode ver a utilização e custos no Centro de contas do Azure. Inicie sessão para o [Centro de contas do Azure](https://account.windowsazure.com/Subscriptions) como o Azure o administrador de conta e selecione a subscrição do Azure que utilizou para registar o Azure Stack. Pode ver os dados de utilização do Azure Stack, o montante cobrado por cada um dos recursos utilizados, conforme mostrado na imagem seguinte:

![fluxo de faturação](media/azure-stack-usage-reporting/pricing-details.png)

Para o Kit de desenvolvimento do Azure Stack, recursos do Azure Stack não são cobrados, para que o preço é mostrado como $0,00.

## <a name="which-azure-stack-deployments-are-charged"></a>As implementações do Azure Stack são cobradas?

Utilização de recursos é gratuita para o Azure Stack Development Kit. Do Azure Stack com vários nó sistemas, a carga de trabalho VMs, serviços de armazenamento e dos serviços de aplicações, são cobrados.

## <a name="are-users-charged-for-the-infrastructure-vms"></a>Os utilizadores são cobrados para as VMs de infraestrutura?

Não. Dados de utilização para algum fornecedor de recursos do Azure Stack VMs são comunicados ao Azure, mas não existem custos para estas VMs, nem para as VMs criadas durante a implementação permitir que a infraestrutura do Azure Stack.  

Os utilizadores cobrados apenas para VMs que são executadas em subscrições de inquilino. Todas as cargas de trabalho tem de ser implementadas em subscrições de inquilino para estar em conformidade com os termos de licenciamento do Azure Stack.

## <a name="i-have-a-windows-server-license-i-want-to-use-on-azure-stack-how-do-i-do-it"></a>Tenho uma licença do Windows Server que pretende utilizar no Azure Stack, como fazê-lo?

Utilizar as licenças existentes evita a geração de medidores de utilização. Licenças existentes do Windows Server podem ser utilizadas no Azure Stack, conforme descrito na secção "Using software existente com o Azure Stack" a [guia de licenciamento do Azure Stack](https://go.microsoft.com/fwlink/?LinkId=851536). Os clientes devem implementar suas máquinas virtuais do Windows Server, conforme descrito no [benefício híbrido para a licença do Windows Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md) artigo para poder utilizar as suas licenças existentes.

## <a name="which-subscription-is-charged-for-the-resources-consumed"></a>Qual assinatura é cobrada pelos recursos consumidos?

A subscrição que é fornecida quando [registar o Azure Stack com o Azure](azure-stack-register.md) é cobrado.

## <a name="what-types-of-subscriptions-are-supported-for-usage-data-reporting"></a>Que tipos de subscrições são suportados para relatórios de dados de utilização?

Para vários nós do Azure Stack, são suportadas subscrições Enterprise Agreement (EA) e CSP. Para o Kit de desenvolvimento do Azure Stack, subscrições de contrato Enterprise (EA), o pay as you go, o CSP e o MSDN suportam relatórios de dados de utilização.

## <a name="does-usage-data-reporting-work-in-sovereign-clouds"></a>O trabalho em clouds independentes de geração de relatórios de dados de utilização?

No Kit de desenvolvimento do Azure Stack, relatórios de dados de utilização requerem subscrições que são criadas no sistema global do Azure. Subscrições criadas das clouds soberanas (as nuvens do Azure Government, Azure Alemanha e China do Azure) não podem ser registadas com o Azure, para que eles não oferecem suporte a relatórios de dados de utilização.

## <a name="why-doesnt-the-usage-reported-in-azure-stack-match-the-report-generated-from-azure-account-center"></a>Por que a utilização comunicada na pilha do Azure não corresponde ao relatório gerado a partir do Centro de contas do Azure?

Há sempre um atraso entre os dados de utilização comunicados pela utilização do Azure Stack APIs e os dados de utilização comunicados pelo centro de contas do Azure. Este atraso é o tempo necessário para carregar dados de utilização do Azure Stack para comércio do Azure. Devido a este atraso, utilização, que ocorre um pouco antes da meia-noite pode aparecer no Azure do dia seguinte. Se utilizar o [utilização do Azure Stack APIs](azure-stack-provider-resource-api.md)e comparar os resultados para a utilização comunicadas no portal de faturação do Azure, verá uma diferença.

## <a name="next-steps"></a>Passos Seguintes

* [API de utilização do fornecedor](azure-stack-provider-resource-api.md)  
* [API de utilização do inquilino](azure-stack-tenant-resource-usage-api.md)
* [FAQ de utilização](azure-stack-usage-related-faq.md)
* [Gerir a utilização e faturação como fornecedor de serviços Cloud](azure-stack-add-manage-billing-as-a-csp.md)
