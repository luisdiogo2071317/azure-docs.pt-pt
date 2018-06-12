---
title: Utilizar OpenFaaS serviço Kubernetes do Azure (AKS)
description: Implementar e utilizar OpenFaaS com o serviço do Azure Kubernetes (AKS)
services: container-service
author: justindavies
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 03/05/2018
ms.author: juda
ms.custom: mvc
ms.openlocfilehash: b5484233c7d3d32e51098baad8c22ec51df8f0d8
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35260622"
---
# <a name="using-openfaas-on-aks"></a>Utilizar OpenFaaS no AKS

[OpenFaaS] [ open-faas] é uma estrutura para a criação de funções sem servidor por cima de contentores. Como um projeto de código aberto, este tem adquiridos adoção em grande escala dentro da Comunidade. Este documento fornece detalhes sobre a instalação e utilização OpenFaas num cluster do serviço de Kubernetes do Azure (AKS).

## <a name="prerequisites"></a>Pré-requisitos

Para concluir os passos neste artigo, precisa do seguinte.

* Compreensão básica do Kubernetes.
* Um cluster do serviço de Kubernetes do Azure (AKS) e as credenciais AKS configuradas no sistema de desenvolvimento.
* CLI do Azure instalado no seu sistema de desenvolvimento.
* Ferramentas da linha de comandos de Git instaladas no seu sistema.

## <a name="get-openfaas"></a>Obter OpenFaaS

Clone o repositório de projeto OpenFaaS ao seu sistema de desenvolvimento.

```azurecli-interactive
git clone https://github.com/openfaas/faas-netes
```

Alterar para o diretório do repositório clonado.

```azurecli-interactive
cd faas-netes
```

## <a name="deploy-openfaas"></a>Implementar OpenFaaS

Como uma boa prática, OpenFaaS e OpenFaaS funções devem ser armazenadas no seu próprio espaço de nomes Kubernetes.

Crie um espaço de nomes para o sistema OpenFaaS.

```azurecli-interactive
kubectl create namespace openfaas
```

Crie um espaço de nomes segundo para OpenFaaS funções.

```azurecli-interactive
kubectl create namespace openfaas-fn
```

Um gráfico de Helm para OpenFaaS está incluído no repositório de clonado. Utilize este gráfico para implementar OpenFaaS no seu cluster AKS.

```azurecli-interactive
helm install --namespace openfaas -n openfaas \
  --set functionNamespace=openfaas-fn, \
  --set serviceType=LoadBalancer, \
  --set rbac=false chart/openfaas/
```

Saída:

```
NAME:   openfaas
LAST DEPLOYED: Wed Feb 28 08:26:11 2018
NAMESPACE: openfaas
STATUS: DEPLOYED

RESOURCES:
==> v1/ConfigMap
NAME                 DATA  AGE
prometheus-config    2     20s
alertmanager-config  1     20s

{snip}

NOTES:
To verify that openfaas has started, run:

  kubectl --namespace=openfaas get deployments -l "release=openfaas, app=openfaas"
```

É criado um endereço IP público para aceder ao OpenFaaS gateway. Para obter este endereço IP, utilize o [kubectl obter serviço] [ kubectl-get] comando. Pode demorar um minuto para o endereço IP a ser atribuída para este serviço.

```console
kubectl get service -l component=gateway --namespace openfaas
```

Saída.

```console
NAME               TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)          AGE
gateway            ClusterIP      10.0.156.194   <none>         8080/TCP         7m
gateway-external   LoadBalancer   10.0.28.18     52.186.64.52   8080:30800/TCP   7m
```

Para testar o sistema OpenFaaS, navegue para o endereço IP externo na porta 8080, `http://52.186.64.52:8080` neste exemplo.

![IU OpenFaaS](media/container-service-serverless/openfaas.png)

Por fim, instale a CLI de OpenFaaS. Neste exemplo utilizado brew, consulte o [documentação OpenFaaS CLI] [ open-faas-cli] para mais opções.

```console
brew install faas-cli
```

## <a name="create-first-function"></a>Criar a primeira função

Agora que OpenFaaS está operacional, crie uma função utilizando o portal de OpenFaas.

Clique em **implementar nova função** e procure **Figlet**. Selecione a função de Figlet e clique em **implementar**.

![Figlet](media/container-service-serverless/figlet.png)

Utilize curl para invocar a função. Substitua o endereço IP no exemplo a seguir a que o gateway de OpenFaas.

