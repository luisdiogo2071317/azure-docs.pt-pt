---
title: Exemplos de consulta simples para o Azure Search | Documentos da Microsoft
description: Exemplos de consulta simples para pesquisa em texto completo, pesquisa de filtro, pesquisa geográfica, pesquisa por facetas e outras cadeias de caracteres de consulta, usadas para consultar um índice da Azure Search.
author: HeidiSteen
manager: cgronlun
tags: Simple query analyzer syntax
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: heidist
ms.openlocfilehash: e4bb72eb8ad6f15b0e5bc14e0e07556e76d0477b
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/31/2018
ms.locfileid: "39369122"
---
# <a name="simple-syntax-query-examples-for-building-queries-in-azure-search"></a>Exemplos de consulta de sintaxe simples para a criação de consultas na Azure Search

[Sintaxe de consulta simples](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) invoca o parser de consulta padrão para a execução de consultas de pesquisa de texto completo em relação a um índice da Azure Search. O analisador de consultas simples é rápido e lida com cenários comuns no Azure Search, incluindo a pesquisa em texto completo e pesquisa geográfica pesquisa por facetas e filtrada. Neste artigo, etapa por meio de exemplos que demonstram as operações de consulta disponíveis quando utiliza a sintaxe simple.

A sintaxe de consulta alternativo é [total Lucene](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), que suporta estruturas de consulta mais complexas, tais como difusa e pesquisa com carateres universais, o que pode demorar mais tempo a processar. Para obter mais informações e exemplos que demonstram a sintaxe completa, consulte [exemplos de consulta de sintaxe Lucene](search-query-lucene-examples.md).

## <a name="formulate-requests-in-postman"></a>Formular pedidos no Postman

