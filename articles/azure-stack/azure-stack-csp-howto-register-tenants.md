---
title: Adicionar inquilinos para utilização e faturação à pilha do Azure | Microsoft Docs
description: Os passos necessários adicionar um utilizador final a pilha de Azure geridos por um fornecedor de serviço em nuvem.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2018
ms.author: mabrigg
ms.reviewer: alfredo
ms.openlocfilehash: 27473ce4057fdb06ab9faf0f46dede62b4ee2246
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37048844"
---
# <a name="add-tenant-for-usage-and-billing-to-azure-stack"></a>Adicionar o inquilino para a utilização e faturação à pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas*

Este artigo descreve os passos necessários adicionar um utilizador final a pilha de Azure geridos por um fornecedor de serviço em nuvem (CSP). Ao novo inquilino utiliza recursos, o Azure pilha reportam a utilização para a subscrição do CSP.

Os CSPs frequentemente oferecem serviços a vários clientes (inquilinos) na sua implementação de pilha do Azure. A adição de inquilinos para o registo de pilha do Azure garante que a utilização de cada inquilino será comunicada e cobrada para a subscrição do CSP correspondente. Se não concluir os passos neste artigo, a utilização de inquilino é cobrada na subscrição utilizada no registo inicial da pilha do Azure. Antes de poder adicionar um cliente do fim a pilha do Azure para controlo de utilização e como gerir o seu inquilino, terá de configurar a pilha do Azure como um CSP. Para os passos e recursos, consulte [gerir a utilização e faturação de pilha do Azure como um fornecedor de serviço em nuvem](azure-stack-add-manage-billing-as-a-csp.md).

O diagrama seguinte mostra os passos que um CSP terá de seguir para ativar um novo cliente para utilizar a pilha do Azure e configurar o controlo para o cliente de utilização. Ao adicionar o cliente do fim, também será gerir os recursos na pilha do Azure. Tem duas opções para gerir os respetivos recursos:

1. Pode manter o inquilino de cliente do fim e fornecer as credenciais para a subscrição do Azure pilha local para o cliente do fim.  
2. Ou o cliente do fim pode trabalhar com a respetiva subscrição localmente e adicione o CSP como convidado com permissões de proprietário.  

**Passos para adicionar um cliente do fim**

![Configurar o fornecedor de serviço em nuvem para controlo de utilização e para gerir a conta de cliente do fim](media\azure-stack-csp-enable-billing-usage-tracking\process-csp-enable-billing.png)

## <a name="create-a-new-customer-in-partner-center"></a>Criar um novo cliente no Centro de parceiros

No Centro de parceiros, crie uma nova subscrição do Azure para o cliente. Para obter instruções, consulte [adicionar um novo cliente](https://msdn.microsoft.com/partner-center/add-a-new-customer).


##  <a name="create-an-azure-subscription-for-the-end-customer"></a>Criar uma subscrição do Azure para o cliente do fim

Depois de criar um registo do cliente no Centro de parceiros, pode propor-los subscrições de produtos no catálogo. Para obter instruções, consulte [criar, suspender ou em Cancelar, subscrições de cliente](https://msdn.microsoft.com/partner-center/create-a-new-subscription).

## <a name="create-a-guest-user-in-the-end-customer-directory"></a>Criar um utilizador convidado no diretório de cliente do fim

Se o cliente do fim irão gerir a sua própria conta, crie um utilizador convidado do respetivo diretório e enviar as informações. O utilizador final, em seguida, adicione o convidado e a permissão de convidado para efetuar a elevação **proprietário** para a conta do Azure pilha CSP.
 
## <a name="update-the-registration-with-the-end-customer-subscription"></a>Atualizar o registo com a subscrição do cliente fim

Atualize o registo com a nova subscrição do cliente. Azure relatórios de utilização do cliente utilizando a identidade do cliente de parceiro Central. Este passo garante que a utilização de cada cliente é considerada na subscrição de CSP individuais que cliente. Isto torna o controlo da utilização de utilizador e de faturação muito mais fácil.

> [!Note]  
> Para realizar este passo, tem de ter [registado Azure pilha](azure-stack-register.md).

1. Abra o Windows PowerShell com uma linha de comandos elevada e execute:  
    `Add-AzureRmAccount`
2. Escreva as credenciais do Azure.
3. Na sessão do PowerShell, execute:

```powershell
    New-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01 -Properties <PSObject>
```
### <a name="new-azurermresource-powershell-parameters"></a>Parâmetros do novo AzureRmResource PowerShell
| Parâmetro | Descrição |
| --- | --- | 
|registrationSubscriptionID | A subscrição do Azure que foi utilizada para o registo inicial da pilha de Azure. |
| customerSubscriptionID | A subscrição do Azure (não pilha do Azure) que pertencem ao cliente a ser registado. Tem de ser criados na oferta CSP; na prática, isto significa que através do Centro de parceiros. Se um cliente tiver mais do que um inquilino do Azure Active Directory, esta subscrição tem de ser criada no inquilino que será utilizado para iniciar sessão na pilha do Azure.
| resourceGroup | O grupo de recursos no Azure no qual o seu registo é armazenado. 
| registrationName | O nome do registo da pilha do Azure. É um objeto armazenado no Azure. | 
| Propriedades | Especifica as propriedades para o recurso. Utilize este parâmetro para especificar os valores de propriedades que são específicas para o tipo de recurso.


> [!Note]  
> Os inquilinos necessitam de ser registado com cada pilha do Azure que utilizam. Se tiver duas implementações de pilha do Azure e um inquilino vai utilizar ambos os parâmetros, terá de atualizar os registos de cada implementação iniciais com a subscrição de inquilino.

## <a name="onboard-tenant-to-azure-stack"></a>Carregar inquilino com a pilha do Azure

Configure a pilha do Azure para suportar os utilizadores a partir de múltiplos inquilinos do Azure AD para utilizar os serviços na pilha do Azure. Para obter instruções, consulte [ativar vários inquilinos na pilha de Azure](azure-stack-enable-multitenancy.md).


## <a name="create-a-local-resource-in-the-end-customer-tenant-in-azure-stack"></a>Criar um recurso local no inquilino do cliente do fim na pilha do Azure

Depois de adicionar o novo cliente a pilha do Azure ou o inquilino de cliente final tiver ativado a sua conta de convidado com privilégios de proprietário, certifique-se de que pode criar um recurso no seu inquilino. Por exemplo, pode [criar máquina virtual do Windows com o portal do Azure pilha](user\azure-stack-quick-windows-portal.md).

## <a name="next-steps"></a>Passos Seguintes

 - Para rever as mensagens de erro se estes são acionados do seu processo de registo, consulte [as mensagens de erro do registo de inquilino](azure-stack-csp-ref-infrastructure.md#usage-and-billing-error-codes).
 - Para obter mais informações sobre como obter as informações de utilização de recursos de pilha do Azure, consulte o artigo [utilização e faturação na pilha de Azure](/azure-stack-billing-and-chargeback.md).
 - Para rever como um cliente final pode adicionar, como o CSP, como o Gestor do respetivo pilha do Azure, de inquilino, consulte [ativar um fornecedor de serviços em nuvem gerir a sua subscrição do Azure pilha](user\azure-stack-csp-enable-billing-usage-tracking.md).
