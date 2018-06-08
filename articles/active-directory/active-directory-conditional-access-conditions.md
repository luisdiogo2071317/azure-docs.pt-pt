---
title: Condições de acesso condicional do Azure Active Directory | Microsoft Docs
description: Saiba como atribuições são utilizadas no acesso condicional do Azure Active Directory para acionar uma política.
services: active-directory
keywords: acesso condicional para aplicações, o acesso condicional com o Azure AD, o acesso seguro aos recursos da empresa, as políticas de acesso condicional
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
ms.date: 06/01/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 5f0ff092a7535448d48642e972d1d36652f1b83f
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "34735146"
---
# <a name="conditions-in-azure-active-directory-conditional-access"></a>Condições de acesso condicional do Azure Active Directory 

Com [acesso condicional do Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), pode controlar o acesso de utilizadores autorizados como as suas aplicações em nuvem. Na política de acesso condicional, é possível definir a resposta ("fazê-lo") para a razão para acionar a política ("quando isto acontece"). 

![Controlo](./media/active-directory-conditional-access-conditions/10.png)


No contexto do acesso condicional:

- "**Quando isto acontece**" denomina **condições**. 
- "**, Em seguida, fazer isto**" denomina **controlos de acesso**.

A combinação das suas condições com os controlos de acesso representa uma política de acesso condicional.

![Controlo](./media/active-directory-conditional-access-conditions/61.png)


