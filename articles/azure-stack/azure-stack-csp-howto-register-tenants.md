---
title: Adiciona os inquilinos para utilização e faturação no Azure Stack | Documentos da Microsoft
description: Os passos necessários adicionar um utilizador final ao Azure Stack, gerido por um fornecedor de serviços Cloud (CSP).
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
ms.date: 01/05/2019
ms.author: sethm
ms.reviewer: alfredo
ms.openlocfilehash: be240ea17a8679d25623f971f7be5ea5c3ad2e8f
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2019
ms.locfileid: "54053247"
---
# <a name="add-tenant-for-usage-and-billing-to-azure-stack"></a>Adicionar o inquilino para a utilização e faturação no Azure Stack

*Aplica-se a: Sistemas integrados do Azure Stack*

Este artigo descreve os passos necessários para adicionar um utilizador final para uma implementação do Azure Stack gerenciada por um fornecedor de serviços Cloud (CSP). Quando o novo inquilino utiliza recursos, o Azure Stack relatórios utilização à respetiva subscrição do CSP.

CSPs muitas vezes oferecem serviços para vários clientes finais (inquilinos) na sua implementação do Azure Stack. Adição de inquilinos para o registo do Azure Stack garante que a utilização de cada inquilino é comunicada e cobrada para a subscrição do CSP correspondente. Se não concluir os passos neste artigo, a utilização de inquilino é cobrada para a subscrição utilizada no registo inicial do Azure Stack. Antes de poder adicionar um cliente final para o Azure Stack para controlo de utilização e para gerir o seu inquilino, tem de configurar o Azure Stack como um CSP. Para obter passos e recursos, consulte [gerir a utilização e faturação para o Azure Stack como fornecedor de serviços Cloud](azure-stack-add-manage-billing-as-a-csp.md).

A figura seguinte mostra os passos que precisa de um CSP a seguir para ativar um novo cliente para utilizar o Azure Stack e configurar o controlo para o cliente de utilização. Ao adicionar o cliente final, também é capaz de gerir recursos no Azure Stack. Tem duas opções para gerir os seus recursos:

1. Pode manter o cliente final e forneça as credenciais para a subscrição do Azure Stack local para o cliente final.  
2. O cliente final pode trabalhar com sua assinatura localmente e adicione o CSP como uma convidada com permissões de proprietário.  

## <a name="steps-to-add-an-end-customer"></a>Passos para adicionar um cliente final

![Configurar o fornecedor de serviços Cloud para o controlo de utilização e gerir a conta de cliente do fim](media/azure-stack-csp-enable-billing-usage-tracking/process-csp-enable-billing.png)

### <a name="create-a-new-customer-in-partner-center"></a>Criar um novo cliente no Centro de parceiros

No Centro de parceiros, crie uma nova subscrição do Azure para o cliente. Para obter instruções, consulte [adicionar um novo cliente](https://msdn.microsoft.com/partner-center/add-a-new-customer).

### <a name="create-an-azure-subscription-for-the-end-customer"></a>Criar uma subscrição do Azure para o cliente final

Depois de ter criado um registo dos seus clientes no Centro de parceiros, pode vendê-los de subscrições para produtos no catálogo. Para obter instruções, consulte [Create, suspender ou cancelar subscrições de cliente](https://msdn.microsoft.com/partner-center/create-a-new-subscription).

### <a name="create-a-guest-user-in-the-end-customer-directory"></a>Criar um utilizador convidado no diretório de cliente final

Se o cliente final gerir a sua própria conta, crie um utilizador convidado em seu diretório e envie-lhes as informações. O utilizador final, em seguida, adicionar o convidado e a permissão de convidado para efetuar a elevação **proprietário** para a conta do Azure Stack CSP.

### <a name="update-the-registration-with-the-end-customer-subscription"></a>Atualizar o registo com a subscrição do cliente final

Atualize o registo com a nova subscrição de cliente. Relatórios de utilização do cliente usando a identidade do cliente a partir do Centro de parceiros do Azure. Este passo garante que é comunicada a utilização de cada cliente sob a subscrição do CSP individual do cliente. Isso facilita a utilização de utilizador e de faturação.

> [!NOTE]  
> Para realizar este passo, tem de ter [registado do Azure Stack](azure-stack-register.md).

1. Abra o Windows PowerShell com uma linha de comandos elevada e execute:  
    `Add-AzureRmAccount`
2. Escreva as credenciais do Azure.
3. Na sessão do PowerShell, execute:

   ```powershell
   New-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01 -Properties <PSObject>
   ```

### <a name="new-azurermresource-powershell-parameters"></a>Parâmetros do PowerShell do novo-AzureRmResource

A secção seguinte descreve os parâmetros para o **New-AzureRmResource** cmdlet:

| Parâmetro | Descrição |
| --- | --- |
|registrationSubscriptionID | A subscrição do Azure que foi utilizada para o registo inicial da pilha do Azure.|
| customerSubscriptionID | A subscrição do Azure (não o Azure Stack) pertencentes ao cliente sejam registrados. Tem de ser criada na oferta do CSP; na prática, isso significa através do Centro de parceiros. Se um cliente tiver mais do que um inquilino do Azure Active Directory, esta subscrição tem de ser criada no inquilino que será utilizado para iniciar sessão no Azure Stack. O ID de subscrição de cliente tem de utilizar letras minúsculas. |
| resourceGroup | O grupo de recursos no Azure em que o registo é armazenado. |
| registrationName | O nome do registo do seu Azure Stack. É um objeto armazenado no Azure. |
| Propriedades | Especifica as propriedades do recurso. Utilize este parâmetro para especificar os valores de propriedades que são específicas para o tipo de recurso.

> [!NOTE]  
> Os inquilinos têm de estar registados com cada Azure Stack que utilizam. Se tiver duas implementações do Azure Stack, e um inquilino, utiliza os dois, tem de atualizar os registros de iniciais de cada implementação com a subscrição do inquilino.

### <a name="onboard-tenant-to-azure-stack"></a>Carregar inquilino para o Azure Stack

Configure o Azure Stack para dar suporte aos usuários a partir de múltiplos inquilinos do Azure AD para utilizar os serviços no Azure Stack. Para obter instruções, consulte [ativar multi-inquilinos no Azure Stack](azure-stack-enable-multitenancy.md).

### <a name="create-a-local-resource-in-the-end-customer-tenant-in-azure-stack"></a>Criar um recurso local no inquilino de cliente final no Azure Stack

Depois de ter adicionado o novo cliente para o Azure Stack ou o inquilino de cliente final tiver ativado a sua conta de convidado com privilégios de proprietário, certifique-se de que pode criar um recurso no seu inquilino. Por exemplo, pode [criar uma máquina virtual do Windows com o portal do Azure Stack](user/azure-stack-quick-windows-portal.md).

## <a name="next-steps"></a>Passos Seguintes

- Para rever as mensagens de erro se eles são acionados no seu processo de registo, consulte [mensagens de erro do registo de inquilinos](azure-stack-csp-ref-infrastructure.md#usage-and-billing-error-codes).
- Para saber mais sobre como recuperar informações de utilização de recursos do Azure Stack, veja [utilização e faturação no Azure Stack](azure-stack-billing-and-chargeback.md).
- Para rever como um cliente final poderá adicioná-lo, como o CSP, como o gestor para o respetivo inquilino do Azure Stack, veja [ativar o fornecedor de serviços Cloud gerir a sua subscrição do Azure Stack](user/azure-stack-csp-enable-billing-usage-tracking.md).
