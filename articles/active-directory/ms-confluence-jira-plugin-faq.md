---
title: "Microsoft Azure Active Directory único início de sessão Plug-in FAQ | Microsoft Docs"
description: "Saiba como configurar o início de sessão entre o Azure Active Directory e do Microsoft Azure Active Directory-início de sessão único para JIRA."
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
ms.openlocfilehash: 571fbd5078f66375f6e81cba2a790121366f9d60
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="microsoft-azure-active-directory-single-sign-on-plugin-faq"></a>Microsoft Azure Active Directory único início de sessão Plug-in FAQ 

## <a name="1-whats-the-microsoft-sso-add-on"></a>1. O que é o suplemento do Microsoft SSO?

Este suplemento fornece a capacidade de início de sessão único para do Atlassian JIRA (incluindo JIRA núcleos, JIRA Software, JIRA suporte técnico) e software do Confluence no local. O suplemento do funciona com o Azure AD como IdP.

## <a name="2-add-on-works-with-which-atlassian-products"></a>2. Suplemento funciona com os produtos Atlassian?

A partir de agora, o suplemento do funciona com versões de local de JIRA e Confluence.

## <a name="3-does-this-add-on-work-on-cloud-version"></a>3. Este suplemento funciona na versão na nuvem?

Não. Só são suportadas no local versões de JIRA e Confluence.

## <a name="4-which-versions-of-jira-and-confluence-are-supported"></a>4. Quais as versões do JIRA e Confluence são suportadas?

Segue-se a lista de versões que são suportados:

* JIRA núcleos e do Software: 6.0 para 7.2.2 
* JIRA serviço para o suporte técnico: 3.0 para 3.2 
* Confluence: 5.0 à versão 5.10

## <a name="5-is-this-add-on-free-or-paid"></a>5. É este suplemento gratuito ou Paid?

Este é um suplemento livre e pode ser instalado na Atlassian Marketplace.

## <a name="6-do-i-need-to-restart-jiraconfluence-once-i-deploy-the-add-on"></a>6. É necessário reiniciar JIRA/Confluence depois de implementar o suplemento

Reinício não é necessário post suplemento implementação. Pode começar a utilizar o suplemento do imediatamente após a implementação.

## <a name="7-how-do-i-get-support-for-the-add-on"></a>7. Como posso obter suporte para o suplemento do?

Entrar-nos em: <email> . Iremos responderá dentro <> horas. Também pode emitir um pedido de suporte com a Microsoft através do canal de portal do Azure. Também pode chamá-nos em: <Number> entre <> estou a <> pm em dias da semana.

## <a name="8-would-this-add-on-work-on-mac-or-ubuntu-installation-of-jira-and-confluence"></a>8. Este suplemento funciona numa instalação Mac ou Ubuntu do JIRA e Confluence?

Foi testado este suplemento apenas em instalações de servidor do Windows de 64 bits de JIRA e confluence.

## <a name="9-does-this-add-on-work-with-other-idps-than-azure-ad"></a>9. Este suplemento funciona com outros IdPs ao Azure AD?

Não. Suplemento funciona apenas com o Azure AD.

## <a name="10-what-version-of-saml-does-the-add-on-work-with"></a>10. Qual a versão de SAML funciona o suplemento com?

Suplemento funciona com SAML 2.0.

## <a name="11-does-the-add-on-do-use-provisioning-as-well"></a>11. É o suplemento do utilizar, bem como o aprovisionamento?

Não. A partir de agora suplemento fornece apenas SAML 2.0 com base em SSO. Utilizador tem de ser aprovisionados na aplicação antes do início de sessão do SSO.

## <a name="12-are-cluster-versions-of-jira-and-confluence-supported-by-add-on"></a>12. São versões de cluster do JIRA e confluence suportado pelo suplemento?

Não. O suplemento do funciona com versões de local de JIRA e Confluence.

## <a name="13-would-this-plugin-work-with-http-version-of-jira-and-confluence"></a>13. Este plug-in funciona com a versão HTTP do JIRA e Confluence?

Não. Funciona o suplemento com HTTPS ativada apenas instalações.

## <a name="14-do-i-need-to-buy-license-of-the-add-on"></a>14. É necessário comprar licenças do suplemento?

É um suplemento livre.