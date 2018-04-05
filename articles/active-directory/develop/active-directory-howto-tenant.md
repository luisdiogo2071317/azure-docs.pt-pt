---
title: Como obter um inquilino do Azure AD | Microsoft Docs
description: Como obter um inquilino do Azure Active Directory para registar e criar aplicações.
services: active-directory
documentationcenter: ''
author: mtillman
manager: mtillman
editor: ''
ms.assetid: 1f4b24eb-ab4d-4baa-a717-2a0e5b8d27cd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/23/2018
ms.author: mtillman
ms.custom: aaddev
ms.openlocfilehash: ab7db49fa07f260de6ebbe4b2cee943b64cab7fe
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2018
---
# <a name="how-to-get-an-azure-active-directory-tenant"></a>Como obter um inquilino do Azure Active Directory
No Azure Active Directory (Azure AD), uma organização é representada por um [inquilino](https://msdn.microsoft.com/library/azure/jj573650.aspx#Anchor_0).  Quando cria uma relação com a Microsoft, tal como inscrever-se num serviço cloud da Microsoft como o Azure, o Microsoft Intune ou o Office 365, uma organização recebe e fica proprietária de uma instância dedicada do serviço Azure AD.  Cada inquilino do Azure AD é distinto e separado dos outros inquilinos do Azure AD.  

Um inquilino aloja os utilizadores duma empresa e respetivas informações: palavras-passe, dados de perfil de utilizador, permissões, etc.  Também contém grupos, aplicações e outras informações relativas a uma organização e à sua segurança.

Para permitir que os utilizadores do Azure AD iniciem sessão na aplicação, tem de registá-la no seu próprio inquilino.  Criar um inquilino do Azure AD e publicar uma aplicação no mesmo é **absolutamente gratuito** (embora possa optar por pagar por funcionalidades premium no seu inquilino).  Na verdade, muitos programadores criam vários inquilinos e aplicações com o objetivo de experimentar, desenvolver e testar.

## <a name="use-an-existing-azure-ad-tenant"></a>Utilizar um inquilino do Azure AD existente

Muitos programadores já têm inquilinos através de serviços ou subscrições que estão associadas a inquilinos do Azure AD (por exemplo: subscrições do Office 365 ou do Azure).  Para verificar se já tem um inquilino, inicie sessão no [portal do Azure](https://portal.azure.com) com a conta que pretende utilizar para gerir a sua aplicação e verifique o canto superior direito, onde são apresentadas as informações da sua conta.  Se tiver um inquilino, irá ter automaticamente sessão iniciada no mesmo e verá o nome do inquilino diretamente sob o nome da sua conta.  Se a sua conta estiver associada a vários inquilinos, pode clicar no nome da sua conta para abrir um menu onde pode alternar entre inquilinos.

Se não tiver um inquilino existente associado à sua conta, irá ver um GUID sob o nome da sua conta e não poderá executar ações como registar aplicações até [criar um novo inquilino](#create-a-new-azure-ad-tenant).

## <a name="create-a-new-azure-ad-tenant"></a>Criar um novo inquilino do Azure AD

Se ainda não tiver um inquilino do Azure AD ou quiser criar um novo, pode fazê-lo com a [experiência de criação de diretórios](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory) no [portal do Azure](https://portal.azure.com).  O processo demorará cerca de um minuto e, no fim, ser-lhe-á pedido que navegue até ao inquilino recém-criado.