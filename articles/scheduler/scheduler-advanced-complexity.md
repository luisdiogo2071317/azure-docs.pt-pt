---
title: Criar agendas complexas e periodicidade avançada com o agendador do Azure
description: Saiba como criar agendas complexas e periodicidade avançada com o agendador do Azure.
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: ''
ms.assetid: 5c124986-9f29-4cbc-ad5a-c667b37fbe5a
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 4293442e13fc4bae871b1f32a3ed4231d9f32632
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
ms.locfileid: "29692339"
---
# <a name="build-complex-schedules-and-advanced-recurrence-with-azure-scheduler"></a>Criar agendas complexas e periodicidade avançada com o agendador do Azure

As principais de uma tarefa do agendador do Azure é a agenda. A agenda determina quando e como programador executa a tarefa. 

Pode utilizar o programador para definir várias agendas periódicas e única para uma tarefa. As agendas de uma única acionados uma vez a uma hora especificada. As agendas de uma única eficazmente são agendamentos periódicos que executar apenas uma vez. Agendamentos periódicos acionados numa frequência predeterminada.

Esta flexibilidade, pode utilizar o Programador de uma ampla variedade de cenários empresariais:

* **Limpeza de dados periódica**. Por exemplo, todos os dias, elimine todos os tweets que são mais antigas do que três meses.
* **Arquivar**. Por exemplo, todos os meses, push fatura histórico para um serviço de cópia de segurança.
* **Pedidos de dados externos**. Por exemplo, a cada 15 minutos, solicitar a um novo relatório de Meteorologia ski partir NOAA.
* **Processamento de imagem**. Por exemplo, cada dia da semana, horas de ponta, utilize a comprimir imagens que foram carregadas nesse dia de informática na cloud.

