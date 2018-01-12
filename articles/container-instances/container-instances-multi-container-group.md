---
title: "Implementar o contentor de vários grupos em instâncias de contentor do Azure"
description: "Saiba como implementar um grupo contentor com vários contentores em instâncias de contentor do Azure."
services: container-instances
author: neilpeterson
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 01/10/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 41a47adb1f1da417038757934f0a6cf7e11555da
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2018
---
# <a name="deploy-a-container-group"></a>Implementar um grupo contentor

Instâncias de contentor do Azure suporta a implementação de vários contentores para um anfitrião único através de um [grupo contentor](container-instances-container-groups.md). Isto é útil ao criar um sidecar de aplicação para o registo, monitorização ou qualquer outra configuração em que um processo anexado segundo as necessita de um serviço.

Este documento explica como executar uma configuração simples contentor multi sidecar ao implementar um modelo Azure Resource Manager.

> [!NOTE]
> Os grupos de contentor multi são atualmente restritos para contentores de Linux. Enquanto que estamos a trabalhar para colocar todas as funcionalidades de contentores do Windows, pode encontrar a atual plataforma as diferenças no [Quotas e disponibilidade de região para instâncias de contentor do Azure](container-instances-quotas.md).

## <a name="configure-the-template"></a>Configurar o modelo

Crie um ficheiro denominado `azuredeploy.json` e copie o seguinte JSON para a mesma.

Neste exemplo, um grupo contentor com dois contentores, é definido um endereço IP público e duas portas expostas. O primeiro contentor no grupo executa uma aplicação da Internet. O contentor segundo, sidecar, faz um pedido de HTTP para a aplicação web principal através de rede local do grupo.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {
    "container1name": "aci-tutorial-app",
    "container1image": "microsoft/aci-helloworld:latest",
    "container2name": "aci-tutorial-sidecar",
    "container2image": "microsoft/aci-tutorial-sidecar"
  },
  "resources": [
    {
      "name": "myContainerGroup",
      "type": "Microsoft.ContainerInstance/containerGroups",
      "apiVersion": "2017-10-01-preview",
      "location": "[resourceGroup().location]",
      "properties": {
        "containers": [
          {
            "name": "[variables('container1name')]",
            "properties": {
              "image": "[variables('container1image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              },
              "ports": [
                {
                  "port": 80
                },
                {
                  "port": 8080
                }
              ]
            }
          },
          {
            "name": "[variables('container2name')]",
            "properties": {
              "image": "[variables('container2image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              }
            }
          }
        ],
        "osType": "Linux",
        "ipAddress": {
          "type": "Public",
          "ports": [
            {
              "protocol": "tcp",
              "port": "80"
            },
            {
                "protocol": "tcp",
                "port": "8080"
            }
          ]
        }
      }
    }
  ],
  "outputs": {
    "containerIPv4Address": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.ContainerInstance/containerGroups/', 'myContainerGroup')).ipAddress.ip]"
    }
  }
}
```

Para utilizar um registo de imagem do contentor privada, adicione um objeto para o documento JSON com o seguinte formato.

```json
"imageRegistryCredentials": [
  {
    "server": "[parameters('imageRegistryLoginServer')]",
    "username": "[parameters('imageRegistryUsername')]",
    "password": "[parameters('imageRegistryPassword')]"
  }
]
```

## <a name="deploy-the-template"></a>Implementar o modelo

Crie um grupo de recursos com o comando [az group create][az-group-create].

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Implementar o modelo com o [criar a implementação do grupo az] [ az-group-deployment-create] comando.

```azurecli-interactive
az group deployment create --resource-group myResourceGroup --name myContainerGroup --template-file azuredeploy.json
```

Dentro de alguns segundos, deverá receber uma resposta inicial a partir do Azure.

## <a name="view-deployment-state"></a>Ver o estado de implementação

Para ver o estado da implementação, utilize o [mostrar de contentor az] [ az-container-show] comando. Esta ação devolve o endereço IP público aprovisionado, pelo que a aplicação pode ser acedida.

```azurecli-interactive
az container show --resource-group myResourceGroup --name myContainerGroup --output table
```

Saída:

```bash
Name              ResourceGroup    ProvisioningState    Image                                                           IP:ports               CPU/Memory       OsType    Location
----------------  ---------------  -------------------  --------------------------------------------------------------  ---------------------  ---------------  --------  ----------
myContainerGroup  myResourceGroup  Succeeded            microsoft/aci-helloworld:latest,microsoft/aci-tutorial-sidecar  52.168.26.124:80,8080  1.0 core/1.5 gb  Linux     westus
```

## <a name="view-logs"></a>Ver registos

Ver o resultado de registo de um contentor utilizando o [az contentor registos] [ az-container-logs] comando. O `--container-name` argumento especifica o contentor a partir da qual pretende extrair os registos. Neste exemplo, o primeiro contentor está especificado.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-app
```

Saída:

```bash
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

```bash
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

## <a name="next-steps"></a>Passos Seguintes

Este artigo abrange os passos necessários para implementar uma instância de contentor do Azure multi contentor. Para uma experiência de instâncias de contentor do Azure do ponto-a-ponto, consulte o tutorial de instâncias de contentor do Azure.

> [!div class="nextstepaction"]
> [Tutorial de instâncias de contentor do Azure][aci-tutorial]

<!-- LINKS - Internal -->
[aci-tutorial]: ./container-instances-tutorial-prepare-app.md
[az-container-logs]: /cli/azure/container#az_container_logs
[az-container-show]: /cli/azure/container#az_container_show
[az-group-create]: /cli/azure/group#az_group_create
[az-group-deployment-create]: /cli/azure/group/deployment#az_group_deployment_create
