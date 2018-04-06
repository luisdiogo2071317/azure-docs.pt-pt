---
title: Azure SQL Database automáticas georredundante cópias de segurança | Microsoft Docs
description: Base de dados SQL cria uma cópia de segurança da base de dados local cada alguns minutos e utiliza o Azure armazenamento georredundante com acesso de leitura para a redundância geográfica automaticamente.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: article
ms.workload: Active
ms.date: 04/04/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: ab1793621950fd57d3f0be545772d85b32f5d7b8
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
# <a name="learn-about-automatic-sql-database-backups"></a>Saiba mais sobre cópias de segurança de base de dados SQL automáticas

Base de dados SQL cria cópias de segurança da base de dados e utiliza o armazenamento com redundância geográfica do Azure acesso de leitura (RA-GRS) para fornecer redundância geográfica automaticamente. Estas cópias de segurança são criadas automaticamente e, sem encargos adicionais. Não precisa de fazer nada para torná-las a acontecer. Cópias de segurança da base de dados são uma parte essencial de qualquer estratégia de recuperação de continuidade e desastre negócio porque protegem os dados de danos acidentais ou eliminação. Se pretender manter cópias de segurança na sua própria contentor de armazenamento pode configurar uma política de retenção de cópias de segurança de longa duração. Para obter mais informações, veja [Retenção de longa duração](sql-database-long-term-retention.md).

## <a name="what-is-a-sql-database-backup"></a>O que é uma cópia de segurança da base de dados SQL?

