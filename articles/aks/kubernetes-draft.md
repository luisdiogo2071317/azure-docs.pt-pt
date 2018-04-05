---
title: Use e rascunho com AKS e registo de contentor do Azure
description: Use e rascunho com AKS e registo de contentor do Azure
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/29/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 2ab79e3a6308d01d836a82f356f43eccb6af9791
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="use-draft-with-azure-container-service-aks"></a>Use e rascunho com o serviço de contentor do Azure (AKS)

Rascunho é uma ferramenta open-source que ajuda a conter e implementar esses contentores num cluster Kubernetes, deixando-livre nos podermos concentrar no ciclo de desenvolvimento – "loop interna" do desenvolvimento concentrado. Rascunho funciona como o código que está a ser desenvolvido, mas antes de consolidar para o controlo de versão. Com o rascunho, pode rapidamente implementar novamente uma aplicação Kubernetes tal como as alterações de código. Para obter mais informações sobre rascunho, consulte o [rascunho documentação no Github][draft-documentation].

Este detalhes de documento com rascunho de um cluster de Kubernetes AKS.

## <a name="prerequisites"></a>Pré-requisitos

Os passos detalhados neste documento partem do princípio de que já criou um cluster do AKS e estabeleceu uma ligação de kubectl com o cluster. Se precisar destes itens, consulte o [início rápido AKS][aks-quickstart].

Também precisa de um registo de Docker privado no registo de contentor do Azure (ACR). Para obter instruções sobre como implementar uma instância ACR, consulte o [início rápido do registo de contentor do Azure][acr-quickstart].

Helm também tem de estar instalado no seu cluster AKS. Para obter mais informações sobre como instalar helm, consulte [Helm de utilização com o serviço de contentor do Azure (AKS)][aks-helm].

