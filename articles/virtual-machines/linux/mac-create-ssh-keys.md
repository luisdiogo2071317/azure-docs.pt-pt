---
title: Criar e utilizar um par de chaves SSH para VMs do Linux no Azure | Microsoft Docs
description: "Como criar e utilizar um par de chaves públicas e privadas SSH para VMs do Linux no Azure para melhorar a segurança do processo de autenticação."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/13/2017
ms.author: iainfou
ms.openlocfilehash: ecd3a01ee5591cb09140edb1b1290ff2d4510200
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="how-to-create-and-use-an-ssh-public-and-private-key-pair-for-linux-vms-in-azure"></a>Como criar e utilizar um par de chaves públicas e privadas SSH para VMs do Linux no Azure
Com um par de chaves SSH (secure shell), pode criar máquinas virtuais (VMs) no Azure que utilizam chaves SSH para autenticação e eliminar a necessidade de palavras-passe para iniciar sessão. Este artigo mostra-lhe como gerar e utilizar rapidamente um par de ficheiros de chaves públicas e privadas RSA versão 2 do protocolo SSH para VMs do Linux. Pode concluir estes passos com o Azure Cloud Shell, um anfitrião do macOS ou Linux ou um Subsistema do Windows para Linux. Para obter passos mais detalhados e exemplos adicionais, veja [passos detalhados para criar pares e certificados de chaves SSH](create-ssh-keys-detailed.md).

## <a name="create-an-ssh-key-pair"></a>Criar um par de chaves SSH
Utilize o `ssh-keygen` comando para criar públicos e privados ficheiros de chaves SSH que estão por predefinição criada no `~/.ssh` diretório. Pode especificar uma localização diferente e o frase de acesso adicional (uma palavra-passe para aceder ao ficheiro de chave privado) quando lhe for pedido. Se um par de chaves SSH existe na localização atual, serão substituídos.

```bash
ssh-keygen -t rsa -b 2048
```

## <a name="use-the-ssh-key-pair"></a>Utilizar o par de chaves SSH
A chave pública que colocar na sua VM do Linux no Azure está armazenada por predefinição em `~/.ssh/id_rsa.pub`, a menos que tenha alterado a localização quando a criou. Se utilizar a [CLI 2.0 do Azure](/cli/azure) para criar a VM, especifique a localização desta chave pública quando utilizar o comando [az vm create](/cli/azure/vm#az_vm_create) com a opção `--ssh-key-path`. Se copiar e colar o conteúdo do ficheiro de chave pública a utilizar no portal do Azure ou um modelo do Resource Manager, certifique-se de que não copia nenhum espaço em branco adicional. Por exemplo, se utilizar o OS X, pode encaminhar o ficheiro de chave pública (por predefinição, **~/.ssh/id_rsa.pub**) para **pbcopy** para copiar o conteúdo (existem outros programas Linux que fazem o mesmo, tais como `xclip`).

Se não estiver familiarizado com chaves públicas SSH, pode ver a chave pública ao executar `cat` da seguinte forma e ao substituir `~/.ssh/id_rsa.pub` pela sua própria localização do ficheiro de chave pública:

```bash
cat ~/.ssh/id_rsa.pub
```

Com a chave pública na VM do Azure, mova o SSH para a VM com o endereço IP ou o nome DNS da sua VM (lembre-se de substituir `azureuser` e `myvm.westus.cloudapp.azure.com` abaixo pelo nome de utilizador de administrador e o nome de domínio completamente qualificado, ou endereço IP):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Se tiver indicado uma frase de acesso quando criou o par de chaves, introduza-a quando lhe for pedido durante o processo de início de sessão. (O servidor é adicionado à pasta `~/.ssh/known_hosts` e não lhe será pedido para ligar novamente até a chave pública na VM do Azure ser alterada ou o nome do servidor ser removido do `~/.ssh/known_hosts`.)

## <a name="next-steps"></a>Passos Seguintes

As VMs criadas com chaves SSH são, por predefinição, configuradas com as palavras-passe desativadas, para tornar as tentativas de adivinhação forçadas muito mais dispendiosas e, desse modo, difíceis. Este tópico descreve a criação de um par de chaves SSH simples para utilização rápida. Se necessitar de mais assistência na criação do par de chaves SSH ou precisar de certificados adicionais, veja [Passos detalhados para criar pares de chaves e certificados SSH](create-ssh-keys-detailed.md).

Pode criar VMs que utilizam o par de chaves SSH com o portal do Azure, CLI e modelos:

* [Criar uma VM do Linux segura com o portal do Azure](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Criar uma VM do Linux segura com a CLI 2.0 do Azure](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Criar uma VM do Linux segura com um modelo do Azure](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
