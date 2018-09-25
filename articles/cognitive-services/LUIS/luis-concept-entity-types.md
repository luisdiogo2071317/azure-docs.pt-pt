---
title: Tipos de entidade em aplicações de LUIS - compreensão de idiomas
titleSuffix: Azure Cognitive Services
description: Adicione entidades (dados de chave no domínio de seu aplicativo) em aplicações de Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 3ed10ac428b7ce2e528ccf46e34c1d394523bdec
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47042453"
---
# <a name="entities-in-luis"></a>Entidades de LUIS

As entidades são palavras ou frases em expressões que são dados de chave no domínio da sua aplicação.

## <a name="entity-compared-to-intent"></a>Em comparação comparada a intenção de entidade
A entidade representa uma palavra ou frase dentro da expressão que pretende que sejam extraídos. Uma expressão pode incluir muitas entidades ou nenhum de todo. Uma entidade representa uma classe, incluindo uma coleção de objetos semelhantes (lugares, coisas, pessoas, eventos ou conceitos). Entidades descrevem informações relevantes para a intenção e, às vezes, são essenciais para a sua aplicação realizar suas tarefas. Por exemplo, uma aplicação de pesquisa de notícias pode incluir entidades como "tópico", "origem", "palavra-chave" e "data de publicação", que são dados de chave para pesquisar notícias. Num aplicativo de reservas de viagens, o "localização", "data", "companhia aérea", "viagens class" e "pedidos" são informações da chave de reservas de voo (relevantes para a intenção de "Bookflight").

Por comparação, a intenção representa a predição da expressão inteira. 

## <a name="entities-represent-data"></a>Entidades representam dados
As entidades são dados que pretende solicitar a partir da expressão. Isso pode ser um nome, data, nome do produto ou qualquer grupo de palavras. 

|Expressão|Entidade|Dados|
|--|--|--|
|Comprar 3 pedidos de Nova Iorque|Número pré-criados<br>Location.Destination|3<br>Nova Iorque|
|Comprar um pedido de nova York para Londres em 5 de Março|Location.Origin<br>Location.Destination<br>DatetimeV2 pré-criados|Nova Iorque<br>Londres<br>5 de março de 2018|

## <a name="entities-are-optional-but-highly-recommended"></a>As entidades são opcionais, mas altamente recomendada
Enquanto os objetivos são necessários, as entidades são opcionais. Não é necessário criar entidades para todos os conceitos na sua aplicação, mas apenas para o necessário para a aplicação tomar medidas. 

Se as expressões não têm detalhes que seu bot tem de continuar, não é necessário para adicioná-los. Como seu aplicativo amadurece, pode adicioná-los mais tarde. 

Se não tiver a certeza de como usaria as informações, adicione algumas entidades pré-concebidas comuns, como datetimeV2, ordinal, e-mail e número de telefone.

## <a name="label-for-word-meaning"></a>Etiqueta para o significado da palavra
Se a opção do word ou disposição do word é o mesmo, mas não significa que a mesma coisa, não classifique-o com a entidade. 

As seguintes expressões, o word `fair` é um homograph. Ele é têm a mesma, mas tem um significado diferente:

|Expressão|
|--|
|Que tipo de fairs condado de estão acontecendo na área de Seattle neste Verão?|
|É a classificação atual para a revisão de Seattle justo?|

Se quisesse uma entidade de eventos para localizar todos os dados de eventos, a palavra da etiqueta `fair` a primeira expressão, mas não na segunda.

## <a name="entities-are-shared-across-intents"></a>Entidades são partilhadas entre intenções
Entidades são partilhadas entre os objetivos. Elas não pertencem a qualquer objetivo único. Objetivos e entidades podem ser associadas a semanticamente mas não é uma relação exclusiva.

Na expressão "reservar quarto-me um pedido para Paris", "Paris" é uma entidade de localização de tipo. Ao reconhecer as entidades que são mencionadas na entrada do usuário, o LUIS ajuda-o a escolher as ações de específicas para satisfazer um objetivo.

## <a name="assign-entities-in-none-intent"></a>Atribuir entidades na intenção None
Todos os objetivos, incluindo o **None** intenção, deve ter entidades com o nome. Isto ajuda a saber mais sobre em que as entidades são nas expressões e quais são palavras em torno das entidades de LUIS. 

## <a name="types-of-entities"></a>Tipos de entidades
LUIS oferece muitos tipos de entidades; entidades previamente concebidas, máquina personalizada aprendeu entidades e entidades de lista.

