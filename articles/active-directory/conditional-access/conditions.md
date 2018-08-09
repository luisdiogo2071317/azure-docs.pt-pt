---
title: Quais são condições no acesso condicional do Azure Active Directory? | Microsoft Docs
description: Saiba como as condições são usadas no acesso condicional do Azure Active Directory para acionar uma política.
services: active-directory
keywords: acesso condicional para aplicações, o acesso condicional com o Azure AD, o acesso seguro aos recursos da empresa, políticas de acesso condicional
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 5f5e2051f9c67fa4e37ce0e1213e14e197222f05
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39627547"
---
# <a name="what-are-conditions-in-azure-active-directory-conditional-access"></a>Quais são condições no acesso condicional do Azure Active Directory? 

Pode controlar o acesso de utilizadores autorizados como suas aplicações na cloud utilizando [acesso condicional do Azure Active Directory (Azure AD)](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-azure-portal). Uma política de acesso condicional, vai definir a resposta para o motivo para acionar a sua política. É uma resposta de exemplo **, em seguida, fazer isso**. É um motivo de exemplo **quando isso acontece,**.

![Motivo e resposta](./media/conditions/10.png)


No contexto do acesso condicional, **quando isso acontece** denomina-se um **condição**. **Em seguida, fazer isso** denomina-se um **controlo de acesso**. A combinação de suas condições e seus controles de acesso representa uma política de acesso condicional.

![Política de acesso condicional](./media/conditions/61.png)


Condições que ainda não configurado numa política de acesso condicional não são aplicadas. Algumas condições são [obrigatório](best-practices.md) para aplicar uma política de acesso condicional para um ambiente. 

Este artigo é uma visão geral das condições e como são utilizados numa política de acesso condicional. 

## <a name="users-and-groups"></a>Utilizadores e grupos

A condição de utilizadores e grupos é obrigatória de uma política de acesso condicional. Na sua política, pode, selecione **todos os utilizadores** ou selecione os utilizadores e grupos específicos.

![Utilizadores e grupos](./media/conditions/111.png)

Quando seleciona **todos os utilizadores**, a política é aplicada a todos os utilizadores no diretório, incluindo utilizadores convidados.

Quando **selecionar utilizadores e grupos**, pode definir as seguintes opções:

* **Todos os utilizadores convidados** destina-se uma política para utilizadores convidados B2B. Esta condição corresponde a uma conta de utilizador que tenha as **userType** atributo definido como **convidado**. Pode utilizar esta definição quando uma política tem de ser aplicadas assim que a conta é criada num fluxo de convite no Azure AD.

* **Funções de diretório** destina-se uma política com base na atribuição de função de um utilizador. Esta condição dá suporte a funções de diretório, como **Administrador Global** ou **administrador de palavras-passe**.

* **Utilizadores e grupos** conjuntos específicos de utilizadores destina-se. Por exemplo, pode selecionar um grupo que contenha todos os membros do departamento de RH, quando uma aplicação de RH está selecionada como a aplicação na cloud. Um grupo pode ser qualquer tipo de grupo no Azure AD, incluindo os grupos de segurança e a distribuição dinâmicos ou atribuídos.

Também pode excluir utilizadores ou grupos específicos de uma política. Um caso de uso comum é a contas de serviço se a sua política impõe a autenticação multifator (MFA). 

Conjuntos específicos de utilizadores o direcionamento é útil para a implementação de uma nova política. Numa nova política, deve visar apenas um conjunto inicial de utilizadores para validar o comportamento da política. 



## <a name="cloud-apps"></a>Aplicações na cloud 

