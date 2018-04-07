---
title: Recomendações de desempenho - SQL Database do Azure | Microsoft Docs
description: Base de dados SQL do Azure fornece recomendações para as bases de dados do SQL Server que podem melhorar o desempenho de consulta atual.
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: article
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: b88b8a313a03ef2ad72aeb053b89505201c23819
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="performance-recommendations-for-sql-database"></a>Recomendações de desempenho para a base de dados SQL

Base de dados SQL do Azure aprende e feita com a sua aplicação. Fornece recomendações personalizadas que permitem-lhe maximizar o desempenho das bases de dados SQL. Base de dados SQL continuamente avalia e analisa o histórico de utilização de bases de dados SQL. As recomendações fornecidas são com base nos padrões de carga de trabalho exclusivo para a base de dados e ajudam a melhorar o desempenho.

> [!TIP]
> [A otimização automática](sql-database-automatic-tuning.md) é o método recomendado para otimização de desempenho. [Insights inteligentes](sql-database-intelligent-insights.md) é o método recomendado para monitorização do desempenho. 
>

## <a name="create-index-recommendations"></a>Criar recomendações de índice
Base de dados SQL continuamente monitoriza as consultas que estejam a executar e identifica os índices que podem melhorar o desempenho. Depois de suficiente confiança que um índice de determinados está em falta, um novo **criar índice** recomendação é criada.

 Base de dados SQL do Azure baseia-se a confiança para estimar os ganhos de desempenho que seria colocar o índice através de tempo. Consoante o ganho de desempenho estimado recomendações são categorizadas como alta, média ou baixa. 

Os índices são criados utilizando recomendações sempre sinalizados como índices criados automaticamente. Pode ver os índices são criados automaticamente ao observar a vista de sys.indexes. Índices criados automática não bloquear comandos de mudança de nome/ALTER. 

Se tentar remover a coluna que tem um índice criado automaticamente sobre os mesmos, transmite o comando. O índice criado automaticamente foi removido com o comando bem. Índices regulares bloquear o comando ALTER/mudança de nome em colunas que são indexados.

Depois de aplicada a recomendação de índice de criar, SQL Database do Azure compara o desempenho das consultas com o desempenho da linha de base. Se o novo índice um desempenho melhorado, a recomendação está assinalada como concluída com êxito e o relatório de impacto está disponível. Se o índice não melhorar o desempenho, foi revertido automaticamente. Base de dados do SQL Server utiliza este processo para garantir que as recomendações melhorar o desempenho de base de dados.

Qualquer **criar índice** recomendação tem uma política de término não permite a aplicar a recomendação se a utilização de recursos de uma base de dados ou agrupamento é elevada. A política de término tem em conta da CPU, dados e/s, registo de e/s e armazenamento disponível. 

Se a CPU, dados e/s ou e/s registo consta superior a 80% os 30 minutos anteriores, as recomendações do índice de criar é adiada. Se o armazenamento disponível for inferior a 10% depois do índice é criado, a recomendação entra no estado de erro. Se, após alguns dias, a otimização automática ainda Deteta que o índice seria vantajoso, o processo inicia-se novamente. 

Este processo repete-se até que seja não existe armazenamento suficiente disponível para criar um índice ou até que o índice não é apresentado como benéfico já.

## <a name="drop-index-recommendations"></a>Remova as recomendações do índice
Para além de detetar os índices em falta, a base de dados SQL continuamente analisa o desempenho dos índices existentes. Se não for utilizado um índice, SQL Database do Azure recomenda a remover. Remover um índice recomenda-se em dois cenários:
* O índice é um duplicado de outro índice (mesmo indexada e incluído coluna, esquema de partição e os filtros).
* O índice ainda não foram utilizado durante um período prolongado (93 dias).

Recomendações de índice de largar também aceder através de verificação após a implementação. Melhora o desempenho, o relatório de impacto estiver disponível. Se degrada o desempenho, a recomendação é revertida.


## <a name="parameterize-queries-recommendations"></a>Parametrizar recomendações de consultas
*Parametrizar consultas* recomendações são apresentados quando tem uma ou mais consultas constantemente estão a ser recompiladas mas final cópias de segurança com o mesmo plano de execução de consulta. Esta condição cria uma oportunidade para aplicar parameterization forçada. Parameterization forçada, por sua vez, permite planos de consulta sejam colocados em cache e reutilizada no futuro, que melhora o desempenho e reduz a utilização de recursos. 

Cada consulta que é emitida inicialmente relativamente do SQL Server tem de ser compilado para gerar um plano de execução. Cada plano gerado é adicionado à cache de planos. As execuções subsequentes da mesma consulta podem reutilizar este plano da cache, o que elimina a necessidade de compilação adicional. 

