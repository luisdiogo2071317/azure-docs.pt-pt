---
title: Encriptar discos numa VM com Linux no Azure | Microsoft Docs
description: Como encriptar discos virtuais numa VM com Linux para segurança melhorada utilizando o 2.0 CLI do Azure
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 2a23b6fa-6941-4998-9804-8efe93b647b3
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/31/2018
ms.author: iainfou
ms.openlocfilehash: 343408366c2970d10a952634ac671721caed74d4
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36936875"
---
# <a name="how-to-encrypt-a-linux-virtual-machine-in-azure"></a>Como encriptar uma máquina virtual do Linux no Azure
Para segurança avançada máquina virtual (VM) e conformidade, podem ser encriptadas discos virtuais e a própria VM. As VMs estão encriptadas com as chaves criptográficas que são protegidas um cofre de chaves do Azure. Pode controla estas chaves criptográficas e pode auditar a sua utilização. Este artigo fornece detalhes sobre como encriptar discos virtuais numa VM com Linux utilizando o 2.0 CLI do Azure. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este artigo requer que está a executar a CLI do Azure versão 2.0.30 ou posterior. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli).

## <a name="overview-of-disk-encryption"></a>Descrição geral de encriptação de disco
Discos virtuais em VMs do Linux são encriptados em rest utilizando [dm-crypt](https://wikipedia.org/wiki/Dm-crypt). Não há sem encargos de encriptação de discos virtuais no Azure. As chaves criptográficas são armazenadas no Cofre de chaves do Azure utilizando a proteção de software, ou pode importar ou gerar as suas chaves nos módulos de segurança de Hardware (HSMs) com certificação FIPS 140-2 normas de nível 2. Manter o controlo destas chaves criptográficas e pode auditar a sua utilização. Estas chaves criptográficas são utilizados para encriptar e desencriptar discos virtuais anexados à VM. Um principal de serviço do Azure Active Directory fornece um mecanismo seguro para emitir estas chaves criptográficas como VMs estão ligados à corrente ou desligar.

O processo de encriptação de uma VM é o seguinte:

1. Crie uma chave criptográfica num cofre de chaves do Azure.
2. Configure a chave criptográfica para ser utilizada para encriptação de discos.
3. Para ler a chave criptográfica no Cofre de chaves do Azure, crie um serviço do Azure Active Directory principal com as permissões adequadas.
4. Emita o comando para encriptar os discos virtuais, especificando o Azure Active Directory serviço principal e adequado chave criptográfica a ser utilizado.
5. O principal de serviço do Azure Active Directory pedidos a chave criptográfica necessária a partir do Cofre de chaves do Azure.
6. Os discos virtuais estão encriptados com a chave criptográfica fornecida.

## <a name="encryption-process"></a>Processo de encriptação
Encriptação de disco baseia-se nos seguintes componentes adicionais:

* **O Cofre de chaves do Azure** - utilizado para salvaguardar as chaves criptográficas e segredos utilizados para o processo de encriptação/desencriptação de disco.
  * Se existir, pode utilizar um cofre de chaves do Azure existente. Não dispõe de dedique um cofre de chaves para encriptação de discos.
  * Separar os limites administrativos e visibilidade chave, pode criar um cofre de chaves dedicado.
* **Azure Active Directory** -processa segura troca de chaves criptográficas necessárias e a autenticação para ações de pedido.
  * Normalmente, pode utilizar uma instância existente do Azure Active Directory para o alojamento da sua aplicação.
  * O principal de serviço fornece um mecanismo seguro para pedir e ser emitido as chaves criptográficas adequadas. Não estiver a desenvolver uma aplicação real, que se integra com o Azure Active Directory.

## <a name="requirements-and-limitations"></a>Requisitos e limitações
Cenários suportados e os requisitos para encriptação de disco de:

* O seguinte servidor Linux SKUs - Ubuntu, CentOS, SUSE e SUSE Linux Enterprise Server (SLES) e Red Hat Enterprise Linux.
* Todos os recursos (por exemplo, o Cofre de chaves, a conta de armazenamento e a VM) tem de ser na mesma região do Azure e subscrição.
* Um padrão, D, DS, G, GS, etc., série VMs.
* A atualizar as chaves criptográficas numa VM com Linux já encriptados.

Encriptação de disco não é atualmente suportada nos seguintes cenários:

* Escalão básico VMs.
* VMs criadas utilizando o modelo de implementação clássica.
* A desativação da encriptação de disco de SO em VMs do Linux.
* Utilização de imagens de Linux personalizadas.

Para obter mais informações sobre cenários suportados e limitações, consulte [para IaaS VMs do Azure Disk Encryption](../../security/azure-security-disk-encryption.md)


## <a name="create-an-azure-key-vault-and-keys"></a>Criar um cofre de chaves do Azure e as chaves
Nos exemplos a seguir, substitua os nomes dos parâmetros de exemplo com os seus próprios valores. Os nomes dos parâmetros de exemplo incluem *myResourceGroup*, *myKey*, e *myVM*.

O primeiro passo é criar um cofre de chaves do Azure para armazenar as chaves criptográficas. O Cofre de chaves do Azure pode armazenar as chaves, os segredos ou palavras-passe que permitem-lhe implementá-los em segurança nos seus serviços e aplicações. Para a encriptação de disco virtual, pode utilizar o Cofre de chaves para armazenar uma chave criptográfica utilizada para encriptar ou desencriptar os discos virtuais.

Ativar o fornecedor do Cofre de chaves do Azure na sua subscrição do Azure com [o registo de fornecedor az](/cli/azure/provider#az-provider-register) e criar um grupo de recursos com [criar grupo az](/cli/azure/group#az-group-create). O exemplo seguinte cria um nome de grupo de recursos *myResourceGroup* no `eastus` localização:

```azurecli-interactive
az provider register -n Microsoft.KeyVault
az group create --name myResourceGroup --location eastus
```

O Cofre de chaves do Azure que contém as chaves criptográficas e os recursos de computação associados, como o armazenamento e a própria VM tem de residir na mesma região. Criar um cofre de chaves do Azure com [az keyvault criar](/cli/azure/keyvault#az-keyvault-create) e ativar o Cofre de chaves para utilização com a encriptação de disco. Especifique um nome exclusivo do Cofre de chaves para *keyvault_name* da seguinte forma:

```azurecli-interactive
keyvault_name=myuniquekeyvaultname
az keyvault create \
    --name $keyvault_name \
    --resource-group myResourceGroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

Pode armazenar as chaves criptográficas utilizando software ou proteção de modelo de segurança de Hardware (HSM). Utilizar um HSM necessita de um cofre de chaves de premium. Não há um custos adicionais para criar um premium Cofre de chaves em vez de padrão Cofre de chaves que armazena as chaves protegidas por software. Para criar um cofre de chaves de premium, no passo anterior adicionar `--sku Premium` ao comando. O exemplo seguinte utiliza as chaves protegidas de software, uma vez que criou um cofre de chaves padrão.

Para ambos os modelos de proteção, a plataforma do Azure tem de ser concedido acesso ao pedir as chaves criptográficas quando arranca a VM para desencriptar os discos virtuais. Criar uma chave criptográfica no seu Cofre de chaves com [criar chave de keyvault az](/cli/azure/keyvault/key#az-keyvault-key-create). O exemplo seguinte cria uma chave denominada *myKey*:

```azurecli-interactive
az keyvault key create --vault-name $keyvault_name --name myKey --protection software
```


## <a name="create-an-azure-active-directory-service-principal"></a>Criar um principal de serviço do Azure Active Directory
Quando os discos virtuais são encriptados ou desencriptados, especifique uma conta para processar a autenticação e a troca de chaves criptográficas do Cofre de chaves. Esta conta, um principal de serviço do Azure Active Directory, permite que a plataforma do Azure pedir as chaves criptográficas adequadas em nome da VM. Uma instância do Azure Active Directory predefinida está disponível na sua subscrição, apesar de muitas organizações têm dedicado diretórios do Azure Active Directory.

Criar um principal de serviço com o Azure Active Directory com [az ad sp criar-para-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac). O exemplo seguinte lê os valores para o principal de serviço e a palavra-passe para utilização nos comandos posteriores:

```azurecli-interactive
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)
```

A palavra-passe apenas é apresentada quando criar o serviço principal. Se assim o desejar, ver e registe a palavra-passe (`echo $sp_password`). Pode listar os principais de serviço com [lista do az ad sp](/cli/azure/ad/sp#az-ad-sp-list) e ver informações adicionais sobre um serviço específico principal com [Mostrar do az ad sp](/cli/azure/ad/sp#az-ad-sp-show).

Para encriptar ou desencriptar discos virtuais com sucesso, as permissões a chave criptográfica armazenada no Cofre de chaves tem de ser definidas para permitir o principal de serviço do Azure Active Directory para ler as chaves. Definir as permissões no seu Cofre de chaves com [az keyvault set-policy](/cli/azure/keyvault#az-keyvault-set-policy). No exemplo seguinte, o ID de principal de serviço fornecido do comando anterior:

```azurecli-interactive
az keyvault set-policy --name $keyvault_name --spn $sp_id \
  --key-permissions wrapKey \
  --secret-permissions set
```


## <a name="create-a-virtual-machine"></a>Criar uma máquina virtual
Criar uma VM com [az vm criar](/cli/azure/vm#az-vm-create) e anexar um disco de dados de 5 Gb. Apenas determinadas imagens do marketplace suportam encriptação de disco. O exemplo seguinte cria uma VM chamada *myVM* utilizando um *Ubuntu 16.04 LTS* imagem:

```azurecli-interactive
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

SSH para a sua VM ao utilizar o *publicIpAddress* apresentado na saída do comando anterior. Criar uma partição e o sistema de ficheiros, em seguida, montar o disco de dados. Para obter mais informações, consulte [ligar a uma VM com Linux para montar o disco novo](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk). Feche a sessão SSH.


## <a name="encrypt-the-virtual-machine"></a>Encriptar a máquina virtual
Para encriptar os discos virtuais, pode reunir todos os componentes anteriores:

1. Especifique o principal de serviço do Azure Active Directory e a palavra-passe.
2. Especifique o Cofre de chaves para armazenar os metadados para os discos encriptados.
3. Especifique as chaves criptográficas a utilizar para a encriptação real e a desencriptação.
4. Especifique se pretende encriptar o disco do SO, os discos de dados ou todos.

Encriptar a VM com [ativar a encriptação de vm az](/cli/azure/vm/encryption#az-vm-encryption-enable). O exemplo seguinte utiliza o *$sp_id* e *$sp_password* variáveis a partir de precedente [az ad sp criar-para-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac) comando:

```azurecli-interactive
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```

Demora algum tempo para conclusão do processo de encriptação de disco. Monitorizar o estado do processo com [mostrar de encriptação de vm az](/cli/azure/vm/encryption#az-vm-encryption-show):

```azurecli-interactive
az vm encryption show --resource-group myResourceGroup --name myVM
```

O resultado será semelhante ao seguinte exemplo truncada:

```json
[
  "dataDisk": "EncryptionInProgress",
  "osDisk": "EncryptionInProgress"
]
```

Aguarde até que o estado do sistema operativo do disco relatórios **VMRestartPending**, em seguida, reinicie a VM com [reinício de vm az](/cli/azure/vm#az-vm-restart):

```azurecli-interactive
az vm restart --resource-group myResourceGroup --name myVM
```

O processo de encriptação de disco está finalizado durante o processo de arranque, por isso, aguarde alguns minutos antes de a verificar o estado de encriptação com [mostrar de encriptação de vm az](/cli/azure/vm/encryption#az-vm-encryption-show):

```azurecli-interactive
az vm encryption show --resource-group myResourceGroup --name myVM
```

O estado agora deve reportar o disco do SO e o disco de dados como **encriptado**.


## <a name="add-additional-data-disks"></a>Adicionar discos de dados adicionais
Assim que encriptou os discos de dados, pode adicionar mais tarde discos virtuais adicionais para a VM e também encriptá-las. Por exemplo, permite adicionar um disco virtual segundo à sua VM, da seguinte forma:

```azurecli-interactive
az vm disk attach \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --disk myDataDisk \
    --new \
    --size-gb 5
```

Volte a executar o comando para encriptar os discos virtuais da seguinte forma:

```azurecli-interactive
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```


## <a name="next-steps"></a>Passos Seguintes
* Para obter mais informações sobre como gerir o Cofre de chaves do Azure, incluindo a eliminação de chaves criptográficas e os cofres, consulte [gerir o Cofre de chaves ao utilizar a CLI](../../key-vault/key-vault-manage-with-cli2.md).
* Para obter mais informações sobre a encriptação de disco, tais como preparar uma VM personalizada encriptada para carregar para o Azure, consulte [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).
