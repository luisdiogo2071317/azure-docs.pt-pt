---
title: Gerir a utilização e faturação para o Azure Stack como fornecedor de serviços Cloud | Documentos da Microsoft
description: Uma passagem de registar o Azure Stack como um fornecedor de Cloud (CSP) e adicionando os clientes para faturação.
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
ms.date: 02/13/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: 1ba3697b5c683ed2e892396db71328e0ed41fdce
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/14/2019
ms.locfileid: "56266922"
---
# <a name="manage-usage-and-billing-for-azure-stack-as-a-cloud-service-provider"></a>Gerir a utilização e faturação para o Azure Stack como fornecedor de serviços Cloud

*Aplica-se a: Sistemas integrados do Azure Stack*

Este artigo descreve a registar o Azure Stack como um fornecedor de Cloud (CSP) e adicionar os clientes.

Como um CSP, trabalhar com diversos clientes através do Azure Stack. Cada cliente tem uma subscrição do CSP no Azure. Tem de direcionar utilização partir do Azure Stack para cada subscrição do utilizador.

A figura seguinte mostra os passos necessários para escolher a sua conta de serviços compartilhados e registar a conta do Azure com a conta do Azure Stack. Depois de registado, pode carregar o cliente final:

[![Processo para ativar a utilização e gestão como um fornecedor de serviços Cloud](media/azure-stack-add-manage-billing-as-a-csp/process-add-useage-as-a-csp.png "processo para ativar a utilização e gestão como um fornecedor de serviços Cloud")](media/azure-stack-add-manage-billing-as-a-csp/process-add-useage-as-a-csp.png#lightbox)

## <a name="create-a-csp-or-apss-subscription"></a>Criar uma subscrição do CSP ou APSS

### <a name="cloud-service-provider-subscription-types"></a>Tipos de subscrição do fornecedor de serviços cloud

Escolha o tipo de conta de serviços compartilhados que utiliza para o Azure Stack. Os tipos de subscrições que podem ser utilizadas para registo de um multi-inquilino do Azure Stack são:

- Fornecedor de serviços cloud
- Subscrição de serviços compartilhados do parceiro

#### <a name="azure-partner-shared-services"></a>Serviços compartilhados de parceiros do Azure

Subscrições de serviços partilhados parceiro (APSS) do Azure são a opção preferencial para o registo quando um CSP direto ou um distribuidor CSP opera o Azure Stack.

Subscrições de APSS estão associadas um inquilino de serviços compartilhados. Quando registar o Azure Stack, forneça credenciais para uma conta que seja o proprietário da subscrição. A conta que utiliza para registar o Azure Stack pode ser diferente da conta de administrador que utiliza para a implementação. Além disso, as duas contas não têm de pertencer ao mesmo domínio; Pode implementar a utilizar o inquilino que já utilizam. Por exemplo, pode usar `ContosoCSP.onmicrosoft.com`, em seguida, registe-se com um inquilino diferente; por exemplo, `IURContosoCSP.onmicrosoft.com`. Lembre-se de iniciar sessão com `ContosoCSP.onmicrosoft.com` ao efetuar a administração diária do Azure Stack. Inicie sessão no Azure com `IURContosoCSP.onmicrosoft.com` quando precisa fazer operações de registo.

Para obter uma descrição das subscrições de APSS e como criá-los, consulte [adicionar Azure Partner serviços compartilhados](https://msdn.microsoft.com/partner-center/shared-services).

#### <a name="csp-subscriptions"></a>Subscrições de CSP

Subscrições do fornecedor de serviços (CSP) de cloud são a opção preferencial para o registo quando um revendedor CSP ou um cliente final opera o Azure Stack.

## <a name="register-azure-stack"></a>Registar o Azure Stack

Utilize a subscrição de APSS criada com as informações na secção anterior para registar o Azure Stack com o Azure. Para obter mais informações, consulte [registar o Azure Stack com a sua subscrição do Azure](azure-stack-registration.md).

## <a name="add-end-customer"></a>Adicionar cliente final

Para configurar o Azure Stack, de modo que quando um novo inquilino utiliza recursos, a utilização das mesmas é comunicada para a subscrição do fornecedor de serviços Cloud (CSP), consulte [adicionar inquilino para utilização e faturação no Azure Stack](azure-stack-csp-howto-register-tenants.md).

## <a name="charge-the-right-subscriptions"></a>Cobrar as subscrições certas

O Azure Stack utiliza um recurso chamado de registo. Um registo é um objeto armazenado no Azure. O objeto de registo documenta as subscrições do Azure para utilizar a cobrar um determinado Azure Stack. Esta seção aborda a importância de registo.

Com o registo do Azure Stack pode:

- Reencaminhar dados de utilização do Azure Stack para o Azure Commerce e uma subscrição do Azure são faturadas.
- Cada cliente de relatório ' utilização numa subscrição diferente com uma implementação multi-inquilino do Azure Stack. Arquitetura "multitenancy" permite que o Azure Stack oferecer suporte a diferentes organizações na mesma instância do Azure Stack.

Para cada Azure Stack, existe uma subscrição predefinida e muitos inquilinos subscrições. A assinatura padrão é uma subscrição do Azure que é cobrada se não houver nenhuma subscrição de inquilino específico. Tem de ser a primeira assinatura sejam registrados. Para trabalhar de relatórios de utilização da multi-inquilino, a subscrição tem de ser uma subscrição do CSP ou APSS.

Em seguida, o registo é atualizado com uma subscrição do Azure para cada inquilino que utiliza o Azure Stack. Subscrições de inquilino tem de ser do tipo CSP e tem de agregação para o parceiro que detém a assinatura padrão. Não é possível registar os clientes de outra pessoa.

Quando o Azure Stack reencaminha as informações de utilização para o global Azure, um serviço do Azure o registo de atua como consultor e utilização de cada inquilino é mapeado para a subscrição do inquilino adequado. Se não tiver sido registado um inquilino, que a utilização de vai para a subscrição predefinida para a instância do Azure Stack a partir do qual foi gerado.

Uma vez que as subscrições de inquilino são subscrições de CSP, sua fatura é enviada para o parceiro CSP e informações de utilização não são visíveis para o cliente final.

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre o programa CSP, veja [programa Cloud Solution Provider](https://partner.microsoft.com/solutions/microsoft-cloud-solutions).
- Para saber mais sobre como recuperar informações de utilização de recursos do Azure Stack, veja [utilização e faturação no Azure Stack](azure-stack-billing-and-chargeback.md).
