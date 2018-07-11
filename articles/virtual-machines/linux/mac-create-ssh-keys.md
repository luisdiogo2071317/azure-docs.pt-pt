---
title: Criar e utilizar um par de chaves SSH para VMs do Linux no Azure | Microsoft Docs
description: Como criar e utilizar um par de chaves públicas-privadas SSH para VMs do Linux no Azure para melhorar a segurança do processo de autenticação.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: cynthn
ms.openlocfilehash: 2e3d86d776f44c47a33bf075cf7f2140a3940e5e
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37928459"
---
# <a name="quick-steps-create-and-use-an-ssh-public-private-key-pair-for-linux-vms-in-azure"></a>Passos rápidos: criar e utilizar um par de chaves públicas-privadas SSH para VMs do Linux no Azure
Com um par de chaves SSH (secure shell), pode criar máquinas virtuais (VMs) no Azure que utilizam chaves SSH para autenticação e eliminar a necessidade de palavras-passe para iniciar sessão. Este artigo mostra-lhe como gerar e utilizar um par de ficheiros de chaves públicas-privadas de SSH para VMs do Linux rapidamente. Pode concluir estes passos com o Azure Cloud Shell, um macOS ou anfitrião Linux, o subsistema Windows para Linux e outras ferramentas que suportam o OpenSSH. 

Para obter mais informações e exemplos, consulte [pares de chaves de passos detalhados para criar SSH](create-ssh-keys-detailed.md).

Para outras formas de gerar e utilizar chaves SSH num computador Windows, consulte [chaves de como utilizar SSH com Windows no Azure](ssh-from-windows.md).

[!INCLUDE [virtual-machines-common-ssh-support](../../../includes/virtual-machines-common-ssh-support.md)]

## <a name="create-an-ssh-key-pair"></a>Criar um par de chaves SSH
Utilize o `ssh-keygen` comando para gerar públicos e privados ficheiros de chaves SSH por predefinição, criada no `~/.ssh` diretório. Pode especificar uma localização diferente e uma frase de acesso adicional (uma palavra-passe para aceder ao ficheiro de chave privado) quando lhe for pedido. Se existir um par de chaves SSH na localização atual, esses arquivos são substituídos.

```bash
ssh-keygen -t rsa -b 2048
```

Se utilizar o [CLI 2.0 do Azure](/cli/azure) para criar a sua VM, opcionalmente, pode gerar ficheiros de chaves públicos e privados SSH ao executar o [criar az vm](/cli/azure/vm#az_vm_create) comando com o `--generate-ssh-keys` opção. As chaves são armazenadas no diretório ~/.ssh. Tenha em atenção que esta opção de comando não substituir chaves se ainda existirem nessa localização.

## <a name="provide-ssh-public-key-when-deploying-a-vm"></a>Forneça a chave pública SSH ao implementar uma VM
Para criar uma VM do Linux utilizar chaves SSH para autenticação, especifique a chave pública SSH ao criar a VM com o portal do Azure, CLI, modelos do Resource Manager ou outros métodos:

* [Criar uma máquina virtual Linux com o portal do Azure](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Criar uma máquina virtual Linux com a CLI do Azure](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Criar uma VM do Linux com um modelo do Azure](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Se não estiver familiarizado com o formato de uma chave pública SSH, pode ver a chave pública, executando `cat` como a seguir, substituindo `~/.ssh/id_rsa.pub` com sua própria localização do ficheiro de chave pública:

```bash
cat ~/.ssh/id_rsa.pub
```

Se copiar e colar o conteúdo do ficheiro de chave pública a utilizar no portal do Azure ou um modelo do Resource Manager, certifique-se de que não copia nenhum espaço em branco adicional. Por exemplo, se usar o macOS, pode encaminhar o ficheiro de chave pública (por predefinição, `~/.ssh/id_rsa.pub`) para **pbcopy** para copiar o conteúdo (existem outros programas Linux que fazem a mesma coisa, como **xclip**).

A chave pública que colocar na sua VM do Linux no Azure é armazenada por predefinição em `~/.ssh/id_rsa.pub`, a menos que tenha alterado a localização quando criou as chaves. Se utilizar o [CLI 2.0 do Azure](/cli/azure) para criar a sua VM com uma chave pública existente, especificar o valor ou a localização desta chave pública ao executar o [criar az vm](/cli/azure/vm#az_vm_create) comando com o `--ssh-key-value` opção. 

## <a name="ssh-to-your-vm"></a>SSH para a VM
Com a chave pública implementada na sua VM do Azure e a chave privada no seu sistema local, o SSH para a VM com o endereço IP ou nome DNS da sua VM. Substitua *azureuser* e *myvm.westus.cloudapp.azure.com* no comando seguinte com o nome de utilizador de administrador e o nome de domínio completamente qualificado (ou endereço IP):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Se tiver indicado uma frase de acesso quando criou o par de chaves, introduza-a quando lhe for pedido durante o processo de início de sessão. (O servidor é adicionado à pasta `~/.ssh/known_hosts` e não lhe será pedido para ligar novamente até a chave pública na VM do Azure ser alterada ou o nome do servidor ser removido do `~/.ssh/known_hosts`.)

As VMs criadas com chaves SSH são, por predefinição, configuradas com as palavras-passe desativadas, para tornar as tentativas de adivinhação forçadas muito mais dispendiosas e, desse modo, difíceis. 

## <a name="next-steps"></a>Passos Seguintes

Este artigo descrito a criação de um par de chaves SSH simple para utilização rápida. 

* Se precisar de mais ajuda para trabalhar com o par de chaves SSH, veja [pares de chaves de obter os passos detalhados para criar e gerir SSH](create-ssh-keys-detailed.md).

* Se tiver problemas com ligações SSH para uma VM do Azure, veja [resolver problemas de SSH ligações a uma VM do Linux do Azure](troubleshoot-ssh-connection.md).


