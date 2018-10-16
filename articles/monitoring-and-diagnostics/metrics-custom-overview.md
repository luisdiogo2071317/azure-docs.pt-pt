---
title: Métricas personalizadas no Azure Monitor
description: Saiba mais sobre métricas personalizadas no Azure Monitor e como eles são modelados.
author: ancav
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: 1bdf1e1f5e58ecb0939d5876e0cef349e32de517
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49344756"
---
# <a name="custom-metrics-in-azure-monitor"></a>Métricas personalizadas no Azure Monitor

Como implementar recursos e aplicações no Azure, deseja iniciar a recolha de telemetria para obter informações sobre o desempenho e estado de funcionamento. Azure disponibiliza algumas métrica out-of-the-box como implementar recursos. Eles são chamados de métricas standard ou plataforma. No entanto, estas métricas são limitadas por natureza. Pode pretender recolher alguns indicadores de desempenho personalizados ou específicos do negócio métricas para fornecer informações mais detalhadas.
Estas métricas "personalizadas" podem ser coletadas por meio da telemetria da sua aplicação, um agente em execução em seus recursos do Azure ou até mesmo fora para dentro de sistema de monitorização e enviado diretamente para o Azure Monitor. Depois de publicado para o Azure Monitor, pode navegar, consultar e alertar relativamente a métricas personalizadas para os seus recursos do Azure e aplicações lado a lado métricas padrão, emitidas pelo Azure.

## <a name="send-custom-metrics"></a>Enviar métricas personalizadas
Métricas personalizadas podem ser enviadas para o Azure Monitor através de uma variedade de métodos.
- Instrumente a sua aplicação com o SDK do Application Insights e enviar telemetria personalizada para o Azure Monitor 
- Instalar a extensão de diagnóstico do Windows no seu [VM do Azure](metrics-store-custom-guestos-resource-manager-vm.md), [conjunto de dimensionamento de Máquina Virtual](metrics-store-custom-guestos-resource-manager-vmss.md), [VM clássica](metrics-store-custom-guestos-classic-vm.md), ou [serviço Cloud clássico](metrics-store-custom-guestos-classic-cloud-service.md)e enviar os contadores de desempenho para o Azure Monitor 
- Instalar o [InfluxDB Telegraf agente](metrics-store-custom-linux-telegraf.md) na sua VM Linux do Azure e as métricas de envio com o plug-in do Azure Monitor de saída
- Enviar métricas personalizadas [diretamente a API de REST do Azure Monitor](metrics-store-custom-rest-api.md) https://<azureregion>.monitoring.azure.com/<AzureResourceID>/metrics

Ao enviar métricas personalizadas para o Azure Monitor, cada ponto de dados (ou valor) comunicou tem de incluir as seguintes informações:

### <a name="authentication"></a>Autenticação
Para submeter métricas personalizadas para o Azure Monitor a entidade submeter a métrica tem de ter um token do Azure Active Directory válido no cabeçalho de "Bearer" do pedido. Existem algumas formas de suporte de adquirir um token de portador válido:
1. [Gerido identidades para recursos do Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) -fornece uma identidade para um recurso de Azure (por exemplo, uma VM). MSI foi projetado para fornecer recursos de permissões para executar determinadas operações – por exemplo, permitindo que um recurso emitir métricas sobre si próprio. Um recurso (ou seu MSI) pode ser concedida permissões de "Editor de métricas de monitorização" outro recurso, permitindo o MSI emitir métricas para outros recursos também.
2. [Principal de serviço do AAD](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals) -o cenário aqui é uma aplicação do AAD (serviço) pode receber permissões para emitir métricas sobre um recurso do Azure.
Para autenticar o pedido, o Azure Monitor valida o token de aplicação com chaves públicas do AAD. A função de "Editor de métricas de monitorização" existente já tem esta permissão, o que está disponível no portal do Azure. O principal de serviço, consoante os recursos que irão emitir métricas personalizadas, pode ser atribuído a função de "Editor de métricas de monitorização" no âmbito necessário (subscrição, grupo de recursos ou recurso específico).

> [!NOTE]
> Quando pedir um token do AAD para emitir métricas personalizadas garantir que é o público-alvo/recurso o token está a ser requerido para https://monitoring.azure.com/ (não se esqueça de incluir à direita '/')

### <a name="subject"></a>Requerente
Esta propriedade captura o ID de recurso do Azure a métrica personalizada é comunicada para. Estas informações serão codificadas no URL da chamada à API feita. Cada API só pode enviar valores de métrica para um único recurso do Azure.

> [!NOTE]
> Não é possível emitir métricas personalizadas com o ID de recurso de um grupo de recursos ou subscrição.
>
>

### <a name="region"></a>Região
Esta propriedade captura em que região do Azure, o recurso de que emissão de métricas para está implementado. Métricas tem de ser emitidas para o Azure Monitor mesmo ponto final regional que a região do recurso está implementado na. Por exemplo, métricas personalizadas para uma VM são implementadas na região E.U.A. oeste têm de ser enviadas para o WestUS regionais do Azure Monitor ponto final. As informações de região também estão codificadas no URL da chamada à API.

