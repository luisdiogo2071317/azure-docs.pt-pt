---
title: Estado de funcionamento e privacidade do utilizador do Azure AD Connect | Microsoft Docs
description: Este documento descreve a privacidade do utilizador com o Azure AD Connect Health.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: billmath
ms.openlocfilehash: bf26e91308cfec0dc8ede20e683919b5764a4868
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="user-privacy-and-azure-ad-connect-health"></a>Privacidade do utilizador e o Azure AD Connect Health 

[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

>[!NOTE] 
>Este artigo lida com a privacidade do Azure AD Connect Health e utilizador.  Para obter informações sobre privacidade do Azure AD Connect e utilizador, consulte o artigo [aqui](../../active-directory/connect/active-directory-aadconnect-gdpr.md).

## <a name="user-privacy-classification"></a>Classificação de privacidade do utilizador
O Azure AD Connect Health se enquadrar o **dados processador** categoria de classificação de GDPR. Como um pipeline de processador de dados, o serviço fornece serviços de processamento de dados para parceiros de chaves e os consumidores de fim. O Azure AD Connect Health não gera dados de utilizador e não tem nenhum controlo independente ao longo do que os dados pessoais são recolhidos e como são utilizadas. Obtenção de dados, agregação, análise e relatórios do Azure AD Connect Health baseiam-se em dados no local existentes. 

## <a name="data-retention-policy"></a>Política de retenção de dados
O Azure AD Connect Health não gerar relatórios, efetuar análises ou fornecem informações para além de 30 dias. Por conseguinte, o Azure AD Connect Health não armazenar, processar ou manter quaisquer dados para além de 30 dias. Esta estrutura está em conformidade com as normas GDPR, normas de conformidade de privacidade do Microsoft e as políticas de retenção de dados do Azure AD. 

Servidores com o Active Directory **dados do serviço de estado de funcionamento não estão atualizados** **erro** alertas para mais de 30 dias consecutivos sugerimos que não existem dados atingiu Connect Health durante esse intervalo de tempo. Estes servidores serão desativados e não é apresentados no portal de Connect Health. Para reativar os servidores, terá de desinstalar e [reinstale o agente de estado de funcionamento](active-directory-aadconnect-health-agent-install.md). Tenha em atenção que isto não se aplicam a **avisos** com o mesmo tipo de alerta. Avisos indicam que estão em falta no servidor que se for alertado para dados parciais. 
 
## <a name="disable-data-collection-and-monitoring-in-azure-ad-connect-health"></a>Desativar a recolha de dados e monitorização no Azure AD Connect Health
O Azure AD Connect Health permite-lhe parar a recolha de dados para cada servidor monitorizado individuais ou para uma instância de um serviço monitorizado. Por exemplo, pode parar a recolha de dados para servidores individuais do AD FS (Serviços de Federação do Active Directory) que são monitorizadas com o Azure AD Connect Health. Também pode parar a recolha de dados para a instância completa do AD FS que está a ser monitorizada através do Azure AD Connect Health. Quando optar por fazê-lo, os servidores correspondentes são eliminados do portal do Azure AD Connect Health, depois de parar a recolha de dados. 

>[!IMPORTANT]
> Precisa de privilégios de Administrador Global do AD do Azure ou a função de contribuinte no RBAC para servidores monitorizada de eliminação do Azure AD Connect Health.
>
> A remover um servidor ou a instância de serviço do Azure AD Connect Health não é uma ação reversível. 

### <a name="what-to-expect"></a>O que esperar?
Se parar a recolha de dados e monitorização para o servidor monitorizado individual ou uma instância de um serviço monitorizado, tenha em atenção o seguinte:

- Quando elimina uma instância de um serviço monitorizado, a instância é removida na lista do serviço de monitorização do Azure AD Connect Health no portal. 
- Quando elimina um servidor monitorizado ou uma instância de um serviço monitorizado, o agente de estado de funcionamento não está desinstalado ou removido a partir dos seus servidores. O agente de estado de funcionamento está configurado para não enviar dados para o Azure AD Connect Health. Terá de desinstalar manualmente o agente de estado de funcionamento nos servidores monitorizados anteriormente.
- Se não o desinstalou o agente de estado de funcionamento antes de executar este passo, poderá ver eventos de erro do servidor (es) relacionados com o agente de estado de funcionamento.
- Todos os dados que pertencem à instância do serviço monitorizado é eliminados de acordo com a política de retenção de dados do Microsoft Azure.

### <a name="disable-data-collection-and-monitoring-for-a-monitored-server"></a>Desativar a recolha de dados e a monitorização para um servidor monitorizado
Consulte [como remover um servidor do Azure AD Connect Health](active-directory-aadconnect-health-operations.md#delete-a-server-from-the-azure-ad-connect-health-service).

### <a name="disable-data-collection-and-monitoring-for-an-instance-of-a-monitored-service"></a>Desativar a recolha de dados e a monitorização para uma instância de um serviço monitorizado
Consulte [como remover uma instância de serviço do Azure AD Connect Health](active-directory-aadconnect-health-operations.md#delete-a-service-instance-from-azure-ad-connect-health-service).


## <a name="re-enable-data-collection-and-monitoring-in-azure-ad-connect-health"></a>Volte a ativar a recolha de dados e monitorização no Azure AD Connect Health
Voltar a ativar a monitorização no Azure AD Connect Health para um serviço monitorizado anteriormente eliminado, tem de desinstalar e [reinstale o agente de estado de funcionamento](active-directory-aadconnect-health-agent-install.md) em todos os servidores.


## <a name="next-steps"></a>Passos Seguintes
* [Rever a política de Microsoft Privacy no Centro de confiança](https://www.microsoft.com/trustcenter)
* [Do Azure AD Connect e privacidade do utilizador](../../active-directory/connect/active-directory-aadconnect-gdpr.md)

