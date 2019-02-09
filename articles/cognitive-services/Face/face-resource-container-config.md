---
title: Configurar contentores
titlesuffix: Face - Azure Cognitive Services
description: Definições de configuração para contentores.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: diberry
ms.openlocfilehash: a70c200f0260d87d35eae8728ed1f908e7061f32
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2019
ms.locfileid: "55979110"
---
# <a name="configure-face-docker-containers"></a>Configurar os contentores do Docker de rostos

O **Face** ambiente de tempo de execução de contentores é configurado usando o `docker run` argumentos de comando. Este contentor tem várias definições necessárias, juntamente com algumas configurações opcionais. Várias [exemplos](#example-docker-run-commands) do comando estão disponíveis. As definições específicas do contentor são as definições de faturas. 

Definições de contentor são [hierárquica](#hierarchical-settings) e podem ser definidas com [variáveis de ambiente](#environment-variable-settings) ou docker [argumentos da linha de comandos](#command-line-argument-settings).

## <a name="configuration-settings"></a>Definições de configuração

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> O [ `ApiKey` ](#apikey-setting), [ `Billing` ](#billing-setting), e [ `Eula` ](#eula-setting) definições são utilizadas em conjunto, e tem de indicar valores válidos para todas as três-los; caso contrário não inicia o contentor. Para obter mais informações sobre como utilizar estas definições de configuração para criar uma instância de um contentor, consulte [faturação](face-how-to-install-containers.md#billing).

## <a name="apikey-configuration-setting"></a>ApiKey definição de configuração

O `ApiKey` definição especifica a chave de recurso do Azure utilizada para controlar informações de faturação para o contentor. Tem de especificar um valor para o ApiKey e o valor tem de ser uma chave válida para o _Face_ recurso especificado para o [ `Billing` ](#billing-setting) definição de configuração.

Esta definição pode ser encontrada no seguinte local:

* Portal do Azure: **Da face** gestão de recursos, em **chaves**

## <a name="applicationinsights-setting"></a>Definição do Application Insights

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Definição de configuração de faturação

O `Billing` definição especifica o URI do ponto final da _Face_ recurso no Azure utilizado para informações de faturação para o contentor do medidor. Tem de especificar um valor para esta definição de configuração e o valor tem de ser um URI de ponto de final válido para um _Face_ recursos no Azure. O contentor de relatórios de utilização sobre a cada 10 a 15 minutos.

Esta definição pode ser encontrada no seguinte local:

* Portal do Azure: **Da face** descrição geral, o nome `Endpoint`

|Necessário| Nome | Tipo de dados | Descrição |
|--|------|-----------|-------------|
|Sim| `Billing` | Cadeia | URI do ponto final de faturação<br><br>Exemplo:<br>`Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0` |

## <a name="eula-setting"></a>EULA definição

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Definições de Fluentd

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>Definições de credenciais de proxy de HTTP

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Definições de registo
 
[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]

## <a name="mount-settings"></a>Definições de montagem

Monta o enlace de utilização para ler e escrever dados de e para o contentor. Pode especificar uma montagem de entrada ou saída de montagem, especificando o `--mount` opção da [docker run](https://docs.docker.com/engine/reference/commandline/run/) comando.

Os contentores de rostos não usam a entrada ou saída monta para armazenar dados de serviço ou de treinamento. 

A sintaxe exata da localização de montagem do anfitrião varia consoante o sistema operativo anfitrião. Além disso, o [computador anfitrião](face-how-to-install-containers.md#the-host-computer)da localização de montagem não estar acessível devido a um conflito entre as permissões usadas pela conta de serviço do Docker e o anfitrião montar permissões de localização. 

|Opcional| Name | Tipo de dados | Descrição |
|-------|------|-----------|-------------|
|Não permitido| `Input` | String | Contentores de rostos não utilize esta opção.|
|Opcional| `Output` | String | O destino de montagem de saída. O valor predefinido é `/output`. Esta é a localização dos registos. Isto inclui registos de contentor. <br><br>Exemplo:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="hierarchical-settings"></a>Definições de hierárquicas

[!INCLUDE [Container shared configuration hierarchical settings](../../../includes/cognitive-services-containers-configuration-shared-hierarchical-settings.md)]

## <a name="example-docker-run-commands"></a>Executar comandos de docker de exemplo 

Os exemplos seguintes utilizam as definições de configuração para ilustrar como escrever e usar `docker run` comandos.  Quando em execução, o contentor continua a ser executada até [parar](face-how-to-install-containers.md#stop-the-container) -lo.

* **Caracteres de continuação de linha**: Os comandos de Docker nas secções seguintes utilizam a barra invertida, `\`, como um caractere de continuação de linha. Substituir ou remova esta com base nos requisitos de seu sistema de operativo de anfitrião. 
* **Ordem de argumento**: Não altere a ordem dos argumentos, a menos que está bastante familiarizada com contentores do Docker.

Substitua {_argument_name_} pelos seus próprios valores:

| Marcador de posição | Valor | Formato ou de exemplo |
|-------------|-------|---|
|{BILLING_KEY} | A chave de ponto final do recurso de rostos. |xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx|
|{BILLING_ENDPOINT_URI} | O valor de ponto final faturação incluindo região.|`https://westcentralus.api.cognitive.microsoft.com/face/v1.0`|

> [!IMPORTANT]
> O `Eula`, `Billing`, e `ApiKey` opções tem de ser especificadas para executar o contentor; caso contrário, não inicia o contentor.  Para obter mais informações, consulte [faturação](face-how-to-install-containers.md#billing).
> O valor de ApiKey é o **chave** na página chaves do Azure Face recursos. 

## <a name="face-container-docker-examples"></a>Exemplos do contentor Docker de rostos

Os exemplos de Docker seguintes são para o contentor de rostos. 

### <a name="basic-example"></a>Exemplo básico 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
  containerpreview.azurecr.io/microsoft/cognitive-services-face \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} \
  ApiKey={BILLING_KEY} 
  ```

### <a name="logging-example-with-command-line-arguments"></a>Exemplo de Registro em log com argumentos da linha de comandos

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} ApiKey={BILLING_KEY} \
  Logging:Console:LogLevel=Information
  ```

### <a name="logging-example-with-environment-variable"></a>Exemplo de registo com a variável de ambiente

  ```
  SET Logging:Console:LogLevel=Information
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} \
  ApiKey={BILLING_KEY}
  ```

## <a name="next-steps"></a>Passos Seguintes

* Revisão [como instalar e executar contentores](face-how-to-install-containers.md)
