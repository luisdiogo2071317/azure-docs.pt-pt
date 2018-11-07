---
title: Resolver problemas relacionados com as ativações pós-falha para o Azure | Documentos da Microsoft
description: Este artigo descreve como resolver problemas comuns durante a ativação pós-falha para o Azure com o Azure Site Recovery.
author: ponatara
manager: abhemraj
ms.service: site-recovery
ms.topic: article
ms.date: 09/11/2018
ms.author: ponatara
ms.openlocfilehash: 420d061b34734c7b5997f5cdd58fe7faaee9cb82
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51236761"
---
# <a name="troubleshoot-errors-when-failing-over-a-virtual-machine-to-azure"></a>Resolver erros ao efetuar a ativação pós-falha de uma máquina virtual para o Azure

Poderá receber um dos seguintes erros durante o processo de ativação pós-falha de uma máquina virtual para o Azure. Para resolver problemas, utilize os passos descritos para cada condição de erro.

## <a name="failover-failed-with-error-id-28031"></a>Falha na ativação pós-falha com o ID do erro 28031

Recuperação de sites não foi possível criar um com falha através de máquina virtual no Azure. Isso pode acontecer devido a uma das seguintes razões:

* Não existe uma quota suficiente disponível para criar a máquina virtual: pode verificar a quota disponível ao aceder à subscrição -> utilização e quotas. Pode abrir um [novo pedido de suporte](https://aka.ms/getazuresupport) para aumentar a quota.

* Está a tentar máquinas de virtuais de ativação pós-falha de famílias de tamanhos diferentes no mesmo conjunto de disponibilidade. Certifique-se de que escolha mesma família de tamanho para todas as máquinas virtuais no mesmo conjunto de disponibilidade. Alterar tamanho acedendo a definições de computação e rede da máquina virtual e, em seguida, repita a ativação pós-falha.

* Há uma diretiva na subscrição que impede a criação de uma máquina virtual. Altere a política para permitir a criação de uma máquina virtual e, em seguida, repita a ativação pós-falha.

## <a name="failover-failed-with-error-id-28092"></a>Falha na ativação pós-falha com o ID do erro 28092

Recuperação de sites não foi possível criar uma interface de rede para a máquina virtual com ativação pós-falha. Certifique-se de que tem quota suficiente disponível para criar interfaces de rede na subscrição. Pode verificar a quota disponível ao aceder à subscrição -> utilização e quotas. Pode abrir um [novo pedido de suporte](https://aka.ms/getazuresupport) para aumentar a quota. Se tiver quota suficiente, em seguida, pode ser um intermitente emitir, repita a operação. Se o problema persistir, mesmo após as repetições, em seguida, deixe um comentário no final deste documento.  

## <a name="failover-failed-with-error-id-70038"></a>Falha na ativação pós-falha com o ID do erro 70038

Recuperação de sites não foi possível criar um com falha através de máquinas de virtuais clássicas no Azure. Ele pode ocorrer porque:

* Um dos recursos, como uma rede virtual que é necessário para a máquina virtual a ser criada não existe. Criar a rede virtual, conforme indicado nas definições de computação e rede da máquina virtual ou modificar a definição a uma rede virtual que já existe e, em seguida, repita a ativação pós-falha.

## <a name="unable-to-connectrdpssh---vm-connect-button-grayed-out"></a>Não é possível ligar/RDP/SSH - VM ligar botão a cinzento

Se o **Connect** botão na ativação pós-falha VM no Azure está a cinzento e não estiver ligado ao Azure através de uma Express Route ou VPN de Site a Site ligação, em seguida,

1. Aceda a **Máquina Virtual** > **redes**, clique no nome da interface de rede necessária.  ![interface de rede](media/site-recovery-failover-to-azure-troubleshoot/network-interface.PNG)
2. Navegue para **configurações de Ip**, em seguida, clique no campo de nome de configuração de IP necessárias. ![IPConfigurations](media/site-recovery-failover-to-azure-troubleshoot/IpConfigurations.png)
3. Para ativar o endereço IP público, clique em **ativar**. ![Ativar o IP](media/site-recovery-failover-to-azure-troubleshoot/Enable-Public-IP.png)
4. Clique em **configurar definições necessárias** > **criar novo**. ![Criar um novo](media/site-recovery-failover-to-azure-troubleshoot/Create-New-Public-IP.png)
5. Introduza o nome do endereço público, escolha as opções predefinidas para **SKU** e **atribuição**, em seguida, clique em **OK**.
6. Agora, para guardar as alterações feitas, clique em **guardar**.
7. Feche os painéis e navegue para **descrição geral** secção de máquina virtual para ligar/RDP.

## <a name="unable-to-connectrdpssh---vm-connect-button-available"></a>Não é possível ligar/RDP/SSH - botão ligação da VM disponível

Se o **Connect** botão na ativação pós-falha VM no Azure está disponível (não a cinzento), em seguida, verificar **diagnósticos de arranque** na sua máquina Virtual e a verificação de erros, conforme listado na [neste artigo](../virtual-machines/windows/boot-diagnostics.md).

1. Se a máquina virtual tiver sido iniciada, experimente fazer failover para um ponto de recuperação mais antigo.
2. Se o aplicativo dentro da máquina virtual não estiver em execução, tente a efetuar ativação pós-falha para um ponto de recuperação consistente com a aplicação.
3. Se a máquina virtual está associado a um domínio, em seguida, certifique-se que esse controlador de domínio está a funcionar com precisão. Isso pode ser feito ao seguir a seguir uma lista de passos:

    a. Crie uma nova máquina virtual na mesma rede.

    b.  Certifique-se de que é capaz de aderir ao mesmo domínio em que a máquina virtual com ativação pós-falha é esperado que surgem.

    c. Se o controlador de domínio estiver **não** funcionar com precisão, em seguida, tente o registo para a máquina virtual através de uma conta de administrador local com ativação pós-falha.
4. Se estiver a utilizar um servidor DNS personalizado, em seguida, certifique-se de que está acessível. Isso pode ser feito ao seguir a seguir uma lista de passos:

    a. Criar uma nova máquina virtual na mesma rede e

    b. Verifique se a máquina virtual é capaz de resolver nomes com o servidor DNS personalizado

>[!Note]
>Ativação de qualquer definição que não seja o diagnóstico de arranque exigiria que o agente da VM do Azure ser instalado na máquina virtual antes da ativação pós-falha

## <a name="unexpected-shutdown-message-event-id-6008"></a>Mensagem de encerramento inesperado (6008 de ID de evento)

Quando a arrancar, uma VM do Windows após a ativação pós-falha, se receber uma mensagem de encerramento inesperado na VM recuperada, ele indica que um Estado de encerramento VM não foi capturado no ponto de recuperação utilizado para a ativação pós-falha. Isto acontece quando recuperar para um ponto quando a VM tinha não foi totalmente encerrada.

Isso normalmente não é preocupante e geralmente pode ser ignorado para ativações pós-falha não planeadas. No caso de uma ativação pós-falha, certifique-se de que a VM está corretamente encerrada antes da ativação pós-falha e fornecer tempo suficiente para pendentes replicação dados no local a serem enviados para o Azure. Em seguida, utilize o **mais recente** opção a [ecrã de ativação pós-falha](site-recovery-failover.md#run-a-failover) para que todos os dados pendentes no Azure são processados num ponto de recuperação, que, em seguida, é utilizado para a ativação pós-falha da VM.

## <a name="retaining-drive-letter-after-failover"></a>Mantendo a letra de unidade após a ativação pós-falha
Para manter a letra de unidade em máquinas virtuais após ativação pós-falha, pode definir o **política de SAN** para a máquina virtual no local para **OnlineAll**. [Leia mais](https://support.microsoft.com/help/3031135/how-to-preserve-the-drive-letter-for-protected-virtual-machines-that-are-failed-over-or-migrated-to-azure).

## <a name="next-steps"></a>Passos Seguintes
- Resolver problemas de [ligação de RDP à VM do Windows](../virtual-machines/windows/troubleshoot-rdp-connection.md)
- Resolver problemas de [ligação SSH à VM do Linux](../virtual-machines/linux/detailed-troubleshoot-ssh-connection.md)

Se precisar de mais ajuda, em seguida, publique sua consulta no [fórum de recuperação de Site](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr) ou deixe um comentário no final deste documento. Temos uma Comunidade ativa que deve ser capaz de ajudá-lo.