> [!NOTE]
> Durante a pré-visualização pública métricas personalizadas só está disponível num subconjunto de regiões do Azure. Uma lista de regiões suportadas é documentada numa secção posterior deste artigo.
>
>

### <a name="timestamp"></a>Carimbo de data/hora
Cada ponto de dados enviado para o Azure Monitor deve ser marcado com um carimbo. Este timestamp captura a datetime em que o valor da métrica foi medido/recolhidos. O Azure Monitor irá aceitar dados métricos carimbos de data máximo 20 minutos no passado e até 5 minutos no futuro.

### <a name="namespace"></a>Espaço de Nomes
Espaços de nomes são uma forma de categorizar ou agrupar métricas semelhante. Espaços de nomes permitem-lhe alcançar isolamento entre grupos de métricas, que podem coletar informações diferentes ou indicadores de desempenho. Por exemplo, poderia ter um namespace chamado *ContosoMemoryMetrics* isto é utilizado track memória utilize as métricas que a aplicação e outro namespace chamado de perfil *ContosoAppTransaction* que monitoriza todos os métricas sobre transações de utilizador na sua aplicação.

### <a name="name"></a>Nome
O nome da métrica que está a ser comunicado. Normalmente, o nome é descritivo o bastante para ajudar a identificar o que está a ser medido. Por exemplo, uma métrica que é medir o número de bytes de memória que está a ser utilizado de uma determinada VM pode ter um nome de métrica como "Bytes no uso da memória".

### <a name="dimension-keys"></a>Chaves de dimensão
Uma dimensão é um par de chave/valor que o ajuda a descrever as características adicionais sobre a métrica a ser recolhida. As características adicionais permitem recolher mais informações sobre a métrica que permite a informações mais detalhadas. Por exemplo, a métrica "Bytes no uso da memória" poderia ter uma chave de dimensão chamada "Processo", que captura a quantos bytes de memória que cada processo numa VM está a consumir. Isto permite-lhe filtrar a métrica para ver quanto processos específicos de memória estão a utilizar ou para identificar os processos de 5 principais por utilização da memória.
Cada métrica personalizada pode ter até 10 dimensões.

### <a name="dimension-values"></a>Valores da Dimensão
Quando um ponto de dados métrico, para cada chave de dimensão em métrica está a ser comunicada, a geração de relatórios tem um valor de dimensão correspondente. Por exemplo, se quiser comunicar a memória utilizada pelas ContosoApp na sua VM:

* O nome da métrica seria *Bytes de memória em utilização*
* A chave de dimensão seria *processo*
* O valor de dimensão seria *ContosoApp.exe*

Ao publicar um valor de métrica, só pode especificar um valor de dimensão único por chave de dimensão. Se a recolher a mesma utilização de memória para vários processos na VM, pode relatar vários valores de métrica para esse timestamp. Cada valor de métrica de especificar um valor de dimensão diferente para a chave de dimensão do processo.

### <a name="metric-values"></a>Valores de métrica
Monitor do Azure armazena todas as métricas em intervalos de granularidade de um minuto. Estamos cientes de que uma métrica poderá ter de amostragem várias vezes (ex. Utilização da CPU) ou mensuradas para muitos eventos discretos (ex. Inicie sessão no latências de transação) durante um determinado minuto. Para limitar o número de valores não processados, que terá de emitir e pague no Azure Monitor, pode pré-agregar localmente e emitir os valores:

* Mínimo: O mínimo observado o valor de todas as amostras/medidas durante o minuto
* Máx.: O número máximo observados valor de todas as amostras/medidas durante o minuto
* Soma: A soma de todos os valores de todas as/medições de exemplo observados durante o minuto
* Contagem: O número de amostras/medidas tomadas durante o minuto

Por exemplo, se houver 4 transações início de sessão para a sua aplicação durante um dado um minuto e resultante foram medidas latências para cada:

|Transação 1|Transação 2|Transação 3|Transação 4|
|---|---|---|---|
|7 ms|4 ms|13 ms|16 ms|
|

A publicação de métrica resultante para o Azure Monitor seria:
* Mínimo: 4
* Máx.: 16
* Soma: 40
* Contagem: 4

Se seu aplicativo não é possível de pré-agregar localmente e tem de emitir cada exemplo discreto ou evento imediatamente após a coleção, pode emitir os valores de medida não processados.
Por exemplo, sempre que uma transação de início de sessão ocorreu na sua aplicação que pretende publicar uma métrica para o Azure Monitor com apenas uma única medida. Portanto, para uma transação de início de sessão demorou 12 ms, em seguida, métrica publicação, seria:
* Mínimo: 12
* Máx.: 12
* Soma: 12
* Contagem: 1

Este processo permite-lhe emitir vários valores para a mesma métrica + a combinação de dimensão durante um determinado minuto. O Azure Monitor, em seguida, irá demorar todos os valores não processados emitidos para um determinado minuto e agregá-las em conjunto.

