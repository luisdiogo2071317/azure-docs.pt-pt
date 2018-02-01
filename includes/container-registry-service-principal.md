## <a name="create-a-service-principal"></a>Criar um principal de serviço

Para criar um principal de serviço com acesso ao seu registo de contentor, pode utilizar o seguinte script. Atualização o `ACR_NAME` variável com o nome do seu registo de contentor e, opcionalmente, o `--role` valor o [az ad sp criar-para-rbac] [ az-ad-sp-create-for-rbac] comando para conceder permissões diferentes. Tem de ter o [CLI do Azure](/cli/azure/install-azure-cli) instalada para utilizar este script.

Depois de executar o script, tome nota do principal de serviço **ID** e **palavra-passe**. Depois de ter as respetivas credenciais, pode configurar as suas aplicações e serviços para autenticar para o registo do contentor como o principal de serviço.

[!code-azurecli-interactive[acr-sp-create](~/cli_scripts/container-registry/service-principal-create/service-principal-create.sh)]

## <a name="use-an-existing-service-principal"></a>Utilize um principal de serviço existente

Para conceder acesso de registo para um principal de serviço existente, tem de atribuir uma nova função para o principal de serviço. Tal como acontece com a criação de um novo serviço principal, pode conceder solicitação, push e pull e acesso de proprietário.

O script seguinte utiliza o [de criação da atribuição de função az] [ az-role-assignment-create] comando para conceder *solicitação* permissões para um principal de serviço que especificar no `SERVICE_PRINCIPAL_ID` variável. Ajustar o `--role` valor se desejar conceder um nível de acesso diferente.

[!code-azurecli-interactive[acr-sp-role-assign](~/cli_scripts/container-registry/service-principal-assign-role/service-principal-assign-role.sh)]

Pode encontrar ambas estas scripts no GitHub, bem como as versões do PowerShell:

* [CLI do Azure][acr-scripts-cli]
* [Azure PowerShell][acr-scripts-psh]

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az_ad_sp_create_for_rbac
[az-role-assignment-create]: /cli/azure/role/assignment#az_role_assignment_create
