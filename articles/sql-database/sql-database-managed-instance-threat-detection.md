---
title: Deteção de ameaças - instância gerida de base de dados SQL do Azure | Documentos da Microsoft
description: Deteção de ameaças Deteta atividades anómalas da base de dados, indicando potenciais ameaças de segurança para a base de dados numa instância gerida.
services: sql-database
author: rmatchoro
manager: craigg
ms.service: sql-database
ms.custom: security, managed instance
ms.topic: conceptual
ms.date: 09/19/2018
ms.author: ronmat
ms.reviewer: vanto
ms.openlocfilehash: 28676d0e027b73281fcb9874669aa6447ec622ff
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46964263"
---
# <a name="azure-sql-database-managed-instance-threat-detection"></a>Deteção de ameaças da instância de gerida de base de dados SQL do Azure

Deteção de ameaças do SQL Deteta atividades anómalas que indiquem tentativas invulgares e potencialmente prejudiciais de acesso ou exploração de bases de dados numa instância de gerida de base de dados do Azure SQL.

## <a name="overview"></a>Descrição geral

Deteção de ameaças Deteta atividades anómalas da base de dados, indicando potenciais ameaças de segurança para a instância gerida. Deteção de ameaças está agora em pré-visualização para a instância gerida.

Deteção de ameaças oferece uma nova camada de segurança, o que permite aos clientes detetar e responder a potenciais ameaças à medida que ocorrem ao fornecer alertas de segurança relativamente a atividades anómalas da base de dados. Deteção de ameaças facilita lidar com potenciais ameaças à instância gerida, sem a necessidade de ser um especialista em segurança ou gerir sistemas de monitorização de segurança avançados. Para uma experiência de investigação completo, é recomendado para ativar a auditoria do Azure geridos instância, que escreve o registo de eventos de base de dados para uma auditoria na sua conta de armazenamento do Azure. 