Condições que não tiver configurado uma política de acesso condicional não são aplicadas. Algumas condições forem [obrigatório](active-directory-conditional-access-best-practices.md#whats-required-to-make-a-policy-work) para aplicar uma política de acesso condicional para um ambiente. 

Este artigo fornece uma descrição geral das condições e como são utilizadas numa política de acesso condicional. 

## <a name="users-and-groups"></a>Utilizadores e grupos

A condição de utilizadores e grupos é obrigatória numa política de acesso condicional. Na sua política, pode selecione **todos os utilizadores** ou selecionar utilizadores e grupos específicos.

![Controlo](./media/active-directory-conditional-access-conditions/111.png)

Quando seleciona:

- **Todos os utilizadores**, a sua política é aplicada a todos os utilizadores no diretório. Isto inclui os utilizadores convidados.

- **Selecionar utilizadores e grupos**, pode definir as seguintes opções:

    - **Todos os utilizadores convidados** -permite-lhe definir como destino uma política para os utilizadores convidados de B2B. Esta condição corresponde a qualquer conta de utilizador com o *userType* atributo definido como *convidado*. Pode utilizar esta definição em casos onde uma política tem de ser aplicadas, assim que a conta é criada no convite de um fluxo no Azure AD.

    - **Funções de diretório** -permite-lhe uma política com base na atribuição de função de um utilizador de destino. Esta condição suporta funções de diretório tal como *Administrador Global* ou *administrador de palavras-passe*.

    - **Utilizadores e grupos** -permite-lhe a conjuntos específicos do destino de utilizadores. Por exemplo, pode selecionar um grupo que contém todos os membros do departamento de RH, quando tiver uma aplicação de RH selecionada como uma aplicação na nuvem.

Um grupo, pode ser qualquer tipo de grupo no Azure AD, incluindo grupos de segurança e a distribuição dinâmicos ou atribuídos

Também pode excluir utilizadores específicos ou grupos de uma política. Um caso de utilização comuns são as contas de serviço, se a política impõe a autenticação multifator (MFA). 

Filtragem específicos conjuntos de utilizadores é útil para a implementação de uma nova política. Na nova política, deve visar apenas um conjunto de utilizadores para validar o comportamento de política inicial. 



## <a name="cloud-apps"></a>Aplicações na cloud 

Uma aplicação de nuvem é um web sites ou serviço. Isto inclui sites web protegidos pelo Proxy de aplicações do Azure. Para obter uma descrição detalhada das aplicações cloud suportados, consulte [atribuição de aplicações da nuvem](active-directory-conditional-access-technical-reference.md#cloud-apps-assignments).    

A condição de aplicações em nuvem é obrigatória numa política de acesso condicional. Na sua política, pode selecione **todas as aplicações em nuvem** ou selecione aplicações específicas.

![Controlo](./media/active-directory-conditional-access-conditions/03.png)

Pode selecionar:

- **Todas as aplicações em nuvem** políticas de linha de base para ser aplicado a toda a organização. Um caso de utilização comum para esta seleção é uma política que requer autenticação multifator quando for detetado o risco de início de sessão para qualquer aplicação na nuvem. Uma política aplicada ao **todas as aplicações em nuvem** aplica-se para o acesso a todos os web sites e serviços. Esta definição não está limitada às aplicações em nuvem que aparecem no **aplicações em nuvem selecione** lista.

- Aplicações de cloud individuais para serviços específicos do destino pela política. Por exemplo, pode exigir que os utilizadores tenham uma [dispositivo compatível com](active-directory-conditional-access-policy-connected-applications.md) para aceder ao SharePoint Online. Também a política é aplicada a outros serviços quando acedem a conteúdo do SharePoint, por exemplo, Teams da Microsoft. 

Também pode excluir aplicações específicas de uma política; No entanto, estas aplicações estão ainda sujeitos as políticas aplicadas aos serviços que acedem a. 



## <a name="sign-in-risk"></a>Risco de início de sessão

Um risco de início de sessão é um indicador para a probabilidade (alta, média ou baixa) que uma tentativa de início de sessão não foi efetuada pelo proprietário de uma conta de utilizador legítimo. Azure AD calcula o nível de risco de início de sessão durante o início de sessão de um utilizador. Pode utilizar o nível de risco de início de sessão calculados como condição numa política de acesso condicional. 

![Condições](./media/active-directory-conditional-access-conditions/22.png)

Para utilizar esta condição, tem de ter [do Azure Active Directory Identity Protection](active-directory-identityprotection.md) ativada.
 
Casos de utilização comum para esta condição são políticas que:

- Impedir que os utilizadores com um risco elevado início de sessão para impedir que utilizadores potencialmente não legítimos aceder às aplicações em nuvem. 
- Exigir autenticação multifator para os utilizadores com um risco de início de sessão médio. Por impor autenticação multifator, pode fornecer confiança adicional que o início de sessão é efetuado pelo proprietário de uma conta legítimo.

Para obter mais informações, veja [Inícios de sessão arriscados](active-directory-identityprotection.md#risky-sign-ins).  

## <a name="device-platforms"></a>Plataformas de dispositivos

A plataforma do dispositivo é caracterizada pelo sistema operativo que está a executar no seu dispositivo. Azure AD identifica a plataforma utilizando as informações fornecidas pelo dispositivo, por exemplo, o agente de utilizador. Porque esta informação é não verificada, recomenda-se que todas as plataformas têm uma política aplicada aos mesmos, ou ao bloquear o acesso, que requerem a conformidade com as políticas do Intune ou que requerem que o dispositivo ser associado a um domínio. A predefinição é para aplicar a política a todas as plataformas de dispositivos. 


![Condições](./media/active-directory-conditional-access-conditions/24.png)

Para uma lista completa das plataformas de dispositivos suportados, consulte [condição de plataforma de dispositivo](active-directory-conditional-access-technical-reference.md#device-platform-condition).


Caso de utilização de um comum para esta condição é uma política que restringe o acesso às suas aplicações em nuvem para [dispositivos geridos](active-directory-conditional-access-policy-connected-applications.md#managed-devices). Para cenários mais, incluindo a condição de plataforma do dispositivo, consulte [acesso condicional de baseado em aplicações do Azure Active Directory](active-directory-conditional-access-mam.md).



## <a name="device-state"></a>Estado do dispositivo

A condição de estado do dispositivo permite associados do Azure AD híbrido e dispositivos marcado como compatíveis para ser excluídos da política de acesso condicional. Isto é útil quando uma política apenas se deve aplicar aos dispositivos não geridos para fornecer segurança adicional de sessão. Por exemplo, apenas impor o controlo de sessão do Microsoft Cloud App Security quando um dispositivo é gerido. 


![Condições](./media/active-directory-conditional-access-conditions/112.png)

Se pretende bloquear o acesso para dispositivos não geridos, deve implementar [acesso condicional baseado no dispositivo](active-directory-conditional-access-policy-connected-applications.md).


## <a name="locations"></a>Localizações

Com as localizações, tem a opção para definir condições que se baseiam em que uma tentativa de ligação foi iniciada a partir da. 
     
![Condições](./media/active-directory-conditional-access-conditions/25.png)

Casos de utilização comum para esta condição são políticas que:

- Exigir a autenticação multifator para os utilizadores que acedem um serviço quando estão fora da rede empresarial.  

- Bloquear o acesso aos utilizadores aceder a um serviço específicos países ou regiões. 

Para obter mais informações, consulte [condições de localização do acesso condicional do Azure Active Directory](active-directory-conditional-access-locations.md).


## <a name="client-apps"></a>Aplicações do cliente

A condição de aplicações de cliente permite-lhe aplicar uma política a diferentes tipos de aplicações, tais como:

- Web sites e serviços
- As aplicações móveis e aplicações de ambiente de trabalho. 



Uma aplicação é classificada como:

- Um web site ou serviço se utiliza os protocolos SSO web, SAML, WS-Fed ou o OpenID Connect para um cliente confidencial.

- A uma aplicação de ambiente de trabalho se utiliza a aplicação móvel OpenID Connect para um cliente nativo ou aplicação móvel.

Para obter uma lista completa das aplicações de cliente pode utilizar na sua política de acesso condicional, consulte [condição de aplicações de cliente](active-directory-conditional-access-technical-reference.md#client-apps-condition) de referência técnica do Azure Active Directory condicional acesso.

Casos de utilização comum para esta condição são políticas que:

- Exigir um [dispositivo compatível com](active-directory-conditional-access-policy-connected-applications.md) para aplicações de ambiente de trabalho e móveis que transferem grandes quantidades de dados para o dispositivo, permitindo que o acesso ao browser partir de qualquer dispositivo.

- Bloquear o acesso de aplicações web, mas permitir o acesso a partir de aplicações de ambiente de trabalho e móveis.

Além de utilizar protocolos de autenticação moderna e de web SSO, pode aplicar esta condição para aplicações de correio que utilizam o Exchange ActiveSync, como as aplicações de correio nativo na maioria dos smartphones. Atualmente, as aplicações de cliente através de protocolos legados tem de ser protegida por AD FS.

Pode selecionar apenas a esta condição se **Exchange Online do Office 365** é apenas aplicação na nuvem que selecionou.

![Aplicações na cloud](./media/active-directory-conditional-access-conditions/32.png)

Selecionar **do Exchange ActiveSync** como aplicações de cliente condição só é suportada se não tiver outras condições numa política configurada. No entanto, pode restringir o âmbito desta condição só se aplicam a plataformas suportadas.

 
![Plataformas suportadas](./media/active-directory-conditional-access-conditions/33.png)

Aplicar esta condição apenas para as plataformas suportadas é o equivalente a todas as plataformas de dispositivos num [condição de plataforma de dispositivo](active-directory-conditional-access-conditions.md#device-platforms).

![Plataformas suportadas](./media/active-directory-conditional-access-conditions/34.png)


 Para obter mais informações, consulte:

- [Configurar o SharePoint Online e Exchange Online para o acesso condicional do Azure Active Directory](active-directory-conditional-access-no-modern-authentication.md)
 
- [Azure Active Directory com base na aplicação acesso condicional](active-directory-conditional-access-mam.md) 


### <a name="legacy-authentication"></a>Autenticação legada  

Acesso condicional aplica-se agora a clientes mais antigos do Office que não suportam a autenticação moderna, bem como os clientes que utilizam os protocolos de correio eletrónico POP, IMAP, SMTP, etc. Isto permite-lhe configurar políticas como **bloquear o acesso a partir de outros clientes**.


![Autenticação legada](./media/active-directory-conditional-access-conditions/160.png)
 



#### <a name="known-issues"></a>Problemas conhecidos

- Configurar uma política para **outros clientes** bloqueia toda a organização de determinados clientes, como SPConnect. Isto acontece porque destes clientes mais antigos autenticar formas inesperado. Este problema não se aplica para o principal de aplicações do Office, como os clientes mais antigos do Office. 

- Pode demorar até 24 horas para a política entre em vigor. 


#### <a name="frequently-asked-questions"></a>Perguntas mais frequentes

**Isto irá bloquear serviços o Exchange Web (EWS)?**

Depende do protocolo de autenticação que EWS está a utilizar. Se a aplicação de EWS utilizem a autenticação moderna, vai ser abordada pela aplicação de cliente "as aplicações móveis e os clientes de ambiente de trabalho". Se a aplicação de EWS estiver a utilizar autenticação básica, vai ser abordada pela aplicação de cliente "Outros clientes".


**Os controlos podem utilizar para outros clientes**

Qualquer controlo pode ser configurado para "Outros clientes de". No entanto, a experiência de utilizador final será bloquear o acesso para todos os casos. "Outros clientes" não suportam controlos, como a MFA, os dispositivos em conformidade, associação a um domínio, etc. 
 
**As condições pode utilizar para outros clientes?**

Quaisquer condições podem ser configuradas para "Outros clientes de".

**Exchange ActiveSync suporta todas as condições e controlos?**

Não. Eis o resumo de suporte Exchange ActiveSync (EAS):

- EAS só suporta filtragem de grupo e utilizador. Não suporta o convidado, funções. Se a condição de convidado/função estiver configurada, todos os utilizadores serão bloqueados, uma vez que vamos não é possível determinar se deve aplicar a política para o utilizador, ou não.

- EAS só funciona com o Exchange que a aplicação de nuvem. 

- EAS não suporta qualquer condição, exceto a aplicação de cliente em si.

- EAS pode ser configurado com qualquer controlo (tudo, exceto a conformidade do dispositivo irá causar blocos).

**As políticas se aplicam a todas as aplicações de cliente por predefinição, passa?**

Não. Não há nenhuma alteração no comportamento de política predefinido. As políticas de continuam a aplicar ao browser e clientes de aplicações móveis/ambiente de trabalho por predefinição.



## <a name="next-steps"></a>Passos Seguintes

- Se quiser saber como configurar uma política de acesso condicional, consulte [introdução ao acesso condicional no Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

- Se estiver pronto para configurar políticas de acesso condicional para o seu ambiente, consulte o [melhores práticas para acesso condicional no Azure Active Directory](active-directory-conditional-access-best-practices.md). 

