---
title: "Condições de acesso condicional do Azure Active Directory | Microsoft Docs"
description: "Saiba como atribuições são utilizadas no acesso condicional do Azure Active Directory para acionar uma política."
services: active-directory
keywords: "acesso condicional para aplicações, o acesso condicional com o Azure AD, o acesso seguro aos recursos da empresa, as políticas de acesso condicional"
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/08/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 22cc43a861e2a9d1d0c508da362a5f0b48d0c268
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="conditions-in-azure-active-directory-conditional-access"></a>Condições de acesso condicional do Azure Active Directory 

Com [acesso condicional do Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), pode controlar o acesso de utilizadores autorizados como as suas aplicações em nuvem. Na política de acesso condicional, é possível definir a resposta ("fazê-lo") para a razão para acionar a política ("quando isto acontece"). 

![Controlo](./media/active-directory-conditional-access-conditions/10.png)


No contexto do acesso condicional:

- "**Quando isto acontece**" denomina **condições**. 
- "**, Em seguida, fazer isto**" denomina **controlos de acesso**.

A combinação das suas condições com os controlos de acesso representa uma política de acesso condicional.

![Controlo](./media/active-directory-conditional-access-conditions/61.png)

Este artigo fornece uma descrição geral das condições e como são utilizadas numa política de acesso condicional. 


## <a name="users-and-groups"></a>Utilizadores e grupos

A condição de utilizadores e grupos é obrigatória numa política de acesso condicional. Na sua política, pode selecione **todos os utilizadores** ou selecionar utilizadores e grupos específicos.

![Controlo](./media/active-directory-conditional-access-conditions/02.png)

Quando seleciona:

- **Todos os utilizadores**, a sua política é aplicada a todos os utilizadores no diretório. Isto inclui os utilizadores convidados.

- **Selecionar utilizadores e grupos**, poderá ter como alvo específicos conjuntos de utilizadores, por exemplo, todos os membros do departamento de RH, quando iniciam sessão numa aplicação HR. 

- Um grupo, pode ser qualquer tipo de grupo no Azure AD, incluindo grupos de segurança e a distribuição dinâmicos ou atribuídos.

Também pode excluir utilizadores específicos ou grupos de uma política. Um caso de utilização comuns são as contas de serviço, se a política impõe a autenticação multifator. 

Filtragem específicos conjuntos de utilizadores é útil para a implementação de uma nova política. Na nova política, deve visar apenas um conjunto de utilizadores para validar o comportamento de política inicial. 



## <a name="cloud-apps"></a>Aplicações na cloud 

