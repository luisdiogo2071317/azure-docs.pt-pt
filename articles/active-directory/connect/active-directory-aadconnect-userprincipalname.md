---
title: Azure AD UserPrincipalName população
description: O documento seguinte descreve a forma como o atributo UserPrincipalName é preenchido.
author: billmath
ms.component: hybrid
ms.author: billmath
ms.date: 06/26/2018
ms.topic: article
ms.workload: identity
ms.service: active-Directory
manager: mtillman
ms.openlocfilehash: 6b3fddcdf6ff9c35d5932b9b83da02f60f9e081e
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063927"
---
# <a name="azure-ad-userprincipalname-population"></a>Azure AD UserPrincipalName população

Este artigo descreve como o atributo UserPrincipalName é preenchido no Azure Active Directory (Azure AD).
O valor do atributo UserPrincipalName é o nome de utilizador do Azure AD para as contas de utilizador.

## <a name="upn-terminology"></a>Terminologia UPN
A seguinte terminologia é utilizada neste artigo:

|Termo|Descrição|
|-----|-----|
|Domínio inicial|O domínio predefinido (m) no inquilino do Azure AD. Por exemplo, contoso.onmicrosoft.com.|
|Microsoft Online encaminhamento endereço de correio eletrónico (MOERA)|Azure AD calcula MOERA do Azure AD MailNickName atributo e de domínio inicial do Azure AD como &lt;MailNickName&gt;&#64;&lt;domínio inicial&gt;.|
|Atributo de mailNickName no local|Um atributo no Active Directory, o valor que representa o alias de um utilizador na organização do Exchange.|
|Atributo de correio no local|Um atributo no Active Directory, o valor que representa o endereço de e-mail de um utilizador|
|Endereço SMTP principal|O endereço de correio eletrónico principal de um objeto de destinatário do Exchange. Por exemplo, SMTP:user\@contoso.com.|
|ID de início de sessão alternativo|Um atributo no local diferente de UserPrincipalName, tais como atributo de correio, utilizado para início de sessão.|

