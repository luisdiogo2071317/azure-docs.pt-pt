---
title: Otimizar autovacuum na base de dados do Azure para o servidor PostgreSQL
description: Este artigo descreve como pode otimizar autovacuum na base de dados do Azure para o servidor PostgreSQL.
author: dianaputnam
ms.author: dianas
editor: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/22/2018
ms.openlocfilehash: 91f24bf90f3e1a8a0c383a5820c6816748090807
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51629260"
---
# <a name="optimizing-autovacuum-on-azure-database-for-postgresql-server"></a>Otimizando autovacuum na base de dados do Azure para o servidor PostgreSQL 
Este artigo descreve como otimizar efetivamente autovacuum na base de dados do Azure para PostgreSQL.

## <a name="overview-of-autovacuum"></a>Descrição geral do autovacuum
PostgreSQL utiliza MVCC para permitir maior simultaneidade de base de dados. Cada atualização resulta num insert e delete e cada delete resulta em linhas que está sendo de forma recuperável marcado para eliminação. Os resultados de marcação de forma recuperável na identificação de cadeias de identificação Inativas, que serão removidas mais tarde. PostgreSQL alcança tudo isso ao executar uma tarefa de aspirador.

Pode ser acionada uma tarefa de aspirador manual ou automaticamente. Cadeias de identificação de mensagens não mais serão existir quando a base de dados está a ter heavy update ou eliminar operações e menos quando está ocioso.  A necessidade de executar o aspirador com mais frequência é maior quando a base de dados está sob carga pesada; fazendo a execução de trabalhos de aspirador **manualmente** inconveniente.

Autovacuum pode ser configurado e os benefícios da otimização. Experimente os valores predefinidos que acompanha o PostgreSQL garantir que o produto funciona em todos os tipos de dispositivos, incluindo Raspberry Pis e os valores de configuração ideal dependem de vários fatores:
- Total de recursos disponíveis - SKU e tamanho de armazenamento.
- Utilização de recursos.
- Características de objeto individuais.

## <a name="autovacuum-benefits"></a>Benefícios de Autovacuum
Se não executar aspirador de tempos em tempos, as tuplas Inativas que acumularem podem resultar em:
- Dados Inchar - maiores bancos de dados e tabelas.
- Índices maiores inferior ao ideal.
- E/s maior.

## <a name="monitoring-bloat-with-autovacuum-queries"></a>Monitorização inchaço com consultas autovacuum
A seguinte consulta de exemplo foi concebida para identificar o número de tuplas de ativos e inativos numa tabela chamada "XYZ": "SELECIONE relname, n_dead_tup, n_live_tup, (n_dead_tup / n_live_tup) AS DeadTuplesRatio, last_vacuum, last_autovacuum FROM pg_catalog.pg_stat_all_ tabelas onde relname = ordem "XYZ" por n_dead_tup DESC; "

## <a name="autovacuum-configurations"></a>Configurações de Autovacuum
Os parâmetros de configuração que controlam autovacuum giram em torno de duas questões-chave:
- Quando ele é iniciado?
- Quanto ele limpa depois de iniciar?