### <a name="sample-custom-metric-publication"></a>Publicação de métrica personalizada de exemplo
O exemplo seguinte, vai criar uma métrica personalizada chamada "Bytes no uso da memória", sob o espaço de nomes métrica "Perfil de memória" para uma Máquina Virtual. A métrica tem uma única dimensão chamada "Processo". Para o período de tempo especificado, são emite valores de métrica para dois processos diferentes:

```json
{
    "time": "2018-08-20T11:25:20-7:00",
    "data": {

      "baseData": {

        "metric": "Memory Bytes in Use",
        "namespace": "Memory Profile",
        "dimNames": [
          "Process"        ],
        "series": [
          {
            "dimValues": [
              "ContosoApp.exe"
            ],
            "min": 10,
            "max": 89,
            "sum": 190,
            "count": 4
          },
          {
            "dimValues": [
              "SalesApp.exe"
            ],
            "min": 10,
            "max": 23,
            "sum": 86,
            "count": 4
          }
        ]
      }
    }
  }
```
> [!NOTE]
> Application Insights, a extensão de diagnóstico do Windows Azure e o agente de InfluxData Telegraf já estão configuradas para emitir os valores de métrica relativamente ao ponto de final regional correto e executar todas as propriedades acima em cada emissões.
>
>

## <a name="custom-metric-definitions"></a>Definições de métricas personalizadas
Não é necessário definir previamente uma métrica personalizada no Azure Monitor, antes de ele é emitido. Uma vez que cada ponto de dados métricos publicado contém o espaço de nomes, nome e informações de dimensão, a primeira vez que uma métrica personalizada é emitida para o Azure Monitor uma definição de métrica é criada automaticamente. Esta definição de métrica, em seguida, é detetável em nenhum outro recurso que a métrica era emitida em relação a através de definições de métricas.

> [!NOTE]
> Monitor do Azure ainda não suporta a definição de "Unidades" para uma métrica personalizada.

## <a name="using-custom-metrics"></a>Utilizar métricas personalizadas
Depois de métricas personalizadas são submetidas para o Azure Monitor, que pode navegar através do portal do Azure, consultá-los por meio das APIs de REST do Azure Monitor ou crie alertas nos mesmos, para que possam ser notificados quando forem cumpridas determinadas condições.
### <a name="browse-your-custom-metrics-via-the-azure-portal"></a>Procurar as suas métricas personalizadas através do portal do Azure
1.  Aceda ao [Portal do Azure](https://portal.azure.com)
2.  Selecione o painel de Monitor
3.  Clique em métricas
4.  Selecione um recurso que ter emitido métricas personalizadas em relação a
5.  Selecione o espaço de nomes de métricas para sua métrica personalizada
6.  Selecione a métrica personalizada

## <a name="supported-regions"></a>Regiões suportadas
Durante a pré-visualização pública, a capacidade de publicar métricas personalizadas só está disponível num subconjunto de regiões do Azure. Isso significa que só podem ser publicadas métricas para recursos em uma das regiões suportadas. A tabela abaixo lista o conjunto de regiões do Azure suportadas para métricas personalizadas e o ponto de extremidade correspondente, devem ser publicadas métricas para recursos nessas regiões.

|Região do Azure|Prefixo de ponto de extremidade regional|
|---|---|
|EUA Leste|https://eastus.monitoring.azure.com/|
|EUA Centro-Sul|https://southcentralus.monitoring.azure.com/|
|EUA Centro-Oeste|https://westcentralus.monitoring.azure.com/|
|EUA Oeste 2|https://westus2.monitoring.azure.com/|
|Sudeste Asiático|https://southeastasia.monitoring.azure.com/|
|Europa do Norte|https://northeurope.monitoring.azure.com/|
|Europa Ocidental|https://westeurope.monitoring.azure.com/|

## <a name="quotas-and-limits"></a>Quotas e limites
O Azure Monitor impõe os seguintes limites de utilização em métricas personalizadas.

|Categoria|Limite|
|---|---|
|Tempo série/subscrições/região do Active Directory|50,000|
|Chaves de dimensão por métrica|10|
|Comprimento de cadeia de caracteres para espaços de nomes de métrica, nomes de métricas, chaves de dimensão e valores de dimensão|256 carateres|
Uma série de tempo ativo é definida como qualquer combinação única de métrica, chave de dimensão, valor de dimensão que tenha tido a valores de métrica publicadas nas últimas 12 horas.

## <a name="next-steps"></a>Passos Seguintes
Utilizar métricas personalizadas de diferentes serviços 
 - [Máquina Virtual](metrics-store-custom-guestos-resource-manager-vm.md)
 - [Conjunto de dimensionamento de máquinas virtuais](metrics-store-custom-guestos-resource-manager-vmss.md)
 - [Máquina virtual (clássico)](metrics-store-custom-guestos-classic-vm.md)
 - [Máquina Virtual do Linux com o agente de Telegraf](metrics-store-custom-linux-telegraf.md)
 - [API REST](metrics-store-custom-rest-api.md)
 - [Serviço em nuvem (clássico)](metrics-store-custom-guestos-classic-cloud-service.md)
 