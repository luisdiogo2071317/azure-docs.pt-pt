---
title: Compreender o nível de compatibilidade para tarefas do Azure Stream Analytics
description: Saiba como configurar um nível de compatibilidade para uma tarefa do Azure Stream Analytics e as principais alterações no nível de compatibilidade mais recentes
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
manager: kfile
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 10/15/2018
ms.openlocfilehash: 661aa85a200d4eafa1c521b06f6e12378d61e93e
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49343593"
---
# <a name="compatibility-level-for-azure-stream-analytics-jobs"></a>Nível de compatibilidade para tarefas do Azure Stream Analytics
 
Nível de compatibilidade refere-se aos comportamentos específicos da versão de um serviço do Azure Stream Analytics. O Azure Stream Analytics é um serviço gerido, com as atualizações de funcionalidade regular e melhorias de desempenho. Normalmente, as atualizações são efetuadas automaticamente disponíveis aos utilizadores finais. No entanto, alguns recursos novos podem introduzir alterações importantes, tal como alteração no comportamento de uma tarefa existente, alterar nos processos de consumo de dados a partir destas tarefas, etc. Um nível de compatibilidade é utilizado para representar as alterações importantes introduzidas no Stream Analytics. Alterações importantes sempre são introduzidas com um novo nível de compatibilidade. 

Nível de compatibilidade certifica-se de que as tarefas existentes é executado sem qualquer falha. Quando cria uma nova tarefa de Stream Analytics, é melhor prática para criá-la utilizando o nível de compatibilidade mais recente que está disponível para. 
 
## <a name="set-a-compatibility-level"></a>Definir um nível de compatibilidade 

Nível de compatibilidade controla o comportamento de tempo de execução de uma tarefa do stream analytics. Pode definir o nível de compatibilidade de uma tarefa do Stream Analytics através do portal ou utilizando o [criar a chamada de REST API de tarefa](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job). O Azure Stream Analytics suporta atualmente dois compatibilidade níveis-"1.0" e "1.1". Por predefinição, o nível de compatibilidade é definido como "1.0" que foi introduzida durante a disponibilidade geral do Azure Stream Analytics. Para atualizar o valor predefinido, navegue para a sua tarefa do Stream Analytics existente > selecione o **nível de compatibilidade** opção **configurar** secção e altere o valor. 

Certifique-se de que parar a tarefa antes de atualizar o nível de compatibilidade. Não é possível atualizar o nível de compatibilidade, se o seu trabalho está num Estado em execução. 

![Nível de compatibilidade no portal](media\stream-analytics-compatibility-level/image1.png)

 
Ao atualizar o nível de compatibilidade, o compilador de T-SQL valida a tarefa com a sintaxe que corresponde ao nível de compatibilidade selecionado. 

## <a name="major-changes-in-the-latest-compatibility-level-11"></a>Grandes alterações no nível de compatibilidade mais recente (1.1)

As seguintes alterações principais foram introduzidas no nível de compatibilidade 1.1:

* **Formato XML do barramento de serviço**  

  * **versões anteriores:** Azure Stream Analytics utilizadas DataContractSerializer, para que o conteúdo da mensagem incluído marcas XML. Por exemplo:
    
   @\u0006string\b3http://schemas.microsoft.com/2003/10/Serialization/\u0001{ "SensorId": "1", "temperatura": 64\}\u0001 

  * **versão atual:** o conteúdo da mensagem contém o fluxo diretamente com nenhuma etiquetas adicionais. Por exemplo:
  
   {"SensorId": "1", "temperatura": 64} 
 
* **Sensibilidade persistente para nomes de campo**  

  * **versões anteriores:** nomes de campos foram alterados em minúsculas quando processadas pelo motor de Azure Stream Analytics. 

  * **versão atual:** sensibilidade é mantida para nomes de campo quando eles são processados pelo motor de Azure Stream Analytics. 

  > [!NOTE] 
  > Sensibilidade mantendo ainda não está disponível para tarefas de análise de Stream alojadas utilizando o ambiente do Edge. Como resultado, todos os nomes de campo são convertidos em minúsculas, se o seu trabalho está alojado no Edge. 

* **FloatNaNDeserializationDisabled**  

  * **versões anteriores:** comando CREATE TABLE não filtrar eventos com NaN (não numérico. Por exemplo, infinito, - Infinity) numa coluna de número de vírgula FLUTUANTE escreva por estarem fora do intervalo documentado para esses números.

  * **versão atual:** CREATE TABLE permite-lhe especificar um forte esquema. O motor do Stream Analytics valida que os dados estão em conformidade com esse esquema. Com esse modelo, o comando pode filtrar eventos com valores de NaN. 

* **Desative upcast automática para cadeias de caracteres de datetime em JSON.**  

  * **versões anteriores:** analisador o JSON seria a cadeia de caracteres automaticamente upcast valores com informações de data/hora/fuso para DateTime escreva e, em seguida, convertê-lo para UTC. Isso resultou em perda de informações de fuso horário.

  * **versão atual:** há não mais automaticamente upcast dos valores de cadeia de caracteres com informações de data/hora/fuso para o tipo DateTime. Como resultado, as informações de fuso horário são mantidas. 

## <a name="next-steps"></a>Passos Seguintes
* [Resolver problemas de entradas do Azure Stream Analytics](stream-analytics-troubleshoot-input.md)
* [Painel de estado de funcionamento de recursos de análise do Stream](stream-analytics-resource-health.md)