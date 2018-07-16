---
title: Documentação de orientação para os dados pessoais armazenados no Azure Application Insights | Documentos da Microsoft
description: Este artigo descreve como gerir dados pessoais armazenados no Azure Application Insights e os métodos para identificar e removê-lo.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 05/18/2018
ms.reviewer: Evgeny.Ternovsky
ms.author: mbullwin
ms.openlocfilehash: a59b57c546f18a7d91160f2ae7282af82fc42160
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39044718"
---
# <a name="guidance-for-personal-data-stored-in-application-insights"></a>Documentação de orientação para os dados pessoais armazenados no Application Insights

O Application Insights é um arquivo de dados em que são provavelmente encontrará os dados pessoais. Este artigo aborda onde no Application Insights esses dados geralmente se encontra, bem como os recursos disponíveis para lidar com esses dados.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="strategy-for-personal-data-handling"></a>Estratégia para a manipulação de dados pessoais

Enquanto é até e sua empresa, para determinar a estratégia com a qual irá lidar com a privada dados (se houver), seguem-se algumas abordagens possíveis. Eles estão listados por ordem de preferência de um ponto de vista técnico da maioria menos preferível para:
* Sempre que possível, parar a recolha de, oculte, tornar anónima ou ajustar de outra forma os dados a ser recolhidos para excluí-lo de que está a ser considerado _privada_. Este método, é a abordagem preferencial, eliminando a necessidade de criar uma estratégia de processamento de dados dispendiosos e impacto.
* Sempre que não é possível, tente normalizar os dados para reduzir o impacto sobre a plataforma de dados e o desempenho. Por exemplo, em vez de iniciar sessão um ID de utilizador explícitas, crie uma pesquisa aos dados que irão a correlacionar o nome de utilizador e os respetivos detalhes para uma ID interna que, em seguida, podem ser registadas em outro lugar. Dessa forma, se um dos seus utilizadores pedir-lhe para eliminar as suas informações pessoais, é possível que apenas a eliminação da linha na tabela de referência correspondente para o usuário será suficiente. 
* Por fim, se devem ser recolhidos dados privados, crie um processo em todo o caminho de API de remoção e o caminho de API de consulta existente para satisfazer quaisquer obrigações que tiver em torno de exportar e eliminar quaisquer dados privados associados a um utilizador.

## <a name="where-to-look-for-private-data-in-application-insights"></a>Onde procurar dados privados no Application Insights?

O Application Insights é um arquivo totalmente flexível, que, ao mesmo tempo que prescreve um esquema aos seus dados, permite-lhe substituir todos os campos com valores personalizados. Como tal, é impossível dizer exatamente onde os dados privados serão encontrados em seu aplicativo específico. As seguintes localizações, no entanto, são bons pontos de partida em seu inventário:

* *Endereços IP*: enquanto o Application Insights irá, por predefinição, oculte todos os campos de endereço IP como "0.0.0.0", é um padrão bastante comum para substituir este valor com o IP de utilizador reais para manter informações de sessão. A consulta do Analytics abaixo pode ser usada para encontrar qualquer tabela que contém os valores na coluna do endereço IP diferente de "0.0.0.0" nas últimas 24 horas:
    ```
    search client_IP != "0.0.0.0"
    | where timestamp > ago(1d)
    | summarize numNonObfuscatedIPs_24h = count() by $table
    ```
* *IDs de utilizador*: por predefinição, o Application Insights irá utilizar IDs gerados aleatoriamente para utilizador e o controlo de sessão. No entanto, é comum ver estes campos substituídos para armazenar um ID mais relevante para a aplicação. Por exemplo: nomes de utilizador, GUIDs do AAD, etc. Estes IDs de muitas vezes, são consideradas como dentro do âmbito como os dados pessoais e, portanto, devem ser manipuladas adequadamente. A nossa recomendação é sempre tentar ofuscar ou tornar anónima a estas identificações. Os campos onde estes valores estão normalmente presentes incluem session_Id, user_Id, user_AuthenticatedId, user_AccountId, bem como a customDimensions.
* *Dados personalizados*: Application Insights permite anexar um conjunto de dimensões personalizadas para qualquer tipo de dados. Essas dimensões podem ser *qualquer* dados. Utilize a seguinte consulta para identificar todas as dimensões personalizadas recolhidas ao longo das últimas 24 horas:
    ```
    search * 
    | where isnotempty(customDimensions)
    | where timestamp > ago(1d)
    | project $table, timestamp, name, customDimensions 
    ```
