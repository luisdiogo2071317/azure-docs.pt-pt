---
title: Definições de contentor do docker
titleSuffix: Language Understanding - Azure Cognitive Services
description: O ambiente de tempo de execução de contentores do LUIS é configurado usando o `docker run` argumentos de comando. LUIS tem várias definições necessárias, juntamente com algumas configurações opcionais.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 12/04/2018
ms.author: diberry
ms.openlocfilehash: 98828589832d69ada11205e471314a153a566766
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53080270"
---
# <a name="configure-containers"></a>Configurar contentores

O ambiente de tempo de execução do contentor de compreensão de idiomas (LUIS) é configurado usando o `docker run` argumentos de comando. LUIS tem várias definições necessárias, juntamente com algumas configurações opcionais. Várias [exemplos](#example-docker-run-commands) do comando estão disponíveis. As definições específicas do contentor são a entrada [definições de montagem](#mount-settings) e as definições de faturas. 

Definições de contentor são [hierárquica](#hierarchical-settings) e podem ser definidas com [variáveis de ambiente](#environment-variable-settings) ou docker [argumentos da linha de comandos](#command-line-argument-settings).

## <a name="configuration-settings"></a>Definições de configuração

Este contentor tem as seguintes definições de configuração:

|Necessário|Definição|Objetivo|
|--|--|--|
|Sim|[ApiKey](#apikey-setting)|Utilizado para controlar informações de faturação.|
|Não|[ApplicationInsights](#applicationinsights-setting)|Permite que adicione [do Azure Application Insights](https://docs.microsoft.com/azure/application-insights) suporte de telemetria ao seu contentor.|
|Sim|[Faturação](#billing-setting)|Especifica o URI do ponto final da _compreensão de idiomas_ recursos no Azure.|
|Sim|[EULA](#eula-setting)| Indica que aceite a licença para o contentor.|
|Não|[Fluentd](#fluentd-settings)|Escrever o registo e, opcionalmente, dados de métrica para um servidor de Fluentd.|
|Não|[Registro em log](#logging-settings)|Fornece suporte para o contentor de registo do ASP.NET Core. |
|Sim|[Monta](#mount-settings)|Ler e escrever dados a partir [computador anfitrião](luis-container-howto.md#the-host-computer) de contentor e para o contentor para o computador anfitrião.|

> [!IMPORTANT]
> O [ `ApiKey` ](#apikey-setting), [ `Billing` ](#billing-setting), e [ `Eula` ](#eula-setting) definições são utilizadas em conjunto, e tem de indicar valores válidos para todas as três-los; caso contrário não inicia o contentor. Para obter mais informações sobre como utilizar estas definições de configuração para criar uma instância de um contentor, consulte [faturação](luis-container-howto.md#billing).

## <a name="apikey-setting"></a>ApiKey definição

O `ApiKey` definição especifica a chave de recurso do Azure utilizada para controlar informações de faturação para o contentor. Tem de especificar um valor para o ApiKey e o valor tem de ser uma chave válida para o _compreensão de idiomas_ recurso especificado para o [ `Billing` ](#billing-setting) definição de configuração.

Esta definição pode ser encontrada em dois locais:

* Portal do Azure: **compreensão de idiomas** gestão de recursos, em **chaves**
* Portal de LUIS: **chaves e ponto final definições** página. 

Não utilize a chave de arranque ou a chave de criação. 

## <a name="applicationinsights-setting"></a>Definição do Application Insights

O `ApplicationInsights` definição permite que adicione [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) suporte de telemetria ao seu contentor. O Application Insights fornece monitorização detalhada do seu contentor. Pode monitorizar facilmente o seu contentor de disponibilidade, desempenho e utilização. Pode também rapidamente identificar e diagnosticar erros no seu contentor.

A tabela seguinte descreve as definições de configuração suportadas ao abrigo do `ApplicationInsights` secção.

|Necessário| Nome | Tipo de dados | Descrição |
|--|------|-----------|-------------|
|Não| `InstrumentationKey` | Cadeia | A chave de instrumentação da instância do Application Insights à qual telemetria são enviados dados para o contentor. Para obter mais informações, consulte [Application Insights para ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). <br><br>Exemplo:<br>`InstrumentationKey=123456789`|


## <a name="billing-setting"></a>Definição de faturação

O `Billing` definição especifica o URI do ponto final da _compreensão de idiomas_ recurso no Azure utilizado para informações de faturação para o contentor do medidor. Tem de especificar um valor para esta definição de configuração e o valor tem de ser um URI de ponto de final válido para um _compreensão de idiomas_ recursos no Azure.

Esta definição pode ser encontrada em dois locais:

* Portal do Azure: **compreensão de idiomas** descrição geral, o nome `Endpoint`
* Portal de LUIS: **chaves e ponto final definições** página, como parte do ponto final do URI.

|Necessário| Nome | Tipo de dados | Descrição |
|--|------|-----------|-------------|
|Sim| `Billing` | Cadeia | URI do ponto final de faturação<br><br>Exemplo:<br>`Billing=https://westus.api.cognitive.microsoft.com/luis/v2.0` |

## <a name="eula-setting"></a>EULA definição

O `Eula` definição indica que aceite a licença para o contentor. Tem de especificar um valor para esta definição de configuração e o valor tem de ser definido `accept`.

|Necessário| Nome | Tipo de dados | Descrição |
|--|------|-----------|-------------|
|Sim| `Eula` | Cadeia | Aceitação da licença<br><br>Exemplo:<br>`Eula=accept` |

Contentores de serviços cognitivos são licenciados ao abrigo [seu contrato](https://go.microsoft.com/fwlink/?linkid=2018657) que regem a utilização do Azure. Se não tiver um contrato existente que regem a utilização do Azure, concorda que seu contrato que rege a utilização do Azure é o [contrato de subscrição Online da Microsoft](https://go.microsoft.com/fwlink/?linkid=2018755), que incorpora o [termos dos Online Services ](https://go.microsoft.com/fwlink/?linkid=2018760). Para pré-visualizações, também concorda com o [termos de utilização suplementares para pré-visualizações do Microsoft Azure](https://go.microsoft.com/fwlink/?linkid=2018815). Ao utilizar o contentor concorda com estes termos.

## <a name="fluentd-settings"></a>Definições de Fluentd

Fluentd é um recoletor de dados de código-fonte aberto para o registo unificado. O `Fluentd` definições gerir ligação do contentor para um [Fluentd](https://www.fluentd.org) server. O contentor de LUIS inclui um provedor de log Fluentd, que permite que o contentor escrever os registos e, opcionalmente, dados de métrica para um servidor de Fluentd.

A tabela seguinte descreve as definições de configuração suportadas ao abrigo do `Fluentd` secção.

| Nome | Tipo de dados | Descrição |
|------|-----------|-------------|
| `Host` | Cadeia | O endereço IP ou nome de anfitrião DNS do servidor Fluentd. |
| `Port` | Número inteiro | A porta do servidor Fluentd.<br/> O valor predefinido é 24224. |
| `HeartbeatMs` | Número inteiro | O intervalo de pulsação, em milissegundos. Se nenhum tráfego de evento foram enviado antes de expira este intervalo, é enviado um heartbeat para o servidor de Fluentd. O valor predefinido é 60000 milissegundos (1 minuto). |
| `SendBufferSize` | Número inteiro | O espaço de memória intermédia rede, em bytes, alocado para operações de envio. O valor predefinido é 32768 bytes (32 kilobytes). |
| `TlsConnectionEstablishmentTimeoutMs` | Número inteiro | O tempo limite, em milissegundos, para estabelecer uma ligação de SSL/TLS com o servidor de Fluentd. O valor predefinido é 10000 milissegundos (10 segundos).<br/> Se `UseTLS` é definido como FALSO, este valor é ignorado. |
| `UseTLS` | Booleano | Indica se o contentor deve utilizar SSL/TLS para comunicar com o servidor de Fluentd. O valor predefinido é false. |

## <a name="logging-settings"></a>Definições de registo

O `Logging` suporte de registo do ASP.NET Core para o contentor de gerir as definições. Pode usar as mesmas definições de configuração e os valores para o contentor que utiliza para uma aplicação ASP.NET Core. 

O contentor de LUIS suporta os seguintes fornecedores de registo:

|Fornecedor|Objetivo|
|--|--|
|[Console](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#console-provider)|O ASP.NET Core `Console` provedor de log. Todas as definições de configuração do ASP.NET Core e valores predefinidos para este fornecedor de registo são suportadas.|
|[Depurar](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#debug-provider)|O ASP.NET Core `Debug` provedor de log. Todas as definições de configuração do ASP.NET Core e valores predefinidos para este fornecedor de registo são suportadas.|
|[Disco](#disk-logging)|O fornecedor de registo JSON. Este fornecedor de registo escreve dados de registo para a montagem de saída.|

### <a name="disk-logging"></a>Registo de disco
  
O `Disk` provedor de log suporta as seguintes definições de configuração:  

| Nome | Tipo de dados | Descrição |
|------|-----------|-------------|
| `Format` | Cadeia | O formato de saída para ficheiros de registo.<br/> **Nota:** este valor tem de ser definido como `json` para ativar o fornecedor de registo. Se este valor for especificado sem também especificar uma montagem de saída ao instanciar um contentor, ocorrerá um erro. |
| `MaxFileSize` | Número inteiro | O tamanho máximo, em megabytes (MB), de um ficheiro de registo. Quando o tamanho do arquivo de log atual atende ou excede este valor, um novo ficheiro de registo é iniciado pelo fornecedor de registo. Se não for especificado -1, o tamanho do ficheiro de registo está limitado apenas pelo tamanho máximo do ficheiro, se existir, para a montagem de saída. O valor predefinido é 1. |

Para obter mais informações sobre como configurar o suporte de registo do ASP.NET Core, consulte [configuração das definições do ficheiro](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#settings-file-configuration).

## <a name="mount-settings"></a>Definições de montagem

Monta o enlace de utilização para ler e escrever dados de e para o contentor. Pode especificar uma montagem de entrada ou saída de montagem, especificando o `--mount` opção da [docker run](https://docs.docker.com/engine/reference/commandline/run/) comando. 

O contentor de LUIS não usa a entrada ou saída monta para armazenar dados de serviço ou de treinamento. 

A sintaxe exata da localização de montagem do anfitrião varia consoante o sistema operativo anfitrião. Além disso, o [computador anfitrião](luis-container-howto.md#the-host-computer)da localização de montagem não estar acessível devido a um conflito entre as permissões usadas pela conta de serviço do docker e o anfitrião montar permissões de localização. 

A tabela seguinte descreve as definições suportadas.

|Necessário| Nome | Tipo de dados | Descrição |
|-------|------|-----------|-------------|
|Sim| `Input` | Cadeia | O destino da montagem de entrada. O valor predefinido é `/input`. Esta é a localização dos ficheiros de pacote do LUIS. <br><br>Exemplo:<br>`--mount type=bind,src=c:\input,target=/input`|
|Não| `Output` | Cadeia | O destino de montagem de saída. O valor predefinido é `/output`. Esta é a localização dos registos. Isto inclui registos de consulta do LUIS e registos de contentor. <br><br>Exemplo:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="hierarchical-settings"></a>Definições de hierárquicas

Definições para o contentor de LUIS são hierárquicas e todos os contentores na [computador anfitrião](luis-container-howto.md#the-host-computer) utilizar uma hierarquia partilhada.

Pode utilizar qualquer um dos seguintes procedimentos para especificar as definições:

* [Variáveis de ambiente](#environment-variable-settings)
* [Argumentos da linha de comandos](#command-line-argument-settings)

Valores de variáveis de ambiente substituem valores de argumento da linha de comandos, que por sua vez, substituir os valores predefinidos para a imagem de contentor. Se especificar valores diferentes numa variável de ambiente e um argumento da linha de comandos para a mesma definição de configuração, o valor na variável de ambiente é utilizado pelo contêiner instanciado.

|Precedência|Definir localização|
|--|--|
|1|Variável de ambiente| 
|2|Linha de comandos|
|3|Valor de padrão de imagem de contentor|

### <a name="environment-variable-settings"></a>Definições de variáveis de ambiente

Os benefícios do uso de variáveis de ambiente são:

* É possível configurar várias definições.
* Vários contentores podem utilizar as mesmas definições.

### <a name="command-line-argument-settings"></a>Definições de argumento da linha de comandos

A vantagem de usar os argumentos da linha de comandos é que cada contentor pode usar diferentes configurações.

## <a name="example-docker-run-commands"></a>Executar comandos de docker de exemplo

Os exemplos seguintes utilizam as definições de configuração para ilustrar como escrever e usar `docker run` comandos.  Quando em execução, o contentor continua a ser executada até [parar](luis-container-howto.md#stop-the-container) -lo.


* **Caracteres de continuação de linha**: os comandos de docker nas secções seguintes utilizam a barra invertida, `\`, como um caractere de continuação de linha. Substituir ou remova esta com base nos requisitos de seu sistema de operativo de anfitrião. 
* **Ordem de argumento**: não altere a ordem dos argumentos, a menos que está bastante familiarizada com contentores do docker.

Substitua {_argument_name_} pelos seus próprios valores:

| Marcador de posição | Valor | Formato ou de exemplo |
|-------------|-------|---|
|{ENDPOINT_KEY} | A chave de ponto final da aplicação LUIS treinada. |xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx|
|{BILLING_ENDPOINT} | O valor de ponto final de faturação está disponível na página de descrição geral de compreensão de idioma do portal do Azure.|https://westus.api.cognitive.microsoft.com/luis/v2.0|

> [!IMPORTANT]
> O `Eula`, `Billing`, e `ApiKey` opções tem de ser especificadas para executar o contentor; caso contrário, não inicia o contentor.  Para obter mais informações, consulte [faturação](luis-container-howto.md#billing).
> O valor de ApiKey é o **chave** também está disponível na página de chaves do recurso de compreensão de linguagem do Azure e as chaves e os pontos finais de página no portal do LUIS. 

### <a name="basic-example"></a>Exemplo básico

O exemplo seguinte tem os argumentos de menor número possíveis de executar o contentor:

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={ENDPOINT_KEY}
```

> [!Note] 
> O comando anterior utiliza o diretório a `c:` unidade para evitar conflitos de permissão no Windows. Se precisar de utilizar um diretório específico como a entrada de diretório, poderá ter de conceder o docker permissão de serviço. O comando docker anterior utiliza a barra invertida, `\`, como um caractere de continuação de linha. Substituir ou remover isso com base no seu [computador anfitrião](luis-container-howto.md#the-host-computer) requisitos do sistema operacional. Não altere a ordem dos argumentos, a menos que está bastante familiarizada com contentores do docker.


### <a name="applicationinsights-example"></a>Exemplo do Application Insights

O exemplo seguinte define o argumento do Application Insights para enviar telemetria para o Application Insights, enquanto o contentor está em execução:

```bash
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={ENDPOINT_KEY}
InstrumentationKey={INSTRUMENTATION_KEY}
```

### <a name="logging-example-with-command-line-arguments"></a>Exemplo de Registro em log com argumentos da linha de comandos

O comando a seguir define o nível de registo `Logging:Console:LogLevel`, para configurar o nível de registo para [ `Information` ](https://msdn.microsoft.com). 

```bash
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={ENDPOINT_KEY} \
Logging:Console:LogLevel=Information
```

### <a name="logging-example-with-environment-variable"></a>Exemplo de registo com a variável de ambiente

A utilização de comandos seguinte uma variável de ambiente, chamado `Logging:Console:LogLevel` para configurar o nível de registo para [ `Information` ](https://msdn.microsoft.com). 

```bash
SET Logging:Console:LogLevel=Information
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={APPLICATION_ID} \
```

## <a name="next-steps"></a>Passos Seguintes

* Revisão [como instalar e executar contentores](luis-container-howto.md)
* Consulte a [perguntas mais frequentes (FAQ) do sobre](luis-resources-faq.md) para resolver problemas relacionados com a funcionalidade de LUIS.