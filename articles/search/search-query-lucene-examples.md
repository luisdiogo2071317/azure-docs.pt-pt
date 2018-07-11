---
title: Exemplos de consulta Lucene para o Azure Search | Documentos da Microsoft
description: Sintaxe de consulta Lucene para a pesquisa difusa, pesquisa de proximidade, aumentos de termos, pesquisa de expressão regular e pesquisa com carateres universais.
author: LiamCa
manager: pablocas
tags: Lucene query analyzer syntax
services: search
ms.service: search
ms.topic: conceptual
ms.date: 06/28/2018
ms.author: liamca
ms.openlocfilehash: a3baa17906e3bfede8a7fc5f8a0bfbde9d2a57ce
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37951027"
---
# <a name="lucene-query-syntax-examples-for-building-queries-in-azure-search"></a>Exemplos de sintaxe de consulta Lucene para a criação de consultas na Azure Search
Ao construir consultas para o Azure Search, pode utilizar qualquer um da predefinição [sintaxe de consulta simples](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) ou a alternativa [analisador de consultas do Lucene no Azure Search](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search). O analisador de consultas de Lucene suporta as construções de consulta mais complexas, como consultas com âmbito de campo, a pesquisa difusa, pesquisa de proximidade, aumentos de termos e pesquisa de expressão regular.

Neste artigo, pode percorrer exemplos que demonstram as operações de consulta disponíveis quando utiliza a sintaxe completa.

## <a name="viewing-the-examples-in-jsfiddle"></a>Ver os exemplos no JSFiddle

