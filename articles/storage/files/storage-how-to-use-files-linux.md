---
title: Utilizar os ficheiros do Azure com o Linux | Microsoft Docs
description: "Saiba como montar uma partilha de ficheiros do Azure através de SMB no Linux."
services: storage
documentationcenter: na
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 6edc37ce-698f-4d50-8fc1-591ad456175d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/20/2017
ms.author: renash
ms.openlocfilehash: cca0d315a815faca5db07099b8e8e451ef55fad5
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="use-azure-files-with-linux"></a>Utilizar os ficheiros do Azure com o Linux
[Ficheiros do Azure](storage-files-introduction.md) é o sistema de ficheiros na cloud fácil de utilizar da Microsoft. Partilhas de ficheiros do Azure podem ser montadas nas distribuições do Linux utilizando o [cliente de kernel CIFS](https://wiki.samba.org/index.php/LinuxCIFS). Este artigo mostra duas formas de montar uma partilha de ficheiros do Azure: a pedido com o `mount` comando e de arranque através da criação de uma entrada no `/etc/fstab`.

> [!NOTE]  
> Para montar uma partilha de ficheiros do Azure fora a região do Azure que está alojado no, tal como no local ou numa região diferente do Azure, o sistema operativo tem de suportar a funcionalidade de encriptação do SMB 3.0.

## <a name="prerequisites-for-mounting-an-azure-file-share-with-linux-and-the-cifs-utils-package"></a>Pré-requisitos para montar uma partilha de ficheiros do Azure com o Linux e o pacote de cifs utils
* **Escolha uma distribuição de Linux que pode ter o pacote de cifs utils instalado.**  
    As distribuições de Linux seguintes estão disponíveis para utilização na galeria do Azure:

    * Ubuntu Server 14.04 +
    * RHEL 7 +
    * CentOS 7 +
    * Debian 8 +
    * openSUSE 13.2 +
    * SUSE Linux Enterprise Server 12

* <a id="install-cifs-utils"></a>**O pacote de cifs utils está instalado.**  
    O pacote de cifs utils pode ser instalado utilizando o Gestor de pacotes de distribuição de Linux da sua preferência. 

    No **Ubuntu** e **baseado em Debian** distribuições, utilize o `apt-get` Gestor de pacotes:

    ```
    sudo apt-get update
    sudo apt-get install cifs-utils
    ```

    No **RHEL** e **CentOS**, utilize o `yum` Gestor de pacotes:

    ```
    sudo yum install samba-client samba-common cifs-utils
    ```

    No **openSUSE**, utilize o `zypper` Gestor de pacotes:

    ```
    sudo zypper install samba*
    ```

    No outras distribuições, utilize o Gestor de pacotes apropriada ou [compilar a partir da origem](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download).

* <a id="smb-client-reqs"></a>**Compreenda os requisitos do cliente SMB.**  
    Podem ser montados os ficheiros do Azure ou através de SMB 2.1 e o SMB 3.0. Para ligações futuras a partir de clientes no local ou noutras regiões do Azure, ficheiros do Azure irão rejeitar SMB 2.1 (ou SMB 3.0 sem encriptação). Se *secure transferência necessária* está ativada para uma conta de armazenamento, ficheiros do Azure irá permitir apenas ligações através de SMB 3.0 com encriptação.
    
    Suporte de encriptação SMB 3.0 foi introduzido na versão de kernel 4.11 de Linux e foi backported a versões mais antigas do kernel para as distribuições do Linux populares. No momento da publicação deste documento, as distribuições seguintes na galeria do Azure suportam esta funcionalidade:

    - Ubuntu Server 16.04 +
    - openSUSE 42.3 +
    - SUSE Linux Enterprise Server 12 SP3+
    
    Se a distribuição de Linux não estiver listada aqui, pode verificar para ver a versão de kernel do Linux com o seguinte comando:

    ```
    uname -r
    ```

* **Opte por utilizar as permissões do diretório/ficheiro da partilha montada**: nos exemplos abaixo, a permissão `0777` é utilizado para dar a leitura, escrita e de executar permissões a todos os utilizadores. Pode ser substituído com outros [chmod permissões](https://en.wikipedia.org/wiki/Chmod) conforme pretendido. 

* **Nome da conta de armazenamento**: para montar uma partilha de ficheiros do Azure, terá do nome da conta de armazenamento.

* **Chave de conta de armazenamento**: para montar uma partilha de ficheiros do Azure, terá da chave de armazenamento () principais ou secundários. Atualmente, não são suportadas chaves SAS para a montagem.

* **Certifique-se de que a porta 445 está aberta**: SMB comunica através da porta TCP 445 - verificar ver se a firewall não está a bloquear TCP portas 445 do computador cliente.

## <a name="mount-the-azure-file-share-on-demand-with-mount"></a>Montar a ficheiro do Azure partilha a pedido com`mount`
1. **[Instalar o pacote de cifs utils para a distribuição de Linux](#install-cifs-utils)**.

2. **Crie uma pasta para o ponto de montagem**: uma pasta para um ponto de montagem pode ser criada em qualquer local no sistema de ficheiros, mas é Convenção comuns para criar esta sob o `/mnt` pasta. Por exemplo:

    ```
    mkdir /mnt/MyAzureFileShare
    ```

3. **Utilize o comando de montagem para montar a partilha de ficheiros do Azure**: não se esqueça de substituir `<storage-account-name>`, `<share-name>`, `<smb-version>`, `<storage-account-key>`, e `<mount-point>` com as informações adequadas para o seu ambiente. Se a distribuição de Linux suportar SMB 3.0 com encriptação (consulte [requisitos do cliente SMB compreender](#smb-client-reqs) para obter mais informações), utilize `3.0` para `<smb-version>`. Para as distribuições do Linux que não suportem SMB 3.0 com encriptação, utilize `2.1` para `<smb-version>`. Tenha em atenção que uma partilha de ficheiros do Azure só pode ser montada fora de uma região do Azure (incluindo no local ou numa região diferente do Azure) com SMB 3.0. 

    ```
    sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> <mount-point> -o vers=<smb-version>,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
    ```

> [!Note]  
> Quando tiver terminado a utilizar a partilha de ficheiros do Azure, pode utilizar `sudo umount <mount-point>` desmontar a partilha.

## <a name="create-a-persistent-mount-point-for-the-azure-file-share-with-etcfstab"></a>Criar um ponto de montagem persistente para a partilha de ficheiros do Azure com o`/etc/fstab`
1. **[Instalar o pacote de cifs utils para a distribuição de Linux](#install-cifs-utils)**.

2. **Crie uma pasta para o ponto de montagem**: uma pasta para um ponto de montagem pode ser criada em qualquer local no sistema de ficheiros, mas é Convenção comuns para criar esta sob o `/mnt` pasta. Sempre que criar isto, tenha em atenção o caminho absoluto da pasta. Por exemplo, o comando seguinte cria uma nova pasta em `/mnt` (o caminho é um caminho absoluto).

    ```
    sudo mkdir /mnt/MyAzureFileShare
    ```

3. **Utilize o seguinte comando para acrescentar a linha seguinte ao `/etc/fstab`** : não se esqueça de substituir `<storage-account-name>`, `<share-name>`, `<smb-version>`, `<storage-account-key>`, e `<mount-point>` com as informações adequadas para o ambiente. Se a distribuição de Linux suportar SMB 3.0 com encriptação (consulte [requisitos do cliente SMB compreender](#smb-client-reqs) para obter mais informações), utilize `3.0` para `<smb-version>`. Para as distribuições do Linux que não suportem SMB 3.0 com encriptação, utilize `2.1` para `<smb-version>`. Tenha em atenção que uma partilha de ficheiros do Azure só pode ser montada fora de uma região do Azure (incluindo no local ou numa região diferente do Azure) com SMB 3.0. 

    ```
    sudo bash -c 'echo "//<storage-account-name>.file.core.windows.net/<share-name> <mount-point> cifs nofail,vers=<smb-version>,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'
    ```

> [!Note]  
> Pode utilizar `sudo mount -a` para montar a partilha de ficheiros do Azure após a edição `/etc/fstab` em vez de ser reiniciado.

## <a name="feedback"></a>Comentários
Utilizadores do Linux, Queremos ouvi da!

Os ficheiros do Azure para o grupo de utilizadores do Linux fornece um fórum para partilhar os seus comentários à medida que avalia e que adotar o armazenamento de ficheiros no Linux. E-mail [Azure ficheiros Linux utilizadores](mailto:azurefileslinuxusers@microsoft.com) para aderir a grupo dos utilizadores.

## <a name="next-steps"></a>Passos Seguintes
Veja estas ligações para obter mais informações sobre os Ficheiros do Azure.
* [Introdução aos ficheiros do Azure](storage-files-introduction.md)
* [Planear uma implementação de ficheiros do Azure](storage-files-planning.md)
* [FAQ](../storage-files-faq.md)
* [Resolução de problemas](storage-troubleshoot-linux-file-connection-problems.md)