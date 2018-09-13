---
title: Configurar alertas de faturação ou de crédito para subscrições do Azure | Documentos da Microsoft
description: Descreve como pode configurar alertas na sua fatura do Azure para que pode evitar surpresas de faturas.
keywords: alerta de crédito, alerta de faturação
services: billing
documentationcenter: ''
author: adpick
manager: adpick
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: adpick
ms.openlocfilehash: 094bfe041ae04e52b6d998ccfd1d964abf192d6a
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35956619"
---
# <a name="set-up-billing-or-credit-alerts-for-your-microsoft-azure-subscriptions"></a>Posso configurar alertas de faturação ou de crédito para as suas subscrições do Microsoft Azure
Se for o administrador de conta para uma subscrição do Azure, pode utilizar o serviço do Azure de faturação alerta criar personalizado alertas de faturação que o ajudam a monitorizar e gerir a faturação das suas contas do Azure.

Este serviço está em pré-visualização, por isso terá de ativá-la pela primeira vez na página de funcionalidades de pré-visualização.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="set-the-alert-threshold-and-email-recipients"></a>Definir os destinatários de e-mail e de limiar de alerta
1. Visite [a página de funcionalidades de pré-visualização](https://account.windowsazure.com/PreviewFeatures) e ative **serviço de alertas de faturação**.

1. Depois de receber a confirmação de e-mail que o serviço de faturação é ativado para a sua subscrição, visite [a página de subscrições](https://account.windowsazure.com/Subscriptions) no portal de contas. Clique na subscrição que pretende monitorizar e, em seguida, clique em **alertas**.

    ![Captura de ecrã da vista de subscrições do Centro de contas do Azure, com alertas realçado][Image1]

2. Em seguida, clique em **Adicionar alerta** para criar seu primeiro. Pode configurar um total de cinco alertas de faturação por subscrição, com um limiar diferente e até duas destinatários de e-mail para cada alerta.

    ![Captura de ecrã da vista de alertas, onde pode adicionar alerta][Image2]

3. Quando adiciona um alerta, atribua um nome exclusivo, escolha um limite de gastos e, escolha os endereços de e-mail em que os alertas são enviados. Ao definir o limiar, pode escolher entre uma **faturação Total** ou uma **crédito monetário** do **alerta para** lista. Para um total de faturação, um alerta é enviado quando gastos da subscrição é superior ao limiar. Para um crédito monetário, é enviado um alerta quando créditos monetários atingem o limite. Normalmente, os créditos monetários aplicam-se a subscrições de avaliação gratuita e o Visual Studio.

    ![Captura de ecrã da vista de alerta de adição, onde pode configurar destinatários][Image3]

Azure suporta qualquer endereço de e-mail, mas não Certifique-se de que isso, verifique a funciona de endereço de e-mail para erros de digitação.

## <a name="check-on-your-alerts"></a>Verificar os alertas
Depois de configurar alertas, o Centro de contas do lista-los e mostra quantos mais pode configurar. Para cada alerta, verá a data e hora que foi enviado, quer se trate de um alerta para Total de faturação ou de crédito monetário e o limite que configura. O formato de data e hora é 24 horas Hora Universal Coordenada (UTC) e a data é o formato aaaa-mm-dd. Clique no sinal de um alerta na lista para editá-lo ou clicar no caixote eliminá-lo.

## <a name="delete-alerts-or-email-addresses-from-the-azure-billing-alert-service"></a>Eliminar alertas ou endereços de e-mail do serviço do Azure de faturação alerta
Se alguma vez precisar de remover todas as informações do serviço, o endereço de e-mail no ficheiro de atualização ou eliminar o alerta inteiramente.

   ![Captura de ecrã da vista de alerta de eliminação, onde pode remover informações pessoais][Image4]

## <a name="billing-alerts-for-enterprise-agreement-ea-customers"></a>Alertas de faturação para clientes Enterprise Agreement (EA)
As subscrições do EA não são suportadas por este serviço, em vez disso, os clientes com EA podem receber alertas para cada departamento numa inscrição através da definição de quotas de gastos. Ver [as Quotas de gastos do departamento](https://ea.azure.com/helpdocs/departmentSpendingQuotas) no portal do EA para começar a utilizar.

## <a name="learn-more-about-azure-cost-management"></a>Saiba mais sobre o Azure cost management
- Os custos de estimativa com o [Calculadora de preços](https://azure.microsoft.com/pricing/calculator/), [custo total da Calculadora de propriedade](https://aka.ms/azure-tco-calculator), e ao adicionar um serviço.
- [Reveja a utilização e custos regularmente no portal do Azure](billing-getting-started.md#costs).
- Ative [recomendações de custos do Assistente do Azure](../advisor/advisor-cost-recommendations.md).

Para obter mais informações, consulte [documentação de orientação de gestão de custos do Azure](billing-getting-started.md).

[Image1]: ./media/azure-billing-set-up-alerts/billingalert1.png 
[Image2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Image3]: ./media/azure-billing-set-up-alerts/billingalerts3.png 
[Image4]: ./media/azure-billing-set-up-alerts/AlertsDeleteScreen1.PNG