Todos os exemplos neste artigo são consultas executáveis que são executadas em relação a um índice de pesquisa previamente carregado na [JSFiddle](https://jsfiddle.net), um editor de código online para testar o script e HTML. 

Para executá-los, com o botão direito sobre os URLs de exemplo de consulta para abrir JSFiddle numa janela do browser separado.

> [!NOTE]
> Os exemplos seguintes tirar partido de um índice de pesquisa que consiste de tarefas disponíveis com base num conjunto de dados fornecido pelos [cidade de Nova Iorque OpenData](https://nycopendata.socrata.com/) iniciativa. Estes dados não devem ser considerados atual ou completa. O índice é um serviço de proteção de segurança fornecida pela Microsoft. Não é necessário uma subscrição do Azure ou o Azure Search para experimentar estas consultas.
>


## <a name="how-to-invoke-full-lucene-parsing"></a>Como invocar a análise de Lucene completo

Todos os exemplos neste artigo de especificar o **queryType = full** parâmetro, que indica que a sintaxe completa é processada pelo Parser de consulta Lucene de pesquisa. 

**Exemplo 1** – com o botão direito o seguinte fragmento de consulta para abri-lo numa nova página do browser que carrega JSFiddle e executa a consulta:

* [& queryType = full & pesquisa = *](https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*)

Na janela do browser nova, a origem de JavaScript e HTML de saída são apresentados lado a lado. O script faz referência a uma consulta completa (não apenas o trecho de código, conforme mostrado na ligação). A consulta completa é mostrada nos URLs para cada exemplo. 

Esta consulta devolve os documentos do nosso índice de tarefas da cidade de nova York (nycjobs, carregado num serviço de proteção de segurança). Para fins de brevidade, a consulta especifica apenas business títulos são devolvidos. A consulta subjacente completa é o seguinte:

    https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*

O **searchFields** parâmetro restringe a pesquisa para apenas o campo de título do negócio. O **queryType** está definida como **completo**, que instrui o Azure Search para utilizar o analisador de consultas de Lucene para esta consulta.

> [!NOTE]
> Para informações gerais sobre o processamento de consultas, consulte [completa como funciona a pesquisa de texto no Azure Search](search-lucene-query-architecture.md). Para obter mais informações sobre parâmetros de pesquisa, consulte [documentos sobre pesquisa (API do REST de serviço de pesquisa do Azure)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).
>

### <a name="fielded-query-operation"></a>Operação de consulta respondidas
Pode modificar os exemplos neste artigo, especificando uma **fieldname:searchterm** construção para definir uma operação de consulta respondidas, em que o campo é uma única palavra, e o termo de pesquisa é também uma única palavra ou frase, opcionalmente, com Operadores booleanos. Alguns exemplos incluem o seguinte:

* business_title:(senior NOT junior)
* Estado: ("New York" e "Nova Jérsia")

Certifique-se de que colocar várias cadeias de caracteres entre aspas, se pretender que ambas as cadeias de caracteres a ser avaliada como uma única entidade, como nesse caso a procurar duas cidades diferentes no campo localização. Além disso, certifique-se o operador está em maiúsculas como pode ver com não e and.

O campo especificado no **fieldname:searchterm** tem de ser um campo pesquisável. Ver [criar índice (API do REST de serviço de pesquisa do Azure)](https://docs.microsoft.com/rest/api/searchservice/create-index) para obter detalhes sobre como os atributos de índice são usados nas definições de campo.

**Exemplo 2** – com o botão direito o seguinte fragmento de consulta, esta consulta pesquisa de títulos de negócios com o sênior do termo em-los, mas não júnior:

* [& queryType = full & pesquisa = business_title:senior não júnior](https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26$select=business_title%26queryType=full%26search=business_title:senior+NOT+junior)

## <a name="fuzzy-search-example"></a>Exemplo de pesquisa difusa
A pesquisa difusa localiza correspondências em termos que têm uma construção semelhante. Por [documentação de Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html), pesquisas difusas baseiam-se nos [Damerau Levenshtein distância](https://en.wikipedia.org/wiki/Damerau%e2%80%93Levenshtein_distance).

Para fazer uma pesquisa difusa, acrescente o til "~" símbolo no final de uma única palavra com um parâmetro opcional, um valor entre 0 e 2, que especifica a distância de edição. Por exemplo, "azul ~" ou "azul ~ 1" retornaria azul, blues e cola.

**Exemplo 3** – com o botão direito o seguinte fragmento de consulta. Esta consulta pesquisa para as tarefas com o representante de prazo (em que ele está incorreto):

* [& queryType = full & pesquisa = business_title:asosiate ~](https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26$select=business_title%26queryType=full%26search=business_title:asosiate~)

> [!Note]
> As consultas difusas não são [analisados](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis), que pode ser uma surpresa, se esperar Lematização ou à Lematização. Análise lexical só é executada em termos de conclusão (uma consulta de termo ou a consulta de frase). Tipos de consulta com os termos incompletos (consulta de prefixo, consulta de caráter universal, consultas de regex, consulta difusa) são adicionados diretamente à árvore de consulta, ignorando a fase de análise. A transformação de apenas executada nos termos de consulta incompleta é minúsculas.
>

## <a name="proximity-search-example"></a>Exemplo de pesquisa de proximidade
Pesquisas de proximidades são usadas para encontrar os termos que estejam perto uns dos outros num documento. Inserir um til "~" símbolo no final de uma frase seguido do número de palavras que criar o limite de proximidade. Por exemplo, "aeroporto de hotel" ~ 5 encontrará o hotel de termos e aeroporto dentro de 5 palavras da outra num documento.

**Exemplo 4** – com o botão direito a consulta. Procure tarefas com o termo "analista sênior" em que ele é separado por mais do que uma palavra:

* [& queryType = full & pesquisa = business_title: "analista sênior" ~ 1](https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~1)

**Exemplo 5** – experimentá-la novamente remover as palavras entre o termo "analista sênior".

* [& queryType = full & pesquisa = business_title: "analista sênior" ~ 0](https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~0)

## <a name="term-boosting-examples"></a>Termo adaptativo exemplos
Aumentos de termos refere-se a classificação de um documento superior se contiver o termo elevado, em relação ao documentos que contenham o termo. Isto é diferente de perfis de classificação em que aumentam a perfis de classificação determinados campos, em vez de termos específicos. O exemplo a seguir ajuda a ilustrar as diferenças.

Considere corresponde a um perfil de classificação que aumenta num determinado campo, como **gênero** no exemplo musicstoreindex. Aumentos de termos poderiam ser usado para melhorar ainda mais determinada pesquisa termos mais do que outras pessoas. Por exemplo, "rock ^ 2 eletrônicos" irá aumentar a documentos que contêm os termos de pesquisa na **gênero** campo maior do que outros campos pesquisáveis no índice. Além disso, documentos que contenham o termo de pesquisa "rock" serão classificados superior do que o outro termo de pesquisa "eletrónico" como resultado o valor de aumento de termo (2).

Para aumentar um termo, utilize o sinal de interpolação, "^", símbolo com um fator de aumento (um número) no final do período de vigência esteja a pesquisar. Quanto maior for o fator de aumento, o mais relevante o termo serão relativas a outros termos de pesquisa. Por predefinição, o fator de aumento é 1. Embora o fator de boost tem de ser positivo, ele pode ser inferior a 1 (por exemplo, 0,2).

**Exemplo 6** – com o botão direito a consulta. Procurar tarefas com o termo "analista de computador", onde podemos ver não há resultados com o computador de palavras e analista, ainda são de trabalhos do analista na parte superior dos resultados.

* [& queryType = full & pesquisa = business_title:computer analista](https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

**Exemplo 7** – tente novamente, esse aumento do tempo resulta com o computador de termo sobre o analista de termo se não existirem ambas as palavras.

* [& queryType = full & pesquisa = business_title:computer ^ 2 analista](https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

## <a name="regular-expression-example"></a>Exemplo de expressão regular
Uma pesquisa de expressão regular encontra uma correspondência com base no conteúdo entre barras "/", como documentado na [RegExp classe](http://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html).

**Exemplo 8** – com o botão direito a consulta. Termo de pesquisa para as tarefas com um o Sênior ou Junior.

* `&queryType=full&$select=business_title&search=business_title:/(Sen|Jun)ior/`

O URL para este exemplo não ficarão compostos corretamente na página. Como solução, copie o URL abaixo e cole-o para o endereço de URL do browser: `https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26queryType=full%26$select=business_title%26search=business_title:/(Sen|Jun)ior/)`

## <a name="wildcard-search-example"></a>Exemplo de pesquisa com carateres universais
Pode usar sintaxe geralmente reconhecido para vários (\*) ou único pesquisas de com carateres universais do caractere (?). Tenha em atenção de que o analisador de consultas de Lucene suporta a utilização desses símbolos com um único termo e não uma frase.

**Exemplo 9** – com o botão direito a consulta. Procure as tarefas que contêm o prefixo "programa", que inclui títulos de negócios com os termos de programação e o programador nela.

* [& queryType = full e $select = business_title e pesquisa = business_title:prog*](https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26queryType=full%26$select=business_title%26search=business_title:prog*)

Não é possível utilizar um * ou? símbolo de como o primeiro caráter de uma pesquisa.

## <a name="next-steps"></a>Passos Seguintes
Experimente a especificar o analisador de consultas do Lucene no seu código. As ligações seguintes explicam como configurar a consultas de pesquisa para o .NET e a API REST. As ligações utilizam a sintaxe simples padrão, por isso terá de aplicar o que aprendeu deste artigo para especificar a **queryType**.

* [Consultar o índice da Azure Search utilizando o SDK .NET](search-query-dotnet.md)
* [Consultar o índice de pesquisa do Azure com a API REST](search-query-rest-api.md)

## <a name="see-also"></a>Consulte também

 [Completa como funciona a pesquisa de texto no Azure Search](search-lucene-query-architecture.md)