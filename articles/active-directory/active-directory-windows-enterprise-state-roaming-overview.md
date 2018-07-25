---
title: Descrição geral do Roaming de estado empresarial | Documentos da Microsoft
description: Fornece informações sobre as definições de Roaming de estado empresarial em dispositivos Windows. Roaming de estado empresarial fornece aos usuários uma experiência unificada em todos os seus dispositivos Windows e reduz o tempo necessário para configurar um novo dispositivo.
services: active-directory
keywords: o que é o Roaming de estado empresarial, sincronização de enterprise, windows na cloud
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: curtand
ms.component: devices
ms.assetid: 83b3b58f-94c1-4ab0-be05-20e01f5ae3f0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: markvi
ms.openlocfilehash: c2c94f488da6399965c59f761ff9f0539cf4de2f
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228119"
---
# <a name="enterprise-state-roaming-overview"></a>Descrição geral do Roaming de Estado Empresarial
Com o Windows 10, [do Azure Active Directory (Azure AD)](fundamentals/active-directory-whatis.md) os utilizadores obtêm a capacidade de sincronizar com segurança os seus dados de definições de aplicação para a cloud e as definições do utilizador. Roaming de estado empresarial fornece aos usuários uma experiência unificada em todos os seus dispositivos Windows e reduz o tempo necessário para configurar um novo dispositivo. Roaming de estado empresarial opera semelhante ao padrão [sincronização de definições de consumidor](http://windows.microsoft.com/en-US/windows-8/sync-settings-pcs) que foi introduzido pela primeira vez no Windows 8. Além disso, Roaming de estado empresarial oferece:

* **Separação da empresa e dados de consumidor** – as organizações estão no controlo dos seus dados e não há mistura de dados da empresa numa conta do consumidor na cloud ou de consumidor os dados numa conta empresarial na cloud.
* **Segurança avançada** – automaticamente os dados são encriptados antes de deixarem o dispositivo do utilizador com o Windows 10 com o Azure Rights Management (Azure RMS) e dados permanecem encriptados em inatividade na cloud. Todos os conteúdos permanecem encriptados em inatividade na cloud, exceto para os espaços de nomes, como nomes de definições e nomes de aplicações do Windows.  
* **Melhor gestão e monitorização** – fornece visibilidade e controlo sobre que sincroniza as definições na sua organização e em que dispositivos por meio da integração de portal do Azure AD. 

Roaming de estado empresarial está disponível em várias regiões do Azure. Pode encontrar a lista atualizada de regiões disponíveis sobre o [serviços do Azure por regiões](https://azure.microsoft.com/regions/#services) página em Azure Active Directory.

| Artigo | Descrição |
| --- | --- |
| [Ativar estado da empresa em Roaming no Azure Active Directory](active-directory-windows-enterprise-state-roaming-enable.md) |Roaming de estado empresarial está disponível para qualquer organização com uma subscrição Premium do Azure Active Directory (Azure AD). Para obter mais detalhes sobre como obter uma subscrição do Azure AD, consulte a [produto do Azure AD](https://azure.microsoft.com/services/active-directory) página. |
| [FAQ de roaming de dados e definições](active-directory-windows-enterprise-state-roaming-faqs.md) |Este tópico responde a algumas perguntas que os administradores de TI podem ter sobre as definições e sincronização de dados de aplicações. |
| [Política de grupo e definições de MDM para sincronização de definições](active-directory-windows-enterprise-state-roaming-group-policy-settings.md) |Windows 10 fornece política de grupo e gestão de dispositivos móveis (MDM) de configurações de diretiva para limitar a sincronização de definições. |
| [Referência de configurações de roaming com o Windows 10](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md) |Segue-se uma lista completa de todas as definições que irá ser movidas e/ou uma cópia de segurança no Windows 10. |
| [Resolução de problemas](active-directory-windows-enterprise-state-roaming-troubleshooting.md) |Este tópico aborda alguns passos básicos para resolução de problemas e contém uma lista dos problemas conhecidos. |

