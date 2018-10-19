---
title: Transferir dados com o Azure Data Box Edge | Microsoft Docs
description: Saiba como adicionar e ligar a partilhas no dispositivo Data Box Edge.
services: databox-edge-gateway
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox-edge-gateway
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/08/2018
ms.author: alkohli
ms.custom: ''
Customer intent: As an IT admin, I need to understand how to add and connect to shares on Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: b5c63a255547bae03acbec771593eac97d474003
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/06/2018
ms.locfileid: "48833103"
---
# <a name="tutorial-transfer-data-with-azure-data-box-edge-preview"></a>Tutorial: transferir dados com o Azure Data Box Edge (Pré-visualização)

Este tutorial descreve como adicionar e ligar a partilhas no Data Box Edge. Assim que as partilhas forem adicionadas, o dispositivo Data Box Edge pode transferir dados para o Azure.

Este procedimento pode demorar cerca de 10 minutos a concluir. 

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Adicionar uma partilha
> * Ligar a uma partilha

> [!IMPORTANT]
> O Data Box Edge está em pré-visualização. Reveja os [Termos de serviço do Azure para pré-visualização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) antes de encomendar e implementar esta solução. 
 
## <a name="prerequisites"></a>Pré-requisitos

Antes de adicionar partilhas ao Data Box Edge, certifique-se de que:

* Instalou o dispositivo físico conforme detalhado em [Instalar um Data Box Edge](data-box-edge-deploy-install.md). 

    O dispositivo físico está ativado, conforme descrito em [Ligar e ativar o Azure Data Box Edge](data-box-edge-deploy-connect-setup-activate.md). O dispositivo está pronto para criar partilhas e transferir dados.


## <a name="add-a-share"></a>Adicionar uma partilha

