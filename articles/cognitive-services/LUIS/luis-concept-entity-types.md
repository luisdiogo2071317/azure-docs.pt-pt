---
title: Compreender tipos de entidade LUIS aplicações do Azure | Microsoft Docs
description: Adicione entidades (dados chaves no domínio da sua aplicação) em aplicações de serviço de inteligente compreensão do idioma (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/28/2018
ms.author: v-geberr
ms.openlocfilehash: 01f451f7a3e09aacb029c2194044320717bfae96
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37083255"
---
# <a name="entities-in-luis"></a>Entidades de LUIS

Entidades são palavras ou frases reconhecíveis no utterances são dados de chave no domínio da sua aplicação.

## <a name="entity-compared-to-intent"></a>Em comparação comparada a intenção de entidade
A entidade representa uma palavra ou frase no interior utterance que pretende que sejam extraídos. Um utterance pode incluir muitas entidades ou none de todo. Uma entidade representa uma classe, incluindo uma coleção de objetos semelhantes (locais, coisas, pessoas, eventos ou conceitos). Entidades descrevem informações relevantes para o objetivo e, por vezes, são essenciais para a sua aplicação efetuar a tarefa. Por exemplo, uma aplicação de pesquisa de notícias pode incluir entidades, tais como "tópico", "origem", "palavra-chave" e "data de publicação", que são dados de chave para Procurar notícias de última hora. Uma aplicação de booking levar, o "localização", "data", "companhia aérea", "levar class" e "bilhetes" são informações da chave para booking voo (relevante para o objetivo de "Bookflight").

Por comparação, o objetivo representa a predição do utterance completo. 

## <a name="entities-represent-data"></a>Entidades representam dados
Entidades são dados que pretende solicitar a partir de utterance. Isto pode ser um nome, data, nome do produto ou qualquer grupo de palavras. 

|Expressão|Entidade|dados|
|--|--|--|
|Comprar 3 pedidos de suporte para a Nova Iorque|Número prebuilt<br>Location.Destination|3<br>Nova Iorque|
|Compre um pedido de Nova Iorque para Londres em 5 de Março|Location.Origin<br>Location.Destination<br>Prebuilt datetimeV2|Nova Iorque<br>Londres<br>5 de Março de 2018|

## <a name="entities-are-optional-but-highly-recommended"></a>Entidades são opcionais mas vivamente recomendado
Enquanto pendentes são necessários, entidades são opcionais. Não é necessário criar entidades para cada conceito na sua aplicação, mas apenas para necessária para a aplicação tomar medidas. 

Se o utterances não dispõe de detalhes da que sua bot tem de continuar, não terá de adicioná-los. Porque a sua aplicação evoluiu, pode adicioná-los mais tarde. 

Se não tem a certeza de como pretende utilizar as informações, adicione alguns entidades prebuilt comuns, tais como datetimeV2, ordinal, e-mail e o número de telefone.

## <a name="label-for-word-meaning"></a>Etiqueta de significado da palavra
Se a opção de palavra ou disposição word é o mesmo, mas não significa a mesma coisa, não identificá-lo com a entidade. 

As seguintes utterances, a palavra `fair` é um homograph. -Está escrito o mesmo, mas tem um significado diferente:

```
What kind of county fairs are happening in the Seattle area this summer?
Is the current rating for the Seattle review fair?
```

Se quisesse uma entidade de eventos para localizar todos os dados de eventos, Etiquetar o word `fair` o utterance primeiro, mas não no segundo.

## <a name="entities-are-shared-across-intents"></a>Entidades são partilhadas entre pendentes
Entidades são partilhadas entre pendentes. Não pertencem a qualquer objetivo único. Entidades e pendentes podem ser associadas a semanticamente mas não é uma relação exclusiva.

No utterance "livro-me um pedido para Paris", "Paris" é uma entidade de localização de tipo. Por reconhecer as entidades que são mencionadas na entrada do utilizador, LUIS ajuda-o a escolher as ações específicas para satisfazer o objetivo.

## <a name="assign-entities-in-none-intent"></a>Atribuir as entidades de intenção None
Todos os pendentes, incluindo o **nenhum** intenção, deve ter entidades com a etiqueta. Isto ajuda a LUIS saber mais sobre onde se encontram as entidades nos utterances e quais são palavras à volta de entidades. 

## <a name="types-of-entities"></a>Tipos de entidades
LUIS oferece muitos tipos de entidades; entidades prebuilt, máquina personalizada aprendidas entidades e entidades de lista.

| Nome | Pode etiqueta | Descrição |
| -- |--|--|
| **Prebuilt** <br/>[Personalizada](#prebuilt)| |  **Definição**<br>Tipos incorporados que representam os conceitos comuns. <br><br>**lista**<br/>número de expressão de chave, ordinal, temperatura, dimensão, dinheiro, idade, percentagem, e-mail, URL, número de telefone e expressão de chave. <br><br>Os nomes das entidades prebuilt estão reservados. <br><br>Todas as entidades prebuilt que são adicionadas à aplicação são devolvidas no [endpoint](luis-glossary.md#endpoint) consulta. Para obter mais informações, consulte [entidades Prebuilt](./Pre-builtEntities.md). <br/><br/>[Exemplo de resposta para a entidade](luis-concept-data-extraction.md#prebuilt-entity-data)|
|<!-- added week of 3/21/08 --> **Expressão regular**<br/>[RegEx](#regex)||**Definição**<br>Expressão regular personalizada para texto formatado utterance não processados. Ignora a maiúsculas e minúsculas e ignora variante cultural.  <br><br>Esta entidade é boa para palavras ou frases reconhecíveis que estejam formatados consistentemente com qualquer variação que também seja consistente.<br><br>Expressão regular de correspondência é aplicada após expansões de verificação ortográfica. <br><br>Se a expressão regular é demasiado complexa, por exemplo, utilizando parênteses Retos muitos, não é possível adicionar a expressão para o modelo. <br><br>**Exemplo**<br>`kb[0-9]{6,}` corresponde à kb123456.<br/><br/>[Início rápido](luis-quickstart-intents-regex-entity.md)<br>[Exemplo de resposta para a entidade](luis-concept-data-extraction.md)|
| **Simples** <br/>[Aprendeu de máquina](#machine-learned) | ✔ | **Definição**<br>Uma entidade simple é uma entidade genérica que descreve um único conceito e adquirida a partir do contexto aprendidas de máquina. Contexto incluem escolha do word, colocação do word e o comprimento de utterance.<br/><br/>Esta é uma boa entidade para palavras ou frases reconhecíveis que não estão formatados consistentemente mas indicam a mesma coisa. <br/><br/>[Início rápido](luis-quickstart-primary-and-secondary-data.md)<br/>[Exemplo de resposta para a entidade](luis-concept-data-extraction.md#simple-entity-data)|  
| **lista** <br/>[Correspondência exata](#exact-match)|| **Definição**<br>Lista de entidades representam um conjunto de fixo, fechado das palavras relacionados, juntamente com os respetivos synoymns no seu sistema. <br><br>Cada entidade de lista pode ter um ou mais formulários. Melhor utilizadas para um conjunto conhecido de variações no formas para representar o mesmo conceito.<br/><br/>LUIS não detetar valores adicionais para entidades da lista. Utilize a ver [dicionário semântico](luis-glossary.md#semantic-dictionary) para encontrar sugestões para novas palavras com base na lista atual.<br/><br>Se existir mais de uma entidade de lista com o mesmo valor, cada entidade é devolvida na consulta de ponto final. <br/><br/>[Início rápido](luis-quickstart-intent-and-list-entity.md)<br>[Exemplo de resposta para a entidade](luis-concept-data-extraction.md#list-entity-data)| 
| **Pattern.any** <br/>[Misto](#mixed) | ✔|**Definição**<br>Patterns.any é um marcador de posição de comprimento variável apenas utilizado na utterance de modelo de um padrão para marcar em que a entidade inicia e termina.  <br><br>**Exemplo**<br>Tendo em conta uma pesquisa de utterance para books com base no título, a pattern.any extrai o título concluído. É um utterance modelo utilizando pattern.any `Who wrote {BookTitle}[?]`.<br/><br/>[Tutorial](luis-tutorial-pattern.md)<br>[Exemplo de resposta para a entidade](luis-concept-data-extraction.md#composite-entity-data)|  
| **Compostos** <br/>[Aprendeu de máquina](#machine-learned) | ✔|**Definição**<br>Uma entidade composta é constituída por outras entidades, tais como entidades prebuilt e simples. As entidades separadas formam uma entidade completa. Entidades de lista não são permitidas em entidades compostas. <br><br>**Exemplo**<br>Uma entidade composta com o nome PlaneTicketOrder pode ter entidades subordinado prebuilt `number` e `ToLocation`. <br/><br/>[Tutorial](luis-tutorial-composite-entity.md)<br>[Exemplo de resposta para a entidade](luis-concept-data-extraction.md#composite-entity-data)|  
| **Hierárquica** <br/>[Aprendeu de máquina](#machine-learned) |✔ | **Definição**<br>Uma entidade hierárquica é uma categoria de entidades reconhecimento do contexto adquiridas.<br><br>**Exemplo**<br>Atribuído uma entidade hierárquica de `Location` com elementos subordinados `ToLocation` e `FromLocation`, cada subordinado pode ser determinado com base no **contexto** dentro de utterance. No utterance, `Book 2 tickets from Seattle to New York`, a `ToLocation` e `FromLocation` são reconhecimento do contexto diferentes com base em palavras em torno deles. <br/><br/>**Não utilize se**<br>Se estiver à procura de uma entidade que tem texto exato correspondências para crianças, independentemente de contexto, deve utilizar uma entidade de lista. Se estiver à procura de uma relação principal-subordinado com outros tipos de entidade, deve utilizar a entidade composta.<br/><br/>[Início rápido](luis-quickstart-intent-and-hier-entity.md)<br>[Exemplo de resposta para a entidade](luis-concept-data-extraction.md#hierarchical-entity-data)|

<a name="prebuilt"></a>
**Prebuilt** entidades são entidades personalizadas fornecidas pelo LUIS. Algumas destas entidades são definidas na open source [Recognizers texto](https://github.com/Microsoft/Recognizers-Text) projeto. Existem muitos [exemplos](https://github.com/Microsoft/Recognizers-Text/tree/master/Specs) no diretório /Specs para os idiomas suportados. Se o seu idioma específico ou entidade não é suportada atualmente, a contribuir para o projeto. 

<a name="machine-learned"></a>
**Aprendeu de máquina** entidades funcionam melhor quando testado através de [consultas de ponto final](luis-concept-test.md#endpoint-testing) e [rever utterances de ponto final](label-suggested-utterances.md). 

<a name="regex"></a>
**Entidades de expressão regular** são definidos por uma expressão regular, o utilizador fornece como parte da definição de entidade. 

<a name="exact-match"></a>
**Com correspondência exacta** entidades utilizam o fornecido na entidade de texto para tornar um texto exato corresponder.

<a name="mixed"></a>
**Misto** entidades utilizam uma combinação de métodos de deteção de entidade.

## <a name="entity-limits"></a>Limites de entidade
Reveja [limites](luis-boundaries.md#model-boundaries) para compreender quantos de cada tipo de entidade pode adicionar um modelo.

## <a name="entity-roles"></a>Funções de entidade
Entidade [funções](luis-concept-roles.md) são utilizados em padrões apenas. 

## <a name="composite-vs-hierarchical-entities"></a>Entidades hierárquica composto vs
Composto entidades e entidades hierárquicas tem relações principal-subordinado e são máquina aprendida. A aprendizagem automática permite LUIS compreender as entidades com base em diferentes contextos (disposição de palavras). Entidades compostas são mais flexíveis porque permitem que os tipos de entidade diferente como subordinados. Elementos subordinados de uma entidade hierárquica são apenas entidades simples. 

|Tipo|Objetivo|Exemplo|
|--|--|--|
|Hierárquica|Principal-subordinado de entidades simples|Location.Origin=New Iorque<br>Location.Destination=London|
|Compostos|Entidades de principal-subordinado: prebuilt, lista, simples, hierárquica| número = 3<br>lista = primeira classe<br>prebuilt.datetimeV2=March 5|

## <a name="data-matching-multiple-entities"></a>Várias entidades de correspondência de dados
Se uma palavra ou expressão corresponder a mais de uma entidade, a consulta de ponto final devolve cada entidade. Se adicionar prebuilt entidade número e prebuild datetimeV2 e ter um utterance `create meeting on 2018/03/12 for lunch with wayne`, LUIS reconhece todas as entidades e devolve uma matriz de entidades como parte da resposta de ponto final JSON: 

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
Se uma palavra ou expressão corresponder a mais de uma entidade de lista, a consulta de ponto final devolve cada entidade de lista.

Para a consulta `when is the best time to go to red rock?`, e a aplicação tiver o word `red` na lista de mais do que uma, LUIS reconhece todas as entidades e devolve uma matriz de entidades como parte da resposta de ponto final JSON: 

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

Poderá ter de utilizar entidades hierárquicas e compostas. Entidades hierárquicas refletem a relação entre as entidades que partilham características ou que são membros de uma categoria. As entidades de subordinados são todos os membros da categoria do respetivo principal. Por exemplo, uma entidade hierárquica PlaneTicketClass com o nome poderá ter as entidades de subordinados EconomyClass e FirstClass. A hierarquia abrange apenas um nível de profundidade.  

Entidades compostas representam partes de um todo. Por exemplo, uma entidade composta com o nome PlaneTicketOrder poderá ter de entidades de subordinados companhia aérea, destino, DepartureCity, DepartureDate e PlaneTicketClass. Criar uma entidade composta de pré-existente entidades simples, subordinados de entidades hierárquicas ou prebuilt entidades.  

LUIS também fornece o tipo de entidade de lista que não é máquina-aprendidas, mas permite à aplicação LUIS especificar uma lista de valores fixa. Consulte [LUIS limites](luis-boundaries.md) referência para rever os limites do tipo de entidade de lista. 

Se tiver considerados hierárquica e composto e a lista de entidades e ainda precisa de mais do que o limite, contacte o suporte. Para tal, recolher informações detalhadas sobre o sistema, visite o [LUIS] [ LUIS] Web site e, em seguida, selecione **suporte**. Se a sua subscrição do Azure inclui serviços de suporte, contacte [o suporte técnico do Azure](https://azure.microsoft.com/support/options/). 

## <a name="best-practices"></a>Melhores práticas

Criar um [entidade](luis-concept-entity-types.md) quando a aplicação de chamada ou bot necessita de alguns parâmetros ou dados de utterance necessária para executar uma ação. Uma entidade é uma palavra ou frase no utterance que precisa de extraídos – talvez como um parâmetro para uma função. 

Para selecionar o tipo correto de entidade para adicionar à sua aplicação, terá de saber como que dados são introduzidos por utilizadores. Cada tipo de entidade encontra-se utilizando um mecanismo de diferentes, tais como a lista de aprendizagem automática, fechada ou expressão regular. Se não souber, começar com uma entidade simple e etiqueta a palavra ou expressão que representa os dados em todos os utterances, em todos os pendentes, incluindo a intenção.  

Reveja utterances de ponto final regularmente para localizar utilização comum, onde uma entidade pode ser identificada como uma expressão regular ou não foi encontrada com uma correspondência exata de texto.  

Como parte de revisão, considere adicionar uma lista de expressão para adicionar um sinal para LUIS palavras ou frases reconhecíveis que são relevantes para o seu domínio, mas não são correspondências exatas e para o qual LUIS não tem uma confiança elevada.  

Consulte [melhores práticas](luis-concept-best-practices.md) para obter mais informações.

## <a name="next-steps"></a>Passos seguintes

Conheça os conceitos sobre boa [utterances](luis-concept-utterance.md). 

Consulte [adicionar entidades](luis-how-to-add-entities.md) para saber mais sobre como adicionar entidades à sua aplicação LUIS.

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website