---
title: Utilizar chaves SSH com o Windows para VMs com Linux | Microsoft Docs
description: Saiba como gerar e utilizar chaves SSH num computador Windows para ligar a uma máquina virtual do Linux no Azure.
services: virtual-machines-linux
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: 2cacda3b-7949-4036-bd5d-837e8b09a9c8
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: danlep
ms.openlocfilehash: d0762f80267fa927681344a3e0de78b0800c8306
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2018
---
# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a>Como utilizar chaves SSH com o Windows no Azure

Este artigo apresenta formas para gerar e utilizar chaves secure shell (SSH) num computador Windows para criar e ligar a uma máquina virtual (VM) do Linux no Azure. Para utilizar chaves SSH de um cliente Linux ou macOS, consulte o [rápida](mac-create-ssh-keys.md) ou [detalhadas](create-ssh-keys-detailed.md) orientações.

[!INCLUDE [virtual-machines-common-ssh-overview](../../../includes/virtual-machines-common-ssh-overview.md)]

[!INCLUDE [virtual-machines-common-ssh-support](../../../includes/virtual-machines-common-ssh-support.md)]

## <a name="windows-packages-and-ssh-clients"></a>Pacotes do Windows e clientes SSH
Ligar a e gerir VMs com Linux no Azure com um *cliente SSH*. Computadores que executam o Linux ou macOS normalmente tem um conjunto de comandos SSH para gerar e gerir chaves SSH e efetuar ligações SSH. 

