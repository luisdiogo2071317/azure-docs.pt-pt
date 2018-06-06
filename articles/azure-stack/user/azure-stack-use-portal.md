---
title: Através do portal do Azure pilha | Microsoft Docs
description: Saiba como aceder e utilizar o portal de utilizador na pilha do Azure.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2018
ms.author: mabrigg
ms.reviewer: efemmano
ms.openlocfilehash: 7ca29ee359349f69c3d5ff21bd9db3f93358206a
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34724317"
---
# <a name="use-the-azure-stack-portal"></a>Utilizar o portal do Azure Stack

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pode utilizar o portal do Azure pilha para subscrever ofertas públicas e utilizar os serviços que fornecem estas ofertas. Se utilizou o portal do Azure global, já estiver familiarizado com o funcionamento do site.

## <a name="access-the-portal"></a>Aceder ao portal

O operador de pilha do Azure (um fornecedor de serviços ou um administrador na sua organização), permitirá sabe o URL correto para aceder ao portal.

- Para um sistema integrado, o URL varia com base na região e nome de domínio externo o operador e vai estar no formato https://portal.&lt; *região*&gt;.&lt; *FQDN*&gt;.
- Se estiver a utilizar o Kit de desenvolvimento de pilha do Azure, o endereço de portal é https://portal.local.azurestack.external.

![Captura de ecrã do portal de utilizador de pilha do Azure](media/azure-stack-use-portal/UserPortal.png)

## <a name="customize-the-dashboard"></a>Personalizar o dashboard

O dashboard contém um conjunto predefinido de mosaicos. Pode selecionar **editar dashboard** para modificar o dashboard predefinido ou selecione **novo dashboard** para criar um dashboard personalizado. Pode facilmente personalizar um dashboard ao adicionar ou remover os mosaicos. Por exemplo, para adicionar um mosaico de computação, selecione **novo**. Clique com botão direito **computação**e, em seguida, selecione **afixar ao dashboard**.

## <a name="create-subscription-and-browse-available-resources"></a>Criar subscrição e a procurar recursos disponíveis

Se ainda não tiver uma subscrição, a primeira coisa que precisa de fazer é subscrever uma oferta. Depois disso, pode procurar os recursos disponíveis. Para procurar e criar recursos, utilize uma das abordagens seguintes:

- Selecione o **Marketplace** mosaico no dashboard.
- No **todos os recursos** mosaico, selecione **criar recursos**.
- No painel de navegação esquerdo, selecione **novo**.

## <a name="learn-how-to-use-available-services"></a>Saiba como utilizar os serviços disponíveis

Se necessitar de orientação sobre como utilizar os serviços disponíveis, poderão ocorrer diferentes opções disponíveis para si.

- Sua organização ou o fornecedor de serviços pode fornecer as suas próprias documentação, que é normalmente o caso, se oferecem serviços personalizados ou aplicações.
- Aplicações de terceiros tem sua própria documentação.
- Para os serviços do Azure consistente, recomendamos vivamente que reveja a documentação de pilha do Azure pela primeira vez. Para aceder à documentação do utilizador a pilha do Azure, selecione o ícone de ajuda e, em seguida, selecione **ajuda + suporte**.

    ![Ajuda e suporte opção na IU](media/azure-stack-use-portal/HelpAndSupport.png)

    Em particular, sugerimos que reveja os artigos seguintes para começar a utilizar:

    - [Considerações da chave: utilizar os serviços ou na criação de aplicações para a pilha do Azure](azure-stack-considerations.md)
    - No **utilizar serviços** secção de documentação, há um artigo de considerações para cada serviço. A página de considerações descreve as diferenças entre o serviço disponibilizadas no Azure e o mesmo serviço oferecido na pilha do Azure. Por exemplo, consulte [considerações de VM](azure-stack-vm-considerations.md). Poderão existir outras informações no **utilizar serviços** secção que seja exclusiva à pilha do Azure.

      Pode utilizar a documentação do Azure como referência geral para um serviço, mas tem de ser informados destas diferenças. Compreender que a documentação contém ligações no **tutoriais de início rápido** mosaico ponto para a documentação do Azure.

## <a name="get-support"></a>Obter suporte

Se precisar de suporte, contacte o fornecedor de serviço ou organização para obter ajuda.

Se estiver a utilizar o Kit de desenvolvimento de pilha do Azure, o [fórum de Azure pilha](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) é a única origem de suporte.

## <a name="next-steps"></a>Passos Seguintes

[Considerações da chave: utilizar os serviços ou na criação de aplicações para a pilha do Azure](azure-stack-considerations.md)
