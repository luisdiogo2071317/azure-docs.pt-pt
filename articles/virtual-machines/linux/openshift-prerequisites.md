---
title: OpenShift em pré-requisitos do Azure | Documentos da Microsoft
description: Pré-requisitos para implementar o OpenShift no Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: ''
ms.author: haroldw
ms.openlocfilehash: 36271116d697e5ee6c6ed08d5fdc6063a511e820
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46984345"
---
# <a name="common-prerequisites-for-deploying-openshift-in-azure"></a>Pré-requisitos comuns para implementar o OpenShift no Azure

Este artigo descreve os pré-requisitos comuns para implementar o OpenShift Origin ou OpenShift Container Platform no Azure.

A instalação do OpenShift utiliza playbooks do Ansible. Ansible utiliza Secure Shell (SSH) para ligar a todos os anfitriões de cluster para concluir os passos de instalação.

Quando iniciar a ligação de SSH para hosts remotos, não é possível introduzir uma palavra-passe. Por esse motivo, a chave privada não pode ter uma palavra-passe associada a ele ou falha de implementação.

Uma vez que as máquinas virtuais (VMs) implementar através de modelos Azure Resource Manager, a mesma chave pública é utilizada para acesso a todas as VMs. Tem de inserir a chave privada correspondente na VM que executa todos os playbooks também. Para fazer isso de forma segura, use um cofre de chave do Azure para passar a chave privada para a VM.

Se for necessário para armazenamento persistente para contentores, volumes persistentes são necessários. OpenShift suporta discos rígidos virtuais (VHDs) do Azure para esta capacidade, mas tem de ser configurado primeiro Azure como o fornecedor de cloud. 

Nesse modelo, o OpenShift:

- Cria um objeto VHD numa conta de armazenamento do Azure.
- Monta o VHD para uma VM e o formato do volume.
- Monta o volume para o pod.

Para esta configuração funcione, OpenShift necessita de permissões para executar as tarefas anteriores no Azure. Conseguir isto com um principal de serviço. O principal de serviço é uma conta de segurança no Azure Active Directory que são concedidas permissões para recursos.

O principal de serviço tem de ter acesso às contas de armazenamento e as VMs que compõem o cluster. Se todos os recursos de cluster do OpenShift implementar um grupo de recursos, o principal de serviço pode ser concedido permissões para esse grupo de recursos.

Este guia descreve como criar os artefactos associados com os pré-requisitos.

> [!div class="checklist"]
> * Crie um cofre de chaves para gerir as chaves SSH para o cluster do OpenShift.
> * Crie um principal de serviço para utilização pelo fornecedor de soluções de Cloud do Azure.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure 
Inicie sessão na sua subscrição do Azure com o [início de sessão az](/cli/azure/reference-index#az_login) comando e siga na tela direções, ou clique em **experimentá-la** para utilizar o Cloud Shell.

```azurecli 
az login
```
## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos com o comando [az group create](/cli/azure/group#az_group_create). Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos. Utilize um grupo de recursos dedicado para alojar o Cofre de chaves. Este grupo é separado do grupo de recursos no qual implementar os recursos do cluster OpenShift. 

O exemplo seguinte cria um grupo de recursos chamado *keyvaultrg* no *eastus* localização:

```azurecli 
az group create --name keyvaultrg --location eastus
```

## <a name="create-a-key-vault"></a>Criar um cofre de chaves
Criar um cofre de chaves para armazenar as chaves SSH para o cluster com o [az keyvault criar](/cli/azure/keyvault#az_keyvault_create) comando. O nome do Cofre de chaves tem de ser globalmente exclusivo.

O exemplo seguinte cria um cofre de chaves com o nome *keyvault* no *keyvaultrg* grupo de recursos:

```azurecli 
az keyvault create --resource-group keyvaultrg --name keyvault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a>Criar uma chave SSH 
Uma chave SSH, é necessário para proteger o acesso ao cluster OpenShift Origin. Criar um par de chaves SSH com o `ssh-keygen` comando (no Linux ou macOS):
 
 ```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> O par de chaves de SSH não pode ter uma palavra-passe.

Para obter mais informações sobre chaves SSH no Windows, consulte [chaves de como criar SSH no Windows](/azure/virtual-machines/linux/ssh-from-windows).

## <a name="store-the-ssh-private-key-in-azure-key-vault"></a>Store a chave privada SSH no Azure Key Vault
A implementação do OpenShift utiliza a chave SSH criado para proteger o acesso ao mestre de OpenShift. Para ativar a implementação obter a chave SSH de forma segura, armazene a chave no Cofre de chaves utilizando o seguinte comando:

```azurecli
az keyvault secret set --vault-name keyvault --name keysecret --file ~/.ssh/openshift_rsa
```

## <a name="create-a-service-principal"></a>Criar um principal de serviço 
OpenShift comunica com o Azure através de um nome de utilizador e palavra-passe ou um principal de serviço. Um principal de serviço do Azure é uma identidade de segurança que pode utilizar com aplicações, serviços e ferramentas de automatização, como o OpenShift. Pode controlar e define as permissões em relação às quais operações o principal de serviço pode executar no Azure. Para melhorar a segurança para além de simplesmente fornecer um nome de utilizador e palavra-passe, este exemplo cria um serviço básico principal.

Criar um serviço principal com [az ad sp create-for-rbac](/cli/azure/ad/sp#az_ad_sp_create_for_rbac) e as credenciais que precisa de OpenShift de saída.

O exemplo seguinte cria um serviço principal e o atribui permissões de contribuinte a um grupo de recursos com o nome myResourceGroup. Se estiver a utilizar o Windows, execute ```az group show --name myResourceGroup --query id``` em separado e usar o resultado para alimentar a opção-- âmbitos.

```azurecli
az ad sp create-for-rbac --name openshiftsp \
          --role Contributor --password {Strong Password} \
          --scopes $(az group show --name myResourceGroup --query id)
```

Tome nota da propriedade appId devolvida do comando:
```json
{
  "appId": "11111111-abcd-1234-efgh-111111111111",            
  "displayName": "openshiftsp",
  "name": "http://openshiftsp",
  "password": {Strong Password},
  "tenant": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
}
```
 > [!WARNING] 
 > Certifique-se de que criar uma palavra-passe segura. Siga as instruções em [Azure AD password rules and restrictions](/azure/active-directory/active-directory-passwords-policy) (Regras e limitações de palavras-passe do Azure AD).

Para obter mais informações sobre principais de serviço, consulte [criar um Azure principal de serviço com a CLI do Azure](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest).

## <a name="next-steps"></a>Passos Seguintes

Este artigo abordou os seguintes tópicos:
> [!div class="checklist"]
> * Crie um cofre de chaves para gerir as chaves SSH para o cluster do OpenShift.
> * Crie um principal de serviço para utilização pelo fornecedor de soluções de Cloud do Azure.

Em seguida, implemente um cluster do OpenShift:

- [Implementar o OpenShift Origin](./openshift-origin.md)
- [Implementar o OpenShift Container Platform](./openshift-container-platform.md)

