---
title: Orientações para dados pessoais armazenados no Log Analytics do Azure | Microsoft Docs
description: Este artigo descreve como gerir os dados pessoais armazenados no Log Analytics do Azure e os métodos para identificar e removê-lo.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: magoedte
ms.openlocfilehash: 056779943d05ca743db63f1bc91be058cfae7b30
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655585"
---
# <a name="guidance-for-personal-data-stored-in-log-analytics"></a>Orientações para os dados pessoais armazenados na análise de registos

Análise de registos é um arquivo de dados em que são provavelmente encontrará dados pessoais. Este artigo abordar onde na análise de registos esses dados a geralmente encontrados, bem como as funcionalidades disponíveis para processar esses dados.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="strategy-for-personal-data-handling"></a>Estratégia para processar dados pessoais

Enquanto será até e da sua empresa, para determinar a estratégia de com o qual irá processar a privada dados (se todo), seguem-se algumas possíveis abordagens. São listados por ordem de preferência, um ponto de vista técnica da maioria de pelo menos, preferível para:

* Sempre que possível, parar a recolha de, obfuscate, anonymize ou, caso contrário, ajuste os dados estão a ser recolhidos para excluí-lo do seja considerado como "privado". Este é _por até que ponto_ a abordagem preferida, poupando a necessidade de criar uma estratégia de processamento de dados muito dispendiosos e impactful.
* Em que não é possível, tente normalizar os dados para reduzir o impacto no desempenho e a plataforma de dados. Por exemplo, em vez de registo de um ID de utilizador explícito, crie um procurar dados que irão correlacionar os respetivos detalhes para um ID interno que, em seguida, pode ser registado noutro local e o nome de utilizador. Dessa forma, deve um dos seus utilizadores pedir-lhe para eliminar as suas informações pessoais, é possível que apenas a eliminação da linha na tabela de referência correspondente para o utilizador serão suficiente. 
* Por fim, se devem ser recolhidos dados privados, crie um processo em todo o caminho de API de remoção e o caminho de API de consulta existente para cumprir as obrigações que tiver em torno da exportação e eliminar quaisquer dados privados associados a um utilizador. 

## <a name="where-to-look-for-private-data-in-log-analytics"></a>Onde procurar dados privados na análise de registos?

Análise de registos é um arquivo flexível, permitindo ao prescribing um esquema aos seus dados substituir todos os campos com valores personalizados. Além disso, pode ser ingerida nenhum esquema personalizada. Como tal, é impossível diga exatamente onde dados privada irão ser encontrados na sua área de trabalho específica. As localizações seguintes, no entanto, são bons pontos de partida no inventário:

* *Endereços IP*: análise de registos recolhe uma variedade de informações de IP em várias tabelas diferentes. Por exemplo, a consulta seguinte mostra todas as tabelas em que os endereços IPv4 tenham sido recolhidos ao longo das últimas 24 horas:
    ```
    search * 
    | where * matches regex @'\b((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)(\.|$)){4}\b' //RegEx originally provided on https://stackoverflow.com/questions/5284147/validating-ipv4-addresses-with-regexp
    | summarize count() by $table
    ```
* *Os IDs de utilizador*: IDs de utilizador encontram-se numa grande variedade de soluções e tabelas. Pode procurar um nome de utilizador específico entre o conjunto de dados completo utilizando o comando de pesquisa:
    ```
    search "[username goes here]"
    ```
Não se esqueça de ter um aspeto apenas para nomes de utilizador legível por humanos, mas também GUIDs que diretamente podem ser rastreadas volta a um utilizador específico!
* *Os IDs de dispositivo*: como, por vezes, são consideradas "privado" IDs de utilizador, IDs de dispositivo. Utilizar a mesma abordagem, conforme indicado acima para IDs de utilizador, para identificar de tabelas em que esta situação pode ser uma preocupação. 
* *Dados personalizados*: análise de registos permite a recolha numa variedade de métodos: registos personalizados e campos personalizados, o [API de Recoletor de dados de HTTP](log-analytics-data-collector-api.md) , e dados personalizados recolhido como parte de registos de eventos do sistema. Todos estes são suscetíveis a que contém dados privada e devem ser examinados para verificar a existem de dados.
* *Os dados capturados de solução*: como o mecanismo de solução é um open-ended, é recomendável rever todas as tabelas geradas por soluções para garantir a conformidade.