Abaixo estão alguns dos parâmetros de configuração de autovacuum que pode atualizar com base nas perguntas acima, juntamente com algumas orientações:
Parâmetro|Descrição|Valor predefinido
---|---|---
autovacuum_vacuum_threshold|Especifica o número mínimo de tuplas atualizadas ou eliminadas necessários para acionar um ASPIRADOR de qualquer uma tabela. A predefinição é 50 cadeias de identificação. Este parâmetro só pode ser definido no ficheiro de postgresql.conf ou na linha de comandos de servidor. A definição pode ser substituída para tabelas individuais ao alterar os parâmetros de armazenamento de tabela.|50
autovacuum_vacuum_scale_factor|Especifica uma fração do tamanho da tabela para adicionar a autovacuum_vacuum_threshold ao decidir se quer acionar vazio. A predefinição é 0,2 (20 por cento do tamanho da tabela). Este parâmetro só pode ser definido no ficheiro de postgresql.conf ou na linha de comandos de servidor. A definição pode ser substituída para tabelas individuais ao alterar os parâmetros de armazenamento de tabela.|5 por cento
autovacuum_vacuum_cost_limit|Especifica o valor de limite de custo que será utilizado em operações de ASPIRADOR automática. Se for -1 é especificado (que é o padrão), será utilizado o valor de regular vacuum_cost_limit. O valor é distribuído proporcionalmente entre os operadores de autovacuum em execução, se existir mais do que uma função de trabalho. A soma dos limites para cada função de trabalho não excede o valor da variável. Este parâmetro só pode ser definido no ficheiro de postgresql.conf ou na linha de comandos de servidor. A definição pode ser substituída para tabelas individuais ao alterar os parâmetros de armazenamento de tabela.|-1
autovacuum_vacuum_cost_delay|Especifica o valor de atraso de custo que será utilizado em operações de ASPIRADOR automática. Se não for especificado -1, será utilizado o valor de regular vacuum_cost_delay. O valor predefinido é 20 milissegundos. Este parâmetro só pode ser definido no ficheiro de postgresql.conf ou na linha de comandos de servidor. A definição pode ser substituída para tabelas individuais ao alterar os parâmetros de armazenamento de tabela.|20 ms
autovacuum_nap_time|Especifica que o atraso mínimo entre autovacuum é executado em qualquer determinada base de dados. Em cada rodada, o daemon examina o banco de dados e emite comandos ASPIRADOR e analisar, conforme necessário para tabelas no banco de dados. O atraso é medido em segundos, e a predefinição é (1 min) de um minuto. Este parâmetro só pode ser definido no ficheiro de postgresql.conf ou na linha de comandos de servidor.|15 s
autovacuum_max_workers|Especifica o número máximo de processos de autovacuum (que não seja o Iniciador do autovacuum) que podem ser executadas ao mesmo tempo. A predefinição é três. Este parâmetro só pode ser definido no início do servidor.|3
As configurações acima podem ser atualizadas com o portal do Azure ou a CLI do Azure.

## <a name="autovacuum-cost"></a>Custo de Autovacuum
Seguem-se a "custos" de execução de uma operação de aspirador:
- Um bloqueio é colocado nos dados aspirador de páginas é executado.
- Computação e memória, são utilizados quando aspirador está em execução.

Isso implica que aspirador não deve executar qualquer um com uma frequência excessiva ou com muito pouca frequência, ele precisa ser adaptável para a carga de trabalho. Recomendamos que teste a todas as alterações de parâmetro autovacuum devido às compensações de cada um deles.

## <a name="autovacuum-start-trigger"></a>Acionador de início de Autovacuum
Autovacuum é acionado quando o número de tuplas Inativos exceder autovacuum_vacuum_threshold + autovacuum_vacuum_scale_factor * reltuples, reltuples aqui é uma constante.

Limpeza de autovacuum precisa manter-se a carga de base de dados, caso contrário, poderia ficar sem armazenamento e lentidão geral nas consultas. Amortizado ao longo do tempo, a taxa na qual aspirador limpa morto cadeias de identificação devem ser igual a taxa a que as tuplas de mensagens não são criadas.

Bases de dados com muitas atualizações/exclusões têm mais tuplas Inativas e precisam de mais espaço. Em geral, bases de dados com muitas atualizações/exclusões se beneficiam de valores de baixas de autovacuum_vacuum_scale_factor e baixa de autovacuum_vacuum_threshold para impedir que prolongado acumulação de cadeias de identificação Inativas. Poderia usar valores mais altos para ambos os parâmetros com bancos de dados mais pequenos, como a necessidade de aspirador é menos urgente. Lembrete que vacuuming frequentes incluem-se ao custo de computação e memória.

