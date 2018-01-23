---
title: Como montar o Blob storage do Azure como um sistema de ficheiros no Linux | Microsoft Docs
description: Montar um contentor de armazenamento de Blobs do Azure com FUSE no Linux
services: storage
documentationcenter: linux
author: seguler
manager: jahogg
ms.service: storage
ms.devlang: bash
ms.topic: article
ms.date: 01/19/2018
ms.author: seguler
ms.openlocfilehash: 299b96c783fb3606347bb448d00d44f0071da429
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="how-to-mount-blob-storage-as-a-file-system-with-blobfuse-preview"></a>Como montar o armazenamento de BLOBs como um sistema de ficheiros com blobfuse (pré-visualização)

## <a name="overview"></a>Descrição geral
[Blobfuse](https://github.com/Azure/azure-storage-fuse) é um controlador de sistema de ficheiros virtual para o Blob Storage do Azure, que lhe permite aceder os dados de BLOBs de blocos existente na sua conta de armazenamento através do sistema de ficheiros do Linux. Armazenamento de Blobs do Azure é um serviço de armazenamento de objeto e, por conseguinte, não tem um espaço de nomes hierárquico. Blobfuse fornece este espaço de nomes utilizando o esquema do diretório virtual com a utilização de reencaminhamento barra '/' como um delimitador.  

Este guia mostra como utilizar blobfuse e montar um contentor de armazenamento de BLOBs em dados de Linux e o acesso. Para saber mais sobre blobfuse, leia os detalhes na [o repositório de blobfuse](https://github.com/Azure/azure-storage-fuse).

> [!WARNING]
> Blobfuse não garante a conformidade de POSIX 100% como simplesmente traduz os pedidos para [APIs REST do Blob](https://docs.microsoft.com/en-us/rest/api/storageservices/blob-service-rest-api). Por exemplo, as operações de mudança de nome são atómicos por POSIX, mas não em blobfuse.
> Para obter uma lista completa das diferenças entre um sistema de ficheiros nativo e blobfuse, visite [o repositório de código de origem blobfuse](https://github.com/azure/azure-storage-fuse).
> 

## <a name="install-blobfuse-on-linux"></a>Instalar blobfuse no Linux
Binários Blobfuse estão disponíveis no [repositórios de software Microsoft para Linux](https://docs.microsoft.com/windows-server/administration/Linux-Package-Repository-for-Microsoft-Software). Para instalar o blobfuse, configure um destes repositórios.

### <a name="configure-the-microsoft-package-repository"></a>Configurar o repositório de pacotes da Microsoft
Configurar o [pacote Linux repositório para produtos da Microsoft](https://docs.microsoft.com/windows-server/administration/Linux-Package-Repository-for-Microsoft-Software).

Por exemplo, uma distribuição Enterprise Linux 6:
```bash
sudo rpm -Uvh https://packages.microsoft.com/config/rhel/6/packages-microsoft-prod.rpm
```

Da mesma forma, altere o url para `.../rhel/7/...` para apontar para uma distribuição Enterprise Linux 7.

Outro exemplo de um Ubuntu 14.04:
```bash
wget https://packages.microsoft.com/config/ubuntu/14.04/packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
sudo apt-get update
```

Da mesma forma, altere o url para `.../ubuntu/16.04/...` para apontar para uma distribuição Ubuntu 16.04.

### <a name="install-blobfuse"></a>Instalar blobfuse

Uma distribuição Ubuntu/Debian:
```bash
sudo apt-get install blobfuse
```

Uma distribuição Enterprise Linux:
```bash
sudo yum install blobfuse
```

## <a name="prepare-for-mounting"></a>Preparar para montagem
Blobfuse requer um caminho temporário no sistema de ficheiros para a memória intermédia e quaisquer ficheiros abertos, que ajuda a proporciona um desempenho semelhante nativo em cache. Para este caminho temporário, escolha a maioria dos disco performant ou utilizar um disco de RAM para um melhor desempenho. 

> [!NOTE]
> Blobfuse armazena todos os conteúdos do ficheiro aberto no caminho temporário. Certifique-se de que tem espaço suficiente para acomodar abrir todos os ficheiros. 
> 

### <a name="optional-use-a-ramdisk-for-the-temporary-path"></a>(Opcional) Utilizar um disco de RAM para o caminho temporário
O exemplo seguinte cria um disco de RAM de 16 GB, bem como criar um diretório para blobfuse. Escolha o tamanho com base nas suas necessidades. Este disco de RAM permite blobfuse para abrir ficheiros até 16 GB de tamanho. 
```bash
sudo mount -t tmpfs -o size=16g tmpfs /mnt/ramdisk
sudo mkdir /mnt/ramdisk/blobfusetmp
sudo chown <youruser> /mnt/ramdisk/blobfusetmp
```

### <a name="use-an-ssd-for-temporary-path"></a>Utilize um SSD para o caminho temporário
No Azure, pode utilizar os discos efémeras (SSD) disponíveis no seu VMs para fornecer uma memória intermédia baixa latência para blobfuse. No Ubuntu distribuições, este disco efémeras está montado na ' / mnt' enquanto está montado no ' / mnt/recurso /' na VM de RedHat e CentOS distribuições.

Certifique-se de que o utilizador tem acesso ao caminho temporário:
```bash
sudo mkdir /mnt/resource/blobfusetmp
sudo chown <youruser> /mnt/resource/blobfusetmp
```

### <a name="configure-your-storage-account-credentials"></a>Configurar as credenciais da conta de armazenamento
Blobfuse requer as suas credenciais para ser armazenado num ficheiro de texto no seguinte formato: 

```
accountName myaccount
accountKey myaccesskey==
containerName mycontainer
```

Depois de criar este ficheiro, certifique-se restringir o acesso para que nenhum outro utilizador pode lê-lo.
```bash
chmod 700 fuse_connection.cfg
```

### <a name="create-an-empty-directory-for-mounting"></a>Criar um diretório vazio para montagem
```bash
mkdir ~/mycontainer
```

## <a name="mount"></a>montagem

> [!NOTE]
> Para obter uma lista completa das opções de montagem, consulte [o repositório de blobfuse](https://github.com/Azure/azure-storage-fuse#mount-options).  
> 

Por ordem para blobfuse de montagem, execute o seguinte comando com o utilizador. Este comando monta o contentor especificado em ' / path/to/fuse_connection.cfg' para a localização ' / mycontainer'.

```bash
blobfuse ~/mycontainer --tmp-path=/mnt/resource/blobfusetmp  --config-file=/path/to/fuse_connection.cfg -o attr_timeout=240 -o entry_timeout=240 -o negative_timeout=120
```

Agora, deve ter acesso para os blobs de blocos através de APIs do sistema de ficheiros normais. Tenha em atenção que o diretório montado só pode ser acedido pelo utilizador montar, que protege o acesso. Se pretender permitir o acesso a todos os utilizadores, pode montar através da opção ```-o allow_other```. 

```bash
cd ~/mycontainer
mkdir test
echo "hello world" > test/blob.txt
```

## <a name="next-steps"></a>Passos Seguintes

* [Home page do Blobfuse](https://github.com/Azure/azure-storage-fuse#blobfuse)
* [Relatório blobfuse problemas](https://github.com/Azure/azure-storage-fuse/issues) 

