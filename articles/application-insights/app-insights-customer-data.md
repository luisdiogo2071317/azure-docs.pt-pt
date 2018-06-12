---
title: Orientações para os dados pessoais armazenados no Azure Application Insights | Microsoft Docs
description: Este artigo descreve como gerir os dados pessoais armazenados no Azure Application Insights e os métodos para identificar e removê-lo.
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
ms.openlocfilehash: 95e421278b46015e761764792e11dec0351b9785
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294426"
---
# <a name="guidance-for-personal-data-stored-in-application-insights"></a>Orientações para os dados pessoais armazenados no Application Insights

Application Insights é um arquivo de dados em que são provavelmente encontrará dados pessoais. Este artigo aborda onde no Application Insights estes dados a geralmente encontrados, bem como as funcionalidades disponíveis para processar dados.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="strategy-for-personal-data-handling"></a>Estratégia para processar dados pessoais

Enquanto será até e da sua empresa, para determinar a estratégia de com o qual irá processar a privada dados (se todo), seguem-se algumas possíveis abordagens. São listados por ordem de preferência, um ponto de vista técnica da maioria de pelo menos, preferível para:
* Sempre que possível, parar a recolha de, obfuscate, anonymize ou, caso contrário, ajuste os dados estão a ser recolhidos para excluí-lo de que está a ser considerados _privada_. Este método é a abordagem preferida, poupando a necessidade de criar um estratégia de manuseamento de dados impactful e dispendiosos.
* Em que não é possível, tente normalizar os dados para reduzir o impacto no desempenho e a plataforma de dados. Por exemplo, em vez de registo de um ID de utilizador explícito, crie uma pesquisa de dados que irá correlacionar o nome de utilizador e os respetivos detalhes para um ID interno que, em seguida, pode ser registado noutro local. Dessa forma, se um dos seus utilizadores pedir-lhe para eliminar as suas informações pessoais, é possível que apenas a eliminação da linha na tabela de referência correspondente para o utilizador serão suficiente. 
* Por fim, se devem ser recolhidos dados privados, crie um processo em todo o caminho de API de remoção e o caminho de API de consulta existente para cumprir as obrigações que tiver em torno da exportação e eliminar quaisquer dados privados associados a um utilizador.

## <a name="where-to-look-for-private-data-in-application-insights"></a>Onde procurar dados privados no Application Insights?

Application Insights é um arquivo completamente flexível, permitindo ao prescribing um esquema aos seus dados substituir todos os campos com valores personalizados. Como tal, é impossível diga exatamente onde dados privada irão ser encontrados na sua aplicação específica. As localizações seguintes, no entanto, são bons pontos de partida no inventário:

* *Endereços IP*: enquanto o Application Insights irá obfuscate, por predefinição, todos os campos de endereço IP para "0.0.0.0", é um padrão razoavelmente comuns para este valor com o IP de utilizador reais para manter as informações da sessão de substituição. A consulta de análise abaixo pode ser utilizada para localizar qualquer tabela que contém os valores existentes na coluna do endereço IP diferente de "0.0.0.0" ao longo das últimas 24 horas:
    ```
    search client_IP != "0.0.0.0"
    | where timestamp > ago(1d)
    | summarize numNonObfuscatedIPs_24h = count() by $table
    ```
* *Os IDs de utilizador*: por predefinição, o Application Insights utilizará IDs gerados aleatoriamente para utilizador e o controlo de sessão. No entanto, é comum para ver estes campos substituídos-lo para armazenar um ID mais relevante para a aplicação. Por exemplo: nomes de utilizador, AAD GUIDs, etc. Estes IDs de muitas vezes, são consideradas como sendo no âmbito como dados pessoais e, por conseguinte, deve ser processada adequadamente. A nossa recomendação é sempre tentar obfuscate ou anonymize estes IDs. Campos onde estes valores normalmente são encontrados incluem session_Id, user_Id, user_AuthenticatedId, user_AccountId, bem como a customDimensions.
* *Dados personalizados*: Application Insights permite-lhe a acrescentar um conjunto de dimensões personalizadas para qualquer tipo de dados. Estas dimensões podem ser *qualquer* dados. Utilize a seguinte consulta para identificar quaisquer dimensões personalizadas recolhidas ao longo das últimas 24 horas:
    ```
    search * 
    | where isnotempty(customDimensions)
    | where timestamp > ago(1d)
    | project $table, timestamp, name, customDimensions 
    ```