O fator de dimensionamento predefinido de 20 por cento funciona bem em tabelas com uma baixa percentagem de cadeias de identificação mortas, mas não em tabelas com uma percentagem elevada de tuplas Inativas. Por exemplo, numa tabela de 20 GB, que isto traduz-se a 4 GB de tuplas Inativas e numa tabela de 1 TB é 200 GB de cadeias de identificação Inativas.

Com o PostgreSQL, pode definir estes parâmetros no nível de instância ou nível de tabela. Hoje em dia, esses parâmetros podem ser definidos no nível da tabela apenas na base de dados do Azure para PostgreSQL.

## <a name="estimating-the-cost-of-autovacuum"></a>Prever o custo de autovacuum
Autovacuum em execução é andS "dispendiosa" há parâmetros para controlar o tempo de execução de operações de aspirador. Os seguintes parâmetros de ajudar a estimar o custo da execução aspirador:
- vacuum_cost_page_hit = 1
- vacuum_cost_page_miss = 10
- vacuum_cost_page_dirty = 20

O processo de aspirador lê páginas físicas e verifica a existência de tuplas Inativas. Todas as páginas no shared_buffers é considerada como têm um custo de 1 (vacuum_cost_page_hit), todas as outras páginas são consideradas como têm um custo de 20 (vacuum_cost_page_dirty) se existem tuplas inativas ou 10 (vacuum_cost_page_miss) se não existem cadeias de identificação de mensagens não existem. A operação de aspirador termina quando o processo excede autovacuum_vacuum_cost_limit.  

Após ter sido atingido o limite, o processo permanecerá suspenso durante a duração especificada pelo parâmetro autovacuum_vacuum_cost_delay antes de a ser iniciado novamente. Se não for atingido o limite, autovacuum começa depois do valor especificado pelo parâmetro autovacuum_nap_time.

Em resumo, os parâmetros autovacuum_vacuum_cost_delay e autovacuum_vacuum_cost_limit controlam quanto limpeza de dados é permitida por hora de unidade. Tenha em atenção que os valores predefinidos são demasiado baixos para a maioria dos escalões de preço. Os valores ideal para esses parâmetros são dependentes de escalão de preços e devem ser configurados em conformidade.

O parâmetro autovacuum_max_workers determina o número máximo de processos de autovacuum que podem ser executados simultaneamente.

Com o PostgreSQL, pode definir estes parâmetros no nível de instância ou nível de tabela. Hoje em dia, esses parâmetros podem ser definidos no nível da tabela apenas na base de dados do Azure para PostgreSQL.

## <a name="optimizing-autovacuum-per-table"></a>Otimizando autovacuum por tabela
Todos os parâmetros de configuração acima podem ser configurados por tabela, por exemplo:
```sql
ALTER TABLE t SET (autovacuum_vacuum_threshold = 1000);
ALTER TABLE t SET (autovacuum_vacuum_scale_factor = 0.1);
ALTER TABLE t SET (autovacuum_vacuum_cost_limit = 1000);
ALTER TABLE t SET (autovacuum_vacuum_cost_delay = 10);
```

Autovacuum é um por processo síncrono de tabela. A maior percentagem de uma tabela de cadeias de identificação morta tem, maior o "Custo" para autovacuum.  A divisão de tabelas que tenham uma alta taxa de atualizações/exclusões em várias tabelas irá ajudar a colocar em paralelo autovacuum e reduzir o "Custo" para concluir autovacuum numa tabela. Também pode aumentar o número de funções de trabalho de paralelo autovacuum para garantir que os operadores livremente estão agendados.

## <a name="next-steps"></a>Passos Seguintes
Reveja o seguinte documenatation PostgreSQL para obter mais informações sobre o uso e ajuste autovacuum:
 - Documentação do PostgreSQL - [capítulo 18, a configuração do servidor](https://www.postgresql.org/docs/9.5/static/runtime-config-autovacuum.html)
 - Documentação do PostgreSQL – [Capítulo 24, tarefas de manutenção de rotina da base de dados](https://www.postgresql.org/docs/9.6/static/routine-vacuuming.html)