Computadores Windows nem sempre dispor de comandos SSH comparáveis instalados. Versões do Windows 10 que incluem o [subsistema Windows para Linux](https://docs.microsoft.com/windows/wsl/about) permitem-lhe executar e aceder utilitários, tais como um cliente SSH nativamente dentro de uma shell de deteção. 

Se pretender utilizar algo que não sejam Bash para Windows, comuns SSH do Windows que pode instalar localmente forem incluídos clientes nos seguintes pacotes:

* [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
* [Git para Windows](https://git-for-windows.github.io/)
* [MobaXterm](http://mobaxterm.mobatek.net/)
* [Cygwin](https://cygwin.com/)

Outra opção consiste em utilizar os utilitários SSH disponíveis no Bash no [Shell de nuvem do Azure](../../cloud-shell/overview.md). 

* Aceder à Shell de nuvem no seu browser em [ https://shell.azure.com ](https://shell.azure.com) ou no [portal do Azure](https://portal.azure.com). 
* Aceder a Shell de nuvem como um terminal a partir de Visual Studio Code instalando o [extensão de conta do Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

## <a name="create-an-ssh-key-pair"></a>Criar um par de chaves SSH
Esta secção mostra duas opções para criar um par de chaves SSH no Windows.

### <a name="create-ssh-keys-with-ssh-keygen"></a>Criar chaves SSH com o ssh-keygen

Se pode executar uma shell de comandos como Bash para Windows ou bash (ou Bash na Shell de nuvem do Azure), crie um par de chaves de SSH utilizando o `ssh-keygen` comando. Escreva o seguinte comando e responder os pedidos. Se um par de chaves SSH existe na localização atual, esses ficheiros são substituídos. 

```bash
ssh-keygen -t rsa -b 2048
```

Para obter mais em segundo plano e informações, consulte o [rápida](mac-create-ssh-keys.md) ou [detalhadas](create-ssh-keys-detailed.md) passos para criar as chaves com `ssh-keygen`.

### <a name="create-ssh-keys-with-puttygen"></a>Criar chaves SSH com PuTTYgen

Se preferir utilizar uma ferramenta baseada na GUI para criar chaves SSH, pode utilizar o gerador de chaves do PuTTYgen, incluído com o [pacote de transferência PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html). 

Para criar um par de chaves SSH RSA com PuTTYgen:

1. Inicie PuTTYgen.

2. Clique em **gerar**. Por predefinição PuTTYgen gera uma chave SSH-2 RSA de 2048 bits.

4. O rato sobre a área em branco para gerar alguns randomness para a chave.

5. Depois da chave pública é gerada, opcionalmente, introduza e confirme uma frase de acesso. Será solicitado para a frase de acesso quando se autenticar para a VM com a chave SSH. Sem uma frase de acesso, se alguém obtém a chave privada, pode iniciar sessão para qualquer serviço que utiliza essa chave ou a VM. Recomendamos que crie uma frase de acesso. Contudo, caso se esqueça da frase de acesso, não tem como a recuperar.

6. A chave pública é apresentada na parte superior da janela. Copiar e colar esta chave pública do formato de uma linha no portal do Azure ou um modelo Azure Resource Manager, ao criar uma VM com Linux. Também pode clicar em **Guardar chave pública** para guardar uma cópia para o seu computador:

    ![Guarde o ficheiro de chave pública PuTTY](./media/ssh-from-windows/save-public-key.png)

7. Opcionalmente, para guardar a chave privada no formato de chave privada (ficheiro. ppk) PuTTy, clique em **Guardar chave privada**. É necessário o ficheiro. ppk de que pretende utilizar o PuTTY posterior para efetuar uma ligação SSH para a VM.

    ![Guarde o ficheiro de chave privada PuTTY](./media/ssh-from-windows/save-ppk-file.png)

    Se pretender guardar a chave privada no formato OpenSSH, o formato de chave privada utilizado por muitos clientes SSH, clique em **conversões** > **OpenSSH exportar chave**.

## <a name="provide-ssh-public-key-when-deploying-a-vm"></a>Forneça a chave pública SSH ao implementar uma VM

Para criar uma VM com Linux que utilizar chaves SSH para a autenticação, forneça a chave pública SSH ao criar a VM com o portal do Azure ou outros métodos.

O exemplo seguinte mostra como poderia copiar e colar esta chave pública no portal do Azure, quando criar uma VM com Linux. A chave pública, em seguida, é normalmente armazenada num `~/.ssh/authorized_keys` na sua nova VM.

   ![Utilize a chave pública, quando criar uma VM no portal do Azure](./media/ssh-from-windows/use-public-key-azure-portal.png)


## <a name="connect-to-your-vm"></a>Ligar à VM

Uma forma de efetuar uma ligação SSH para a VM com Linux a partir do Windows é para utilizar um cliente SSH. Este é o método preferencial, se tiver um cliente SSH instalado no seu sistema do Windows, ou utilizar ferramentas SSH no Bash na Shell de nuvem do Azure. Se preferir uma ferramenta baseada na GUI, pode ligar com o PuTTY.  

### <a name="use-an-ssh-client"></a>Utilizar um cliente SSH
Com a chave pública implementado na sua VM do Azure e a chave privada no seu sistema local, o SSH para a VM com o endereço IP ou nome DNS da sua VM. Substitua *azureuser* e *myvm.westus.cloudapp.azure.com* no seguinte comando com o nome de utilizador de administrador e o nome de domínio completamente qualificado (ou endereço IP):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Se uma frase de acesso que configurou quando criou o par de chaves, introduza a frase de acesso quando lhe for pedido durante o processo de início de sessão.

### <a name="connect-with-putty"></a>Ligar com PuTTY

Se tiver instalado o [pacote de transferência PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) e anteriormente gerou uma chave privada PuTTY (ficheiro. ppk), pode ligar à VM Linux com o PuTTY.

1. Inicie o PuTTy.

2. Preencha o nome de anfitrião ou endereço IP da sua VM do portal do Azure:

    ![Abrir nova ligação PuTTY](./media/ssh-from-windows/putty-new-connection.png)

3. Antes de selecionar **abra**, clique em **ligação** > **SSH** > **Auth** separador. Procure e selecione a sua chave privada PuTTY (ficheiro. ppk):

    ![Selecione a sua chave privada PuTTY para autenticação](./media/ssh-from-windows/putty-auth-dialog.png)

4. Clique em **abra** para ligar à VM.

## <a name="next-steps"></a>Passos Seguintes

* Para obter passos detalhados, as opções e avançados exemplos de como trabalhar com chaves SSH, consulte [pares de chaves de passos detalhados sobre como criar SSH](create-ssh-keys-detailed.md).

* Também pode utilizar o PowerShell na Shell de nuvem do Azure para gerar chaves SSH e efetuar ligações SSH para VMs com Linux. Consulte o [início rápido do PowerShell](../../cloud-shell/quickstart-powershell.md#ssh).

* Se tiver problemas em utilizar o SSH para ligar à sua VMs com Linux, consulte [resolver problemas de SSH ligações a uma VM com Linux do Azure](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
