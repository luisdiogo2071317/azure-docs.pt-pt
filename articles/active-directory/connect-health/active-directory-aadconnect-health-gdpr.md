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
ms.openlocfilehash: 5fedbac439636b56da217e7babd30820bce7b342
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "33931763"
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

### <a name="disable-data-collection-and-monitoring-for-an-instance-of-a-monitored-service"></a>Desativar a recolha de dados e a monitorização para uma instância de um serviço monitorizado
Consulte [como remover uma instância de serviço do Azure AD Connect Health](active-directory-aadconnect-health-operations.md#delete-a-service-instance-from-azure-ad-connect-health-service).

### <a name="disable-data-collection-and-monitoring-for-a-monitored-server"></a>Desativar a recolha de dados e a monitorização para um servidor monitorizado
Consulte [como remover um servidor do Azure AD Connect Health](active-directory-aadconnect-health-operations.md#delete-a-server-from-the-azure-ad-connect-health-service).

### <a name="disable-data-collection-and-monitoring-for-all-monitored-services-in-azure-ad-connect-health"></a>Desativar a recolha de dados e a monitorização de todos os serviços monitorizados no Azure AD Connect Health
O Azure AD Connect Health fornece também a opção para parar a recolha de dados de **todos os** registado serviços no inquilino. Recomendamos cuidada consideração e a confirmação completa de todos os administradores globais antes de efetuar a ação. Depois de iniciar o processo, serviço de integridade ligar deixará de receber, processamento e quaisquer dados de todos os seus serviços de relatórios. Os dados existentes no serviço de estado de funcionamento ligar ficarão retidos por mais do que 30 dias.
Se pretender parar a recolha de dados do servidor específico, siga os passos na eliminação de servidores específicos. Para parar a recolha de dados tenant-wise, siga os seguintes passos para parar a recolha de dados e eliminar todos os serviços do inquilino.

1.  Clique em **definições gerais** em configuração, no painel principal. 
2.  Clique em **parar a recolha de dados** botão no topo do painel. As outras opções de definições de configuração do inquilino serão desativadas quando o processo é iniciado.  
 
 ![Parar a recolha de dados](./media/active-directory-aadconnect-health-gdpr/gdpr4.png)
  
3.  Certifique-se a lista de serviços de integrado que são afetadas por parar as coleções de dados. 
4.  Introduza o nome de inquilino exato para ativar o **eliminar** botão de ação
5.  Clique em **eliminar** para acionar a eliminação de todos os serviços. Ligar estado de funcionamento deixará de receber, processamento, quaisquer dados enviados do seu serviços integrado de relatórios. Todo o processo de pode demorar até 24 horas. Repare que este passo não é reversível. 
6.  Após a conclusão do processo, não visualizará quaisquer serviços registados no Connect Health mais. 

 ![Após a interrupção da recolha de dados](./media/active-directory-aadconnect-health-gdpr/gdpr5.png)

## <a name="re-enable-data-collection-and-monitoring-in-azure-ad-connect-health"></a>Volte a ativar a recolha de dados e monitorização no Azure AD Connect Health
Voltar a ativar a monitorização no Azure AD Connect Health para um serviço monitorizado anteriormente eliminado, tem de desinstalar e [reinstale o agente de estado de funcionamento](active-directory-aadconnect-health-agent-install.md) em todos os servidores.

### <a name="re-enable-data-collection-and-monitoring-for-all-monitored-services"></a>Volte a ativar a recolha de dados e monitorização para todos os serviços monitorizados

É possível retomar a recolha de dados tenant-Wise no Azure AD Connect Health. Recomendamos cuidada consideração e a confirmação completa de todos os administradores globais antes de efetuar a ação.

>[!IMPORTANT]
> Os seguintes passos ficará disponíveis após 24 horas de desativar a ação.
> Após a ativação da recolha de dados, os conhecimentos apresentado e monitorização de dados no Connect Health não apresentará quaisquer dados legados recolhidos antes. 

1.  Clique em **definições gerais** em configuração, no painel principal. 
2.  Clique em **ativar recolha de dados** botão no topo do painel. 
 
 ![Ativar a recolha de dados](./media/active-directory-aadconnect-health-gdpr/gdpr6.png)
 
3.  Introduza o nome de inquilino exato para ativar o **ativar** botão.
4.  Clique em **ativar** botão para conceder permissão de recolha de dados no serviço de estado de funcionamento ligar. As alterações serão aplicadas em breve. 
5.  Siga o [processo de instalação](active-directory-aadconnect-health-agent-install.md) reinstalar o agente nos servidores para ser monitorizada e de serviços será apresentado no portal.  


## <a name="next-steps"></a>Passos Seguintes
* [Rever a política de Microsoft Privacy no Centro de confiança](https://www.microsoft.com/trustcenter)
* [Do Azure AD Connect e privacidade do utilizador](../../active-directory/connect/active-directory-aadconnect-gdpr.md)

