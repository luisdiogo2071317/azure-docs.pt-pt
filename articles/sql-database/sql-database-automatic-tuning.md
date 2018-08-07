---
title: Base de dados SQL do Azure - otimização automática | Documentos da Microsoft
description: Base de dados SQL do Azure analisa a consulta SQL e se adapta automaticamente a carga de trabalho do utilizador.
services: sql-database
author: danimir
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: v-daljep
ms.reviewer: carlrab
ms.openlocfilehash: 6ed113d11a0c61620ae4a9d48eee957bb979dc11
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39526798"
---
# <a name="automatic-tuning-in-azure-sql-database"></a>A otimização automática na base de dados do Azure SQL

Otimização automática da base de dados do SQL do Azure fornece desempenho máximo e cargas de trabalho estáveis através da utilização de Inteligência Artificial de ajuste de desempenho contínua.

A otimização automática é um serviço totalmente gerido que utiliza inteligência incorporada para monitorizar continuamente as consultas executadas numa base de dados e melhora automaticamente a seu desempenho. Isso é feito por meio de adaptação dinamicamente a base de dados para as cargas de trabalho de alteração e a aplicar recomendações de otimização. A otimização automática aprende horizontalmente de todas as bases de dados no Azure através de Inteligência Artificial e melhora a suas ações de otimização dinâmica. Mais tempo de execução de uma base de dados do SQL do Azure com ajuste automático em, melhor desempenho.

Otimização automática da base de dados do SQL do Azure pode ser um dos recursos mais importantes que pode ativar para fornecer estável e aumentar o desempenho de cargas de trabalho.

## <a name="what-can-automatic-tuning-do-for-you"></a>O que pode otimização automática fazer por?

- Ajuste de desempenho automatizado de bases de dados do Azure SQL
- Verificação automatizada de ganhos de desempenho
- Reversão automática e correção automática
- Ajuste o registro de histórico
- Ajuste de scripts do T-SQL de ação para as implementações manuais
- Monitorização de desempenho da carga de trabalho proativa
- Aumentar horizontalmente a capacidade em centenas de milhares de bases de dados
- Impacto positivo para recursos de DevOps e o custo total de propriedade

## <a name="safe-reliable-and-proven"></a>Seguro, fiável e comprovado

Operações de otimização aplicadas a bases de dados do Azure SQL são totalmente seguras para o desempenho das cargas de trabalho mais intensa. O sistema foi desenvolvido com cuidado para não interferem com as cargas de trabalho do utilizador. Recomendações de otimização automáticas são aplicadas apenas às horas de uma baixa utilização. O sistema também temporariamente pode desativar a operações de otimização automática para proteger o desempenho da carga de trabalho. Nesse caso, a mensagem "Desativada pelo sistema" será apresentada no portal do Azure. A otimização automática regards cargas de trabalho com a prioridade mais alta do recurso.

Mecanismos de otimização automática são maduros e tem sido ideal em centenas de milhares de bases de dados em execução no Azure. Operações automatizadas de otimização aplicadas são verificadas automaticamente para garantir que existe uma melhoria positiva para o desempenho da carga de trabalho. Recomendações de desempenho regredido são dinamicamente detetadas e imediatamente revertidas. Através do registo de histórico de otimização não existe um rastreio claro de ajuste os aprimoramentos feitos para cada base de dados do SQL do Azure. 

![Como funciona o trabalho de otimização automática](./media/sql-database-automatic-tuning/how-does-automatic-tuning-work.png)

Otimização automática da base de dados do SQL do Azure está a partilhar sua lógica de núcleo com o mecanismo de otimização automática do SQL Server. Para obter informações técnicas adicionais no mecanismo de inteligência incorporada, consulte [otimização automática do SQL Server](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning).

## <a name="use-automatic-tuning"></a>Utilizar a otimização automática

