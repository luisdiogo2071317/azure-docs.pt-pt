---
title: Deteção de ameaças - base de dados SQL do Azure | Documentos da Microsoft
description: Deteção de ameaças Deteta atividades anómalas da base de dados, indicando potenciais ameaças de segurança para a base de dados.
services: sql-database
author: rmatchoro
manager: craigg
ms.service: sql-database
ms.subservice: advanced-threat-protection
ms.custom: security
ms.topic: conceptual
ms.date: 05/17/2018
ms.author: ronmat
ms.reviewer: carlrab
ms.openlocfilehash: c82692525f06fda93f94a8d856eb65254e5fd211
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "40003948"
---
# <a name="azure-sql-database-threat-detection"></a>Deteção de ameaças da base de dados SQL do Azure

A deteção de ameaças da base de dados SQL do Azure Deteta atividades anómalas que indiquem tentativas invulgares e potencialmente prejudiciais de acesso ou exploração de bases de dados.

Deteção de ameaças é parte da [proteção contra ameaças avançadas do SQL](sql-advanced-threat-protection.md) oferta (ATP), o que é um pacote unificado para funções de segurança avançadas do SQL. Deteção de ameaças pode ser acessada e gerenciada através do portal SQL ATP central.

## <a name="what-is-threat-detection"></a>O que é a deteção de ameaças?

