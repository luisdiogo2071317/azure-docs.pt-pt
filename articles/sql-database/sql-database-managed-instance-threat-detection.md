---
title: Deteção de ameaças - instância gerida de base de dados SQL do Azure | Documentos da Microsoft
description: Deteção de ameaças Deteta atividades anómalas da base de dados, indicando potenciais ameaças de segurança para a base de dados numa instância gerida.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto
manager: craigg
ms.date: 12/06/2018
ms.openlocfilehash: c59d0ea489343dbf748412910c4f759f601de0e2
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2019
ms.locfileid: "54042381"
---
# <a name="azure-sql-database-managed-instance-threat-detection-preview"></a>Base de dados SQL do Azure geridos a deteção de ameaças da instância (pré-visualização)

SQL do Azure [deteção de ameaças](sql-database-threat-detection-overview.md) para [SQL Database Managed Instance](sql-database-managed-instance-index.yml) Deteta atividades anómalas que indiquem tentativas invulgares e potencialmente prejudiciais de acesso ou exploração de bases de dados. Deteção de ameaças pode identificar **injeção SQL potencial**, **acesso a partir do Centro de dados ou localização invulgar**, **acesso a partir de familiarizado aplicativo de principal ou potencialmente prejudicial**, e **credenciais SQL de força bruta** -veja mais detalhes no [alertas de deteção de ameaças](sql-database-threat-detection-overview.md#azure-sql-database-threat-detection-alerts).

Pode receber notificações sobre as ameaças detetadas através de [notificações por e-mail](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) ou [portal do Azure](sql-database-threat-detection-overview.md#explore-threat-detection-alerts-for-your-database-in-the-azure-portal)

[Deteção de ameaças](sql-database-threat-detection-overview.md) faz parte da [proteção contra ameaças avançadas do SQL](sql-advanced-threat-protection.md) oferta (ATP), o que é um pacote unificado para funções de segurança avançadas do SQL. Deteção de ameaças pode ser acessada e gerenciada através do portal SQL ATP central. O serviço de deteção de ameaças é cobrado 15$ / mês por instância gerida, com os primeiros 30 dias sem encargos.

## <a name="set-up-threat-detection-for-your-managed-instance-in-the-azure-portal"></a>Configurar a deteção de ameaças da sua instância gerida no portal do Azure

1. Iniciar o portal do Azure no [ https://portal.azure.com ](https://portal.azure.com).
2. Navegue para a página de configuração de instância gerida que pretende proteger. Na **configurações** página, selecione **deteção de ameaças**.
3. Na página de configuração de deteção de ameaças
   - Ative **ON** deteção de ameaças.
   - Configurar o **lista de e-mails** para receber alertas de segurança após a deteção de atividades anómalas da base de dados.
   - Selecione o **conta de armazenamento do Azure** onde são guardados os registos de auditoria de ameaças anómalas.
4. Clique em **guardar** para guardar a política de deteção de ameaças novos ou atualizados.

   ![Deteção de ameaças](./media/sql-database-managed-instance-threat-detection/threat-detection.png)

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [deteção de ameaças](sql-database-threat-detection-overview.md).
- Saiba mais sobre a instância gerida, veja [o que é uma instância gerida](sql-database-managed-instance.md).
- Saiba mais sobre [deteção de ameaças da base de dados única](sql-database-threat-detection.md).
- Saiba mais sobre [auditoria de instância gerida](https://go.microsoft.com/fwlink/?linkid=869430).
- Saiba mais sobre [Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-intro).
