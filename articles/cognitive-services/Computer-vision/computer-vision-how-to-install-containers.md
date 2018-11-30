---
title: Como instalar e executar contentores
titlesuffix: Computer Vision - Cognitive Services - Azure
description: Como transferir, instalar e executar contentores para imagem digitalizada neste tutorial passo a passo.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 11/14/2018
ms.author: diberry
ms.openlocfilehash: 67dbf3bdf6631785fc876283847e36349e857a77
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/30/2018
ms.locfileid: "52634647"
---
# <a name="install-and-run-containers"></a>Instalar e executar contentores

Contentorização é uma abordagem para distribuição de software em que uma aplicação ou serviço é empacotado como uma imagem de contentor. A configuração e dependências para a aplicação ou serviço estão incluídas na imagem de contentor. A imagem de contentor, em seguida, pode ser implementada num anfitrião de contentor com pouca ou nenhuma modificação. Os contentores são isolados umas das outras e o sistema operacional subjacente, com requisitos de espaço mais pequenos do que uma máquina virtual. Contentores podem ser instanciados a partir de imagens de contentor para tarefas de curta duração e removidos quando já não for necessário.

A parte de reconhecer texto de imagem digitalizada também está disponível como um contentor do Docker. Permite-lhe detetar e extrair texto impresso de imagens de vários objetos com diferentes superfícies e fundos, como recibos, pôsteres e cartões de visita.  
> [!IMPORTANT]
> O contentor de reconhecer texto atualmente funciona apenas com o inglês.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="preparation"></a>Preparação

Tem de cumprir os seguintes pré-requisitos antes de utilizar o contentor de reconhecer texto:

