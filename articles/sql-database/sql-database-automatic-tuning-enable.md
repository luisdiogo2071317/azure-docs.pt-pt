---
title: Ativar o ajuste automático da base de dados do Azure SQL | Documentos da Microsoft
description: Pode ativar a otimização automática na base de dados do Azure SQL facilmente.
services: sql-database
author: danimir
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: v-daljep
ms.reviewer: carlrab
ms.openlocfilehash: 147d9ecfa64267322aeef40d84e1f3c79611b2f0
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39593002"
---
# <a name="enable-automatic-tuning"></a>Ativar o ajuste automático

Base de dados SQL do Azure é um serviço de dados automaticamente geridos que constantemente monitoriza as suas consultas e identifica a ação que pode efetuar para melhorar o desempenho da carga de trabalho. Pode rever as recomendações e manualmente aplicá-las ou permitir que a base de dados do SQL Azure automaticamente aplicar ações corretivas – isso é conhecido como **modo de sintonização automática**.

Otimização automática pode ser ativada ao servidor ou ao nível de base de dados através da [portal do Azure](sql-database-automatic-tuning-enable.md#azure-portal), [REST API](sql-database-automatic-tuning-enable.md#rest-api) chamadas e [T-SQL](sql-database-automatic-tuning-enable.md#t-sql) comandos.

## <a name="enable-automatic-tuning-on-server"></a>Ativar o ajuste automático no servidor
No nível do servidor pode escolher para herdar a configuração de otimização automática do "O Azure utiliza por predefinição" ou não para herdar a configuração. Predefinições do Azure são FORCE_LAST_GOOD_PLAN está ativada, CREATE_INDEX está ativada e DROP_INDEX está desativada.

### <a name="azure-portal"></a>Portal do Azure
Para ativar a otimização automática na lógica do Azure SQL Database **servidor**, navegue para o servidor no portal do Azure e, em seguida, selecione **otimização automática** no menu.

![Servidor](./media/sql-database-automatic-tuning-enable/server.png)

> [!NOTE]
> Tenha em atenção que **DROP_INDEX** opção neste momento não é compatível com aplicativos utilizar sugestões de índice e de alternância de partição e não deve ser ativada nestes casos.
>

Selecione as opções de otimização automática que pretende ativar e selecione **aplicar**.

Opções de otimização automática num servidor são aplicadas a todas as bases de dados neste servidor. Por predefinição, todas as bases de dados herdam a configuração do seu servidor principal, mas isso pode ser substituído e especificado individualmente para cada base de dados.

### <a name="rest-api"></a>API REST

Saiba mais sobre como utilizar a REST API para ativar o ajuste automático num servidor, consulte [SQL Server de otimização automática métodos UPDATE e GET HTTP](https://docs.microsoft.com/rest/api/sql/serverautomatictuning).


## <a name="enable-automatic-tuning-on-an-individual-database"></a>Ativar o ajuste automático numa base de dados individual

A base de dados do SQL do Azure permite-lhe especificar individualmente a configuração de otimização automática para cada base de dados. O nível de base de dados pode optar para herdar a configuração de otimização automática do servidor principal, "O Azure utiliza por predefinição" ou não para herdar a configuração. Predefinições do Azure estão definidas para FORCE_LAST_GOOD_PLAN está ativada, CREATE_INDEX está ativada e DROP_INDEX está desativada.

> [!NOTE]
> A recomendação geral é gerenciar a configuração de otimização automática na **ao nível do servidor** para que as definições de configuração podem ser aplicadas automaticamente em cada base de dados. Configurar a otimização automática numa base de dados individual apenas se tiver essa base de dados ter definições diferentes das de outras pessoas a herdar as definições do mesmo servidor.
>

### <a name="azure-portal"></a>Portal do Azure

Para ativar o ajuste automático num **base de dados individual**, navegue para a base de dados no portal do Azure e selecione **otimização automática**.

Definições de otimização automática individuais podem ser configuradas em separado para cada base de dados. Manualmente pode configurar uma opção de otimização automática individual, ou especificar que uma opção herda as definições do servidor.

![Base de Dados](./media/sql-database-automatic-tuning-enable/database.png)

Tenha em atenção que a opção de DROP_INDEX neste momento não é compatível com aplicativos utilizar sugestões de índice e de alternância de partição e não deve ser ativada nestes casos.

Assim que tiver selecionado a configuração pretendida, clique em **aplicar**.

### <a name="rest-api"></a>API REST

Saiba mais sobre como utilizar a REST API para ativar o ajuste automático numa base de dados, consulte [da base de dados de otimização automática SQL métodos UPDATE e GET HTTP](https://docs.microsoft.com/rest/api/sql/databaseautomatictuning).

### <a name="t-sql"></a>T-SQL

Para ativar uma base de dados através de T-SQL de otimização automática, ligue à base de dados e execute a seguinte consulta:

   ```T-SQL
   ALTER DATABASE current SET AUTOMATIC_TUNING = AUTO | INHERIT | CUSTOM
   ```
   
A definição de otimização automática como AUTO aplicará predefinições do Azure. Defini-la para HERDAR, configuração de otimização automática será herdada do servidor principal. Escolher PERSONALIZADO, terá de configurar manualmente a otimização automática.

Para configurar opções de otimização automática individuais através de T-SQL, ligue à base de dados e executar a consulta como esta:

   ```T-SQL
   ALTER DATABASE current SET AUTOMATIC_TUNING (FORCE_LAST_GOOD_PLAN = ON, CREATE_INDEX = DEFAULT, DROP_INDEX = OFF)
   ```
   
Definir a opção de otimização individual on, irá substituir qualquer definição de base de dados herdada e ativar a opção de otimização. Defini-la como OFF, também substituir qualquer definição de base de dados herdada e desativar a opção de otimização. Opção de otimização automática, para que a predefinição é especificada, irá herdar a configuração do nível da base de dados, a definição de otimização automática.  

Encontre opções de T-SQL para configurar a otimização automática, consulte sobre a nossa mais [alterar base de dados definir opções (Transact-SQL) para o servidor lógico de base de dados SQL](https://docs.microsoft.com/en-us/sql/t-sql/statements/alter-database-transact-sql-set-options?view=sql-server-2017&tabs=sqldbls#arguments-1)

## <a name="disabled-by-the-system"></a>Desativada pelo sistema
A otimização automática está a monitorizar todas as ações que demora na base de dados e, em alguns casos ele pode determinar que a otimização automática não é possível trabalhar corretamente no banco de dados. Nesta situação, opção de otimização será desativada pelo sistema. Na maioria dos casos, isto acontece porque Store de consulta não está ativado ou está no estado só de leitura numa base de dados específico.

## <a name="configure-automatic-tuning-e-mail-notifications"></a>Configurar notificações de e-mail de otimização automática

Ver [notificações por correio eletrónico de otimização automática](sql-database-automatic-tuning-email-notifications.md) guia.

## <a name="next-steps"></a>Passos Seguintes
* Leitura a [artigo de otimização automática](sql-database-automatic-tuning.md) para saber mais sobre otimização automática e como o pode ajudar a melhorar o desempenho.
* Ver [recomendações de desempenho](sql-database-advisor.md) para uma descrição geral das recomendações de desempenho da base de dados do Azure SQL.
* Ver [informações de desempenho de consulta](sql-database-query-performance.md) para saber mais sobre como ver o impacto de desempenho de suas consultas principais.
