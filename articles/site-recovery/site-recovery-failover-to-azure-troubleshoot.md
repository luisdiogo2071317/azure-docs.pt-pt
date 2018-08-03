---
title: Resolver problemas de ativação pós-falha para falhas do Azure | Documentos da Microsoft
description: Este artigo descreve formas de resolver erros comuns na ativação pós-falha para o Azure
services: site-recovery
documentationcenter: ''
author: ponatara
manager: abhemraj
editor: ''
ms.assetid: ''
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/06/2018
ms.author: ponatara
ms.openlocfilehash: b7b5dcd88b6e4e09dd9beb21e83ef405df148115
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39443389"
---
# <a name="troubleshoot-errors-when-failing-over-a-virtual-machine-to-azure"></a>Resolver erros ao efetuar a ativação pós-falha de uma máquina virtual para o Azure

Poderá receber um dos seguintes erros durante o processo de ativação pós-falha de uma máquina virtual para o Azure. Para resolver problemas, utilize os passos descritos para cada condição de erro.

## <a name="failover-failed-with-error-id-28031"></a>Falha na ativação pós-falha com o ID do erro 28031

Recuperação de sites não foi possível criar um com falha através de máquina virtual no Azure. Isso pode acontecer devido a uma das seguintes razões:

* Não existe uma quota suficiente disponível para criar a máquina virtual: pode verificar a quota disponível ao aceder à subscrição -> utilização e quotas. Pode abrir um [novo pedido de suporte](http://aka.ms/getazuresupport) para aumentar a quota.

* Está a tentar máquinas de virtuais de ativação pós-falha de famílias de tamanhos diferentes no mesmo conjunto de disponibilidade. Certifique-se de que escolha mesma família de tamanho para todas as máquinas virtuais no mesmo conjunto de disponibilidade. Alterar tamanho acedendo a definições de computação e rede da máquina virtual e, em seguida, repita a ativação pós-falha.

* Há uma diretiva na subscrição que impede a criação de uma máquina virtual. Altere a política para permitir a criação de uma máquina virtual e, em seguida, repita a ativação pós-falha.

## <a name="failover-failed-with-error-id-28092"></a>Falha na ativação pós-falha com o ID do erro 28092

Recuperação de sites não foi possível criar uma interface de rede para a máquina virtual com ativação pós-falha. Certifique-se de que tem quota suficiente disponível para criar interfaces de rede na subscrição. Pode verificar a quota disponível ao aceder à subscrição -> utilização e quotas. Pode abrir um [novo pedido de suporte](http://aka.ms/getazuresupport) para aumentar a quota. Se tiver quota suficiente, em seguida, pode ser um intermitente emitir, repita a operação. Se o problema persistir, mesmo após as repetições, em seguida, deixe um comentário no final deste documento.  

## <a name="failover-failed-with-error-id-70038"></a>Falha na ativação pós-falha com o ID do erro 70038

Recuperação de sites não foi possível criar um com falha através de máquinas de virtuais clássicas no Azure. Ele pode ocorrer porque:

* Um dos recursos, como uma rede virtual que é necessário para a máquina virtual a ser criada não existe. Criar a rede virtual, conforme indicado nas definições de computação e rede da máquina virtual ou modificar a definição a uma rede virtual que já existe e, em seguida, repita a ativação pós-falha.

## <a name="unable-to-connectrdpssh-to-the-failed-over-virtual-machine-due-to-grayed-out-connect-button-on-the-virtual-machine"></a>Não é possível ligar/RDP/SSH na máquina virtual devido a ativação pós-falha cinzento botão Conectar na máquina virtual

Se o botão ligar está a cinzento e não estiver ligado ao Azure através de uma Express Route ou VPN de Site a Site ligação, em seguida,

1. Aceda a **Máquina Virtual** > **redes**, clique no nome da interface de rede necessária.  ![interface de rede](media/site-recovery-failover-to-azure-troubleshoot/network-interface.PNG)
1. Navegue para **configurações de Ip**, em seguida, clique no campo de nome de configuração de IP necessárias. ![IPConfigurations](media/site-recovery-failover-to-azure-troubleshoot/IpConfigurations.png)
1. Para ativar o endereço IP público, clique em **ativar**. ![Ativar o IP](media/site-recovery-failover-to-azure-troubleshoot/Enable-Public-IP.png)
1. Clique em **configurar definições necessárias** > **criar novo**. ![Criar um novo](media/site-recovery-failover-to-azure-troubleshoot/Create-New-Public-IP.png)
1. Introduza o nome do endereço público, escolha as opções predefinidas para **SKU** e **atribuição**, em seguida, clique em **OK**.
1. Agora, para guardar as alterações feitas, clique em **guardar**.
1. Feche os painéis e navegue para **descrição geral** secção de máquina virtual para ligar/RDP.

## <a name="unable-to-connectrdpssh-to-the-failed-over-virtual-machine-even-though-connect-button-is-available-not-grayed-out-on-the-virtual-machine"></a>Não é possível ligar/RDP/SSH ao longo do virtual machine, embora Connect botão está disponível (não a cinzento) na máquina virtual

Verifique **diagnósticos de arranque** na sua máquina Virtual e a verificação de erros, conforme listado neste artigo.

1. Se a máquina virtual tiver sido iniciada, experimente fazer failover para um ponto de recuperação mais antigo.
1. Se o aplicativo dentro da máquina virtual não estiver em execução, tente a efetuar ativação pós-falha para um ponto de recuperação consistente com a aplicação.
1. Se a máquina virtual está associado a um domínio, em seguida, certifique-se que esse controlador de domínio está a funcionar com precisão. Isso pode ser feito ao seguir a seguir uma lista de passos.
    a. criar uma nova máquina virtual na mesma rede

    b.  Certifique-se de que é capaz de aderir ao mesmo domínio em que a máquina virtual com ativação pós-falha é esperado que surgem.

    c. Se o controlador de domínio estiver **não** funcionar com precisão, em seguida, tente o registo para a máquina virtual através de uma conta de administrador local com ativação pós-falha
1. Se estiver a utilizar um servidor DNS personalizado, em seguida, certifique-se de que está acessível. Isso pode ser feito ao seguir a seguir uma lista de passos.
    a. Crie uma nova máquina virtual na mesma rede e b. Verifique se a máquina virtual é capaz de resolver nomes com o servidor DNS personalizado

>[!Note]
>Ativação de qualquer definição que não seja o diagnóstico de arranque exigiria que o agente da VM do Azure ser instalado na máquina virtual antes da ativação pós-falha

## <a name="next-steps"></a>Passos Seguintes

Se precisar de mais ajuda, em seguida, publique sua consulta no [fórum de recuperação de Site](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr) ou deixe um comentário no final deste documento. Temos uma Comunidade ativa que deve ser capaz de ajudá-lo.
