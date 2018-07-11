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
ms.component: protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 42792170593dbd94d0eae9b408c70f326891508a
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "36232472"
---
# <a name="what-are-conditions-in-azure-active-directory-conditional-access"></a>Quais são condições no acesso condicional do Azure Active Directory? 

Com o [acesso condicional do Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), pode controlar o acesso de utilizadores autorizados como suas aplicações na cloud. Uma política de acesso condicional, vai definir a resposta ("fazê-lo") para o motivo para acionar a política ("quando isso acontece"). 

![Controlo](./media/active-directory-conditional-access-conditions/10.png)


No contexto do acesso condicional:

- "**Quando isso acontece**" é chamado **condições**. 
- "**, Em seguida, fazer isso**" é chamado **controlos de acesso**.

A combinação de suas condições com controlos de acesso representa uma política de acesso condicional.

![Controlo](./media/active-directory-conditional-access-conditions/61.png)


Condições que não tiver configurado uma política de acesso condicional não são aplicadas. Algumas condições são [obrigatório](active-directory-conditional-access-best-practices.md#whats-required-to-make-a-policy-work) para aplicar uma política de acesso condicional para um ambiente. 

Este artigo fornece uma descrição geral as condições e como elas são usadas numa política de acesso condicional. 

## <a name="users-and-groups"></a>Utilizadores e grupos

A condição de utilizadores e grupos é obrigatória de uma política de acesso condicional. Na sua política, pode, selecione **todos os utilizadores** ou selecione os utilizadores e grupos específicos.

![Controlo](./media/active-directory-conditional-access-conditions/111.png)

Quando seleciona:

- **Todos os utilizadores**, a política é aplicada a todos os utilizadores no diretório. Isto inclui os utilizadores convidados.

- **Selecionar utilizadores e grupos**, pode definir as seguintes opções:

    - **Todos os utilizadores convidados** -permite-lhe direcionar uma política para utilizadores convidados B2B. Esta condição corresponde a qualquer conta de utilizador com o *userType* atributo definido como *convidado*. Pode utilizar esta definição em casos onde uma política tem de ser aplicadas assim que a conta é criada num fluxo de convite no Azure AD.

    - **Funções de diretório** -permite-lhe uma política com base na atribuição de função de um utilizador de destino. Esta condição oferece suporte, como funções de diretório *Administrador Global* ou *administrador de palavras-passe*.

    - **Utilizadores e grupos** -permite-lhe a conjuntos específicos do destino de utilizadores. Por exemplo, pode selecionar um grupo que contenha todos os membros do departamento de RH, quando tiver uma aplicação de RH selecionada como a aplicação na cloud.

Um grupo, pode ser qualquer tipo de grupo no Azure AD, incluindo os grupos de segurança e a distribuição dinâmicos ou atribuídos

Também pode excluir utilizadores ou grupos específicos de uma política. Um caso de utilização comuns são as contas de serviço, se a sua política impõe a autenticação multifator (MFA). 

Conjuntos específicos de utilizadores o direcionamento é útil para a implementação de uma nova política. Numa nova política, deve visar apenas um conjunto inicial de utilizadores para validar o comportamento da política. 



## <a name="cloud-apps"></a>Aplicações na cloud 

Uma aplicação da cloud é um sites da web ou serviço. Isto inclui a sites da web protegidos pelo Proxy de aplicações do Azure. Para obter uma descrição detalhada das aplicações cloud suportados, consulte [atribuição de aplicações da cloud](active-directory-conditional-access-technical-reference.md#cloud-apps-assignments).    

A condição de aplicações na cloud é obrigatória de uma política de acesso condicional. Na sua política, pode, selecione **todas as aplicações na cloud** ou selecione as aplicações específicas.

![Controlo](./media/active-directory-conditional-access-conditions/03.png)

Pode selecionar:

- **Todas as aplicações na cloud** às políticas de linha de base a ser aplicado em toda a organização. Um caso de utilização comuns para esta seleção é uma política que requer autenticação multifator quando é detetado um risco de início de sessão para qualquer aplicação na cloud. Uma política aplicada ao **todas as aplicações na cloud** aplica-se para o acesso a todos os web sites e serviços. Esta definição não se limita às aplicações de cloud que são apresentadas na **selecionar nuvem aplicações** lista.

- Aplicações de cloud individuais para serviços específicos do destino pela política. Por exemplo, pode exigir que os utilizadores tenham uma [dispositivo em conformidade](active-directory-conditional-access-policy-connected-applications.md) para aceder ao SharePoint Online. Esta política também é aplicada a outros serviços, quando acede a conteúdo do SharePoint, por exemplo, Microsoft Teams. 

Também pode excluir aplicações específicas de uma política; No entanto, estas aplicações estão ainda sujeitos as políticas aplicadas aos serviços que acedem. 



## <a name="sign-in-risk"></a>Risco de início de sessão

Um risco de início de sessão é um indicador para a probabilidade (alta, média ou baixa) que uma tentativa de início de sessão não foi efetuada pelo proprietário legítimo de uma conta de utilizador. O Azure AD calcula o nível de risco de início de sessão durante o início de sessão de um utilizador. Pode usar o nível de risco de início de sessão calculados como condição de uma política de acesso condicional. 

![Condições](./media/active-directory-conditional-access-conditions/22.png)

Para utilizar esta condição, tem de ter [do Azure Active Directory Identity Protection](active-directory-identityprotection.md) ativada.
 
Casos de utilização comuns para esta condição são as políticas que:

- Impedir que os utilizadores com um risco de início de sessão de alta para impedir que os utilizadores potencialmente não legítimos acedem às suas aplicações na cloud. 
- Exigir autenticação multifator para os utilizadores com um risco de início de sessão médio. Ao impor autenticação multifator, pode fornecer confiança adicional que o início de sessão é efetuado pelo proprietário legítimo de uma conta.

Para obter mais informações, veja [Inícios de sessão arriscados](active-directory-identityprotection.md#risky-sign-ins).  

## <a name="device-platforms"></a>Plataformas de dispositivos

A plataforma do dispositivo é caracterizada pelo sistema operativo que está a executar no seu dispositivo. O Azure AD identifica a plataforma utilizando as informações fornecidas pelo dispositivo, por exemplo, o agente de utilizador. Uma vez que esta informação é não verificada, recomenda-se que todas as plataformas têm uma política aplicada aos mesmos, a bloquear o acesso, que requerem a conformidade com as políticas do Intune ou exigir que o dispositivo ser associado a um domínio. A predefinição é aplicar a política a todas as plataformas de dispositivo. 


![Condições](./media/active-directory-conditional-access-conditions/24.png)

Para obter uma lista completa das plataformas de dispositivos suportadas, consulte [condição de plataforma de dispositivo](active-directory-conditional-access-technical-reference.md#device-platform-condition).


Caso de utilização de uma comum para esta condição é uma política que restringe o acesso às suas aplicações na cloud para [dispositivos geridos](active-directory-conditional-access-policy-connected-applications.md#managed-devices). Para obter mais cenários, incluindo a condição de plataforma do dispositivo, consulte [acesso condicional com base na aplicação do Azure Active Directory](active-directory-conditional-access-mam.md).



## <a name="device-state"></a>Estado do dispositivo

A condição de estado de dispositivo permite que o Azure AD híbrido associou e dispositivos marcado como compatíveis a serem excluídos da política de acesso condicional. Isto é útil quando uma política deve ser aplicadas apenas para o dispositivo não gerido para fornecer segurança de sessão adicionais. Por exemplo, apenas impor o controlo de sessão do Microsoft Cloud App Security quando um dispositivo for não gerido. 


![Condições](./media/active-directory-conditional-access-conditions/112.png)

Se desejar bloquear o acesso para dispositivos não geridos, deve implementar [acesso condicional com base no dispositivo](active-directory-conditional-access-policy-connected-applications.md).


## <a name="locations"></a>Localizações

Com locais, tem a opção para definir condições que se baseiam em que uma tentativa de ligação foi iniciada a partir da. 
     
![Condições](./media/active-directory-conditional-access-conditions/25.png)

Casos de utilização comuns para esta condição são as políticas que:

- Exigir autenticação multifator para os utilizadores que acedem um serviço, quando estiverem desconectados da rede corporativa.  

- Bloquear o acesso para os utilizadores que acedem um serviço de países ou regiões específicos. 

Para obter mais informações, consulte [o que é a condição de localização no acesso condicional do Azure Active Directory?](active-directory-conditional-access-locations.md)


## <a name="client-apps"></a>Aplicações do cliente

A condição de aplicações de cliente permite-lhe aplicar uma política para diferentes tipos de aplicativos, tais como:

- Web sites e serviços
- Aplicações móveis e aplicativos de desktop. 



Um aplicativo é classificado como:

- Um site ou serviço se utiliza os protocolos SSO web, SAML, WS-Fed ou OpenID Connect para um cliente confidencial.

- A uma aplicação móvel ou um aplicativo de desktop se utiliza a aplicação móvel do OpenID Connect para um cliente nativo.

Para obter uma lista completa das aplicações de cliente pode utilizar na sua política de acesso condicional, consulte [condição de aplicações de cliente](active-directory-conditional-access-technical-reference.md#client-apps-condition) na referência técnica do Azure Active Directory condicional acesso.

Casos de utilização comuns para esta condição são as políticas que:

- Exigir um [dispositivo em conformidade](active-directory-conditional-access-policy-connected-applications.md) para aplicações de ambiente de trabalho e móveis que transferir grandes quantidades de dados no dispositivo, permitindo o acesso ao browser a partir de qualquer dispositivo.

- Bloquear o acesso de aplicações web, mas permitir o acesso das aplicações móveis e de Desktops.

Além de utilizar protocolos de autenticação moderna e de web SSO, pode aplicar esta condição a aplicações de correio que utilizam o Exchange ActiveSync, como as aplicações de e-mail nativo na maioria dos smartphones. Atualmente, as aplicações de cliente através de protocolos herdados precisam de ser protegidas a utilizar o AD FS.

Só pode selecionar esta condição se **Office 365 Exchange Online** é a única aplicação na cloud que selecionou.

![Aplicações na cloud](./media/active-directory-conditional-access-conditions/32.png)

Selecionando **Exchange ActiveSync** como aplicações de cliente condição só é suportada se não tiver outras condições numa política configurada. No entanto, pode restringir o âmbito desta condição para aplicar apenas a plataformas suportadas.

 
![Plataformas suportadas](./media/active-directory-conditional-access-conditions/33.png)

Aplicar esta condição apenas a plataformas suportadas é o equivalente a todas as plataformas de dispositivo num [condição de plataforma de dispositivo](active-directory-conditional-access-conditions.md#device-platforms).

![Plataformas suportadas](./media/active-directory-conditional-access-conditions/34.png)


 Para obter mais informações, consulte:

- [Configurar o SharePoint Online e Exchange Online para o acesso condicional do Azure Active Directory](active-directory-conditional-access-no-modern-authentication.md)
 
- [O Azure Active Directory com base na aplicação acesso condicional](active-directory-conditional-access-mam.md) 


### <a name="legacy-authentication"></a>Autenticação de legado  

Acesso condicional aplica-se agora para clientes com Office antigos que não suportam a autenticação moderna, bem como os clientes que utilizam os protocolos de email como POP, IMAP, SMTP, etc. Isto permite-lhe configurar políticas, como **bloquear o acesso a partir de outros clientes**.


![Autenticação de legado](./media/active-directory-conditional-access-conditions/160.png)
 



#### <a name="known-issues"></a>Problemas conhecidos

- Configurar uma política para **outros clientes** bloqueia toda a organização de determinados clientes, como SPConnect. Isso se deve destes clientes mais antigos de autenticação de maneiras inesperadas. Este problema não é aplicável para as principais aplicações do Office, como os clientes do Office mais antigas. 

- Pode demorar até 24 horas para a política para entrar em vigor. 


#### <a name="frequently-asked-questions"></a>Perguntas mais frequentes

**Isto bloqueia a serviços do Exchange Web (EWS)?**

Depende do protocolo de autenticação que utiliza o EWS. Se a aplicação do EWS estiver a utilizar autenticação moderna, serão abordado pela aplicação de cliente "aplicações móveis e clientes de ambiente de trabalho". Se o aplicativo de EWS está usando autenticação básica, serão abordado pela aplicação de cliente "Outros clientes".


**Quais controles pode utilizar para outros clientes**

Qualquer controle pode ser configurado para "Outros clientes". No entanto, a experiência do utilizador final será bloquear o acesso para todos os casos. "Outros clientes" não suportam a controles, como a MFA, dispositivo em conformidade, associação a um domínio, etc. 
 
**Quais condições pode utilizar para outros clientes?**

Todas as condições podem ser configuradas para "Outros clientes".

**O Exchange ActiveSync suporta todas as condições e controlos?**

Não. Este é o resumo de suporte do Exchange ActiveSync (EAS):

- EAS suporta apenas o utilizador e a filtragem de grupo. Ele não dá suporte a convidado, funções. Se a condição de convidado/função estiver configurada, serão bloqueados todos os utilizadores, uma vez que não é possível determinar se deve aplicar a política para o usuário, ou não.

- EAS só funciona com o Exchange como a aplicação na cloud. 

- EAS não suporta qualquer condição, exceto o próprio aplicativo de cliente.

- EAS pode ser configurado com qualquer controle (tudo exceto a conformidade do dispositivo irá levar a bloco).

**São as políticas aplicáveis a todas as aplicações de cliente por padrão no futuro?**

Não. Não há nenhuma alteração no comportamento de política predefinida. As políticas de continuam a aplicar ao navegador e clientes de aplicações/ambiente de trabalho móvel por predefinição.



## <a name="next-steps"></a>Passos Seguintes

- Se quiser saber como configurar uma política de acesso condicional, consulte [exigir a MFA para aplicações específicas com acesso condicional do Azure Active Directory](active-directory-conditional-access-app-based-mfa.md).

- Se estiver pronto para configurar políticas de acesso condicional para o seu ambiente, veja a [melhores práticas para acesso condicional no Azure Active Directory](active-directory-conditional-access-best-practices.md). 

