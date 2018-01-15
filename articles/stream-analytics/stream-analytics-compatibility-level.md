---
title: "Compreenda o nível de compatibilidade para tarefas do Azure Stream Analytics. | Microsoft Docs"
description: "Saiba como definir um nível de compatibilidade para uma tarefa do Azure Stream Analytics e principais alterações no nível de compatibilidade mais recente"
keywords: "Nível de compatibilidade, transmissão em fluxo de dados"
documentationcenter: 
services: stream-analytics
author: SnehaGunda
manager: kfile
editor: 
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 01/03/2018
ms.author: sngun
ms.openlocfilehash: 0d89259d54fba0bd57881ec69cb61b5af6d603b5
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="compatibility-level-for-azure-stream-analytics-jobs"></a>Nível de compatibilidade para tarefas do Azure Stream Analytics
 
Nível de compatibilidade refere-se os comportamentos de versão específica de um serviço Azure Stream Analytics. O Azure Stream Analytics é um serviço gerido, com funcionalidade regular atualizações e melhoramentos de desempenho. Normalmente, as atualizações são automaticamente disponibilizadas aos utilizadores finais. No entanto, algumas funcionalidades novas poderá introduzir alterações principais tal como alteração no comportamento de uma tarefa existente, alterar em processos a consumir dados a partir destas tarefas, etc. Um nível de compatibilidade é utilizado para representar uma alteração principais introduzida no Stream Analytics. Alterações principais são sempre apresentadas com um novo nível de compatibilidade. 

Nível de compatibilidade certifica-se de que as tarefas existentes ser executadas sem qualquer falha. Quando cria uma nova tarefa de Stream Analytics, é uma melhor prática de criá-la utilizando o nível de compatibilidade mais recente que está disponível para si. 
 
## <a name="set-a-compatibility-level"></a>Definir um nível de compatibilidade 

Nível de compatibilidade controla o comportamento de tempo de execução de uma tarefa do stream analytics. Pode definir o nível de compatibilidade para uma tarefa de Stream Analytics, utilizando o portal ou utilizando o [criar chamada da REST API de tarefa](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job). O Azure Stream Analytics suporta atualmente dois compatibilidade níveis-"1.0" e "1.1". Por predefinição, o nível de compatibilidade é definido como "1.0" que foi introduzida durante a disponibilidade geral do Azure Stream Analytics. Para atualizar o valor predefinido, navegue até à sua tarefa do Stream Analytics existente > selecione o **nível de compatibilidade** opção **configurar** secção e altere o valor. 

Certifique-se de que parar a tarefa antes de atualizar o nível de compatibilidade. Não é possível atualizar o nível de compatibilidade, se a tarefa está num Estado em execução. 

![Nível de compatibilidade no portal](media\stream-analytics-compatibility-level/image1.png)

 
Quando atualizar o nível de compatibilidade, o compilador de T-SQL valida a tarefa com a sintaxe que corresponde ao nível de compatibilidade selecionado. 

## <a name="major-changes-in-the-latest-compatibility-level-11"></a>Principais alterações no nível de compatibilidade mais recente (1.1)

As seguintes alterações principais são introduzidas no nível de compatibilidade 1.1:

* **Formato XML de barramento de serviço**  

  * **versões anteriores:** Azure Stream Analytics utilizado DataContractSerializer, por isso o conteúdo da mensagem incluídos etiquetas XML. Por exemplo:
    
   @\u0006string\b3http://schemas.microsoft.com/2003/10/Serialization/\u0001{"SensorId": "1", "temperatura": 64\}\u0001 

  * **versão atual:** o conteúdo da mensagem contém a sequência diretamente com nenhuma etiquetas adicionais. Por exemplo:
  
   {"SensorId": "1", "temperatura": 64} 
 
* **Sensibilidade persistentes para nomes de campo**  

  * **versões anteriores:** nomes de campo foram alterados para inferior caso quando processadas pelo motor de Azure Stream Analytics. 

  * **versão atual:** sensibilidade é persistente para nomes de campo quando são processados pelo motor de Azure Stream Analytics. 
 
* **FloatNaNDeserializationDisabled**  

  * **versões anteriores:** comando CREATE TABLE não foi possível filtrar eventos com NaN (não numérico. Por exemplo, infinito, - infinito) numa coluna de número de vírgula FLUTUANTE escreva porque estão fora do intervalo documentado para estes números.

  * **versão atual:** CREATE TABLE permite-lhe especificar um esquema forte. O motor de Stream Analytics valida que os dados está em conformidade com neste esquema. Com este modelo, o comando pode filtrar eventos com valores NaN. 

* **Desative upcast automática para cadeias de datetime em JSON.**  

  * **versões anteriores:** parser o JSON seria cadeia automaticamente upcast valores com informações de data/fuso para DateTime escreva e, em seguida, convertê-lo para UTC. Isto resultou na perda de informações de fuso horário.

  * **versão atual:** há não mais automaticamente upcast dos valores de cadeia com informações de data/fuso ao tipo DateTime. Como resultado, as informações de fuso horário são mantidas. 

## <a name="next-steps"></a>Passos Seguintes
* [Guia de resolução de problemas para o Azure Stream Analytics](stream-analytics-troubleshooting-guide.md)
* [Painel de estado de funcionamento de recursos de análise de fluxo](stream-analytics-resource-health.md)