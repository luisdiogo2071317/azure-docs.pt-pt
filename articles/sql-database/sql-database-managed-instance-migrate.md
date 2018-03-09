---
title: "Migrar a instância do SQL Server para instância geridos base de dados SQL do Azure | Microsoft Docs"
description: "Saiba como migrar uma instância do SQL Server para a base de dados geridas por instância de SQL do Azure."
keywords: "migração de base de dados, migração de base de dados do sql server, ferramentas de migração de base de dados, migrar base de dados, migrar base de dados sql"
services: sql-database
author: bonova
ms.reviewer: carlrab
manager: cguyer
ms.service: sql-database
ms.custom: managed instance
ms.topic: article
ms.date: 03/07/2018
ms.author: bonova
ms.openlocfilehash: 44a02ac20a9e9e5fb7171d28e4852ab1a6e63881
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="sql-server-instance-migration-to-azure-sql-database-managed-instance"></a>Migração de instância do SQL Server para instância geridos base de dados SQL do Azure

Neste artigo, pode saber mais sobre os métodos para migrar um SQL Server 2005 ou posterior instância de versão para o Azure instância SQL Server da base de dados geridos (pré-visualização). 

> [!NOTE]
> Para migrar uma base de dados para uma base de dados individual ou um conjunto elástico, consulte [migrar uma base de dados do SQL Server a SQL Database do Azure](sql-database-cloud-migrate.md).

A Instância Gerida da Base de Dados SQL é uma expansão do serviço de Base de Dados SQL existente, que fornece uma terceira opção de implementação juntamente com as bases de dados individuais e conjuntos elásticos.  Foi concebido para ativar a base de dados comparação de precisão-e-shift para uma PaaS completamente gerido, ao reestruturar a aplicação. A Instância Gerida da Base de Dados SQL oferece compatibilidade superior com o modelo de programação SQL Server no local e suporte disponível para a grande maioria das funcionalidades do SQL Server, bem como as ferramentas e serviços que acompanham.

A nível elevado, o processo de migração de aplicação aspeto no diagrama seguinte:

![processo de migração](./media/sql-database-managed-instance-migration/migration-process.png)

## <a name="assess-managed-instance-compatibility"></a>Avaliar a compatibilidade de instância geridos

Em primeiro lugar, determine se a instância de geridos é compatível com os requisitos da base de dados da sua aplicação. Instância gerida foi concebida para fornecer fácil comparação de precisão e shift migração para a maioria das aplicações existentes que utilizam o SQL Server no local ou em máquinas virtuais. No entanto, por vezes, pode necessitar de funcionalidades ou capacidades que ainda não são suportadas e o custo de implementar uma solução são demasiado elevadas. 

