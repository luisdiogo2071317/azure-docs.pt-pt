---
title: Instalar, executar contentores
titlesuffix: Face - Azure Cognitive Services
description: Como transferir, instalar e executar contentores para rosto neste tutorial passo a passo.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 01/22/2019
ms.author: diberry
ms.openlocfilehash: 2d9d4c3012691965d4396cb3927634111a7dd937
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55225283"
---
# <a name="install-and-run-containers"></a>Instalar e executar contentores

Contentorização é uma abordagem para distribuição de software em que uma aplicação ou serviço é empacotado como uma imagem de contentor. A configuração e dependências para a aplicação ou serviço estão incluídas na imagem de contentor. A imagem de contentor, em seguida, pode ser implementada num anfitrião de contentor com pouca ou nenhuma modificação. Os contentores são isolados umas das outras e o sistema operacional subjacente, com requisitos de espaço mais pequenos do que uma máquina virtual. Contentores podem ser instanciados a partir de imagens de contentor para tarefas de curta duração e removidos quando já não for necessário.

Face fornece um contentor de Linux padronizado para o Docker, rosto, que Deteta rostos humanos em imagens e identifica os atributos, incluindo pontos de referência do rosto (como noses e olhos), sexo, idade e outras funcionalidades faciais prever a máquina com o nome. Além de deteção, Face pode verificar se dois rostos na mesma imagem ou imagens diferentes são o mesmo com uma pontuação de confiança, ou comparam rostos em relação a uma base de dados para ver se um aspeto semelhante ou idêntica face já existe. Ele também pode organizar o rostos semelhantes em grupos, com as características de visual partilhadas.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="preparation"></a>Preparação

Tem de cumprir os seguintes pré-requisitos antes de utilizar o contentor de Face:

