---
title: Configurar contentores
titlesuffix: Text Analytics - Cognitive Services - Azure
description: Definições de configuração para os contentores de análise de texto.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: conceptual
ms.date: 11/14/2018
ms.author: diberry
ms.openlocfilehash: 61f435c55d7925554facd9bbaa63767023a779d4
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52965024"
---
# <a name="configure-containers"></a>Configurar contentores

Análise de texto fornece cada contentor com uma estrutura comum de configuração, para que possa facilmente configurar e gerir as definições de armazenamento, registo e telemetria e segurança para os seus contentores.

## <a name="configuration-settings"></a>Definições de configuração

Definições de configuração em contentores de análise de texto são hierárquicas, e todos os contentores de utilizarem uma hierarquia de partilhado, com base na estrutura de nível superior seguinte:

* [ApiKey](#apikey-configuration-setting)
* [ApplicationInsights](#applicationinsights-configuration-settings)
* [Autenticação](#authentication-configuration-settings)
* [Faturação](#billing-configuration-setting)
* [EULA](#eula-configuration-setting)
* [Fluentd](#fluentd-configuration-settings)
* [Registro em log](#logging-configuration-settings)
* [Monta](#mounts-configuration-settings)

Pode utilizar tanto [variáveis de ambiente](#configuration-settings-as-environment-variables) ou [argumentos da linha de comandos](#configuration-settings-as-command-line-arguments) para especificar as definições de configuração ao instanciar um contentor dos contentores de análise de texto.

Valores de variáveis de ambiente substituem valores de argumento da linha de comandos, que por sua vez, substituir os valores predefinidos para a imagem de contentor. Em outras palavras, se especificar diferentes valores numa variável de ambiente e um argumento da linha de comandos para a mesma definição de configuração, tais como `Logging:Disk:LogLevel`, em seguida, criar uma instância de um contentor, o valor na variável de ambiente é utilizado pelo instanciadas contentor.

### <a name="configuration-settings-as-environment-variables"></a>Definições de configuração como variáveis de ambiente

Pode utilizar o [sintaxe de variável de ambiente do ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.1&tabs=basicconfiguration#configuration-by-environment) para especificar as definições de configuração.

O contentor lê as variáveis de ambiente de utilizador quando o contentor é instanciado. Se existir uma variável de ambiente, o valor da variável de ambiente substitui o valor predefinido para a definição de configuração especificado. A vantagem de usar variáveis de ambiente é que múltiplas definições de configuração podem ser definidas antes de instanciar a contentores e vários contentores automaticamente podem utilizar o mesmo conjunto de definições de configuração.

Por exemplo, os comandos seguintes utilizam uma variável de ambiente para configurar o nível de registo de consola para [LogLevel.Information](https://msdn.microsoft.com), em seguida, cria uma instância de um contentor a partir da imagem de contentor de análise de sentimentos. O valor da variável de ambiente substitui a definição de configuração predefinida.

  ```Docker
  SET Logging:Console:LogLevel=Information
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 mcr.microsoft.com/azure-cognitive-services/sentiment Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0 ApiKey=0123456789
  ```

### <a name="configuration-settings-as-command-line-arguments"></a>Definições de configuração como argumentos da linha de comandos

Pode utilizar o [sintaxe de argumentos da linha de comandos do ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.1&tabs=basicconfiguration#arguments) para especificar as definições de configuração.

Pode especificar as definições de configuração no opcional `ARGS` parâmetro do [docker run](https://docs.docker.com/engine/reference/commandline/run/) comando utilizado para criar uma instância de um contentor a partir de uma imagem de contentor transferido. A vantagem de usar os argumentos da linha de comandos é que cada contentor pode utilizar um conjunto de definições de configuração diferente e personalizado.

Por exemplo, o seguinte comando cria uma instância de um contentor a partir da imagem de contentor de análise de sentimentos e configura a consola do nível de registo para LogLevel.Information, substituir a definição de configuração predefinida.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 mcr.microsoft.com/azure-cognitive-services/sentiment Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0 ApiKey=0123456789 Logging:Console:LogLevel=Information
  ```

## <a name="apikey-configuration-setting"></a>ApiKey definição de configuração

O `ApiKey` definição de configuração especifica a chave de configuração do recurso de análise de texto no Azure utilizado para controlar informações de faturas para o contentor. Tem de especificar um valor para esta definição de configuração e o valor tem de ser uma chave de configuração válida para o recurso de análise de texto especificado para o [ `Billing` ](#billing-configuration-setting) definição de configuração.

> [!IMPORTANT]
> O [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), e [ `Eula` ](#eula-configuration-setting) definições de configuração são utilizadas em conjunto, e tem de indicar valores válidos para todas as três -los; caso contrário, não inicia o contentor. Para obter mais informações sobre como utilizar estas definições de configuração para criar uma instância de um contentor, consulte [faturação](how-tos/text-analytics-how-to-install-containers.md#billing).

## <a name="applicationinsights-configuration-settings"></a>Definições de configuração do Application Insights

As definições de configuração no `ApplicationInsights` seção permite que adicione [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) suporte de telemetria ao seu contentor. O Application Insights fornece monitorização detalhada do seu contentor até o nível de código. Pode monitorizar facilmente o seu contentor de disponibilidade, desempenho e utilização. Pode também rapidamente identificar e diagnosticar erros no seu contentor sem esperar por um utilizador para comunicá-las.

A tabela seguinte descreve as definições de configuração suportadas ao abrigo do `ApplicationInsights` secção.

| Nome | Tipo de dados | Descrição |
|------|-----------|-------------|
| `InstrumentationKey` | Cadeia | A chave de instrumentação da instância do Application Insights à qual telemetria são enviados dados para o contentor. Para obter mais informações, consulte [Application Insights para ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). |

## <a name="authentication-configuration-settings"></a>Definições de configuração de autenticação

O `Authentication` definições de configuração fornecem opções de segurança do Azure para o contentor. Embora as definições de configuração nesta secção estão disponíveis para todos os contentores em contentores de análise de texto, a maneira na qual são utilizados os valores de definição de configuração é específica para cada contentor e contentores podem não utilize esta secção em todos os.

A tabela seguinte descreve as definições de configuração suportadas ao abrigo do `Authentication` secção.

| Nome | Tipo de dados | Descrição |
|------|-----------|-------------|
| `ApiKey` | cadeia de caracteres ou matriz | As chaves de subscrição do Azure utilizadas pelo contêiner para aceder a outros recursos do Azure, se necessário, o contentor.<br/> Se utilizar mais do que uma chave de subscrição do contentor, este valor com a, em seguida, é especificado como uma matriz de cadeias de caracteres; caso contrário, um valor de cadeia é utilizado para especificar uma chave de subscrição individual utilizada pelo contentor. |

## <a name="billing-configuration-setting"></a>Definição de configuração de faturação

O `Billing` definição de configuração especifica o ponto final do URI do recurso de análise de texto no Azure utilizada para informações de faturação para o contentor do medidor. Tem de especificar um valor para esta definição de configuração e o valor tem de ser um ponto final válido URI para um recurso de análise de texto no Azure.

> [!IMPORTANT]
> O [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), e [ `Eula` ](#eula-configuration-setting) definições de configuração são utilizadas em conjunto, e tem de indicar valores válidos para todas as três -los; caso contrário, não inicia o contentor. Para obter mais informações sobre como utilizar estas definições de configuração para criar uma instância de um contentor, consulte [faturação](how-tos/text-analytics-how-to-install-containers.md#billing).

## <a name="eula-configuration-setting"></a>Definição de configuração do EULA

O `Eula` definição de configuração indica que aceite a licença para o contentor. Tem de especificar um valor para esta definição de configuração e o valor tem de ser definido `accept`.

> [!IMPORTANT]
> O [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), e [ `Eula` ](#eula-configuration-setting) definições de configuração são utilizadas em conjunto, e tem de indicar valores válidos para todas as três -los; caso contrário, não inicia o contentor. Para obter mais informações sobre como utilizar estas definições de configuração para criar uma instância de um contentor, consulte [faturação](how-tos/text-analytics-how-to-install-containers.md#billing).

Contentores de serviços cognitivos são licenciados sob o contrato que rege a utilização do Azure. Se não tiver um contrato existente que regem a utilização do Azure, concorda que seu contrato que rege a utilização do Azure é o Microsoft contrato de subscrição Online (o qual incorpora os termos dos Online Services). Para pré-visualizações, também concorda com os termos de utilização suplementares para pré-visualizações do Microsoft Azure. Ao utilizar o contentor concorda com estes termos.

## <a name="fluentd-configuration-settings"></a>Definições de configuração Fluentd

O `Fluentd` secção gere as definições de configuração para [Fluentd](https://www.fluentd.org), um recoletor de dados de código-fonte aberto para o registo unificado. Contentores de análise de texto inclui um provedor de log Fluentd que permite que o contentor escrever o registo e, opcionalmente, dados de métrica para um servidor de Fluentd.

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

O `Logging` suporte de registo do ASP.NET Core para o contentor de gerir as definições de configuração. Pode usar as mesmas definições de configuração e os valores para o contentor que pode fazer para uma aplicação ASP.NET Core. Suporta os seguintes fornecedores de registo de contentores de análise de texto:

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

Os contentores de Docker fornecidos por contêineres de análise de texto foram concebidos para ser sem estado e imutável. Em outras palavras, os arquivos criados dentro de um contentor são armazenados numa camada de contentor gravável, persistir apenas enquanto o contentor está em execução e não pode ser acessado facilmente. Se esse contêiner é parada ou removida, os arquivos criados dentro desse contentor com o mesmo são destruídos.

No entanto, pois são contentores do Docker, pode utilizar opções de armazenamento do Docker, como volumes e vincular monta, para ler e escrever dados persistentes fora do contentor, se o contentor de oferecer suporte a ele. Para obter mais informações sobre como especificar nem gerenciar as opções de armazenamento do Docker, consulte [gerir dados no Docker](https://docs.docker.com/storage/).

> [!NOTE]
> Normalmente, não tem de alterar os valores para estas definições de configuração. Em vez disso, usará os valores especificados nestas definições de configuração como destinos ao especificar monta de entrada e saída para o contentor. Para obter mais informações sobre como especificar monta de entrada e saída, consulte [entrada e saída monta](#input-and-output-mounts).

A tabela seguinte descreve as definições de configuração suportadas ao abrigo do `Mounts` secção.

| Nome | Tipo de dados | Descrição |
|------|-----------|-------------|
| `Input` | Cadeia | O destino da montagem de entrada. O valor predefinido é `/input`. |
| `Output` | Cadeia | O destino de montagem de saída. O valor predefinido é `/output`. |

### <a name="input-and-output-mounts"></a>Monta de entrada e saída

Por predefinição, cada contentor pode suportar um *entrada de montagem*, de que o contentor pode ler os dados e um *montagem de saída*, para que o contentor pode escrever os dados. Contentores não são necessários para suportar a entrada ou saída monta, no entanto, e cada contentor pode utilizar monta de entrada e saída para fins de contentor específicas para além das opções de registo suportadas por contêineres de análise de texto. A seguinte tabela listas de entrada e saída montagem o suporte para cada contentor nos contentores de análise de texto.

| Contentor | Montagem de entrada | Montagem de saída |
|-----------|-------------|--------------|
|[Extração de expressões-chave](#working-with-key-phrase-extraction) | Não suportado | Opcional |
|[Deteção de idioma](#working-with-language-detection) | Não suportado | Opcional |
|[Análise de sentimentos](#working-with-sentiment-analysis) | Não suportado | Opcional |

Pode especificar uma montagem de entrada ou saída de montagem, especificando o `--mount` opção da [docker run](https://docs.docker.com/engine/reference/commandline/run/) comando utilizado para criar uma instância de um contentor a partir de uma imagem de contentor transferido. Por predefinição, a entrada montar utiliza `/input` como o destino e o resultado de montagem utiliza `/output` como respectivo destino. Pode ser especificada qualquer opção de armazenamento do Docker, disponível para o anfitrião do contentor de Docker a `--mount` opção.

Por exemplo, o comando seguinte define uma montagem de enlace de Docker para o `D:\Output` pasta na máquina host como a montagem de saída, em seguida, cria uma instância de um contentor a partir da imagem de contentor de análise de sentimentos, guardar ficheiros de registo no formato JSON para a montagem de saída.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 --mount type=bind,source=D:\Output,destination=/output mcr.microsoft.com/azure-cognitive-services/sentiment Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0 ApiKey=0123456789 Logging:Disk:Format=json
  ```
