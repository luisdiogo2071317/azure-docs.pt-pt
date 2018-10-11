---
title: Delegar ofertas no Azure Stack | Documentos da Microsoft
description: Saiba como colocar outras pessoas responsável pela criação de ofertas e inscrever utilizadores para.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2018
ms.author: sethm
ms.reviewer: alfredop
ms.openlocfilehash: 77819c5592fe8b61ed4e3fcb5f874fc0bf5ca602
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "49077989"
---
# <a name="delegate-offers-in-azure-stack"></a>Delegar ofertas no Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Como o operador do Azure Stack, muitas vezes, pretende colocar a outras pessoas responsável por inscrever-se os utilizadores e a criação de subscrições. Por exemplo, se for um fornecedor de serviços, poderá revendedores Inscreva-se os clientes e geri-los em seu nome. Ou, se é membro de um grupo de TI central numa empresa, pode querer delegar início de sessão de utilizador até outra a equipe de TI.

Delegação torna mais fácil aceder e gerir mais utilizadores que pode fazer por mesmo, conforme mostrado na ilustração seguinte. 

![Níveis de delegação](media/azure-stack-delegated-provider/image1.png)

Com a delegação, o fornecedor delegado gere uma oferta (a oferta delegada) e clientes finais obter subscrições sob essa oferta sem o envolvimento do administrador do sistema. 

## <a name="understand-delegation-roles-and-steps"></a>Compreender as funções de delegação e os passos

### <a name="delegation-roles"></a>Funções de delegação

As seguintes funções fazem parte da delegação:

* O *operador do Azure Stack* gere a infraestrutura do Azure Stack e cria um modelo de oferta. O operador delega a outras pessoas para fornecer ofertas para o respetivo inquilino.

* Os operadores do Azure Stack delegados são os utilizadores com *proprietário* ou *contribuinte* direitos nas subscrições chamado *delegada fornecedores*. Eles podem pertencer a outras organizações, como outros inquilinos do Azure Active Directory (Azure AD).

* *Os utilizadores* Inscreva-se as ofertas e utilizá-los para gerir as cargas de trabalho, criação de VMs, armazenamento de dados e assim por diante.

### <a name="delegation-steps"></a>Passos de delegação

Existem dois passos básicos para configurar a delegação:

1. *Criar uma subscrição do fornecedor delegado* inscrevendo-se um utilizador a uma oferta que contém apenas o serviço de assinaturas. Os utilizadores que subscrevem esta oferta, em seguida, podem expandir as ofertas delegadas a outros utilizadores inscrevendo-os para essas ofertas.

2. *Delegar uma oferta para o fornecedor delegado*. Esta oferta permite que o fornecedor delegado criar subscrições ou expandir a oferta para seus usuários. O fornecedor de delegado pode levar a oferta e oferecê-lo a outros utilizadores.

O gráfico seguinte mostra os passos para configurar a delegação.

![Criar o fornecedor de delegado e ativá-los para a inscrição de utilizadores](media/azure-stack-delegated-provider/image2.png)

**Requisitos do fornecedor delegado**

Para atuar como um fornecedor de delegados, um utilizador tem de estabelecer uma relação com o principal fornecedor através da criação de uma subscrição. Esta subscrição identifica o fornecedor de delegados como tendo o direito de apresentar as ofertas de delegado em nome do fornecedor de principal.

Depois desta relação é estabelecida, o operador do Azure Stack pode delegar uma oferta para o fornecedor de delegado. O fornecedor de delegado pode levar a oferta, mude o nome (mas não alterar seu substância) e oferecê-lo aos seus clientes.

## <a name="delegation-walkthrough"></a>Instruções de delegação

As secções seguintes fornecem instruções práticas sobre como configurar um fornecedor de delegados, delegar uma oferta e verificar que os utilizadores podem inscrever-se a oferta delegada.

### <a name="set-up-roles"></a>Configurar funções

Para utilizar estas instruções, tem duas contas do Azure AD para além da sua conta de operador do Azure Stack. Se não tiver estas duas contas, terá de criá-los. As contas podem pertencer a qualquer utilizador do Azure AD e são referidas como o fornecedor de delegado e o utilizador.

| **Função** | **Direitos organizacionais** |
| --- | --- |
| Fornecedor delegado |Utilizador |
| Utilizador |Utilizador |

### <a name="identify-the-delegated-provider"></a>Identificar o fornecedor delegado

1. Inicie sessão no portal do administrador como um operador do Azure Stack.

1. Para criar uma oferta que permite que um utilizador para se tornar um fornecedor de delegado:

   a.  [Criar um plano](azure-stack-create-plan.md).
       Este plano deve incluir apenas o serviço de assinaturas. Este artigo utiliza um plano com o nome **PlanForDelegation** como exemplo.

   b.  [Criar uma oferta](azure-stack-create-offer.md) com base neste plano. Este artigo utiliza uma oferta com o nome **OfferToDP** como exemplo.

   c.  Adicionar o fornecedor de delegados como um subscritor para esta oferta, selecionando **subscrições** > **Add** > **nova subscrição de inquilino**.

   ![Adicionar o fornecedor de delegados como um subscritor](media/azure-stack-delegated-provider/image3.png)

   > [!NOTE]
   > Com todas as ofertas do Azure Stack, que tem a opção de fazer com que a oferta públicos e permitindo utilizadores inscrever-se, ou mantê-lo privado e permitindo que o operador do Azure Stack, gerir a inscrição. Fornecedores delegados são, normalmente, um pequeno grupo. Deseja controlar quem admitiram a ele, então manter esta oferta privada faz sentido na maioria dos casos.

