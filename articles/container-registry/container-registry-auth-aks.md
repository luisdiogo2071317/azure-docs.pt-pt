---
title: "Autenticar com o registo de contentor do Azure do serviço de contentor do Azure"
description: "Saiba como fornecer acesso a imagens no seu registo de contentor privada do serviço de contentor Azure utilizando um principal de serviço do Azure Active Directory."
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 1/12/2018
ms.author: nepeters
ms.openlocfilehash: 86a160d8f2dbfb0e385d9dbed7cf6d789f5a8df6
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2018
---
# <a name="authenticate-with-azure-container-registry-from-azure-container-service"></a>Autenticar com o registo de contentor do Azure do serviço de contentor do Azure

Quando estiver a utilizar o registo de contentor do Azure (ACR) com o serviço de contentor do Azure (AKS), um mecanismo de autenticação tem de ser estabelecida. Este documento fornece detalhes sobre as configurações recomendadas para a autenticação entre esses dois serviços do Azure.

## <a name="grant-aks-access-to-acr"></a>Acesso de AKS conceder ao ACR

Quando é criado um cluster AKS, um principal de serviço também é criado para gerir operability de cluster com recursos do Azure. Este principal de serviço também pode ser utilizado para autenticação com um registo ACR. Para tal, uma atribuição de função tem de ser criada para conceder o acesso de leitura principal do serviço para o recurso ACR. 

O exemplo seguinte pode ser utilizado para concluir esta operação.

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

## <a name="access-with-kubernetes-secret"></a>Acesso com Kubernetes segredo

Em alguns casos, o serviço principal que está a ser utilizado por AKS não pode ser confinado ao registo ACR. Para estes casos, pode criar um principal de serviço exclusivo e definir o âmbito apenas no registo de ACR.

O script seguinte pode ser utilizado para criar o serviço principal. 

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

As credenciais de principal de serviço agora podem ser armazenadas num Kubernetes [segredo de solicitação de imagem] [ image-pull-secret] e referenciado durante a execução contentores num AKS cluster. 

O comando seguinte cria o Kubernetes secreta. Substitua o nome do servidor com o servidor de início de sessão ACR, o nome de utilizador com o id de principal de serviço e a palavra-passe com a palavra-passe de principal de serviço.

```bash
kubectl create secret docker-registry acr-auth --docker-server <acr-login-server> --docker-username <service-principal-ID> --docker-password <service-principal-password> --docker-email <email-address>
```

O segredo Kubernetes pode ser utilizado num pod implementação com o `ImagePullSecrets` parâmetro. 

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