Base de dados do SQL Server utiliza tecnologia do SQL Server para criar [completa](https://msdn.microsoft.com/library/ms186289.aspx), [diferencial](https://msdn.microsoft.com/library/ms175526.aspx), e [registo de transações](https://msdn.microsoft.com/library/ms191429.aspx) cópias de segurança para o objetivo de ponto no tempo restaurar (PITR). As cópias de segurança do registo de transações acontecem, geralmente, a cada 5-10 minutos, com a frequência com base no nível de desempenho e a quantidade de atividade de base de dados. Transação registo as cópias de segurança, com as cópias de segurança completas e diferenciais, permitem-lhe restaurar uma base de dados para um específico ponto no tempo para o mesmo servidor que aloja a base de dados. Ao restaurar uma base de dados, figuras do serviço de saída que registo completo, valor diferencial e transação cópias de segurança tem de ser restaurados.


Pode utilizar estas cópias de segurança para:

* Restaure uma base de dados para um ponto no tempo dentro do período de retenção. Esta operação irá criar uma nova base de dados no mesmo servidor que a base de dados original.
* Restaure uma base de dados eliminada para o tempo que foi eliminado ou a qualquer altura dentro do período de retenção. A base de dados eliminada só pode ser restaurado no mesmo servidor onde a base de dados original foi criado.
* Restaure uma base de dados para outra região geográfica. Isto permite-lhe recuperar de desastres geográfica quando não é possível aceder ao servidor e base de dados. Cria uma nova base de dados em qualquer servidor existente em qualquer lugar do mundo. 
* Restaure uma base de dados a partir de uma cópia de segurança de longa duração específica se a base de dados foi configurado com uma política de retenção de longo prazo. Isto permite-lhe restaurar uma versão antiga da base de dados para satisfazer um pedido de conformidade ou para executar uma versão antiga da aplicação. Consulte [retenção de longo prazo](sql-database-long-term-retention.md).
* Para efetuar um restauro, consulte [restaurar base de dados de cópias de segurança](sql-database-recovery-using-backups.md).

> [!NOTE]
> No armazenamento do Azure, o termo *replicação* refere-se ao copiar ficheiros de uma localização para outra. Do SQL Server *replicação de base de dados* refere-se a manter a várias bases de dados secundárias sincronizados com uma base de dados primária. 
> 

## <a name="how-often-do-backups-happen"></a>Frequência cópias de segurança acontecem?
Cópias de segurança da base de dados completa semanal, acontecem cópias de segurança da base de dados diferencial acontecem, geralmente, cada algumas horas e registo de transações cópias de segurança acontecem, geralmente, a cada 5-10 minutos. A primeira cópia de segurança completa é agendada imediatamente depois de uma base de dados. Normalmente, terminar dentro de 30 minutos, mas poderá demorar mais quando a base de dados é de um tamanho significativo. Por exemplo, a cópia de segurança inicial pode demorar mais tempo na base de dados restaurada ou uma cópia da base de dados. Após a primeira cópia de segurança completa, todas cópias de segurança adicionais são agendadas automaticamente e geridas automaticamente em segundo plano. A temporização exata de todas as cópias de segurança da base de dados é determinada pelo serviço de base de dados do SQL Server como equilibra a carga de trabalho geral do sistema. 

A armazenamento de cópia de segurança a georreplicação ocorre com base na agenda de replicação de armazenamento do Azure.

## <a name="how-long-do-you-keep-my-backups"></a>O período de tempo, manter a minha cópias de segurança?
Cada cópia de segurança da base de dados do SQL Server tem um período de retenção baseia o [camada de serviço](sql-database-service-tiers.md) da base de dados. O período de retenção para uma base de dados no:


* Camada de serviço básico é de 7 dias.
* Camada de serviço Standard é 35 dias.
* Camada de serviço Premium é 35 dias.
* Camada de objetivo geral é configurável com máximos de 35 dias (7 dias por predefinição) *
* Camada de negócio crítico (pré-visualização) é configurável com máximos de 35 dias (7 dias por predefinição) *

\* Durante a pré-visualização, o período de retenção de cópias de segurança não é configurável e é resolvido para 7 dias.

Se converter uma base de dados com maior retenção de cópias de segurança para uma base de dados com retenção mais curta, todas as cópias de segurança existentes anteriores ao período de retenção de camada de destino já não estão disponíveis.

Se atualizar uma base de dados com um período de retenção mais curto para uma base de dados com um período mais longo, base de dados SQL mantém cópias de segurança existentes até que o período de retenção mais longo for atingido. 

Se eliminar uma base de dados, a base de dados SQL mantém as cópias de segurança da mesma forma que faria para uma base de dados online. Por exemplo, suponha que elimine uma base de dados básica que tem um período de retenção de sete dias. É guardada uma cópia de segurança que é de quatro dias de antiguidade para três dias mais.

> [!IMPORTANT]
> Se eliminar o Azure SQL server que aloja as bases de dados do SQL Server, todas as bases de dados que pertencem ao servidor também são eliminados e não podem ser recuperados. Não é possível restaurar um servidor eliminado.
> 

## <a name="how-to-extend-the-backup-retention-period"></a>Como expandir o período de retenção de cópias de segurança?

Se a sua aplicação requer que as cópias de segurança estão disponíveis para o período de tempo que o período de retenção de cópias de segurança PITR máximo, pode configurar uma política de retenção de cópias de segurança de longa duração para bases de dados individuais (política imediatamente disponíveis). Isto permite-lhe expandir o período de retenção de incorporada-it do máximo de 35 dias para até 10 anos. Para obter mais informações, veja [Retenção de longa duração](sql-database-long-term-retention.md).

Depois de adicionar a política de imediatamente disponíveis para uma base de dados utilizando o portal do Azure ou a API, as cópias de segurança da base de dados completa semanal serão copiadas automaticamente para um contentor de armazenamento RA-GRS separado para a retenção de longa duração (armazenamento imediatamente disponíveis). Se a base de dados é encriptado com TDE as cópias de segurança são encriptadas automaticamente inativos. Base de dados SQL automaticamente eliminará as cópias de segurança expiradas com base no respetivo timestamp e a política de imediatamente disponíveis. Depois de configurar a política, não precisa de gerir a agenda de cópia de segurança ou preocupar com a limpeza dos ficheiros antigos. Pode utilizar o portal do Azure ou o PowerShell para ver, restaurar ou eliminar estas cópias de segurança.

## <a name="are-backups-encrypted"></a>As cópias de segurança são encriptadas?

Quando TDE está ativada para uma base de dados SQL do Azure, cópias de segurança também são encriptadas. Todas as novas bases de dados do SQL do Azure estão configurados com TDE ativada por predefinição. Para obter mais informações sobre TDE, consulte [encriptação transparente de dados com a SQL Database do Azure](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

## <a name="are-the-automatic-backups-compliant-with-gdpr"></a>As cópias de segurança automáticas são compatíveis com GDPR?
Se a cópia de segurança contiver dados pessoais, que está sujeita a Regulamento de proteção de dados gerais (GDPR), é necessário para aplicar medidas de segurança avançada para proteger os dados contra acesso não autorizado. Para cumprir o GDPR, terá de uma forma de gerir os pedidos de dados de proprietários de dados sem ter de aceder a cópias de segurança.  Para cópias de segurança de curto prazo, uma solução pode ser bastante a cópia de segurança janela em 30 dias, que é o tempo permitido para concluir os pedidos de acesso de dados.  Se forem necessárias mais termo as cópias de segurança, recomenda-se para armazenar apenas os dados de "pseudonymized" em cópias de segurança. Por exemplo, se os dados sobre uma pessoa tem de ser eliminada ou atualizada, não serão necessários eliminar ou atualizar as cópias de segurança existentes. Pode encontrar mais informações sobre as GDPR as melhores práticas no [governação de dados de conformidade de GDPR](https://info.microsoft.com/DataGovernanceforGDPRCompliancePrinciplesProcessesandPractices-Registration.html).

## <a name="next-steps"></a>Passos Seguintes

- Cópias de segurança da base de dados são uma parte essencial de qualquer estratégia de recuperação de continuidade e desastre negócio porque protegem os dados de danos acidentais ou eliminação. Para saber mais sobre as outras soluções do SQL Database do Azure negócio continuidade, consulte [descrição geral da continuidade do negócio](sql-database-business-continuity.md).
- Restaurar para um ponto no tempo no portal do Azure, consulte [restaurar base de dados para um ponto no tempo no portal do Azure](sql-database-recovery-using-backups.md).
- Restaurar para um ponto no tempo com o PowerShell, consulte [restaurar base de dados para um ponto no tempo com o PowerShell](scripts/sql-database-restore-database-powershell.md).
- Para configurar, gerir e restaurar a partir de retenção de longo prazo de automática cópias de segurança num cofre dos serviços de recuperação do Azure no portal do Azure, consulte [gerir retenção de cópias de segurança de longa duração no portal do Azure](sql-database-long-term-backup-retention-configure.md).
- Para configurar, gerir e restaurar a partir de retenção de longa duração das cópias de segurança num cofre do Azure Recovery Services com o PowerShell automatizadas, consulte [gerir retenção de cópias de segurança de longo prazo com o PowerShell](sql-database-long-term-backup-retention-configure.md).
