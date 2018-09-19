---
title: O Azure AD Connect Health - dados de estado de funcionamento do serviço não estão à altura do alerta de data | Documentos da Microsoft
description: Este documento descreve a causa do alerta de "dados de estado de funcionamento do serviço não estão atualizados" e a resolução de problemas.
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
ms.openlocfilehash: 430ea5f0a6f737d7632a4352c24d893368b80558
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46315099"
---
# <a name="health-service-data-is-not-up-to-date-alert"></a>Dados de estado de funcionamento do serviço não são atualizada alerta

## <a name="overview"></a>Descrição geral
<li>O Azure AD Connect Health gera um alerta de dados atualizados quando ele não receberá todos os pontos de dados do servidor de duas horas. É o título do alerta **dados de estado de funcionamento do serviço não estão atualizados**. </li>
<li>O **aviso** alerta de estado é acionado se Connect Health não receber os elementos de dados parciais enviados do servidor de duas horas. Alerta de estado de aviso não aciona notificações por e-mail para o administrador de inquilinos. </li>
<li>O **erro** alerta de estado é acionado se Connect Health não receber quaisquer elementos de dados enviados do servidor de duas horas. Acionadores de alerta de estado de erro notificações por email para o administrador de inquilinos. </li>

>[!IMPORTANT] 
> Este alerta segue Connect Health [política de retenção de dados](reference-connect-health-user-privacy.md#data-retention-policy)

## <a name="troubleshooting-steps"></a>Passos de resolução de problemas 
* Certifique-se entrar e cumprir os [secção de requisitos](how-to-connect-health-agent-install.md#requirements).
* Uso [ferramenta de teste de conectividade](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) para detetar problemas de conectividade.


## <a name="next-steps"></a>Passos Seguintes
* [FAQ do Azure AD Connect Health](reference-connect-health-faq.md)
