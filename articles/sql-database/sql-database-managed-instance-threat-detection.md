---
title: "Deteção - de ameaças base de dados SQL do Azure gerida instância | Microsoft Docs"
description: "A Deteção de Ameaças deteta atividades de base de dados anómalas, indicando potenciais ameaças de segurança para a base de dados."
services: sql-database
author: rmatchoro
manager: craigg
ms.service: sql-database
ms.custom: security, managed instance
ms.topic: article
ms.date: 03/07/2018
ms.author: ronmat
ms.reviewer: carlrab
ms.openlocfilehash: 2112a0a3997af478de6b8c80abcf7924a66302f0
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="azure-sql-database-managed-instance-threat-detection"></a>Base de dados SQL do Azure gerida a deteção de ameaças de instância

SQL Server a deteção de ameaças Deteta atividades anómalas, indicando invulgar e potencialmente prejudicial tenta aceder ao ou exploram bases de dados numa instância de geridos de base de dados no Azure SQL (pré-visualização).

## <a name="overview"></a>Descrição geral

A deteção de ameaças Deteta atividades de base de dados anómalas, indicando potenciais ameaças de segurança para a instância geridos. A deteção de ameaças está agora em pré-visualização para a instância geridos.

A deteção de ameaças fornece uma nova camada de segurança, o que permite que os clientes detetar e reagir a potenciais ameaças à medida que ocorrem, fornecendo alertas de segurança em atividades de base de dados anómalas. A deteção de ameaças torna simples a potenciais ameaças de endereço para a instância geridos sem a necessidade de um especialista de segurança ou gerir sistemas de monitorização de segurança avançada. Para uma experiência de investigação completa, é recomendado para ativar a auditoria do Azure gerida instância, que escreve eventos de base de dados para uma auditoria iniciar sessão na sua conta do storage do Azure. 

