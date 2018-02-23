---
title: "Sincronização do Azure AD Connect: extensões de diretórios | Microsoft Docs"
description: "Este tópico descreve a funcionalidade de extensões de diretório no Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 995ee876-4415-4bb0-a258-cca3cbb02193
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 4430f445a836f4baa90511c71bb734eda8674249
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/22/2018
---
# <a name="azure-ad-connect-sync-directory-extensions"></a>Sincronização do Azure AD Connect: extensões de diretórios
Pode utilizar extensões de diretórios para expandir o esquema no Azure Active Directory (Azure AD) com os seus próprios atributos do Active Directory no local. Esta funcionalidade permite-lhe criar aplicações de LOB ao consumir atributos que continua a gerir no local. Estes atributos podem ser consumidos através de [extensões de diretórios do Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) ou [Microsoft Graph](https://graph.microsoft.io/). Pode ver os atributos disponíveis utilizando [Explorador do Azure AD Graph](https://graphexplorer.azurewebsites.net/) e [Microsoft Graph Explorer](https://developer.microsoft.com/en-us/graph/graph-explorer), respetivamente.

Atualmente, não carga de trabalho do Office 365 consome estes atributos.

Configurar os atributos adicionais que pretende sincronizar no caminho de definições personalizadas no Assistente de instalação.

![Assistente de extensão de esquema](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)  

A instalação mostra os seguintes atributos, são candidatos válidos:

* Tipos de objeto de utilizador e grupo
* Atributos de valor único: String, booleano, número inteiro, binário
* Atributos com múltiplos valores: cadeia, binário


>[!NOTE]
> Azure AD Connect suporta sincronizar atributos do Active Directory com múltiplos valores para o Azure AD como extensões de diretórios com múltiplos valores. Mas não existem funcionalidades no Azure AD atualmente suportam a utilização de extensões de diretórios com múltiplos valores.

A lista de atributos é lido a partir da cache do esquema que é criada durante a instalação do Azure AD Connect. Se tiver expandido o esquema do Active Directory com atributos adicionais, tem de [atualizar o esquema](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) antes destes novos atributos estão visíveis.

Um objeto no Azure AD pode ter até 100 atributos de extensões de diretórios. O comprimento máximo é 250 caracteres. Se um valor de atributo é maior, o motor de sincronização trunca-lo.

Durante a instalação do Azure AD Connect, uma aplicação está registada em que estes atributos estão disponíveis. Pode ver esta aplicação no portal do Azure.

![Aplicação de extensões de esquema](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3new.png)

Os atributos têm o prefixo com a extensão \_{AppClientId}\_. AppClientId tem o mesmo valor para todos os atributos no inquilino do Azure AD.

Estes atributos estão agora disponíveis através da Azure AD Graph API. Pode consultá-los utilizando [Explorador do Azure AD Graph](https://graphexplorer.azurewebsites.net/).

![Explorador do Azure AD Graph](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

Ou pode consultar os atributos através do Microsoft Graph API, utilizando [Microsoft Graph Explorer](https://developer.microsoft.com/en-us/graph/graph-explorer#).

>[!NOTE]
> Terá de voltar a pedir os atributos a ser devolvido. Selecionar explicitamente os atributos como esta: https://graph.microsoft.com/beta/users/abbie.spencer@fabrikamonline.com? $select = extension_9d98ed114c4840d298fad781915f27e4_employeeID, extension_9d98ed114c4840d298fad781915f27e4_division. 
>
> Para obter mais informações, consulte [Microsoft Graph: utilizar os parâmetros de consulta](https://developer.microsoft.com/en-us/graph/docs/concepts/query_parameters#select-parameter).

## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre o [sincronização do Azure AD Connect](active-directory-aadconnectsync-whatis.md) configuração.

Saiba mais sobre como [Integrar as identidades no local ao Azure Active Directory](active-directory-aadconnect.md).
