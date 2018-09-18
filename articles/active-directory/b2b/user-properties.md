---
title: Propriedades de um utilizador de colaboração do Azure Active Directory B2B | Documentos da Microsoft
description: Propriedades de utilizador de colaboração do Azure Active Directory B2B são configuráveis
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 5f999a17cd375a3338aa936e2f405c36f6021ebc
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "45984816"
---
# <a name="properties-of-an-azure-active-directory-b2b-collaboration-user"></a>Propriedades de um utilizador de colaboração do Azure Active Directory B2B

Um utilizador de colaboração do Azure Active Directory (Azure AD) empresa-empresa (B2B) é um utilizador com UserType = convidado. Normalmente, este utilizador convidado é de uma organização parceira e tem os privilégios no diretório do convite, limitados por predefinição.

Consoante as necessidades da organização de convite, um utilizador de colaboração B2B do Azure AD pode ter um dos seguintes Estados de conta:

- Estado de 1: Que se encontrem numa instância externa do Azure AD e representada como um utilizador convidado na organização de convite. Neste caso, o utilizador B2B inicia sessão utilizando uma conta do Azure AD que pertença ao inquilino de convidado. Se a organização de parceiro não utilizar o Azure AD, o utilizador convidado no Azure AD ainda é criado. Os requisitos são que eles resgatar o convite e do Azure AD verifica o respetivo endereço de e-mail. Essa organização também é denominada um inquilinos de (JIT) just-in-time ou um "viral" inquilinos.

- Estado 2: Que se encontrem numa conta Microsoft e representada como um utilizador convidado na organização do anfitrião. Neste caso, o utilizador convidado inicia sessão com uma conta Microsoft. Identidade de redes sociais do utilizador convidado (google.com ou semelhante), que não é uma conta Microsoft, é criado como uma conta Microsoft durante o resgate de oferta.

- Estado de 3: Que se encontrem no Active Directory no local da organização de anfitrião e sincronizados com o Azure da organização de anfitrião AD. Durante este lançamento, tem de utilizar o PowerShell para alterar manualmente o UserType de tais usuários na cloud.

- Estado de 4: Que se encontrem no Azure da organização do host AD com UserType = convidado e as credenciais que gere a organização de anfitrião.

  ![Exibindo iniciais na máquina](media/user-properties/redemption-diagram.png)


Agora, vamos ver o aspeto de um utilizador de colaboração B2B do Azure AD no estado 1 no Azure AD.

### <a name="before-invitation-redemption"></a>Antes de resgate de convite

![Antes de resgate da oferta](media/user-properties/before-redemption.png)

### <a name="after-invitation-redemption"></a>Após o resgate de convite

![Após o resgate de oferta](media/user-properties/after-redemption.png)

## <a name="key-properties-of-the-azure-ad-b2b-collaboration-user"></a>Principais propriedades de utilizador de colaboração B2B do Azure AD
### <a name="usertype"></a>userType
Esta propriedade indica a relação do utilizador para os inquilinos do anfitrião. Esta propriedade pode ter dois valores:
- Membro: Este valor indica um funcionário da organização anfitrião e um utilizador na folha de pagamento da organização. Por exemplo, este utilizador espera que tenham acesso aos sites de apenas internos. Este utilizador não deve ser considerado um funcionário externo.

- Convidado: Este valor indica que um utilizador que não seja considerado interno para a empresa, como um funcionário externo, parceiro, cliente ou utilizador semelhante. Esse usuário não espera receber memorando interno de um CEO ou receber os benefícios da empresa, por exemplo.

  > [!NOTE]
  > O UserType tem sem relação com a forma como o utilizador inicia sessão, a função de diretório do utilizador e assim por diante. Esta propriedade simplesmente indica a relação do utilizador para a organização de anfitrião e permite que a organização impor políticas que dependem desta propriedade.

### <a name="source"></a>Origem
Esta propriedade indica como o utilizador inicia sessão.

- Convidado utilizador: Este utilizador foi convidado mas ainda não tiver resgatado um convite.

