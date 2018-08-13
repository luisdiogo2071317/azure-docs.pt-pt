---
title: O que é o acesso condicional no Azure Active Directory? | Microsoft Docs
description: Saiba como é que o acesso condicional no Azure Active Directory o ajuda a implementar decisões de acesso automatizadas que, não só têm por base quem tenta aceder a um recurso, como também de que forma se acede a esse recurso.
services: active-directory
keywords: acesso condicional a aplicações, acesso condicional com o Azure AD, acesso seguro a recursos da empresa, políticas de acesso condicional
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: protection
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/18/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 7483cda4b2170e18549ebfbe0c8d30f8d3016013
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39532034"
---
# <a name="what-is-conditional-access-in-azure-active-directory"></a>O que é o acesso condicional no Azure Active Directory?

A segurança é uma das principais preocupações para as organizações que utilizam a cloud. Um dos principais aspetos da segurança na cloud é a identidade e o acesso no que diz respeito a gerir os recursos na cloud. Num mundo “mobile-first, cloud-first”, os utilizadores podem aceder aos recursos da sua organização com diversos dispositivos e aplicações em qualquer local. Como resultado, limitar-se a concentrar-se apenas em quem pode aceder a um recurso já não é suficiente. Para controlar o equilíbrio entre segurança e produtividade, também tem de ter em conta de que forma é que se acede a um determinado recurso numa decisão de controlo de acesso. Com o acesso condicional do Azure AD, pode solucionar este requisito. O acesso condicional é uma capacidade do Azure Active Directory. Permite-lhe implementar decisões de controlo de acesso automatizadas relativamente ao acesso às aplicações na cloud que têm por base condições. 

![Controlo](./media/active-directory-conditional-access-azure-portal/81.png)

Este artigo mostra-lhe uma descrição geral conceptual do acesso condicional no Azure AD.



## <a name="common-scenarios"></a>Cenários comuns

Num mundo “mobile-first, cloud-first”, o Azure Active Directory proporciona o início de sessão único em dispositivos, aplicações e serviços, em qualquer local. Com a proliferação de dispositivos (incluindo dispositivos BYOD), do trabalho fora das redes das empresas e das aplicações SaaS de terceiros, as equipas de TI estão perante dois objetivos opostos:

- Capacitar os utilizadores a ser produtivos em qualquer local e em qualquer momento
- Proteger os recursos da empresa em todos os momentos

Com a utilização de políticas de acesso condicional, pode aplicar os controlos de acesso certos sob as condições necessárias. O acesso condicional do Azure AD oferece-lhe mais segurança quando é preciso e não atrapalha os utilizadores quando não é preciso. 

Seguem-se algumas preocupações comuns relativas ao acesso e com as quais o acesso condicional pode ajudar:



