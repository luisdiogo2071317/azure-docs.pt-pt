---
title: Instalar e executar contentores
titleSuffix: Text Analytics - Cognitive Services - Azure
description: Como transferir, instalar e executar contentores para análise de texto neste tutorial passo a passo.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 11/14/2018
ms.author: diberry
ms.openlocfilehash: 99bdb42d9a0d86d0d2acc4a6272e0c802042e6b5
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51635019"
---
# <a name="install-and-run-containers"></a>Instalar e executar contentores

Contentorização é uma abordagem para distribuição de software em que uma aplicação ou serviço é empacotado como uma imagem de contentor. A configuração e dependências para a aplicação ou serviço estão incluídas na imagem de contentor. A imagem de contentor, em seguida, pode ser implementada num anfitrião de contentor com pouca ou nenhuma modificação. Os contentores são isolados umas das outras e o sistema operacional subjacente, com requisitos de espaço mais pequenos do que uma máquina virtual. Contentores podem ser instanciados a partir de imagens de contentor para tarefas de curta duração e removidos quando já não for necessário.

Análise de texto fornece o seguinte conjunto de contentores do Docker, cada um deles contendo um subconjunto de funcionalidades:

| Contentor| Descrição |
|----------|-------------|
|Extração de Expressões-Chave | Extrai as expressões-chave para identificar os pontos principais. Por exemplo, para o texto de entrada “The food was delicious and there were wonderful staff”, a API devolve os pontos de conversa principais: “food” e “wonderful staff”. |
|Deteção de Idioma | Para até 120 idiomas, Deteta e reporta em que idioma está escrito o texto de entrada. O contentor comunica um código de idioma único para cada documento que está incluído no pedido. O código de idioma é emparelhado com uma pontuação que indica a força da pontuação. |
|Análise de Sentimentos | Analisa o texto não processado para dicas sobre o sentimento positivo ou negativo. Esta API devolve uma pontuação de sentimento entre 0 e 1 para cada documento, em que 1 é o mais positivo. Os modelos de análise previamente são treinados com um corpo extenso de tecnologias de texto e de linguagem natural da Microsoft. Para os [idiomas selecionados](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages.md), a API pode analisar e classificar qualquer texto não processado que fornecer, devolvendo diretamente os resultados à aplicação de chamada. |

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="preparation"></a>Preparação

Tem de cumprir os seguintes pré-requisitos antes de utilizar contentores de análise de texto:

