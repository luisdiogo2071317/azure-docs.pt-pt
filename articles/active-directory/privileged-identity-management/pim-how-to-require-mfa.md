---
title: Exigir autenticação multifator para funções de diretório do Azure AD no PIM | Documentos da Microsoft
description: Saiba como exigir autenticação multifator (MFA) para funções de diretório do Azure AD no Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 06/06/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 9777a7d2ed07ea2edf4bea661a5065c808de9f2b
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43188358"
---
# <a name="require-multi-factor-authentication-for-azure-ad-directory-roles-in-pim"></a>Exigir autenticação multifator para funções de diretório do Azure AD no PIM
Recomendamos que exigem autenticação multifator (MFA) para todos os administradores. Isso reduz o risco de um ataque devido a uma palavra-passe comprometida.

Pode exigir que os utilizadores concluam uma submissão da MFA no início de sessão. A mensagem de blogue [MFA para Office 365 e o MFA do Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/) compara o que está incluído nas subscrições do Office e o Azure, com os recursos contidos na oferta do Microsoft Azure multi-factor Authentication.

Também pode exigir que os utilizadores concluam um desafio MFA quando ativam uma função no Azure AD PIM. Dessa forma, se o utilizador não foi concluída uma submissão da MFA quando eles conectado, eles serão solicitados a fazê-lo pelo PIM.

## <a name="requiring-mfa-in-azure-ad-privileged-identity-management"></a>Exigir a MFA no Azure AD Privileged Identity Management
Na gestão de identidades no PIM como um administrador com função privilegiada, poderá ver alertas que recomendamos a MFA para contas com privilégios. Clique no alerta de segurança no dashboard do PIM e um novo painel será aberto com uma lista das contas de administrador que deve exigir a MFA.  Pode exigir a MFA, selecionando a várias funções e, em seguida, clicando a **corrigir** botão, ou pode clicar na elipse junto ao funções individuais e, em seguida, clique nas **corrigir** botão.

> [!IMPORTANT]
> Certo, agora, o MFA do Azure só funciona com trabalho contas profissionais ou escolares, não as contas Microsoft (normalmente, uma conta pessoal que serve para iniciar sessão nos serviços Microsoft, como o Skype, Xbox, Outlook.com, etc.). Por este motivo, qualquer pessoa que usar uma conta Microsoft não pode ser um administrador elegível porque estes não podem utilizar o MFA para ativar as suas funções. Se esses usuários precisam continuar a gerir cargas de trabalho com uma conta Microsoft, elevá-las para os administradores permanentes por agora.
> 
> 

Além disso, pode alterar o requisito de MFA para uma função específica ao clicar no mesmo na secção funções do dashboard do PIM. Em seguida, clique em **definições** no painel de função e, em seguida, selecionar **ativar** em autenticação multifator.

## <a name="how-azure-ad-pim-validates-mfa"></a>Como o Azure AD PIM valida a MFA
Existem duas opções para validar a MFA quando um utilizador ativa a uma função.

A opção mais simples é contar com MFA do Azure para utilizadores que estão a ativar uma função com privilégios. Para tal, verifique primeiro que esses utilizadores são licenciados, se necessário e tenham registado para MFA do Azure. Obter mais informações sobre como fazer isso são na [introdução ao multi-factor Authentication na cloud](../authentication/howto-mfa-getstarted.md). Também é recomendável, mas não obrigatório, que configura o Azure AD para impor a MFA para estes utilizadores quando iniciam sessão. Isto acontece porque as verificações MFA serão feitas pelo Azure AD PIM em si.

Em alternativa, se os utilizadores autenticam no local pode ter o seu fornecedor de identidade a ser responsável pela MFA. Por exemplo, se tiver configurado para exigir a autenticação baseada em smart card antes de aceder ao Azure AD, dos serviços de Federação do AD [proteger recursos da cloud com o Azure multi-factor Authentication e o AD FS](../authentication/howto-mfa-adfs.md) inclui instruções Para configurar o AD FS para enviar afirmações com o Azure AD. Quando um utilizador tenta ativar uma função, o Azure AD PIM aceitará que MFA já foi validado para o utilizador depois de receber as declarações adequadas.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Passos Seguintes

- [Configurar definições de função de diretório do Azure AD no PIM](pim-how-to-change-default-settings.md)
- [Configurar alertas de segurança para funções de diretório do Azure AD no PIM](pim-how-to-configure-security-alerts.md)
