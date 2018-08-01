---
title: Introdução à API de relatórios do Azure AD | Documentos da Microsoft
description: Como começar com o Azure Active Directory reporting API
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 05/07/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 89562e6a2bfb977585cec1925a5f306c69c3d1e8
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39390675"
---
# <a name="get-started-with-the-azure-active-directory-reporting-api"></a>Introdução ao Azure Active Directory reporting API

O Azure Active Directory fornece-lhe diversas [relatórios](active-directory-reporting-azure-portal.md). Os dados destes relatórios podem ser bastante úteis para as suas aplicações, como sistemas SIEM e ferramentas de auditoria e de business intelligence. 

Ao utilizar a API de relatórios do Azure AD, pode obter acesso programático aos dados através de um conjunto de APIs baseadas em REST. Pode chamar estas APIs a partir de várias linguagens e ferramentas de programação.

Este artigo fornece um mapa para acessar os dados de relatório com a API relacionada.

Caso se depare com problemas, consulte [como obter suporte do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto).


## <a name="prerequisites"></a>Pré-requisitos

Para acessar a API de relatórios, mesmo se estiver a planear a aceder à API usando um script, terá de:

1. Atribuir funções (Administrador Global do leitor de segurança, administrador de segurança)
2. Registar uma aplicação
3. Conceder permissões
4. Recolher as definições de configuração

Para obter instruções detalhadas, consulte a [pré-requisitos para aceder ao Azure Active Directory reporting API](active-directory-reporting-api-prerequisites-azure-portal.md).

## <a name="apis-with-graph-explorer"></a>APIs com API do Graph

Pode utilizar o [MSGraph explorer](https://developer.microsoft.com/en-us/graph/graph-explorer) para verificar seu início de sessão e dados da API de auditoria. Certifique-se de iniciar sessão na sua conta com ambos os botões de início de sessão na IU do Explorador do gráfico e defina **AuditLog.Read.All** e **Directory.Read.All** permissões para o seu inquilino, conforme mostrado.   

![API do Graph](./media/active-directory-reporting-api-getting-started-azure-portal/graph-explorer.png)

![Modificar as permissões da interface do Usuário](./media/active-directory-reporting-api-getting-started-azure-portal/modify-permissions.png)

## <a name="use-certificates-to-access-the-azure-ad-reporting-api"></a>Utilizar certificados para aceder a relatórios a API do Azure AD 

Considere utilizar a API de relatórios do AD do Azure com certificados, se pretender recuperar dados de relatórios sem intervenção do utilizador.

Para obter instruções detalhadas, consulte [utilizar a API de relatórios do AD do Azure com certificados para obter dados](active-directory-reporting-api-with-certificates.md).


## <a name="explore"></a>Explorar

Obtenha sua primeira impressão às APIs de relatórios:
   
   - [Utilizar os exemplos da API de auditoria](active-directory-reporting-api-audit-samples.md) 
   - [Utilizar os exemplos para a API de relatório de atividade de início de sessão](active-directory-reporting-api-sign-in-activity-samples.md)


## <a name="next-steps"></a>Passos Seguintes

 * [Referência da API de auditoria](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
 * [Relatório de atividade de início de sessão de referência da API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
 * [Resolver erros no Azure AD a API de relatórios](active-directory-reporting-troubleshoot-graph-api.md)