Deteção de ameaças SQL integra alertas no [Centro de segurança do Azure](https://azure.microsoft.com/services/security-center/), e, em cada instância protegida geridos é cobrada de acordo com o mesmo preço do escalão Standard de centro de segurança do Azure, US $15/nó/mês, onde cada um protegido instância gerida é contabilizado como um nó.  

## <a name="set-up-threat-detection-for-your-managed-instance-in-the-azure-portal"></a>Configurar a deteção de ameaças da sua instância gerida no portal do Azure
1. Iniciar o portal do Azure no [ https://portal.azure.com ](https://portal.azure.com).
2. Navegue para a página de configuração de instância gerida que pretende proteger. Na **configurações** página, selecione **deteção de ameaças**. 
3. Na página de configuração de deteção de ameaças 
   - Ative **ON** deteção de ameaças.
   - Configurar o **lista de e-mails** para receber alertas de segurança após a deteção de atividades anómalas da base de dados.
   - Selecione o **conta de armazenamento do Azure** onde são guardados os registos de auditoria de ameaças anómalas. 
4.  Clique em **guardar** para guardar a política de deteção de ameaças novos ou atualizados.

   ![Deteção de ameaças](./media/sql-database-managed-instance-threat-detection/threat-detection.png)

## <a name="explore-anomalous-managed-instance-activities-upon-detection-of-a-suspicious-event"></a>Explore a atividades anómalas da instância gerida após a deteção de um evento suspeita

1. Receber uma notificação por e-mail após a deteção de atividades anómalas da base de dados. 

   O e-mail fornece informações sobre o evento de segurança suspeito, incluindo a natureza das atividades anómalas, o nome de base de dados, o nome do servidor e a hora do evento. Além disso, ele fornece informações sobre as causas possíveis e as ações recomendadas para investigar e mitigar a potencial ameaça à instância gerida.

   ![e-mail de deteção de ameaças](./media/sql-database-managed-instance-threat-detection/threat-detection-email.png)

2. Clique nas **ver alertas recentes do SQL** ligação no e-mail para iniciar o portal do Azure e mostrar a página de alertas do Centro de segurança do Azure, que fornece uma visão geral do Active Directory foram detetadas de ameaças SQL na base de dados a instância gerida.

   ![ameaças ativas](./media/sql-database-managed-instance-threat-detection/active-threats.png)

3. Clique num alerta específico para obter detalhes adicionais e ações para esta ameaça a investigar e remediar ameaças futuras.

   Por exemplo, injeção de SQL é uma dos problemas comuns de segurança de aplicação Web na Internet. Injeção de SQL é utilizada para atacar aplicações condicionadas por dados. Os atacantes tiram partido das vulnerabilidades das aplicações para injetar instruções SQL maliciosas nos campos de entrada do aplicativo, violar ou modificar os dados na base de dados. Para os alertas de Injeção de SQL, os detalhes do alerta incluem a instrução SQL vulnerável, que foi explorada.

   ![injeção de SQL](./media/sql-database-managed-instance-threat-detection/sql-injection.png)

## <a name="managed-instance-threat-detection-alerts"></a>Alertas de deteção de ameaças da instância geridas 

Deteção de ameaças da instância gerida Deteta atividades anómalas que indiquem tentativas invulgares e potencialmente prejudiciais de acesso ou exploração de bases de dados e ele pode acionar os seguintes alertas:
- **Vulnerabilidade a Injeção de SQL**: este alerta é acionado quando uma aplicação gera uma instrução SQL defeituosa na base de dados. Tal pode indicar uma possível vulnerabilidade a ataques de injeção de SQL. Existem dois motivos possíveis para a geração de uma instrução defeituosa:
 - Um defeito no código da aplicação que constitui a instrução SQL defeituosa
 - O código de aplicação ou os procedimentos armazenados não saneiam a entrada de utilizador ao criar a instrução SQL defeituosa, o que pode ser explorado para Injeção SQL
- **Potencial injeção SQL**: este alerta é acionado quando uma exploração ativa ocorre contra uma vulnerabilidade de aplicação identificada para a injeção SQL. Isso significa que o atacante está a tentar injetar instruções SQL maliciosas usando o código de aplicativo vulnerável ou procedimentos armazenados.
- **Acesso a partir de uma localização invulgar**: este alerta é acionado quando ocorre uma alteração no padrão de acesso para uma instância gerida, em que alguém iniciou sessão à instância gerida de uma localização geográfica invulgar. Em alguns casos, o alerta Deteta uma ação legítima (uma nova aplicação ou operação de manutenção do desenvolvedor). Em outros casos, o alerta Deteta uma ação maliciosa (ex-funcionário, atacante externo e assim por diante).
- **Acesso a partir do Centro de dados do Azure invulgar**: este alerta é acionado quando ocorre uma alteração no padrão de acesso para a instância gerida, em que alguém iniciou sessão à instância gerida a partir de um centro de dados do Azure que não foi visualizado, aceder a este gerida Instância durante o período recente. Em alguns casos, o alerta Deteta uma ação legítima (sua nova aplicação no Azure, o Power BI, o Editor de consultas de SQL do Azure e assim por diante). Noutros casos, o alerta deteta uma ação maliciosa de um recurso/serviço do Azure (ex-funcionário, atacante externo).
- **Acesso a partir de principal invulgar**: este alerta é acionado quando ocorre uma alteração no padrão de acesso ao servidor de instância gerida, em que alguém iniciou sessão para a instância gerida através de um principal invulgar (utilizador SQL). Em alguns casos, o alerta Deteta uma ação legítima (operação de manutenção do novo desenvolvedor de aplicativos). Noutros casos, o alerta deteta uma ação maliciosa (ex-funcionário, atacante externo).
- **Acesso de uma localização potencialmente prejudicial**: este alerta é acionado quando uma aplicação potencialmente prejudicial é utilizada para aceder à base de dados. Em alguns casos, o alerta deteta testes de penetração em ação. Noutros casos, o alerta deteta um ataque através de ferramentas de ataque comuns.
- **Credenciais SQL de força bruta**: este alerta é acionado quando existe um número anormalmente elevado de inícios de sessão falhados com diferentes credenciais. Em alguns casos, o alerta deteta testes de penetração em ação. Em outros casos, o alerta Deteta um ataque de força bruta.

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre a instância gerida, consulte [o que é uma instância gerida](sql-database-managed-instance.md)
- Saiba mais sobre [auditoria de instância gerida](https://go.microsoft.com/fwlink/?linkid=869430) 
- Saiba mais sobre [Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-intro)
