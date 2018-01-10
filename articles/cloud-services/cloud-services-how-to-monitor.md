---
title: "Monitorizar um serviço em nuvem do Azure | Microsoft Docs"
description: "Descreve o que monitorizar um serviço em nuvem do Azure envolve e que algumas das suas opções são."
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/22/2017
ms.author: adegeo
ms.openlocfilehash: c63a49c65f2d8261caa534308477888c752a89da
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2018
---
# <a name="introduction-to-cloud-service-monitoring"></a>Introdução à monitorização do serviço de nuvem

Pode monitorizar as métricas de chave de desempenho para qualquer serviço de nuvem. Cada função do serviço de nuvem recolhe dados mínimos: utilização de CPU, utilização de rede e utilização do disco. Se o serviço de nuvem tem o `Microsoft.Azure.Diagnostics` extensão aplicado a uma função, essa função pode recolher pontos adicionais de dados. Este artigo fornece uma introdução ao diagnóstico do Azure para serviços em nuvem.

Com a monitorização básica, os dados de contador de desempenho das instâncias de função é amostragem e recolhidos em intervalos de 3 minutos. Estes dados de monitorização básicos não são armazenados na sua conta de armazenamento e tiverem sem custos adicionais associados à mesma.

Com monitorização avançada, métricas adicionais são amostragem e recolhidas em intervalos de 5 minutos, 1 hora e 12 horas. Os dados agregados são armazenados numa conta do storage, nas tabelas e são removidos depois de 10 dias. A conta de armazenamento utilizada é configurada por função; Pode utilizar contas de armazenamento diferentes para diferentes funções. Este é configurado com uma cadeia de ligação no [. csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) e [. cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) ficheiros.


## <a name="basic-monitoring"></a>Monitorização básica

Conforme indicado na introdução, um serviço em nuvem recolhe automaticamente dados de monitorização básicos a partir da máquina virtual do anfitrião. Estes dados incluem a percentagem de CPU, rede/out e leitura/escrita de disco. Os dados de monitorização recolhidos é automaticamente apresentados nas páginas de descrição geral e as métricas do serviço de nuvem, no portal do Azure. 

A monitorização básica não necessita de uma conta de armazenamento. 

![monitorização de mosaicos do serviço de nuvem básico](media/cloud-services-how-to-monitor/basic-tiles.png)

## <a name="advanced-monitoring"></a>Monitorização avançada

Monitorização avançada envolve a utilização a extensão de diagnóstico do Azure (e opcionalmente o Application Insights SDK) na função de que pretende monitorizar. A extensão de diagnóstico utiliza um ficheiro de configuração (por função) com o nome **diagnostics.wadcfgx** para configurar as métricas de diagnóstico monitorizadas. Os dados de diagnóstico do Azure recolhe de extensão é armazenado numa conta de armazenamento do Azure, que está configurada no **.wadcfgx** e no [. csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) e [. cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) ficheiros. Isto significa que existe um extra custo associado avançadas de monitorização.

Como é criada a cada função, o Visual Studio adiciona a extensão de diagnóstico do Azure ao mesmo. Esta extensão pode recolher os seguintes tipos de informações:

* Contadores de desempenho personalizado
* Registos de aplicações
* Registos de eventos do Windows
* Origem de evento de .NET
* Registos do IIS
* O manifesto com base em ETW
* Informações de falhas
* Registos de erros do cliente

Enquanto são agregados todos os dados para a conta de armazenamento, o portal fornece uma forma de nativa para os dados de gráfico. Pode utilizar outro serviço, como o Application Insights, para correlacionar e apresentar os dados.

### <a name="use-application-insights"></a>Utilizar o Application Insights

Quando publica o serviço em nuvem do Visual Studio, é-lhe dada a opção para enviar os dados de diagnóstico para o Application Insights. Pode criar o recurso do Application Insights nessa altura ou enviar os dados para um recurso existente. O serviço em nuvem pode ser monitorizado pelo Application Insights para disponibilidade, desempenho, falhas e utilização. Gráficos personalizados podem ser adicionados ao Application Insights para que possa ver os dados que lhe interessam mais. Podem ser recolhidos dados de instância de função utilizando o Application Insights SDK no projeto de serviço em nuvem. Para obter mais informações sobre como integrar o Application Insights, consulte [Application Insights com serviços em nuvem](../application-insights/app-insights-cloudservices.md).

Tenha em atenção que ao utilizar o Application Insights para apresentar os contadores de desempenho (e outras definições) que especificou através da extensão de diagnóstico do Windows Azure, apenas irá obter uma experiência mais rica, integrando o Application Insights SDK para as funções de trabalho e web.


## <a name="add-advanced-monitoring"></a>Adicionar monitorização avançada

Primeiro, se não tiver um **clássico** conta do storage, [criar um](../storage/common/storage-create-storage-account.md#create-a-storage-account). Certifique-se a conta de armazenamento é criada com o **modelo de implementação clássica** especificado.

Em seguida, navegue para o **conta de armazenamento (clássica)** recursos. Selecione **definições** > **chaves de acesso** e copie o **cadeia de ligação principal** valor. Este valor é necessário para o serviço em nuvem. 

Existem dois ficheiros de configuração, tem de alterar para obter um diagnóstico avançado ativar a ser, **servicedefinition. Csdef** e **serviceconfiguration. Cscfg**. Provavelmente se tiver dois **. cscfg** ficheiros, um com o nome **Serviceconfiguration** para a implementação para o Azure e um com o nome **ServiceConfiguration.local.cscfg** que é utilizado para implementações de depuração local. Altere ambos os parâmetros.

No **servicedefinition. Csdef** ficheiro, adicione uma nova definição denominada `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` para cada função que utiliza o diagnóstico avançado. Visual Studio adiciona este valor para o ficheiro ao criar um novo projeto. No caso de este está em falta, pode adicioná-lo agora. 

```xml
<ServiceDefinition name="AnsurCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRoleWithSBQueue1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
```

Isto define uma nova definição tem de ser adicionada a cada **serviceconfiguration. Cscfg** ficheiro. Abra e altere cada **. cscfg** ficheiro. Adicionar uma definição denominada `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString`. Definir o valor para uma o **cadeia de ligação principal** da conta de armazenamento clássico ou para `UseDevelopmentStorage=true`, se pretender utilizar o armazenamento local no computador de desenvolvimento.

```xml
<ServiceConfiguration serviceName="AnsurCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="WorkerRoleWithSBQueue1">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=mystorage;AccountKey=KWwkdfmskOIS240jnBOeeXVGHT9QgKS4kIQ3wWVKzOYkfjdsjfkjdsaf+sddfwwfw+sdffsdafda/w==" />

<!-- or use the local development machine for storage
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
-->
```

## <a name="next-steps"></a>Passos Seguintes

- [Saiba mais sobre o Application Insights com serviços em nuvem.](../application-insights/app-insights-cloudservices.md)

