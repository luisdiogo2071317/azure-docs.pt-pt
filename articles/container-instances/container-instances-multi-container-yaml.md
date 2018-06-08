---
title: Implementar o contentor de vários grupos em instâncias de contentor do Azure com a CLI do Azure e YAML
description: Saiba como implementar um grupo contentor com vários contentores em instâncias de contentor Azure utilizando a CLI do Azure e um ficheiro YAML.
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 06/08/2018
ms.author: marsma
ms.openlocfilehash: 5dfee15e978d2dba0f50d1dc4b78953698389950
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "34851330"
---
# <a name="deploy-a-multi-container-container-group-with-yaml"></a>Implementar um grupo contentor multi contentor com YAML

Instâncias de contentor do Azure suporta a implementação de vários contentores para um anfitrião único utilizando um [grupo contentor](container-instances-container-groups.md). Contentor de contentor multi os grupos é úteis quando criar um sidecar de aplicação para o registo, monitorização ou qualquer outra configuração em que um processo anexado segundo as necessita de um serviço.

Existem dois métodos para implementar grupos de contentor multi utilizando a CLI do Azure:

* Implementação de ficheiros YAML (Este artigo)
* [Implementação de modelo do Resource Manager](container-instances-multi-container-group.md)

Devido à natureza mais concisa o formato YAML implementação com um ficheiro YAML recomenda-se quando a implementação inclui *apenas* instâncias de contentor. Se precisar de implementar recursos adicionais do serviço do Azure (por exemplo, uma partilha de ficheiros do Azure) no momento da implementação de instância de contentor, recomenda-se a implementação de modelo do Resource Manager.

> [!NOTE]
> Os grupos de contentor multi são atualmente restritos para contentores de Linux. Enquanto que estamos a trabalhar para colocar todas as funcionalidades de contentores do Windows, pode encontrar a atual plataforma as diferenças no [Quotas e disponibilidade de região para instâncias de contentor do Azure](container-instances-quotas.md).

## <a name="configure-the-yaml-file"></a>Configurar o ficheiro YAML

Para implementar um grupo contentor multi contentor com o [criar contentor de az] [ az-container-create] comando na CLI do Azure, tem de especificar a configuração do grupo contentor num ficheiro YAML e passar o ficheiro YAML como um parâmetro para o comando.

Comece por copiar o seguinte YAML para um novo ficheiro designado **aci.yaml implementar**.

Este ficheiro YAML define um grupo contentor com dois contentores, um endereço IP público e duas portas expostas. O primeiro contentor no grupo executa uma aplicação de web para a internet. O contentor segundo, sidecar, periodicamente faz pedidos de HTTP para a aplicação web em execução no contentor do primeiro através da rede local do grupo contentor.

```YAML
apiVersion: 2018-06-01
location: eastus
name: myContainerGroup
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      image: microsoft/aci-helloworld:latest
      resources:
        requests:
          cpu: 1
          memoryInGb: 1.5
      ports:
      - port: 80
      - port: 8080
  - name: aci-tutorial-sidecar
    properties:
      image: microsoft/aci-tutorial-sidecar
      resources:
        requests:
          cpu: 1
          memoryInGb: 1.5
  osType: Linux
  ipAddress:
    type: Public
    ports:
    - protocol: tcp
      port: '80'
    - protocol: tcp
      port: '8080'
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

## <a name="deploy-the-container-group"></a>Implementar o grupo de contentor

Criar um grupo de recursos com o [criar grupo az] [ az-group-create] comando:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Implementar o grupo de contentor com o [criar contentor de az] [ az-container-create] comando, passar o ficheiro YAML como um argumento:

```azurecli-interactive
az container create --resource-group myResourceGroup --name myContainerGroup -f deploy-aci.yaml
```

Dentro de alguns segundos, deverá receber uma resposta inicial do Azure.

## <a name="view-deployment-state"></a>Ver o estado de implementação

Para ver o estado da implementação, utilize o seguinte [mostrar de contentor az] [ az-container-show] comando:

```azurecli-interactive
az container show --resource-group myResourceGroup --name myContainerGroup --output table
```

Se gostaria de ver a aplicação em execução, navegue para o endereço IP no seu browser. Por exemplo, o IP está `52.168.26.124` neste resultado de exemplo:

```bash
Name              ResourceGroup    ProvisioningState    Image                                                           IP:ports               CPU/Memory       OsType    Location
----------------  ---------------  -------------------  --------------------------------------------------------------  ---------------------  ---------------  --------  ----------
myContainerGroup  myResourceGroup  Succeeded            microsoft/aci-helloworld:latest,microsoft/aci-tutorial-sidecar  52.168.26.124:80,8080  1.0 core/1.5 gb  Linux     eastus
```

## <a name="view-logs"></a>Ver registos

Ver o resultado de registo de um contentor utilizando o [az contentor registos] [ az-container-logs] comando. O `--container-name` argumento especifica o contentor a partir da qual pretende extrair os registos. Neste exemplo, o primeiro contentor está especificado.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-app
```

