---
title: Início de sessão experiências com o Azure AD Identity Protection | Microsoft Docs
description: Fornece uma descrição geral da experiência do utilizador quando Identity Protection tem atenuados ou remediado um utilizador ou quando a autenticação multifator é requerida por uma política.
services: active-directory
keywords: proteção de identidade do Azure Active Directory, o cloud app discovery, gestão de aplicações, segurança, risco, nível de risco, vulnerabilidade, política de segurança
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: de5bf637-75a7-4104-b6d8-03686372a319
ms.service: active-directory
ms.component: protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 424ea1eca637653c5e7cfed9dc4b970d5afd91ad
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34713413"
---
# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a>Início de sessão experiências com o Azure AD Identity Protection
Com o Azure Active Directory Identity Protection, pode:

* exigir que os utilizadores para se registar para autenticação multifator
* processar o risco de inícios de sessão e utilizadores comprometidos

A resposta do sistema a estes problemas tem um impacto na experiência de início de sessão do utilizador, porque diretamente iniciar sessão, fornecendo um nome de utilizador e uma palavra-passe não será possível já. São necessários passos adicionais para obter um utilizador em segurança para o negócio.

Este artigo fornece uma descrição geral da experiência de início de sessão do utilizador para todos os casos que podem ocorrer.

**Multi-Factor Authentication**

* Registo de autenticação multifator

**Início de sessão em risco**

* Recuperação de risco início de sessão
* Risco início de sessão bloqueado
* Registo de autenticação multifator durante um risco início de sessão

**Utilizador em risco**

* Recuperação de conta comprometida
* Conta comprometida bloqueada

## <a name="multi-factor-authentication-registration"></a>Registo de autenticação multifator
A melhor experiência de utilizador para ambos, o fluxo de recuperação de conta comprometida e arriscado fluxo início de sessão, é quando o utilizador Self-pode recuperar. Se os utilizadores estão registados para autenticação multifator, já tem um número de telefone associado à sua conta que pode ser utilizada para passar os desafios de segurança. Não é necessário nenhum envolvimento de suporte técnico ou o administrador de ajuda para recuperar de compromisso de conta. Assim, é altamente recomendado que para incentivar os utilizadores registados para autenticação multifator. 

Os administradores podem definir uma política que requer que os utilizadores configurar as contas para verificação de segurança adicional. Esta política permite aos utilizadores ignorar o registo de autenticação multifator para 14 dias. O período de tolerância de 14 dias não é configurável.

**O registo de autenticação multifator tem três passos:**

1. No primeiro passo, o utilizador obtém uma notificação sobre o requisito para definir a conta para multi-factor authentication. 
   
    ![Remediação](./media/active-directory-identityprotection-flows/140.png "remediação")
2. Para configurar a autenticação multifator, tem de permitir que o sistema saber como pretende ser contactado.
   
    ![Remediação](./media/active-directory-identityprotection-flows/141.png "remediação")
3. O sistema submete a um desafio e tem de responder.
   
    ![Remediação](./media/active-directory-identityprotection-flows/142.png "remediação")

## <a name="risky-sign-in-recovery"></a>Recuperação de risco início de sessão
Quando um administrador tiver configurado uma política relativamente a riscos de início de sessão, os utilizadores afetados são notificados quando tentarem iniciar sessão. 

**O fluxo de início de sessão arriscado tem dois passos:** 

1. O utilizador é informado atividade invulgar foi detetado sobre o seu início de sessão, tais como iniciar sessão a partir de uma nova localização, dispositivo ou aplicação. 
   
    ![Remediação](./media/active-directory-identityprotection-flows/120.png "remediação")
2. O utilizador é necessário para provar a sua identidade por resolver um desafio de segurança. Se o utilizador está registado para autenticação multifator precisam de arredondar trip um código de segurança para o número de telefone. Uma vez que esta apenas um risco início de sessão e não uma conta comprometida, o utilizador não terá de alterar a palavra-passe neste fluxo. 
   
    ![Remediação](./media/active-directory-identityprotection-flows/121.png "remediação")

## <a name="risky-sign-in-blocked"></a>Risco início de sessão bloqueado
Os administradores também podem optar por definir uma política de início de sessão risco para impedir que os utilizadores após o início de sessão consoante o nível de risco. Para obter desbloqueado, os utilizadores finais tem de contactar um administrador ou de ajuda para o suporte técnico ou, pode tentar iniciar sessão a partir de um dispositivo ou localização familiar. Self-recuperação ao resolver a autenticação multifator não é uma opção neste caso.

![Remediação](./media/active-directory-identityprotection-flows/200.png "remediação")

## <a name="compromised-account-recovery"></a>Recuperação de conta comprometida
Quando tiver sido configurada uma política de segurança de risco do utilizador, os utilizadores que cumprem o utilizador nível especificado na política de risco (e, por isso, são considerados comprometidos) deve seguir o fluxo de recuperação de comprometimento de utilizador antes de poderem iniciar sessão. 

**O fluxo de recuperação de comprometimento de utilizador tem três passos:**

1. O utilizador é informado que a segurança da conta está em risco devido a atividade suspeita ou fuga credenciais.
   
    ![Remediação](./media/active-directory-identityprotection-flows/101.png "remediação")
2. O utilizador é necessário para provar a sua identidade por resolver um desafio de segurança. Se o utilizador está registado para autenticação multifator personalizada pode recuperar de ser comprometida. Precisam de arredondar trip um código de segurança para o número de telefone. 
   
   ![Remediação](./media/active-directory-identityprotection-flows/110.png "remediação")
3. Por fim, o utilizador é forçado a alterar a palavra-passe, uma vez que outra pessoa poderá ter tido acesso à respetiva conta. 
   Capturas de ecrã desta experiência são abaixo.
   
   ![Remediação](./media/active-directory-identityprotection-flows/111.png "remediação")

## <a name="compromised-account-blocked"></a>Conta comprometida bloqueada
Para obter um utilizador que foi bloqueado por uma política de segurança do utilizador risco desbloqueada, o utilizador tem de contactar um administrador ou suporte técnico. Self-recuperação ao resolver a autenticação multifator não é uma opção neste caso.

![Remediação](./media/active-directory-identityprotection-flows/104.png "remediação")

## <a name="reset-password"></a>Repor palavra-passe
Se os utilizadores comprometidos estão bloqueados no início de sessão, um administrador pode gerar uma palavra-passe temporária para os mesmos. Os utilizadores terão de alterar a palavra-passe durante um próximo início de sessão.

![Remediação](./media/active-directory-identityprotection-flows/160.png "remediação")

## <a name="see-also"></a>Consulte também
* [Proteção de identidade do Azure Active Directory](active-directory-identityprotection.md) 

