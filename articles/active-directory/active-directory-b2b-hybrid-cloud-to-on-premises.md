---
title: Os utilizadores do Azure AD B2B conceder acedem às suas aplicações no local | Microsoft Docs
description: Mostra como atribuir nuvem B2B os utilizadores acedam a aplicações no local com a colaboração B2B do Azure AD.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/20/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 028bbb28c7091db3c3ebea321ca2e167b999949d
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/11/2018
ms.locfileid: "34068801"
---
# <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications"></a>Os utilizadores do Azure AD B2B conceder acedem às suas aplicações no local

Como uma organização que utiliza as capacidades de colaboração B2B do Azure Active Directory (Azure AD) para convidar utilizadores convidados de organizações de parceiros ao seu Azure AD, agora pode fornecer estes utilizadores B2B acesso a aplicações no local. Estas aplicações no local podem utilizar a autenticação baseada em SAML ou autenticação integrada de Windows (IWA) com a delegação restringida de Kerberos (KCD).

## <a name="access-to-saml-apps"></a>Acesso a aplicações SAML

Se a sua aplicação no local utiliza autenticação baseada em SAML, pode facilmente tornar estas aplicações disponíveis para os utilizadores de colaboração B2B do Azure AD através do portal do Azure.

Terá de efetuar ambos os procedimentos seguintes:

- Integrar a aplicação SAML utilizando o modelo não Galeria de aplicações, conforme descrito em [configurar início de sessão único para aplicações que não estejam na Galeria de aplicações do Azure Active Directory](active-directory-saas-custom-apps.md). Certifique-se de que tenha em atenção utiliza para o **URL de início de sessão** valor.
-  Utilizar o Proxy de aplicações do Azure AD para publicar a aplicação no local, com **do Azure Active Directory** configurado como origem de autenticação. Para obter instruções, consulte [publicar aplicações através do Proxy de aplicações do Azure AD](manage-apps/application-proxy-publish-azure-portal.md). 

   Quando configura o **Url interno** definição, utilize o URL de início de sessão que especificou no modelo de aplicação não galeria. Desta forma, os utilizadores podem aceder a aplicação a partir de fora do limite da organização. Proxy de aplicações efetua o SAML-início de sessão único para a aplicação no local.
 
   ![Mostra o URL interno de definições de aplicação no local e autenticação](media/active-directory-b2b-hybrid-cloud-to-on-premises/OnPremAppSettings.PNG)

## <a name="access-to-iwa-and-kcd-apps"></a>Acesso a aplicações IWA e KCD

Para fornecer B2B utilizadores acesso a aplicações no local que estejam protegidos com a autenticação integrada do Windows e a delegação restrita de Kerberos, terá dos seguintes componentes:

