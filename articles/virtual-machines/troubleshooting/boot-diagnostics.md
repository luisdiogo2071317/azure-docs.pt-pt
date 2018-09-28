---
title: Diagnóstico de arranque para VMs no Azure | Microsoft Docs
description: Descrição geral de duas funcionalidades de depuração para máquinas virtuais no Azure
services: virtual-machines
author: Deland-Han
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.openlocfilehash: 64641f8acfe7b58763756e2a0707fa799ee804b2
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47414679"
---
# <a name="how-to-use-boot-diagnostics-to-troubleshoot-virtual-machines-in-azure"></a>Como utilizar o diagnóstico de arranque para resolver problemas relacionados com máquinas virtuais no Azure

O suporte de duas funcionalidades de depuração está agora disponível no Azure: saída da consola e captura de ecrã de suporte para o modelo de implementação do Gestor de recursos de máquinas virtuais do Azure. 

Quando colocar a sua própria imagem para o Azure ou mesmo arrancar uma das imagens da plataforma, podem existir muitos motivos por que uma máquina virtual entra num Estado de não arranque. Estas funcionalidades permitem-lhe facilmente diagnosticar e recuperar suas máquinas virtuais de falhas de arranque.

Para máquinas virtuais do Linux, pode visualizar facilmente a saída de registo da consola a partir do Portal. Para máquinas virtuais Windows e Linux, o Azure também permite-lhe ver uma captura de ecrã da VM do hipervisor. Esses dois recursos são suportadas para máquinas virtuais do Azure em todas as regiões. Tenha em atenção que as capturas de ecrã e a saída podem demorar até 10 minutos a aparecer na sua conta de armazenamento.

## <a name="common-boot-errors"></a>Erros de arranque comuns

- [0xC000000E](https://support.microsoft.com/help/4010129)
- [0xC000000F](https://support.microsoft.com/help/4010130)
- [0xC0000011](https://support.microsoft.com/help/4010134)
- [0xC0000034](https://support.microsoft.com/help/4010140)
- [0xC0000098](https://support.microsoft.com/help/4010137)
- [0xC00000BA](https://support.microsoft.com/help/4010136)
- [0xC000014C](https://support.microsoft.com/help/4010141)
- [0xC0000221](https://support.microsoft.com/help/4010132)
- [0xC0000225](https://support.microsoft.com/help/4010138)
- [0xC0000359](https://support.microsoft.com/help/4010135)
- [0xC0000605](https://support.microsoft.com/help/4010131)
- [Não foi encontrado nenhum sistema operativo](https://support.microsoft.com/help/4010142)
- [Falha de arranque ou INACCESSIBLE_BOOT_DEVICE](https://support.microsoft.com/help/4010143)

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a>Ativar o diagnóstico numa máquina virtual nova
1. Ao criar uma nova máquina virtual a partir do Portal do Azure, selecione o **do Azure Resource Manager** na lista pendente de modelo de implementação:
 
    ![Resource Manager](./media/virtual-machines-common-boot-diagnostics/screenshot3.jpg)

2. Na **configurações**, ativar a **diagnósticos de arranque**e, em seguida, selecione uma conta de armazenamento que pretende colocar esses ficheiros de diagnóstico.
 
    ![Criar VM](./media/virtual-machines-common-boot-diagnostics/create-storage-account.png)

    > [!NOTE]
    > A funcionalidade de diagnóstico de arranque não suporta a conta de armazenamento premium. Se utilizar a conta de armazenamento premium para o diagnóstico de arranque, poderá receber o erro StorageAccountTypeNotSupported quando iniciar a VM.
    >
    > 

3. Se estiver a implementar a partir de um modelo Azure Resource Manager, navegue para o recurso de máquina virtual e acrescente a secção de perfil de diagnóstico. Não se esqueça de utilizar o cabeçalho da versão da API "2015-06-15".

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. O perfil de diagnóstico permite-lhe selecionar a conta de armazenamento onde quer colocar estes registos.

    ```json
            "diagnosticsProfile": {
                "bootDiagnostics": {
                "enabled": true,
                "storageUri": "[concat('http://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
                }
            }
            }
        }
    ```

Para implementar uma máquina virtual de exemplo com o diagnóstico de arranque ativado, consulte nosso repositório aqui.

## <a name="enable-boot-diagnostics-on-existing-virtual-machine"></a>Ativar o diagnóstico de arranque numa máquina virtual existente 

Para ativar o diagnóstico de arranque numa máquina virtual existente, siga estes passos:

1. Inicie sessão para o [portal do Azure](https://portal.azure.com)e, em seguida, selecione a máquina virtual.
2. Na **suporte + resolução de problemas**, selecione **diagnósticos de arranque** > **definições**, altere o estado para **em**e, em seguida, Selecione uma conta de armazenamento. 
4. Certifique-se de que a opção de diagnóstico de arranque está selecionada e, em seguida, guarde a alteração.

    ![Atualizar VM Existente](./media/virtual-machines-common-boot-diagnostics/enable-for-existing-vm.png)

3. Reinicie a VM para entrar em vigor.


