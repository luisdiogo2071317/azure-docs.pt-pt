---
title: Configurar contentores
titlesuffix: Face - Azure Cognitive Services
description: Definições de configuração para contentores.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: conceptual
ms.date: 11/14/2018
ms.author: diberry
ms.openlocfilehash: 30546d31e96d7d7fa1009f16a50fe8fda12ead67
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53105109"
---
# <a name="configure-containers"></a>Configurar contentores

O contentor de rostos utiliza uma estrutura comum de configuração, para que possa facilmente configurar e gerir as definições de armazenamento, registo e telemetria e segurança para os seus contentores.

## <a name="configuration-settings"></a>Definições de configuração

Definições de configuração no contentor Face hierárquicas e todos os contentores de utilizarem uma hierarquia de partilhado, com base na estrutura de nível superior seguinte:

* [ApiKey](#apikey-configuration-setting)
* [ApplicationInsights](#applicationinsights-configuration-settings)
* [Autenticação](#authentication-configuration-settings)
* [Faturação](#billing-configuration-setting)
* [CloudAI](#cloudai-configuration-settings)
* [EULA](#eula-configuration-setting)
* [Fluentd](#fluentd-configuration-settings)
* [Registro em log](#logging-configuration-settings)
* [Monta](#mounts-configuration-settings)

Pode utilizar tanto [variáveis de ambiente](#configuration-settings-as-environment-variables) ou [argumentos da linha de comandos](#configuration-settings-as-command-line-arguments) para especificar as definições de configuração ao instanciar um contentor de rostos.

Valores de variáveis de ambiente substituem valores de argumento da linha de comandos, que por sua vez, substituir os valores predefinidos para a imagem de contentor. Em outras palavras, se especificar diferentes valores numa variável de ambiente e um argumento da linha de comandos para a mesma definição de configuração, tais como `Logging:Disk:LogLevel`, em seguida, criar uma instância de um contentor, o valor na variável de ambiente é utilizado pelo instanciadas contentor.

### <a name="configuration-settings-as-environment-variables"></a>Definições de configuração como variáveis de ambiente

Pode utilizar o [sintaxe de variável de ambiente do ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.1&tabs=basicconfiguration#configuration-by-environment) para especificar as definições de configuração.

O contentor lê as variáveis de ambiente de utilizador quando o contentor é instanciado. Se existir uma variável de ambiente, o valor da variável de ambiente substitui o valor predefinido para a definição de configuração especificado. A vantagem de usar variáveis de ambiente é que múltiplas definições de configuração podem ser definidas antes de instanciar a contentores e vários contentores automaticamente podem utilizar o mesmo conjunto de definições de configuração.

Por exemplo, os comandos seguintes utilizam uma variável de ambiente para configurar o nível de registo de consola para [LogLevel.Information](https://msdn.microsoft.com), em seguida, cria uma instância de um contentor a partir da imagem de contentor de rostos. O valor da variável de ambiente substitui a definição de configuração predefinida.

  ```Docker
  SET Logging:Console:LogLevel=Information
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789
  ```

### <a name="configuration-settings-as-command-line-arguments"></a>Definições de configuração como argumentos da linha de comandos

Pode utilizar o [sintaxe de argumentos da linha de comandos do ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.1&tabs=basicconfiguration#arguments) para especificar as definições de configuração.

Pode especificar as definições de configuração no opcional `ARGS` parâmetro do [docker run](https://docs.docker.com/engine/reference/commandline/run/) comando utilizado para criar uma instância de um contentor a partir de uma imagem de contentor transferido. A vantagem de usar os argumentos da linha de comandos é que cada contentor pode utilizar um conjunto de definições de configuração diferente e personalizado.

Por exemplo, o seguinte comando cria uma instância de um contentor a partir da imagem de contentor de rostos e configura a consola do nível de registo para LogLevel.Information, substituir a definição de configuração predefinida.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 Logging:Console:LogLevel=Information
  ```

## <a name="apikey-configuration-setting"></a>ApiKey definição de configuração

O `ApiKey` definição de configuração especifica a chave de configuração do recurso de Face no Azure utilizado para controlar informações de faturas para o contentor. Tem de especificar um valor para esta definição de configuração e o valor tem de ser uma chave de configuração válida para o recurso de rostos especificado para o [ `Billing` ](#billing-configuration-setting) definição de configuração.

> [!IMPORTANT]
> O [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), e [ `Eula` ](#eula-configuration-setting) definições de configuração são utilizadas em conjunto, e tem de indicar valores válidos para todas as três -los; caso contrário, não inicia o contentor. Para obter mais informações sobre como utilizar estas definições de configuração para criar uma instância de um contentor, consulte [faturação](face-how-to-install-containers.md#billing).

## <a name="applicationinsights-configuration-settings"></a>Definições de configuração do Application Insights

As definições de configuração no `ApplicationInsights` seção permite que adicione [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) suporte de telemetria ao seu contentor. O Application Insights fornece monitorização detalhada do seu contentor até o nível de código. Pode monitorizar facilmente o seu contentor de disponibilidade, desempenho e utilização. Pode também rapidamente identificar e diagnosticar erros no seu contentor sem esperar por um utilizador para comunicá-las.

A tabela seguinte descreve as definições de configuração suportadas ao abrigo do `ApplicationInsights` secção.

| Nome | Tipo de dados | Descrição |
|------|-----------|-------------|
| `InstrumentationKey` | Cadeia | A chave de instrumentação da instância do Application Insights à qual telemetria são enviados dados para o contentor. Para obter mais informações, consulte [Application Insights para ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). |

## <a name="authentication-configuration-settings"></a>Definições de configuração de autenticação

O `Authentication` definições de configuração fornecem opções de segurança do Azure para o contentor. Embora as definições de configuração nesta secção estão disponíveis, o contentor de rostos não utilize esta secção.

| Nome | Tipo de dados | Descrição |
|------|-----------|-------------|
| `Storage` | Cadeia | A chave de instrumentação da instância do Application Insights à qual telemetria são enviados dados para o contentor. Para obter mais informações, consulte [Application Insights para ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). |

## <a name="billing-configuration-setting"></a>Definição de configuração de faturação

O `Billing` definição de configuração especifica o ponto final do URI do recurso de Face no Azure utilizada para informações de faturação para o contentor do medidor. Tem de especificar um valor para esta definição de configuração e o valor tem de ser um ponto final válido URI para um recurso de Face no Azure.

> [!IMPORTANT]
> O [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), e [ `Eula` ](#eula-configuration-setting) definições de configuração são utilizadas em conjunto, e tem de indicar valores válidos para todas as três -los; caso contrário, não inicia o contentor. Para obter mais informações sobre como utilizar estas definições de configuração para criar uma instância de um contentor, consulte [faturação](face-how-to-install-containers.md#billing).

## <a name="cloudai-configuration-settings"></a>Definições de configuração de CloudAI

As definições de configuração no `CloudAI` secção fornecer opções de contentor específicas exclusivas para o seu contentor. As seguintes definições e os objetos são suportados para o contentor de Face no `CloudAI` secção

| Nome | Tipo de dados | Descrição |
|------|-----------|-------------|
| `Storage` | Object | O cenário de armazenamento utilizado pelo contentor de rostos. Para obter mais informações sobre cenários de armazenamento e as definições associadas para o `Storage` objeto, consulte [as definições de cenário de armazenamento](#storage-scenario-settings) |

### <a name="storage-scenario-settings"></a>Definições de cenário de armazenamento

O contentor de Face armazena BLOBs, cache, metadados e dados de fila, consoante o que está a ser armazenado. Por exemplo, os índices de treinamento e resultados de um grupo de pessoas grandes são armazenados como dados de Blobs. O contentor de Face fornece dois cenários de armazenamento diferente ao interagir com e armazenar esses tipos de dados:

* Memória  
  Todos os quatro tipos de dados são armazenados na memória. Eles não estão a ser distribuídos, nem são persistentes. Se o contentor de Face está parado ou removido, todos os dados no armazenamento para esse contentor for destruída.  
  Este é o cenário de armazenamento padrão para o contentor de rostos.
* Azure  
  O contentor de rostos utiliza o armazenamento do Azure e o Azure Cosmos DB para distribuir estes quatro tipos de dados entre armazenamento persistente. Dados de BLOBs e filas são processados pelo armazenamento do Azure. Dados de metadados e o cache são processados pelo Azure Cosmos DB, com a API do MongoDB. Se o contentor de Face está parado ou removido, todos os dados no armazenamento para esse contentor permanece armazenado no armazenamento do Azure e o Azure Cosmos DB.  
  Os recursos utilizados pelo cenário de armazenamento do Azure têm os seguintes requisitos adicionais
  * O recurso de armazenamento do Azure tem de utilizar o tipo de conta StorageV2
  * O recurso do Azure Cosmos DB tem de utilizar a API do MongoDB

Os cenários de armazenamento e as definições de configuração associados são geridas pelos `Storage` objeto no `CloudAI` seção de configuração. As seguintes definições de configuração estão disponíveis no `Storage` objeto:

| Nome | Tipo de dados | Descrição |
|------|-----------|-------------|
| `StorageScenario` | Cadeia | O cenário de armazenamento suportado pelo contentor. Os seguintes valores estão disponíveis<br/>`Memory` -Valor de predefinido. Contentor utiliza o armazenamento não persistentes, e distribuída e dentro da memória, para utilização temporária, de nó único. Se o contentor está parado ou removido, o armazenamento para esse contentor for destruído.<br/>`Azure` -Container utiliza recursos do Azure para o armazenamento. Se o contentor está parado ou removido, o armazenamento para esse contêiner é mantido.|
| `ConnectionStringOfAzureStorage` | Cadeia | A cadeia de ligação para o recurso de armazenamento do Azure utilizado pelo contentor.<br/>Esta definição aplica-se apenas se `Azure` é especificado para o `StorageScenario` definição de configuração. |
| `ConnectionStringOfCosmosMongo` | Cadeia | A cadeia de ligação do MongoDB para o recurso do Azure Cosmos DB utilizada pelo contentor.<br/>Esta definição aplica-se apenas se `Azure` é especificado para o `StorageScenario` definição de configuração. |

Por exemplo, o seguinte comando Especifica o cenário de armazenamento do Azure e fornece as cadeias de ligação de exemplo para os recursos de armazenamento do Azure e o Cosmos DB utilizados para armazenar os dados para o contentor de rostos.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 CloudAI:Storage:StorageScenario=Azure CloudAI:Storage:ConnectionStringOfCosmosMongo="mongodb://samplecosmosdb:0123456789@samplecosmosdb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb" CloudAI:Storage:ConnectionStringOfAzureStorage="DefaultEndpointsProtocol=https;AccountName=sampleazurestorage;AccountKey=0123456789;EndpointSuffix=core.windows.net"
  ```

O cenário de armazenamento é processado separadamente da monta de entrada e saída monta. Pode especificar uma combinação desses recursos para um único contentor. Por exemplo, o comando seguinte define uma montagem de enlace de Docker para o `D:\Output` pasta na máquina host como a montagem de saída, em seguida, cria uma instância de um contentor a partir da imagem de contentor Face, guardar ficheiros de registo no formato JSON para a montagem de saída. O comando também especifica o cenário de armazenamento do Azure e fornece as cadeias de ligação de exemplo para os recursos de armazenamento do Azure e o Cosmos DB utilizados para armazenar os dados para o contentor de rostos.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 --mount type=bind,source=D:\Output,destination=/output containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 Logging:Disk:Format=json CloudAI:Storage:StorageScenario=Azure CloudAI:Storage:ConnectionStringOfCosmosMongo="mongodb://samplecosmosdb:0123456789@samplecosmosdb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb" CloudAI:Storage:ConnectionStringOfAzureStorage="DefaultEndpointsProtocol=https;AccountName=sampleazurestorage;AccountKey=0123456789;EndpointSuffix=core.windows.net"
  ```

## <a name="eula-configuration-setting"></a>Definição de configuração do EULA

O `Eula` definição de configuração indica que aceite a licença para o contentor. Tem de especificar um valor para esta definição de configuração e o valor tem de ser definido `accept`.

> [!IMPORTANT]
> O [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), e [ `Eula` ](#eula-configuration-setting) definições de configuração são utilizadas em conjunto, e tem de indicar valores válidos para todas as três -los; caso contrário, não inicia o contentor. Para obter mais informações sobre como utilizar estas definições de configuração para criar uma instância de um contentor, consulte [faturação](face-how-to-install-containers.md#billing).

Contentores de serviços cognitivos são licenciados ao abrigo [seu contrato](https://go.microsoft.com/fwlink/?linkid=2018657) que regem a utilização do Azure. Se não tiver um contrato existente que regem a utilização do Azure, concorda que seu contrato que rege a utilização do Azure é o [contrato de subscrição Online da Microsoft](https://go.microsoft.com/fwlink/?linkid=2018755) (que incorpora o [termos dos Online Services ](https://go.microsoft.com/fwlink/?linkid=2018760)). Para pré-visualizações, também concorda com o [termos de utilização suplementares para pré-visualizações do Microsoft Azure](https://go.microsoft.com/fwlink/?linkid=2018815). Ao utilizar o contentor concorda com estes termos.

## <a name="fluentd-configuration-settings"></a>Definições de configuração Fluentd

O `Fluentd` secção gere as definições de configuração para [Fluentd](https://www.fluentd.org), um recoletor de dados de código-fonte aberto para o registo unificado. o contentor de Face inclui um provedor de log Fluentd que permite que o contentor escrever o registo e, opcionalmente, dados de métrica para um servidor de Fluentd.

A tabela seguinte descreve as definições de configuração suportadas ao abrigo do `Fluentd` secção.

| Nome | Tipo de dados | Descrição |
|------|-----------|-------------|
| `Host` | Cadeia | O endereço IP ou nome de anfitrião DNS do servidor Fluentd. |
| `Port` | Número inteiro | A porta do servidor Fluentd.<br/> O valor predefinido é 24224. |
| `HeartbeatMs` | Número inteiro | O intervalo de pulsação, em milissegundos. Se nenhum tráfego de evento foram enviado antes de expira este intervalo, é enviado um heartbeat para o servidor de Fluentd. O valor predefinido é 60000 milissegundos (1 minuto). |
| `SendBufferSize` | Número inteiro | O espaço de memória intermédia rede, em bytes, alocado para operações de envio. O valor predefinido é 32768 bytes (32 kilobytes). |
| `TlsConnectionEstablishmentTimeoutMs` | Número inteiro | O tempo limite, em milissegundos, para estabelecer uma ligação de SSL/TLS com o servidor de Fluentd. O valor predefinido é 10000 milissegundos (10 segundos).<br/> Se `UseTLS` é definido como FALSO, este valor é ignorado. |
| `UseTLS` | Booleano | Indica se o contentor deve utilizar SSL/TLS para comunicar com o servidor de Fluentd. O valor predefinido é false. |

## <a name="logging-configuration-settings"></a>Definições de configuração de registo

O `Logging` suporte de registo do ASP.NET Core para o contentor de gerir as definições de configuração. Pode usar as mesmas definições de configuração e os valores para o contentor que pode fazer para uma aplicação ASP.NET Core. Os seguintes fornecedores de registo são compatíveis com o contentor de Face:

* [Console](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#console-provider)  
  O ASP.NET Core `Console` provedor de log. Todas as definições de configuração do ASP.NET Core e valores predefinidos para este fornecedor de registo são suportadas.
* [Depurar](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#debug-provider)  
  O ASP.NET Core `Debug` provedor de log. Todas as definições de configuração do ASP.NET Core e valores predefinidos para este fornecedor de registo são suportadas.
* Disco  
  O fornecedor de registo JSON. Este fornecedor de registo escreve dados de registo para a montagem de saída.  
  O `Disk` provedor de log suporta as seguintes definições de configuração:  

  | Nome | Tipo de dados | Descrição |
  |------|-----------|-------------|
  | `Format` | Cadeia | O formato de saída para ficheiros de registo.<br/> **Nota:** este valor tem de ser definido como `json` para ativar o fornecedor de registo. Se este valor for especificado sem também especificar uma montagem de saída ao instanciar um contentor, ocorrerá um erro. |
  | `MaxFileSize` | Número inteiro | O tamanho máximo, em megabytes (MB), de um ficheiro de registo. Quando o tamanho do arquivo de log atual atende ou excede este valor, um novo ficheiro de registo é iniciado pelo fornecedor de registo. Se não for especificado -1, o tamanho do ficheiro de registo está limitado apenas pelo tamanho máximo do ficheiro, se existir, para a montagem de saída. O valor predefinido é 1. |

Para obter mais informações sobre como configurar o suporte de registo do ASP.NET Core, consulte [configuração das definições do ficheiro](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#settings-file-configuration).

## <a name="mounts-configuration-settings"></a>Monta definições de configuração

Os contentores de Docker fornecidos pelo Face destinam-se para ser sem estado e imutável. Em outras palavras, os arquivos criados dentro de um contentor são armazenados numa camada de contentor gravável, persistir apenas enquanto o contentor está em execução e não pode ser acessado facilmente. Se esse contêiner é parada ou removida, os arquivos criados dentro desse contentor com o mesmo são destruídos.

No entanto, pois são contentores do Docker, pode utilizar opções de armazenamento do Docker, como volumes e vincular monta, para ler e escrever dados persistentes fora do contentor, se o contentor de oferecer suporte a ele. Para obter mais informações sobre como especificar nem gerenciar as opções de armazenamento do Docker, consulte [gerir dados no Docker](https://docs.docker.com/storage/).

> [!NOTE]
> Normalmente, não tem de alterar os valores para estas definições de configuração. Em vez disso, usará os valores especificados nestas definições de configuração como destinos ao especificar monta de entrada e saída para o contentor. Para obter mais informações sobre como especificar monta de entrada e saída, consulte [entrada e saída monta](#input-and-output-mounts).

A tabela seguinte descreve as definições de configuração suportadas ao abrigo do `Mounts` secção.

| Nome | Tipo de dados | Descrição |
|------|-----------|-------------|
| `Input` | Cadeia | O destino da montagem de entrada. O valor predefinido é `/input`. |
| `Output` | Cadeia | O destino de montagem de saída. O valor predefinido é `/output`. |

### <a name="input-and-output-mounts"></a>Monta de entrada e saída

Por predefinição, cada contentor pode suportar um *entrada de montagem*, de que o contentor pode ler os dados e um *montagem de saída*, para que o contentor pode escrever os dados. Contentores não são necessários para suportar a entrada ou saída monta, no entanto, e cada contentor pode utilizar monta de entrada e saída para fins de contentor específicas além das opções de registo suportadas pelo contentor de rostos.

O contentor de rostos não suporta a entrada monta e, opcionalmente, suporta saída monta.

Pode especificar uma montagem de entrada ou saída de montagem, especificando o `--mount` opção da [docker run](https://docs.docker.com/engine/reference/commandline/run/) comando utilizado para criar uma instância de um contentor a partir de uma imagem de contentor transferido. Por predefinição, a entrada montar utiliza `/input` como o destino e o resultado de montagem utiliza `/output` como respectivo destino. Pode ser especificada qualquer opção de armazenamento do Docker, disponível para o anfitrião do contentor de Docker a `--mount` opção.

Por exemplo, o comando seguinte define uma montagem de enlace de Docker para o `D:\Output` pasta na máquina host como a montagem de saída, em seguida, cria uma instância de um contentor a partir da imagem de contentor Face, guardar ficheiros de registo no formato JSON para a montagem de saída.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 --mount type=bind,source=D:\Output,destination=/output containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 Logging:Disk:Format=json
  ```

O contentor de rostos não usa a entrada ou saída monta para armazenar dados de treinamento ou base de dados. Em vez disso, o contêiner de Face fornece cenários de armazenamento para o gerenciamento de dados de treinamento e base de dados. Para obter mais informações sobre a utilização de cenários de armazenamento, consulte [definições de cenário de armazenamento](#storage-scenario-settings).