Saída:

```console
listening on port 80
::1 - - [09/Jan/2018:23:17:48 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [09/Jan/2018:23:17:51 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [09/Jan/2018:23:17:54 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

Para ver os registos para o contentor do lado do automóvel, execute o mesmo comando, especificando o segundo nome do contentor.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-sidecar
```

Saída:

```console
Every 3s: curl -I http://localhost                          2018-01-09 23:25:11

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0  1663    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
HTTP/1.1 200 OK
X-Powered-By: Express
Accept-Ranges: bytes
Cache-Control: public, max-age=0
Last-Modified: Wed, 29 Nov 2017 06:40:40 GMT
ETag: W/"67f-16006818640"
Content-Type: text/html; charset=UTF-8
Content-Length: 1663
Date: Tue, 09 Jan 2018 23:25:11 GMT
Connection: keep-alive
```

Como pode ver, a sidecar periodicamente é efetuar um pedido de HTTP para a aplicação web principal através de rede local do grupo para se certificar de que está em execução. Neste exemplo sidecar foi ser expandido para acionar um alerta se recebeu um código de resposta HTTP diferente de 200 OK.

## <a name="deploy-from-private-registry"></a>Implementar a partir do registo privado

Para utilizar um registo de imagem do contentor privada, incluem o seguinte YAML com valores modificados para o seu ambiente:

```YAML
  imageRegistryCredentials:
  - server: imageRegistryLoginServer
    username: imageRegistryUsername
    password: imageRegistryPassword
```

Por exemplo, o seguinte YAML implementa um grupo contentor com um único contentor cuja imagem é retirada de um registo de contentor do Azure privada com o nome "myregistry":

```YAML
apiVersion: 2018-06-01
location: eastus
name: myContainerGroup2
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      image: myregistry.azurecr.io/aci-helloworld:latest
      resources:
        requests:
          cpu: 1
          memoryInGb: 1.5
      ports:
      - port: 80
  osType: Linux
  ipAddress:
    type: Public
    ports:
    - protocol: tcp
      port: '80'
  imageRegistryCredentials:
  - server: myregistry.azurecr.io
    username: myregistry
    password: REGISTRY_PASSWORD
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

## <a name="export-container-group-to-yaml"></a>Exportar grupo contentor para YAML

Pode exportar a configuração de um grupo contentor existente para um ficheiro YAML utilizando o comando da CLI do Azure [exportação de contentor az][az-container-export].

Útil para preservar a configuração de um grupo contentor, exportação permite-lhe armazenar as configurações de grupo do contentor no controlo de versão para "configuração como código". Em alternativa, utilize o ficheiro exportado como um ponto de partida quando desenvolver uma nova configuração de YAML.

Exportar a configuração para o grupo de contentor que criou anteriormente ao emitir o seguinte [exportação de contentor az] [ az-container-export] comando:

```azurecli-interactive
az container export --resource-group rg604 --name myContainerGroup --file deployed-aci.yaml
```

Nenhuma saída é apresentada se o comando for concluída com êxito, mas pode ver o conteúdo do ficheiro para ver o resultado. Por exemplo, as primeiro algumas linhas com `head`:

```console
$ head deployed-aci.yaml
apiVersion: 2018-02-01-preview
location: eastus
name: myContainerGroup
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      environmentVariables: []
      image: microsoft/aci-helloworld:latest
      ports:
```

> [!NOTE]
> A partir da CLI do Azure versão 2.0.34, há um [conhecido problema] [ cli-issue-6525] no qual exportou os grupos de contentor especifiquem uma versão de API antiga do **2018-02-01-preview** (visto no anterior Exemplo de saída JSON). Se gostaria de voltar a implementar através do ficheiro YAML exportado, com segurança pode atualizar o `apiVersion` valor no ficheiro YAML exportado para **2018-06-01**.

## <a name="next-steps"></a>Passos Seguintes

Este artigo abrange os passos necessários para implementar uma instância de contentor do Azure multi contentor. Para uma experiência de instâncias de contentor do Azure ponto-a-ponto, incluindo a utilização de um registo de contentor do Azure privado, consulte o tutorial de instâncias de contentor do Azure.

> [!div class="nextstepaction"]
> [Tutorial do Azure Container Instances][aci-tutorial]

<!-- LINKS - External -->
[cli-issue-6525]: https://github.com/Azure/azure-cli/issues/6525

<!-- LINKS - Internal -->
[aci-tutorial]: ./container-instances-tutorial-prepare-app.md
[az-container-create]: /cli/azure/container#az-container-create
[az-container-export]: /cli/azure/container#az-container-export
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-create]: /cli/azure/group#az-group-create
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[template-reference]: https://docs.microsoft.com/azure/templates/microsoft.containerinstance/containergroups
