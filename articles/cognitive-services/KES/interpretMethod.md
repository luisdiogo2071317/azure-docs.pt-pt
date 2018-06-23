---
title: Interpretar método na API do serviço de exploração de dados de conhecimento | Microsoft Docs
description: Saiba como utilizar o método Interpret no conhecimento exploração de serviço (KES) API nos serviços cognitivos.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: ef68d98dacf393abf8d030b9312217ea380947d2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351715"
---
# <a name="interpret-method"></a>Interpretar método
O *interpretar* método assume uma cadeia de consulta de linguagem natural e devolve formatado interpretações da intenção do utilizador com base nos dados de gramática e no índice.  Para fornecer uma experiência de pesquisa interativa, este método pode ser chamado como cada caráter é introduzido pelo utilizador com o *concluída* parâmetro definido como 1 para ativar as sugestões de conclusão automática.

## <a name="request"></a>Pedir
`http://<host>/interpret?query=<query>[&<options>]`

Nome|Valor| Descrição
----|----|----
consulta    | Cadeia de texto | Consulta introduzida pelo utilizador.  Se concluir estiver definido como 1, consulta será interpretada como um prefixo para gerar sugestões de conclusão automática de consulta.        
concluir | 0 (predefinição) ou 1 | 1 significa que as sugestões de conclusão automática são gerados com base nos dados de gramática e no índice.         
count    | Número (predefinição = 10) | Número máximo de interpretações para devolver.         
deslocamento   | Número (predefinição = 0) | Índice da interpretação primeiro a devolver.  Por exemplo, *contagem = 2 & Desvio = 0* devolve interpretações 0 e 1. *contagem = 2 & Desvio = 2* devolve interpretações 2 e 3.       
tempo limite  | Número (predefinição = 1000) | Tempo limite em milissegundos. Apenas interpretações encontradas antes do tempo limite decorreu são devolvidas.

Utilizar o *contagem* e *desvio* parâmetros, um grande número de resultados pode ser obtido a incremental através de vários pedidos.

## <a name="response-json"></a>Resposta (JSON)
JSONPath     | Descrição
---------|---------
$.query |*consulta* parâmetro do pedido.
$.interpretations   |Matriz de 0 ou mais formas para corresponder a consulta de entrada relativamente a gramática.
$.interpretations [\*] .logprob   |Probabilidade de registo relativo da interpretação (< = 0).  Estejam mais predispostos valores superiores.
$.interpretations [\*] .parse |Cadeia XML que mostra a forma como cada parte da consulta foi interpretado.
$.interpretations [\*] .rules |Matriz de 1 ou mais regras definidas na gramática invocada durante a interpretação.
$.interpretations [\*] .rules [\*]. nome    |Nome da regra.
$.interpretations [\*] .rules [\*] .output  |Saída semântica da regra.
$.interpretations [\*] .rules [\*]. output.type |Tipo de dados de saída semântica.
$.interpretations [\*] .rules [\*]. output.value|Valor de saída semântica.  
$.aborted | TRUE se o pedido excedeu o tempo limite.

### <a name="parse-xml"></a>Analisar o XML
A análise XML annotates a consulta (concluída) com informações sobre como corresponde contra as regras de gramática e atributos no índice.  Segue-se um exemplo do domínio publicações académico:

```xml
<rule name="#GetPapers">
  papers by 
  <attr name="academic#Author.Name" canonical="heungyeung shum">harry shum</attr>
  <rule name="#GetPaperYear">
    written in
    <attr name="academic#Year">2000</attr>
  </rule>
</rule>
```

O `<rule>` elemento delimits o intervalo de consulta correspondente a regra especificada pelo respetivo `name` atributo.  Pode ser aninhada quando a análise envolve referências de regra de gramática.

O `<attr>` elemento delimits o intervalo de consulta correspondente o atributo de índice especificado pelo respetivo `name` atributo.  Quando a correspondência envolve um sinónimo da consulta de entrada, o `canonical` atributo irá conter o valor canónico o sinónimo desde o índice de correspondência.

## <a name="example"></a>Exemplo
No exemplo académico publicações, o pedido seguinte devolve até 2 sugestões de conclusão automática para a consulta de prefixo "papers por jaime":

`http://<host>/interpret?query=papers by jaime&complete=1&count=2`

A resposta contém dois parte superior ("contagem = 2") interpretações mais prováveis que concluir a consulta parcial "papers por jaime": "papers por jaime teevan" e "papers por verde jaime".  As conclusões de consulta de serviço gerado em vez de apenas a considerar exacta correspondências para o autor "jaime", porque o pedido especificado "concluída = 1". Tenha em atenção que o valor canónico "j l verde" correspondido através o sinónimo "verde jamie", conforme indicado na análise.


```json
{
  "query": "papers by jaime",
  "interpretations": [
    {
      "logprob": -5.615,
      "parse": "<rule name=\"#GetPapers\">papers by <attr name=\"academic#Author.Name\">jaime teevan</attr></rule>",
      "rules": [
        {
          "name": "#GetPapers",
          "output": {
            "type": "query",
            "value": "Composite(Author.Name=='jaime teevan')"
          }
        }
      ]
    },
    {
      "logprob": -5.849,
      "parse": "<rule name=\"#GetPapers\">papers by <attr name=\"academic#Author.Name\" canonical=\"j l green\">jaime green</attr></rule>",
      "rules": [
        {
          "name": "#GetPapers",
          "output": {
            "type": "query",
            "value": "Composite(Author.Name=='j l green')"
          }
        }
      ]
    }
  ]
}
```  

Quando o tipo de saída semântica é "consulta", tal como neste exemplo, os objetos correspondentes podem ser obtidos através da transmissão *output.value* para o [ *avaliar* ](evaluateMethod.md) API através de *expr* parâmetro.

`http://<host>/evaluate?expr=Composite(AA.AuN=='jaime teevan')`
  
