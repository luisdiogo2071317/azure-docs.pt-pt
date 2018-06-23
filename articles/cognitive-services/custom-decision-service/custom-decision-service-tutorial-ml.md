---
title: Machine Learning - serviços cognitivos do Azure | Microsoft Docs
description: Um tutorial do machine learning no serviço de decisão de personalizada no Azure, uma API baseado na nuvem para a nível contextual das decision-making.
services: cognitive-services
author: slivkins
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/08/2018
ms.author: slivkins;marcozo;alekh
ms.openlocfilehash: 50814d67ee39c6657954610358462d877843416e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354596"
---
# <a name="machine-learning"></a>Aprendizagem automática

Este tutorial aborda o avançadas do machine learning funcionalidades no serviço de decisão personalizada. O tutorial consiste em duas partes: [featurization](#featurization-concepts-and-implementation) e [funcionalidade especificação](#feature-specification-format-and-apis). Featurization refere-se que representa os dados como "funcionalidades" do machine learning. Especificação de funcionalidade abrange o formato JSON e as APIs auxiliar para especificar as funcionalidades.

Por predefinição, o machine learning no serviço de decisão personalizada é transparente para o cliente. Funcionalidades automaticamente são extraídas de conteúdo e é utilizado um algoritmo de aprendizagem reinforcement padrão. Vários outros serviços do Azure cognitivos tira partido da funcionalidade de extração: [entidade de ligação](../entitylinking/home.md), [análise de texto](../text-analytics/overview.md), [emoções](../emotion/home.md), e [computador visão](../computer-vision/home.md). Este tutorial pode ser ignorado se apenas a funcionalidade predefinido é utilizada.

## <a name="featurization-concepts-and-implementation"></a>Featurization: conceitos e implementação

Serviço de decisão personalizado toma decisões de um de cada. Cada decisão envolve escolher entre vários alternativas, a.k.a., ações. Dependendo da aplicação, a decisão pode optar por uma única ação ou uma lista de ações classificados (abreviada).

Por exemplo, personalizar a seleção dos artigos na página de front-de um Web site. Aqui, ações correspondem aos artigos, não sendo cada decisão que artigos para mostrar para um determinado utilizador.

Cada ação é representada por um vetor de propriedades, henceforth chamado *funcionalidades*. Pode especificar novas funcionalidades, para além das funcionalidades extraídas automaticamente. Pode ainda dar instruções ao serviço de decisão de personalizada a algumas funcionalidades de registo, mas ignorá-las para o machine learning.

### <a name="native-vs-internal-features"></a>Nativo vs funcionalidades internos

Pode especificar funcionalidades num formato mais natural para a sua aplicação, -la de ser um número, uma cadeia ou uma matriz. Estas funcionalidades são denominadas "funcionalidades nativas". Serviço decisão personalizado traduz cada funcionalidade nativa para uma ou mais funcionalidades numérico, denominadas "funcionalidades internas".

A tradução para funcionalidades internas é o seguinte:

- funcionalidades numérico permanecem igual.
- uma matriz numérico traduz-se várias funcionalidades numérico, um para cada elemento da matriz.
- uma funcionalidade de valor de cadeia `"Name":"Value"` está por predefinição convertida uma funcionalidade com o nome `"NameValue"` e o valor de 1.
- Opcionalmente, uma cadeia pode ser representada como [matriz de palavras](https://en.wikipedia.org/wiki/Bag-of-words_model). Em seguida, uma funcionalidade interna é criada para cada palavra na cadeia, cujo valor é o número de ocorrências desta palavra.
- Valor de zero internas funcionalidades estão omitidas.

### <a name="shared-vs-action-dependent-features"></a>Partilhado vs funcionalidades dependentes da ação

Algumas funcionalidades Consulte a decisão de toda e são os mesmos para todas as ações. Chamamos-los *funcionalidades partilhadas*. Algumas outras funcionalidades são específicas para uma ação específica. Chamamos-los *funcionalidades dependentes da ação* (ADFs).

No exemplo em execução, funcionalidades partilhadas foi descrevem o utilizador e/ou o estado do mundo. Funcionalidades como geolocalização, idade e sexo do utilizador, e os eventos principais estavam a acontecer neste momento. Funcionalidades de ação dependentes foi descrevem as propriedades de um artigo especificado, tais como os tópicos abrangidos por este artigo.

### <a name="interacting-features"></a>Funcionalidades de interação

As funcionalidades, muitas vezes, "interagir": o efeito de um depende de outros utilizadores. Por exemplo, a funcionalidade X é se o utilizador é interessado no desporto. Funcionalidade Y é se um determinado artigo sobre desporto. Em seguida, o efeito da funcionalidade Y está altamente dependente de funcionalidade X.

Para contemplar a interação entre funcionalidades X e Y, crie um *quadratic* funcionalidade cujo valor é X\*Y. (Iremos também diga, "cross" X e Y.) Pode escolher os pares de funcionalidades são ultrapassados.

> [!TIP]
> Uma funcionalidade partilhada deve ser ultrapassada com funcionalidades de ação dependentes para influenciar a respetiva classificação. Uma funcionalidade de ação dependentes deve ser ultrapassada com funcionalidades partilhadas para contribuir para a personalização.

Por outras palavras, uma funcionalidade partilhada não ultrapassada com qualquer ADFs influencia cada ação da mesma forma. Um ADF não ultrapassado com qualquer funcionalidade partilhada influencia demasiado cada decisão. Estes tipos de funcionalidades podem reduzir a variância de reward estimativas.

### <a name="implementation-via-namespaces"></a>Implementação através de espaços de nomes

Pode implementar funcionalidades ultrapassadas (bem como outros conceitos featurization) através de "VW linha de comandos" no Portal. A sintaxe baseia-se no [Vowpal Wabbit](http://hunch.net/~vw/) linha de comandos.

Central para a implementação é o conceito de *espaço de nomes*: um subconjunto das funcionalidades com nome. Cada funcionalidade pertence a exatamente um espaço de nomes. O espaço de nomes pode ser especificado explicitamente quando o valor de funcionalidade é fornecido para o serviço de decisão personalizada. É a única forma de fazer referência a uma funcionalidade na linha de comandos VW.

Um espaço de nomes é "partilhado" ou "dependentes da ação": o consiste apenas em funcionalidades partilhadas ou -consiste apenas em funcionalidades dependentes da ação da mesma ação.

> [!TIP]
> É uma boa prática moldar funcionalidades em espaços de nomes especificados explicitamente. Funcionalidades relacionadas no mesmo espaço de nomes de grupo.

Se o espaço de nomes não for fornecido, a funcionalidade é atribuída automaticamente ao espaço de nomes predefinido.

> [!IMPORTANT]
> Funcionalidades e espaços de nomes não é necessário estar consistente através de ações. Em particular, um espaço de nomes pode ter diferentes funcionalidades de ações diferentes. Além disso, um espaço de nomes especificado pode ser definido para algumas ações e não para outros.

Várias funcionalidades internas veio do mesmo valor de cadeia nativo da funcionalidade são agrupadas no mesmo espaço de nomes. Duas funcionalidades nativas que se situam em diferentes espaços de nomes são tratadas como distintos, mesmo que tenha o mesmo nome da funcionalidade.

> [!IMPORTANT]
> Enquanto os IDs de espaço de nomes de longa e descritivo são comuns, a linha de comandos VW não distinguir entre cujo id começa com a mesma letra de espaços de nomes. Em que modo, IDs de espaço de nomes são letras único, tal como `x` e `y`.

Os detalhes de implementação são os seguintes:

- Para espaços de nomes de se estender `x` e `y`, escrever `-q xy` ou `--quadratic xy`. Em seguida, cada funcionalidade do `x` é cruzado com cada funcionalidade no `y`. Utilize `-q x:` para cruzada `x` com cada espaço de nomes e `-q ::` para cruzada todos os pares de espaços de nomes.

- A ignorar todas as funcionalidades no espaço de nomes `x`, escrever `--ignore x`.

Estes comandos são aplicados em separado, para cada ação sempre que os espaços de nomes são definidos.

### <a name="estimated-average-as-a-feature"></a>Médio estimado como uma funcionalidade

Como uma experimentação profundamente, o que seria o reward médio de uma determinada ação se de que foram escolhido para todas as decisões? Essa reward médio podia ser utilizado como uma medida de "qualidade geral" desta ação. Não é conhecido exatamente sempre outras ações foi escolhidas em vez disso, em algumas decisões. No entanto, podem ser estimada através de reinforcement técnicas de aprendizagem. A qualidade desta estimativa normalmente melhora ao longo do tempo.

Pode optar por incluir "estimado reward média" como uma funcionalidade para uma determinada ação. Em seguida, serviço de decisão personalizada atualizará automaticamente esta estimativa como novos dados são recebidos. Esta funcionalidade é denominada a *funcionalidade marginal* desta ação. Funcionalidades marginais podem ser utilizadas para o machine learning e de auditoria.

Para adicionar funcionalidades marginais, escrever `--marginal <namespace>` na linha de comandos VW. Definir `<namespace>` no JSON da seguinte forma:

```json
{<namespace>: {"mf_name":1 "action_id":1}
```

Este espaço de nomes, juntamente com outras funcionalidades dependentes da ação de uma determinada ação de inserção. Fornecer esta definição para cada decisão, com o mesmo `mf_name` e `action_id` para todas as decisões.

A funcionalidade marginal é adicionada para cada ação com `<namespace>`. O `action_id` pode ser qualquer nome de funcionalidade que identifica exclusivamente a ação. O nome da funcionalidade está definido como `mf_name`. Em particular, marginal funcionalidades com diferentes `mf_name` são tratadas como funcionalidades diferentes — é aprendida uma ponderação diferente para cada `mf_name`.

A utilização de predefinida é que `mf_name` é igual para todas as ações. Em seguida, uma ponderação é aprendida para todas as funcionalidades marginais.

Também pode especificar várias funcionalidades marginais para a mesma ação, com os mesmos valores mas nomes das funcionalidades diferentes.

```json
{<namespace>: {"mf_name1":1 "action_id":1 "mf_name2":1 "action_id":1}}
```

### <a name="1-hot-encoding"></a>codificação de acesso frequente 1

Pode escolher representar algumas funcionalidades como vetores de bits, em que cada bit corresponde a um intervalo de valores possíveis. Este bit está definida como 1, se, e apenas se, a funcionalidade situam-se neste intervalo. Assim, existe um bit "ativos" está definido como 1 e outros estão definidos para 0. Este representação é geralmente conhecida como *acesso frequente 1 codificação*.

A codificação de acesso frequente 1 é típica de funcionalidades categórico como "região geográfica" que não tenham uma representação numérica inerentemente significativa. Também é recomendado para funcionalidades numéricos cuja influência no reward é provável que seja não linear. Por exemplo, um artigo especificado pode ser relevantes para um determinado grupo de antiguidade e irrelevantes a qualquer pessoa younger ou anterior.

Qualquer valor de cadeia de funcionalidade é frequente de 1 codificado por predefinição: é criada uma funcionalidade interna distinta para cada valor possível. Atualmente, o automática acesso frequente 1 codificação para funcionalidades numéricos e/ou com intervalos personalizados não são fornecidos.

> [!TIP]
> Os algoritmos do machine learning tratar todos os valores possíveis de uma determinada funcionalidade interno de forma uniforme: através de um comuns "ponderação". A codificação de acesso frequente 1 permite uma "ponderação" separada para cada intervalo de valores. Efetuar os intervalos menores leva a melhor recompensas depois suficiente os dados são recolhidos, mas podem aumentar a quantidade de dados necessários para convergir a recompensas melhor.

## <a name="feature-specification-format-and-apis"></a>Especificação de funcionalidade: formato e APIs

Pode especificar funcionalidades através de várias APIs auxiliar. Todos os APIs utilizam um formato de JSON comum. Seguem-se as APIs e o formato num nível conceptual. A especificação é complemented por um esquema de Swagger.

O modelo JSON básico para a especificação de funcionalidade é o seguinte:

```json
{
"<name>":<value>, "<name>":<value>, ... ,
"namespace1": {"<name>":<value>, ... },
"namespace2": {"<name>":<value>, ... },
...
}
```

Aqui `<name>` e `<value>` funcionar para o nome da funcionalidade e valor de funcionalidade, respetivamente. `<value>` pode ser uma cadeia, um número inteiro, flutuante, booleano ou uma matriz. Uma funcionalidade não encapsulada para um espaço de nomes é atribuída automaticamente no espaço de nomes predefinido.

Para representar uma cadeia como uma matriz de palavras, utilize uma sintaxe especial `"_text":"string"` em vez de `"<name>":<value>`. Efetivamente, é criada uma funcionalidade interna separada para cada palavra na cadeia de. O valor é o número de ocorrências desta palavra.

Se `<name>` começa com "_" (e não se encontra `"_text"`), em seguida, a funcionalidade é ignorada.

> [!TIP]
> Por vezes, intercale as funcionalidades de várias origens JSON. Para sua comodidade, pode representá-los da seguinte forma:
>
> ```json
> {
> "source1":<features>,
> "source2":<features>,
> ...
> }
> ```

Aqui `<features>` refere-se para a especificação de funcionalidade básica definida anteriormente. Níveis mais aprofundadas de "aninhamento" são permitidas demasiado. Serviço de decisão personalizado encontra automaticamente os objetos JSON "mais profundo existente", que podem ser interpretados como `<features>`.

#### <a name="feature-set-api"></a>API de conjunto de funcionalidades

A API de definir funcionalidade devolve uma lista das funcionalidades no formato JSON descrito anteriormente. Pode utilizar vários pontos finais da API de conjunto de funcionalidade. Cada ponto final é identificada pelo id do conjunto de funcionalidades e um URL. O mapeamento entre funcionalidade definido IDs e URLs está definido no Portal.

Chame a API de definir funcionalidade através da inserção do id do conjunto de funcionalidades correspondente no local adequado no JSON. Para funcionalidades dependentes da ação, a chamada é automaticamente parametrizada por id de ação. Pode especificar vários IDs de definir a funcionalidade para a mesma ação.

#### <a name="action-set-api-json-version"></a>API definir ação (versão JSON)

A API de definir ação tem uma versão na qual são especificadas ações e funcionalidades no JSON. Funcionalidades podem ser especificadas explicitamente e/ou através de APIs de conjunto de funcionalidade. Funcionalidades partilhadas podem ser especificadas uma vez para todas as ações.

#### <a name="ranking-api-http-post-call"></a>Classificação de API (chamada de HTTP POST)

Classificação API tem uma versão que utiliza a chamada de POST de HTTP. O corpo desta chamada Especifica ações e funcionalidades através de uma sintaxe JSON flexível.

Ações podem ser especificadas explicitamente e/ou através de ação Definir IDs. Sempre que um id do conjunto de ação é encontrado, é executada uma chamada para o ponto final de API de definir ação correspondente.

Para a API definir ação, as funcionalidades podem ser especificadas explicitamente e/ou através de APIs de definir a funcionalidade. Funcionalidades partilhadas podem ser especificadas uma vez para todas as ações.