- **[Risco de início de sessão](conditional-access/conditions.md#sign-in-risk)**: o Azure AD Identity Protection deteta riscos de início de sessão. Como restringe o acesso se um risco de início de sessão detetado indicar um ator malicioso? E se quiser ter provas mais sólidas de que um início de sessão foi feito por um utilizador legítimo? E se as suas dúvidas forem tão fundamentadas para o levarem inclusivamente a impedir utilizadores específicos de aceder a uma aplicação? Ao configurar 

- **[Localização da rede](conditional-access/location-condition.md)**: o Azure AD está acessível em todo o lado. E se alguém tentar aceder numa localização de rede que não está sob o controlo do seu departamento de TI? A utilização de uma combinação de nome de utilizador e palavra-passe pode ser uma prova de identidade suficientemente forte para tentativas de acesso aos seus recursos feitas a partir da rede da sua empresa. E se precisar de uma prova de identidade mais sólida para as tentativas de acesso que são iniciadas noutros países ou regiões do mundo inesperados? E se quiser até bloquear tentativas de acesso em determinadas localizações?  

- **[Gestão de dispositivos](conditional-access/conditions.md#device-platforms)**: no Azure AD, os utilizadores podem aceder às aplicações na cloud a partir de vários dispositivos, incluindo dispositivos móveis e também pessoais. E se quiser que as tentativas de acesso sejam feitas apenas em dispositivos geridos pelo seu departamento de TI? E se quiser inclusivamente impedir determinados tipos de dispositivos de aceder às aplicações na cloud no seu ambiente? 

- **[Aplicação cliente](conditional-access/conditions.md#client-apps)**: nos nossos dias, é possível aceder a muitas aplicações na cloud através de diferentes tipos de aplicações, como aplicações baseadas na Web, aplicações móveis ou aplicações de ambiente de trabalho. E se for feita uma tentativa de acesso com um tipo de aplicação cliente que origina problemas conhecidos? E se quiser que seja utilizado um dispositivo gerido pelo departamento de TI para determinados tipos de aplicações? 

Estas perguntas, e as respostas às mesmas, representam cenários de acesso comuns para o acesso condicional do Azure Ad. O acesso condicional é uma capacidade do Azure Active Directory que lhe permite utilizar uma abordagem baseada em políticas para lidar com os cenários de acesso.


## <a name="conditional-access-policies"></a>Políticas de acesso condicional

As políticas de acesso condicional são uma definição de um cenário de acesso que utilizam o seguinte padrão:

![Controlo](./media/active-directory-conditional-access-azure-portal/10.png)

**Then do this** (Depois, fazer isto) define a resposta da política. É importante ter em conta que o objeto das políticas de acesso condicional não é conceder acesso a uma aplicação na cloud. No Azure AD, a concessão do acesso às aplicações na cloud está sujeita a atribuições de utilizador. Com uma política de acesso condicional, pode controlar de que forma é que os utilizadores autorizados (utilizadores a quem foi concedido acesso a uma aplicação na cloud) podem aceder a aplicações na cloud sob determinadas condições. Na resposta, vai aplicar requisitos adicionais, como a autenticação multifator, um dispositivo gerido, entre outros. No contexto do acesso condicional do Azure AD, os requisitos que a política aplica são denominados controlos de acesso. Na sua forma mais restritiva, a sua política pode bloquear o acesso. Para obter mais informações, veja [Access controls in Azure Active Directory conditional access](conditional-access/controls.md) (Controlos de acesso no acesso condicional do Azure Active Directory).
     

**When this happens** (Quando isto acontece) define o motivo para acionar a política. Esse motivo é caracterizado por um conjunto de condições que foram verificadas. No acesso condicional do Azure AD, as duas condições de atribuição desempenham papéis especiais:

- **[Utilizadores](conditional-access/conditions.md#users-and-groups)**: os utilizadores que tentam aceder (**Quem**). 

- **[Aplicações na cloud](conditional-access/conditions.md#cloud-apps)**: os alvos de uma tentativa de acesso (**O quê**).    

Estas duas condições são obrigatórias nas políticas de acesso condicional. Para além dessas duas condições obrigatórias, também pode incluir outras que descrevem a forma como a tentativa de acesso é feita. Os exemplos mais comuns são utilizar dispositivos móveis ou localizações fora da rede da sua empresa. Para obter mais informações, veja [Conditions in Azure Active Directory conditional access](conditional-access/conditions.md) (Condições no acesso condicional do Azure Active Directory).   

A combinação de condições e dos controlos de acesso representa uma política de acesso condicional. 

![Controlo](./media/active-directory-conditional-access-azure-portal/51.png)

Com o acesso condicional do Azure AD, pode controlar a forma como os utilizadores autorizados podem aceder às aplicações na cloud. O objetivo das políticas de acesso condicional é aplicar controlos de acesso adicionais a tentativas de acesso a uma aplicação na cloud que são condicionados pela forma como essas tentativas são feitas.

Uma das vantagens da utilização de uma abordagem baseada em políticas para proteger o acesso às aplicações na cloud é poder começar a desenhar os requisitos das políticas para o seu ambiente com a estrutura descrita neste artigo sem se preocupar com a implementação técnica. 


## <a name="license-requirements-for-using-conditional-access"></a>Requisitos de licenciamento para o acesso condicional

A utilização do acesso condicional requer uma licença do Azure AD Premium. Para encontrar a licença certa para os seus requisitos, veja [Comparação das funcionalidades disponíveis geralmente das edições Gratuita, Básica e Premium](https://azure.microsoft.com/pricing/details/active-directory/).


## <a name="next-steps"></a>Passos seguintes

- Se quiser saber mais sobre:
    - Condições, veja [Conditions in Azure Active Directory conditional access](conditional-access/conditions.md) (Condições no acesso condicional do Azure Active Directory).

    - Controlos de acesso, veja [Access controls in Azure Active Directory conditional access](conditional-access/controls.md) (Controlos de acesso no acesso condicional do Azure Active Directory).

- Se quiser obter alguma experiência na configuração de políticas de acesso condicional, veja [Require MFA for specific apps with Azure Active Directory conditional access](conditional-access/app-based-mfa.md) (Pedir MFA para aplicações específicas com o acesso condicional do Azure Active Directory).

- Se estiver pronto para configurar políticas de acesso condicional para o seu ambiente, veja [Best practices for conditional access in Azure Active Directory](conditional-access/best-practices.md) (Melhores práticas do acesso condicional no Azure Active Directory). 

- Se quiser um plano de implementação passo a passo com políticas recomendadas, veja [Conditional access deployment plan](http://aka.ms/conditionalaccessdeploymentplan) (Plano de implementação do acesso condicional).