Neste artigo, iremos guiá-as tarefas de exemplo que pode criar através do programador. Podemos fornecer os dados JSON que descreve cada agenda. Se utilizar o [API REST do agendador](https://msdn.microsoft.com/library/mt629143.aspx), pode utilizar este mesmo JSON para [criar uma tarefa do agendador](https://msdn.microsoft.com/library/mt629145.aspx).

## <a name="supported-scenarios"></a>Cenários suportados
Os exemplos neste artigo mostram o leque de cenários que o agendador suporta. Os exemplos amplamente ilustram como criar agendas de muitos padrões de comportamento, incluindo:

* Execute uma vez uma data e hora específicas.
* Execute e repetir um número específico de vezes.
* Executar imediatamente e repetir.
* Executar e repetir cada  *n*  minutos, horas, dias, semanas ou meses, começando uma hora específica.
* Execute e repetir uma frequência semanal ou mensal, mas apenas em dias específicos da semana ou em dias específicos do mês.
* Execute e repetir várias vezes num período. Por exemplo, a última sexta-feira em último feira de cada mês, ou 5 15 da Manhã e nas 17:15:00 todos os dias.

## <a name="date-and-date-time"></a>Data e a data / hora
Data referências siga de trabalhos do programador do [ISO 8601 especificação](http://en.wikipedia.org/wiki/ISO_8601)e incluir apenas a data.

Siga as referências de data / hora nas tarefas do programador a [ISO 8601 especificação](http://en.wikipedia.org/wiki/ISO_8601)e incluem a data e hora. Uma data / hora em que não especifica um desvio UTC é prestada UTC.  

## <a name="use-json-and-the-rest-api-to-create-a-schedule"></a>Utilizar JSON e a API REST para criar uma agenda
Para criar uma agenda básica utilizando o [API REST do agendador](https://msdn.microsoft.com/library/mt629143), primeiro [registar a sua subscrição com um fornecedor de recursos](https://msdn.microsoft.com/library/azure/dn790548.aspx). O nome do fornecedor do programador é **Microsoft.Scheduler**. Em seguida, [criar uma coleção de tarefas](https://msdn.microsoft.com/library/mt629159.aspx). Por fim, [criar uma tarefa](https://msdn.microsoft.com/library/mt629145.aspx). 

Quando cria uma tarefa, pode especificar o agendamento e de periodicidade utilizando JSON, como esta excerpt:

    {
        "startTime": "2012-08-04T00:00Z", // Optional
         …
        "recurrence":                     // Optional
        {
            "frequency": "week",     // Can be "year", "month", "day", "week", "hour", or "minute"
            "interval": 1,                // How often to fire
            "schedule":                   // Optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]                      
            },
            "count": 10,                  // Optional (default to recur infinitely)
            "endTime": "2012-11-04",      // Optional (default to recur infinitely)
        },
        …
    }

## <a name="job-schema-basics"></a>Noções básicas do esquema de tarefa
A tabela seguinte fornece uma descrição dos elementos principais que utilizar para definir periodicidade e agendar uma tarefa de alto nível:

| Nome JSON | Descrição |
|:--- |:--- |
| **startTime** |Um valor de data / hora. Para agendas básicas, **startTime** é a primeira ocorrência. Para agendas complexas, a tarefa é iniciada não define que **startTime**. |
| **recurrence** |Especifica regras de periodicidade para a tarefa e a periodicidade em que a tarefa é executada. O objeto de periodicidade suporta os elementos **frequência**, **intervalo**, **endTime**, **contagem**, e **agenda**. Se **periodicidade** estiver definida, **frequência** é necessária. Outros **periodicidade** elementos são opcionais. |
| **frequency** |Uma cadeia que representa a unidade de frequência com que a tarefa de voltar a ocorrer. Os valores suportados são "minutos", "horas", "dia", "semanas" e "mês". |
| **interval** |Um número inteiro positivo. **intervalo** indica o intervalo para o **frequência** valor que determina a frequência a execução da tarefa. Por exemplo, se **intervalo** é 3 e **frequência** é "semanas", a tarefa de voltar a ocorrer cada três semanas.<br /><br />Agendador suporta um máximo **intervalo** de 18 para a frequência de mensal, 78 para a frequência de semanal e 548 para a frequência diária. Para a hora e a frequência de minuto, o intervalo suportado é 1 < = **intervalo** < = 1000. |
| **endTime** |Uma cadeia que especifica a data / hora para além dos quais não executar a tarefa. Pode definir um valor para **endTime** que está no passado. Se **endTime** e **contagem** não são especificado, a tarefa será executada infinitamente. Não é possível incluir os **endTime** e **contagem** na mesma tarefa. |
| **count** |Um número inteiro positivo (maior que zero) que especifica o número de vezes que a tarefa será executada antes de ser concluída.<br /><br />**contagem** representa o número de vezes que a tarefa é executada antes de determinar que está a ser concluído. Por exemplo, para uma tarefa que é executada diariamente com um **contagem** do 5 e uma data de início da segunda, a tarefa é concluída após a execução no sexta-feira. Se a data de início é no passado, a primeira execução é calculada desde o momento de criação.<br /><br />Se não **endTime** ou **contagem** for especificado, a tarefa será executada infinitamente. Não é possível incluir os **endTime** e **contagem** na mesma tarefa. |
| **schedule** |Uma tarefa com uma frequência especificada altera a periodicidade com base numa agenda de periodicidade. A **agenda** valor contém modificações com base em minutos, horas, dias da semana, os dias do mês e o número da semana. |

## <a name="job-schema-defaults-limits-and-examples"></a>As predefinições de esquema de tarefa, os limites e exemplos
O artigo, vamos discutir de cada um dos seguintes elementos detalhadamente:

| Nome JSON | Tipo de valor | Necessário? | Valor predefinido | Valores válidos | Exemplo |
|:--- |:--- |:--- |:--- |:--- |:--- |
| **startTime** |string |Não |Nenhum |ISO 8601 de tempos de data |`"startTime" : "2013-01-09T09:30:00-08:00"` |
| **recurrence** |objeto |Não |Nenhuma |O objeto de periodicidade |`"recurrence" : { "frequency" : "monthly", "interval" : 1 }` |
| **frequency** |string |Sim |Nenhum |"minutos", "horas", "dia", "semanas", "meses" |`"frequency" : "hour"` |
| **interval** |número |Sim |Nenhuma |1 a 1000 |`"interval":10` |
| **endTime** |string |Não |Nenhum |Valor de data / hora que representa uma hora no futuro |`"endTime" : "2013-02-09T09:30:00-08:00"` |
| **count** |número |Não |Nenhum |>= 1 |`"count": 5` |
| **schedule** |objeto |Não |Nenhuma |O objeto de agenda |`"schedule" : { "minute" : [30], "hour" : [8,17] }` |

## <a name="deep-dive-starttime"></a>Descrição aprofundada: startTime
A tabela seguinte descreve como **startTime** controla a forma que uma tarefa é executada:

| valor de startTime | Sem periodicidade | Periodicidade, nenhuma agenda | Periodicidade com agenda |
|:--- |:--- |:--- |:--- |
| **Hora de início não** |Execute imediatamente uma vez. |Execute imediatamente uma vez. Execute as execuções subsequentes calculadas a partir do último tempo de execução. |Execute imediatamente uma vez.<br /><br />Execute as execuções subsequentes com base numa agenda de periodicidade. |
| **Hora de início no passado** |Execute imediatamente uma vez. |Calcular o tempo de execução futuro primeiro após a hora de início e executadas a essa hora.<br /><br />Execute as execuções subsequentes calculadas a partir do último tempo de execução. <br /><br />Para obter mais informações, consulte o exemplo que se segue nesta tabela. |Tarefa começa *não sooner que* a hora de início especificada. A primeira ocorrência é baseada na agenda calculada a partir da hora de início.<br /><br />Execute as execuções subsequentes com base numa agenda de periodicidade. |
| **Hora de início no futuro ou a hora atual** |Execute uma vez à hora de início especificada. |Execute uma vez à hora de início especificada.<br /><br />Execute as execuções subsequentes calculadas a partir do último tempo de execução.|Tarefa começa *não sooner que* a hora de início especificada. A primeira ocorrência é com base numa agenda, calculada a partir da hora de início.<br /><br />Execute as execuções subsequentes com base numa agenda de periodicidade. |

Vamos ver um exemplo do que acontece quando **startTime** está no passado, com periodicidade, mas com nenhuma agenda.  Partem do princípio de que a hora atual é 2015-04-08 13:00, **startTime** é 2015-04-07 14:00, e **periodicidade** é dois dias (definido com **frequência**: dia e **intervalo**: 2.) Tenha em atenção que **startTime** está no passado e ocorre antes da hora atual.

Nas seguintes condições, a primeira execução será no 2015-04-09 em 14:00\. O motor do Scheduler calcula as ocorrências de execução a partir da hora de início. Quaisquer instâncias no passado são eliminadas. O motor utiliza a instância seguinte que ocorre no futuro. Neste caso, **startTime** é 2015-04-07 2:00 PM, pelo que a instância seguinte é dois dias a esse tempo, o que é 2015-04-09 às 2:00 PM.

Tenha em atenção que a primeira execução será o mesmo se **startTime** é 2015-04-05 14:00 ou 2015-04-01 14:00\. Após a primeira execução, as execuções subsequentes são calculadas com base na agenda. Estes serão no 2015-04-11 às 2:00 PM, em seguida, no 2015-04-13 às 2:00 PM, em seguida, no 2015-04-15 em 2:00 PM e assim sucessivamente.

Por fim, quando uma tarefa tem uma agenda, se horas e minutos não se encontram definidos na agenda, a predefinição de valores para as horas e minutos da primeira execução, respetivamente.

## <a name="deep-dive-schedule"></a>Descrição detalhada da: agenda
Pode utilizar **agenda** para *limite* o número de execuções de tarefa. Por exemplo, se uma tarefa com um **frequência** "mês" tem uma agenda que é executado apenas no dia 31, a tarefa é executada apenas nos meses que tenham um prazo de trinta primeiro dia.

Também pode utilizar **agenda** para *expanda* o número de execuções de tarefa. Por exemplo, se uma tarefa com um **frequência** "mês" tem uma agenda que é executada em dias do mês, 1 e 2, a tarefa é executada nos dias do mês em vez de apenas uma vez primeiro e segundo um mês.

Se especificar vários elementos de agendamento, a ordem de avaliação é desde a maior à mais pequena: número da semana, dia do mês, dia da semana, horas e minutos.

A tabela seguinte descreve os elementos de agenda detalhadamente:

| Nome JSON | Descrição | Valores válidos |
|:--- |:--- |:--- |
| **minutes** |Minutos da hora em que a tarefa é executada. |Uma matriz de números inteiros. |
| **hours** |Horas do dia em que a tarefa é executada. |Uma matriz de números inteiros. |
| **weekDays** |Dias da semana a tarefa é executada. Pode ser especificado apenas com uma frequência semanal. |Uma matriz de qualquer um dos seguintes valores (tamanho máximo de matriz é 7):<br />-"Segunda-feira"<br />-"Terça-feira"<br />-"Quarta-feira"<br />-"Quinta-feira"<br />-"Sexta-feira"<br />-"Sábado"<br />-"Domingo"<br /><br />Não maiúsculas e minúsculas. |
| **monthlyOccurrences** |Determina os dias do mês a execução da tarefa. Pode ser especificado apenas com uma frequência mensal. |Uma matriz de **monthlyOccurrences** objetos:<br /> `{ "day": day, "occurrence": occurrence}`<br /><br /> **dia** é o dia da semana a tarefa é executada. Por exemplo, *{Domingo}* é cada Domingo do mês. Necessário.<br /><br />**ocorrência** é a ocorrência de dias durante o mês. Por exemplo, *Domingo, -1* é Domingo último do mês. Opcional. |
| **monthDays** |Dia do mês que a tarefa é executada. Pode ser especificado apenas com uma frequência mensal. |Uma matriz dos seguintes valores:<br />-Qualquer valor < = -1 e > =-31<br />-Qualquer valor > = 1 e < = 31|

## <a name="examples-recurrence-schedules"></a>Exemplos: As agendas de periodicidade
Os exemplos seguintes mostram vários agendas de periodicidade. Os exemplos focar-se o objeto de agenda e respetivos subelementos.

Estas agendas partem do princípio de que **intervalo** está definido para 1\. Os exemplos também presumem correto **frequência** valores para os valores existentes na **agenda**. Por exemplo, não é possível utilizar um **frequência** de "dia" e têm um **dias do mês** modificação no **agenda**. Iremos descrevem estas restrições anteriormente no artigo.

| Exemplo | Descrição |
|:--- |:--- |
| `{"hours":[5]}` |Execute 5 AM todos os dias.<br /><br />Programador corresponde à cópia de segurança cada valor na "horas", com cada valor em "minutos", um por um, para criar uma lista de todas as vezes que a tarefa é executada. |
| `{"minutes":[15], "hours":[5]}` |Executar todos os dias às 5:15. |
| `{"minutes":[15], "hours":[5,17]}` |Executar todos os dias às 5:15 e 17:15. |
| `{"minutes":[15,45], "hours":[5,17]}` |Executar todos os dias às 5:15, 5:45, 17:15 e 17:45. |
| `{"minutes":[0,15,30,45]}` |Executar de 15 em 15 minutos. |
| `{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` |Executar hora a hora.<br /><br />Esta tarefa é executada a cada hora. O minuto é controlado pelo valor para **startTime**, se for especificado. Se não **startTime** valor for especificado, o minuto é controlado pelo tempo de criação. Por exemplo, se a hora de início ou a hora de criação (que se aplica) for 12:25 PM, a tarefa será executada às 00:25, 01:25, 02:25,..., 23:25.<br /><br />A agenda é equivalente a uma tarefa com um **frequência** de "horas", uma **intervalo** de 1 e não **agenda** valor. A diferença é que pode utilizar esta agenda com diferentes **frequência** e **intervalo** valores para criar outras tarefas. Por exemplo, se **frequência** é "mês", o agendamento executa uma só vez por mês, em vez de todos os dias (se **frequência** é "dia"). |
| `{minutes:[0]}` |Executar de hora a hora, à hora certa.<br /><br />Esta tarefa também é executada a cada hora, mas na hora (12 AM, 1 AM, 2 AM e assim sucessivamente). Isto é equivalente a uma tarefa com um **frequência** de "horas", um **startTime** valor de zero minutos e não **agenda**, se a frequência é de "dia". No entanto, se o **frequência** é "semana" ou "mês", a agenda executa apenas um dia, uma semana ou um dia, mês, respetivamente. |
| `{"minutes":[15]}` |Execute em 15 minutos decorridos desde a hora a cada hora.<br /><br />É executada a cada hora, começando às 00:15, 1:15:00, 2:15 AM, e assim sucessivamente. Termina em 23:15:00. |
| `{"hours":[17], "weekDays":["saturday"]}` |Execute as de 17: 00 no Sábado todas as semanas. |
| `{hours":[17], "weekDays":["monday", "wednesday", "friday"]}` |Execute as de 17: 00 na segunda-feira, Quartas e sexta-feira todas as semanas. |
| `{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` |Executar às 17:15 e 17:45 de segunda-feira, quarta-feira e sexta-feira todas as semanas. |
| `{"hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}` |Execute 5 AM e 5 PM na segunda-feira, Quartas e sexta-feira todas as semanas. |
| `{"minutes":[15,45], "hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}` |Executar em 5:15 AM, 5:45 AM, as 17:15:00 e as 17:45:00 na segunda-feira, Quartas e sexta-feira todas as semanas. |
| `{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` |Executar de 15 em 15 minutos nos dias de semana. |
| `{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` |Execute a cada 15 minutos em dias da semana, entre as 09: 00 e 4:45 PM. |
| `{"weekDays":["sunday"]}` |Execute em Sundays na hora de início. |
| `{"weekDays":["tuesday", "thursday"]}` |Execute no executado às Terças e às quintas-hora de início. |
| `{"minutes":[0], "hours":[6], "monthDays":[28]}` |Executar às 6: 00 no dia eighth vinte e de cada mês (pressupondo que um **frequência** "mês"). |
| `{"minutes":[0], "hours":[6], "monthDays":[-1]}` |Execute às 6: 00 no último dia do mês.<br /><br />Se pretender executar uma tarefa no último dia de um mês, utilize -1 em vez de dia 28, 29, 30 ou 31. |
| `{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` |Execute às 6: 00 no primeiro e último dia de cada mês. |
| `{monthDays":[1,-1]}` |Execute no primeiro e último dia de cada mês da hora de início. |
| `{monthDays":[1,14]}` |Execute no primeiro e fourteenth dia de cada mês da hora de início. |
| `{monthDays":[2]}` |Execute no segundo dia do mês na hora de início. |
| `{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` |Execute o primeiro sexta-feira de cada mês às 5 AM. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` |Execute o primeiro sexta-feira de cada mês da hora de início. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` |Execute o terceiro sexta-feira do fim do mês, todos os meses, a hora de início. |
| `{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` |Executar na primeira e última sextas-feiras de cada mês às 5:15. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` |Execute na primeira e última sexta-feira de cada mês na hora de início. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` |Execute o quinto sexta-feira de cada mês da hora de início.<br /><br />Se não houver nenhuma sexta-feira quinta num mês, a tarefa não é executado. Poderá considerar a utilização -1 em vez de 5 para a ocorrência se pretender executar a tarefa em ocorrer última sexta-feira do mês. |
| `{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` |Executar de 15 em 15 minutos na última sexta-feira do mês. |
| `{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` |Executar às 5:15, 5:45, 17:15 e 17:45 na terceira quarta-feira de cada mês. |

## <a name="see-also"></a>Consulte também

- [O que é o Scheduler?](scheduler-intro.md)
- [Conceitos, terminologia e hierarquia de entidades do Azure Scheduler](scheduler-concepts-terms.md)
- [Começar a utilizar o Scheduler no portal do Azure](scheduler-get-started-portal.md)
- [Planos e faturação no Azure Scheduler](scheduler-plans-billing.md)
- [Referência da API REST do Azure Scheduler](https://msdn.microsoft.com/library/mt629143)
- [Referência de cmdlets do PowerShell do Azure Scheduler](scheduler-powershell-reference.md)
- [Agendador do Azure elevado disponibilidade e fiabilidade](scheduler-high-availability-reliability.md)
- [Limites, predefinições e códigos de erro do Azure Scheduler](scheduler-limits-defaults-errors.md)
- [Autenticação de saída do Azure Scheduler](scheduler-outbound-authentication.md)