## <a name="how-to-export-and-delete-private-data"></a>Como exportar e eliminar dados privados

Tal como mencionado no [estratégia para processar dados pessoais](#strategy-for-personal-data-handling) secção anterior, é __vivamente__ recomendado se todos os possível, reestruturar a política de recolha de dados para desativar a recolha de dados privados, obfuscating ou anonymizing-lo ou, caso contrário, modificando-o para removê-lo do seja considerado como "privado". A processar o dados foremost resultará numa custos para e a sua equipa para definir e automatizar uma estratégia, uma interface para os seus clientes interagir com os seus dados através de compilação e os custos de manutenção em curso. Além disso, é viáveis dispendiosa para análise de registos e um grande volume de consulta em simultâneo ou chamadas de API de remoção tem o potencial de afetar negativamente todos os outros interação com a funcionalidade de análise de registos. Que consiga aceder tal, existem, de facto, alguns cenários válidos onde devem ser recolhidos dados privados. Para estes casos, os dados devem ser processados conforme descrito nesta secção.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

### <a name="view-and-export"></a>Ver e exportar

Para as ver e exportar pedidos de dados, o [API de consulta](https://dev.loganalytics.io/) deve ser utilizada. Lógica para converter um adequado para fornecer aos seus utilizadores a forma dos dados serão até que implementar. [As funções do Azure](https://azure.microsoft.com/services/functions/) faz com que um excelente local para alojar esse lógica.

### <a name="delete"></a>Eliminar

> [!WARNING]
> Elimina na análise de registos é destrutivas e não reversível! Tenha muito cuidado na respetiva execução.

Efetuamos disponíveis como parte do processamento de privacidade um *remover* caminho de API. Este caminho deve ser utilizado com moderação devido aos riscos associados a fazê-lo, o potencial impacto do desempenho e o potencial de desfasamento de segurança de todas as agregações, medidas e outros aspetos dos seus dados de análise de registos. Consulte o [estratégia para processar dados pessoais](#strategy-for-personal-data-handling) na secção abordagens alternativas processar dados privados.

Remoção é uma operação altamente privilegiada que nenhuma aplicação ou o utilizador no Azure (incluindo mesmo o proprietário do recurso) têm permissões para executar sem explicitamente a ser concedidos uma função no Gestor de recursos do Azure. Esta função é _dados Purger_ e deve ser delegada cautiously devido a possibilidade de perda de dados. 

Assim que tiver sido atribuída a função do Azure Resource Manager, dois caminhos de API novos estão disponíveis: 

* [Após a remoção] (https://docs.microsoft.com/rest/api/loganalytics/workspaces%202015-03-20/purge) - pega num objeto especificar parâmetros de dados, eliminar e devolve uma referência GUID 
* Estado de remoção de GET - a chamada de remoção POST irá devolver um cabeçalho 'x-ms-Estado-location' que irão incluir um URL que pode ser chamada para determinar o estado da sua API de remoção. Por exemplo:

    ```
    x-ms-status-location: https://management.azure.com/subscriptions/[SubscriptionId]/resourceGroups/[ResourceGroupName]/providers/Microsoft.OperatonalInsights/workspaces/[WorkspaceName]/operations/purge-[PurgeOperationId]?api-version=2015-03-20
    ```

Enquanto esperamos a vasta maioria das operações de remoção para concluir muito mais rápida do que o nosso SLA, devido ao respetivo impacto nas pesada na plataforma de dados utilizado pela análise de registos, o formal SLA para a conclusão das operações de remoção está definido no prazo de 30 dias. 

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre como os dados são recolhidos, processados e protegidos, consulte o artigo [segurança de dados de análise de registos](log-analytics-data-security.md).