---
title: Autenticar com o registo de contentores do Azure de serviço Kubernetes do Azure
description: Saiba como fornecer acesso a imagens no seu registo de contentor privadas do serviço de Kubernetes do Azure ao utilizar um principal de serviço do Azure Active Directory.
services: container-service
author: mmacy
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 02/24/2018
ms.author: marsma
ms.openlocfilehash: 36b66fdcd157e4c44fcd451c8cc07ba68242b583
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888767"
---
# <a name="authenticate-with-azure-container-registry-from-azure-kubernetes-service"></a>Autenticar com o registo de contentores do Azure de serviço Kubernetes do Azure

Ao utilizar o Azure Container Registry (ACR) com o Azure Kubernetes Service (AKS), um mecanismo de autenticação deve ser estabelecido. Este documento detalha as configurações recomendadas para a autenticação entre esses dois serviços do Azure.

## <a name="grant-aks-access-to-acr"></a>Acesso de AKS de concessão para o ACR

Quando é criado um cluster do AKS, também é criado um principal de serviço para gerir a capacidade de operação de cluster com recursos do Azure. Este principal de serviço também pode ser utilizado para a autenticação com um registo do ACR. Para fazer isso, uma atribuição de função tem de ser criado para conceder o acesso de leitura principal do serviço para o recurso do ACR.

O exemplo a seguir pode ser utilizado para concluir esta operação.

```bash
#!/bin/bash

AKS_RESOURCE_GROUP=myAKSResourceGroup
AKS_CLUSTER_NAME=myAKSCluster
ACR_RESOURCE_GROUP=myACRResourceGroup
ACR_NAME=myACRRegistry

# Get the id of the service principal configured for AKS
CLIENT_ID=$(az aks show --resource-group $AKS_RESOURCE_GROUP --name $AKS_CLUSTER_NAME --query "servicePrincipalProfile.clientId" --output tsv)

# Get the ACR registry resource id
ACR_ID=$(az acr show --name $ACR_NAME --resource-group $ACR_RESOURCE_GROUP --query "id" --output tsv)

# Create role assignment
az role assignment create --assignee $CLIENT_ID --role Reader --scope $ACR_ID
```

## <a name="access-with-kubernetes-secret"></a>Acesso com o segredo do Kubernetes

Em alguns casos, o que está a ser utilizado pelo AKS principal de serviço não pode ser confinada para o registo do ACR. Nesses casos, pode criar um principal de serviço exclusivo e definir o âmbito para apenas o registo do ACR.

O seguinte script pode ser utilizado para criar o serviço principal.

```bash
#!/bin/bash

ACR_NAME=myacrinstance
SERVICE_PRINCIPAL_NAME=acr-service-principal

# Populate the ACR login server and resource id.
ACR_LOGIN_SERVER=$(az acr show --name $ACR_NAME --query loginServer --output tsv)
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)

# Create a contributor role assignment with a scope of the ACR resource.
SP_PASSWD=$(az ad sp create-for-rbac --name $SERVICE_PRINCIPAL_NAME --role Reader --scopes $ACR_REGISTRY_ID --query password --output tsv)

# Get the service principle client id.
CLIENT_ID=$(az ad sp show --id http://$SERVICE_PRINCIPAL_NAME --query appId --output tsv)

# Output used when creating Kubernetes secret.
echo "Service principal ID: $CLIENT_ID"
echo "Service principal password: $SP_PASSWD"
```

As credenciais do principal de serviço agora podem ser armazenadas no Kubernetes [segredo de solicitação de imagem] [ image-pull-secret] e referenciado quando contentores em execução num cluster do AKS.

O comando seguinte cria segredo do Kubernetes. Substitua o nome do servidor com o servidor de início de sessão do ACR, o nome de utilizador com o id de principal de serviço e a palavra-passe com a palavra-passe de principal de serviço.

```bash
kubectl create secret docker-registry acr-auth --docker-server <acr-login-server> --docker-username <service-principal-ID> --docker-password <service-principal-password> --docker-email <email-address>
```

O segredo do Kubernetes pode ser usado numa implementação de pod com o `ImagePullSecrets` parâmetro.

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: acr-auth-example
spec:
  template:
    metadata:
      labels:
        app: acr-auth-example
    spec:
      containers:
      - name: acr-auth-example
        image: myacrregistry.azurecr.io/acr-auth-example
      imagePullSecrets:
      - name: acr-auth
```

<!-- LINKS - external -->
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[image-pull-secret]: https://kubernetes.io/docs/concepts/configuration/secret/#using-imagepullsecrets
