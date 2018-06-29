---
title: Integrar do Azure Active Directory com o serviço de Kubernetes do Azure
description: Como criar clusters do serviço de Kubernetes Azure ativado o Azure Active Directory.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 6/17/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 272d98613e13c1bb76c75befd6bd5e0115c32610
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37097245"
---
# <a name="integrate-azure-active-directory-with-aks---preview"></a>Integrar o AKS - pré-visualização do Azure Active Directory

Serviço de Kubernetes do Azure (AKS) pode ser configurado para utilizar o Azure Active Directory para autenticação de utilizador. Nesta configuração, pode iniciar sessão para um cluster do serviço de Kubernetes do Azure utilizando o seu token de autenticação do Azure Active Directory. Além disso, os administradores de cluster são capazes de configurar o controlo de acesso baseado em funções Kubernetes com base na associação de grupo utilizadores identidade ou diretório.

Este documento fornece detalhes sobre todos os pré-requisitos necessários para AKS e o Azure AD a criar, implementar um cluster do Azure AD ativado e criar uma função RBAC simple no AKS cluster.

> [!IMPORTANT]
> Integração do RBAC Kubernetes. o serviço (AKS) e o Azure AD do Azure está atualmente na **pré-visualização**. As pré-visualizações são tornadas disponíveis para si na condição de concordar com os [termos suplementares de utilização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Alguns aspetos desta funcionalidade podem alterar-se após a disponibilidade geral (GA).
>

## <a name="authentication-details"></a>Detalhes de autenticação

Autenticação do Azure AD é fornecida para o Azure Kubernetes clusters com OpenID Connect. OpenID Connect é uma camada de identidade desenvolvida com o protocolo OAuth 2.0. Mais informações sobre o OpenID Connect podem ser encontradas no [abrir ID connect documentação][open-id-connect].

De dentro do Kubernetes cluster, autenticação de Token do Webhook é utilizada para verificar os tokens de autenticação. Autenticação de token do Webhook está configurada e gerida como parte do AKS cluster. Obter mais informações sobre o Webhook autenticação com token pode ser encontrada no [documentação de autenticação de webhook][kubernetes-webhook].

> [!NOTE]
> Quando configurar o Azure AD para autenticação de AKS, são configurados dois aplicação do Azure AD. Esta operação deve ser feita por um administrador de inquilino do Azure.

## <a name="create-server-application"></a>Criar aplicação de servidor

A primeira aplicação do Azure AD é utilizada para obter uma associação do grupo de utilizadores do Azure AD.

1. Selecione **Azure Active Directory** > **Registos das aplicações** > **Novo registo de aplicação**.

  Dê um nome, selecione da aplicação **aplicação Web / API** para o tipo de aplicação e introduza um valor URI formatado para **URL de início de sessão**. Selecione **criar** quando terminar.

  ![Criar registo do Azure AD](media/aad-integration/app-registration.png)

2. Selecione **manifesto** e editar o `groupMembershipClaims` valor `"All"`.

  Guarde as atualizações, depois de concluído.

  ![Associação ao grupo de atualização para todos](media/aad-integration/edit-manifest.png)

3. Novamente na aplicação do Azure AD, selecione **definições** > **chaves**.

  Adicionar uma descrição da chave, selecione um prazo de expiração e selecione **guardar**. Tome nota do valor de chave. Quando implementar um Azure AD ativado AKS cluster, este valor é referido como o `Server application secret`.

  ![Obter a chave privada de aplicação](media/aad-integration/application-key.png)

4. Regressar à aplicação do Azure AD, selecione **definições** > **as permissões necessárias** > **adicionar**  >   **Selecione uma API** > **Microsoft Graph** > **selecione**.

  Em **PERMISSÕES de aplicação** coloque uma marca junto a **ler os dados de diretório**.

  ![Definir as permissões de gráfico de aplicação](media/aad-integration/read-directory.png)

5. Em **PERMISSÕES DELEGADAS**, coloque uma marca junto a **iniciar sessão e ler o perfil de utilizador** e **ler os dados de diretório**. Guarde as atualizações uma vez concluídas.

  ![Definir as permissões de gráfico de aplicação](media/aad-integration/delegated-permissions.png)

6. Selecione **feito** e **conceder permissões** para concluir este passo. Este passo irá falhar se a conta atual não é um administrador inquilino.

  ![Definir as permissões de gráfico de aplicação](media/aad-integration/grant-permissions.png)

7. Voltar à aplicação e tome nota do **ID da aplicação**. Quando implementar um cluster do Azure AKS preparados no AD, este valor é referido como o `Server application ID`.

  ![Obter ID da aplicação](media/aad-integration/application-id.png)

## <a name="create-client-application"></a>Criar aplicação de cliente

A aplicação do Azure AD segundo é utilizada quando iniciam sessão com a CLI de Kubernetes (kubectl.)

1. Selecione **Azure Active Directory** > **Registos das aplicações** > **Novo registo de aplicação**.

  Dê um nome, selecione da aplicação **nativo** para o tipo de aplicação e introduza um valor URI formatado para **URI de redirecionamento**. Selecione **criar** quando terminar.

  ![Criar registo do AAD](media/aad-integration/app-registration-client.png)

2. A partir da aplicação do Azure AD, selecione **definições** > **as permissões necessárias** > **adicionar** > **selecionar um API** e procure o nome da aplicação de servidor criada no último passo deste documento.

  ![Configurar permissões de aplicação](media/aad-integration/select-api.png)

3. Coloque uma marca de verificação junto de aplicações e clique em **selecione**.

  ![Selecione o ponto final da aplicação AKS AAD servidor](media/aad-integration/select-server-app.png)

4. Selecione **feito** e **conceder permissões** para concluir este passo.

  ![Conceder permissões](media/aad-integration/grant-permissions-client.png)

5. Novamente na aplicação AD, tome nota do **ID da aplicação**. Quando implementar um cluster do Azure AKS preparados no AD, este valor é referido como o `Client application ID`.

  ![Obter o ID de aplicação](media/aad-integration/application-id-client.png)

## <a name="get-tenant-id"></a>Obter o ID de inquilino

Por fim, obter o ID do inquilino do Azure. Este valor é também utilizado quando implementar o cluster AKS.

A partir do portal do Azure, selecione **do Azure Active Directory** > **propriedades** e tome nota do **ID de diretório**. Quando implementar um cluster do Azure AKS preparados no AD, este valor é referido como o `Tenant ID`.

![Obter o ID de inquilino do Azure](media/aad-integration/tenant-id.png)

## <a name="deploy-cluster"></a>Implementar o Cluster

Utilize o [criar grupo az] [ az-group-create] comando para criar um grupo de recursos para o cluster AKS.

```azurecli
az group create --name myAKSCluster --location eastus
```

Implementar o cluster utilizando o [az aks criar] [ az-aks-create] comando. Substitua os valores no comando de exemplo abaixo os valores recolhidos durante a criação de aplicações do Azure AD.

```azurecli
az aks create --resource-group myAKSCluster --name myAKSCluster --generate-ssh-keys --enable-rbac \
  --aad-server-app-id 7ee598bb-0000-0000-0000-83692e2d717e \
  --aad-server-app-secret wHYomLe2i1mHR2B3/d4sFrooHwADZccKwfoQwK2QHg= \
  --aad-client-app-id 7ee598bb-0000-0000-0000-83692e2d717e \
  --aad-tenant-id 72f988bf-0000-0000-0000-2d7cd011db47
```

## <a name="create-rbac-binding"></a>Criar o enlace do RBAC

Antes de pode ser utilizada uma conta do Azure Active Directory com o cluster AKS, um enlace de função de cluster ou enlace de função tem de ser criado.

Em primeiro lugar, utilize o [az aks get-credenciais] [ az-aks-get-credentials] comando com o `--admin` argumento para iniciar sessão no cluster com acesso de administrador.

```azurecli
az aks get-credentials --resource-group myAKSCluster --name myAKSCluster --admin
```

Em seguida, utilize o manifesto seguinte para criar um ClusterRoleBinding para uma conta do Azure AD. Atualize o nome de utilizador com um inquilino do Azure AD. Neste exemplo permite a conta de acesso completo a todos os espaços de nomes do cluster.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: contoso-cluster-admins
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: "user@contoso.com"
```

Também pode ser criado um enlace de função para todos os membros de um grupo do Azure AD. O manifesto seguinte fornece todos os membros de `kubernetes-admin` grupo acesso de administrador para o cluster.

 ```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: contoso-cluster-admins
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- apiGroup: rbac.authorization.k8s.io
   kind: Group
   name: "kubernetes-admin"
```

Para obter mais informações sobre como proteger um cluster de Kubernetes com RBAC, consulte [utilizando o RBAC autorização][rbac-authorization].

## <a name="access-cluster-with-azure-ad"></a>Cluster de acesso com o Azure AD

Em seguida, extraia o contexto para o utilizador não admin a utilizar o [az aks get-credenciais] [ az-aks-get-credentials] comando.

```azurecli
az aks get-credentials --resource-group myAKSCluster --name myAKSCluster
```

Depois de executar qualquer comando kubectl, será solicitado para autenticar com o Azure. Siga o ecrã instruções.

```console
$ kubectl get nodes

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code BUJHWDGNL to authenticate.

NAME                       STATUS    ROLES     AGE       VERSION
aks-nodepool1-42032720-0   Ready     agent     1h        v1.9.6
aks-nodepool1-42032720-1   Ready     agent     1h        v1.9.6
aks-nodepool1-42032720-2   Ready     agent     1h        v1.9.6
```

Depois de concluído, o token de autenticação é colocado em cache. Apenas são reprompted para iniciar sessão quando o token expirou ou o ficheiro de configuração de Kubernetes recriado.

## <a name="next-steps"></a>Próximos Passos

Saiba mais sobre como proteger clusters de Kubernetes com RBAC com o [utilizando o RBAC autorização] [ rbac-authorization] documentação.

<!-- LINKS - external -->
[kubernetes-webhook]:https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[rbac-authorization]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az_aks_get_credentials
[az-group-create]: /cli/azure/group#az_group_create
[open-id-connect]: ../active-directory/develop/active-directory-protocols-openid-connect-code.md