- Externo do Active Directory: Este utilizador está hospedado numa organização externa e autentica utilizando uma conta do Azure AD que pertence a outra organização. Este tipo de início de sessão corresponde a 1 de estado.

- Conta Microsoft: este utilizador está hospedado numa conta Microsoft e é autenticado ao utilizar uma conta Microsoft. Este tipo de início de sessão corresponde a estado 2.

- Windows Server Active Directory: Este utilizador é iniciou sessão a partir do Active Directory no local e que pertence a esta organização. Este tipo de início de sessão corresponde a 3 de estado.

- O Azure Active Directory: Este utilizador é autenticado com uma conta do Azure AD que pertence a esta organização. Este tipo de início de sessão corresponde a 4 de estado.
  > [!NOTE]
  > Origem e UserType são propriedades independentes. Um valor de origem não implica um valor específico para UserType.

## <a name="can-azure-ad-b2b-users-be-added-as-members-instead-of-guests"></a>Utilizadores B2B do Azure AD podem ser adicionados como membros em vez de convidados?
Normalmente, um utilizador do Azure AD B2B e o utilizador convidado são sinônimos. Por conseguinte, um utilizador de colaboração B2B do Azure AD é adicionado como um utilizador com UserType = convidados por predefinição. No entanto, em alguns casos, a organização do parceiro é um membro de uma grande organização a que a organização de anfitrião também pertence. Se assim for, a organização de anfitrião possa querer tratar os utilizadores na organização do parceiro como membros em vez de convidados. Utilize as APIs do Azure AD B2B convite Manager para adicionar ou Convide um utilizador da organização parceira para a organização do anfitrião como membro.

## <a name="filter-for-guest-users-in-the-directory"></a>Filtro para os utilizadores convidados no diretório

![Filtrar utilizadores convidados](media/user-properties/filter-guest-users.png)

## <a name="convert-usertype"></a>Converter UserType
Atualmente, é possível que os utilizadores converter UserType de membro para o convidado e vice-versa, com o PowerShell. No entanto, a propriedade UserType é suposto para representar a relação do utilizador para a organização. Por conseguinte, o valor desta propriedade deve alterar apenas se altera a relação do utilizador para a organização. Se mudar a relação do utilizador, devem problemas, como se deve alterar o nome principal de utilizador (UPN), ser resolvidos? O utilizador deve continuar a ter acesso aos mesmos recursos? Deve ser atribuída uma caixa de correio? Por conseguinte, não recomendamos que altere o UserType com o PowerShell como uma atividade atômica. Além disso, no caso desta propriedade se torna imutável com o PowerShell, não recomendamos tendo uma dependência nesse valor.

## <a name="remove-guest-user-limitations"></a>Remover limitações de utilizador convidado
Pode haver casos em que pretende atribuir aos seus usuários de convidado privilégios mais altos. Pode adicionar um utilizador convidado a qualquer função e até remover as restrições de utilizador de convidado predefinidas no diretório para dar os mesmos privilégios como membros de um utilizador.

É possível desativar as limitações de utilizador convidado padrão, para que um utilizador convidado no diretório da empresa é dado as mesmas permissões que um utilizador de membro.

![Remover limitações de utilizador convidado](media/user-properties/remove-guest-limitations.png)

## <a name="can-i-make-guest-users-visible-in-the-exchange-global-address-list"></a>Posso fazer os utilizadores convidados visível na lista de endereços Global do Exchange?
Sim. Por predefinição, os objetos de convidado não são visíveis na lista de endereços global da sua organização, mas pode utilizar o Azure Active Directory PowerShell para que fiquem visíveis. Para obter detalhes, consulte **posso fazer objetos de convidado visível na lista de endereços global?** na [acesso de convidado em grupos do Office 365](https://support.office.com/article/guest-access-in-office-365-groups-bfc7a840-868f-4fd6-a390-f347bf51aff6#PickTab=FAQ). 

## <a name="next-steps"></a>Passos Seguintes

* [O que é a colaboração B2B do Azure AD?](what-is-b2b.md)
* [Tokens de utilizador de colaboração B2B](user-token.md)
* [Mapeamento de afirmações de utilizador de colaboração de B2B](claims-mapping.md)