Por fim, tem de instalar [Docker](https://www.docker.com).

## <a name="install-draft"></a>Instalar rascunho

A CLI de rascunho é um cliente que é executado no seu sistema de desenvolvimento e permite-lhe quicky implementa código num Kubernetes cluster. 

> [!NOTE] 
> Se instalou rascunho anteriores à versão 0.12, deve primeiro eliminar rascunho do seu cluster `helm delete --purge draft` e, em seguida, remova a configuração local executando `rm -rf ~/.draft`. Se estiver no MacOS, pode executar `brew upgrade draft`.

Para instalar a CLI de rascunho num utilize Mac `brew`. Para opções de instalação adicionais consulte, a [rascunho instalar guia][install-draft].

```console
brew tap azure/draft
brew install draft
```

A inicializar o rascunho com o `draft init` comando.

```console
draft init
```

## <a name="configure-draft"></a>Configurar rascunho

Rascunho baseia-se as imagens de contentor localmente e, em seguida, implementa-los ou no registo local (no caso de Minikube) ou tem de especificar o registo de imagem a utilizar. Este exemplo utiliza o Azure contentor registo (ACR), pelo que tem de estabelecer uma relação de confiança entre o cluster AKS e registo ACR e configurar o rascunho para enviar o contentor para o ACR.

### <a name="create-trust-between-aks-cluster-and-acr"></a>Criar confiança entre o AKS cluster e ACR

Para estabelecer fidedignidade entre um cluster AKS e um registo ACR, modifique o Azure Active Directory serviço Prinicipal utilizado com AKS ao adicionar a função de contribuinte a ela com o âmbito do repositório ACR. Para tal, execute os comandos seguintes, substituindo _&lt;aks-nome-de-rg&gt;_ e _&lt;nome do cluster aks&gt;_ com o grupo de recursos e o nome do seu AKS cluster, e _&lt;acr-rg-nom&gt;_ e _&lt;nome acr-repo&gt;_ com o nome de grupo e o repositório de recurso do ACR repositório com a qual pretende criar confiança.

```console
export AKS_SP_ID=$(az aks show -g <aks-rg-name> -n <aks-cluster-name> --query "servicePrincipalProfile.clientId" -o tsv)
export ACR_RESOURCE_ID=$(az acr show -g <acr-rg-name> -n <acr-repo-name> --query "id" -o tsv)
az role assignment create --assignee $AKS_SP_ID --scope $ACR_RESOURCE_ID --role contributor
```

(Estes passos e outros mecanismos de autenticação para aceder à ACR tenham [a autenticação com o ACR](../container-registry/container-registry-auth-aks.md).)

### <a name="configure-draft-to-push-to-and-deploy-from-acr"></a>Configurar rascunho push e implementar a partir de ACR

Agora que não existe uma relação de confiança entre AKS e ACR, os passos seguintes permitem a utilização de ACR do seu cluster AKS.
1. Definir a configuração de rascunho `registry` valor executando `draft config set registry <registry name>.azurecr.io`, onde _&lt;nome do registo&lt;_ é o nome do seu registo ACR.
2. Inicie sessão no registo ACR executando `az acr login -n <registry name>`. 

Uma vez que tem agora sessão iniciada num localmente ACR e criou uma relação de confiança com AKS e ACR, sem palavras-passe ou segredos são necessários para push ou pull do ACR para AKS. A autenticação ocorre ao nível do Azure Resource Manager, utilizando o Azure Active Directory. 

## <a name="run-an-application"></a>Executar uma aplicação

O repositório de rascunho inclui várias aplicações de exemplo que podem ser utilizados para a demonstração de rascunho. Crie uma cópia clonada do repositório.

```console
git clone https://github.com/Azure/draft
```

Mude para o diretório de exemplos de Java.

```console
cd draft/examples/example-java/
```

Utilize o `draft create` comando para iniciar o processo. Este comando cria os objetos que são utilizados para executar a aplicação num Kubernetes cluster. Estes itens incluem um Dockerfile, um gráfico de Helm e um `draft.toml` ficheiro, que é o ficheiro de configuração de rascunho.

```console
draft create
```

Saída:

```console
--> Draft detected the primary language as Java with 92.205567% certainty.
--> Ready to sail
```

Para executar a aplicação num Kubernetes cluster, utilize o `draft up` comando. Este comando cria o Dockerfile para criar uma imagem de contentor, pushes a imagem para o ACR e, finalmente, instala o gráfico de Helm para iniciar a aplicação no AKS.

Na primeira vez que este é executado, enviar e extrair a imagem de contentor podem demorar algum tempo; Depois das camadas de base são colocadas em cache, o tempo decorrido é significativamente reduzido.

```console
draft up
```

Saída:

```console
Draft Up Started: 'example-java'
example-java: Building Docker Image: SUCCESS ⚓  (1.0003s)
example-java: Pushing Docker Image: SUCCESS ⚓  (3.0007s)
example-java: Releasing Application: SUCCESS ⚓  (0.9322s)
example-java: Build ID: 01C9NPDYQQH2CZENDMZW7ESJAM
Inspect the logs with `draft logs 01C9NPDYQQH2CZENDMZW7ESJAM`
```

## <a name="test-the-application"></a>Testar a aplicação

Para testar a aplicação, utilize o `draft connect` comando. Proxies este comando uma ligação para o Kubernetes pod, permitindo uma ligação de local segura. Quando terminar, a aplicação pode ser acedida no URL fornecido.

Em alguns casos, pode demorar alguns minutos para que a imagem do contentor ser transferida e a aplicação para iniciar. Se receber um erro ao aceder a aplicação, tente novamente a ligação.

```console
draft connect
```

Saída:

```console
Connecting to your app...SUCCESS...Connect to your app on localhost:46143
Starting log streaming...
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
== Spark has ignited ...
>> Listening on 0.0.0.0:4567
```

Agora pode testar a aplicação ao navegar para http://localhost:46143 (por exemplo anterior; a porta pode ser diferente). Quando terminar de testar a utilização de aplicação `Control+C` para parar a ligação de proxy.

> [!NOTE]
> Também pode utilizar o `draft up --auto-connect` comando para criar e implementar a sua aplicação e imediatamente ligar ao contentor de execução do primeiro para tornar a iteração ciclo mesmo mais rapidamente.

## <a name="expose-application"></a>Expor a aplicação

Ao testar uma aplicação numa Kubernetes, poderá querer disponibilizar a aplicação na internet. Isto pode ser feito através de um serviço de Kubernetes com um tipo de [LoadBalancer] [ kubernetes-service-loadbalancer] ou um [controlador entrada][kubernetes-ingress]. Este detalhes de documentos através de um serviço Kubernetes.


Em primeiro lugar, o rascunho do pacote tem de ser atualizado para especificar que um serviço com um tipo `LoadBalancer` deve ser criado. Para tal, atualizar o tipo de serviço a `values.yaml` ficheiro.

```console
vi charts/java/values.yaml
```

Localize o `service.type` propriedade e atualize o valor de `ClusterIP` para `LoadBalancer`.

```yaml
replicaCount: 2
image:
  repository: openjdk
  tag: 8-jdk-alpine
  pullPolicy: IfNotPresent
service:
  name: java
  type: LoadBalancer
  externalPort: 80
  internalPort: 4567
resources:
  limits:
    cpu: 100m
    memory: 128Mi
  requests:
    cpu: 100m
    memory: 128Mi
  ```

Executar `draft up` para executar novamente a aplicação.

```console
draft up
```

Pode demorar alguns minutos para que o serviço devolver um endereço IP público. Para monitorizar a utilização de progresso de `kubectl get service` comando com uma veja.

```console
kubectl get service -w
```

Inicialmente, o *IP externo* para o serviço é apresentado como `pending`.

```
example-java-java   10.0.141.72   <pending>     80:32150/TCP   14m
```

Quando o endereço EXTERNAL-IP mudar de `pending` para `IP address`, utilize `Control+C` para parar o processo de observação do kubectl.

```
example-java-java   10.0.141.72   52.175.224.118   80:32150/TCP   17m
```

Para ver a aplicação, navegue para o endereço IP externo.

```console
curl 52.175.224.118
```

Saída:

```
Hello World, I'm Java
```

## <a name="iterate-on-the-application"></a>Repetir a aplicação

Agora que foi configurado um rascunho e a aplicação é executada no Kubernetes, são definidas para iteração do código. Código de sempre que pretende testar atualizada, execute o `draft up` comando para atualizar a aplicação em execução.

Neste exemplo, atualize a aplicação do Java Olá mundo.

```console
vi src/main/java/helloworld/Hello.java
```

Atualize o texto de Olá, mundo.

```java
package helloworld;

import static spark.Spark.*;

public class Hello {
    public static void main(String[] args) {
        get("/", (req, res) -> "Hello World, I'm Java in AKS!");
    }
}
```

Execute o `draft up --auto-connect` comando para voltar a implementar a aplicação apenas assim que um pod está pronto para responder.

```console
draft up --auto-connect
```

Saída

```
Draft Up Started: 'example-java'
example-java: Building Docker Image: SUCCESS ⚓  (1.0003s)
example-java: Pushing Docker Image: SUCCESS ⚓  (4.0010s)
example-java: Releasing Application: SUCCESS ⚓  (1.1336s)
example-java: Build ID: 01C9NPMJP6YM985GHKDR2J64KC
Inspect the logs with `draft logs 01C9NPMJP6YM985GHKDR2J64KC`
Connect to java:4567 on localhost:39249
Your connection is still active.
Connect to java:4567 on localhost:39249
[java]: SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
[java]: SLF4J: Defaulting to no-operation (NOP) logger implementation
[java]: SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
[java]: == Spark has ignited ...
[java]: >> Listening on 0.0.0.0:4567

```

Por fim, ver a aplicação para ver as atualizações.

```console
curl 52.175.224.118
```

Saída:

```
Hello World, I'm Java in AKS!
```

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre a utilização de rascunho, consulte a documentação de rascunho no GitHub.

> [!div class="nextstepaction"]
> [Documentação de rascunho][draft-documentation]

<!-- LINKS - external -->
[draft-documentation]: https://github.com/Azure/draft/tree/master/docs
[install-draft]: https://github.com/Azure/draft/blob/master/docs/install.md
[kubernetes-ingress]: ./ingress.md
[kubernetes-service-loadbalancer]: https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer

<!-- LINKS - internal -->
[acr-quickstart]: ../container-registry/container-registry-get-started-azure-cli.md
[aks-helm]: ./kubernetes-helm.md
[aks-quickstart]: ./kubernetes-walkthrough.md
