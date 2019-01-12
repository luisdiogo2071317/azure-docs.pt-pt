---
title: Deteção de ameaças - base de dados SQL do Azure | Documentos da Microsoft
description: Deteção de ameaças Deteta atividades anómalas da base de dados, indicando potenciais ameaças de segurança para a base de dados num único banco de dados ou conjunto elástico.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 01/11/2019
ms.openlocfilehash: 330726eecc19659d978b1072ad02ad6d5a4ccb8b
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2019
ms.locfileid: "54244380"
---
# <a name="azure-sql-database-threat-detection-for-single-database"></a>Deteção de ameaças da base de dados SQL do Azure para a base de dados

SQL do Azure [deteção de ameaças](sql-database-threat-detection-overview.md) para [base de dados SQL](sql-database-technical-overview.md) bases de dados individuais Deteta atividades anómalas que indiquem tentativas invulgares e potencialmente prejudiciais de acesso ou exploração de bases de dados. Deteção de ameaças pode identificar **injeção SQL potencial**, **acesso a partir do Centro de dados ou localização invulgar**, **acesso a partir de familiarizado aplicativo de principal ou potencialmente prejudicial**, e **credenciais SQL de força bruta** -veja mais detalhes no [alertas de deteção de ameaças](sql-database-threat-detection-overview.md#azure-sql-database-threat-detection-alerts).

Pode receber notificações sobre as ameaças detetadas através de [notificações por e-mail](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) ou [portal do Azure](sql-database-threat-detection-overview.md#explore-threat-detection-alerts-for-your-database-in-the-azure-portal)

[Deteção de ameaças](sql-database-threat-detection-overview.md) faz parte da [proteção contra ameaças avançadas do SQL](sql-advanced-threat-protection.md) oferta (ATP), o que é um pacote unificado para funções de segurança avançadas do SQL. Deteção de ameaças pode ser acessada e gerenciada através do portal SQL ATP central. O serviço de deteção de ameaças é cobrado 15$ / mês por servidor lógico, com os primeiros 30 dias sem encargos.

## <a name="set-up-threat-detection-for-your-database-in-the-azure-portal"></a>Configurar a deteção de ameaças da base de dados no portal do Azure

1. Iniciar o portal do Azure no [ https://portal.azure.com ](https://portal.azure.com).
2. Navegue para a página de configuração do servidor SQL Database do Azure que pretende proteger. Nas definições de segurança, selecione **proteção avançada contra ameaças**.
3. Sobre o **proteção avançada contra ameaças** página de configuração:

   - Ative a proteção avançada contra ameaças no servidor.
   - Na **as definições de deteção de ameaças**, na **enviar alertas para** texto caixa, forneça a lista de e-mails para receber alertas de segurança após a deteção de atividades anómalas da base de dados.
  
   ![Configurar a deteção de ameaças](./media/sql-database-threat-detection/set_up_threat_detection.png)

## <a name="set-up-threat-detection-using-powershell"></a>Configurar a deteção de ameaças através do PowerShell

Para obter um exemplo de script, consulte [configurar a deteção de ameaças e auditoria com o PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [deteção de ameaças](sql-database-threat-detection-overview.md).
- Saiba mais sobre [deteção de ameaças na instância gerida](sql-database-managed-instance-threat-detection.md).  
- Saiba mais sobre [proteção do SQL avançada contra ameaças](sql-advanced-threat-protection.md).
- Saiba mais sobre [auditoria de base de dados SQL do Azure](sql-database-auditing.md)
- Saiba mais sobre [Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-intro)
- Para obter mais informações sobre os preços, consulte o [página de preços de base de dados SQL](https://azure.microsoft.com/pricing/details/sql-database/)  
