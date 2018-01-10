---
title: Perguntas mais frequentes (FAQ) sobre a Azure Search | Microsoft Docs
description: "Obtenha respostas a questões recorrentes sobre o serviço de pesquisa do Microsoft Azure"
services: search
author: HeidiSteen
manager: jhubbard
ms.service: search
ms.technology: search
ms.topic: article
ms.date: 08/03/2017
ms.author: heidist
ms.openlocfilehash: f61fe2930bc70e800e5d79773e0de6827621e845
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2018
---
# <a name="azure-search---frequently-asked-questions-faq"></a>Pesquisa do Azure - perguntas mais frequentes sobre (FAQ)

 Encontrar respostas a perguntas mais comuns sobre conceitos, código e cenários relacionadas com a Azure Search.

## <a name="platform"></a>Plataforma

### <a name="how-is-azure-search-different-from-full-text-search-in-my-dbms"></a>Como é diferente da pesquisa em texto completo na minha DBMS da Azure Search?

A pesquisa do Azure suporta várias origens de dados, [análise linguístico para vários idiomas](https://docs.microsoft.com/rest/api/searchservice/language-support), [analysis personalizado invulgares e interessantes entradas de dados](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search), controlos classificação através de pesquisa [da classificação perfis](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)e funcionalidades de experiência de utilizador como typeahead, realçando acessos e navegação por facetas. Também inclui outras funcionalidades, como sinónimos e a sintaxe de consulta avançada, mas os são geralmente não differentiating funcionalidades.

### <a name="what-is-the-difference-between-azure-search-and-elasticsearch"></a>O que é a diferença entre a Azure Search e Elasticsearch?

Quando a comparação com as tecnologias de pesquisa, os clientes com frequência peça especificações na forma como a pesquisa do Azure compara com Elasticsearch. Os clientes que escolher da Azure Search através de Elasticsearch para a sua pesquisa de projetos de aplicação, normalmente, tal porque fizemos uma tarefa chave mais fácil ou que precisam de integração incorporada com outras tecnologias Microsoft:

+ A pesquisa do Azure é um serviço em nuvem completamente gerido com contratos de nível de serviço (SLA) 99,9% quando aprovisionado com redundância suficiente (2 réplicas para acesso de leitura, 3 réplicas para leitura e escrita).
+ Microsoft [processadores de linguagem Natural](https://docs.microsoft.com/rest/api/searchservice/language-support) oferecem analysis linguístico leading edge.  
+ [Indexadores de pesquisa do Azure](search-indexer-overview.md) pode pesquisam uma variedade de origens de dados do Azure para indexação inicial e incrementais.
+ Se precisar de resposta rápida às flutuações de consulta ou indexação volumes, pode utilizar [controlos de deslize](search-manage.md#scale-up-or-down) no Azure portal ou executar um [script do PowerShell](search-manage-powershell.md), ignorando diretamente a gestão de partições horizontais.  
+ [Classificação e funcionalidades de otimização](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) fornece os meios para que influencia as pontuações classificação, para além de que o motor de busca individualmente pode fornecer de pesquisa.

### <a name="can-i-pause-azure-search-service-and-stop-billing"></a>Pode colocar em pausa o serviço da Azure Search e parar a faturação?

Não é possível colocar em pausa o serviço. Capacidade de cálculo e recursos de armazenamento são atribuídos para utilização exclusiva quando o serviço é criado. Não é possível libertar e recuperar esses recursos a pedido.

## <a name="indexing-operations"></a>Operações de indexação

### <a name="backup-and-restore-or-download-and-move-indexes-or-index-snapshots"></a>Cópia de segurança e restaurem (ou transfira e moverem) índices ou instantâneos de índice?

Embora possa [obter uma definição de índice](https://docs.microsoft.com/rest/api/searchservice/get-index) em qualquer altura, não há nenhum extração de índice, um instantâneo ou um funcionalidade de restauro da cópia de segurança para a transferência um *preenchido* índice em execução na nuvem para um sistema local, ou movê-lo para outro serviço da Azure Search.

Os índices são criados e preenchidos a partir do código que escrever e executar apenas na pesquisa do Azure na nuvem. Normalmente, os clientes que pretendem mover um índice para outro serviço fazê-lo, editando o respectivo código para utilizar um novo ponto final e, em seguida, execute novamente a indexação. Se pretender que a capacidade de criar um instantâneo ou um índice de cópia de segurança, converter um voto [voz do utilizador](https://feedback.azure.com/forums/263029-azure-search/suggestions/8021610-backup-snapshot-of-index).

### <a name="can-i-index-from-sql-database-replicas-applies-to-azure-sql-database-indexershttpsdocsmicrosoftcomazuresearchsearch-howto-connecting-azure-sql-database-to-azure-search-using-indexers"></a>Pode indexar de réplicas de base de dados do SQL Server (aplica-se a [indexadores de base de dados do Azure SQL](https://docs.microsoft.com/azure/search/search-howto-connecting-azure-sql-database-to-azure-search-using-indexers))

Não existem sem restrições sobre a utilização de réplicas primários ou secundários como uma origem de dados ao criar um índice a partir do zero. No entanto, se atualizar um índice com atualizações incrementais (com base nos registos alterados) requer a réplica primária. Este requisito provém de base de dados SQL que garantias de alterações no apenas réplicas primárias. Se tentar utilizar réplicas secundárias para uma carga de trabalho de atualização do índice, não há nenhuma garantia, obter todos os dados.

## <a name="search-operations"></a>Operações de pesquisa

### <a name="can-i-search-across-multiple-indexes"></a>Pode procurar em vários índices?

Não, esta operação não é suportada. Pesquisa sempre tem um âmbito num índice único.

### <a name="can-i-restrict-search-corpus-access-by-user-identity"></a>Pode restringir pesquisa corpus acesso pela identidade de utilizador?

Pode implementar [filtros de segurança](https://docs.microsoft.com/azure/search/search-security-trimming-for-azure-search) com `search.in()` filtro. O filtro composes bem com [Directory(AAD) Active Directory do Azure como o serviços de gestão de identidade](https://docs.microsoft.com/azure/search/search-security-trimming-for-azure-search-with-aad) cortar os resultados da pesquisa com base no definido associação de grupo do utilizador.

### <a name="why-are-there-zero-matches-on-terms-i-know-to-be-valid"></a>Por que motivo existem zero corresponde à medida que poderei sabê-lo para ser válida?

O cenário mais comum é não saber que cada tipo de consulta suporta níveis de estatísticas linguístico e comportamentos de pesquisa diferentes. Pesquisa em texto completo, que é a carga de trabalho predominant, inclui uma fase de análise de idioma de quebras de termos de licenciamento para baixo para formulários de raiz. Este aspeto do consulta análise casts uma rede mais ampla através de correspondências possíveis, porque o termo tokenized corresponde a um maior número de variantes.

Caráter universal, difusa regex consultas, no entanto, não são analisadas como consultas termo ou a expressão regulares e podem provocar se a consulta não corresponde ao formato word no índice de pesquisa analisado em fraca devolução de chamada. Para obter mais informações sobre a análise de consulta e análise, consulte [consultar arquitetura](https://docs.microsoft.com/azure/search/search-lucene-query-architecture).

### <a name="my-wildcard-searches-are-slow"></a>Minhas pesquisas com carateres universais são lentas.

A maioria das consultas de pesquisa com carateres universais, como prefixo, difusa e regex, são foi reescrita internamente com correspondência termos no índice de pesquisa. Este processamento adicional de analisar o índice de pesquisa adiciona a latência. Consulta de pesquisa mais, abrangente, como `a*` por exemplo, que estão provavelmente ser foi reescrita com muitos termos pode ser extremamente lento. Para pesquisas com carateres universais de performant, considere definir um [analisador personalizado](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search).

### <a name="why-is-the-search-rank-a-constant-or-equal-score-of-10-for-every-hit"></a>Por que motivo é a classificação de pesquisa uma classificação igual ou constante de 1.0 para todos os acessos?

Por predefinição, os resultados da pesquisa são classificados com base no [propriedades análises de correspondência de termos](search-lucene-query-architecture.md#stage-4-scoring)e ordenados conjunto alto a baixa nos resultados. No entanto, alguns tipos (universal, prefixo, regex) de consulta contribuir sempre uma pontuação constante para o modelo de pontuação de documento geral. Este comportamento é propositado. A pesquisa do Azure impõe uma pontuação constante para permitir correspondências encontradas através da expansão de consulta sejam incluídos nos resultados, sem afetar a classificação.

Por exemplo, suponha que uma entrada de "apresentação *" numa pesquisa com carateres universais produz correspondências em "tours", "tourettes" e "tourmaline". Tendo em conta a natureza estes resultados, não há nenhuma forma razoável inferir os termos encontram-se mais importantes do que outros. Por este motivo, estamos a ignorar frequências termo quando a classificação de resultados em consultas de caráter universal de tipos, o prefixo e o regex. Os resultados da pesquisa com base numa entrada parcial recebem uma pontuação constante para evitar bias para correspondências potencialmente inesperadas.

## <a name="design-patterns"></a>Padrões de estrutura

### <a name="what-is-the-best-approach-for-implementing-localized-search"></a>O que é a melhor abordagem para implementar a pesquisa localizada?

Maioria dos clientes escolha campos dedicados através de uma coleção quando for necessário para suportar diferentes regiões (idiomas) no mesmo índice. Campos de região específica possibilitam a atribuir um analisador adequado. Por exemplo, atribuir o analisador de francês da Microsoft para um campo que contém as cadeias francês. Também simplifica a filtragem. Se souber que uma consulta é iniciada numa página fr-fr, pode limitar os resultados da pesquisa para este campo. Ou, crie um [perfil de classificação](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) para dar o campo mais peso relativo. Suporta a pesquisa do Azure através de [50 analisadores de idioma](https://docs.microsoft.com/azure/search/search-language-support) à sua escolha.

## <a name="next-steps"></a>Passos Seguintes

É a sua pergunta sobre uma funcionalidade ou funcionalidade em falta? A funcionalidade de pedido no [voz do utilizador web site](https://feedback.azure.com/forums/263029-azure-search).

## <a name="see-also"></a>Consulte também

 [StackOverflow: A pesquisa do Azure](https://stackoverflow.com/questions/tagged/azure-search)   
 [Como completa a pesquisa em texto funciona na Azure Search](search-lucene-query-architecture.md)  
 [O que é o Azure Search?](search-what-is-azure-search.md)