A deteção de ameaças do SQL Server integra-se de alertas com [Centro de segurança do Azure](https://azure.microsoft.com/services/security-center/), e, em cada instância geridos protegido é faturada o mesmo preço como o escalão padrão de centro de segurança do Azure, no $15/nó/mês, onde cada protegidas instância geridos é contabilizado como um nó.  

## <a name="set-up-threat-detection-for-your-managed-instance-in-the-azure-portal"></a>Configurar a deteção de ameaças para a instância geridos no portal do Azure
1. Inicie o portal do Azure em [ https://portal.azure.com ](https://portal.azure.com).
2. Navegue para a página de configuração da instância gerido que pretende proteger. No **definições** página, selecione **a deteção de ameaças**. 
3. Na página de configuração da deteção de ameaças 
   - Ativar **ON** deteção de ameaças.
   - Configurar o **lista de e-mails** para receber alertas de segurança após a deteção de atividades de base de dados anómalas.
   - Selecione o **conta do storage do Azure** onde os registos de auditoria de ameaça anómalos são guardados. 
4.  Clique em **guardar** para guardar a política de deteção de ameaças novos ou atualizados.

   ![Deteção de ameaças](./media/sql-database-managed-instance-threat-detection/threat-detection.png)

## <a name="explore-anomalous-managed-instance-activities-upon-detection-of-a-suspicious-event"></a>Explorar a atividades anómalas de instância geridas mediante a deteção de um evento suspeita

1. Receberá uma notificação por e-mail mediante a deteção de atividades de base de dados anómalas. 

   O e-mail fornece informações sobre o evento de segurança suspeita, incluindo a natureza as atividades anómalas, nome de base de dados, nome do servidor e a hora do evento. Além disso, fornece informações sobre as causas possíveis e as ações para investigar e mitigar a potencial ameaça à instância geridos recomendadas.

   ![Deteção de ameaças-correio eletrónico](./media/sql-database-managed-instance-threat-detection/threat-detection-email.png)

2. Clique em de **vê os alertas recentes do SQL Server** ligação no e-mail para iniciar o portal do Azure e mostrar a página de alertas do Centro de segurança do Azure, que fornece uma descrição geral do Active Directory ameaças SQL detetado na base de dados da instância geridos.

   ![ameaças ativas](./media/sql-database-managed-instance-threat-detection/active-threats.png)

3. Clique num alerta específico para obter detalhes adicionais e ações para investigar esta ameaça e a remediação ameaças futuras.

   Por exemplo, a injeção de SQL é uma dos problemas comuns de segurança de aplicação Web na Internet. Injeção de SQL é utilizada para atacar aplicações condicionada por dados. Os atacantes tirar partido de vulnerabilidades de aplicação para injetar maliciosas instruções SQL para campos de entrada de aplicação, ser, ou modificar dados na base de dados. Existência de alertas de Injeção de SQL, detalhes do alerta incluem a instrução SQL vulnerável que foi forem explorada.

   ![injeção de SQL](./media/sql-database-managed-instance-threat-detection/sql-injection.png)

## <a name="managed-instance-threat-detection-alerts"></a>Alertas de deteção de ameaças de instância geridas 

Para a instância geridos, a deteção de ameaças Deteta atividades anómalas, indicando invulgar e potencialmente prejudicial tenta aceder ou explorar menores, bases de dados e pode acionar os seguintes alertas:
- **Vulnerabilidade a Injeção de SQL**: este alerta é acionado quando uma aplicação gera uma instrução SQL defeituosa na base de dados. Tal pode indicar uma possível vulnerabilidade a ataques de injeção de SQL. Existem dois motivos possíveis para a geração de uma instrução defeituosa:
 - Um defeito no código da aplicação que constitui a instrução SQL defeituosa
 - O código de aplicação ou os procedimentos armazenados não saneiam a entrada de utilizador ao criar a instrução SQL defeituosa, o que pode ser explorado para Injeção SQL
- **Potencial injeção SQL**: este alerta é acionado quando uma exploração ativa ocorre contra uma vulnerabilidade de aplicação identificada para a injeção SQL. Significa que o atacante está a tentar inserir maliciosas instruções SQL com o código da aplicação vulnerável ou procedimentos armazenados.
- **Acesso a partir da localização invulgar**: este alerta é acionado quando existe uma alteração no padrão de acesso a uma instância gerido, onde alguém iniciou sessão no instância geridos a partir de uma localização geográfica invulgar. Em alguns casos, o alerta Deteta uma ação legítima (uma nova aplicação ou operação de manutenção de programadores). Noutros casos, o alerta Deteta uma ação maliciosa (empregado anteriores, atacante externo e assim sucessivamente).
- **Acesso a partir do Centro de dados do Azure invulgar**: este alerta é acionado quando existe uma alteração no padrão de acesso à instância gerido, onde alguém iniciou sessão no instância geridos de um centro de dados do Azure que não foi visualizada aceder a este gerido Instância durante o período de recente. Em alguns casos, o alerta Deteta uma ação legítima (a nova aplicação no Azure, o Power BI, o Editor de consultas de SQL do Azure e assim sucessivamente). Noutros casos, o alerta deteta uma ação maliciosa de um recurso/serviço do Azure (ex-funcionário, atacante externo).
- **Acesso a partir de principal familiarizado**: este alerta é acionado quando existe uma alteração no padrão de acesso ao servidor de instância gerido, onde alguém tem sessão iniciada para a instância geridos utilizando um principal de atividade invulgar (utilizador do SQL Server). Em alguns casos, o alerta Deteta uma ação legítima (operação de manutenção do novo Programador de aplicações). Noutros casos, o alerta deteta uma ação maliciosa (ex-funcionário, atacante externo).
- **Acesso de uma localização potencialmente prejudicial**: este alerta é acionado quando uma aplicação potencialmente prejudicial é utilizada para aceder à base de dados. Em alguns casos, o alerta deteta testes de penetração em ação. Noutros casos, o alerta deteta um ataque através de ferramentas de ataque comuns.
- **Credenciais SQL de força bruta**: este alerta é acionado quando existe um número anormalmente elevado de inícios de sessão falhados com diferentes credenciais. Em alguns casos, o alerta deteta testes de penetração em ação. Noutros casos, o alerta Deteta um ataque de força bruta.

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre a instância geridos, consulte [o que é uma instância geridos](sql-database-managed-instance.md)
- Saiba mais sobre [geridos a auditoria de instância](https://go.microsoft.com/fwlink/?linkid=869430) 
- Saiba mais sobre [Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-intro)