* *Os dados na memória e em trânsito*: Application Insights irá monitorizar exceções, pedidos, chamadas de dependência e rastreios. Muitas vezes, podem ser recolhidos dados privados o código e o nível de chamada HTTP. Reveja as exceções, pedidos, dependências e tabelas de rastreios para identificar quaisquer esses dados. Utilize [inicializadores de telemetria](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling) sempre que possível obfuscate estes dados.
* *Capturas de depurador de instantâneos*: O [instantâneo depurador](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger) funcionalidade no Application Insights permite-lhe recolher os instantâneos de depuração, sempre que é detetada uma exceção na instância de produção da aplicação. Instantâneos irão expor o rastreio da pilha completa à esquerda para as exceções, bem como os valores de variáveis locais em cada passo na pilha. Infelizmente, esta funcionalidade não permite a eliminação seletiva de pontos de snap ou acesso programático para os dados dentro do instantâneo. Por conseguinte, se a taxa de retenção de instantâneo de predefinição não satisfaz os requisitos de conformidade, recomenda-se a desativar a funcionalidade.

## <a name="how-to-export-and-delete-private-data"></a>Como exportar e eliminar dados privados

É __vivamente__ recomendado, quando possível, ao reestruturar a política de recolha de dados para desativar a recolha de dados privados, obfuscating ou anonymizing, ou de outra forma de modificação para removê-lo do seja considerado como "privado". A processar os dados depois de este foi recolhido, irá resultar em custos para e a sua equipa para definir e automatizar uma estratégia, uma interface para os seus clientes interagir com os seus dados através de compilação e os custos de manutenção em curso. Além disso, é viáveis dispendiosa para o Application Insights e um grande volume de consulta em simultâneo ou chamadas de API de remoção tem o potencial de afetar negativamente todos os outros interação com a funcionalidade do Application Insights. Que consiga aceder tal, existem, de facto, alguns cenários válidos onde devem ser recolhidos dados privados. Para estes casos, os dados devem ser processados conforme descrito nesta secção.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

### <a name="view-and-export"></a>Ver e exportar

Para as ver e exportar pedidos de dados, o [API de consulta](https://dev.applicationinsights.io/quickstart) deve ser utilizada. Lógica para converter um adequado para fornecer aos seus utilizadores a forma dos dados serão até que implementar. [As funções do Azure](https://azure.microsoft.com/services/functions/) um excelente local para alojar esse lógica.

### <a name="delete"></a>Eliminar

> [!WARNING]
> Elimina no Application Insights é destrutivas e não reversível! Tenha muito cuidado na respetiva execução.

Efetuamos disponíveis como parte de um processamento de privacidade story um caminho de API "Remover". Este caminho deve ser utilizado com moderação devido aos riscos associados a fazê-lo, o potencial impacto do desempenho e o potencial de desfasamento de segurança de todas as agregações, medidas e outros aspetos dos seus dados do Application Insights. Consulte o [estratégia para processar dados pessoais](#strategy-for-personal-data-handling) secção acima para abordagens alternativas processar dados privados.

Remoção é uma operação altamente privilegiada que nenhuma aplicação ou o utilizador no Azure (incluindo mesmo o proprietário do recurso) têm permissões para executar sem explicitamente a ser concedidos uma função no Gestor de recursos do Azure. Esta função é _dados Purger_ e deve ser delegada cuidadosamente devido a possibilidade de perda de dados.

Assim que tiver sido atribuída a função do Azure Resource Manager, dois caminhos de API novo são documentação de programador disponível, completo e chamar forma ligada:

* [Remover POST](https://docs.microsoft.com/rest/api/application-insights/components/purge) - pega num objeto especificar parâmetros de dados, eliminar e devolve uma referência GUID
* Estado de remoção de GET - a chamada de remoção POST irá devolver um cabeçalho 'x-ms-Estado-location' que irão incluir um URL que pode ser chamada para determinar o estado da sua API de remoção. Por exemplo:
   ```
   x-ms-status-location: https://management.azure.com/subscriptions/[SubscriptionId]/resourceGroups/[ResourceGroupName]/providers/microsoft.insights/components/[ComponentName]/operations/purge-[PurgeOperationId]?api-version=2015-05-01
   ```

Embora a vasta maioria das operações de remoção de é concluída muito mais rápida do que o SLA, devido ao respetivo impacto nas pesada na plataforma de dados utilizado pelo Application Insights, o formal SLA para a conclusão das operações de remoção está definido no prazo de 30 dias.

## <a name="next-steps"></a>Passos seguintes
Para obter mais informações sobre como os dados são recolhidos, processados e protegidos, consulte o artigo [segurança de dados do Application Insights](app-insights-data-retention-privacy.md).