**Motor do docker**: tem de ter o motor de Docker instalado localmente. Docker disponibiliza pacotes que configurar o ambiente do Docker num [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms), e [Windows](https://docs.docker.com/docker-for-windows/). No Windows, o Docker tem de ser configurado para dar suporte a contentores do Linux. Também podem ser implementados diretamente para contentores do docker [do Azure Kubernetes Service](/azure/aks/), [Azure Container Instances](/azure/container-instances/), ou uma [Kubernetes](https://kubernetes.io/) cluster implementado para [o azure Stack](/azure/azure-stack/). Para obter mais informações sobre a implementação de Kubernetes no Azure Stack, veja [implementar o Kubernetes no Azure Stack](/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy).

Docker tem de ser configurado para permitir que os contentores para se ligar com e enviar dados de faturação para o Azure.

**Familiaridade com o registo de contentor da Microsoft e a Docker**: deve ter um conhecimento básico dos conceitos de registo de contentor da Microsoft e a Docker, como registos, repositórios, contentores e imagens de contentor, bem como dados de conhecimento de básico `docker` comandos.  

Para obter um manual sobre noções básicas do Docker e um contentor, consulte a [descrição geral do Docker](https://docs.docker.com/engine/docker-overview/).

### <a name="server-requirements-and-recommendations"></a>Requisitos de servidor e recomendações

A tabela seguinte descreve os núcleos de CPU mínimos e recomendados, pelo menos 2,6 GHz (gigahertz) ou mais rápida e memória, em gigabytes (GB), ao alocar para cada contentor de análise de texto.

| Contentor | Mínimo | Recomendado |
|-----------|---------|-------------|
|Extração de Expressões-Chave | 1 núcleo, 2 GB de memória | 1 núcleo, 4 GB de memória |
|Deteção de Idioma | 1 núcleo, 2 GB de memória | 1 núcleo, 4 GB de memória |
|Análise de Sentimentos | 1 núcleo, 8 GB de memória | 1 núcleo, 8 GB de memória |

## <a name="download-container-images-from-microsoft-container-registry"></a>Transferir imagens de contentor a partir do registo de contentor da Microsoft

Imagens de contentor para análise de texto estão disponíveis a partir do registo de contentor do Microsoft. A tabela seguinte lista os repositórios disponíveis do registo de contentor da Microsoft para os contentores de análise de texto. Cada repositório contém uma imagem de contentor, que tem de ser transferida para executar o contentor localmente.

| Contentor | Repositório |
|-----------|------------|
|Extração de Expressões-Chave | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
|Deteção de Idioma | `mcr.microsoft.com/azure-cognitive-services/language` |
|Análise de Sentimentos | `mcr.microsoft.com/azure-cognitive-services/sentiment` |

Utilize o [solicitação docker](https://docs.docker.com/engine/reference/commandline/pull/) comando para transferir uma imagem de contentor a partir de um repositório. Por exemplo, para transferir a imagem de contentor de extração de expressões chave mais recente a partir do repositório, utilize o seguinte comando:

```Docker
docker pull mcr.microsoft.com/azure-cognitive-services/keyphrase:latest
```

Para obter uma descrição completa de etiquetas disponíveis para os contentores de análise de texto, consulte os seguintes contentores sobre o Docker Hub:

* [Extração de expressões-chave](https://go.microsoft.com/fwlink/?linkid=2018757)
* [Deteção de idioma](https://go.microsoft.com/fwlink/?linkid=2018759)
* [Análise de sentimentos](https://go.microsoft.com/fwlink/?linkid=2018654)

> [!TIP]
> Pode utilizar o [imagens do docker](https://docs.docker.com/engine/reference/commandline/images/) command para listar as imagens de contentor transferido. Por exemplo, o comando seguinte lista o ID, o repositório e a etiqueta de cada imagem de contentor transferido, formatada como uma tabela:
>
>  ```Docker
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>  ```
>

## <a name="instantiate-a-container-from-a-downloaded-container-image"></a>Criar uma instância de um contentor a partir de uma imagem de contentor transferido

Utilize o [docker run](https://docs.docker.com/engine/reference/commandline/run/) comando para criar uma instância de um contentor a partir de uma imagem de contentor transferido. Por exemplo, o seguinte comando:

* Cria uma instância de um contentor a partir da imagem de contentor de análise de sentimentos
* Aloca um núcleo de CPU e 8 gigabytes (GB) de memória
* Expõe a porta TCP 5000 e aloca um TTY pseudo para o contentor
* Remove automaticamente o contentor depois sai

```Docker
docker run --rm -it -p 5000:5000 --memory 8g --cpus 1 mcr.microsoft.com/azure-cognitive-services/sentiment Eula=accept Billing=https://westus.api.cognitive.microsoft.com/text/analytics/v2.0 ApiKey=0123456789

```

> [!IMPORTANT]
> O `Eula`, `Billing`, e `ApiKey` opções da linha de comandos tem de ser especificadas para instanciar o contentor; caso contrário, não inicia o contentor.  Para obter mais informações, consulte [faturação](#billing).

Uma vez instanciado, pode chamar as operações do contêiner com o URI de anfitrião do contentor. Por exemplo, o URI de anfitrião do seguinte representa o contentor de análise de sentimentos que foi instanciado no exemplo anterior:

```http
http://localhost:5000/
```

> [!TIP]
> Pode aceder a [especificação de OpenAPI](https://swagger.io/docs/specification/about/) (anteriormente conhecido como a especificação Swagger), que descreve as operações suportadas por um contentor instanciado, a partir do `/swagger` URI relativo para esse contentor. Por exemplo, o URI seguinte fornece acesso a especificação de OpenAPI para o contentor de análise de sentimentos foi instanciado no exemplo anterior:
>
>  ```http
>  http://localhost:5000/swagger
>  ```

Pode [chamar as operações de REST API](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-call-api) disponível a partir do seu contentor, ou utilize o [Azure SDK dos serviços cognitivos Text Analytics](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics) biblioteca de cliente para chamar essas operações.  
> [!IMPORTANT]
> Tem de ter o Azure SDK dos serviços cognitivos Text Analytics versão 2.1.0 ou posterior se pretender utilizar a biblioteca de cliente com o seu contentor.

A única diferença entre chamar uma operação de determinado a partir do seu contentor e a chamada de que a mesma operação a partir de um serviço correspondente no Azure é que irá utilizar o anfitrião do URI do seu contentor, em vez do anfitrião do URI de uma região do Azure, para chamar a operação. Por exemplo, se quiser utilizar uma instância de análise de texto em execução na região Oeste nos Azure, chamaria a seguinte operação de REST API:

```http
POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases
```

Se quiser utilizar um contentor de extração de expressões chave em execução no seu computador local em sua configuração padrão, chamaria a seguinte operação de REST API:

```http
POST http://localhost:5000/text/analytics/v2.0/keyPhrases
```

### <a name="billing"></a>Faturação

Os contentores de análise de texto enviam informações de faturação para o Azure, através de um recurso de análise de texto correspondente na sua conta do Azure. As seguintes opções da linha de comandos são utilizadas pelos contentores de análise de texto para efeitos de faturação:

| Opção | Descrição |
|--------|-------------|
| `ApiKey` | A chave de API do recurso de análise de texto utilizado para controlar informações de faturação.<br/>O valor desta opção tem de ser definido para uma chave de API para o recurso aprovisionado do Azure de análise de texto especificado no `Billing`. |
| `Billing` | O ponto final do recurso de análise de texto utilizado para controlar informações de faturação.<br/>O valor desta opção tem de ser definido para o ponto final do URI de um recurso de análise de texto Azure aprovisionado.|
| `Eula` | Indica que aceite a licença para o contentor.<br/>O valor desta opção tem de ser definido `accept`. |

> [!IMPORTANT]
> Todas as três opções tem de ser especificadas com valores válidos ou não inicia o contentor.

Para obter mais informações sobre estas opções, consulte [configurar contentores](../text-analytics-resource-container-config.md).

## <a name="summary"></a>Resumo

Neste artigo, aprendeu conceitos e fluxo de trabalho para transferir, instalar e análise de texto contentores em execução. Em resumo:

* Análise de texto fornece três contentores do Linux para o Docker, encapsulando a extração de expressões-chave, deteção de idioma e análise de sentimentos.
* Imagens de contentor são transferidas a partir de um registo de contentor privado no Azure.
* Executam imagens de contentor no Docker.
* Pode utilizar a REST API ou o SDK para chamar operações em contentores de análise de texto ao especificar o URI do contentor do anfitrião.
* Tem de especificar informações de faturação ao instanciar um contentor.
* * * Cognitivos contentores de serviços não estão licenciados para executar sem a ser ligado ao Azure para medição. Os clientes têm de ativar os contentores comunicar informações de faturação com o serviço de medição em todos os momentos. Contentores de serviços cognitivos não enviar dados de cliente (por exemplo, a imagem ou texto que está a ser analisado) para a Microsoft.  

## <a name="next-steps"></a>Passos Seguintes

* Revisão [configurar contentores](../text-analytics-resource-container-config.md) para definições de configuração
* Revisão [descrição geral da análise de texto](../overview.md) para saber mais sobre a deteção de expressões-chave, deteção de idioma e análise de sentimentos  
* Consulte a [API de análise de texto](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) para obter detalhes sobre os métodos suportados pelo contentor.
* Consulte a [perguntas mais frequentes (FAQ) do sobre](../text-analytics-resource-faq.md) para resolver problemas relacionados com a funcionalidade de imagem digitalizada.