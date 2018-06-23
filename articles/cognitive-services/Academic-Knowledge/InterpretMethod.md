---
title: Interpretar método na API de conhecimento académico | Microsoft Docs
description: Utilize o método de Interpret para repor interpretações formatadas das cadeias de consulta do utilizador com base em dados de gráfico académico e a gramática académico nos serviços cognitivos da Microsoft.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: a46c792f14fabf6562666d1067ef880bd505741f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351451"
---
# <a name="interpret-method"></a>Interpretar método

O **interpretar** REST API assume um utilizador final a cadeia de consulta (ou seja, uma consulta introduzida por um utilizador da sua aplicação) e devolve formatado interpretações da intenção do utilizador com base nos dados de gráfico académico e a gramática académico.

Para fornecer uma experiência interativa, é possível chamar este método repetidamente após cada caráter introduzido pelo utilizador. Nesse caso, deverá definir o **concluída** parâmetro como 1 para ativar as sugestões de conclusão automática. Se a aplicação não necessitar de conclusão automática, deve definir o **concluída** parâmetro como 0.

**Ponto final REST:**

    https://westus.api.cognitive.microsoft.com/academic/v1.0/interpret?

## <a name="request-parameters"></a>Parâmetros do Pedido

Nome     | Valor | Necessário?  | Descrição
---------|---------|---------|---------
**query**    | Cadeia de texto | Sim | Consulta introduzida pelo utilizador.  Se concluir estiver definido como 1, consulta será interpretada como um prefixo para gerar sugestões de conclusão automática de consulta.        
**modelo**    | Cadeia de texto | Não  | Nome do modelo que pretende consultar.  Atualmente, o valor será assumida a *mais recente*.        
**concluir** | 0 ou 1 | Não<br>predefinição: 0  | 1 significa que as sugestões de conclusão automática são gerados com base nos dados de gramática e do graph.         
**Contagem**    | Number | Não<br>predefinição: 10 | Número máximo de interpretações para devolver.         
**deslocamento**   | Number | Não<br>predefinição: 0  | Índice da interpretação primeiro a devolver. Por exemplo, *contagem = 2 & Desvio = 0* devolve interpretações 0 e 1. *contagem = 2 & Desvio = 2* devolve interpretações 2 e 3.       
**tempo limite**  | Number | Não<br>predefinição: 1000 | Tempo limite em milissegundos. Apenas interpretações encontradas antes do tempo limite decorreu são devolvidas.
<br>
  
## <a name="response-json"></a>Resposta (JSON)
Nome     | Descrição
---------|---------
**query** |O *consulta* parâmetro do pedido.
**interpretações** |Uma matriz de 0 ou mais diferentes formas de correspondência de intervenção do utilizador em relação a gramática.
**.logprob interpretações [x]**  |A probabilidade de registo natural relativo da interpretação. Valores maiores estejam mais predispostos.
**.parse interpretações [x]**  |Uma cadeia XML que mostra a forma como cada parte da consulta foi interpretado.
**.rules interpretações [x]**  |Uma matriz de 1 ou mais regras definidas na gramática que foram invocada durante a interpretação. Para a API de conhecimento académico, existe será sempre 1 regra.
**interpretações [x]. nome de .rules [y]**  |Nome da regra.
**interpretações [.output .rules [y] x]**  |Resultado da regra.
**interpretações [.output.type .rules [y] x]** |O tipo de dados de saída da regra.  Para a API de conhecimento académico, sempre será "consulta".
**interpretações [.output.value .rules [y] x]**  |A saída da regra. Para a API de conhecimento académico, esta é uma cadeia de expressão de consulta que pode ser transmitida para métodos de avaliar e calchistogram.
**abortada** | TRUE se o pedido excedeu o tempo limite.

<br>
#### <a name="example"></a>Exemplo:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/interpret?query=papers by jaime&complete=1&count=2
 ```
<br>A resposta abaixo contém dois parte superior (devido ao parâmetro *contagem = 2*) interpretações mais prováveis que conclua a entrada de utilizador parcial *papers por jaime*: *papers por jaime teevan*  e *papers por verde jaime*.  As conclusões de consulta de serviço gerado em vez de apenas a considerar exacta correspondências para o autor *jaime* porque o pedido especificado *concluída = 1*. Tenha em atenção que o valor canónico *j l verde* correspondência através o sinónimo *verde jamie*, conforme indicado na análise.


```JSON
{
  "query": "papers by jaime",
  "interpretations": [
    {
      "logprob": -12.728,
      "parse": "<rule name=\"#GetPapers\">papers by <attr name=\"academic#AA.AuN\">jaime teevan</attr></rule>",
      "rules": [
        {
          "name": "#GetPapers",
          "output": {
            "type": "query",
            "value": "Composite(AA.AuN=='jaime teevan')"
          }
        }
      ]
    },
    {
      "logprob": -12.774,
      "parse": "<rule name=\"#GetPapers\">papers by <attr name=\"academic#AA.AuN\" canonical=\"j l green\">jaime green</attr></rule>",
      "rules": [
        {
          "name": "#GetPapers",
          "output": {
            "type": "query",
            "value": "Composite(AA.AuN=='j l green')"
          }
        }
      ]
    }
  ]
}
```  
<br>Para obter resultados de entidade para uma interpretação, utilize *output.value* do **interpretar** API e passar que para o **avaliar** API através de *expr*  parâmetro. Neste exemplo, a consulta para a primeira interpretação é: 
```
evaluate?expr=Composite(AA.AuN=='jaime teevan')
```
 