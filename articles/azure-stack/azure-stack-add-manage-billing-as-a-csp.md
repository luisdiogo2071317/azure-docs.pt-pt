---
title: Gerir a utilização e faturação de pilha do Azure como um fornecedor de serviços na nuvem | Microsoft Docs
description: Uma movimentação pela através de registar a pilha do Azure como um fornecedor de nuvem e adicionar os clientes.
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
ms.date: 02/27/2018
ms.author: mabrigg
ms.reviewer: alfredo
ms.openlocfilehash: 21a52af4943004789b0a9bdbe4695ab1a603c046
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34796704"
---
# <a name="manage-usage-and-billing-for-azure-stack-as-a-cloud-service-provider"></a>Gerir a utilização e faturação de pilha do Azure como um fornecedor de serviços Cloud 

*Aplica-se a: Azure pilha integrado sistemas*

Este artigo explica-lhe como registar pilha do Azure como um fornecedor de nuvem (CSP) e adicionar os clientes.

Como um CSP, que é provável que têm muitos clientes diferentes com a pilha do Azure. Cada cliente tem uma subscrição do CSP no Azure e precisa de direcionar a utilização do seu pilha do Azure para a subscrição de cada utilizador.

O diagrama seguinte mostra os passos que precisa para escolher a sua conta de serviços partilhados e registar a conta do azure com a conta. Quando isto foi feito, pode carregar os seus clientes finais.

**Passos para adicionar a utilização de controlo como um CSP**

![Processo para ativar a gestão como um fornecedor de serviço em nuvem e de utilização.](media\azure-stack-add-manage-billing-as-a-csp\process-add-useage-as-a-csp.png)

## <a name="create-a-csp-or-cspss-subscription"></a>Criar uma subscrição do CSP ou CSPSS

### <a name="cloud-service-provider-subscription-types"></a>Tipos de subscrição do fornecedor de serviços cloud

Terá de escolher o tipo de conta de serviços partilhados que utiliza para a pilha do Azure. Os tipos de subscrições que podem ser utilizadas para o registo de uma pilha de Azure multi-inquilino são:

 - Fornecedor de serviços cloud 
 - Subscrição de serviços partilhados de parceiro 

#### <a name="csp-shared-services"></a>CSP partilhado serviços

Subscrições de serviços partilhados fornecedor serviço (CSPSS) na nuvem são a opção preferencial para o registo quando um CSP direta ou distribuidor CSP funciona pilha do Azure.

Subscrições de CSPSS estão associadas um inquilino de serviços partilhados. Quando registar pilha do Azure, terá de fornecer credenciais para uma conta que é proprietário da subscrição. A conta que utiliza para registar a pilha do Azure pode ser diferente da conta de administrador que utiliza para implementação; Efetue os dois *não* têm de pertencer ao mesmo domínio. Por outras palavras, pode implementar utilizando o inquilino que já utilizam. Por exemplo poderá utilizar ContosoCSP.onmicrosoft.com, em seguida, registe a utilização de um inquilino diferente, por exemplo IURContosoCSP.onmicrosoft.com. Terá de Lembre-se de que inicie sessão utilizando ContosoCSP.onmicrosoft.com quando fizer a administração de pilha do Azure do dia para escolha. Quando inicia sessão Azure utilizando IURContosoCSP.onmicrosoft.com quando precisar de efetuar operações de registo.

Consulte o seguinte para obter uma descrição das subscrições CSPSS e instruções sobre como criar subscrição [adicionar serviços do Azure parceiro partilhado](https://msdn.microsoft.com/partner-center/shared-services).

#### <a name="csp-subscriptions"></a>Subscrições de CSP

Subscrições de fornecedor de serviços (CSP) da nuvem são a opção preferencial para o registo quando um revendedor CSP ou um cliente do fim funciona pilha do Azure.

## <a name="register-azure-stack"></a>Registar a pilha do Azure

Para registar a pilha do Azure, consulte [registar a pilha do Azure com a sua subscrição do Azure](azure-stack-registration.md).

## <a name="add-end-customer"></a>Adicionar cliente de fim

Para configurar a pilha do Azure para que quando um novo inquilino utiliza recursos respetiva utilização será reportada à respetiva subscrição do fornecedor de serviços em nuvem (CSP), consulte [adicionar o inquilino para a utilização e faturação a pilha de Azure](azure-stack-csp-howto-register-tenants.md).

## <a name="charge-the-right-subscriptions"></a>Cobram as subscrições à direita

Pilha do Azure utiliza uma funcionalidade chamada de registo. Um registo é um objeto, armazenado no Azure, o que documentos que subscrições do Azure a utilizar para cobram para um determinado pilha do Azure. Esta secção aborda a importância de registo.

Pilha do Azure utilizando o registo pode:
 - Reencaminhar dados de utilização de pilha do Azure para o Azure comércio e cobrar uma subscrição do Azure.
 - Utilização de cada cliente de relatório em diferentes subscrições com uma implementação multi-inquilino de pilha do Azure. Arquitetura "multitenancy" permite que a pilha do Azure suportar a diferentes organizações na mesma instância de pilha do Azure.

Para cada pilha do Azure, é uma subscrição predefinida e como as subscrições de inquilino conforme necessário. A subscrição predefinida é uma subscrição do Azure que é cobrada se não houver nenhuma subscrição de inquilino específico. Tem de ser o primeiro a ser registado. Para a utilização de multi-inquilino relatórios funcionem, a subscrição tem de ser uma subscrição do CSP ou CSPSS.

Em seguida, o registo é atualizado com uma subscrição do Azure para cada inquilino que vai utilizar a pilha do Azure. Subscrições de inquilino tem de ser do tipo CSP e tem de agregação para o parceiro de proprietário da subscrição de predefinição. Por outras palavras, não é possível registar os clientes de outra pessoa.

Quando a pilha de Azure reencaminha as informações de utilização para o global Azure, um serviço no Azure consulta o registo e utilização de cada inquilino é mapeado para a subscrição de inquilino adequado. Se não tiver sido registado um inquilino, essa utilização vai para a subscrição predefinida para a instância de pilha do Azure a partir da qual foi originado.

Uma vez que as subscrições de inquilino são subscrições de CSP, os respetivos fatura é enviada para o parceiro de CSP e as informações de utilização não são visíveis para o cliente do fim.



## <a name="next-steps"></a>Passos Seguintes

 - Para saber mais sobre o programa CSP, consulte [programa fornecedor de solução em nuvem](https://partnercenter.microsoft.com/en-us/partner/programs).
 - Para obter mais informações sobre como obter as informações de utilização de recursos de pilha do Azure, consulte o artigo [utilização e faturação na pilha de Azure](azure-stack-billing-and-chargeback.md).
