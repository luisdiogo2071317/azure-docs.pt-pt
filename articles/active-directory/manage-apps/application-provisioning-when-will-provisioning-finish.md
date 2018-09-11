---
title: Aprovisionamento de utilizadores a uma aplicação da galeria do Azure AD está a demorar horas ou mais | Documentos da Microsoft
description: Como descobrir por que o aprovisionamento da sua aplicação poderá estar a demorar mais tempo do que o esperado
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 2b46fecc44803130e4f79667d0e083314c4419c2
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44357797"
---
# <a name="user-provisioning-to-an-azure-ad-gallery-application-is-taking-hours-or-more"></a>Aprovisionamento de utilizadores a uma aplicação da galeria do Azure AD está a demorar horas ou mais

Ao primeiro ativar aprovisionamento automático para um aplicativo, a sincronização inicial pode demorar entre 20 minutos a várias horas, dependendo do tamanho do diretório do Azure AD e o número de utilizadores no âmbito de aprovisionamento. 

Sincronizações subsequentes após a sincronização inicial estar mais rápido, como o serviço de aprovisionamento armazena as marcas d'água que representam o estado dos dois sistemas após a sincronização inicial, melhorando o desempenho de sincronizações subsequentes.

## <a name="how-to-improve-provisioning-performance"></a>Como melhorar o desempenho de aprovisionamento

Se a sincronização inicial está a demorar mais do que algumas horas, há uma coisa que pode fazer para melhorar o desempenho:

-   **Filtros de âmbito do utilizador.** Filtros de âmbito permitem-lhe ajustar os dados que o serviço de aprovisionamento extrai do Azure AD ao filtrar os utilizadores com base nos valores de atributo específico. Para obter mais informações sobre filtros de âmbito, veja [aprovisionamento de aplicações baseadas em atributos com filtros de âmbito](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).

## <a name="next-steps"></a>Passos Seguintes
[Automate User Provisioning and Deprovisioning to SaaS Applications with Azure Active Directory](user-provisioning.md) (Automatizar o aprovisionamento e o desaprovisionamento de utilizadores em Aplicações SaaS com o Azure Active Directory)

