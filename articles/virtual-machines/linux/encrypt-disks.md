---
title: Encriptar discos numa VM do Linux no Azure | Documentos da Microsoft
description: Como encriptar discos virtuais numa VM do Linux para uma maior segurança com a CLI 2.0 do Azure
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
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
ms.author: cynthn
ms.openlocfilehash: 75ec087536d6f833a9a2106b1fdf4ed1fd73ef8e
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37932416"
---
# <a name="how-to-encrypt-a-linux-virtual-machine-in-azure"></a>Como encriptar uma máquina virtual do Linux no Azure
Para segurança aprimorada de máquina virtual (VM) e de conformidade, discos virtuais e a própria VM podem ser encriptados. As VMs são encriptadas com chaves criptográficas que são protegidas num Azure Key Vault. Pode controlar estas chaves criptográficas e pode auditar o seu uso. Este artigo detalha como encriptar discos virtuais numa VM do Linux com a CLI 2.0 do Azure. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este artigo requer a execução da versão 2.0.30 da CLI do Azure ou posterior. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli).

## <a name="overview-of-disk-encryption"></a>Descrição geral da encriptação de disco
Discos virtuais em VMs do Linux são encriptados através de rest [dm-crypt](https://wikipedia.org/wiki/Dm-crypt). Não existe nenhum custo associado para encriptar discos virtuais no Azure. As chaves criptográficas são armazenadas no Azure Key Vault com a proteção de software, ou pode importar ou gerar as suas chaves em módulos de segurança de Hardware (HSMs) com certificação FIPS 140-2 de padrões no nível 2. Manter o controlo destas chaves criptográficas e pode auditar o seu uso. Estas chaves criptográficas são utilizados para encriptar e desencriptar os discos virtuais anexados a sua VM. Um principal de serviço do Azure Active Directory fornece um mecanismo seguro para a emissão estas chaves criptográficas, como as VMs têm a tecnologia e desativar.

O processo para encriptar uma VM é o seguinte:

1. Crie uma chave criptográfica num Azure Key Vault.
2. Configure a chave criptográfica para ser utilizada para encriptar discos.
3. Para ler a chave criptográfica no Azure Key Vault, crie um Azure Active Directory principal de serviço com as permissões adequadas.
4. Emita o comando para encriptar os discos virtuais, especificando o Azure Active Directory service principal e apropriado chave criptográfica para ser utilizado.
5. O principal de serviço do Azure Active Directory solicita a chave criptográfica necessária no Azure Key Vault.
6. Os discos virtuais são encriptados com a chave criptográfica fornecida.

## <a name="encryption-process"></a>Processo de criptografia
Encriptação de disco baseia-se dos seguintes componentes adicionais:

* **O Azure Key Vault** - utilizado para salvaguardar chaves criptográficas e segredos utilizados para o processo de encriptação/desencriptação do disco.
  * Se existir, pode utilizar um Azure Key Vault existente. Não é preciso dedicar um Key Vault para encriptação de discos.
  * Para separar os limites administrativos e visibilidade de chave, pode criar um cofre de chaves dedicado.
* **O Azure Active Directory** -lida com a troca segura das chaves de criptografia necessárias e autenticação para ações pedidas.
  * Normalmente, pode utilizar uma instância existente do Azure Active Directory para que hospeda o aplicativo.
  * O principal de serviço fornece um mecanismo seguro para pedir e emitido as chaves criptográficas apropriadas. Não estiver a desenvolver um aplicativo real que se integra com o Azure Active Directory.

## <a name="requirements-and-limitations"></a>Requisitos e limitações
Cenários suportados e os requisitos para encriptação de disco:

* O seguinte servidor Linux SKUs - Ubuntu, CentOS, SUSE e SUSE Linux Enterprise Server (SLES) e Red Hat Enterprise Linux.
* Todos os recursos (por exemplo, o Key Vault, a conta de armazenamento e a VM) tem de ser na mesma região do Azure e subscrição.
* Um padrão, D, DS, G, GS, etc., as VMs da série.
* A atualizar as chaves criptográficas numa VM do Linux já encriptados.

Encriptação de disco não é atualmente suportada nos seguintes cenários:

* VMs de escalão básico.
* VMs criadas com o modelo de implementação clássica.
* A desativação da encriptação de disco de SO em VMs do Linux.
* Utilização de imagens do Linux personalizadas.

Para obter mais informações sobre cenários suportados e limitações, consulte [Azure Disk Encryption para IaaS VMs](../../security/azure-security-disk-encryption.md)


## <a name="create-an-azure-key-vault-and-keys"></a>Criar um Azure Key Vault e chaves
Nos exemplos a seguir, substitua os nomes de parâmetros de exemplo pelos seus próprios valores. Os nomes de parâmetros de exemplo incluem *myResourceGroup*, *myKey*, e *myVM*.

A primeira etapa é criar um Azure Key Vault para armazenar as chaves criptográficas. O Azure Key Vault pode armazenar chaves, segredos ou palavras-passe que permitem-lhe implementá-los em segurança nas suas aplicações e serviços. Para a encriptação de disco virtual, utilize o Key Vault para armazenar uma chave criptográfica utilizada para encriptar ou desencriptar os discos virtuais.

Ativar o fornecedor do Azure Key Vault com a sua subscrição do Azure com [Registre-se fornecedor de az](/cli/azure/provider#az-provider-register) e crie um grupo de recursos com [criar grupo az](/cli/azure/group#az-group-create). O exemplo seguinte cria um nome de grupo de recursos *myResourceGroup* no `eastus` localização:

```azurecli-interactive
az provider register -n Microsoft.KeyVault
az group create --name myResourceGroup --location eastus
```

O Cofre de chaves do Azure que contém as chaves criptográficas e os recursos de computação associado como o armazenamento e a própria VM têm de residir na mesma região. Criar um Azure Key Vault com [az keyvault criar](/cli/azure/keyvault#az-keyvault-create) e ativar o Key Vault para utilização com a encriptação de disco. Especifique um nome exclusivo do Key Vault para *keyvault_name* da seguinte forma:

```azurecli-interactive
keyvault_name=myuniquekeyvaultname
az keyvault create \
    --name $keyvault_name \
    --resource-group myResourceGroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

Pode armazenar as chaves criptográficas com o software ou de proteção de modelo de segurança de Hardware (HSM). Utilizar um HSM requer um cofre de chaves de premium. Existe um custo adicional para a criação de uma premium Key Vault, em vez de padrão Key Vault que armazena as chaves protegidas por software. Para criar um cofre de chaves de premium, no passo anterior adicione `--sku Premium` ao comando. O exemplo seguinte utiliza as chaves protegidas por software, uma vez que criou um Key Vault padrão.

Para ambos os modelos de proteção, a plataforma do Azure tem de ter acesso para solicitar as chaves criptográficas quando a VM arranca para desencriptar os discos virtuais. Criar uma chave criptográfica no seu Cofre de chaves com [criar chave de Cofre de chaves de az](/cli/azure/keyvault/key#az-keyvault-key-create). O exemplo seguinte cria uma chave denominada *myKey*:

```azurecli-interactive
az keyvault key create --vault-name $keyvault_name --name myKey --protection software
```


## <a name="create-an-azure-active-directory-service-principal"></a>Criar um principal de serviço do Azure Active Directory
Quando os discos virtuais são encriptados ou desencriptados, especificar uma conta para lidar com a autenticação e a troca de chaves criptográficas do Key Vault. Esta conta, um principal de serviço do Azure Active Directory, permite que a plataforma do Azure pedir as chaves criptográficas apropriadas em nome da VM. Uma instância do Azure Active Directory padrão está disponível na sua subscrição, embora muitas organizações têm dedicado diretórios do Azure Active Directory.

Criar um principal de serviço com o Azure Active Directory com [az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac). O exemplo seguinte lê os valores para o principal de serviço e a palavra-passe para ser utilizado nos comandos posteriores:

```azurecli-interactive
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)
```

A palavra-passe só é apresentada quando criar o serviço principal. Se assim o desejar, ver e registar a palavra-passe (`echo $sp_password`). Pode listar os principais de serviço com [lista do az ad sp](/cli/azure/ad/sp#az-ad-sp-list) e ver informações adicionais sobre um serviço específico principal com [show do az ad sp](/cli/azure/ad/sp#az-ad-sp-show).

Com êxito criptografar ou descriptografar discos virtuais, as permissões na chave de criptografia armazenadas no Key Vault tem de ser definidas para permitir o principal de serviço do Azure Active Directory para ler as chaves. Definir as permissões no Cofre de chaves com [az keyvault conjunto-policy](/cli/azure/keyvault#az-keyvault-set-policy). No exemplo a seguir, o ID de principal de serviço é fornecido no comando anterior:

```azurecli-interactive
az keyvault set-policy --name $keyvault_name --spn $sp_id \
  --key-permissions wrapKey \
  --secret-permissions set
```


## <a name="create-a-virtual-machine"></a>Criar uma máquina virtual
Crie uma VM com [az vm criar](/cli/azure/vm#az-vm-create) e anexar um disco de dados de 5 Gb. Apenas determinadas imagens do marketplace suportam a encriptação de disco. O exemplo seguinte cria uma VM com o nome *myVM* usando um *Ubuntu 16.04 LTS* imagem:

```azurecli-interactive
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

SSH para a VM com o *publicIpAddress* apresentados no resultado do comando anterior. Criar uma partição e o sistema de ficheiros, em seguida, Monte o disco de dados. Para obter mais informações, consulte [ligar a uma VM do Linux para montar o disco novo](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk). Feche a sessão SSH.


## <a name="encrypt-the-virtual-machine"></a>Encriptar a máquina virtual
Para encriptar os discos virtuais, reunir todos os componentes anteriores:

1. Especifica o principal de serviço do Azure Active Directory e a palavra-passe.
2. Especifique o Key Vault para armazenar os metadados para os discos encriptados.
3. Especifica as chaves criptográficas para utilizar para a encriptação real e a desencriptação.
4. Especifique se pretende encriptar o disco do SO, os discos de dados ou todos.

Encriptar VM com [ativar a encriptação de vm de az](/cli/azure/vm/encryption#az-vm-encryption-enable). O exemplo seguinte utiliza a *$sp_id* e *$sp_password* variáveis a partir de anterior [az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac) comando:

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

Demora algum tempo para conclusão do processo de encriptação de disco. Monitorizar o estado do processo com [show do az vm encryption](/cli/azure/vm/encryption#az-vm-encryption-show):

```azurecli-interactive
az vm encryption show --resource-group myResourceGroup --name myVM
```

O resultado é semelhante ao seguinte exemplo de como truncado:

```json
[
  "dataDisk": "EncryptionInProgress",
  "osDisk": "EncryptionInProgress"
]
```

Aguarde até que o estado para o sistema operativo do disco relatórios **VMRestartPending**, em seguida, reinicie a VM com [reinício da vm az](/cli/azure/vm#az-vm-restart):

```azurecli-interactive
az vm restart --resource-group myResourceGroup --name myVM
```

O processo de encriptação de disco é finalizado durante o processo de inicialização, por isso, aguarde alguns minutos antes de a verificar o estado de encriptação com [show do az vm encryption](/cli/azure/vm/encryption#az-vm-encryption-show):

```azurecli-interactive
az vm encryption show --resource-group myResourceGroup --name myVM
```

O estado agora deve reportar o disco do SO e o disco de dados como **Encrypted**.


## <a name="add-additional-data-disks"></a>Adicionar discos de dados adicionais
Depois de ter a criptografar seus discos de dados, pode adicionar mais tarde discos virtuais adicionais para a VM e também criptografá-los. Por exemplo, permite adicionar um segundo disco virtual à sua VM da seguinte forma:

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
* Para obter mais informações sobre como gerir o Azure Key Vault, incluindo a eliminar as chaves criptográficas e os cofres, consulte [gerir Key Vault com a CLI](../../key-vault/key-vault-manage-with-cli2.md).
* Para obter mais informações sobre a encriptação de disco, como preparar uma VM encriptada personalizada para carregar para o Azure, consulte [do Azure Disk Encryption](../../security/azure-security-disk-encryption.md).
