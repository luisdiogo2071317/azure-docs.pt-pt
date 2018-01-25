## <a name="create-a-service-principal"></a>Criar um principal de serviço

Para criar um principal de serviço com acesso ao seu registo de contentor, pode utilizar o seguinte script. Atualização o `ACR_NAME` variável com o nome do seu registo de contentor e, opcionalmente, o `--role` valor o [az ad sp criar-para-rbac] [ az-ad-sp-create-for-rbac] comando para conceder permissões diferentes. Tem de ter o [CLI do Azure](/cli/azure/install-azure-cli) instalada para utilizar este script.

Depois de executar o script, tome nota do principal de serviço **ID** e **palavra-passe**. Depois de ter as respetivas credenciais, pode configurar as suas aplicações e serviços para autenticar para o registo do contentor como o principal de serviço.

```bash
#!/bin/bash

# Modify for your environment. The ACR_NAME is the name of your Azure Container
# Registry, and the SERVICE_PRINCIPAL_NAME can be any unique name within your
# subscription (you can use the default below).
ACR_NAME=myregistryname
SERVICE_PRINCIPAL_NAME=acr-service-principal

# Populate some values required for subsequent command args
ACR_LOGIN_SERVER=$(az acr show --name $ACR_NAME --query loginServer --output tsv)
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)

# Create the service principal with rights scoped to the registry.
# Default permissions are for both docker push and pull access. Modify the
# '--role' argument value as desired:
# reader:      pull only
# contributor: push and pull
# owner:       push, pull, and assign roles
SP_PASSWD=$(az ad sp create-for-rbac --name $SERVICE_PRINCIPAL_NAME --scopes $ACR_REGISTRY_ID --role reader --query password --output tsv)
SP_APP_ID=$(az ad sp show --id http://$SERVICE_PRINCIPAL_NAME --query appId --output tsv)

# Output the service principal's credentials; use these in your services and
# applications to authenticate to the container registry.
echo "Service principal ID: $SP_APP_ID"
echo "Service principal password: $SP_PASSWD"
```

## <a name="use-an-existing-service-principal"></a>Utilize um principal de serviço existente

Para conceder acesso de registo para um principal de serviço existente, tem de atribuir uma nova função para o principal de serviço. Tal como acontece com a criação de um novo serviço principal, pode conceder solicitação, push e pull e acesso de proprietário.

O script seguinte utiliza o [de criação da atribuição de função az] [ az-role-assignment-create] comando para conceder *solicitação* permissões para um principal de serviço que especificar no `SERVICE_PRINCIPAL_ID` variável. Ajustar o `--role` valor se desejar conceder um nível de acesso diferente.

```bash
#!/bin/bash

# Modify for your environment. The ACR_NAME is the name of your Azure Container
# Registry, and the SERVICE_PRINCIPAL_ID is the service principal's 'appId' or
# one of its 'servicePrincipalNames' values.
ACR_NAME=myregistryname
SERVICE_PRINCIPAL_ID=<service-principal-ID>

# Populate value required for subsequent command args
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)

# Assign the desired role to the service principal. Modify the '--role' argument
# value as desired:
# reader:      pull only
# contributor: push and pull
# owner:       push, pull, and assign roles
az role assignment create --assignee $SERVICE_PRINCIPAL_ID --scope $ACR_REGISTRY_ID --role reader
```

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az_ad_sp_create_for_rbac
[az-role-assignment-create]: /cli/azure/role/assignment#az_role_assignment_create