---
title: Formato de esquema na API do serviço de exploração de dados de conhecimento | Microsoft Docs
description: Saiba mais sobre o formato de esquema no conhecimento exploração de serviço (KES) API nos serviços cognitivos.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 3009392a5acb12a8f4df3d30a2cbe5e74f2172fc
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351727"
---
# <a name="schema-format"></a>Formato de esquema
O esquema está especificado num ficheiro JSON que descreve a estrutura de atributo dos objetos no ficheiro de dados utilizado para criar o índice.  Para cada atributo, o esquema Especifica o nome, tipo de dados, operations opcionais e lista de sinónimos opcional.  Um objeto pode ter 0 ou mais valores de cada atributo.  Segue-se um exemplo de um domínio de publicação académico simplificado:

``` json
{
  "attributes":[
    {"name":"Title", "type":"String"},
    {"name":"Year", "type":"Int32"},
    {"name":"Author", "type":"Composite"},
    {"name":"Author.Id", "type":"Int64", "operations":["equals"]},
    {"name":"Author.Name", "type":"String"},
    {"name":"Author.Affiliation", "type":"String"},
    {"name":"Keyword", "type":"String", "synonyms":"Keyword.syn"}
  ]
}
```

Os nomes de atributos são sensíveis a maiúsculas identificadores que começar com uma letra e incluir apenas letras (A-Z), números (0-9) e caráter de sublinhado (\_).  O atributo reservado "logprob" é utilizado para especificar as probabilidades de registo natural relativo entre objetos.

## <a name="attribute-type"></a>Tipo de Atributo
Segue-se uma lista de tipos de dados de atributos suportado:

| Tipo | Descrição | Operações | Exemplo |
|------|-------------|------------|---------|
| Cadeia | Cadeia (1 a 1024 carateres) | é igual a, starts_with | "Olá mundo" |
| Int32 | Número inteiro com sinal de 32 bits | é igual a, starts_with, is_between | 2016 |
| Int64 | Número inteiro com sinal de 64 bits | é igual a, starts_with, is_between | 9876543210 |
| Valor de duplo | Valor de vírgula flutuante de dupla precisão | é igual a, starts_with, is_between | 1.602E-19 |
| Date | Data (1400-01-01 para 9999-12-31) | é igual a, is_between | ' 2016-03-14' |
| GUID | Identificador exclusivo global | é igual a | "602DD052-CC47-4B23-A16A-26B52D30C05B" |
| Blobs | Internamente não indexada comprimido | *Nenhum* | "Atribuir a cada pessoa em cada organização no planet alcançar mais" |
| Compostos | Composição de vários atributos secundárias| *N/D* | {"Name": "harry shum", "Afiliação": "microsoft"} |

Atributos de cadeia são utilizados para representar os valores de cadeia que podem ser apresentadas como parte da consulta de utilizador.  Suportam a correspondência exacta *é igual a* operação, bem como o *starts_with* operação para cenários de conclusão de consulta, tais como correspondência "micros" com "microsoft".  Sensível e difusa correspondente para processar erros de ortografia será suportado numa versão futura.

Int64/Int32/duplo atributos são utilizados para representar os valores numéricos.  O *is_between* operação ativa o suporte de inequality (lt le, gt, ge) em tempo de execução.  O *starts_with* operação suporta cenários de conclusão de consulta, tais como correspondentes com "2016", "20" ou "3". com "3.14".

Os atributos de data são utilizados de forma eficiente codificar valores de data.  O *is_between* operação ativa o suporte de inequality (lt le, gt, ge) em tempo de execução.
  
GUID de atributos são utilizados para representar eficientemente valores GUID com suporte de predefinido para o *é igual a* operação.

Atributos de blob são utilizados de forma eficiente codificar blobs potencialmente grande de dados para a pesquisa de tempo de execução do objeto correspondente, sem suporte para qualquer operação de indexação com base no conteúdo dos valores de blob.

### <a name="composite-attributes"></a>Atributos compostos
Atributos compostos são utilizados para representar um agrupamento de valores de atributo.  O nome de cada atributo secundárias começa com o nome do atributo composto seguido por ".".  Os valores de atributos compostos estão especificados como um objeto JSON que contém os valores de atributo aninhada.  Atributos compostos podem ter vários valores de objeto.  No entanto, os atributos compostos poderão não ter secundárias atributos que são próprios atributos compostos.

O exemplo de publicação académico acima, isto permite que o serviço ao consultar papers por "harry shum" enquanto ele é "Microsoft".  Sem composto atributos, o serviço apenas pode consultar papers onde um dos autores é "harry shum" e um dos autores é "Microsoft".  Para obter mais informações, consulte [consultas composto](SemanticInterpretation.md#composite-function).

## <a name="attribute-operations"></a>Operações de atributo
Por predefinição, cada atributo está indexado para suportar todas as operações disponíveis para o tipo de dados do atributo.  Se não for necessária uma operação específica, o conjunto de operações indexadas pode ser especificado explicitamente para reduzir o tamanho do índice.  No seguinte fragmento do esquema de exemplo acima, o atributo Author.Id está indexado para suportar apenas o *é igual a* operação, mas não adicionais *starts_with* e *is_between*  operações para atributos de Int32.
```json
{"name":"Author.Id", "type":"Int32", "operations":["equals"]}
```

Quando um atributo é referenciado no interior de uma gramática a *starts_with* operação tem de ser especificado no esquema por ordem para o serviço gerar conclusões de consultas parciais.  

## <a name="attribute-synonyms"></a>Atributo sinónimos
Muitas vezes, é necessário para fazer referência a um valor de atributo de cadeia específica por um sinónimo.  Por exemplo, os utilizadores podem mencionar "Microsoft" como "MSFT" ou "MS".  Nestes casos, a definição do atributo pode especificar o nome de um ficheiro de esquema localizado no mesmo diretório do ficheiro de esquema.  Cada linha no ficheiro sinónimo representa uma entrada de sinónimo no seguinte formato JSON: `["<canonical>", "<synonym>"]`.  O esquema de exemplo, "AuthorName.syn" é um ficheiro JSON que contém valores de sinónimo para o atributo Author.Name.

`{"name":"Author.Name", "type":"String", "synonyms":"AuthorName.syn"}`


Um valor canónico pode ter vários sinónimos.  Vários valores canónicos podem partilhar um sinónimo comuns.  Nestes casos, o serviço irá resolver a ambiguidade através de vários interpretações.  Abaixo está um exemplo de ficheiro de sinónimos AuthorName.syn correspondente para o esquema acima:
```json
["harry shum","heung-yeung shum"]
["harry shum","h shum"]
["henry shum","h shum"]
...
```
