---
title: Utilizar extensões do PostgreSQL na base de dados do Azure para PostgreSQL
description: Descreve a capacidade de estender a funcionalidade da sua base de dados com extensões na base de dados do Azure para PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 11/12/2018
ms.openlocfilehash: 43db0e66c05b6b5c3331d3fba3e4db363d61c9f8
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624357"
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql"></a>Extensões do PostgreSQL na base de dados do Azure para PostgreSQL
PostgreSQL fornece a capacidade de estender a funcionalidade da sua base de dados a utilizar extensões. As extensões permitem para agrupar vários objetos relacionados do SQL em conjunto num único pacote que pode ser carregado ou removido da sua base de dados com um único comando. Após a ser carregado no banco de dados, as extensões podem funcionar tal como as funcionalidades incorporadas. Para obter mais informações sobre as extensões do PostgreSQL, consulte [empacotamento objetos relacionados numa extensão](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).

## <a name="how-to-use-postgresql-extensions"></a>Como utilizar as extensões do PostgreSQL
Extensões do PostgreSQL tem de estar instaladas na base de dados antes de poder utilizá-los. Para instalar uma extensão específica, execute o [criar extensão](https://www.postgresql.org/docs/9.6/static/sql-createextension.html) comando da ferramenta de psql para carregar os objetos em pacote na sua base de dados.

Base de dados do Azure para PostgreSQL atualmente suporta um subconjunto das extensões de chave conforme indicado abaixo. Além dos indicados não são suportadas; Não é possível criar sua própria extensão com base de dados do Azure para o serviço PostgreSQL.

## <a name="extensions-supported-by-azure-database-for-postgresql"></a>Extensões suportadas pela base de dados do Azure para PostgreSQL
As tabelas seguintes listam as extensões de PostgreSQL padrão que são atualmente suportadas pela base de dados do Azure para PostgreSQL. Estas informações também estão disponíveis através da execução `SELECT * FROM pg_available_extensions;`.

### <a name="data-types-extensions"></a>Extensões de tipos de dados

> [!div class="mx-tableFixed"]
| **Extensão** | **Descrição** |
|---|---|
| [chkpass](https://www.postgresql.org/docs/9.6/static/chkpass.html) | Fornece um tipo de dados para a encriptação automática de palavras-passe. |
| [citext](https://www.postgresql.org/docs/9.6/static/citext.html) | Fornece um tipo de cadeia de caracteres de maiúsculas e minúsculas. |
| [cube](https://www.postgresql.org/docs/9.6/static/cube.html) | Fornece um tipo de dados de cubos multidimensionais. |
| [hstore](https://www.postgresql.org/docs/9.6/static/hstore.html) | Fornece um tipo de dados para armazenamento de conjuntos de pares chave/valor. |
| [isn](https://www.postgresql.org/docs/9.6/static/isn.html) | Fornece tipos de dados para padrões de numeração de produto internacional. |
| [ltree](https://www.postgresql.org/docs/9.6/static/ltree.html) | Fornece um tipo de dados para estruturas de árvore hierárquicas. |

### <a name="functions-extensions"></a>Extensões de funções

> [!div class="mx-tableFixed"]
| **Extensão** | **Descrição** |
|---|---|
| [earthdistance](https://www.postgresql.org/docs/9.6/static/earthdistance.html) | Fornece um meio para calcular distâncias grande ciclo na superfície da terra. |
| [fuzzystrmatch](https://www.postgresql.org/docs/9.6/static/fuzzystrmatch.html) | Fornece várias funções para determinar as semelhanças e a distância entre cadeias de caracteres. |
| [intarray](https://www.postgresql.org/docs/9.6/static/intarray.html) | Fornece funções e operadores para manipular matrizes null livres de números inteiros. |
| [pgcrypto](https://www.postgresql.org/docs/9.6/static/pgcrypto.html) | Fornece funções criptográficas. |
| [pg\_partman](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | Gere as tabelas particionadas ao tempo ou ID. |
| [pg\_trgm](https://www.postgresql.org/docs/9.6/static/pgtrgm.html) | Fornece funções e operadores para determinar a semelhança de alfanumérico texto com base na correspondência de trigram. |
| [tablefunc](https://www.postgresql.org/docs/9.6/static/tablefunc.html) | Fornece funções que manipulam tabelas inteiras, incluindo tabela cruzada. |
| [uuid-ossp](https://www.postgresql.org/docs/9.6/static/uuid-ossp.html) | Gera identificadores exclusivos universalmente (UUIDs não). |

### <a name="full-text-search-extensions"></a>Extensões de pesquisa em texto completo

> [!div class="mx-tableFixed"]
| **Extensão** | **Descrição** |
|---|---|
| [dict\_int](https://www.postgresql.org/docs/9.6/static/dict-int.html) | Fornece um modelo de dicionário de pesquisa de texto para números inteiros. |
| [unaccent](https://www.postgresql.org/docs/9.6/static/unaccent.html) | Um dicionário de pesquisa de texto que remove os acentos (sinais diacríticos) de lexemes. |

### <a name="index-types-extensions"></a>Extensões de tipos de índice

> [!div class="mx-tableFixed"]
| **Extensão** | **Descrição** |
|---|---|
| [btree\_gin](https://www.postgresql.org/docs/9.6/static/btree-gin.html) | Fornece exemplo GIN operador classes que implementam a árvore B, como o comportamento para determinados tipos de dados. |
| [btree\_gist](https://www.postgresql.org/docs/9.6/static/btree-gist.html) | Fornece classes de operador de índice essência que implementam a árvore B. |

### <a name="language-extensions"></a>Extensões de linguagem

> [!div class="mx-tableFixed"]
| **Extensão** | **Descrição** |
|---|---|
| [plpgsql](https://www.postgresql.org/docs/9.6/static/plpgsql.html) | PL/pgSQL carregável linguagem de procedimento. |
| [plv8](https://plv8.github.io/) | Uma extensão de linguagem do Javascript para o PostgreSQL que pode ser utilizada para procedimentos armazenados, acionadores, etc. |

### <a name="miscellaneous-extensions"></a>Diversas extensões

> [!div class="mx-tableFixed"]
| **Extensão** | **Descrição** |
|---|---|
| [pg\_buffercache](https://www.postgresql.org/docs/9.6/static/pgbuffercache.html) | Fornece um meio para examinar o que acontece no cache de buffer compartilhado em tempo real. |
| [pg\_prewarm](https://www.postgresql.org/docs/9.6/static/pgprewarm.html) | Fornece uma forma de carregar dados de relação para o cache de buffer. |
| [pg\_stat\_statements](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) | Fornece um meio para controlar as estatísticas de execução de todas as instruções SQL executadas por um servidor. (Veja abaixo para uma observação sobre esta extensão). |
| [pgrowlocks](https://www.postgresql.org/docs/9.6/static/pgrowlocks.html) | Fornece um meio para mostrar informações de bloqueios ao nível da linha. |
| [pgstattuple](https://www.postgresql.org/docs/9.6/static/pgstattuple.html) | Fornece um meio para que mostra estatísticas de nível de cadeias de identificação. |
| [postgres\_fdw](https://www.postgresql.org/docs/9.6/static/postgres-fdw.html) | Wrapper de dados externo utilizado para aceder aos dados armazenados em servidores de PostgreSQL externos. |
| [hypopg](https://hypopg.readthedocs.io/en/latest/) | Fornece um meio de criação de índices hipotéticos que não de custos da CPU ou disco. |


### <a name="postgis-extensions"></a>Extensões de PostGIS

> [!div class="mx-tableFixed"]
| **Extensão** | **Descrição** |
|---|---|
| [PostGIS](http://www.postgis.net/), postgis\_topologia, postgis\_tiger\_geocoder, postgis\_sfcgal | Objetos espaciais e geográficos para PostgreSQL. |
| endereço\_standardizer, endereço\_standardizer\_dados\_-nos | Utilizado para analisar um endereço em elementos que constituem. Utilizados para suportar o passo de normalização de endereço de geocodificação. |
| [pgrouting](https://pgrouting.org/) | Estende os PostGIS / funcionalidade de encaminhamento de base de dados do PostgreSQL geoespacial para fornecer dados geoespaciais. |


### <a name="using-pgstatstatements"></a>Usando pg_stat_statements
O [pg\_stat\_extensão instruções](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) é pré-carregada em cada base de dados do Azure para o servidor PostgreSQL para lhe fornecer um meio de controlar as estatísticas de execução de instruções SQL.
A definição `pg_stat_statements.track`, que controla quais declarações são contabilizadas por extensão, a predefinição é `top`, que significa que todas as declarações emitidas diretamente por clientes são controladas. Os dois outros controle se níveis são `none` e `all`. Esta definição pode ser configurada como um parâmetro de servidor através da [portal do Azure](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-portal) ou o [CLI do Azure](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-cli).

Há uma compensação entre as informações de execução de consulta fornece pg_stat_statements e o impacto no desempenho do servidor como registos de cada instrução de SQL. Se não estiver ativamente a utilizar a extensão de pg_stat_statements, recomendamos que defina `pg_stat_statements.track` para `none`. Tenha em atenção que alguns terceiros monitorizar os serviços podem contar com pg_stat_statements para fornecer informações de desempenho de consulta, por isso, confirme se for este o caso para ou não.


## <a name="next-steps"></a>Passos Seguintes
Se não vir uma extensão que gostaria de utilizar, informe-nos. Vote pedidos existentes ou criar novos comentários e pedidos no nosso [fórum de comentários do cliente](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).