| Nome | Pode Etiquetar | Descrição |
| -- |--|--|
| **Pré-criados** <br/>[Personalizado](#prebuilt)| |  **Definição**<br>Tipos internos que representam conceitos comuns. <br><br>**Lista**<br/>número de expressões-chave, ordinal, temperatura, dimensão, dinheiro, idade, percentagem, e-mail, URL, número de telefone e expressões-chave. <br><br>Os nomes de entidades pré-criados estão reservados. <br><br>Todas as entidades pré-concebidas que são adicionadas à aplicação são retornadas no [ponto final](luis-glossary.md#endpoint) consulta. Para obter mais informações, consulte [entidades pré-concebidas](./luis-prebuilt-entities.md). <br/><br/>[Resposta de exemplo para a entidade](luis-concept-data-extraction.md#prebuilt-entity-data)|
|<!-- added week of 3/21/08 --> **Expressão regular**<br/>[RegEx](#regex)||**Definição**<br>Expressão regular personalizada para texto formatado de expressão não processados. Ele ignora caso e ignora a variante cultural.  <br><br>Esta entidade é bom para palavras ou frases que consistentemente são formatados com qualquer variação que também é consistente.<br><br>Correspondência de expressões regulares é aplicada após a verificação ortográfica alterações. <br><br>Se a expressão regular é demasiado complexa, como o uso de muitos Retos, não é possível adicionar a expressão para o modelo. <br><br>**Exemplo**<br>`kb[0-9]{6,}` kb123456 correspondências.<br/><br/>[Início rápido](luis-quickstart-intents-regex-entity.md)<br>[Resposta de exemplo para a entidade](luis-concept-data-extraction.md)|
| **Simples** <br/>[Ficou a saber de máquina](#machine-learned) | ✔ | **Definição**<br>Uma entidade é uma entidade genérica que descreve um único conceito e será adquirida do contexto aprendidas por máquina. Contexto incluem a escolha do word, o posicionamento de word e o comprimento da expressão.<br/><br/>Esta é uma boa entidade palavras ou expressões que não estão formatados de forma consistente, mas indicam a mesma coisa. <br/><br/>[Início rápido](luis-quickstart-primary-and-secondary-data.md)<br/>[Resposta de exemplo para a entidade](luis-concept-data-extraction.md#simple-entity-data)|  
| **Lista** <br/>[Correspondência exata](#exact-match)|| **Definição**<br>Lista de entidades representam um conjunto de palavras relacionadas, juntamente com seus synoymns fixo e fechado no seu sistema. <br><br>Cada entidade de lista pode ter um ou mais formulários. Melhor utilizados para um conjunto conhecido de variações em maneiras de representar o mesmo conceito.<br/><br/>LUIS não Deteta valores adicionais para entidades de lista. Utilize o **Recomendamos** funcionalidade para ver sugestões para novas palavras com base na lista atual.<br/><br>Se houver mais de uma entidade de lista com o mesmo valor, cada entidade é devolvida na consulta de ponto final. <br/><br/>[Início rápido](luis-quickstart-intent-and-list-entity.md)<br>[Resposta de exemplo para a entidade](luis-concept-data-extraction.md#list-entity-data)| 
| **Pattern.any** <br/>[Misto](#mixed) | ✔|**Definição**<br>Patterns.any é um marcador de posição de comprimento variável utilizado apenas na expressão de modelo de um padrão para marcar onde a entidade começa e termina.  <br><br>**Exemplo**<br>Devido uma pesquisa de expressão para livros com base no título, a pattern.any extrai o título completo. É uma expressão de modelo usando pattern.any `Who wrote {BookTitle}[?]`.<br/><br/>[Tutorial](luis-tutorial-pattern.md)<br>[Resposta de exemplo para a entidade](luis-concept-data-extraction.md#composite-entity-data)|  
| **Composição** <br/>[Ficou a saber de máquina](#machine-learned) | ✔|**Definição**<br>Uma entidade composta é constituída por outras entidades, como entidades previamente concebidas, simples, regex, lista, hierárquica. As entidades separadas formam uma entidade inteira. <br><br>**Exemplo**<br>Uma entidade composta com o nome PlaneTicketOrder pode ter entidades subordinadas pré-criados `number` e `ToLocation`. <br/><br/>[Tutorial](luis-tutorial-composite-entity.md)<br>[Resposta de exemplo para a entidade](luis-concept-data-extraction.md#composite-entity-data)|  
| **Hierárquica** <br/>[Ficou a saber de máquina](#machine-learned) |✔ | **Definição**<br>Uma entidade hierárquica é uma categoria de contextualmente aprendidas entidades simples.<br><br>**Exemplo**<br>Dada uma entidade hierárquica dos `Location` com subordinados `ToLocation` e `FromLocation`, cada filho pode ser determinado com base no **contexto** dentro da expressão. Na expressão, `Book 2 tickets from Seattle to New York`, o `ToLocation` e `FromLocation` contextualmente diferentes com base em palavras em torno delas. <br/><br/>**Não utilize se**<br>Se estiver à procura de uma entidade que tem as correspondências de texto exato para crianças, independentemente do contexto, deve usar uma entidade de lista. Se estiver procurando por uma relação principal-subordinado com outros tipos de entidade, deve usar a entidade composta.<br/><br/>[Início rápido](luis-quickstart-intent-and-hier-entity.md)<br>[Resposta de exemplo para a entidade](luis-concept-data-extraction.md#hierarchical-entity-data)|

<a name="prebuilt"></a>
**Pré-criados** entidades são entidades personalizadas fornecidas pelo LUIS. Algumas destas entidades definidas no código aberto [reconhecedores texto](https://github.com/Microsoft/Recognizers-Text) projeto. Existem muitas [exemplos](https://github.com/Microsoft/Recognizers-Text/tree/master/Specs) no diretório /Specs para as culturas suportadas. Se sua cultura específica ou a entidade não é atualmente suportada, contribuem para o projeto. 

<a name="machine-learned"></a>
**Ficou a saber de máquina** entidades funcionam melhor quando testado por meio [consultas de ponto final](luis-concept-test.md#endpoint-testing) e [expressões de ponto final de revisão](luis-how-to-review-endoint-utt.md). 

<a name="regex"></a>
**Entidades de expressão regular** são definidos por uma expressão regular, o utilizador fornece como parte da definição de entidade. 

<a name="exact-match"></a>
**Com correspondência exacta** entidades utilizam o texto fornecido na entidade para tornar um texto exato corresponder.

<a name="mixed"></a>
**Misto** entidades usar uma combinação de métodos de deteção de entidade.

## <a name="entity-limits"></a>Limites de entidade
Revisão [limites](luis-boundaries.md#model-boundaries) para compreender quantos de cada tipo de entidade é possível adicionar a um modelo.

## <a name="entity-roles"></a>Funções de entidade
Entidade [funções](luis-concept-roles.md) são utilizados em padrões apenas. 

## <a name="composite-vs-hierarchical-entities"></a>Entidades hierárquica do vs compostos
Entidades compostas e entidades hierárquicas tanto têm relações pai-filho e são máquina aprendida. O machine learning permite que o LUIS compreender as entidades com base em diferentes contextos (disposição de palavras). Entidades compostas são mais flexíveis porque permitem que os tipos de entidade diferentes como filhos. Subordinados de uma entidade hierárquica são apenas as entidades simples. 

|Tipo|Objetivo|Exemplo|
|--|--|--|
|Hierárquica|Principal-subordinado de entidades simples|Location.Origin=New York<br>Location.Destination=London|
|Composição|Entidades de principal-subordinado: pré-criados, lista, simples, hierárquica| número = 3<br>lista = de primeira classe<br>prebuilt.datetimeV2=March 5|

## <a name="data-matching-multiple-entities"></a>Correspondência de várias entidades de dados
Se uma palavra ou frase corresponder a mais de uma entidade, a consulta de ponto final devolve cada entidade. Se adicionar a entidade de número pré-criados e prebuild datetimeV2 e ter uma expressão `create meeting on 2018/03/12 for lunch with wayne`, LUIS reconhece todas as entidades e retorna uma matriz de entidades como parte da resposta do ponto final JSON: 

```JSON
{
  "query": "create meeting on 2018/03/12 for lunch with wayne",
  "topScoringIntent": {
    "intent": "Calendar.Add",
    "score": 0.9333419
  },
  "entities": [
    {
      "entity": "2018/03/12",
      "type": "builtin.datetimeV2.date",
      "startIndex": 18,
      "endIndex": 27,
      "resolution": {
        "values": [
          {
            "timex": "2018-03-12",
            "type": "date",
            "value": "2018-03-12"
          }
        ]
      }
    },
    {
      "entity": "2018",
      "type": "builtin.number",
      "startIndex": 18,
      "endIndex": 21,
      "resolution": {
        "value": "2018"
      }
    },
    {
      "entity": "03/12",
      "type": "builtin.number",
      "startIndex": 23,
      "endIndex": 27,
      "resolution": {
        "value": "0.25"
      }
    }
  ]
}
```

## <a name="data-matching-multiple-list-entities"></a>Várias entidades de lista de correspondência de dados
Se uma palavra ou frase corresponder a mais de uma entidade de lista, a consulta de ponto final devolve cada entidade de lista.

Para a consulta `when is the best time to go to red rock?`, e a aplicação com a palavra `red` em mais de uma lista, LUIS reconhece todas as entidades e retorna uma matriz de entidades como parte da resposta do ponto final JSON: 

```JSON
{
  "query": "when is the best time to go to red rock?",
  "topScoringIntent": {
    "intent": "Calendar.Find",
    "score": 0.06701678
  },
  "entities": [
    {
      "entity": "red",
      "type": "Colors",
      "startIndex": 31,
      "endIndex": 33,
      "resolution": {
        "values": [
          "Red"
        ]
      }
    },
    {
      "entity": "red rock",
      "type": "Cities",
      "startIndex": 31,
      "endIndex": 38,
      "resolution": {
        "values": [
          "Destinations"
        ]
      }
    }
  ]
}
```

## <a name="if-you-need-more-than-the-maximum-number-of-entities"></a>Se precisar de mais do que o número máximo de entidades 

Poderá ter de utilizar entidades hierárquicas e compostas. Entidades hierárquicas refletem a relação entre entidades que partilham características ou que são membros de uma categoria. As entidades subordinadas são todos os membros da categoria de seu pai. Por exemplo, uma entidade hierárquica PlaneTicketClass com o nome pode ter as entidades subordinadas EconomyClass e FirstClass. A hierarquia abrange apenas um nível de profundidade.  

Entidades compostas representam partes de um todo. Por exemplo, uma entidade composta com o nome PlaneTicketOrder pode ter entidades subordinadas companhia aérea, destino, DepartureCity, DepartureDate e PlaneTicketClass. Criar uma entidade composta de entidades simples já existentes, filhos de entidades hierárquicas ou entidades pré-concebidas.  

LUIS também fornece o tipo de entidade de lista que não será adquirida para máquina, mas permite que a sua aplicação LUIS especificar uma lista fixa de valores. Ver [limites de LUIS](luis-boundaries.md) referência para rever os limites do tipo de entidade de lista. 

Se já considerada hierárquico e compostos e lista entidades e ainda precisa de mais do que o limite, contacte o suporte. Para fazê-lo, coletar informações detalhadas sobre o seu sistema, vá para o [LUIS](luis-reference-regions.md#luis-website) Web site e, em seguida, selecione **suporte**. Se a sua subscrição do Azure inclui os serviços de suporte, contacte [suporte técnico do Azure](https://azure.microsoft.com/support/options/). 

## <a name="best-practices"></a>Melhores práticas

Criar uma [entidade](luis-concept-entity-types.md) quando o aplicativo de chamada ou bot tem alguns parâmetros ou dados de expressão necessário para executar uma ação. Uma entidade é uma palavra ou frase na expressão que precisa extraídos – talvez como um parâmetro para uma função. 

Para selecionar o tipo correto de entidade para adicionar ao seu aplicativo, precisa saber como esses dados são inseridos por utilizadores. Cada tipo de entidade está usando um mecanismo diferente, como a lista de machine learning, fechada ou uma expressão regular. Se não souber, começar com uma entidade e etiquetar a palavra ou expressão que representa esses dados em todas as expressões, em todos os objetivos, incluindo o None intenção.  

Reveja as expressões de ponto final em intervalos regulares para localizar um uso comum em que uma entidade pode ser identificada como uma expressão regular ou encontrada com uma correspondência exata de texto.  

Como parte da revisão, considere a adição de uma lista de frase para adicionar um sinal para LUIS palavras ou expressões que significativo ao seu domínio, mas não são correspondências exatas e para o qual o LUIS não tem uma alta confiança.  

Ver [melhores práticas](luis-concept-best-practices.md) para obter mais informações.

## <a name="next-steps"></a>Passos Seguintes

Aprenda conceitos sobre o good [expressões com](luis-concept-utterance.md). 

Ver [adicionar entidades](luis-how-to-add-entities.md) para saber mais sobre como adicionar entidades à sua aplicação LUIS.