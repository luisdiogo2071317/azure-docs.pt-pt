---
title: "Problema ao guardar as credenciais de administrador ao configurar o aprovisionamento de utilizadores a uma aplicação de galeria do Azure AD | Microsoft Docs"
description: "Como resolver problemas comuns deparam quando configurar aprovisionamento de utilizadores a uma aplicação já listados na Galeria de aplicações do Azure AD"
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: asmalser
ms.openlocfilehash: 6617345c8923b1fc8081b01ddfe8b4bedf10b6ea
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/27/2018
---
# <a name="problem-saving-administrator-credentials-while-configuring-user-provisioning-to-an-azure-active-directory-gallery-application"></a>Problema ao guardar as credenciais de administrador ao configurar o aprovisionamento de utilizadores a uma aplicação de galeria do Azure Active Directory 

Quando utilizar o portal do Azure para configurar [aprovisionamento de utilizadores automática](active-directory-saas-app-provisioning.md) para uma aplicação empresarial, pode encontrar uma situação em que:

* O **credenciais de administrador** introduzida para a aplicação são válidos e o **Testar ligação** botão funciona. No entanto, não não possível guardar as credenciais e o portal do Azure devolve uma mensagem de erro genérica.

Se baseados em SAML-início de sessão único também estiver configurado para a mesma aplicação, a causa do erro mais provável é o limite de armazenamento interno, por aplicação do Azure AD para os certificados e as credenciais foi excedido.

Azure AD atualmente tem uma capacidade de armazenamento máximo de um kilobytes para todos os certificados, tokens secretos, credenciais e dados de configuração relacionadas com associados a uma única instância de uma aplicação (também conhecido como um serviço principal registo no Azure AD).

Quando baseados em SAML-início de sessão único é configurado, o certificado utilizado para assinar os tokens SAML é armazenado aqui e, muitas vezes, mais consome 50% por cento do espaço.

Qualquer tokens secretos, os URIs, endereços de correio eletrónico de notificação, os nomes de utilizador e palavras-passe que obterem introduzidas durante a configuração do aprovisionamento de utilizadores podem fazer com que o limite de armazenamento seja excedido.

## <a name="how-to-work-around-this-issue"></a>Como resolver este problema 

Existem duas formas possíveis para contornar este problema hoje:

1. **Utilizar a Galeria duas instâncias da aplicação, um para o início de sessão único e outra para o aprovisionamento de utilizadores** -colocar a aplicação de galeria [LinkedIn elevar](active-directory-saas-linkedinelevate-tutorial.md) como exemplo, pode adicionar elevar o LinkedIn da galeria do e configurar -o para o início de sessão único. Para o aprovisionamento, adicione outra instância do LinkedIn elevar da Galeria de aplicações do Azure AD e dê-lhe o nome "LinkedIn Elevate (aprovisionamento)." Esta segunda instância, configurar [aprovisionamento](active-directory-saas-linkedinelevate-provisioning-tutorial.md), mas não único início de sessão. Quando utilizar esta solução, os mesmos utilizadores e grupos têm de ser [atribuído](active-directory-coreapps-assign-user-azure-portal.md) para ambas as aplicações. 

2. **Reduzir a quantidade de dados de configuração armazenadas** -todos os dados introduzidos no [credenciais de administrador](active-directory-saas-app-provisioning.md#how-do-i-set-up-automatic-provisioning-to-an-application) secção do separador de aprovisionamento é armazenada no mesmo local como o certificado SAML. Enquanto pode não ser possível reduzir o período de todos estes dados, alguns campos de configuração opcionais, como o **correio eletrónico de notificação** pode ser removido.

## <a name="next-steps"></a>Passos Seguintes
[Configurar o utilizador e aprovisionamento de aprovisionamento automatizados para aplicações SaaS](active-directory-saas-app-provisioning.md)