* *Os dados na memória e em trânsito*: Application Insights irá controlar exceções, pedidos, chamadas de dependência e rastreios. Muitas vezes, podem ser recolhidos dados privados no código e no nível de chamada HTTP. Reveja as exceções, pedidos, dependências e rastreios tabelas para identificar os dados. Uso [inicializadores de telemetria](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling) sempre que possível de ofuscar estes dados.
* *Capturas de depurador de instantâneos*: A [Snapshot Debugger](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger) recurso do Application Insights permite-lhe recolher instantâneos de depuração, sempre que uma exceção é capturada na instância de produção do seu aplicativo. Instantâneos irão expor o rastreio de pilha completa que leva às exceções, bem como os valores para variáveis locais em cada etapa na pilha. Infelizmente, esta funcionalidade não permite a eliminação seletiva de pontos de ajuste ou acesso programático aos dados dentro do instantâneo. Portanto, se a taxa de retenção de instantâneos padrão não satisfaz os requisitos de conformidade, a recomendação é desativar a funcionalidade.

## <a name="how-to-export-and-delete-private-data"></a>Como exportar e eliminar dados privados

É __vivamente__ recomendado, quando possível, para reestruturar sua política de recolha de dados para desativar a recolha de dados privadas, ofuscar ou anonymizing, ou de outra forma de modificá-la para removê-lo de que está a ser considerado como "private". Depois que ele tem sido recolhido, irá resultar em custos para e sua equipe para definir e automatizar uma estratégia, criar uma interface para os seus clientes interagir com os dados, através de custos de manutenção contínua e o tratamento de dados. Além disso, é computacionalmente dispendiosa do Application Insights e um grande volume de consultas em simultâneo ou chamadas de API de remoção têm o potencial de afetar negativamente todos os outros interação com a funcionalidade do Application Insights. Dito isso, há, de fato, alguns cenários válidos onde devem ser recolhidos dados privados. Nesses casos, os dados devem ser manipulados conforme descrito nesta secção.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

### <a name="view-and-export"></a>Ver e exportar

Para ambos ver e exportar pedidos de dados, o [API de consulta](https://dev.applicationinsights.io/quickstart) deve ser utilizado. Lógica para converter a forma dos dados a um adequado para fornecer aos seus utilizadores será deve implementar. [As funções do Azure](https://azure.microsoft.com/services/functions/) é um ótimo local para alojar essa lógica.

### <a name="delete"></a>Eliminar

> [!WARNING]
> Exclusões no Application Insights estão destrutiva e não reversível! Tenha muito cuidado em sua execução.

Tornamos disponíveis como parte de um tratamento de privacidade de história um caminho de API "Remover". Este caminho deve ser utilizado com moderação, devido ao risco associado ao fazer isso, o possível impacto de desempenho e o potencial de inclinar completo agregações, medições e outros aspectos dos seus dados do Application Insights. Consulte a [estratégia para a manipulação de dados pessoais](#strategy-for-personal-data-handling) secção acima para abordagens alternativas manipular dados privados.

Remoção é uma operação com privilégios bastante elevados que nenhuma aplicação ou o utilizador no Azure (incluindo até mesmo o proprietário do recurso) terão permissão para executar sem ser explicitamente concedido uma função no Azure Resource Manager. Esta função é _Purger dados_ e deve ser delegada com cuidado devido a potencial perda de dados.

Assim que tiver sido atribuída a função do Azure Resource Manager, dois novos caminhos de API são documentação de programador disponíveis, completo e chamam forma ligada:

* [Após remover](https://docs.microsoft.com/rest/api/application-insights/components/purge) – usa um objeto especificar parâmetros de dados para eliminar e retorna uma referência GUID
* Estado de remoção de GET - a chamada de remoção POST irá devolver um cabeçalho "x-ms-Estado-location", que irá incluir um URL que pode chamar para determinar o estado da sua API de remoção. Por exemplo:
   ```
   x-ms-status-location: https://management.azure.com/subscriptions/[SubscriptionId]/resourceGroups/[ResourceGroupName]/providers/microsoft.insights/components/[ComponentName]/operations/purge-[PurgeOperationId]?api-version=2015-05-01
   ```

> [!IMPORTANT]
>  Embora a grande maioria das operações de remoção pode ser muito mais rápida do que o SLA, devido a seu impacto pesado na plataforma de dados utilizado pelo Application Insights, concluída **o formal SLA para a conclusão de operações de remoção está definido em 30 dias**.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre como os dados são recolhidos, processados e protegidos, veja [segurança de dados do Application Insights](app-insights-data-retention-privacy.md).