Efetue os seguintes passos no [portal do Azure](https://portal.azure.com/) para criar uma partilha.

1. Regresse ao portal do Azure. Aceda a **Todos os recursos** e procure o seu recurso Data Box Edge.
    
2. Na lista filtrada de recursos, selecione o recurso Data Box Edge e, em seguida, aceda a **Descrição Geral**. Clique em **+ Adicionar partilha** na barra de comando do dispositivo.
   
   ![Adicionar uma partilha](./media/data-box-edge-deploy-add-shares/click-add-share.png)

3. Em **Adicionar Partilha**, especifique as definições de partilha. Indique um nome exclusivo para a partilha. 

   Os nomes das partilhas só podem ter números, letras minúsculas e hífenes. O nome da partilha tem de ter entre 3 e 63 carateres e começar com uma letra ou um número. Cada hífen tem de ser precedido e seguido de um caráter que não seja um hífen.
    
    1. Selecione um **Tipo** para a partilha. O tipo pode ser SMB ou NFS, sendo que SMB é a predefinição. SMB é o padrão para clientes Windows, e NFS é utilizado para clientes Linux. 

    2. Dependendo se escolher a partilha SMB ou NFS, as opções apresentadas são ligeiramente diferentes. 

    3. Tem de fornecer uma conta de armazenamento onde a partilha irá residir. É criado um contentor na conta de armazenamento com o nome da partilha, se o contentor ainda não existir. Se o contentor já existir, é utilizado o contentor existente. 
    
    4. Escolha o **Serviço de armazenamento** do blob de blocos, blob de páginas ou ficheiros. O tipo de serviço escolhido depende do formato no qual pretende que os dados residam no Azure. Por exemplo, neste exemplo, queremos que os dados sejam armazenados como blocos de blobs no Azure, por conseguinte, selecionamos Blob de Blocos. Se escolher o Blob de Páginas, certifique-se de que os dados têm um alinhamento de 512 bytes. Por exemplo, um VHDX tem sempre um alinhamento de 512 bytes.
   
    5. Este passo depende de estar a criar uma partilha SMB ou NFS. 
     
        - **Se criar uma partilha SMB** - no campo de utilizador local Todos os privilégios, escolha **Criar novo** ou **Utilizar existente**. Se criar um novo utilizador local, forneça o **nome de utilizador**, a **palavra-passe** e, em seguida **confirme a palavra-passe**. Esta ação atribui as permissões ao utilizador local. Depois de ter atribuído as permissões aqui, pode utilizar o Explorador de Ficheiros para modificar estas permissões.

            Se marcar **permitir apenas operações de leitura** para esta partilha de dados, terá a opção de especificar utilizadores só de leitura.

            ![Adicionar uma partilha SMB](./media/data-box-edge-deploy-add-shares/add-share-smb-1.png)
   
        - **Se criar uma partilha NFS** - tem de fornecer os endereços IP dos clientes permitidos que podem aceder à partilha.

            ![Adicionar uma partilha NFS](./media/data-box-edge-deploy-add-shares/add-share-nfs-1.png)
   
4. Clique em **Criar** para criar a partilha. 
    
    Será notificado de que a criação da partilha está em curso. Depois de criar a partilha com as definições especificadas, o painel **Partilhas** é atualizado para refletir a nova partilha. 
    
    ![Lista atualizada de partilhas](./media/data-box-edge-deploy-add-shares/updated-list-of-shares.png) 

## <a name="connect-to-the-share"></a>Ligar à partilha

Pode agora ligar a uma ou mais partilhas que criou no passo anterior. Dependendo se tiver uma partilha SMB ou NFS, os passos podem ser diferentes. 

### <a name="connect-to-an-smb-share"></a>Ligar a uma partilha SMB

Efetue estes passos no cliente do Windows Server ligado ao seu Data Box Edge para ligar às partilhas.


1. Abra uma janela de comando. Na linha de comandos, escreva:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

    Introduza a palavra-passe da partilha quando lhe for pedido. A saída de exemplo deste comando é apresentada aqui.

    ```powershell
    Microsoft Windows [Version 10.0.16299.192) 
    (c) 2017 microsoft Corporation. All rights reserved . 
    
    C: \Users\DataBoxEdgeUser>net use \\10.10.10.60\newtestuser /u:Tota11yNewUser 
    Enter the password for 'TotallyNewUser' to connect to '10.10.10.60': 
    The command completed successfully. 
    
    C: \Users\DataBoxEdgeUser>
    ```   


2. Prima Windows + R. Na janela **Executar**, especifique o `\\<device IP address>`. Clique em **OK**. Esta ação abre o Explorador de Ficheiros. Agora deve conseguir ver as partilhas que criou como pastas. Selecione e faça duplo clique numa partilha (pasta) para ver o conteúdo.
 
    ![Ligar a uma partilha SMB](./media/data-box-edge-deploy-add-shares/connect-to-share2.png)

    Os dados são escritos nestas partilhas à medida que são gerados, e o dispositivo emite os dados para a cloud.

### <a name="connect-to-an-nfs-share"></a>Ligar a uma partilha NFS

Efetue estes passos no seu cliente Linux ligado ao Data Box Edge.

1. Certifique-se de que o cliente tem o cliente NFSv4 instalado. Para instalar o cliente NFS, utilize o seguinte comando:

   `sudo apt-get install nfs-common`

    Para obter mais informações, aceda a [Instalar cliente NFSv4](https://help.ubuntu.com/community/SettingUpNFSHowTo#NFSv4_client).

2. Depois de o cliente NFS estar instalado, utilize o comando seguinte para montar a partilha NFS que criou no seu dispositivo Data Box Edge:

   `sudo mount <device IP>:/<NFS share on device> /home/username/<Folder on local Linux computer>`

    Antes de montar as partilhas, certifique-se de que os diretórios que irão funcionar como pontos de montagem já foram criados. Estes diretórios não devem conter quaisquer ficheiros ou subpastas.

    O exemplo seguinte mostra como ligar através de NFS a uma partilha no dispositivo Data Box Edge. O IP do dispositivo é `10.10.10.60`. A partilha `mylinuxshare2` é montada no ubuntuVM. O ponto de montagem da partilha é `/home/databoxubuntuhost/edge`.

    `sudo mount -t nfs 10.10.10.60:/mylinuxshare2 /home/databoxubuntuhost/Edge`

> [!NOTE] 
> Os seguintes avisos aplicam-se à versão de pré-visualização:
> - Depois de um ficheiro ser criado nas partilhas, mudar o nome do ficheiro não é suportado. 
> - A eliminação de um ficheiro de uma partilha não elimina a entrada na conta de armazenamento.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a conhecer tópicos do Data Box Edge, como:

> [!div class="checklist"]
> * Adicionar uma partilha
> * Ligar a uma partilha


Avance para o tutorial seguinte para saber como transformar os dados com o Data Box Edge.

> [!div class="nextstepaction"]
> [Transformar dados com o Data Box Edge](./data-box-edge-deploy-configure-compute.md)