Utilize [através do Assistente de dados de migração (DMA)](https://docs.microsoft.com/sql/dma/dma-overview) para detetar potenciais com impacto na funcionalidade de base de dados na base de dados do Azure SQL de problemas de compatibilidade. DMA ainda não suporta instância geridos como destino de migração, mas é recomendado executar avaliação SQL Database do Azure e reveja cuidadosamente a lista de paridade de funcionalidades comunicado e problemas de compatibilidade em relação a documentação do produto. A maioria dos problemas de bloqueios impedir uma migração para a SQL Database do Azure foram removida com instância geridos. Para a instância, as funcionalidades, como consultas entre bases de dados, transações entre bases de dados na instância do mesma, o servidor ligado para outras origens, CLR, global temporários tabelas do SQL, vistas de nível de instância, o Mediador de serviço e o tipo estão disponíveis em instâncias geridas. 

No entanto, existem alguns casos quando precisa de considerar uma opção alternativa, tal como [SQL Server em máquinas virtuais no Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/). Eis alguns exemplos:

- Se necessitar de acesso direto para o sistema operativo ou o sistema de ficheiros, por exemplo, a instalação de terceiros ou agentes personalizados na mesma máquina virtual com o SQL Server.
- Se tiver dependência strict nas funcionalidades que ainda não são suportadas, tais como FileStream / FileTable, PolyBase e transações entre instância.
- Se absolutamente tem de permanecer numa versão específica do SQL Server (2012, para a instância).
- Se os requisitos de computação são muito inferiores essa instância geridos oferece em pré-visualização pública (um vCore, para a instância) e a consolidação de base de dados não é aceitável opção.

## <a name="choose-app-connectivity-option"></a>Escolha a opção de conectividade de aplicação

Instância gerida é totalmente incluída na sua rede virtual, pelo que fornece o nível de segurança e isolamento ultimate para os seus dados. O diagrama seguinte mostra várias opções para implementar várias topologias de aplicação completamente no Azure ou num ambiente híbrido, independentemente se escolher um serviço completamente gerido ou alojado modelo para as suas aplicações de front-end.

![topologias de implementação de aplicação](./media/sql-database-managed-instance-migration/application-deployment-topologies.png)

Qualquer uma das opções selecionadas permitem uma conectividade para um SQL Server ponto final apenas através de endereços IP privados, que garante que o nível de isolamento para os seus dados ideal. <!--- For more information, see How to connect your application to Managed Instance.--->

## <a name="deploy-to-an-optimally-sized-managed-instance"></a>Implementar uma instância de forma ideal tamanho geridos

Instância gerida é adaptada para cargas de trabalho no local que estão a planear mover para a nuvem. Introduz um novo modelo de compra que fornece uma maior flexibilidade na selecionando o nível adequado de recursos para as cargas de trabalho. No mundo no local, está provavelmente habituado a estas cargas de trabalho de dimensionamento utilizando núcleos físicos. O novo modelo de compra para a instância geridos é com base no virtuais núcleos, ou "vCores", com armazenamento adicional e/s disponíveis em separado. O modelo de vCore é um mais simples de forma a compreender os requisitos de computação na nuvem versus o que utiliza no local hoje. Este novo modelo permite-lhe dimensionar o ambiente de destino na nuvem.

Pode selecionar a computação e recursos de armazenamento na implementação de tempo e, em seguida, altere-a, posteriormente, sem introduzir período de indisponibilidade para a sua aplicação.

![dimensionamento de instância gerido](./media/sql-database-managed-instance-migration/managed-instance-sizing.png)

Para saber como criar a infraestrutura de VNet e geridos instância - e restaurar uma base de dados a partir de um ficheiro de cópia de segurança, consulte [criar uma instância geridos](sql-database-managed-instance-tutorial-portal.md).

> [!IMPORTANT]
> É importante manter o seu destino VNet e sub-rede sempre em accordance com [requisitos geridos VNET de instância](sql-database-managed-instance-vnet-configuration.md#requirements). Qualquer incompatibilidade pode impedi-lo de criar novas instâncias ou utilizando o aqueles que já criou.

## <a name="select-migration-method-and-migrate"></a>Selecione o método de migração e migrar

Gerido cenários de utilizador de destinos de instância exigindo a migração de base de dados em massa no local ou implementações de base de dados de IaaS. Estão a escolha ideal quando precisar de comparação de precisão e deslocar o back-end das aplicações que regularmente utilizam o nível de instância e / ou entre bases de dados funcionalidades. Se este for o seu cenário, pode mover uma instância completa para um ambiente correspondente no Azure sem a necessidade de rearchitecture as suas aplicações. 

Mover instâncias do SQL Server, terá de planear cuidadosamente:

-   A migração de todas as bases de dados que têm de ser colocalizados (aqueles em execução na mesma instância)
-   A migração de objetos de nível de instância que a aplicação depende, incluindo inícios de sessão, as credenciais, as tarefas de agente do SQL Server e operadores e acionadores ao nível do servidor. 

Instância gerida é um serviço completamente gerido que lhe permite delegar algumas das atividades DBA regulares da plataforma, que estão incorporados no. Por conseguinte, alguns dados de nível de instância não tem de ser migradas, tais como tarefas de manutenção de cópias de segurança regulares ou sempre em configuração, como [elevada disponibilidade](sql-database-high-availability.md) está incorporada.

Instância gerida suporta as seguintes várias opções de migração de base de dados:

### <a name="azure-database-migration-service"></a>Serviço de Migração de Bases de Dados do Azure

O [Azure serviço de migração da base de dados (DMS)](../dms/dms-overview.md) é um serviço completamente gerido concebido para permitir migrações totalmente integradas de várias origens de base de dados para plataformas de dados do Azure com o período de indisponibilidade mínimo. Este serviço simplifica as tarefas necessárias para mover terceiros existente e bases de dados do SQL Server para o Azure. Opções de implementação em pré-visualização pública incluem a SQL Database do Azure, geridos instância e do SQL Server uma Máquina Virtual no Azure. O DMS é o método recomendado para migração para as cargas de trabalho da empresa. 

![DMS](./media/sql-database-managed-instance-migration/dms.png)

Para mais informações sobre passos neste cenário e a configuração para o DMS, consulte o artigo [migrar no local da base de dados à instância geridos utilizando o DMS](../dms/tutorial-sql-server-to-azure-sql.md).  

### <a name="native-restore-from-url"></a>Nativo RESTAURO a partir do URL

RESTAURO de cópias de segurança nativas (ficheiros. bak) obtidas a partir do SQL Server no local ou [SQL Server em máquinas virtuais](https://azure.microsoft.com/services/virtual-machines/sql-server/), disponível no [Storage do Azure](https://azure.microsoft.com/services/storage/), é uma das principais capacidades na instância de geridos de BD do SQL que permite rápido e fácil offline da base de migração. 

O diagrama seguinte explica o processo de alto nível:

![migration-flow](./media/sql-database-managed-instance-migration/migration-flow.png)

A tabela seguinte fornece mais informações sobre o método que pode utilizar, dependendo da versão do SQL Server de origem estiver a executar:

|Passo|Motor de SQL e versão|Cópia de segurança / restaurar método|
|---|---|---|
|Colocar a cópia de segurança para armazenamento do Azure|Prior SQL 2012 SP1 CU2|Carregar o ficheiro. bak diretamente ao armazenamento do Azure|
||2012 SP1 CU2 - 2016|Utilizar cópias de segurança do direta preterido [WITH CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql) sintaxe|
||2016 e acima|Utilizando a cópia de segurança direta [com CREDENCIAIS de SAS](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url)|
|Restaurar a partir de armazenamento do Azure para a instância gerida|[Restaurar de URL com a CREDENCIAL de SAS](sql-database-managed-instance-tutorial-portal.md#restore-the-wide-world-importers-database-from-a-backup-file)|

> [!IMPORTANT]
> PRestore das bases de dados do sistema não é suportada. Para migrar objetos do nível de instância (armazenados nas bases de dados mestra ou msdb), é recomendável criar scripts-los e executar scripts T-SQL na instância de destino.

Para um tutorial completo que inclui a restaurar uma cópia de segurança da base de dados para uma instância geridos utilizando uma credencial SAS, consulte [criar uma instância geridos](sql-database-managed-instance-tutorial-portal.md).

### <a name="migrate-using-bacpac-file"></a>Migrar através do ficheiro BACPAC

Pode importar a SQL Database do Azure e geridos por instância de um criar uma cópia da base de dados original, com os dados, num ficheiro BACPAC. Consulte [importar um ficheiro BACPAC para uma nova base de dados do Azure SQL](sql-database-import.md).

## <a name="monitor-applications"></a>Monitorizar aplicações

Controlar o comportamento da aplicação e o desempenho após a migração. Na instância geridos, algumas alterações são ativadas apenas uma vez a [nível de compatibilidade de base de dados foi alterado](https://docs.microsoft.com/sql/relational-databases/databases/view-or-change-the-compatibility-level-of-a-database). Migração de base de dados para a SQL Database do Azure mantém o nível de compatibilidade original na maioria dos casos. Se o nível de compatibilidade de uma base de dados do utilizador foi 100 ou superior antes da migração, este irá permanecer o mesmo após a migração. Se o nível de compatibilidade de uma base de dados do utilizador foi 90 antes da migração, na base de dados atualizada, o nível de compatibilidade é definido para 100, que é o nível mais baixa suportada compatibilidade na instância geridos. Nível de compatibilidade de bases de dados do sistema é 140.

Para reduzir riscos de migração, altere o nível de compatibilidade de base de dados apenas após a monitorização do desempenho. Utilize o arquivo de consultas como ferramenta ideal para obter informações sobre o desempenho da carga de trabalho antes e após alteração de nível de compatibilidade da base de dados, conforme explicado no [manter a estabilidade do desempenho durante a atualização para a versão mais recente do SQL Server](https://docs.microsoft.com/sql/relational-databases/performance/query-store-usage-scenarios#CEUpgrade).

Assim que uma plataforma completamente gerida, tome as vantagens que são fornecidas automaticamente como parte do serviço base de dados SQL. Por exemplo, não tem de criar cópias de segurança na instância geridos - o serviço efetua cópias de segurança para si automaticamente. Pode já não tem preocupar agendamento, tendo e gerir cópias de segurança. Instância gerida fornece a capacidade de restaurar para qualquer ponto no tempo nesta utilizando do período de retenção [ponto no tempo de recuperação (PITR)](sql-database-recovery-using-backups.md#point-in-time-restore). Durante a pré-visualização pública, o período de retenção for corrigido para sete dias.
Além disso, não precisa de preocupar com a definição de elevada disponibilidade como [elevada disponibilidade](sql-database-high-availability.md) está incorporada.

Para reforçar a segurança, considere a utilização de algumas das funcionalidades que estão disponíveis:
- Azure ao nível da base de dados a autenticação do Active Directory
- Auditoria e deteção de ameaças para monitorizar atividades
- Controlar o acesso a dados confidenciais e com privilégios ([segurança ao nível da linha](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) e [máscara de dados dinâmicos](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking)).

## <a name="next-steps"></a>Passos Seguintes

- Para obter informações sobre a instância geridos, consulte [o que é uma instância gerido?](sql-database-managed-instance.md)
- Para um tutorial que inclui um restauro a partir da cópia de segurança, consulte [criar uma instância geridos](sql-database-managed-instance-tutorial-portal.md).
- Tutorial de apresentação para migração de utilizando DMS, consulte [migrar no local da base de dados à instância geridos utilizando o DMS](../dms/tutorial-sql-server-to-azure-sql.md).  
