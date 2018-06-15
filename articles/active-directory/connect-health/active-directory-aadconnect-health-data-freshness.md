---
title: O Azure AD Connect Health - dados de estado de funcionamento do serviço não estão até alerta data | Microsoft Docs
description: Este documento descreve a causa do alerta de "dados de estado de funcionamento do serviço não estão atualizados" e como resolução de problemas.
services: active-directory
documentationcenter: ''
author: zhiweiw
manager: maheshu
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: zhiweiw
ms.openlocfilehash: 45e88320a64770239c8f322212e12ca7826cd0da
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
ms.locfileid: "29715016"
---
# <a name="health-service-data-is-not-up-to-date-alert"></a>Dados de estado de funcionamento do serviço não estão atualizada alerta

## <a name="overview"></a>Descrição geral
<li>O Azure AD Connect Health gera um alerta de raiz de dados quando este não recebe todos os pontos de dados do servidor de duas horas. O título do alerta é **dados do serviço de estado de funcionamento não estão atualizados**. </li>
<li>O **aviso** alerta de estado é acionado se Connect Health não recebe os elementos de dados parcial enviados do servidor de duas horas. Alerta de estado de aviso não acionar notificações por e-mail para o administrador inquilino. </li>
<li>O **erro** alerta de estado é acionado se Connect Health não receber quaisquer elementos de dados enviados do servidor de duas horas. Acionadores de alerta de estado de erro para que o administrador inquilino. notificações de e-mail </li>

>[!IMPORTANT] 
> Este alerta segue Connect Health [política de retenção de dados](active-directory-aadconnect-health-gdpr.md#data-retention-policy)

## <a name="troubleshooting-steps"></a>Passos de resolução de problemas 
* Certifique-se a abordar e cumprir as [secção requisitos](active-directory-aadconnect-health-agent-install.md#requirements).
* Utilize [ferramenta de teste de conectividade](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) para detetar problemas de conectividade.


## <a name="next-steps"></a>Passos Seguintes
* [FAQ do Azure AD Connect Health](active-directory-aadconnect-health-faq.md)
