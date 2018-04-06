---
title: Perguntas mais frequentes sobre o SSO do diretório de Active Directory do Azure Plug-in | Microsoft Docs
description: Obtenha respostas às perguntas mais frequentes sobre como configurar o início de sessão entre o Azure Active Directory e Jira/Confluence.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4b663047-7f88-443b-97bd-54224b232815
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2018
ms.author: jeedes
ms.openlocfilehash: b642d6de9fd9265d7f7ee11b827a36d20e5bcce7
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
# <a name="faq-for-the-azure-active-directory-sso-plug-in"></a>Perguntas mais frequentes sobre o SSO do diretório de Active Directory do Azure Plug-in 

## <a name="what-does-the-plug-in-do"></a>O que faz o plug-in efetue?

O plug-in fornece a capacidade de-início de sessão único (SSO) para Atlassian Jira (incluindo Jira núcleos, Jira Software, Jira suporte técnico) e o software do Confluence no local. O plug-in funciona com o Azure Active Directory (Azure AD) como um fornecedor de identidade (IdP).

## <a name="which-atlassian-products-does-the-plug-in-work-with"></a>Os produtos Atlassian é o plug-in funcionam com?

O plug-in funciona com versões no local de Jira e Confluence.

## <a name="does-the-plug-in-work-on-cloud-versions"></a>É o plug-in funcionam em versões de nuvem?

Não. O plug-in suporta apenas no local as versões do Jira e Confluence.

## <a name="which-versions-of-jira-and-confluence-does-the-plug-in-support"></a>Quais as versões do Jira e Confluence é o suporte de plug-in?

O plug-in suporta estas versões:

* Jira núcleos e do Software: 6.0 para 7.2.2 
* Suporte técnico do serviço de Jira: 3.0 para 3.2 
* Confluence: 5.0 à versão 5.10

## <a name="is-the-plug-in-free-or-paid"></a>É o plug-in gratuita ou paga?

É um suplemento livre. Pode instalá-lo no Marketplace Atlassian.

## <a name="do-i-need-to-restart-jira-or-confluence-after-i-deploy-the-plug-in"></a>É necessário reiniciar Jira ou Confluence depois de implementar o plug-in

Não é necessário um reinício. Pode começar a utilizar o plug-in imediatamente.

## <a name="how-do-i-get-support-for-the-plug-in"></a>Como posso obter suporte para o plug-in?

Entrar o [equipa de integração do Azure AD SSO](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). A equipa responde no 48 de 24 horas de expediente. 

Também pode emitir um pedido de suporte com a Microsoft através do canal de portal do Azure.

## <a name="would-the-plug-in-work-on-a-mac-or-ubuntu-installation-of-jira-and-confluence"></a>Seria o trabalho de plug-in numa instalação Mac ou Ubuntu do Jira e Confluence?

Ter testámos o plug-in apenas em instalações do Windows Server de 64 bits do Jira e Confluence.

## <a name="does-the-plug-in-work-with-idps-other-than-azure-ad"></a>É o plug-in funcionam com IdPs diferente do Azure AD?

Não. Funciona apenas com o Azure AD.

## <a name="what-version-of-saml-does-the-plug-in-work-with"></a>Que versão do SAML não o trabalho com o plug-in?

Funciona com SAML 2.0.

## <a name="does-the-plug-in-do-user-provisioning"></a>Faz o plug-in? aprovisionamento de utilizadores

Não. O plug-in fornece baseados em SAML 2.0 SSO apenas. O utilizador tem de ser aprovisionados na aplicação antes do SSO início de sessão.

## <a name="does-the-plug-in-support-cluster-versions-of-jira-and-confluence"></a>É as suporte de plug-in cluster versões Jira e Confluence?

Não. O plug-in funciona com versões no local de Jira e Confluence.

## <a name="does-the-plug-in-work-with-http-versions-of-jira-and-confluence"></a>É o plug-in funcionam com versões HTTP de Jira e Confluence?

Não. O plug-in funciona com instalações ativadas por HTTPS.