A otimização automática tem de ser habilitada manualmente na sua subscrição. Para ativar a otimização automática através do portal do Azure, consulte [ativar o ajuste automático](sql-database-automatic-tuning-enable.md).

A sintonização automática pode funcionam de forma autónoma por meio de automaticamente aplicar recomendações de otimização, incluindo a verificação automática de ganhos de desempenho. 

Para obter mais controle, a aplicação automática de recomendações de otimização pode ser desativada e recomendações de otimização podem ser aplicadas manualmente através do portal do Azure. Também é possível utilizar a solução para ver automatizadas apenas com recomendações de Otimização e aplicá-las manualmente por meio de scripts e ferramentas da sua preferência. 

Para uma visão geral de como automática funciona de Otimização e para cenários de uso típico, veja o vídeo incorporado:


> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Improve-Azure-SQL-Database-Performance-with-Automatic-Tuning/player]
>

## <a name="automatic-tuning-options"></a>Opções de otimização automática

Opções de otimização automática disponíveis no banco de dados do Azure SQL são:
 1. **Criar índice** -identifica os índices que podem melhorar o desempenho da carga de trabalho, cria índices e verifica automaticamente que melhorou o desempenho de consultas.
 2. **DROP INDEX** -identifica os índices de redundantes e duplicados e os que não foram utilizados por um muito longo período de tempo. Tenha em atenção que esta opção não é compatível com aplicativos utilizar sugestões de índice e de alternância de partição.
 3. **FORÇAR último plano bom** -identifica as consultas SQL com o plano de execução que é mais lento do que o plano de boa anterior e consulta com o último plano bom conhecido em vez do plano regredido.

Base de dados SQL do Azure identifica **CREATE INDEX**, **DROP INDEX**, e **força última boa planear** recomendações que podem otimizar a sua base de dados e mostram-las no portal do Azure. Encontrar mais informações sobre a identificação de índices que devem ser alteradas em [localizar recomendações de índice no portal do Azure](sql-database-advisor-portal.md). Também pode aplicar manualmente recomendações com o portal ou pode permitir que a base de dados do Azure SQL para automaticamente aplicar recomendações, monitorizar a carga de trabalho após a alteração e certifique-se de que a recomendação melhorámos o desempenho da carga de trabalho. 

Opções de otimização automática podem ser ativadas ou desativadas por base de dados independente, ou podem ser configuradas em servidores lógicos e aplicados em cada base de dados que herda as definições do servidor. Servidores lógicos podem herdar as predefinições do Azure para definições de sintonização automática. Predefinições do Azure neste momento estão definidas para FORCE_LAST_GOOD_PLAN está ativada, CREATE_INDEX está ativada e DROP_INDEX está desativada.

Configurar opções num servidor de otimização automática e a herdar as definições para bases de dados que pertencem ao servidor principal é um método recomendado para configurar a otimização automática como ele simplifica a gestão de opções de otimização automática para um grande número de bases de dados.

## <a name="next-steps"></a>Passos Seguintes

- Para ativar a otimização automática na base de dados do Azure SQL para gerir a sua carga de trabalho, consulte [ativar o ajuste automático](sql-database-automatic-tuning-enable.md).
- Analisar manualmente e aplicar recomendações de otimização automática, consulte [localizar e aplicar recomendações de desempenho](sql-database-advisor-portal.md).
- Para saber mais sobre a criação de notificações por e-mail para recomendações de otimização automática, consulte [notificações para a otimização automática por E-Mail](sql-database-automatic-tuning-email-notifications.md)
- Para saber mais sobre como inteligência incorporada utilizada na otimização automática, consulte [Inteligência Artificial ajusta bases de dados do Azure SQL](https://azure.microsoft.com/blog/artificial-intelligence-tunes-azure-sql-databases/).
- Para saber mais sobre o funcionamento de otimização automático como na base de dados do Azure SQL e SQL server 2017, veja [otimização automática do SQL Server](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning).
