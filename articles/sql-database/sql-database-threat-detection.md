---
title: Deteção - base de dados SQL do Azure de ameaças | Microsoft Docs
description: A Deteção de Ameaças deteta atividades de base de dados anómalas, indicando potenciais ameaças de segurança para a base de dados.
services: sql-database
author: rmatchoro
manager: craigg
ms.service: sql-database
ms.custom: security
ms.topic: article
ms.date: 05/17/2018
ms.author: ronmat
ms.reviewer: carlrab
ms.openlocfilehash: db10bbdd39920b05d9fd8c3907f22c3ee5d08b02
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="azure-sql-database-threat-detection"></a>Deteção de ameaças de base de dados SQL do Azure

Deteção de ameaças de base de dados SQL do Azure Deteta atividades anómalas, indicando invulgar e potencialmente prejudicial tenta aceder ou explorar menores, bases de dados.

A deteção de ameaças faz parte o [proteção contra ameaças avançadas do SQL Server](sql-advanced-threat-protection.md) oferta (ATP), que é um pacote unificado para capacidades avançadas de segurança do SQL Server. A deteção de ameaças pode ser acedida e gerida através do portal do central ATP do SQL Server.

## <a name="what-is-threat-detection"></a>O que é a deteção de ameaças?

A deteção de ameaças do SQL Server fornece uma nova camada de segurança, o que permite que os clientes detetar e reagir a potenciais ameaças à medida que ocorrem, fornecendo alertas de segurança em atividades anómalas. Os utilizadores recebem um alerta após a atividades suspeitas da base de dados, potenciais vulnerabilidades, e a injeção de SQL de ataques, bem como de acesso de base de dados anómalas e padrões de consulta. A deteção de ameaças do SQL Server integra-se de alertas com [Centro de segurança do Azure](https://azure.microsoft.com/services/security-center/), que inclui detalhes de ação de atividade e recomendamos suspeita sobre a investigar e mitigar a ameaça. A deteção de ameaças do SQL Server torna simples para resolver potenciais ameaças à base de dados sem a necessidade de ser um especialista de segurança ou faça a gestão de sistemas de monitorização de segurança avançada. 

Para uma experiência de investigação completa, é recomendado ativar [auditoria de base de dados SQL](sql-database-auditing.md), que escreve eventos de base de dados para uma auditoria iniciar sessão na sua conta do storage do Azure.  

## <a name="set-up-threat-detection-for-your-database-in-the-azure-portal"></a>Configurar a deteção de ameaças para a base de dados no portal do Azure
1. Inicie o portal do Azure em [ https://portal.azure.com ](https://portal.azure.com).
2. Navegue para a página de configuração do servidor SQL Database do Azure que pretende proteger. Nas definições de segurança, selecione **Advanced Threat Protection**.
3. No **Advanced Threat Protection** página de configuração:

   - Ative a proteção avançada contra ameaças no servidor.
   - No **as definições de deteção de ameaças**, além de **enviar alertas para** texto caixa, forneça a lista de mensagens de correio eletrónico para receber alertas de segurança após a deteção de atividades de base de dados anómalas.
  
   ![Configurar a deteção de ameaças](./media/sql-database-threat-detection/set_up_threat_detection.png)

## <a name="set-up-threat-detection-using-powershell"></a>Configurar a deteção de ameaças através do PowerShell

Para obter um exemplo de script, consulte [configurar a auditoria e deteção de ameaças através do PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a>Explore as atividades de base de dados anómalas mediante a deteção de um evento suspeita

Receberá uma notificação por e-mail mediante a deteção de atividades de base de dados anómalas. O e-mail fornece informações sobre o evento de segurança suspeita, incluindo a natureza as atividades anómalas, nome de base de dados, nome do servidor, nome da aplicação e a hora do evento. Além disso, o e-mail fornece informações sobre as causas possíveis e as ações para investigar e mitigar a potencial ameaça à base de dados recomendadas.

![Relatório de atividade anómala](./media/sql-database-threat-detection/anomalous_activity_report.png)
     
1. Clique em de **vê os alertas recentes do SQL Server** ligação no e-mail para iniciar o portal do Azure e mostrar a página de alertas do Centro de segurança do Azure, que fornece uma descrição geral do Active Directory ameaças detetadas na base de dados do SQL Server.

   ![Activty ameaças](./media/sql-database-threat-detection/active_threats.png)

2. Clique num alerta específico para obter detalhes adicionais e ações para investigar esta ameaça e a remediação ameaças futuras.

   Por exemplo, a injeção de SQL é uma dos problemas mais comuns de segurança de aplicação Web na Internet que é utilizado para atacar aplicações condicionada por dados. Os atacantes tirar partido de vulnerabilidades de aplicação para injetar maliciosas instruções SQL para campos de entrada de aplicação, ser, ou modificar dados na base de dados. Existência de alertas de Injeção de SQL, detalhes do alerta incluem a instrução SQL vulnerável que foi forem explorada.

   ![Alerta específico](./media/sql-database-threat-detection/specific_alert.png)

## <a name="explore-threat-detection-alerts-for-your-database-in-the-azure-portal"></a>Explorar a alertas de deteção de ameaças para a base de dados no portal do Azure

Deteção de ameaças de base de dados do SQL Server integra-se os alertas com [Centro de segurança do Azure](https://azure.microsoft.com/services/security-center/). Um em direto mosaicos de deteção de ameaças de SQL Server dentro da base de dados e painéis ATP do SQL Server no portal do Azure controla o estado das ameaças de Active Directory.

Clique em **alerta de deteção de ameaças** para iniciar o Centro de segurança do Azure alertas página e obter uma descrição geral do Active Directory ameaças SQL detetado na base de dados.

   ![Alerta de deteção de ameaças](./media/sql-database-threat-detection/threat_detection_alert.png)
   
   ![Alert2 de deteção de ameaças](./media/sql-database-threat-detection/threat_detection_alert_atp.png)

## <a name="azure-sql-database-threat-detection-alerts"></a>Alertas de deteção de ameaças de base de dados SQL do Azure 
Para a SQL Database do Azure, a deteção de ameaças Deteta atividades anómalas, indicando invulgar e potencialmente prejudicial tenta aceder ou explorar menores, bases de dados e pode acionar os seguintes alertas:
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

* Saiba mais sobre [SQL Advanced Threat Protection](sql-advanced-threat-protection.md). 
* Saiba mais sobre [auditoria de base de dados SQL do Azure](sql-database-auditing.md)
* Saiba mais sobre [Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-intro)
* Para obter mais informações sobre preços, consulte o [página de preços de base de dados do SQL Server](https://azure.microsoft.com/pricing/details/sql-database/)  
