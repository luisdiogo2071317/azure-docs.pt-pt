---
title: Capturar uma imagem de uma VM do Linux | Documentos da Microsoft
description: Saiba como capturar uma imagem de uma baseado em Linux máquina virtual do Azure (VM) criada com o modelo de implementação clássica.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: 17d7ffee-a58e-4290-9de1-64c3cf1ddc05
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: cynthn
ms.openlocfilehash: ae87af45ffc442c0de6c7f703694a994e536cdb8
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37929217"
---
# <a name="how-to-capture-a-classic-linux-virtual-machine-as-an-image"></a>How to capture a classic Linux virtual machine as an image (Como capturar uma máquina virtual do Linux clássica como uma imagem)
> [!IMPORTANT]
> O Azure tem dois modelos de implementação diferentes para criar e trabalhar com recursos: [Resource Manager e clássica](../../../resource-manager-deployment-model.md). Este artigo explica como utilizar o modelo de implementação clássica. A Microsoft recomenda que as implementações mais novas utilizem o modelo Resource Manager. Saiba como [executar estes passos com o modelo do Resource Manager](../capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Este artigo mostra como capturar uma máquina de virtual do Azure (VM) clássica com o Linux como uma imagem para criar outras máquinas virtuais. Esta imagem inclui o disco de SO e discos de dados ligados à VM. Ele não inclui a configuração da rede, por isso terá de configurar que quando cria noutra VM a partir da imagem.

Azure armazena a imagem sob **imagens**, juntamente com quaisquer imagens que carregou. Para obter mais informações sobre imagens, consulte [sobre imagens de máquinas virtuais no Azure][About Virtual Machine Images in Azure].

## <a name="before-you-begin"></a>Antes de começar
Estes passos partem do princípio de que já criou a VM do Azure com o modelo de implementação clássica e configurado o sistema operativo, incluindo a anexar quaisquer discos de dados. Se precisar de criar uma VM, leia [como criar uma Máquina Virtual Linux][How to Create a Linux Virtual Machine].

## <a name="capture-the-virtual-machine"></a>Capturar a máquina virtual
1. [Ligar à VM](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) através de um cliente SSH à sua escolha.
2. Na janela do SSH, escreva o seguinte comando. A saída do `waagent` pode divergir ligeiramente dependendo da versão desse utilitário:

    ```bash
    sudo waagent -deprovision+user
    ```

    O comando anterior tenta limpar o sistema e torná-la adequada para reprovisionamento. Esta operação executa as seguintes tarefas:

   * Remove as chaves de anfitrião SSH (se Provisioning. regeneratesshhostkeypair 'y' no ficheiro de configuração)
   * Limpa a configuração de nomes em /etc/resolv.conf
   * Remove o `root` palavra-passe de utilizador de nomedeanfitrião sombra (se Provisioning. deleterootpassword 'y' no ficheiro de configuração)
   * Remove as concessões de cliente DHCP em cache
   * Repõe o nome do anfitrião para localhost.localdomain
   * Elimina o último de conta de utilizador (obtida a partir do /var/lib/waagent) aprovisionado **e dados associados**.

     > [!NOTE]
     > Desaprovisionamento elimina ficheiros e dados para "generalize" a imagem. Apenas execute este comando numa VM que pretende capturar como um novo modelo de imagem. Não garante que a imagem será removida de todas as informações confidenciais ou se é adequada para redistribuição a terceiros.

3. Tipo **y** para continuar. Pode adicionar o `-force` parâmetro para evitar esta etapa de confirmação.
4. Tipo **saída** para fechar o cliente SSH.

   > [!NOTE]
   > Os restantes passos partem do princípio de que já [instalado a CLI do Azure](../../../cli-install-nodejs.md) no seu computador cliente. Todos os passos seguintes também podem ser feitos [portal do Azure](http://portal.azure.com).

5. A partir do computador cliente, abra o CLI do Azure e inicie sessão na sua subscrição do Azure. Para obter detalhes, leia [ligar a uma subscrição do Azure a partir da CLI do Azure](/cli/azure/authenticate-azure-cli).

   > [!NOTE]
   > No portal do Azure, inicie sessão no portal.

6. Certifique-se de que está no modo de gestão do serviço:

    ```azurecli
    azure config mode asm
    ```

7. Encerre a VM que já está a ser desaprovisionada. O exemplo seguinte desliga a VM com o nome `myVM`:

    ```azurecli
    azure vm shutdown myVM
    ```
   Se for necessário, pode ver uma lista de todas as VMs criadas na sua subscrição com `azure vm list`

   > [!NOTE]
   > Se estiver a utilizar o portal do Azure, selecione a VM e clique em **parar** para encerrar a VM.

8. Quando a VM é parada, capture a imagem. O exemplo seguinte captura de VM com o nome `myVM` e cria uma imagem generalizada com o nome `myNewVM`:

    ```azurecli
    azure vm capture -t myVM myNewVM
    ```

    O `-t` subcomando elimina a máquina virtual original.

    > [!NOTE]
    > No portal do Azure, pode capturar uma imagem, selecionando **imagem** no hub menu. Tem de fornecer as seguintes informações para a imagem: nome, grupo de recursos, localização, tipo de sistema operativo e caminho do blob de armazenamento.

9. A nova imagem está agora disponível na lista de imagens que podem ser utilizados para configurar qualquer nova VM. Pode vê-la com o comando:

   ```azurecli
   azure vm image list
   ```

   Na [portal do Azure](http://portal.azure.com), a nova imagem aparece no **imagens de VM (clássico)** que pertence ao **computação** serviços. Pode acessar **imagens de VM (clássico)** ao clicar em **todos os serviços** na parte superior do Azure, serviço lista e, em seguida, verificar **computação** serviços.   

   ![Captura de imagens com êxito](./media/capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a>Passos Seguintes
A imagem está pronta para ser utilizado para criar VMs. Pode utilizar o comando da CLI do Azure `azure vm create` e forneça o nome da imagem que criou. Para obter mais informações, consulte [utilizar a CLI do Azure com o modelo de implementação clássica](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

Em alternativa, utilize o [portal do Azure](http://portal.azure.com) para criar uma VM personalizada com o **imagem** método e selecionar a imagem que criou. Para obter mais informações, consulte [como criar uma VM personalizada][How to Create a Custom Virtual Machine].

**Consulte também:** [Guia do usuário do agente Linux do Azure](../../extensions/agent-linux.md)

[About Virtual Machine Images in Azure]:../../virtual-machines-linux-classic-about-images.md
[How to Create a Custom Virtual Machine]:create-custom-classic.md
[How to Attach a Data Disk to a Virtual Machine]:attach-disk-classic.md
[How to Create a Linux Virtual Machine]:create-custom-classic.md