### <a name="azure-stack-operator-creates-the-delegated-offer"></a>Operador de pilha do Azure cria a oferta delegada

A próxima etapa é criar o plano e uma oferta que pretende delegar e que irão utilizar os seus utilizadores. É uma boa idéia definir esta oferta, exatamente como pretende que os utilizadores para vê-la porque o fornecedor delegado não é possível alterar os planos e quotas inclui.

1. Como um operador do Azure Stack [criar um plano](azure-stack-create-plan.md) e [uma oferta](azure-stack-create-offer.md) com base no plano. Este artigo utiliza uma oferta com o nome **DelegatedOffer** como exemplo.

   > [!NOTE]
   > Esta oferta não precisa ser público, mas pode torná-lo públicas se desejar. No entanto, na maioria dos casos quiser apenas fornecedores delegados para ter acesso a oferta. Depois de delegar uma oferta privada conforme descrito nos passos seguintes, o fornecedor de delegado tem acesso ao mesmo.

1. Delegar a oferta. Aceda a **DelegatedOffer**. Sob **configurações**, selecione **fornecedores delegados** > **adicionar**.

1. Selecione a subscrição para o fornecedor de delegado na lista pendente e, em seguida, selecione **delegado**.

   ![Adicionar um fornecedor delegado](media/azure-stack-delegated-provider/image4.png)

### <a name="delegated-provider-customizes-the-offer"></a>Fornecedor delegado personaliza a oferta

Inicie sessão no portal de utilizador como o fornecedor de delegado e, em seguida, criar uma nova oferta com a oferta delegada como modelo.

1. Selecione **+ criar um recurso** > **inquilino ofertas + planos** > **oferecem**.

    ![Criar uma nova oferta](media/azure-stack-delegated-provider/image5.png)

1. Atribua um nome para a oferta. Este artigo usa **ResellerOffer** como exemplo. Selecione em que baseá-la e, em seguida, selecione a oferta delegada **criar**.

   ![Atribuir um nome](media/azure-stack-delegated-provider/image6.png)

   >[!IMPORTANT]
   >É importante compreender que Fornecedores delegados apenas podem escolher ofertas são delegadas a eles. Eles não podem fazer alterações para essas ofertas. Apenas um operador do Azure Stack pode alterar estas ofertas, por exemplo, alterar seus planos e quotas. Um fornecedor delegado não construir uma oferta de planos base e planos de suplementos. 

3. O fornecedor de delegado pode fazer com que estas ofertas público através do portal de sua própria URL. Para tornar a oferta pública, selecione **navegue**e, em seguida **oferece**. Selecione a oferta e, em seguida, selecione **alterar estado**.

4. As ofertas delegadas públicas agora estão visíveis apenas através do portal do delegado. Para localizar e altere este URL:

    a.  Selecione **navegue** > **todos os serviços**e, no **geral** categoria, selecione **subscrições**. Selecione a subscrição do fornecedor delegado. Por exemplo, **DPSubscription** > **propriedades**.

    b.  Copie o portal de URL para uma localização separada, como o bloco de notas.

    ![Selecione a subscrição do fornecedor delegado](media/azure-stack-delegated-provider/dpportaluri.png)  

   Concluir a criação de uma oferta delegada como um fornecedor de delegado. Terminar sessão como o fornecedor de delegado e feche a janela do browser que está a utilizar.

### <a name="sign-up-for-the-offer"></a>Inscreva-se para a oferta

1. Numa nova janela do browser, aceda ao portal do delegado URL que guardou no passo anterior. Inicie sessão no portal, como um utilizador.

   >[!NOTE]
   >As ofertas delegadas não estão visíveis, a menos que utilize o portal do delegado.

1. No dashboard, selecione **obter uma subscrição**. Verá que apenas as ofertas delegadas que foram criadas pelo fornecedor de delegados são apresentadas ao utilizador.

   ![Ver e selecionar ofertas](media/azure-stack-delegated-provider/image8.png)

O processo indesejado de delegar uma oferta é concluído. Agora um utilizador pode inscrever-se para esta oferta ao obter uma subscrição para o mesmo.

## <a name="move-subscriptions-between-delegated-providers"></a>Mover subscrições entre fornecedores delegados

Se for necessário, uma subscrição pode ser movida entre subscrições do fornecedor delegado novo ou existente que pertencem ao mesmo inquilino do diretório. Isso é usando o cmdlet do PowerShell [movimentação AzsSubscription](https://docs.microsoft.com/powershell/module/azs.subscriptions.admin).

Isso é útil quando:
- Carregar um novo membro da Equipe que irá assumir a função de fornecedor delegado e pretende atribuir a esta equipa membro-subscrições do utilizador que foram anteriormente criadas na subscrição do fornecedor predefinido.
- Tem várias subscrições de fornecedores delegados no mesmo diretório-inquilino (Azure Active Directory) e precisa de mover subscrições do utilizador entre eles. Isto pode ser o caso em que um membro da equipe se move entre equipes e a subscrição tem de ser alocada para o novo agrupamento.


## <a name="next-steps"></a>Passos Seguintes

[Aprovisionar uma VM](azure-stack-provision-vm.md)