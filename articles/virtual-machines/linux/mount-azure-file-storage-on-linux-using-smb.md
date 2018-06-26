---
title: Armazenamento de ficheiros do Azure de montagem em VMs do Linux utilizando o SMB | Microsoft Docs
description: Como montar o File storage do Azure em VMs do Linux com o SMB 2.0 de CLI do Azure
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: iainfoulds
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/13/2017
ms.author: iainfou
ms.openlocfilehash: 2255c8fd7cd873ae9b6511e1a7b9e2ac13f9fb66
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36936773"
---
# <a name="mount-azure-file-storage-on-linux-vms-using-smb"></a>Armazenamento de ficheiros do Azure de montagem em VMs do Linux utilizando o SMB

Este artigo mostra-lhe como utilizar o serviço de armazenamento de ficheiros do Azure numa VM com Linux com uma montagem SMB 2.0 de CLI do Azure. File storage do Azure oferece partilhas de ficheiros na nuvem utilizando o protocolo SMB padrão. Os requisitos são:

- [uma conta do Azure](https://azure.microsoft.com/pricing/free-trial/)
- [Ficheiros de chaves públicas e privadas SSH](mac-create-ssh-keys.md)

## <a name="quick-commands"></a>Comandos Rápidos

* Um grupo de recursos
* Uma rede virtual do Azure
* Um grupo de segurança de rede com um SSH entrada
* Uma sub-rede
* Uma conta de armazenamento do Azure
* Chaves de conta de armazenamento do Azure
* Uma partilha de armazenamento de ficheiros do Azure
* Uma VM com Linux

Substitua qualquer exemplos as suas próprias definições.

### <a name="create-a-directory-for-the-local-mount"></a>Criar um diretório de montagem local

```bash
mkdir -p /mnt/mymountpoint
```

### <a name="mount-the-file-storage-smb-share-to-the-mount-point"></a>Montar o partilha do SMB para o ponto de montagem de armazenamento de ficheiros

```bash
sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mnt/mymountpoint -o vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

### <a name="persist-the-mount-after-a-reboot"></a>Manter a montagem após um reinício
Para tal, adicione a seguinte linha para o `/etc/fstab`:

```bash
//myaccountname.file.core.windows.net/mysharename /mnt/mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a>Instruções detalhadas

O File storage oferece partilhas de ficheiros na nuvem que utilizam o protocolo SMB padrão. Com a versão mais recente do armazenamento de ficheiros, também é possível montar uma partilha de ficheiros de qualquer sistema operativo que suporta o SMB 3.0. Quando utiliza uma montagem SMB no Linux, obtenha cópias de segurança fácil para um robusta, permanente arquivo local de armazenamento que é suportado por um SLA.

A mover os ficheiros de uma VM para uma montagem SMB que está alojada no armazenamento de ficheiros é uma excelente forma de registos de depuração. Isto acontece porque a mesma partilha SMB pode ser montada localmente na estação de trabalho Mac, Linux ou do Windows. SMB não é a melhor solução para transmissão em fluxo Linux ou os registos de aplicação em tempo real, porque o protocolo SMB não está incorporado para processar as tarefas de registo pesada. Uma ferramenta de camada de registo dedicado, unificada como Fluentd seria uma melhor opção que SMB para a recolha de Linux e aplicação de registo de saída.

Nestas instruções detalhadas, iremos criar os pré-requisitos necessários para primeiro criar a partilha de ficheiros de armazenamento e montá-la através de SMB numa VM com Linux.

1. Criar um grupo de recursos com [criar grupo az](/cli/azure/group#az_group_create) para conter a partilha de ficheiros.

    Para criar um grupo de recursos denominado `myResourceGroup` na localização "EUA Oeste", utilize o seguinte exemplo:

    ```azurecli
    az group create --name myResourceGroup --location westus
    ```

2. Criar uma conta de armazenamento do Azure com [criar conta de armazenamento az](/cli/azure/storage/account#az_storage_account_create) para armazenar os ficheiros concretos.

    Para criar uma conta de armazenamento denominada mystorageaccount utilizando o armazenamento de Standard_LRS SKU, utilize o seguinte exemplo:

    ```azurecli
    az storage account create --resource-group myResourceGroup \
        --name mystorageaccount \
        --location westus \
        --sku Standard_LRS
    ```

3. Mostra as chaves de conta de armazenamento.

    Quando cria uma conta de armazenamento, as chaves de conta são criadas pares para que pode ser rotação sem qualquer interrupção do serviço. Quando mudar para a segunda chave na par, crie um novo par de chaves. Chaves de conta de armazenamento novas sempre são criadas pares, assegurando que tem sempre, pelo menos, uma chave de conta de armazenamento não utilizados pronta para mudar para.

    Ver as chaves de conta de armazenamento com o [lista de chaves de conta de armazenamento az](/cli/azure/storage/account/keys#az_storage_account_keys_list). A conta de armazenamento de chaves para o nomeado `mystorageaccount` estão listados no exemplo seguinte:

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount
    ```

    Para extrair uma chave única, utilize o `--query` sinalizador. O exemplo seguinte extrai a primeira chave (`[0]`):

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount \
        --query '[0].{Key:value}' --output tsv
    ```

4. Crie partilha do File storage.

    A partilha do File storage contém a partilha SMB com [criar partilha de armazenamento az](/cli/azure/storage/share#az_storage_share_create). A quota é sempre expresso em gigabytes (GB). A passagem de uma das chaves de precedente `az storage account keys list` comando. Crie uma partilha denominada mystorageshare com uma quota de 10 GB ao utilizar o exemplo seguinte:

    ```azurecli
    az storage share create --name mystorageshare \
        --quota 10 \
        --account-name mystorageaccount \
        --account-key nPOgPR<--snip-->4Q==
    ```

5. Crie um diretório do ponto de montagem.

    Crie um diretório local no sistema de ficheiros do Linux para montar a partilha SMB. Nada escritas ou ler a partir do diretório de montagem local é reencaminhado para a partilha do SMB que está alojada no armazenamento de ficheiros. Para criar um diretório local no /mnt/mymountdirectory, utilize o seguinte exemplo:

    ```bash
    sudo mkdir -p /mnt/mymountpoint
    ```

6. Monte a partilha SMB para o diretório local.

    Forneça o seu próprio nome de utilizador de conta de armazenamento e a chave da conta de armazenamento para as credenciais de montagem da seguinte forma:

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountpoint -o vers=3.0,username=mystorageaccount,password=mystorageaccountkey,dir_mode=0777,file_mode=0777
    ```

7. Manter a montagem SMB através de reinícios.

    Quando reiniciar a VM com Linux, a partilha SMB montada é desmontada durante o encerramento. Para remontar na partilha SMB no arranque, adicione uma linha para o /etc/fstab Linux. Linux utiliza o ficheiro de fstab para listar os sistemas de ficheiros que necessita de montagem durante o processo de arranque. Adicionar a partilha SMB garante que a partilha do File storage é um sistema de ficheiro permanentemente instalado para a VM com Linux. É possível adicionar o armazenamento de ficheiros de partilha do SMB para uma nova VM ao utilizar init de nuvem.

    ```bash
    //myaccountname.file.core.windows.net/mystorageshare /mnt/mymountpoint cifs vers=3.0,username=mystorageaccount,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## <a name="next-steps"></a>Passos Seguintes

- [Nuvem init a utilizar para personalizar uma VM com Linux durante a criação](using-cloud-init.md)
- [Adicionar um disco a uma VM do Linux](add-disk.md)
- [Encriptar discos uma VM com Linux utilizando a CLI do Azure](encrypt-disks.md)