Uma aplicação de nuvem é um web sites ou serviço. Isto inclui sites web protegidos pelo Proxy de aplicações do Azure. Para obter uma descrição detalhada das aplicações cloud suportados, consulte [atribuição de aplicações da nuvem](active-directory-conditional-access-technical-reference.md#cloud-apps-assignments).    

A condição de aplicações em nuvem é obrigatória numa política de acesso condicional. Na sua política, pode selecione **todas as aplicações em nuvem** ou selecione aplicações específicas.

![Controlo](./media/active-directory-conditional-access-conditions/03.png)

Pode selecionar:

- **Todas as aplicações em nuvem** políticas de linha de base para ser aplicado a toda a organização. Um caso de utilização comum para esta seleção é uma política que requer autenticação multifator quando for detetado o risco de início de sessão para qualquer aplicação na nuvem.

- Aplicações de cloud individuais para serviços específicos do destino pela política. Por exemplo, pode exigir que os utilizadores tenham uma [dispositivo compatível com](active-directory-conditional-access-policy-connected-applications.md) para aceder ao SharePoint Online. Também a política é aplicada a outros serviços quando acedem a conteúdo do SharePoint, por exemplo, Teams da Microsoft. 

Também pode excluir aplicações específicas de uma política; No entanto, estas aplicações estão ainda sujeitos as políticas aplicadas aos serviços que acedem a. 



## <a name="sign-in-risk"></a>Risco de início de sessão

Um risco de início de sessão é um indicador para a probabilidade (alta, média ou baixa) que uma tentativa de início de sessão não foi efetuada pelo proprietário de uma conta de utilizador legítimo. Azure AD calcula o nível de risco de início de sessão durante o início de sessão de um utilizador. Pode utilizar o nível de risco de início de sessão calculados como condição numa política de acesso condicional. 

![Condições](./media/active-directory-conditional-access-conditions/22.png)

Para utilizar esta condição, tem de ter [do Azure Active Directory Identity Protection](active-directory-identityprotection.md) ativada.
 
Casos de utilização comum para esta condição são políticas que:

- Bloquear os utilizadores com um risco elevado início de sessão para impedir que utilizadores potencialmente não legítimos aceder às aplicações em nuvem. 
- Exigir a autenticação multifator quando a utilizadores com um risco de início de sessão médio. Por impor autenticação multifator, pode fornecer confiança adicional que o início de sessão é efetuado pelo proprietário de uma conta legítimo.

Para obter mais informações, veja [Inícios de sessão arriscados](active-directory-identityprotection.md#risky-sign-ins).  

## <a name="device-platforms"></a>Plataformas de dispositivos

A plataforma do dispositivo é caracterizada pelo sistema operativo que está a executar no seu dispositivo. Azure AD identifica a plataforma utilizando as informações fornecidas pelo dispositivo, por exemplo, o agente de utilizador. Porque esta informação é não verificada, recomenda-se que todas as plataformas têm uma política aplicada aos mesmos, ou ao bloquear o acesso, que requerem a conformidade com as políticas do Intune ou que requerem que o dispositivo ser associado a um domínio. A predefinição é para aplicar a política a todas as plataformas de dispositivos. 


![Condições](./media/active-directory-conditional-access-conditions/02.png)

Para uma lista completa das plataformas de dispositivos suportados, consulte [condição de plataforma de dispositivo](active-directory-conditional-access-technical-reference.md#device-platform-condition).


Caso de utilização de um comum para esta condição é uma política que restringe o acesso às suas aplicações em nuvem para [dispositivo fidedigno](active-directory-conditional-access-policy-connected-applications.md#trusted-devices). Para cenários mais, incluindo a condição de plataforma do dispositivo, consulte [acesso condicional de baseado em aplicações do Azure Active Directory](active-directory-conditional-access-mam.md).


## <a name="locations"></a>Localizações

Com as localizações, tem a opção para definir condições que se baseiam em que uma tentativa de ligação foi iniciada a partir da. 
     
![Condições](./media/active-directory-conditional-access-conditions/03.png)

Casos de utilização comum para esta condição são políticas que:

- Exigir a autenticação multifator para os utilizadores que acedem um serviço quando estão fora da rede empresarial.  

- Bloquear o acesso aos utilizadores aceder a um serviço específicos países ou regiões. 

Para obter mais informações, consulte [condições de localização do acesso condicional do Azure Active Directory](active-directory-conditional-access-locations.md).


## <a name="client-apps"></a>Aplicações do cliente

A condição de aplicações de cliente permite-lhe aplicar uma política a diferentes tipos de aplicações, tais como:

- Web sites e serviços
- As aplicações móveis e aplicações de ambiente de trabalho. 

![Condições](./media/active-directory-conditional-access-conditions/04.png)

Uma aplicação é classificada como:

- Um web site ou serviço se utiliza os protocolos SSO web, SAML, WS-Fed ou o OpenID Connect para um cliente confidencial.

- A uma aplicação de ambiente de trabalho se utiliza a aplicação móvel OpenID Connect para um cliente nativo ou aplicação móvel.

Para obter uma lista completa das aplicações de cliente pode utilizar na sua política de acesso condicional, consulte o [referência técnica do Azure Active Directory condicional acesso](active-directory-conditional-access-technical-reference.md#client-apps-condition).

Casos de utilização comum para esta condição são políticas que:

- Exigir um [dispositivo compatível com](active-directory-conditional-access-policy-connected-applications.md) para aplicações de ambiente de trabalho e móveis que transferem grandes quantidades de dados para o dispositivo, permitindo que o acesso ao browser partir de qualquer dispositivo.

- Bloquear o acesso de aplicações web, mas permitir o acesso a partir de aplicações de ambiente de trabalho e móveis.

Além de utilizar protocolos de autenticação moderna e de web SSO, pode aplicar esta condição para aplicações de correio que utilizam o Exchange ActiveSync, como as aplicações de correio nativo na maioria dos smartphones. Atualmente, as aplicações de cliente através de protocolos legados tem de ser protegida por AD FS.

 Para obter mais informações, consulte:

- [Configurar o SharePoint Online e Exchange Online para o acesso condicional do Azure Active Directory](active-directory-conditional-access-no-modern-authentication.md)
 
- [Azure Active Directory com base na aplicação acesso condicional](active-directory-conditional-access-mam.md) 








## <a name="next-steps"></a>Passos Seguintes

- Se quiser saber como configurar uma política de acesso condicional, consulte [introdução ao acesso condicional no Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

- Se estiver pronto para configurar políticas de acesso condicional para o seu ambiente, consulte o [melhores práticas para acesso condicional no Azure Active Directory](active-directory-conditional-access-best-practices.md). 

