---
title: Formato de dados na API do serviço de exploração de dados de conhecimento | Microsoft Docs
description: Saiba mais sobre o formato dos dados no conhecimento exploração de serviço (KES) API nos serviços cognitivos.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: a763505ac6458d68df74ae73e71029b81202ec8b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351476"
---
# <a name="data-format"></a>Formato de dados
O ficheiro de dados descreve a lista de objetos para o índice.
Cada linha no ficheiro Especifica os valores de atributo de um objeto no [formato JSON](http://json.org/) com codificação UTF-8.
Para além de atributos definidos no [esquema](SchemaFormat.md), cada objeto tem um atributo de "logprob" opcional que especifica a probabilidade de registo relativo entre os objetos.
Quando o serviço devolve os objetos por ordem de diminuir a probabilidade, podemos utilizar "logprob" para indicar a devolução ordem dos objetos correspondentes.
Dada uma probabilidade *p* entre 0 e 1, pode ser calculada a probabilidade de registo correspondente como registo (*p*), onde log() é a função de registo natural.
Quando for especificado nenhum valor para logprob, é utilizado o valor predefinido 0.

```json
{"logprob":-5.3, "Title":"latent dirichlet allocation", "Year":2003, "Author":{"Name":"david m blei", "Affiliation":"uc berkeley"}, "Author":{"Name":"andrew y ng", "Affiliation":"stanford"}, "Author":{"Name":"michael i jordan", "Affiliation":"uc berkeley"}}
{"logprob":-6.1, "Title":"probabilistic latent semantic indexing", "Year":1999, "Author":{"Name":"thomas hofmann", "Affiliation":"uc berkeley"}}
...
```

Para atributos de cadeia, GUID e BLOBs, o valor é representado como uma cadeia delimitada por aspas de JSON.  Para atributos numéricos (Int32, Int64, dupla), o valor é representado como um número JSON.  Para composto atributos, o valor é um objeto JSON que especifica os valores de atributo de secundárias.  Para compilações de índice mais rápidas, presort os objetos, diminuindo a probabilidade de registo.

Em geral, um atributo poderá ter de 0 ou mais valores.  Se um atributo não tem nenhum valor, iremos basta removê-la do JSON.  Se um atributo tiver 2 ou mais valores, iremos pode repetir o par de valor de atributo do objeto de JSON.  Em alternativa, é possível atribuir uma matriz JSON que contém vários valores para o atributo.

```json
{"logprob":0, "Title":"0 keyword"}
{"logprob":0, "Title":"1 keyword", "Keyword":"foo"}
{"logprob":0, "Title":"2 keywords", "Keyword":"foo", "Keyword":"bar"}
{"logprob":0, "Title":"2 keywords (alt)", "Keyword":["foo", "bar"]}
```
