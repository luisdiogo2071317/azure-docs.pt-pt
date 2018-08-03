---
title: Delegar ofertas no Azure Stack | Documentos da Microsoft
description: Saiba como colocar outras pessoas responsável pela criação de ofertas e inscrever utilizadores para.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 157f0207-bddc-42e5-8351-197ec23f9d46
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: brenduns
ms.reviewer: alfredop
ms.openlocfilehash: 161a17f2360767dacc4f418a078c695d7cb8daa7
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39449751"
---
# <a name="delegate-offers-in-azure-stack"></a>Delegar ofertas no Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Como o operador do Azure Stack, muitas vezes, pretende colocar a outras pessoas responsável pela criação de ofertas e inscrever-se os utilizadores. Por exemplo, se for um fornecedor de serviços, poderá revendedores Inscreva-se os clientes e geri-los em seu nome. Ou, se é membro de um grupo de TI central numa empresa, pode querer delegar início de sessão de utilizador até outra a equipe de TI.

Delegação torna mais fácil aceder e gerir mais utilizadores que pode fazer por mesmo. A ilustração seguinte mostra um nível de delegação, mas o Azure Stack oferece suporte a mais de um nível. Fornecedores delegados (DPs) podem delegar a outros fornecedores, até cinco níveis.

![Níveis de delegação](media/azure-stack-delegated-provider/image1.png)

## <a name="understand-delegation-roles-and-steps"></a>Compreender as funções de delegação e os passos

### <a name="delegation-roles"></a>Funções de delegação

As seguintes funções fazem parte da delegação:

* O *operador do Azure Stack* gere a infraestrutura do Azure Stack e cria um modelo de oferta. O operador delega a outras pessoas para fornecer ofertas aos respetivos utilizadores.

* Os operadores do Azure Stack delegados são chamados *delegado fornecedores*. Eles podem pertencer a outras organizações, como a outros utilizadores do Azure Active Directory (Azure AD).

* *Os utilizadores* Inscreva-se as ofertas e utilizá-los para gerir as cargas de trabalho, criação de VMs, armazenamento de dados e assim por diante.

### <a name="delegation-steps"></a>Passos de delegação

Existem dois passos básicos para configurar a delegação:

1. *Criar um fornecedor delegado* inscrevendo-se um utilizador a uma oferta com base num plano que tem apenas o serviço de assinaturas. Os utilizadores que subscrevem esta oferta, em seguida, podem expandir as ofertas e inscrição de utilizadores para as ofertas.

1. *Delegar uma oferta para o fornecedor delegado*. Esta oferta é um modelo para o que o fornecedor de delegado pode oferecer. O fornecedor de delegado pode levar a oferta e oferecê-lo a outros utilizadores.

O gráfico seguinte mostra os passos para configurar a delegação.

![Criar o fornecedor de delegado e ativá-los para a inscrição de utilizadores](media/azure-stack-delegated-provider/image2.png)

**Requisitos do fornecedor delegado**

Para atuar como um fornecedor de delegados, um utilizador tem de estabelecer uma relação com o principal fornecedor através da criação de uma subscrição. Esta subscrição identifica o fornecedor de delegados como tendo o direito de ofertas presentes em nome do fornecedor de principal.

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

1. Selecione **novos** > **inquilino ofertas + planos** > **oferecem**.

    ![Criar uma nova oferta](media/azure-stack-delegated-provider/image5.png)

1. Atribua um nome para a oferta. Este artigo usa **ResellerOffer** como exemplo. Selecione em que baseá-la e, em seguida, selecione a oferta delegada **criar**.

   ![Atribuir um nome](media/azure-stack-delegated-provider/image6.png)

   >[!IMPORTANT]
   >É importante compreender que ao contrário de um operador do Azure Stack, um fornecedor delegado não construir uma oferta de planos base e planos de suplementos. Fornecedores delegados só podem escolher ofertas são delegadas a eles, eles não é possível efetuar alterações a essas ofertas.

1. Fazer com que a oferta pública, selecionando **navegue**e, em seguida **oferece**. Selecione a oferta e, em seguida, selecione **alterar estado**.

1. O fornecedor delegado expõe estas ofertas através do portal de sua própria URL. Estas ofertas são visíveis apenas através do portal do delegado. Para localizar e altere este URL:

    a.  Selecione **navegue** > **mais serviços** > **subscrições**. Em seguida, selecione a subscrição do fornecedor delegado. Por exemplo, **DPSubscription** > **propriedades**.

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

## <a name="multiple-tier-delegation"></a>Delegação de múltiplas camadas

Delegação de múltiplas camadas permite que um fornecedor de delegado delegar a oferta para outras entidades. Por exemplo criar mais profundo revendedor canais where:

* O fornecedor que está a gerir o Azure Stack delega uma oferta para um distribuidor.
* Os delegados de distribuidor para um revendedor.

Para criar várias camadas de delegação de oferta, o fornecedor delegado delega a oferta para o fornecedor seguinte. O processo é o mesmo para o fornecedor de delegado que para o operador do Azure Stack. Para obter mais informações, consulte [operador do Azure Stack cria a oferta delegada](#cloud-operator-creates-the-delegated-offer).

## <a name="next-steps"></a>Passos Seguintes

[Aprovisionar uma VM](azure-stack-provision-vm.md)