---
title: Resolução de problemas de pilha do Microsoft Azure | Microsoft Docs
description: Azure pilha Development Kit (ASDK) informações de resolução de problemas.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 6c715f07f75c9196b7cf2cc8659c6e541e1260da
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="microsoft-azure-stack-development-kit-asdk-troubleshooting"></a>Resolução de problemas do Microsoft Azure pilha Development Kit (ASDK)
Este documento fornece informações de resolução de problemas comuns para o ASDK. Se estiver a ter um problema que não é documentado, certifique-se verificar o [fórum MSDN do Azure pilha](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) para obter assistência adicional e informações.  

> [!IMPORTANT]
> Porque o ASDK é um ambiente de avaliação, não há nenhum oficial de suporte oferecida através da Microsoft ao cliente suportar (CSS).

As recomendações de resolução de problemas que são descritos nesta secção são derivadas de várias origens e podem ou não poderão resolver o problema específico. Exemplos de código são fornecidos "tal como está" e não podem ser garantidos resultados esperados. Nesta secção está sujeita a edições frequentes e atualizações, melhoramentos ao produto são implementados.

## <a name="deployment"></a>Implementação
### <a name="deployment-failure"></a>Falha de implementação
Se ocorrer uma falha durante a instalação, pode reiniciar a implementação do passo falhado utilizando-opção volte a executar o script de implementação como no exemplo seguinte:

  ```powershell
  cd C:\CloudDeployment\Setup
  .\InstallAzureStackPOC.ps1 -Rerun
  ```

### <a name="at-the-end-of-the-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>No final da implementação, a sessão do PowerShell é aberta e não mostra resultados
Este comportamento é, provavelmente, apenas o resultado do comportamento predefinido de uma janela de comando do PowerShell, quando foi selecionada. A implementação do kit de desenvolvimento foi bem sucedida, mas o script foi colocada em pausa quando selecionar a janela. Pode verificar a configuração foi concluída, procurando a palavra "selecionar" no titlebar da janela de comando. Prima a tecla ESC para anular-a e a mensagem de conclusão deve ser apresentada depois.

## <a name="virtual-machines"></a>Máquinas virtuais
### <a name="default-image-and-gallery-item"></a>Item de imagem e galeria predefinido
Tem de adicionar um item de imagem e da galeria do Windows Server antes de implementar VMs na pilha do Azure.

### <a name="after-restarting-my-azure-stack-host-some-vms-may-not-automatically-start"></a>Depois de reiniciar o meu anfitrião de pilha do Azure, algumas VMs poderão não iniciar automaticamente.
Depois de reiniciar o seu anfitrião, poderá reparar serviços do Azure pilha não estão imediatamente disponíveis. Isto acontece porque a pilha de Azure [infraestrutura VMs](asdk-architecture.md#virtual-machine-roles) e RPs tomar algumas tempo para verificar a consistência, mas, eventualmente, serão automaticamente iniciado.

Também poderá reparar que inquilino que VMS não iniciar automaticamente após um reinício do anfitrião de kit de desenvolvimento de pilha do Azure. Este é um problema conhecido e requer apenas alguns passos manuais para colocá-los online:

1.  No anfitrião de kit de desenvolvimento de pilha do Azure, iniciar **Gestor de clusters de ativação pós-falha** no menu Iniciar.
2.  Selecione o cluster **S Cluster.azurestack.local**.
3.  Selecione **funções**.
4.  VMs de inquilino são apresentados num *guardado* estado. Assim que estiver a executar todas as VMs de infraestrutura, as VMs de inquilino com o botão direito e selecione **iniciar** para retomar a VM.

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>Tem de eliminar algumas máquinas virtuais, mas ainda ver os ficheiros VHD no disco. Este comportamento é esperado?
Sim, isto é comportamento esperado. Foi concebido desta forma porque:

* Quando elimina uma VM, VHDs não são eliminados. Os discos estão separados recursos no grupo de recursos.
* Quando uma conta de armazenamento é eliminada, a eliminação está visível imediatamente através do Azure Resource Manager, mas os discos que podem conter ainda são mantidos no armazenamento até que a recolha de lixo é executado.

Se vir "orphan" VHDs, é importante saber se o que fazem parte da pasta para a conta de armazenamento que foi eliminada. Se a conta de armazenamento não foi eliminada, é normal são ainda não existe.

Pode ler mais sobre como configurar a recuperação de limiar e a pedido de retenção no [gerir contas de armazenamento](.\.\azure-stack-manage-storage-accounts.md).

## <a name="storage"></a>Armazenamento
### <a name="storage-reclamation"></a>Recuperação de armazenamento
Pode demorar até 14 horas para a capacidade de reclamada a aparecer no portal. Recuperação de espaço depende de vários fatores, incluindo a percentagem de utilização dos ficheiros de contentor interno no arquivo de blob de bloco. Por conseguinte, dependendo da quantidade de dados é eliminado, não há nenhuma garantia, a quantidade de espaço que foi recuperado quando executa o recoletor de lixo.

## <a name="next-steps"></a>Passos Seguintes
[Visite o Fórum de suporte de pilha do Azure](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack)