- **Autenticação através do Proxy de aplicações do Azure AD**. Utilizadores de B2B tem de conseguir autenticar para a aplicação no local. Para tal, tem de publicar a aplicação no local através do Proxy de aplicações do Azure AD. Para obter mais informações, consulte [começar com o Proxy de aplicações e instalar o conector](manage-apps/application-proxy-enable.md) e [publicar aplicações através do Proxy de aplicações do Azure AD](manage-apps/application-proxy-publish-azure-portal.md).
- **Autorização através de um objeto de utilizador no diretório no local B2B**. A aplicação tem de ser capaz de efetuar as verificações de acesso de utilizador e conceder acesso aos recursos corretos. IWA e KCD necessitam de um objeto de utilizador no local no Windows Server Active Directory para concluir esta autorização. Conforme descrito em [como-início de sessão único com KCD funciona](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md#how-single-sign-on-with-kcd-works), Proxy de aplicações tem este objeto de utilizador para representar o utilizador e obter um token de Kerberos para a aplicação. 

   Para o cenário de utilizador B2B, existem dois métodos que pode utilizar para criar os objetos de utilizador de convidados que são necessários para autorização no diretório no local disponíveis:

   - Microsoft Identity Manager (MIM) e o [agente de gestão de MIM para o Microsoft Graph](#create-b2b-guest-user-objects-through-mim-preview). 
   - [Um script do PowerShell](#create-b2b-guest-user-objects-through-a-script-preview). Utilizar o script é uma solução mais simples que não necessita de MIM. 

O diagrama seguinte fornece uma descrição geral de alto nível como do Azure AD do Proxy da aplicação e a geração do objeto de utilizador B2B no no local de trabalho de diretório em conjunto para conceder acesso às suas aplicações no local IWA e KCD utilizadores de B2B. Os passos numerados são descritos em detalhe o diagrama abaixo.

![Diagrama das soluções de script MIM e B2B](media/active-directory-b2b-hybrid-cloud-to-on-premises/MIMScriptSolution.PNG)

1.  Um utilizador de uma organização de parceiro (inquilino Fabrikam) está convidado para o inquilino Contoso.
2.  Um objeto de utilizador convidado é criado no inquilino Contoso (por exemplo, um objeto de utilizador com um UPN de guest_fabrikam.com#EXT#@contoso.onmicrosoft.com).
3.  O convidado Fabrikam é importado da Contoso através de MIM ou através do script do PowerShell de B2B.
4.  Uma representação ou "requisitos de espaço de" do objeto de utilizador de convidado Fabrikam (convidado #EXT #) é criado no diretório no local, Contoso.com, através de MIM ou através do script do PowerShell de B2B.
5.  A aplicação de acessos no local do utilizador convidado, app.contoso.com.
6.  O pedido de autenticação está autorizado através do Proxy de aplicações, utilizando a delegação restringida de Kerberos. 
7.  Porque o objeto de utilizador convidado existe localmente, a autenticação é efetuada com êxito.

### <a name="lifecycle-management-policies"></a>Políticas de gestão do ciclo de vida

Pode gerir os objetos de utilizador de B2B no local através de políticas de gestão do ciclo de vida. Por exemplo:

- Pode configurar políticas de autenticação multifator (MFA) para o utilizador convidado para que a MFA é utilizado durante a autenticação de Proxy de aplicações. Para obter mais informações, consulte [acesso condicional para os utilizadores de colaboração do B2B](active-directory-b2b-mfa-instructions.md).
- Qualquer sponsorships revisões de acesso, verificações de conta, etc., que são executados na nuvem B2B utilizador aplica-se aos utilizadores no local. Por exemplo, se o utilizador de nuvem é eliminado através de políticas de gestão do ciclo de vida, o utilizador no local também é eliminado através da sincronização de MIM ou através de sincronização do Azure AD Connect. Para obter mais informações, consulte [gerir o acesso de convidado com o acesso do Azure AD revê](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md).

### <a name="create-b2b-guest-user-objects-through-mim-preview"></a>Criar objetos de utilizador convidado B2B através de MIM (pré-visualização)

Para obter informações sobre como utilizar o MIM 2016 Service Pack 1 e o agente de gestão de MIM para o Microsoft Graph para criar objetos de utilizador convidado no diretório no local, consulte [colaboração de empresa-empresa (B2B) do Azure AD com o Microsoft Identity Manager (MIM) 2016 SP1 com o Proxy de aplicações do Azure (pré-visualização pública)](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016-graph-b2b-scenario).

### <a name="create-b2b-guest-user-objects-through-a-script-preview"></a>Criar objetos de utilizador convidado B2B através de um script (pré-visualização)

Não há um script de exemplo do PowerShell disponível que pode utilizar como ponto de partida para criar objetos de utilizador convidado no Active Directory no local.

Pode transferir o script e o ficheiro Leia-me a [Centro de transferências](https://www.microsoft.com/download/details.aspx?id=51495). Escolha o **Script e leia-me para solicitar B2B do Azure AD os utilizadores no-prem.zip** ficheiro.

Antes de utilizar o script, certifique-se de que revê os pré-requisitos e as considerações importantes sobre no ficheiro Leia-me associado. Além disso, tenha em atenção que o script é disponibilizado apenas como uma amostra. A equipa de desenvolvimento ou de um parceiro tem de personalizar e rever o script antes de executar a mesma.

## <a name="license-considerations"></a>Considerações de licença

Certifique-se de que tem as licenças de acesso cliente de correto (CALs) para os utilizadores convidados externos que acedem a aplicações no local. Para obter mais informações, consulte a secção "Conectores externos" [licenças de acesso de cliente e gestão de licenças](https://www.microsoft.com/en-us/licensing/product-licensing/client-access-license.aspx). Consulte o seu representante da Microsoft ou revendedor local sobre as necessidades de licenciamento específicas.

## <a name="next-steps"></a>Passos Seguintes

- [Azure colaboração B2B do Active Directory para organizações híbrida](active-directory-b2b-hybrid-organizations.md)

- Para obter uma descrição geral do Azure AD Connect, consulte [integrar os diretórios no local ao Azure Active Directory](connect/active-directory-aadconnect.md).

