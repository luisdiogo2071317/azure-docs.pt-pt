---
title: Gerir a utilização e faturação para o Azure Stack como fornecedor de serviços Cloud | Documentos da Microsoft
description: Uma passagem de registar o Azure Stack como um fornecedor de Cloud (CSP) e adicionando os clientes para faturação.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2018
ms.author: brenduns
ms.reviewer: alfredo
ms.openlocfilehash: 9bb4a4ea81f2dc0fb11e2f7cae1b9d02b0edfdde
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43341440"
---
# <a name="manage-usage-and-billing-for-azure-stack-as-a-cloud-service-provider"></a>Gerir a utilização e faturação para o Azure Stack como fornecedor de serviços Cloud 

*Aplica-se a: sistemas integrados do Azure Stack*

Este artigo descreve a registar o Azure Stack como um fornecedor de Cloud (CSP) e adicionar os clientes.

Como um CSP, trabalhar com diversos clientes através do Azure Stack. Cada cliente tem uma subscrição do CSP no Azure. Precisará direcionar a utilização a partir do Azure Stack para cada subscrição do utilizador.

O diagrama seguinte mostra os passos que terá de escolher a sua conta de serviços compartilhados e registar a conta do Azure com a conta do Azure Stack. Registrado, pode carregar o cliente final.

**Passos para adicionar controlo como um CSP de utilização**

![Processo para ativar a utilização e gestão como um fornecedor de serviços Cloud.](media\azure-stack-add-manage-billing-as-a-csp\process-add-useage-as-a-csp.png)

## <a name="create-a-csp-or-cspss-subscription"></a>Criar uma subscrição do CSP ou CSPSS

### <a name="cloud-service-provider-subscription-types"></a>Tipos de subscrição do fornecedor de serviços cloud

Terá de escolher o tipo de conta de serviços compartilhados que utilizar para o Azure Stack. Os tipos de subscrições que podem ser utilizadas para registo de um multi-inquilino do Azure Stack são:

 - Fornecedor de serviços cloud 
 - Subscrição de serviços compartilhados do parceiro 

#### <a name="csp-shared-services"></a>Serviços compartilhados de CSP

Subscrições de serviços partilhados fornecedor Service (CSPSS) da cloud são a opção preferencial para o registo quando um CSP direto ou um distribuidor de CSP opera o Azure Stack.

Subscrições de CSPSS estão associadas um inquilino de serviços compartilhados. Quando registar o Azure Stack, terá de fornecer credenciais para uma conta que seja o proprietário da subscrição. A conta que utiliza para registar o Azure Stack pode ser diferente da conta de administrador que utiliza para a implementação. Além disso, as duas contas fazer *não* têm de pertencer ao mesmo domínio. Em outras palavras, pode implementar a utilizar o inquilino que já utilizam. Por exemplo pode utilizar ContosoCSP.onmicrosoft.com, em seguida, registe-se de que a utilizar um inquilino diferente, por exemplo IURContosoCSP.onmicrosoft.com. Precisará lembrar-se de que entrar usando o ContosoCSP.onmicrosoft.com ao fazê-lo a administração do dia para fazer do Azure Stack. Quando iniciar sessão no Azure com IURContosoCSP.onmicrosoft.com quando precisa fazer operações de registo.

Consulte o seguinte para obter uma descrição das subscrições de CSPSS e instruções sobre como criar a subscrição [adicionar Azure Partner serviços compartilhados](https://msdn.microsoft.com/partner-center/shared-services).

#### <a name="csp-subscriptions"></a>Subscrições de CSP

Subscrições do fornecedor de serviços (CSP) de cloud são a opção preferencial para o registo quando um revendedor CSP ou um cliente final opera o Azure Stack.

## <a name="register-azure-stack"></a>Registar o Azure Stack

Utilize a subscrição de CSPSS criada seguindo as informações na secção anterior para registar o Azure Stack com o Azure. Para obter mais informações, consulte [registar o Azure Stack com a sua subscrição do Azure](azure-stack-registration.md).

## <a name="add-end-customer"></a>Adicionar cliente final

Para configurar o Azure Stack, de modo que quando um novo inquilino utiliza recursos de seu uso será reportado à respetiva subscrição do fornecedor de serviços Cloud (CSP), consulte [adicionar inquilino para utilização e faturação no Azure Stack](azure-stack-csp-howto-register-tenants.md).

## <a name="charge-the-right-subscriptions"></a>Cobrar as subscrições certas

O Azure Stack utiliza um recurso chamado de registo. Um registo é um objeto armazenado no Azure. O objeto de registo documenta as subscrições do Azure para utilizar a cobrar um determinado Azure Stack. Esta seção aborda a importância de registo.

Com o registo do Azure Stack pode:
 - Reencaminhar dados de utilização do Azure Stack para o Azure Commerce e uma subscrição do Azure são faturadas.
 - Comunique a utilização de cada um dos clientes numa subscrição diferente com uma implementação multi-inquilino do Azure Stack. Arquitetura "multitenancy" permite que o Azure Stack oferecer suporte a diferentes organizações na mesma instância do Azure Stack.

Para cada Azure Stack, existe uma subscrição predefinida e muitos inquilinos subscrições. A assinatura padrão é uma subscrição do Azure que é cobrada se não existir uma subscrição de inquilino específico. Tem de ser o primeiro a subscrição registada. Para trabalhar de relatórios de utilização da multi-inquilino, a subscrição tem de ser uma subscrição do CSP ou CSPSS.

Em seguida, o registo é atualizado com uma subscrição do Azure para cada inquilino que irá utilizar o Azure Stack. Subscrições de inquilino tem de ser do tipo CSP e tem de agregação para o parceiro que detém a assinatura padrão. Em outras palavras, não é possível registar os clientes de outra pessoa.

Quando o Azure Stack reencaminha as informações de utilização para o global Azure, um serviço do Azure o registo de atua como consultor e utilização de cada inquilino é mapeado para a subscrição do inquilino adequado. Se não tiver sido registado um inquilino, que a utilização de vai para a subscrição predefinida para a instância do Azure Stack a partir do qual foi gerado.

Uma vez que as subscrições de inquilino são subscrições de CSP, sua fatura é enviada para o parceiro CSP e informações de utilização não são visíveis para o cliente final.

## <a name="next-steps"></a>Passos Seguintes

 - Para saber mais sobre o programa CSP, veja [programa Cloud Solution Provider](https://partner.microsoft.com/solutions/microsoft-cloud-solutions).
 - Para saber mais sobre como recuperar informações de utilização de recursos do Azure Stack, veja [utilização e faturação no Azure Stack](azure-stack-billing-and-chargeback.md).
