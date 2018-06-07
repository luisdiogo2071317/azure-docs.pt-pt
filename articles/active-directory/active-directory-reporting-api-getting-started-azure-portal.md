---
title: Começar a utilizar o Azure AD reporting API | Microsoft Docs
description: Como começar com o Azure Active Directory API do relatório
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 05/07/2018
ms.author: dhanyahk;rolyon
ms.reviewer: dhanyahk
ms.openlocfilehash: df0b672a07575a0d26fff89c90008043d02818dd
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34589105"
---
# <a name="get-started-with-the-azure-active-directory-reporting-api"></a>Introdução ao Azure Active Directory API do relatório

Azure Active Directory disponibiliza uma variedade de [relatórios](active-directory-reporting-azure-portal.md). Os dados destes relatórios podem ser bastante úteis para as suas aplicações, como sistemas SIEM e ferramentas de auditoria e de business intelligence. 

Ao utilizar o Azure AD API do relatório, pode obter acesso programático para os dados através de um conjunto de APIs baseado em REST. Pode chamar estas APIs a partir de várias linguagens e ferramentas de programação.

Este artigo fornece um plano para aceder os dados de relatórios utilizando a API relacionada.

Caso se depare com problemas, consulte [como obter suporte do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto).


## <a name="prerequisites"></a>Pré-requisitos

Para aceder à API do Reporting Services, mesmo se estiver a planear no aceder à API através de um script, tem de:

1. Atribuir funções (leitor de segurança, o administrador de segurança, o Administrador Global)
2. Registar uma aplicação
3. Conceder permissões
4. Recolher as definições de configuração


 
Para obter instruções detalhadas, consulte o [pré-requisitos para aceder ao Azure Active Directory API do relatório](active-directory-reporting-api-prerequisites-azure-portal.md).


## <a name="recommendation"></a>Recomendação 

Se estiver a planear a obter dados de relatórios sem intervenção do utilizador, deve considerar a utilização do Azure AD API de relatórios com certificados.

Para obter instruções detalhadas, consulte [obter dados através da API de relatórios do AD do Azure com certificados](active-directory-reporting-api-with-certificates.md).


## <a name="explore"></a>Explorar

Obter uma primeira impressão das APIs Reporting Services:
   
   - [Utilizando os exemplos para a API de auditoria](active-directory-reporting-api-audit-samples.md) 
 
   - [Os exemplos a utilizar para o relatório de atividade de início de sessão API](active-directory-reporting-api-sign-in-activity-samples.md)


## <a name="customize"></a>Personalizar  

Crie a sua própria solução: 
   
   - [Utilizar a referência da API de auditoria](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 

   - [Utilizar a referência da API do relatório de atividade de início de sessão](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)