**Motor do docker**: tem de ter o motor de Docker instalado localmente. Docker disponibiliza pacotes que configurar o ambiente do Docker num [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms), e [Windows](https://docs.docker.com/docker-for-windows/). No Windows, o Docker tem de ser configurado para dar suporte a contentores do Linux. Também podem ser implementados diretamente para contentores do docker [do Azure Kubernetes Service](/azure/aks/), [Azure Container Instances](/azure/container-instances/), ou uma [Kubernetes](https://kubernetes.io/) cluster implementado para [o azure Stack](/azure/azure-stack/). Para obter mais informações sobre a implementação de Kubernetes no Azure Stack, veja [implementar o Kubernetes no Azure Stack](/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy).

Docker tem de ser configurado para permitir que os contentores para se ligar com e enviar dados de faturação para o Azure.

**Familiaridade com o registo de contentor da Microsoft e a Docker**: deve ter um conhecimento básico dos conceitos de registo de contentor da Microsoft e a Docker, como registos, repositórios, contentores e imagens de contentor, bem como dados de conhecimento de básico `docker` comandos.  

Para obter um manual sobre noções básicas do Docker e um contentor, consulte a [descrição geral do Docker](https://docs.docker.com/engine/docker-overview/).

### <a name="server-requirements-and-recommendations"></a>Requisitos de servidor e recomendações

O contentor de reconhecer texto requer um mínimo de 1 núcleo de CPU, pelo menos 2,6 GHz (gigahertz) ou mais rápida e 8 gigabytes (GB) de memória alocada, mas recomendamos, pelo menos, 2 núcleos de CPU e 8 GB de memória alocada.

## <a name="request-access-to-the-private-container-registry"></a>Pedir acesso para o registo de contentor privado

Primeiro tem de concluir e submeter o [formulário de pedido de contentores de imagem digitalizada dos serviços cognitivos](https://aka.ms/VisionContainersPreview) para pedir acesso para o contentor de reconhecer texto. O formulário solicita as informações sobre si, sua empresa e o cenário de utilizador que vai utilizar o contentor. Depois de submeter, a equipe de serviços cognitivos do Azure analisa o formulário para se certificar de que cumpre os critérios para acesso ao registo de contentor privado.

> [!IMPORTANT]
> Tem de utilizar um endereço de e-mail associado à conta de (Azure AD) de uma conta Microsoft (MSA) ou o Azure Active Directory no formulário.

Se o pedido for aprovado, em seguida, receber um e-mail com instruções que descrevem como obter as suas credenciais e aceder ao registo de contentor privado.

## <a name="create-a-computer-vision-resource-on-azure"></a>Criar um recurso de imagem digitalizada no Azure

Tem de criar um recurso de imagem digitalizada no Azure, se quiser usar o contêiner de reconhecer texto. Depois de criar o recurso, em seguida, utilize o URL de ponto final e a chave de subscrição do recurso para instanciar o contentor. Para obter mais informações sobre como criar uma instância de um contentor, consulte [criar uma instância de um contentor a partir de uma imagem de contentor transferido](#instantiate-a-container-from-a-downloaded-container-image).

Execute os seguintes passos para criar e recuperar informações a partir de um recurso do Azure:

1. Crie um recurso do Azure no portal do Azure.  
   Se quiser usar o contêiner de reconhecer texto, primeiro tem de criar um recurso de imagem digitalizada correspondente no portal do Azure. Para obter mais informações, consulte [início rápido: criar uma conta dos serviços cognitivos no portal do Azure](../cognitive-services-apis-create-account.md).

1. Obtenha a chave de subscrição e o URL de ponto final para o recurso do Azure.  
   Depois de criar o recurso do Azure, tem de utilizar a chave de subscrição e o URL de ponto final desse recurso para instanciar o contentor de reconhecer texto correspondente. Pode copiar a chave de subscrição e o URL do ponto final da, respectivamente, os início rápido e as chaves de páginas do recurso de imagem digitalizada no portal do Azure.

## <a name="log-in-to-the-private-container-registry"></a>Inicie sessão particular registo de contentor

Existem várias formas de autenticar com o registo de contentor privado para contentores de serviços cognitivos, mas o método recomendado na linha de comando é utilizando o [CLI do Docker](https://docs.docker.com/engine/reference/commandline/cli/).

Utilize o [início de sessão do docker](https://docs.docker.com/engine/reference/commandline/login/) comando, conforme mostrado no exemplo a seguir, para iniciar sessão no `containerpreview.azurecr.io`, o registo de contentor privado para contentores de serviços cognitivos. Substitua *\<nome de utilizador\>* com o nome de utilizador e *\<palavra-passe\>* com a palavra-passe que forneceu as credenciais que recebeu do Azure Equipa de serviços cognitiva.

```docker
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Se proteger as suas credenciais de um arquivo de texto, poderá concatenar os conteúdos de texto de ficheiros, com o `cat` comando, para o `docker login` comando conforme mostrado no exemplo a seguir. Substitua *\<passwordFile\>* com o caminho e nome do ficheiro de texto que contém a palavra-passe e *\<username\>* com o nome de utilizador fornecido as credenciais.

```docker
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```

## <a name="download-container-images-from-the-private-container-registry"></a>Transfira imagens de contentor a partir do registo de contentor privado

A imagem de contentor para o contentor de reconhecer texto está disponível de um registo de contentor de Docker privado, com o nome `containerpreview.azurecr.io`, no Azure Container Registry. A imagem de contentor para o contentor de reconhecer texto deve ser transferida a partir do repositório para executar o contentor localmente.

Utilize o [solicitação docker](https://docs.docker.com/engine/reference/commandline/pull/) comando para transferir uma imagem de contentor a partir do repositório. Por exemplo, para transferir a imagem de contentor de reconhecer texto mais recente do repositório, utilize o seguinte comando:

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:latest
```

Para obter uma descrição completa de etiquetas disponíveis para o contentor de reconhecer texto, consulte [reconhecer texto](https://go.microsoft.com/fwlink/?linkid=2018655) no Docker Hub.

> [!TIP]
> Pode utilizar o [imagens do docker](https://docs.docker.com/engine/reference/commandline/images/) command para listar as imagens de contentor transferido. Por exemplo, o comando seguinte lista o ID, o repositório e a etiqueta de cada imagem de contentor transferido, formatada como uma tabela:
>
>  ```Docker
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>  ```
>

## <a name="instantiate-a-container-from-a-downloaded-container-image"></a>Criar uma instância de um contentor a partir de uma imagem de contentor transferido

Utilize o [docker run](https://docs.docker.com/engine/reference/commandline/run/) comando para criar uma instância de um contentor a partir de uma imagem de contentor transferido. Por exemplo, o seguinte comando:

* Cria uma instância de um contentor a partir da imagem de contentor de reconhecer texto
* Aloca dois núcleos de CPU e 8 gigabytes (GB) de memória
* Expõe a porta TCP 5000 e aloca um TTY pseudo para o contentor
* Remove automaticamente o contentor depois sai

```docker
docker run --rm -it -p 5000:5000 --memory 8g --cpus 2 containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text Eula=accept Billing=https://westus.api.cognitive.microsoft.com/vision/v2.0 ApiKey=0123456789
```

Uma vez instanciado, pode chamar as operações do contêiner com o URI de anfitrião do contentor. Por exemplo, o URI de anfitrião do seguinte representa o contentor de reconhecer texto que foi instanciado no exemplo anterior:

```http
http://localhost:5000/
```

> [!IMPORTANT]
> Pode aceder a [especificação de OpenAPI](https://swagger.io/docs/specification/about/) (anteriormente conhecido como a especificação Swagger), que descreve as operações suportadas por um contentor instanciado, a partir do `/swagger` URI relativo para esse contentor. Por exemplo, o URI seguinte fornece acesso a especificação de OpenAPI para o contentor de reconhecer texto que foi instanciado no exemplo anterior:
>
>  ```http
>  http://localhost:5000/swagger
>  ```

Pode [chamar as operações de REST API](https://docs.microsoft.com/azure/cognitive-services/computer-vision/vision-api-how-to-topics/howtocallvisionapi) disponível a partir do seu contentor de forma assíncrona ou síncrona clama pelo reconhecimento de texto, ou utilize o [Azure SDK dos serviços cognitivos computador visão](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision) cliente biblioteca para chamar essas operações.  
> [!IMPORTANT]
> Tem de ter o Azure SDK dos serviços cognitivos computador visão versão 3.2.0 ou posterior se pretender utilizar a biblioteca de cliente com o seu contentor.

### <a name="asynchronous-text-recognition"></a>Reconhecimento de texto assíncrona

Pode utilizar o `POST /vision/v2.0/recognizeText` e `GET /vision/v2.0/textOperations/*{id}*` operações em conjunto, de forma assíncrona reconhecer texto impresso numa imagem, semelhante a como o serviço de visão do computador utiliza as operações REST correspondentes. O contentor de reconhecer texto reconhece apenas texto impresso, texto manuscrito não, neste momento, pelo que a `mode` parâmetro normalmente especificado para a operação de serviço de visão do computador é ignorada pelo contentor de reconhecer texto.

### <a name="synchronous-text-recognition"></a>Reconhecimento de texto síncrona

Pode utilizar o `POST /vision/v2.0/recognizeTextDirect` operação de forma síncrona reconhecer texto impresso numa imagem. Uma vez que esta operação é síncrona, o corpo do pedido para esta operação é o mesmo que para o `POST /vision/v2.0/recognizeText` operação, mas a resposta body para esta operação é o mesmo que devolvido pelo `GET /vision/v2.0/textOperations/*{id}*` operação.

### <a name="billing"></a>Faturação

O contentor de reconhecer texto envia informações de faturação para o Azure, através de um recurso de imagem digitalizada correspondente na sua conta do Azure. As opções seguintes são utilizadas pelo contentor de reconhecer texto para efeitos de faturação:

| Opção | Descrição |
|--------|-------------|
| `ApiKey` | A chave de API do recurso de imagem digitalizada utilizada para controlar informações de faturas.<br/>O valor desta opção tem de ser definido para uma chave de API para o recurso aprovisionado do Azure de visão do computador especificado no `Billing`. |
| `Billing` | O ponto final do recurso de imagem digitalizada utilizado para controlar informações de faturas.<br/>O valor desta opção tem de ser definido para o ponto final do URI de um recurso do Azure de visão do computador aprovisionado.|
| `Eula` | Indica que aceite a licença para o contentor.<br/>O valor desta opção tem de ser definido `accept`. |

> [!IMPORTANT]
> Todas as três opções tem de ser especificadas com valores válidos ou não inicia o contentor.

Para obter mais informações sobre estas opções, consulte [configurar contentores](computer-vision-resource-container-config.md).

## <a name="summary"></a>Resumo

Neste artigo, aprendeu conceitos e fluxo de trabalho para transferir, instalar e de imagem digitalizada contentores em execução. Em resumo:

* Imagem digitalizada fornece uma contentores do Linux para o Docker, para detetar e extrair texto impresso.
* Imagens de contentor são transferidas a partir de um registo de contentor privado no Azure.
* Executam imagens de contentor no Docker.
* Pode utilizar a REST API ou o SDK para chamar operações em contentores de imagem digitalizada, especificando o URI do contentor do anfitrião.
* Tem de especificar informações de faturação ao instanciar um contentor.
* * * Cognitivos contentores de serviços não estão licenciados para executar sem a ser ligado ao Azure para medição. Os clientes têm de ativar os contentores comunicar informações de faturação com o serviço de medição em todos os momentos. Contentores de serviços cognitivos não enviar dados de cliente (por exemplo, a imagem ou texto que está a ser analisado) para a Microsoft.  

## <a name="next-steps"></a>Passos Seguintes

* Revisão [configurar contentores](computer-vision-resource-container-config.md) para definições de configuração
* Revisão [descrição geral de imagem digitalizada](Home.md) para saber mais sobre o reconhecimento de texto manuscrito e impresso  
* Consulte a [API de imagem digitalizada](//westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) para obter detalhes sobre os métodos suportados pelo contentor.
* Consulte a [perguntas mais frequentes (FAQ) do sobre](FAQ.md) para resolver problemas relacionados com a funcionalidade de imagem digitalizada.