Deteção de ameaças do SQL oferece uma nova camada de segurança, o que permite aos clientes detetar e responder a potenciais ameaças à medida que ocorrem ao fornecer alertas de segurança relativamente a atividades anómalas. Os utilizadores recebem um alerta após a atividades suspeitas da base de dados, potenciais vulnerabilidades, e injeção de SQL, ataques, bem como de acesso de base de dados anómalas e padrões de consulta. Deteção de ameaças SQL integra alertas no [Centro de segurança do Azure](https://azure.microsoft.com/services/security-center/), que inclui detalhes da ação de atividade e recomendamos suspeita sobre como investigar e mitigar a ameaça. Deteção de ameaças SQL simplifica a resolução de potenciais ameaças à base de dados sem a necessidade de ser um especialista em segurança ou gerir sistemas de monitorização de segurança avançada. 

Para uma experiência de investigação completo, é recomendado que ative [a auditoria de base de dados SQL](sql-database-auditing.md), que escreve o registo de eventos de base de dados para uma auditoria na sua conta de armazenamento do Azure.  

## <a name="set-up-threat-detection-for-your-database-in-the-azure-portal"></a>Configurar a deteção de ameaças da base de dados no portal do Azure
1. Iniciar o portal do Azure no [ https://portal.azure.com ](https://portal.azure.com).
2. Navegue para a página de configuração do servidor SQL Database do Azure que pretende proteger. Nas definições de segurança, selecione **proteção avançada contra ameaças**.
3. Sobre o **proteção avançada contra ameaças** página de configuração:

   - Ative a proteção avançada contra ameaças no servidor.
   - Na **as definições de deteção de ameaças**, na **enviar alertas para** texto caixa, forneça a lista de e-mails para receber alertas de segurança após a deteção de atividades anómalas da base de dados.
  
   ![Configurar a deteção de ameaças](./media/sql-database-threat-detection/set_up_threat_detection.png)

## <a name="set-up-threat-detection-using-powershell"></a>Configurar a deteção de ameaças através do PowerShell

Para obter um exemplo de script, consulte [configurar a deteção de ameaças e auditoria com o PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a>Explore a atividades anómalas da base de dados após a deteção de um evento suspeita

Receber uma notificação por e-mail após a deteção de atividades anómalas da base de dados. O e-mail fornece informações sobre o evento de segurança suspeito, incluindo a natureza das atividades anómalas, o nome de base de dados, nome do servidor, o nome da aplicação e a hora do evento. Além disso, o e-mail fornece informações sobre as causas possíveis e as ações recomendadas para investigar e mitigar a potencial ameaça à base de dados.

![Relatório de atividade anómala](./media/sql-database-threat-detection/anomalous_activity_report.png)
     
1. Clique nas **ver alertas recentes do SQL** ligação no e-mail para iniciar o portal do Azure e mostrar a página de alertas do Centro de segurança do Azure, que fornece uma visão geral do Active Directory ameaças detetadas na base de dados SQL.

   ![Ameaças de atividade](./media/sql-database-threat-detection/active_threats.png)

2. Clique num alerta específico para obter detalhes adicionais e ações para esta ameaça a investigar e remediar ameaças futuras.

   Por exemplo, injeção de SQL é uma dos problemas mais comuns de segurança de aplicação Web na Internet que é utilizado para atacar aplicações condicionadas por dados. Os atacantes tiram partido das vulnerabilidades das aplicações para injetar instruções SQL maliciosas nos campos de entrada do aplicativo, violar ou modificar os dados na base de dados. Para os alertas de Injeção de SQL, os detalhes do alerta incluem a instrução SQL vulnerável, que foi explorada.

   ![Alerta específico](./media/sql-database-threat-detection/specific_alert.png)

## <a name="explore-threat-detection-alerts-for-your-database-in-the-azure-portal"></a>Explore os alertas de deteção de ameaças da base de dados no portal do Azure

Deteção de ameaças da base de dados SQL integra seus alertas no [Centro de segurança do Azure](https://azure.microsoft.com/services/security-center/). Um mosaicos dinâmicos de deteção de ameaças SQL na base de dados e os painéis da ATP do SQL no portal do Azure monitoriza o estado de ameaças ativas.

Clique em **alerta de deteção de ameaças** para iniciar o Centro de segurança do Azure a alertas de página e obter uma visão geral do Active Directory SQL ameaças detetadas na base de dados.

   ![Alerta de deteção de ameaças](./media/sql-database-threat-detection/threat_detection_alert.png)
   
   ![Alert2 de deteção de ameaças](./media/sql-database-threat-detection/threat_detection_alert_atp.png)

## <a name="azure-sql-database-threat-detection-alerts"></a>Alertas de deteção de ameaças da base de dados SQL do Azure 
Deteção de ameaças da base de dados do Azure SQL Deteta atividades anómalas que indiquem tentativas invulgares e potencialmente prejudiciais de acesso ou exploração de bases de dados e ele pode acionar os seguintes alertas:
- **Vulnerabilidade a Injeção de SQL**: este alerta é acionado quando uma aplicação gera uma instrução SQL defeituosa na base de dados. Tal pode indicar uma possível vulnerabilidade a ataques de injeção de SQL. Existem dois motivos possíveis para a geração de uma instrução defeituosa:
   - Um defeito no código da aplicação que constitui a instrução SQL defeituosa
   - O código de aplicação ou os procedimentos armazenados não saneiam a entrada de utilizador ao criar a instrução SQL defeituosa, o que pode ser explorado para Injeção SQL
- **Potencial injeção SQL**: este alerta é acionado quando uma exploração ativa ocorre contra uma vulnerabilidade de aplicação identificada para a injeção SQL. Significa que o atacante está a tentar injetar instruções SQL maliciosas através dos procedimentos armazenados ou código de aplicação com vulnerabilidade.
- **Acesso de uma localização invulgar**: este alerta é acionado quando ocorre uma alteração no padrão de acesso ao servidor SQL, no qual alguém iniciou sessão no servidor SQL a partir de uma localização geográfica invulgar. Em alguns casos, o alerta deteta uma ação legítima (uma nova manutenção de programador ou aplicação). Noutros casos, o alerta deteta uma ação maliciosa (ex-funcionário, atacante externo).
- **Acesso de um centro de dados do Azure invulgar**: este alerta é acionado quando ocorre uma alteração no padrão de acesso ao servidor SQL, no qual alguém iniciou sessão no servidor SQL a partir de um centro de dados do Azure invulgar que foi visto neste servidor durante o período recente. Em alguns casos, o alerta deteta uma ação legítima (a sua nova aplicação no Azure, Power BI, Editor de Consultas SQL do Azure). Noutros casos, o alerta deteta uma ação maliciosa de um recurso/serviço do Azure (ex-funcionário, atacante externo).
- **Acesso de um principal invulgar**: este alerta é acionado quando ocorre uma alteração no padrão de acesso ao servidor SQL, no qual alguém iniciou sessão no servidor SQL com recurso a um principal (utilizador SQL) invulgar. Em alguns casos, o alerta deteta uma ação legítima (nova aplicação, manutenção de programador). Noutros casos, o alerta deteta uma ação maliciosa (ex-funcionário, atacante externo).
- **Acesso de uma localização potencialmente prejudicial**: este alerta é acionado quando uma aplicação potencialmente prejudicial é utilizada para aceder à base de dados. Em alguns casos, o alerta deteta testes de penetração em ação. Noutros casos, o alerta deteta um ataque através de ferramentas de ataque comuns.
- **Credenciais SQL de força bruta**: este alerta é acionado quando existe um número anormalmente elevado de inícios de sessão falhados com diferentes credenciais. Em alguns casos, o alerta deteta testes de penetração em ação. Noutros casos, o alerta deteta um ataque de força bruta.

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [proteção do SQL avançada contra ameaças](sql-advanced-threat-protection.md). 
* Saiba mais sobre [auditoria de base de dados SQL do Azure](sql-database-auditing.md)
* Saiba mais sobre [Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-intro)
* Para obter mais informações sobre os preços, consulte o [página de preços de base de dados SQL](https://azure.microsoft.com/pricing/details/sql-database/)  