Uma aplicação na cloud é um Web site ou serviço. Web sites protegidos pelo Proxy de aplicações do Azure AD também são aplicações na cloud. Para obter uma descrição detalhada das aplicações na cloud suportados, consulte [atribuições de aplicações da cloud](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-technical-reference#cloud-apps-assignments). 

O **aplicações na cloud** condição é obrigatória numa política de acesso condicional. Na sua política, pode, selecione **todas as aplicações na cloud** ou selecione as aplicações específicas.

![Incluem aplicações na cloud](./media/conditions/03.png)

- Selecione **todas as aplicações na cloud** às políticas de linha de base para se aplicam a toda a organização. Utilize esta selecção para políticas que exigem a autenticação multifator quando é detetado um risco de início de sessão para qualquer aplicação na cloud. Uma política aplicada ao **todas as aplicações na cloud** aplica-se para o acesso a todos os Web sites e serviços. Esta definição não se limita às aplicações de cloud que são apresentadas na **selecionar aplicações** lista. 

- Selecione as aplicações de cloud individuais para serviços específicos do destino pela política. Por exemplo, pode exigir que os utilizadores tenham uma [dispositivo em conformidade](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-mam#app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online) para aceder ao SharePoint Online. Esta política também é aplicada a outros serviços, quando acede a conteúdo do SharePoint. Um exemplo é o Microsoft Teams. 

Pode excluir aplicações específicas de uma política. No entanto, estas aplicações estão ainda sujeitos as políticas aplicadas para os serviços que acedem. 



## <a name="sign-in-risk"></a>Risco de início de sessão

Um risco de início de sessão é um indicador da alta, média ou baixa probabilidade de uma tentativa de início de sessão não foi efetuada pelo proprietário legítimo de uma conta de utilizador. O Azure AD calcula que o nível de risco de início de sessão durante um usuário estejam início de sessão-no. O nível de risco de início de sessão calculados pode ser uma condição de uma política de acesso condicional. 

![Níveis de risco de início de sessão](./media/conditions/22.png)

Para utilizar esta condição, tem de ter [do Azure Active Directory Identity Protection](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-identityprotection-enable) ativada.
 
Casos de utilização comuns para esta condição são as políticas que tenham as proteções seguintes: 

- Impedir que os utilizadores com um início de sessão de alto risco. Esta proteção impede que potencialmente não legítimos os utilizadores acedem às suas aplicações na cloud. 
- Exigir a autenticação multifator para os utilizadores com um risco de início de sessão médio. Ao impor a autenticação multifator, pode fornecer confiança adicional que o início de sessão é feito pelo proprietário legítimo de uma conta.

Para obter mais informações, veja [Inícios de sessão arriscados](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-security-risky-sign-ins).  

## <a name="device-platforms"></a>Plataformas de dispositivos

A plataforma do dispositivo é caracterizada por sistema operativo que é executado no seu dispositivo. O Azure AD identifica a plataforma utilizando as informações fornecidas pelo dispositivo, por exemplo, o agente de utilizador. Esta informação é não verificada. Recomendamos que todas as plataformas têm uma política aplicada às mesmas. A política deve ou bloquear o acesso, exigir a conformidade com políticas do Microsoft Intune ou exigir que o dispositivo ser associado a um domínio. A predefinição é aplicar uma política a todas as plataformas de dispositivo. 


![Configurar as plataformas de dispositivo](./media/conditions/24.png)

Para obter uma lista de plataformas de dispositivos suportados, consulte [condição de plataforma de dispositivo](technical-reference.md#device-platform-condition).


Caso de utilização de uma comum para esta condição é uma política que restringe o acesso às suas aplicações na cloud para [dispositivos geridos](require-managed-devices.md). Para obter mais cenários, incluindo a condição de plataforma do dispositivo, consulte [acesso condicional com base na aplicação do Azure Active Directory](app-based-conditional-access.md).



## <a name="device-state"></a>Estado do dispositivo

A condição de estado do dispositivo exclui híbridos que associados ao Azure AD e a dispositivos marcado como conforme a partir de uma política de acesso condicional. Esta condição é útil quando uma política deve ser aplicada apenas a um dispositivo não gerido para fornecer segurança de sessão adicionais. Por exemplo, apenas impor o controlo de sessão do Microsoft Cloud App Security quando um dispositivo for não gerido. 


![Configurar o estado do dispositivo](./media/conditions/112.png)

Se desejar bloquear o acesso para dispositivos não geridos, implemente [acesso condicional com base no dispositivo](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam#app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online).


## <a name="locations"></a>Localizações

Ao utilizar localizações, pode definir condições com base no qual foi tentada uma ligação. 

![Configurar localizações](./media/conditions/25.png)

Casos de utilização comuns para esta condição são as políticas com as proteções seguintes:

- Exigir a autenticação multifator para os utilizadores que acedem um serviço, quando estiverem desconectados da rede corporativa.  

- Bloquear o acesso para os utilizadores que acedem um serviço de países ou regiões específicos. 

Para obter mais informações, consulte [o que é a condição de localização no acesso condicional do Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-locations).


## <a name="client-apps"></a>Aplicações do cliente

Ao utilizar a condição de aplicações de cliente, pode aplicar uma política para diferentes tipos de aplicativos. Os exemplos são os Web sites, serviços, aplicações móveis e aplicativos de desktop. 



Um aplicativo é classificado da seguinte forma:

- Um Web site ou serviço se utiliza protocolos SSO web, SAML, WS-Fed, ou OpenID Connect para um cliente confidencial.

- Uma aplicação móvel ou um aplicativo de desktop se utiliza a aplicação móvel do OpenID Connect para um cliente nativo.

Para obter uma lista das aplicações de cliente pode utilizar na sua política de acesso condicional, consulte [condição de aplicações de cliente](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#client-apps-condition) na referência técnica do Azure Active Directory condicional acesso.

Casos de utilização comuns para esta condição são as políticas com as proteções seguintes: 

- Exigir um [dispositivo em conformidade](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam#app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online) para aplicações de ambiente de trabalho e móveis que transferir grandes quantidades de dados no dispositivo. Ao mesmo tempo, permita o acesso ao browser a partir de qualquer dispositivo.

- Bloquear o acesso de aplicações web, mas permitir o acesso das aplicações móveis e de Desktops.

Pode aplicar esta condição para SSO da web e protocolos de autenticação moderna. Também pode aplicá-la para aplicativos de email que usam o Microsoft Exchange ActiveSync. Os exemplos são as aplicações de e-mail nativo na maioria dos smartphones. 

Só pode selecionar a condição de aplicações de cliente se o Microsoft Office 365 Exchange Online é a única aplicação na cloud que selecionou.

![Aplicações na cloud](./media/conditions/32.png)

Selecionando **Exchange ActiveSync** como um cliente, a condição de aplicações é suportada apenas se não tiver outras condições configuradas numa política. No entanto, pode restringir o âmbito desta condição para aplicar apenas a plataformas suportadas.

 
![Aplicar política apenas a plataformas suportadas](./media/conditions/33.png)

Aplicar esta condição apenas a plataformas suportadas é igual para todas as plataformas de dispositivo num [condição de plataforma de dispositivo](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-mam#app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online).

![Configurar as plataformas de dispositivo](./media/conditions/34.png)


 Para obter mais informações, veja estes artigos:

- [Configurar o SharePoint Online e Exchange Online para o acesso condicional do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-no-modern-authentication).
 
- [O Azure Active Directory com base na aplicação acesso condicional](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam). 


### <a name="legacy-authentication"></a>Autenticação legada  

Acesso condicional aplica-se agora para clientes do Microsoft Office mais antigos que não suportam a autenticação moderna. Também se aplica aos clientes que utilizam os protocolos de email, como POP, IMAP e SMTP. Ao utilizar a autenticação de legado, pode configurar políticas, como **bloquear o acesso a partir de outros clientes**.


![Configurar aplicações de cliente](./media/conditions/160.png)  


#### <a name="known-issues"></a>Problemas conhecidos

- Configurar uma política para **outros clientes** bloqueia toda a organização de determinados clientes, como SPConnect. Este bloco acontece porque os clientes antigos autenticar-se de maneiras inesperadas. O problema não se aplica a aplicativos do Office principais, como os clientes do Office mais antigos. 

- Pode demorar até 24 horas para a política para entrar em vigor. 


#### <a name="frequently-asked-questions"></a>Perguntas mais frequentes

**P:** esta autenticação bloqueará serviços Web do Microsoft Exchange?

Depende do protocolo de autenticação que usa serviços Web do Exchange. Se o aplicativo de serviços Web do Exchange usa autenticação moderna, é abrangido pela **aplicações móveis e clientes de ambiente de trabalho** aplicação de cliente. Autenticação básica é abrangida pela **outros clientes** aplicação de cliente.


**P:** que controlos se pode utilizar para **outros clientes**?

Qualquer controle pode ser configurado para **outros clientes**. No entanto, a experiência de utilizador final será bloqueado o acesso para todos os casos. **Outros clientes** não oferecem suporte a controles, como a MFA, o dispositivo em conformidade e associação a um domínio. 
 
**P:** quais condições se pode utilizar para **outros clientes**?

Qualquer condição pode ser configurada para **outros clientes**.

**P:** faz o Exchange ActiveSync suporta todas as condições de controles?

Não. A lista seguinte resume o suporte do Exchange ActiveSync: 

- Exchange ActiveSync suporta apenas utilizador e da filtragem de grupo. Ele não dá suporte convidados ou funções. Se uma condição de convidado ou a função estiver configurada, todos os utilizadores são bloqueados. Exchange ActiveSync bloqueia todos os utilizadores porque ele não é possível determinar se deve aplicar a política para o usuário, ou não.

- Exchange ActiveSync funciona apenas com o Microsoft Exchange Online como a aplicação na cloud. 

- Exchange ActiveSync não suporta qualquer condição, exceto o próprio aplicativo de cliente. 

- Exchange ActiveSync pode ser configurado com qualquer controle. Todos os controles, exceto a conformidade do dispositivo levam a um bloco.

**P:** são as políticas aplicáveis a todas as aplicações de cliente por padrão no futuro?

Não. Não há nenhuma alteração no comportamento de política predefinida. As políticas de continuam a aplicar ao navegador e aplicativos móveis e clientes de ambiente de trabalho, por predefinição.



## <a name="next-steps"></a>Passos Seguintes

- Para saber como configurar uma política de acesso condicional, consulte [início rápido: exigir a MFA para aplicações específicas com acesso condicional do Azure Active Directory](app-based-mfa.md).

- Para configurar políticas de acesso condicional para o seu ambiente, consulte a [melhores práticas para acesso condicional no Azure Active Directory](best-practices.md). 