As consultas com valores não parametrizada podem levar a sobrecarga de desempenho porque o plano de execução é recompilado sempre que os valores não parametrizada são diferentes. Em muitos casos, as consultas mesmas com valores de parâmetro diferentes geram os mesmo planos de execução. No entanto, estes esquemas, ainda separadamente são adicionadas à cache de plano. 

O processo de recompilar planos de execução utiliza recursos de base de dados, aumenta a consulta de tempo de duração e capacidade excedida da cache de planos. Estes eventos, por sua vez, fazer com que os planos sejam expulsos da cache. Este comportamento do SQL Server pode ser alterado definindo a opção de parameterization forçada na base de dados. 

Para ajudar a calcular o impacto desta recomendação, são fornecidos com uma comparação entre a utilização de CPU real e a utilização de CPU prevista (como se a recomendação foram aplicada). Esta recomendação pode ajudar a obter poupanças da CPU. Também o pode ajudar a reduzir a duração de consulta e a sobrecarga da cache do plano, que significa que mais dos planos podem permanecer na cache e ser reutilizadas. Pode aplicar esta recomendação rapidamente, selecionando o **aplicar** comando. 

Depois de aplicar esta recomendação, permite parameterization forçada dentro de minutos na base de dados. Iniciar o processo de monitorização, dura aproximadamente 24 horas. Após este período, pode ver o relatório de validação. Este relatório mostra a utilização da CPU da base de dados 24 horas antes e após a recomendação. Assistente de base de dados do SQL tem um mecanismo de segurança que é automaticamente revertida a recomendação aplicada quando tenha sido detetado regressão de desempenho.

## <a name="fix-schema-issues-recommendations-preview"></a>Corrigir problemas de esquema recomendações (pré-visualização)

> [!IMPORTANT]
> Microsoft está a descontinuar atualmente "Corrigir o problema de esquema" recomendações. Recomendamos que utilize [Insights inteligente](sql-database-intelligent-insights.md) para monitorizar os problemas de desempenho da base de dados, incluindo problemas de esquema que as recomendações "Corrigir o problema de esquema" anteriormente abrangidas.
> 

**Corrigir problemas de esquema** recomendações são apresentados quando o serviço de base de dados SQL avisos uma anomalias no número de erros SQL relacionados com o esquema, que estão a ocorrer na sua base de dados do SQL Server. Esta recomendação, normalmente, é apresentada quando a sua base de dados encontra vários erros relacionados com o esquema (nome de coluna inválido, nome de objeto inválido e assim sucessivamente) dentro de uma hora.

"Problemas de esquema" são uma classe de erros de sintaxe no SQL Server. Podem ocorrerem quando a definição da consulta SQL e a definição do esquema da base de dados não estão alinhados. Por exemplo, uma das colunas que é esperado pela consulta pode estar em falta na tabela de destino ou vice-versa. 

A recomendação "Corrigir o problema de esquema" é apresentada quando o serviço SQL Database do Azure avisos uma anomalias no número de erros SQL relacionados com o esquema, que estão a ocorrer na sua base de dados do SQL Server. A tabela seguinte mostra os erros que estão relacionados com problemas de esquema:

| Código de erro SQL | Mensagem |
| --- | --- |
| 201 |Função ou procedimento "*'espera o parâmetro'*', que não foi fornecido. |
| 207 |Nome de coluna inválido ' *'. |
| 208 |Nome de objeto inválido ' *'. |
| 213 |Nome da coluna ou o número de valores fornecidos não corresponde a definição da tabela. |
| 2812 |Não foi possível encontrar o procedimento armazenado ' *'. |
| 8144 |Função ou procedimento * tem demasiados argumentos especificados. |

## <a name="next-steps"></a>Passos Seguintes
Monitorizar as recomendações e continuar a aplicar-lhes para otimizar o desempenho. Cargas de trabalho de base de dados são dinâmicas e alterar continuamente. SQL Database Advisor continua a monitorizar e fornecer recomendações podem potencialmente melhorar o desempenho da base de dados. 

* Para obter mais informações sobre a otimização automática de índices de base de dados e os planos de execução de consulta, consulte [otimização automática da base de dados do Azure SQL](sql-database-automatic-tuning.md).
* Para obter mais informações sobre como monitorizar o desempenho de base de dados com automatizada de diagnóstico e análise da causa raiz dos problemas de desempenho automaticamente, consulte [Azure SQL inteligente Insights](sql-database-intelligent-insights.md).
*  Para obter mais informações sobre como utilizar as recomendações de desempenho no portal do Azure, consulte [recomendações de desempenho no portal do Azure](sql-database-advisor-portal.md).
* Consulte [Query Performance Insight](sql-database-query-performance.md) para saber mais sobre e ver o impacto do desempenho das consultas superiores.


