---
title: 'Sincronização do Azure AD Connect: como gerir a conta de serviço do Azure AD | Documentos da Microsoft'
description: Este tópico documenta como restaurar a conta de serviço do Azure AD.
services: active-directory
keywords: Error":"invalid_grant","error_description":"aadsts70002, AADSTS50054, como repor a palavra-passe para a conta de serviço do conector de sincronização do Azure AD Connect
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 6077043a-27f1-4304-a44b-81dc46620f24
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 93dcbe5ceac87d84db9638de6d5fe120de8c2d36
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46313650"
---
# <a name="azure-ad-connect-sync-how-to-manage-the-azure-ad-service-account"></a>Sincronização do Azure AD Connect: como gerir a conta de serviço do Azure AD
A conta de serviço utilizada pelo conector do AD do Azure deve para ser o serviço gratuitamente. Se precisar de repor as respetivas credenciais, então este tópico é para si. Por exemplo, se um Administrador Global tiver por engano repor a palavra-passe da conta de serviço com o PowerShell.

## <a name="reset-the-credentials"></a>Repor as credenciais
Se a conta de serviço definida no conector do Azure AD não consegue contactar o Azure AD devido a problemas de autenticação, a palavra-passe pode ser reposta.

1. Inicie sessão para o servidor de sincronização do Azure AD Connect e inicie o PowerShell.
2. Execute `Add-ADSyncAADServiceAccount`.  
   ![Addadsyncaadserviceaccount de cmdlet do PowerShell](./media/how-to-connect-azureadaccount/addadsyncaadserviceaccount.png)
3. Fornece credenciais de Administrador Global do Azure AD.

Este cmdlet repõe a palavra-passe para a conta de serviço e atualizá-la no Azure AD e no motor de sincronização.

## <a name="known-issues-these-steps-can-solve"></a>Problemas conhecidos que podem resolver estes passos
Esta secção é uma lista de erros relatados por clientes que foram corrigidos por um credenciais repor na conta de serviço do Azure AD.

- - -
Evento 6900  
O servidor encontrou um erro inesperado ao processar uma notificação de alteração de palavra-passe:  
Error":"invalid_grant","error_description":"aadsts70002: Erro ao validar as credenciais. AADSTS50054: Palavra-passe antiga é utilizado para autenticação.

- - -
Evento 659  
Erro ao obter a configuração de sincronização da política de palavra-passe. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
Error":"invalid_grant","error_description":"aadsts70002: Erro ao validar as credenciais. AADSTS50054: Palavra-passe antiga é utilizado para autenticação.

## <a name="next-steps"></a>Passos Seguintes
**Tópicos de descrição geral**

* [Sincronização do Azure AD Connect: entender e personalizar a sincronização](how-to-connect-sync-whatis.md)
* [Integrar as identidades no local ao Azure Active Directory](whatis-hybrid-identity.md)

