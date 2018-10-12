---
title: Anexar um disco de dados geridos para uma VM do Windows - Azure | Documentos da Microsoft
description: Como anexar um disco de dados geridos para uma VM do Windows com o portal do Azure.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: cynthn
ms.openlocfilehash: 8d83af114ebb5e5ff78372897d3e08ed592d4012
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/11/2018
ms.locfileid: "49093906"
---
# <a name="attach-a-managed-data-disk-to-a-windows-vm-by-using-the-azure-portal"></a>Anexar um disco de dados geridos para uma VM do Windows com o portal do Azure

Este artigo mostra-lhe como anexar um novo disco de dados geridos para uma máquina virtual de Windows (VM) com o portal do Azure. O tamanho da VM determina quantos discos de dados, pode anexar. Para obter mais informações, consulte [tamanhos de máquinas virtuais](sizes.md).


## <a name="add-a-data-disk"></a>Adicionar um disco de dados

1. Na [portal do Azure](https://portal.azure.com), no menu à esquerda, selecione **máquinas virtuais**.
2. Selecione uma máquina virtual a partir da lista.
3. Sobre o **Máquina Virtual** página, selecione **discos**.
4. Sobre o **discos** página, selecione **adicionar disco de dados**.
5. Na lista pendente para o novo disco, selecione **criar disco**.
6. Na **disco gerido de criar** página, escreva um nome para o disco e ajustar as outras definições conforme necessário. Quando terminar, selecione **Criar**.
7. Na **discos** página, selecione **guardar** para guardar a nova configuração de disco para a VM.
8. Depois do Azure cria o disco e anexa-o para a máquina virtual, o novo disco está listado nas definições de disco da máquina virtual em **discos de dados**.


## <a name="initialize-a-new-data-disk"></a>Inicializar um novo disco de dados

1. Ligue à VM.
1. Selecione o Windows **começar** menu no interior da VM em execução e introduza **Diskmgmt. msc** na caixa de pesquisa. O **gestão de discos** consola é aberta.
2. Gestão de discos reconhece que tem um disco novo, não inicializado e o **Inicializar disco** é apresentada a janela.
3. Certifique-se de que o novo disco está selecionado e, em seguida, selecione **OK** para inicializá-la.
4. O novo disco aparece como **não alocado**. Com o botão direito em qualquer lugar no disco e selecione **novo volume simples**. O **Assistente de novo Volume simples** é aberta a janela.
5. Avance no assistente, manter todos os padrões, e quando terminar, selecione **concluir**.
6. Fechar **gestão de discos**.
7. É apresentada uma janela de pop-up a notificá-lo de que precisa de formatar o disco novo antes de poder utilizá-lo. Selecione **formatar disco**.
8. Na **Formatar novo disco** janela, verifique as definições e, em seguida, selecione **iniciar**.
9. É apresentado um aviso a informar de que os discos de formatação apague todos os dados. Selecione **OK**.
10. Quando a formatação estiver concluída, selecione **OK**.

## <a name="use-trim-with-standard-storage"></a>Utilizar TRIM com o armazenamento standard

Se utilizar o armazenamento standard (HDD), deve ativar os **TRIM** comando. O **TRIM** comando descarta blocos não utilizados no disco, de modo a que é cobrado apenas pelo armazenamento que está realmente usando. Usando **TRIM**, podem poupar nos custos, se cria arquivos grandes e, em seguida, eliminá-los mais tarde. 

Para verificar a **TRIM** definir, abra um prompt de comando na sua VM do Windows e introduza o seguinte comando:

```
fsutil behavior query DisableDeleteNotify
```

Se o comando devolve 0, **TRIM** está ativada corretamente. Caso contrário, se ela retornar 1, execute o seguinte comando para habilitar **TRIM**:

```
fsutil behavior set DisableDeleteNotify 0
```

Depois de eliminar dados do seu disco, é possível garantir a **TRIM** alinhadas corretamente através da execução de operações de desfragmentação com **TRIM**:

```
defrag.exe <volume:> -l
```

Também pode formatar o volume para garantir que todo o volume é ajustado.

## <a name="next-steps"></a>Passos Seguintes

- Também pode [anexar um disco de dados com o PowerShell](attach-disk-ps.md).
- Se a sua aplicação precisa de utilizar o *D:* unidade para armazenar dados, pode [alterar a letra de unidade do disco temporário Windows](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
