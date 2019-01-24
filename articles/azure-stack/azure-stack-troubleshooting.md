---
title: Resolução de problemas do Microsoft Azure Stack | Documentos da Microsoft
description: O Azure Stack de resolução de problemas.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: a20bea32-3705-45e8-9168-f198cfac51af
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2019
ms.author: jeffgilb
ms.reviewer: unknown
ms.openlocfilehash: a74fb749e130b565c44c637bfc16ff09e3314a05
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54857170"
---
# <a name="microsoft-azure-stack-troubleshooting"></a>Resolução de problemas do Microsoft Azure Stack

Este documento fornece informações de resolução de problemas comuns para o Azure Stack. 

> [!NOTE]
> Uma vez que o Azure Stack Technical Development Kit (ASDK) é oferecido como um ambiente de avaliação, não existe nenhum oficial de suporte dos serviços de suporte de cliente do Microsoft. Se estiver tendo um problema, certifique-se verificar a [fórum MSDN do Azure Stack](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) para obter mais ajuda e informações.  

As recomendações para a solução de problemas que são descritos nesta secção são derivadas de várias origens e podem ou não podem ser resolvido o problema específico. Exemplos de código são fornecidos tal como estão e não for possível garantir os resultados esperados. Esta secção é sujeitos a edições freqüentes e atualizações, melhorias ao produto são implementadas.

## <a name="deployment"></a>Implementação
### <a name="general-deployment-failure"></a>Falha de implementação geral
Se ocorrer uma falha durante a instalação, pode reiniciar implementação a partir do passo falhado, utilizando a – opção nova execução do script de implementação.  

### <a name="at-the-end-of-asdk-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>No final da implementação de ASDK, a sessão do PowerShell ainda está aberta e não mostra nenhuma saída.
Este comportamento é provavelmente somente o resultado do comportamento predefinido de uma janela de comando do PowerShell, quando foi selecionada. A implementação do kit de desenvolvimento foi concluída com êxito, mas o script foi colocada em pausa quando selecionar a janela. Pode verificar a configuração foi concluída para procurar a palavra "selecionar" na barra de títulos da janela de comando.  Prima a tecla ESC para desmarcá-lo e a mensagem de conclusão deve ser mostrada depois dela.

### <a name="deployment-fails-due-to-lack-of-external-access"></a>Implementação falha devido à falta de acesso externo
Quando a implementação falha nos estágios onde é necessário o acesso externo, retornará uma exceção semelhante ao seguinte exemplo:

```
An error occurred while trying to test identity provider endpoints: System.Net.WebException: The operation has timed out.
   at Microsoft.PowerShell.Commands.WebRequestPSCmdlet.GetResponse(WebRequest request)
   at Microsoft.PowerShell.Commands.WebRequestPSCmdlet.ProcessRecord()at, <No file>: line 48 - 8/12/2018 2:40:08 AM
```
Se este erro ocorrer, verifique para garantir que todos os requisitos mínimos de sistema de rede tiverem sido cumpridos, revendo os [documentação de tráfego de rede de implementação](deployment-networking.md). Uma ferramenta de verificação da rede também está disponível para parceiros como parte do Kit de ferramentas de parceiros.

Falhas de implementação com a exceção acima são normalmente devido a problemas de ligação a recursos na Internet

Para verificar se que este é o seu problema, pode executar os seguintes passos:

1. Powershell aberto
2. Enter-PSSession para o WAS01 ou qualquer uma das ERCs VMs
3. Execute o commandlet: Test-NetConnection login.windows.net-porta 443

Se este comando falhar, verifique se o comutador TOR e outros dispositivos de rede estiverem configurados para [permitir tráfego de rede](azure-stack-network.md).

## <a name="virtual-machines"></a>Máquinas virtuais
### <a name="default-image-and-gallery-item"></a>Item de imagem e galeria predefinido
Tem de adicionar um item de imagem e a galeria do Windows Server antes de implementar VMs no Azure Stack.

### <a name="after-restarting-my-azure-stack-host-some-vms-may-not-automatically-start"></a>Depois de reiniciar o meu host do Azure Stack, algumas VMs podem não iniciar automaticamente.
Depois de reiniciar o seu anfitrião, poderá reparar serviços do Azure Stack não estão disponíveis imediatamente.  Isto acontece porque o Azure Stack [VMs de infraestrutura](../azure-stack/asdk/asdk-architecture.md#virtual-machine-roles) e fornecedores de recursos demoram algum tempo para verificar a consistência, mas, eventualmente, serão iniciada automaticamente.

Também pode observar esse inquilino que VMS não são iniciados automaticamente após um reinício do anfitrião do kit de desenvolvimento do Azure Stack. Isso é um problema conhecido e requer apenas alguns passos manuais para colocá-los online:

1.  No anfitrião de kit de desenvolvimento do Azure Stack, inicie **Gestor de clusters de ativação pós-falha** do Menu Iniciar.
2.  Selecione o cluster **S Cluster.azurestack.local**.
3.  Selecione **funções**.
4.  VMs de inquilino são apresentados numa *guardado* estado. Assim que estiver a executar todas as VMs de infraestrutura, as VMs do inquilino com o botão direito e selecione **iniciar** para retomar a VM.

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>Eliminou algumas máquinas virtuais, mas ainda verá os arquivos VHD no disco. Este comportamento é esperado?
Sim, este comportamento está previsto. Ele foi projetado desta forma, porque:

* Quando elimina uma VM, os VHDs não são eliminados. Os discos são recursos separados, no grupo de recursos.
* Quando uma conta de armazenamento é excluída, a eliminação está visível imediatamente através do Azure Resource Manager, mas os discos podem conter ainda são mantidos no armazenamento até que a coleta de lixo é executado.

Se vir "orphan" VHDs, é importante saber se elas fazem parte da pasta para uma conta de armazenamento que foi eliminada. Se a conta de armazenamento não foi eliminada, é normal que eles ainda estão ali.

Pode ler mais sobre como configurar a recuperação de limiar e sob demanda de retenção no [gerir contas de armazenamento](azure-stack-manage-storage-accounts.md).

## <a name="storage"></a>Armazenamento
### <a name="storage-reclamation"></a>Recuperação de armazenamento
Pode demorar até 14 horas para a capacidade de recuperada a aparecer no portal. Recuperação de espaço depende de vários fatores, incluindo a percentagem de utilização de ficheiros de contentor interno no arquivo de BLOBs de bloco. Por conseguinte, dependendo da quantidade de dados é eliminado, não é garantido a quantidade de espaço que poderia ser recuperado quando o coletor de lixo é executado.