```azurecli-interactive
curl -X POST http://52.186.64.52:8080/function/figlet -d "Hello Azure"
```

Saída:

```console
 _   _      _ _            _
| | | | ___| | | ___      / \    _____   _ _ __ ___
| |_| |/ _ \ | |/ _ \    / _ \  |_  / | | | '__/ _ \
|  _  |  __/ | | (_) |  / ___ \  / /| |_| | | |  __/
|_| |_|\___|_|_|\___/  /_/   \_\/___|\__,_|_|  \___|

```

## <a name="create-second-function"></a>Criar uma segunda função

Crie uma segunda função. Neste exemplo será implementado utilizando a CLI OpenFaaS e inclui uma imagem personalizada contentor e obter dados a partir de uma base de dados do Cosmos. Vários itens têm de ser configurados antes de criar a função.

Em primeiro lugar, crie um novo grupo de recursos para a BD do Cosmos.

```azurecli-interactive
az group create --name serverless-backing --location eastus
```

Implemente uma instância de CosmosDB de tipo `MongoDB`. A instância tem um nome exclusivo, atualize `openfaas-cosmos` para algo exclusivo para o seu ambiente.

```azurecli-interactive
az cosmosdb create --resource-group serverless-backing --name openfaas-cosmos --kind MongoDB
```

Obter a cadeia de ligação de base de dados do Cosmos e armazená-las numa variável.

Atualize o valor para o `--resource-group` argumento para o nome do grupo de recursos e o `--name` argumento para o nome da sua base de dados do Cosmos.

```azurecli-interactive
COSMOS=$(az cosmosdb list-connection-strings \
  --resource-group serverless-backing \
  --name openfaas-cosmos \
  --query connectionStrings[0].connectionString \
  --output tsv)
```

Agora a preencher a base de dados do Cosmos com dados de teste. Crie um ficheiro denominado `plans.json` e copie o seguinte json.

```json
{
    "name" : "two_person",
    "friendlyName" : "Two Person Plan",
    "portionSize" : "1-2 Person",
    "mealsPerWeek" : "3 Unique meals per week",
    "price" : 72,
    "description" : "Our basic plan, delivering 3 meals per week, which will feed 1-2 people.",
    "__v" : 0
}
```

Utilize o *mongoimport* ferramenta para a instância de CosmosDB com dados de carga.

Se necessário, instale as ferramentas do MongoDB. O exemplo seguinte instala estas ferramentas brew a utilizar, consulte o [documentação do MongoDB] [ install-mongo] para outras opções.

```azurecli-interactive
brew install mongodb
```

Carregar os dados para a base de dados.

```azurecli-interactive
mongoimport --uri=$COSMOS -c plans < plans.json
```

Saída:

```console
2018-02-19T14:42:14.313+0000    connected to: localhost
2018-02-19T14:42:14.918+0000    imported 1 document
```

Execute o seguinte comando para criar a função. Atualize o valor da `-g` argumento com o seu endereço de gateway OpenFaaS.

```azurecli-interctive
faas-cli deploy -g http://52.186.64.52:8080 --image=shanepeckham/openfaascosmos --name=cosmos-query --env=NODE_ENV=$COSMOS
```

Depois de implementada, deverá ver o ponto final de OpenFaaS recentemente criado para a função.

```console
Deployed. 202 Accepted.
URL: http://52.186.64.52:8080/function/cosmos-query
```

Teste a função utilizando curl. Atualize o endereço IP com o endereço de gateway OpenFaaS.

```console
curl -s http://52.186.64.52:8080/function/cosmos-query
```

Saída:

```json
[{"ID":"","Name":"two_person","FriendlyName":"","PortionSize":"","MealsPerWeek":"","Price":72,"Description":"Our basic plan, delivering 3 meals per week, which will feed 1-2 people."}]
```

Também pode testar a função dentro da IU OpenFaaS.

![texto alternativo](media/container-service-serverless/OpenFaaSUI.png)

## <a name="next-steps"></a>Próximos Passos

A implementação predefinida do OpenFaas tem de ser bloqueados para funções e OpenFaaS Gateway. [Mensagem de blogue de Alex Ellis](https://blog.alexellis.io/lock-down-openfaas/) inclui mais detalhes sobre as opções de configuração segura.

<!-- LINKS - external -->
[install-mongo]: https://docs.mongodb.com/manual/installation/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[open-faas]: https://www.openfaas.com/
[open-faas-cli]: https://github.com/openfaas/faas-cli
