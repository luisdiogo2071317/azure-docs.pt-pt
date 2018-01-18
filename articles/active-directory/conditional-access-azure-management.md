---
title: "Gerir o acesso à gestão do Azure com o acesso condicional no Azure Active Directory"
description: "Saiba como utilizar o acesso condicional no Azure AD para gerir o acesso à gestão do Azure."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: bryanla
ms.assetid: 0adc8b11-884e-476c-8c43-84f9bf12a34b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/22/2017
ms.author: skwan
ms.openlocfilehash: 22d0e53c201853e2c316089479ffbd4d9e5d92be
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="manage-access-to-azure-management-with-conditional-access"></a>Gerir o acesso à gestão do Azure com o acesso condicional

Acesso condicional no Azure Active Directory (Azure AD) controla o acesso à nuvem aplicações com base em condições específicas que especificar. Para permitir o acesso, criar políticas de acesso condicional permitirem ou bloquear o acesso com base em se pretende ou não são cumpridos os requisitos na política. 

Normalmente, utilize o acesso condicional para controlar o acesso às suas aplicações em nuvem. Também pode configurar políticas para controlar o acesso à gestão do Azure com base em determinadas condições (por exemplo, o risco de início de sessão, localização, dispositivo ou) e impõem os requisitos, como a autenticação multifator.

Para criar uma política de gestão do Azure, selecione **Microsoft Azure Management** em **aplicações em nuvem** quando escolher a aplicação para o qual pretende aplicar a política.

![Acesso condicional para a gestão do Azure](./media/conditional-access-azure-mgmt.png)

A política, criar aplica-se a todos os pontos finais de gestão do Azure, incluindo o portal clássico do Azure, o portal do Azure, o fornecedor do Azure Resource Manager, clássico APIs de gestão de serviço e o Azure PowerShell.

> [!CAUTION]
> Certifique-se de que compreende como o acesso condicional funciona antes de configurar uma política para gerir o acesso à gestão do Azure. Certifique-se de que não crie condições que foi possível bloquear a sua própria acesso ao portal do.

Para obter mais informações sobre como configurar e utilizar o acesso condicional, consulte [acesso condicional no Azure Active Directory](active-directory-conditional-access-azure-portal.md).