## <a name="what-is-userprincipalname"></a>O que é o UserPrincipalName?
UserPrincipalName é um atributo que é um nome de início de sessão de estilo de Internet para um utilizador com base num padrão Internet [RFC 822](http://www.ietf.org/rfc/rfc0822.txt). 

### <a name="upn-format"></a>Formato UPN
Um UPN é composta por um prefixo UPN (nome de conta de utilizador) e um sufixo UPN (um nome de domínio DNS). O prefixo é associado com a utilização de sufixo de "\@" símbolo. Por exemplo, "alguém\@example.com". Um UPN têm de ser exclusivo entre todos os objetos de principais de segurança dentro de uma floresta de diretório. 

## <a name="upn-in-azure-ad"></a>UPN no Azure AD 
O UPN é utilizado pelo Azure AD para permitir aos utilizadores iniciar sessão.  O UPN que um utilizador pode utilizar, depende se pretende ou não o domínio ter sido verificado.  Se o domínio ter sido verificado, um utilizador com esse sufixo terá permissão para início de sessão para o Azure AD.  

O atributo é sincronizado pelo Azure AD Connect.  Durante a instalação, pode ver os domínios que foram verificados e aqueles que não têm.

   ![Domínios não verificados](./media/active-directory-aadconnect-get-started-express/unverifieddomain.png) 

## <a name="alternate-login-id"></a>ID de início de sessão alternativo
Em alguns ambientes, os utilizadores finais podem apenas ser em consideração os seus endereços de e-mail e não o UPN.  A utilização do endereço de e-mail pode ser devido a uma política da empresa ou uma dependência de aplicação de linha de negócio no local.

ID de início de sessão alternativo permite-lhe configurar uma experiência de início de sessão onde os utilizadores podem iniciar sessão com um atributo que não seja o UPN, como o correio.

Para ativar o ID de início de sessão alternativo com o Azure AD, não existem passos de configurações adicionais necessários ao utilizar o Azure AD Connect. ID alternativo pode ser configurado diretamente a partir do assistente. Consulte o artigo do Azure AD-configuração de sessão para os seus utilizadores na secção sincronização. Sob o **Nome Principal de utilizador** pendente, selecione o atributo para o ID de início de sessão alternativo.

![Domínios não verificados](./media/active-directory-aadconnect-userprincipalname/altloginid.png)  

Para obter mais informações, consulte [ID de início de sessão alternativo configurar](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id) e [configuração do início de sessão do Azure AD](active-directory-aadconnect-get-started-custom.md#azure-ad-sign-in-configuration)

## <a name="non-verified-upn-suffix"></a>Sufixo UPN não verificado
Se o sufixo de ID de início de sessão do local UserPrincipalName atributo/alternativo não é verificado com o inquilino do Azure AD, o valor do atributo UserPrincipalName do Azure AD está definido para MOERA. Azure AD calcula MOERA do atributo MailNickName do Azure AD e do domínio inicial do Azure AD como &lt;MailNickName&gt;&#64;&lt;domínio inicial&gt;.

## <a name="verified-upn-suffix"></a>Verificado sufixo UPN
Se o UserPrincipalName no local. o atributo/alternativo sufixo do ID de início de sessão é verificado com o inquilino do Azure AD, em seguida, o valor do atributo UserPrincipalName do Azure AD vai ser o mesmo que o valor de ID de início de sessão do local UserPrincipalName atributo/alternativo.

## <a name="azure-ad-mailnickname-attribute-value-calculation"></a>Cálculo de valor de atributo do Azure AD MailNickName
Uma vez que foi possível definir o valor do atributo UserPrincipalName do Azure AD MOERA, é importante compreender a forma como o valor do atributo MailNickName do Azure AD, que é o prefixo MOERA, é calculado.

Quando um objeto de utilizador está sincronizado para um inquilino do Azure AD pela primeira vez, o Azure AD verifica os seguintes itens na ordem indicada e define o valor do atributo MailNickName ao primeiro existente:

- Atributo de mailNickName no local
- Prefixo do endereço SMTP principal
- Prefixo do nome de atributo de correio no local
- Prefixo de ID de início de sessão de atributo/alternativo de userPrincipalName no local
- Prefixo do endereço de smtp secundário

Quando as atualizações para um objeto de utilizador são sincronizadas com o inquilino do Azure AD, o Azure AD atualiza o valor do atributo MailNickName apenas no caso de existir uma atualização para o valor do atributo mailNickName no local.

>[!IMPORTANT]
>Azure AD recalcula o valor do atributo UserPrincipalName apenas no caso de uma atualização para o valor de ID de início de sessão do local UserPrincipalName atributo/alternativa está sincronizada para o inquilino do Azure AD. 
>
>Sempre que o Azure AD recalcula o atributo UserPrincipalName, recalcula também o MOERA. 

## <a name="upn-scenarios"></a>Cenários UPN
Seguem-se cenários de exemplo de como o UPN é calculado com base num determinado cenário.

### <a name="scenario-1-non-verified-upn-suffix--initial-synchronization"></a>Cenário 1: Sufixo UPN não verificado – a sincronização inicial

![Scenario1](media/active-directory-aadconnect-userprincipalname/example1.png)

Objeto de utilizador no local:
- mailNickName: &lt;não definida&gt;
- proxyAddresses: {SMTP:us1@contoso.com}
- correio: us2@contoso.com
- userPrincipalName: us3@contoso.com'

Sincronizar o objeto de utilizador para o inquilino do Azure AD pela primeira vez
- Definir o atributo de MailNickName do Azure AD para o prefixo do endereço SMTP principal.
- Definir MOERA &lt;MailNickName&gt;&#64;&lt;domínio inicial&gt;.
- Defina o atributo UserPrincipalName do Azure AD para MOERA.

Objeto de utilizador de inquilino do AD do Azure:
- MailNickName: us1           
- UserPrincipalName: us1@contoso.onmicrosoft.com


### <a name="scenario-2-non-verified-upn-suffix--set-on-premises-mailnickname-attribute"></a>Cenário 2: Sufixo UPN não verificado – conjunto no local atributo mailNickName

![Scenario2](media/active-directory-aadconnect-userprincipalname/example2.png)

Objeto de utilizador no local:
- mailNickName      : us4
- proxyAddresses: {SMTP:us1@contoso.com}
- correio: us2@contoso.com
- userPrincipalName: us3@contoso.com

Sincronizar a atualização no atributo de mailNickName no local para o inquilino do Azure AD
- Atualize o atributo de MailNickName do Azure AD com o atributo de mailNickName no local.
- Porque não existe nenhuma atualização para o atributo de userPrincipalName no local, não há nenhuma alteração para o atributo UserPrincipalName do Azure AD.

Objeto de utilizador de inquilino do AD do Azure:
- MailNickName      : us4
- UserPrincipalName: us1@contoso.onmicrosoft.com

### <a name="scenario-3-non-verified-upn-suffix--update-on-premises-userprincipalname-attribute"></a>Cenário 3: Sufixo UPN não verificado – atualização no local atributo userPrincipalName

![Scenario3](media/active-directory-aadconnect-userprincipalname/example3.png)

Objeto de utilizador no local:
- mailNickName      : us4
- proxyAddresses: {SMTP:us1@contoso.com}
- correio: us2@contoso.com
- userPrincipalName: us5@contoso.com

Sincronizar atualizações de atributo de userPrincipalName no local para o inquilino do Azure AD
- Atualização no atributo de userPrincipalName no local aciona novo cálculo do atributo MOERA e UserPrincipalName do Azure AD.
- Definir MOERA &lt;MailNickName&gt;&#64;&lt;domínio inicial&gt;.
- Defina o atributo UserPrincipalName do Azure AD para MOERA.

Objeto de utilizador de inquilino do AD do Azure:
- MailNickName      : us4
- UserPrincipalName: us4@contoso.onmicrosoft.com

### <a name="scenario-4-non-verified-upn-suffix--update-primary-smtp-address-and-on-premises-mail-attribute"></a>Cenário 4: Atributo de correio do sufixo UPN não verificado – endereço SMTP principal de atualização e no local

![Scenario4](media/active-directory-aadconnect-userprincipalname/example4.png)

Objeto de utilizador no local:
- mailNickName      : us4
- proxyAddresses: {SMTP:us6@contoso.com}
- correio: us7@contoso.com
- userPrincipalName: us5@contoso.com

Sincronizar atualizações de atributo de correio no local e o endereço SMTP principal para o inquilino do Azure AD
- Após a sincronização inicial de objeto de utilizador, de atualizações para o local de atributo de correio e o endereço SMTP principal não irá afetar o MailNickName do AD do Azure ou o atributo UserPrincipalName.

Objeto de utilizador de inquilino do AD do Azure:
- MailNickName      : us4
- UserPrincipalName: us4@contoso.onmicrosoft.com

### <a name="scenario-5-verified-upn-suffix--update-on-premises-userprincipalname-attribute-suffix"></a>Cenário 5: Sufixo UPN verificado – atualização no local sufixo de atributo userPrincipalName

![Scenario5](media/active-directory-aadconnect-userprincipalname/example5.png)

Objeto de utilizador no local:
- mailNickName      : us4
- proxyAddresses: {SMTP:us6@contoso.com}
- correio: us7@contoso.com
- serPrincipalName  : us5@verified.contoso.com

Sincronizar atualizações de atributo de userPrincipalName no local para o inquilino do Azure AD
- Atualizar no local no novo de acionadores cálculo userPrincipalName atributo do atributo UserPrincipalName do Azure AD.
- Defina o atributo UserPrincipalName do Azure AD para o atributo de userPrincipalName no local como o sufixo UPN é verificado com o inquilino do Azure AD.

Objeto de utilizador de inquilino do AD do Azure:
- MailNickName      : us4     
- UserPrincipalName: us5@verified.contoso.com

## <a name="next-steps"></a>Próximos Passos
- [Integrar os diretórios no local com o Azure Active Directory](active-directory-aadconnect.md)
- [Instalação personalizada do Azure AD Connect](active-directory-aadconnect-get-started-custom.md)
