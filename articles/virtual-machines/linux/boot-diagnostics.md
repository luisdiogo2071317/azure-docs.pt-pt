---
title: Arranque diagnósticos para máquinas virtuais do Linux no Azure | Documentação da Microsoft
description: Descrição geral das funcionalidades de depuração dois para computadores virtuais Linux no Azure
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: Deland-Han
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/19/2018
ms.author: delhan
ms.openlocfilehash: bf8e1b338012898ed3de3f443cf492b6890af796
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="how-to-use-boot-diagnostics-to-troubleshoot-linux-virtual-machines-in-azure"></a>Como utilizar o diagnóstico de arranque para resolver problemas de computadores virtuais Linux no Azure

Está agora disponível o suporte de duas funcionalidades de depuração no Azure: Saída da Consola e Captura de Ecrã para o modelo de implementação das Máquinas Virtuais do Azure (Resource Manager). 

Quando colocar a sua própria imagem no Azure ou mesmo arrancar uma das imagens da plataforma, podem existir muitos motivos pelos quais uma Máquina Virtual entra num estado de não arranque. Estas funcionalidades permitem-lhe diagnosticar e recuperar facilmente Máquinas Virtuais de falhas de arranque.

Para Máquinas Virtuais do Linux, pode visualizar facilmente a saída do registo da consola a partir do Portal:

![Portal do Azure](./media/boot-diagnostics/screenshot1.png)
 
No entanto, para Máquinas Virtuais do Windows e do Linux, o Azure também permite ver uma captura de ecrã da VM a partir do hipervisor:

![Erro](./media/boot-diagnostics/screenshot2.png)

Estas duas funcionalidades são suportadas para Máquinas Virtuais do Azure em todas as regiões. Tenha em atenção que as capturas de ecrã e a saída podem demorar até 10 minutos a aparecer na sua conta de armazenamento.

## <a name="common-boot-errors"></a>Erros de arranque comuns

- [Problemas de sistema de ficheiros](https://support.microsoft.com/help/3213321/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck) 
- [Problemas de kernel](https://support.microsoft.com/help/4091524/how-recovery-azure-linux-vm-from-kernel-related-boot-related-issues/) 
- [Erros FSTAB](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors)

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a>Ativar o diagnóstico numa máquina virtual nova
1. Quando criar uma nova Máquina Virtual a partir do Portal de Pré-visualização, selecione **Azure Resource Manager** na lista pendente do modelo de implementação:
 
    ![Resource Manager](./media/boot-diagnostics/screenshot3.jpg)

2. Configure a opção de Monitorização para selecionar a conta de armazenamento onde quer colocar estes ficheiros de diagnóstico.
 
    ![Criar VM](./media/boot-diagnostics/screenshot4.jpg)

3. Se estiver a implementar a partir de um modelo do Azure Resource Manager, navegue para o recurso da Máquina Virtual e acrescente a secção do perfil de diagnóstico. Não se esqueça de utilizar o cabeçalho da versão da API "2015-06-15".

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

## <a name="update-an-existing-virtual-machine"></a>Atualizar uma máquina virtual existente

Para ativar o diagnóstico de arranque através do portal, também pode atualizar uma máquina virtual existente através do portal. Selecione as opções Diagnóstico de Arranque e Guardar. Reinicie a VM para entrar em vigor.

![Atualizar VM Existente](./media/boot-diagnostics/screenshot5.png)

## <a name="next-steps"></a>Passos Seguintes

Se vir um erro "Falha ao obter o conteúdo do registo" quando utilizar o diagnóstico de arranque de VM, consulte [Falha ao obter o conteúdo do erro de registo na VM diagnóstico de arranque](https://support.microsoft.com/help/4094480/failed-to-get-contents-of-the-log-error-in-vm-boot-diagnostics-in-azur).