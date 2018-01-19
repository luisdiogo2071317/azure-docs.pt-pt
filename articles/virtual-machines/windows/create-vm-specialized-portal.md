---
title: Criar uma VM do Windows a partir de um VHD especializado no portal do Azure | Microsoft Docs
description: Crie uma nova VM do Windows a partir de um VHD no portal do Azure.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: cynthn
ms.openlocfilehash: 33e94777047ea8a116ae6f393439521356a53509
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="create-a-vm-from-a-vhd-using-the-azure-portal"></a>Criar uma VM a partir de um VHD com o portal do Azure


Existem várias formas de criar as VMs no Azure. Se já tiver um VHD para utilizar ou pretende copiar o VHD do e VM existente a utilizar, pode criar uma nova VM ao anexar o VHD como disco do SO. Este processo *anexa* o VHD para uma nova VM como disco do SO.

Também pode criar uma nova VM do VHD de uma VM que tenha sido eliminada. Por exemplo, se tiver uma VM do Azure que não está a funcionar corretamente, pode eliminar a VM e utilizar o VHD para criar uma nova VM. Pode reutilizar o mesmo VHD ou criar a cópia do VHD ao criar um instantâneo e, em seguida, criar um novo disco gerido do instantâneo. Demora alguns mais passos, mas assegura que pode manter o VHD original e também fornece um instantâneo de contingência se for necessário.

Tiver uma VM no local que pretende utilizar para criar uma VM no Azure. Pode carregar o VHD e anexe-o a uma nova VM. Para carregar um VHD, terá de utilizar o PowerShell ou outra ferramenta para carregá-la para uma conta de armazenamento, em seguida, criar um disco gerido do VHD. Para obter mais informações, consulte [carregar um VHD especializado](create-vm-specialized.md#option-2-upload-a-specialized-vhd)

Se pretender utilizar uma VM ou o VHD para criar várias VMs, não deve utilizar este método. Para implementações maiores, deve [criar uma imagem](capture-image-resource.md) e, em seguida, [utilizar essa imagem para criar várias VMs](create-vm-generalized-managed.md).


## <a name="copy-a-disk"></a>Copiar um disco

Criar um instantâneo e, em seguida, criar um disco do instantâneo. Isto permite-lhe manter o VHD original de uma enquadram-se novamente.

1. No menu à esquerda, clique em **todos os recursos**.
2. No **todos os tipos de** pendente, anule a seleção **Selecionar tudo** e, em seguida, desloque para baixo e selecione **discos** para localizar os discos disponíveis.
3. Clique no disco que pretende utilizar. O **descrição geral** página para o disco é aberto.
4. Na página de descrição geral, no menu na parte superior, clique em **+ criar instantâneo**. 
5. Escreva um nome para o instantâneo.
6. Escolha um **grupo de recursos** para o instantâneo. Pode utilizar um grupo de recursos existente ou crie um novo.
7. Escolha se pretende utilizar standard (HDD) ou armazenamento Premium (SDD).
8. Quando tiver terminado, clique em **criar** para criar o instantâneo.
9. Depois do instantâneo ter sido criado, clique em **+ criar um recurso** no menu esquerdo.
10. Na barra de pesquisa, escreva **disco gerido** e selecione **discos geridos** da lista.
11. No **discos geridos** página, clique em **criar**.
12. Escreva um nome para o disco.
13. Escolha um **grupo de recursos** para o disco. Pode utilizar um grupo de recursos existente ou crie um novo. Esta será também o grupo de recursos onde criar a VM a partir do disco.
14. Escolha se pretende utilizar standard (HDD) ou armazenamento Premium (SDD).
15. No **tipo de origem**, certifique-se **instantâneo** está selecionada.
16. No **instantâneo de origem** pendente, selecione o instantâneo de que pretende utilizar.
17. Fazer ajustamentos outros conforme necessário e, em seguida, clique em **criar** para criar o disco.

## <a name="create-a-vm-from-a-disk"></a>Criar uma VM a partir de um disco

Assim que tiver o disco gerido VHD que pretende utilizar, pode criar a VM no portal.

1. No menu à esquerda, clique em **todos os recursos**.
2. No **todos os tipos de** pendente, anule a seleção **Selecionar tudo** e, em seguida, desloque para baixo e selecione **discos** para localizar os discos disponíveis.
3. Clique no disco que pretende utilizar. O **descrição geral** página para o disco é aberto.
Na página Descrição geral, certifique-se de que **estado do disco** está listado como **Unattached**. Se não for, poderá ter de exposição do disco da VM ou eliminar a VM para libertar o disco.
4. No menu na parte superior do painel, clique em **+ criar VM**.
5. No **Noções básicas** página para a nova VM, escreva um nome e selecione a um grupo de recursos existente ou crie um novo.
6. No **tamanho** página, selecione uma página de tamanho da VM e, em seguida, clique em **selecione**.
7. No **definições** página, pode optar por permitir que o portal criar todos os novos recursos ou pode selecionar um existente **rede Virtual** e **grupo de segurança de rede**. O portal crie sempre uma nova NIC e o endereço IP público para a nova VM. 
8. Efetue as alterações às opções de monitorização e adicione quaisquer extensões conforme necessário.
9. Quando tiver terminado, clique em **OK**. 
10. Se a configuração de VM ser aprovado na validação, clique em **OK** para iniciar a implementação.

## <a name="next-steps"></a>Passos Seguintes

Também pode utilizar o PowerShell para [carregar um VHD para o Azure e criar uma VM especializada](create-vm-specialized.md).