**Motor do docker**: Tem de ter o motor do Docker instalado localmente. Docker disponibiliza pacotes que configurar o ambiente do Docker num [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms), e [Windows](https://docs.docker.com/docker-for-windows/). No Windows, o Docker tem de ser configurado para dar suporte a contentores do Linux. Também podem ser implementados diretamente para contentores do docker [do Azure Kubernetes Service](../../aks/index.yml), [Azure Container Instances](../../container-instances/index.yml), ou uma [Kubernetes](https://kubernetes.io/) cluster implementado para [o azure Stack](../../azure-stack/index.yml). Para obter mais informações sobre a implementação de Kubernetes no Azure Stack, veja [implementar o Kubernetes no Azure Stack](../../azure-stack/user/azure-stack-solution-template-kubernetes-deploy.md).

Docker tem de ser configurado para permitir que os contentores para se ligar com e enviar dados de faturação para o Azure.

**Familiaridade com o registo de contentor da Microsoft e a Docker**: Deve ter um conhecimento básico dos conceitos de registo de contentor da Microsoft e a Docker, como registos, repositórios, contentores e imagens de contentor, bem como dados de conhecimento do basic `docker` comandos.  

Para obter um manual sobre noções básicas do Docker e um contentor, consulte a [descrição geral do Docker](https://docs.docker.com/engine/docker-overview/).

### <a name="container-requirements-and-recommendations"></a>Requisitos do contentor e recomendações

O contentor de Face requer um mínimo de 1 núcleo de CPU, pelo menos 2,6 GHz (gigahertz) ou mais rápida e 4 gigabytes (GB) de memória alocada, mas recomendamos, pelo menos, 2 núcleos de CPU e 6 GB de memória alocada.

## <a name="request-access-to-the-private-container-registry"></a>Pedir acesso para o registo de contentor privado

Primeiro tem de concluir e submeter o [formulário de pedido de contentores de imagem digitalizada dos serviços cognitivos](https://aka.ms/VisionContainersPreview) para pedir acesso para o contentor de rostos. O formulário solicita as informações sobre si, sua empresa e o cenário de utilizador que vai utilizar o contentor. Depois de submeter, a equipe de serviços cognitivos do Azure analisa o formulário para se certificar de que cumpre os critérios para acesso ao registo de contentor privado.

> [!IMPORTANT]
> Tem de utilizar um endereço de e-mail associado à conta de (Azure AD) de uma conta Microsoft (MSA) ou o Azure Active Directory no formulário.

Se o pedido for aprovado, em seguida, receber um e-mail com instruções que descrevem como obter as suas credenciais e aceder ao registo de contentor privado.

## <a name="create-a-face-resource-on-azure"></a>Criar um recurso de Face no Azure

Tem de criar um recurso de Face no Azure, se quiser usar o contêiner de rostos. Depois de criar o recurso, em seguida, utilize o URL de ponto final e a chave de subscrição do recurso para instanciar o contentor. Para obter mais informações sobre como criar uma instância de um contentor, consulte [criar uma instância de um contentor a partir de uma imagem de contentor transferido](#instantiate-a-container-from-a-downloaded-container-image).

Execute os seguintes passos para criar e recuperar informações a partir de um recurso de Face:

1. Crie um recurso de Face no portal do Azure.  
   Se quiser usar o contêiner de rostos, primeiro tem de criar um recurso de Face correspondente no portal do Azure. Para obter mais informações, consulte [início rápido: Criar uma conta dos serviços cognitivos no portal do Azure](../cognitive-services-apis-create-account.md).

1. Obtenha a chave de subscrição e o URL de ponto final para o recurso do Azure.  
   Depois de criar o recurso do Azure, tem de utilizar a chave de subscrição e o URL de ponto final desse recurso para instanciar o contentor de rostos correspondente. Pode copiar a chave de subscrição e o URL de ponto final da, respectivamente, os início rápido e as chaves de páginas do recurso de Face no portal do Azure.

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

A imagem de contentor para o contentor de Face está disponível de um registo de contentor de Docker privado, com o nome `containerpreview.azurecr.io`, no Azure Container Registry. A imagem de contentor para o contentor de rosto tem de ser transferida a partir do repositório para executar o contentor localmente.

Utilize o [solicitação docker](https://docs.docker.com/engine/reference/commandline/pull/) comando para transferir uma imagem de contentor a partir do repositório. Por exemplo, para transferir a imagem de contentor Face mais recente do repositório, utilize o seguinte comando:

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-face:latest
```

Para obter uma descrição completa de etiquetas disponíveis para o contentor de rostos, consulte [reconhecer texto](https://go.microsoft.com/fwlink/?linkid=2018655) no Docker Hub.

> [!TIP]
> Pode utilizar o [imagens do docker](https://docs.docker.com/engine/reference/commandline/images/) command para listar as imagens de contentor transferido. Por exemplo, o comando seguinte lista o ID, o repositório e a etiqueta de cada imagem de contentor transferido, formatada como uma tabela:
>
>  ```Docker
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>  ```
>

## <a name="instantiate-a-container-from-a-downloaded-container-image"></a>Criar uma instância de um contentor a partir de uma imagem de contentor transferido

Utilize o [docker run](https://docs.docker.com/engine/reference/commandline/run/) comando para criar uma instância de um contentor a partir de uma imagem de contentor transferido. Por exemplo, o seguinte comando:

* Cria uma instância de um contentor a partir da imagem de contentor de rostos
* Aloca dois núcleos de CPU e 6 gigabytes (GB) de memória
* Expõe a porta TCP 5000 e aloca um TTY pseudo para o contentor
* Remove automaticamente o contentor depois sai

```Docker
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789
```

> [!IMPORTANT]
> O `Eula`, `Billing`, e `ApiKey` opções tem de ser especificadas para instanciar o contentor; caso contrário, não inicia o contentor.  Para obter mais informações, consulte [faturação](#billing).

Uma vez instanciado, pode chamar as operações do contêiner com o URI de anfitrião do contentor. Por exemplo, o URI de anfitrião do seguinte representa o contentor de rostos que foi instanciado no exemplo anterior:

```http
http://localhost:5000/
```

> [!TIP]
> Pode aceder a [especificação de OpenAPI](https://swagger.io/docs/specification/about/) (anteriormente conhecido como a especificação Swagger), que descreve as operações suportadas por um contentor instanciado, a partir do `/swagger` URI relativo para esse contentor. Por exemplo, o URI seguinte fornece acesso a especificação de OpenAPI para o contentor de rostos que foi instanciado no exemplo anterior:
>
>  ```http
>  http://localhost:5000/swagger
>  ```

Pode [chamar as operações de REST API](https://docs.microsoft.com/azure/cognitive-services/face/face-api-how-to-topics/howtodetectfacesinimage) disponível a partir do seu contentor, ou utilize o [biblioteca de clientes de rosto do Azure Cognitive Services](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/) biblioteca de cliente para chamar essas operações.  
> [!IMPORTANT]
> Tem de ter o Cognitive Services Face biblioteca de clientes Azure versão 2.0 ou posterior se pretender utilizar a biblioteca de cliente com o seu contentor.

A única diferença entre chamar uma operação de determinado a partir do seu contentor e a chamada de que a mesma operação a partir de um serviço correspondente no Azure é que irá utilizar o anfitrião do URI do seu contentor, em vez do anfitrião do URI de uma região do Azure, para chamar a operação. Por exemplo, se quiser utilizar uma instância de rostos em execução na região do Azure do Oeste-nos para detetar rostos, chamaria a seguinte operação de REST API:

```http
POST https://westus.api.cognitive.microsoft.com/face/v1.0/detect
```

Se quisesse usar um contêiner de rostos em execução no seu computador local em sua configuração padrão para detetar rostos, chamaria a seguinte operação de REST API:

```http
POST http://localhost:5000/face/v1.0/detect
```

### <a name="billing"></a>Faturação

O contentor de Face envia informações de faturação para o Azure, através de um recurso de rostos correspondente na sua conta do Azure. As seguintes opções da linha de comandos são utilizadas pelo contentor de rostos para fins de faturação:

| Opção | Descrição |
|--------|-------------|
| `ApiKey` | A chave de API do recurso Face utilizada para controlar informações de faturas.<br/>O valor desta opção tem de ser definido para uma chave de API para o recurso do Face Azure aprovisionado especificado no `Billing`. |
| `Billing` | O ponto final do recurso Face utilizado para controlar informações de faturas.<br/>O valor desta opção tem de ser definido para o ponto final do URI de um recurso do Face Azure aprovisionado.|
| `Eula` | Indica que aceite a licença para o contentor.<br/>O valor desta opção tem de ser definido `accept`. |

> [!IMPORTANT]
> Todas as três opções tem de ser especificadas com valores válidos ou não inicia o contentor.

Para obter mais informações sobre estas opções, consulte [configurar contentores](face-resource-container-config.md).

## <a name="summary"></a>Resumo

Neste artigo, aprendeu conceitos e fluxo de trabalho para transferir, instalar e contentores de rostos em execução. Em resumo:

* Face fornece um contentor do Linux para o Docker, chamado de rostos, para detetar rostos ou identificar rostos através de uma base de dados de pessoas.
* Imagens de contentor são transferidas a partir de um registo de contentor privado no Azure.
* Executam imagens de contentor no Docker.
* Pode utilizar a REST API ou o SDK para chamar operações em contentores de Face ao especificar o URI do contentor do anfitrião.
* Tem de especificar informações de faturação ao instanciar um contentor.

> [!IMPORTANT]
> Contentores de serviços cognitivos não estão licenciados para executar sem a ser ligado ao Azure para medição. Os clientes têm de ativar os contentores comunicar informações de faturação com o serviço de medição em todos os momentos. Contentores de serviços cognitivos não enviar dados de cliente (por exemplo, a imagem ou texto que está a ser analisado) para a Microsoft.

## <a name="next-steps"></a>Passos Seguintes

* Revisão [configurar contentores](face-resource-container-config.md) para definições de configuração
* Revisão [descrição geral de Face](Overview.md) para saber mais sobre a detetar e identificar rostos  
* Consulte a [a API Face](//westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) para obter detalhes sobre os métodos suportados pelo contentor.
* Consulte a [perguntas mais frequentes (FAQ) do sobre](FAQ.md) para resolver problemas relacionados com a funcionalidade de rostos.
* Utilizar mais [contentores de serviços cognitivos](../cognitive-services-container-support.md)
