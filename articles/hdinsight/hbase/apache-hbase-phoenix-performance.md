---
title: Desempenho de Phoenix no Azure HDInsight | Microsoft Docs
description: Melhores práticas para otimizar o desempenho de Phoenix.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: ashishth
ms.openlocfilehash: b4c1e3fb919ab9ad88a15b51a5e204290a7a12cf
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
ms.locfileid: "34164640"
---
# <a name="phoenix-performance-best-practices"></a>Melhores práticas de desempenho do Phoenix

É o aspeto do desempenho de Phoenix mais importante para otimizar o HBase subjacente. O Phoenix cria um modelo de dados relacional visível HBase que converte as consultas SQL em operações de HBase, tais como análises. A estrutura do seu esquema de tabela, a seleção e ordenação dos campos existentes na sua chave primária e a utilização de todos os índices afetar o desempenho de Phoenix.

## <a name="table-schema-design"></a>Design do esquema de tabela

Quando cria uma tabela no Phoenix, essa tabela é armazenada numa tabela de HBase. A tabela HBase contém os grupos de colunas (famílias de coluna) que são acedidos em conjunto. Uma linha na tabela Phoenix é uma linha na tabela HBase, onde cada linha é composta por células com a versão associadas uma ou mais colunas. Logicamente, uma única linha de HBase é uma coleção de pares chave-valor, cada um com o mesmo valor de rowkey. Ou seja, cada par chave-valor, tem um atributo de rowkey e o valor do atributo que rowkey é o mesmo para numa linha específica.

O design do esquema de uma tabela de Phoenix inclui as principais a chave de estrutura, design famílias de coluna, design de coluna individuais, e como os dados estão particionados.

### <a name="primary-key-design"></a>Estrutura de chave primária

A chave primária definida numa tabela na Phoenix determina a forma como os dados são armazenados no rowkey da tabela HBase subjacente. No HBase, a única forma de aceder numa linha específica é com o rowkey. Além disso, os dados armazenados na tabela de HBase são ordenados pelo rowkey. O Phoenix baseia-se o valor de rowkey para concatenar os valores de cada uma das colunas da linha, pela ordem que estão definidos na chave primária.

Por exemplo, uma tabela de contactos tem o nome próprio, nome de último, número de telefone e endereço, todos na mesma família de coluna. Pode definir uma chave primária com base num crescente número de sequência:

|rowkey|       Endereço|   telefone| firstName| Apelido|
|------|--------------------|--------------|-------------|--------------|
|  1000|1111 San Gabriel Dr.|1-425-000-0002|    João|Dinis|
|  8396|5415 San Gabriel Dr.|1-230-555-0191|  Calvin|Raji|

No entanto, se a consulta frequentemente por lastName esta chave primária pode não efetuar, porque cada consulta requer uma análise completa de tabela para ler o valor de cada lastName. Em vez disso, pode definir uma chave primária no número de segurança social colunas, firstName e lastName. Esta última coluna é para eliminar a ambiguidade residentes duas na no mesmo endereço com o mesmo nome, tal como um pai e filho.

|rowkey|       Endereço|   telefone| firstName| Apelido| socialSecurityNum |
|------|--------------------|--------------|-------------|--------------| ---|
|  1000|1111 San Gabriel Dr.|1-425-000-0002|    João|Dinis| 111 |
|  8396|5415 San Gabriel Dr.|1-230-555-0191|  Calvin|Raji| 222 |

Chaves geradas pelo Phoenix seria com esta nova a linha de chave primária:

|rowkey|       Endereço|   telefone| firstName| Apelido| socialSecurityNum |
|------|--------------------|--------------|-------------|--------------| ---|
|  Dole-John-111|1111 San Gabriel Dr.|1-425-000-0002|    João|Dinis| 111 |
|  Raji-Calvin-222|5415 San Gabriel Dr.|1-230-555-0191|  Calvin|Raji| 222 |

Na primeira linha acima, os dados para o rowkey são representados conforme mostrado:

|rowkey|       key|   valor| 
|------|--------------------|---|
|  Dole-John-111|Endereço |1111 San Gabriel Dr.|  
|  Dole-John-111|telefone |1-425-000-0002|  
|  Dole-John-111|firstName |João|  
|  Dole-John-111|Apelido |Dinis|  
|  Dole-John-111|socialSecurityNum |111| 

Este rowkey agora armazena uma cópia dos dados duplicada. Considere o tamanho e número de colunas que incluir na sua chave primária, porque este valor está incluído com cada célula da tabela de HBase subjacente.

Além disso, se a chave primária tem valores de que forma monótona estão aumentar, deve criar a tabela com *salt registos* para ajudar a evitar a criação de hotspots de escrita - consulte [dados de partição](#partition-data).

### <a name="column-family-design"></a>Estrutura de família de coluna

Se algumas colunas são acedidas com mais frequência do que outros, deve criar vários famílias de coluna para separar as colunas acedidas com frequência das colunas raramente acedidas.

Além disso, se algumas colunas tendem a ser acedido em conjunto, colocar essas colunas na mesma família de coluna.

### <a name="column-design"></a>Estrutura de coluna

* Manter colunas VARCHAR em cerca de 1 MB devido a custos de e/s de colunas de grandes dimensões. Durante o processamento de consultas, HBase materializes células na completo antes de lhes enviar através do cliente e o cliente recebe-los numa completo antes de manipulação-los a desativar o código de aplicação.
* Armazenar utilizando um formato compacto, como protobuf, Avro, msgpack ou BSON de valores de coluna. Não é recomendado JSON, conforme é maior.
* Considere a compressão de dados antes de armazenamento ao cortar latência e os custos de e/s.

### <a name="partition-data"></a>Dados de partição

O Phoenix permite-lhe controlar o número de regiões onde são distribuídos os seus dados, que pode aumentar significativamente o desempenho de leitura/escrita. Ao criar uma tabela de Phoenix, pode salt ou previamente dividir os dados.

Para salt uma tabela durante a criação, especifique o número de registos de salt:

    CREATE TABLE CONTACTS (...) SALT_BUCKETS = 16

Este salting divide a tabela ao longo de valores de chaves primárias, escolher os valores automaticamente. 

Para controlar qual ocorrem as divisões de tabela, previamente pode dividir a tabela, fornecendo os valores de intervalo no qual a divisão ocorre. Por exemplo criar uma tabela de divisão ao longo de três regiões:

    CREATE TABLE CONTACTS (...) SPLIT ON ('CS','EU','NA')

## <a name="index-design"></a>Estrutura de índice

Um índice Phoenix é uma tabela de HBase que armazena uma cópia de alguns ou todos os dados da tabela indexada. Um índice melhora o desempenho para tipos específicos de consultas.

Quando tiver vários índices definidos e, em seguida, consultar uma tabela, o Phoenix seleciona automaticamente o índice melhor para a consulta. O índice principal é criado automaticamente com base nas chaves primárias que selecionar.

Para consultas previstas, também pode criar índices secundários especificando as respetivas colunas.

Ao conceber a sua índices:

* Só crie os índices que precisa.
* Limite o número de índices em tabelas com frequência atualizados. Atualizações para uma tabela implica escritas para a tabela principal e as tabelas de índice.

## <a name="create-secondary-indexes"></a>Criar índices secundários

Índices secundários podem melhorar o desempenho de leitura ao ativar o que poderia ser uma análise da tabela completa para uma pesquisa de ponto, ao custo de espaço de armazenamento e velocidade de escrita. Índices secundários podem ser adicionados ou removidos após a criação da tabela e não necessitam de alterações para as consultas existentes – consultas apenas são executadas mais rápido. Consoante as suas necessidades, considere a criação de índices cobertos, índices funcionais ou ambos.

### <a name="use-covered-indexes"></a>Utilizar cobertos índices

Índices cobertos são índices que incluem dados a partir da linha para além dos valores que são indexados. Depois de encontrar a entrada de índice pretendido, não é necessário para aceder à tabela primária.

Por exemplo, no exemplo contacte pode criar um índice secundário apenas a coluna de socialSecurityNum de tabela. Este índice secundário seria acelerar consultas Filtrar por valores socialSecurityNum, mas a obter outros valores de campo será necessária outra ler contra a tabela principal.

|rowkey|       Endereço|   telefone| firstName| Apelido| socialSecurityNum |
|------|--------------------|--------------|-------------|--------------| ---|
|  Dole-John-111|1111 San Gabriel Dr.|1-425-000-0002|    João|Dinis| 111 |
|  Raji-Calvin-222|5415 San Gabriel Dr.|1-230-555-0191|  Calvin|Raji| 222 |

No entanto, se pretender procurar os firstName e lastName fornecido a socialSecurityNum normalmente, pode criar um índice coberto que inclui o nome próprio e apelido como dados real na tabela de índice:

    CREATE INDEX ssn_idx ON CONTACTS (socialSecurityNum) INCLUDE(firstName, lastName);

Este índice coberta permite que a consulta seguinte para adquirir todos os dados apenas por ler a tabela que contém o índice secundário:

    SELECT socialSecurityNum, firstName, lastName FROM CONTACTS WHERE socialSecurityNum > 100;

### <a name="use-functional-indexes"></a>Utilizar índices funcionais

Índices funcionais permitem-lhe criar um índice numa expressão arbitrária que pretende utilizar nas consultas. Depois de ter um índice funcional no local e uma consulta utiliza dessa expressão, o índice pode ser utilizado para obter os resultados em vez da tabela de dados.

Por exemplo, pode criar um índice para que possa efetuar pesquisas sensível no combinado nome próprio e apelido de uma pessoa:

     CREATE INDEX FULLNAME_UPPER_IDX ON "Contacts" (UPPER("firstName"||' '||"lastName"));

## <a name="query-design"></a>Estrutura da consulta

As principais considerações na estrutura da consulta são:

* Compreender o plano de consulta e verifique se o comportamento esperado.
* Associação de forma eficiente.

### <a name="understand-the-query-plan"></a>Compreender o plano de consulta

No [SQLLine](http://sqlline.sourceforge.net/), utilize EXPLICATIVO seguido a consulta SQL para ver o plano de operações que Phoenix irá executar. Verifique se o plano de:

* Utiliza a chave primária, quando adequado.
* Utiliza as adequadas índices secundários, em vez da tabela de dados.
* Utiliza o intervalo de análise ou ignorar análise sempre que possível, em vez de análise da tabela.

#### <a name="plan-examples"></a>Plano de exemplos

Por exemplo, imaginemos que tem uma tabela chamada FLIGHTS que armazena informações de atraso do voo.

Para selecionar todos os flights com um airlineid de `19805`, onde airlineid é um campo que não na chave primária ou em qualquer índice:

    select * from "FLIGHTS" where airlineid = '19805';

Execute o comando de explicativo da seguinte forma:

    explain select * from "FLIGHTS" where airlineid = '19805';

O plano de consulta tem o seguinte aspeto:

    CLIENT 1-CHUNK PARALLEL 1-WAY ROUND ROBIN FULL SCAN OVER FLIGHTS
        SERVER FILTER BY AIRLINEID = '19805'

Neste plano, tenha em atenção o frase análise completa sobre o FLIGHTS. Esta frase indica que a execução uma análise da tabela ao longo de todas as linhas na tabela, em vez de através da opção de análise de intervalo ou ignorar análise mais eficiente.

Agora, a indicar que pretende consultar flights em 2 de Janeiro de 2014 para a operadora `AA` onde o respetivo flightnum era maior que 1. Vamos partem do princípio de que as colunas ano, mês, dayofmonth, operadora e flightnum existem na tabela de exemplo e fazem parte de todos os da chave primária composta. A consulta deverá ter o seguinte aspeto:

    select * from "FLIGHTS" where year = 2014 and month = 1 and dayofmonth = 2 and carrier = 'AA' and flightnum > 1;

Vamos examinar o plano para esta consulta com:

    explain select * from "FLIGHTS" where year = 2014 and month = 1 and dayofmonth = 2 and carrier = 'AA' and flightnum > 1;

O plano resultante é:

    CLIENT 1-CHUNK PARALLEL 1-WAY ROUND ROBIN RANGE SCAN OVER FLIGHTS [2014,1,2,'AA',2] - [2014,1,2,'AA',*]

Os valores entre parênteses Retos são o intervalo de valores para as chaves primárias. Neste caso, os valores de intervalo corrigidos com 2014 do ano, mês, 1 e dayofmonth 2, mas permitir valores flightnum iniciar com 2 e no (`*`). Este plano de consulta confirma que a chave primária está a ser utilizada como esperado.

Em seguida, crie um índice na tabela FLIGHTS denominada `carrier2_idx` que esteja num apenas o campo da operadora. Este índice também inclui flightdate, tailnum, origem e flightnum como colunas cobertas cujos dados também são armazenados no índice.

    CREATE INDEX carrier2_idx ON FLIGHTS (carrier) INCLUDE(FLIGHTDATE,TAILNUM,ORIGIN,FLIGHTNUM);

Diga-se de que pretende obter operadora juntamente com as flightdate e tailnum, como a seguinte consulta:

    explain select carrier,flightdate,tailnum from "FLIGHTS" where carrier = 'AA';

Deverá ver este índice que está a ser utilizado:

    CLIENT 1-CHUNK PARALLEL 1-WAY ROUND ROBIN RANGE SCAN OVER CARRIER2_IDX ['AA']

Para uma lista completa dos itens que pode ser apresentado no explicam o plano de resultados, consulte a secção explicam planos de [Apache Phoenix otimização guia](https://phoenix.apache.org/tuning_guide.html).

### <a name="join-efficiently"></a>Associação de forma eficiente

Geralmente, deve evitar associações, a menos que lado pequeno, especialmente em consultas frequentes.

Se necessário, pode fazê-lo associações grande com o `/*+ USE_SORT_MERGE_JOIN */` sugestão, mas uma associação grande é uma operação dispendiosa através de grandes quantidades de linhas. Se o tamanho geral de todas as tabelas de right hand lado iria exceder a memória disponível, utilize o `/*+ NO_STAR_JOIN */` sugestão.

## <a name="scenarios"></a>Cenários

As diretrizes seguintes descrevem algumas padrões comuns.

### <a name="read-heavy-workloads"></a>Cargas de trabalho pesado de leitura

Para ler pesado casos de utilização, certifique-se de que está a utilizar os índices. Além disso, para guardar leitura tempo sobrecarga, considere a criação de índices cobertos.

### <a name="write-heavy-workloads"></a>Cargas de trabalho pesado de escrita

Para cargas de trabalho pesado de escrita em que a chave primária forma monótona está a aumentar, crie salt registos para ajudar a evitar hotspots de escrita, em detrimento de débito de leitura global devido aos análises adicionais necessários. Além disso, quando utilizar UPSERT para escrever um grande número de registos, desative Committed e batch cópias de segurança de registos.

### <a name="bulk-deletes"></a>Elimina em massa

Quando eliminar um grande conjunto de dados, ative Committed antes de emitir a consulta de eliminação, para que o cliente não precisa de se lembrar as chaves de linha para todas as linhas eliminadas. Committed impede que o cliente de colocação em memória intermédia as linhas afetadas por eliminar, para que essa Phoenix pode eliminá-los diretamente nos servidores de região sem a despesa dos devolvê-las para o cliente.

### <a name="immutable-and-append-only"></a>Imutável e só de acréscimo

Se o seu cenário favorece velocidade de escrita ao longo da integridade dos dados, considere desativar o registo de escrita antecipada ao criar as tabelas:

    CREATE TABLE CONTACTS (...) DISABLE_WAL=true;

Para obter mais informações sobre esta e outras opções, consulte [Phoenix gramática](http://phoenix.apache.org/language/index.html#options).

## <a name="next-steps"></a>Passos Seguintes

* [Guia de otimização Phoenix](https://phoenix.apache.org/tuning_guide.html)
* [Índices secundários](http://phoenix.apache.org/secondary_indexing.html)
