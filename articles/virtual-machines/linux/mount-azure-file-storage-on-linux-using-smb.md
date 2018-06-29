---
title: Armazenamento de ficheiros do Azure de montagem em VMs do Linux utilizando o SMB | Microsoft Docs
description: Como montar o File storage do Azure em VMs do Linux utilizando o SMB com a CLI do Azure
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/28/2018
ms.author: cynthn
ms.openlocfilehash: 2019324030b2e4c469d0b9ba937fb40a9d0675f1
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37099716"
---
# <a name="mount-azure-file-storage-on-linux-vms-using-smb"></a>Armazenamento de ficheiros do Azure de montagem em VMs do Linux utilizando o SMB


Este artigo mostra-lhe como utilizar o serviço de armazenamento de ficheiros do Azure numa VM com Linux com uma montagem SMB com a CLI do Azure. File storage do Azure oferece partilhas de ficheiros na nuvem utilizando o protocolo SMB padrão. 

O File storage oferece partilhas de ficheiros na nuvem que utilizam o protocolo SMB padrão. Podem montar uma partilha de ficheiros a partir de qualquer sistema operativo que suporta o SMB 3.0. Quando utiliza uma montagem SMB no Linux, obtenha cópias de segurança fácil para um robusta, permanente arquivo local de armazenamento que é suportado por um SLA.

A mover os ficheiros de uma VM para uma montagem SMB que está alojada no armazenamento de ficheiros é uma excelente forma de registos de depuração. A mesma partilha SMB pode ser montada localmente na estação de trabalho Mac, Linux ou Windows. SMB não é a melhor solução para transmissão em fluxo Linux ou os registos de aplicação em tempo real, porque o protocolo SMB não está incorporado para processar as tarefas de registo pesada. Uma ferramenta de camada de registo dedicado, unificada como Fluentd seria uma melhor opção que SMB para a recolha de Linux e aplicação de registo de saída.

Este guia requer que estiver a executar a CLI do Azure versão 2.0.4 ou posterior. Execute **az --version** para descobrir a versão. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure](/cli/azure/install-azure-cli). 


## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos denominado *myResourceGroup* no *EUA Leste* localização.

```bash
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-storage-account"></a>Criar uma conta de armazenamento

Criar uma nova conta de armazenamento, no grupo de recursos que criou, utilizando [criar conta de armazenamento az](/cli/azure/storage/account#create). Este exemplo cria uma conta de armazenamento com o nome *mySTORAGEACCT<random number>*  e coloca o nome dessa conta de armazenamento de na variável **STORAGEACCT**. Os nomes das contas de armazenamento tem de ser exclusivos, utilizando `$RANDOM` acrescenta um número ao fim para o tornar único.

```bash
STORAGEACCT=$(az storage account create \
    --resource-group "myResourceGroup" \
    --name "mystorageacct$RANDOM" \
    --location eastus \
    --sku Standard_LRS \
    --query "name" | tr -d '"')
```

## <a name="get-the-storage-key"></a>Obter a chave de armazenamento

Quando cria uma conta de armazenamento, as chaves de conta são criadas pares para que pode ser rotação sem qualquer interrupção do serviço. Quando mudar para a segunda chave na par, crie um novo par de chaves. Chaves de conta de armazenamento novas são sempre criadas pares, pelo que terá sempre, pelo menos, uma chave de conta de armazenamento não utilizados pronta para mudar para.

Ver as chaves de conta de armazenamento utilizando o [lista de chaves de conta de armazenamento az](/cli/azure/storage/account/keys#list). Neste exemplo armazena o valor da chave 1 no **STORAGEKEY** variável.

```bash
STORAGEKEY=$(az storage account keys list \
    --resource-group "myResourceGroup" \
    --account-name $STORAGEACCT \
    --query "[0].value" | tr -d '"')
```

## <a name="create-a-file-share"></a>Criar uma partilha de ficheiros

Criar o ficheiro armazenamento partilha utilizando [criar partilha de armazenamento az](/cli/azure/storage/share#create). 

Nomes de partilha têm de ser todas as letras minúsculas, números e hífenes único, mas não podem começar com um hífen. Para obter detalhes completos sobre a nomenclatura de partilhas de ficheiros e ficheiros, veja [Naming and Referencing Shares, Directories, Files, and Metadata (Nomenclatura e Referência de Partilhas, Diretórios, Ficheiros e Metadados)](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata).

Este exemplo cria uma partilha denominada *myshare* com uma quota de 10 GiB. 

```bash
az storage share create --name myshare \
    --quota 10 \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY
```

## <a name="create-a-mount-point"></a>Criar um ponto de montagem

Para montar a partilha de ficheiros do Azure no seu computador Linux, tem de certificar-se de que tem o **cifs utils** pacote instalado. Para obter instruções de instalação, consulte [instalar o pacote de cifs utils para a distribuição de Linux](../../storage/files/storage-how-to-use-files-linux.md#install-cifs-utils).

Ficheiros do Azure utiliza o protocolo SMB, que comunica através da porta TCP 445.  Se tiver problemas montar a partilha de ficheiros do Azure, certifique-se que a firewall não está a bloquear a porta TCP 445.


```bash
mkdir -p /mnt/MyAzureFileShare
```

## <a name="mount-the-share"></a>Montar a partilha

Monte a partilha de ficheiros do Azure para o diretório local. 

```bash
sudo mount -t cifs //$STORAGEACCT.file.core.windows.net/myshare /mnt/MyAzureFileShare -o vers=3.0,username=$STORAGEACCT,password=$STORAGEKEY,dir_mode=0777,file_mode=0777,serverino
```



## <a name="persist-the-mount"></a>Manter a montagem

Quando reiniciar a VM com Linux, a partilha SMB montada é desmontada durante o encerramento. Para remontar na partilha SMB no arranque, adicione uma linha para o /etc/fstab Linux. Linux utiliza o ficheiro de fstab para listar os sistemas de ficheiros que necessita de montagem durante o processo de arranque. Adicionar a partilha SMB garante que a partilha do File storage é um sistema de ficheiro permanentemente instalado para a VM com Linux. É possível adicionar o armazenamento de ficheiros de partilha do SMB para uma nova VM ao utilizar init de nuvem.

```bash
//myaccountname.file.core.windows.net/mystorageshare /mnt/mymountpoint cifs vers=3.0,username=mystorageaccount,password=myStorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```
Para maior segurança em ambientes de produção, deve armazenar as suas credenciais fora fstab.

## <a name="next-steps"></a>Passos Seguintes

- [Nuvem init a utilizar para personalizar uma VM com Linux durante a criação](using-cloud-init.md)
- [Adicionar um disco a uma VM do Linux](add-disk.md)
- [Encriptar discos uma VM com Linux utilizando a CLI do Azure](encrypt-disks.md)