Os exemplos seguintes tirar partido de um índice de pesquisa de empregos em nova IORQUE consistindo em tarefas disponíveis com base num conjunto de dados fornecido pelos [cidade de Nova Iorque OpenData](https://nycopendata.socrata.com/) iniciativa. Estes dados não devem ser considerados atual ou completa. O índice é um serviço de proteção de segurança fornecidas pela Microsoft, o que significa que não é necessário uma subscrição do Azure ou o Azure Search para experimentar estas consultas.

O que é necessário é o Postman ou uma ferramenta equivalente para emitir o pedido HTTP na GET. Para obter mais informações, consulte [teste com clientes REST](search-fiddler.md).

### <a name="set-the-request-header"></a>Definir o cabeçalho de pedido

1. No cabeçalho do pedido, defina **Content-Type** para `application/json`.

2. Adicionar uma **chave de api**e defina-o como esta cadeia: `252044BE3886FE4A8E3BAA4F595114BB`. Esta é uma chave de consulta para o serviço de pesquisa de área de segurança que aloja o índice de empregos em nova IORQUE.

Depois de especificar o cabeçalho do pedido, pode reutilizá-lo para todas as consultas neste artigo, alternando apenas a **pesquisa =** cadeia de caracteres. 

  ![Cabeçalho de pedido do Postman](media/search-query-lucene-examples/postman-header.png)

### <a name="set-the-request-url"></a>Definir o URL do pedido

O pedido é um comando GET emparelhado com um URL que contém a cadeia de pesquisa e ponto final do Azure Search.

  ![Cabeçalho de pedido do Postman](media/search-query-lucene-examples/postman-basic-url-request-elements.png)

A composição do URL tem os seguintes elementos:

+ **`https://azs-playground.search.windows.net/`** um serviço de pesquisa de área de segurança é mantido pela equipe de desenvolvimento do Azure Search. 
+ **`indexes/nycjobs/`** é o índice de empregos em nova IORQUE da coleção de índices desse serviço. O nome do serviço e o índice são necessários no pedido.
+ **`docs`** é a coleção de documentos que todo o conteúdo pesquisável. A consulta a chave de api fornecida no cabeçalho do pedido só funciona em operações de leitura visando a coleção de documentos.
+ **`api-version=2017-11-11`** Define a api-version, que é um parâmetro necessário em cada solicitação.
+ **`search=*`** é a cadeia de consulta, que, na consulta inicial, é nulo, retornando os resultados de 50 primeiras (por predefinição).

## <a name="send-your-first-query"></a>Enviar a sua primeira consulta

Como passo de verificação, cole o seguinte pedido GET e clique em **enviar**. Os resultados são retornados como verbosos de documentos JSON. Pode copiar-colar este URL no primeiro exemplo a seguir.

  ```http
  https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&search=*
  ```

A cadeia de consulta **`search=*`**, equivale uma pesquisa não especificada para a pesquisa de nula ou estar vazia. Não é especialmente útil, mas é a pesquisa mais simples, que pode fazer.

Opcionalmente, pode adicionar **`$count=true`** para o URL para devolver uma contagem dos documentos correspondentes aos critérios de pesquisa. Numa cadeia de caracteres de pesquisa em branco, trata todos os documentos no índice (2802 no caso de empregos em nova IORQUE).

## <a name="how-to-invoke-simple-query-parsing"></a>Como invocar a análise de consulta simples

Para consultas interativas, não é necessário especificar nada: a predefinição é simple. No código, se anteriormente invocado **queryType = full** para a sintaxe de consulta completa, foi possível repor de volta à predefinição com **queryType = simples**.

## <a name="example-1-field-scoped-query"></a>Exemplo 1: Consulta de âmbito de campo

Não é a primeira consulta sintaxe específica (a consulta funciona para a sintaxe simples e completa), mas vamos levar com este exemplo para introduzir um conceito de consulta de linha de base que produz uma resposta JSON razoavelmente legível. Para fins de brevidade, a consulta destina-se apenas a *business_title* campo e especifica apenas os títulos de negócios são retornados. 

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&search=*
```

O **searchFields** parâmetro restringe a pesquisa para apenas o campo de título do negócio. O **selecione** parâmetro determina quais os campos que estão incluídos no conjunto de resultados.

A resposta para esta consulta deve ser semelhante à seguinte captura de ecrã.

  ![Resposta de exemplo do postman](media/search-query-lucene-examples/postman-sample-results.png)

Deve ter observado que a pontuação de pesquisa também é devolvida para cada documento, apesar de pontuação de pesquisa não está especificada. Isto acontece porque a pontuação de pesquisa é metadados, com o valor que indica a ordem de classificação de resultados. As pontuações uniformes de 1 ocorrerem quando não existe nenhuma classificação, seja porque a pesquisa não era a pesquisa em texto completo ou porque não existe nenhum critério para aplicar. Para pesquisa de nulo, não há nenhum critério e as linhas que voltam estão na ordem arbitrária. Como os critérios de pesquisa permite na definição mais, verá as pontuações evoluam em valores significativos de pesquisa.

## <a name="example-2-look-up-by-id"></a>Exemplo 2: Pesquisar por ID

Neste exemplo é um pouco atípica, mas ao avaliar os comportamentos de pesquisa, talvez queira inspecionar todo o conteúdo de um documento para compreender por que foram incluído ou excluído dos resultados. Para devolver um documento inteiro, utilize um [operação de pesquisa de](https://docs.microsoft.com/rest/api/searchservice/lookup-document) para passar no ID do documento.

Todos os documentos têm um identificador exclusivo. Para experimentar a sintaxe para uma consulta de pesquisa, primeiro devolva uma lista de IDs de documento para que pode encontrar uma para utilizar. Para empregos em nova IORQUE, os identificadores são armazenados no `id` campo.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=id&$select=id&search=*
 ```

O exemplo seguinte é uma consulta de pesquisa devolver um documento específico com base em `id` "9E1E3AF9-0660-4E00-AF51-9B654925A2D5", que apareceu primeiro na resposta anterior. A seguinte consulta devolve o documento inteiro, campos selecionados não apenas. 

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs/9E1E3AF9-0660-4E00-AF51-9B654925A2D5?api-version=2017-11-11&$count=true&search=*
 ```

## <a name="example-3-search-precision"></a>Exemplo 3: Precisão da pesquisa

Consultas de prazo são termos simples, talvez muitos deles, que são avaliados de forma independente. Consultas de frase entre aspas e avaliadas como uma cadeia de caracteres textual. Precisão de correspondência é controlado por operadores e searchMode.

Exemplo 1: **`&search=fire`** devolve 150 resultados, em que todas as correspondências contenham fire o word em algum lugar no documento.

```
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&search=fire
```

Exemplo 2: **`&search=fire department`** devolve resultados de 2002. Correspondências são devolvidas para documentos que contenham fogo ou departamento.

```
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&search=fire department
```

Exemplo 3: **`&search="fire department"`** devolve 82 resultados. Colocar a cadeia de caracteres de aspas é uma pesquisa textual em ambos os termos e forem encontradas correspondências em termos com token no índice que consiste dos termos combinados. Isto explica por que uma pesquisa, como **`search=+fire +department`** não é equivalente. Ambos os termos são necessários, mas são analisados de forma independente. 

```
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&search="fire department"
```

## <a name="example-4-booleans-with-searchmode"></a>Exemplo 4: Booleanos com searchMode

Sintaxe simples suporta operadores booleanos na forma de carateres (`+, -, |`). O parâmetro searchMode informa as compensações entre a precisão e lembre-se, com `searchMode=any` favorável recolhimento (correspondência em critérios qualifica um documento para o conjunto de resultados), e `searchMode=all` favorável precisão (todos os critérios devem corresponder). A predefinição é `searchMode=any`, que pode ser confuso se são empilhadas uma consulta com vários operadores e introdução mais abrangente, em vez de resultados mais estreito. Isso é especialmente verdadeiro com não, onde os resultados incluem todos os documentos "que não contenha" uma condição específica.

Utilizar o searchMode padrão (qualquer), são devolvidos documentos de 2800: aquelas que contém a parte multi prazo "fire departamento", além de todos os documentos que não têm o termo "Metrotech Center".

```
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchMode=any&search="fire department"  -"Metrotech Center"
```
Alterar searchMode para `all` impõe um efeito cumulativo nos critérios e retorna o conjunto de resultados mais pequeno - 21 documentos - consiste em documentos que contêm a frase inteira "fire departamento" menos essas tarefas no endereço Metrotech Center.

```
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchMode=all&search="fire department"  -"Metrotech Center"
```


## <a name="example-5-structuring-results"></a>Exemplo 5: Resultados de estruturação

Resulta de vários controle de parâmetros que campos são na pesquisa, o número de documentos devolvidos em cada lote e a ordem de classificação. Neste exemplo resurfaces alguns dos exemplos anteriores, limitar os resultados a campos específicos utilizando o **$select** instrução e critérios de pesquisa textuais, 82 correspondências retornadas 

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"
```
Anexado no exemplo anterior, pode ordenar por título. Esse tipo funciona porque é civil_service_title *ordenável* no índice.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"&$orderby=civil_service_title
```

Resultados de paginação é implementado com o **$top** parâmetro, neste caso, retornar os documentos de 5 principais:

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"&$orderby=civil_service_title&$top=5&$skip=0
```

Para obter o próximo 5, ignore o primeiro batch:

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"&$orderby=civil_service_title&$top=5&$skip=5
```

## <a name="next-steps"></a>Passos Seguintes
Tente especificar consultas no seu código. As ligações seguintes explicam como configurar a consultas de pesquisa para o .NET e a API de REST com a sintaxe simples predefinido.

* [Consultar o índice da Azure Search utilizando o SDK .NET](search-query-dotnet.md)
* [Consultar o índice de pesquisa do Azure com a API REST](search-query-rest-api.md)

Referência de sintaxe adicional, a arquitetura de consulta e exemplos podem ser encontrados nas seguintes ligações:

+ [Exemplos de consulta de sintaxe Lucene para a criação de consultas avançadas](search-query-lucene-examples.md)
+ [Completa como funciona a pesquisa de texto no Azure Search](search-lucene-query-architecture.md)
+ [Sintaxe de consulta simples](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)
+ [Consultas de Lucene completas](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)
