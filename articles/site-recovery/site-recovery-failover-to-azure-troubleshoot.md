---
title: Resolver problemas de ativação pós-falha para falhas do Azure | Microsoft Docs
description: Este artigo descreve formas de resolver erros comuns no falha através do Azure
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
ms.date: 03/09/2018
ms.author: ponatara
ms.openlocfilehash: 838eac510fc17d56f808f541f4e205a279f63c56
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36318896"
---
# <a name="troubleshoot-errors-when-failing-over-a-virtual-machine-to-azure"></a>Resolver erros ao efetuar a ativação pós-falha de uma máquina virtual para o Azure

Poderá receber um dos seguintes erros ao efetuar ativação pós-falha de uma máquina virtual para o Azure. Para resolver problemas, utilize os passos descritos para cada condição de erro.

## <a name="failover-failed-with-error-id-28031"></a>Falha na ativação pós-falha com o ID de erro 28031

Recuperação de site não conseguiu criar uma máquina virtual no Azure de ativação pós-falha. Pode ocorrer devido a um dos seguintes motivos:

* Não é uma quota suficiente disponível para criar a máquina virtual: pode verificar a quota disponível acedendo a subscrição -> utilização + quotas. Pode abrir um [novo pedido de suporte](http://aka.ms/getazuresupport) para aumentar a quota.

* Está a tentar máquinas de virtuais de ativação pós-falha das famílias de tamanho diferente no mesmo conjunto de disponibilidade. Certifique-se de que escolha a mesma família de tamanho para todas as máquinas virtuais no mesmo conjunto de disponibilidade. Alterar tamanho acedendo às definições de computação e rede da máquina virtual e, em seguida, repita a ativação pós-falha.

* Há uma política na subscrição que impede a criação de uma máquina virtual. Altere a política para permitir a criação de uma máquina virtual e, em seguida, repita a ativação pós-falha.

## <a name="failover-failed-with-error-id-28092"></a>Falha na ativação pós-falha com o ID de erro 28092

Recuperação de site não conseguiu criar uma interface de rede para a ativação pós-falha da máquina virtual. Certifique-se de que tem quota suficiente disponível para criar as interfaces de rede na subscrição. Pode verificar a quota disponível acedendo a subscrição -> utilização + quotas. Pode abrir um [novo pedido de suporte](http://aka.ms/getazuresupport) para aumentar a quota. Se tiver uma quota suficiente, em seguida, esta situação pode ter um intermitente emitir, repita a operação. Se o problema persistir, mesmo após várias tentativas, em seguida, deixe um comentário no final deste documento.  

## <a name="failover-failed-with-error-id-70038"></a>Falha na ativação pós-falha com o ID de erro 70038

Recuperação de site não conseguiu criar uma máquina de virtual clássico no Azure de ativação pós-falha. Pode ocorrer porque:

* Um dos recursos, tais como uma rede virtual que é necessário para a máquina virtual a ser criado não existe. Criar a rede virtual como fornecido com as definições de computação e rede da máquina virtual ou modifique a definição a uma rede virtual que já existe e, em seguida, repita a ativação pós-falha.

## <a name="unable-to-connectrdpssh-to-the-failed-over-virtual-machine-due-to-grayed-out-connect-button-on-the-virtual-machine"></a>Não é possível ligar/RDP/SSH para a máquina virtual devido a ativação pós-falha a cinzento botão de ligar a máquina virtual

Se ligar botão fica a cinzento e não estiver ligado ao Azure através de uma Express Route ou VPN de Site para Site ligação, em seguida,

1. Aceda a **Máquina Virtual** > **redes**, clique no nome da interface de rede necessários.  ![interface de rede](media/site-recovery-failover-to-azure-troubleshoot/network-interface.PNG)
2. Navegue para **configurações de Ip**, em seguida, clique no campo nome do necessária da configuração de IP. ![IPConfigurations](media/site-recovery-failover-to-azure-troubleshoot/IpConfigurations.png)
3. Para ativar o endereço IP público, clique em **ativar**. ![Ativar o IP](media/site-recovery-failover-to-azure-troubleshoot/Enable-Public-IP.png)
4. Clique em **configurar definições necessárias** > **criar nova**. ![Criar um novo](media/site-recovery-failover-to-azure-troubleshoot/Create-New-Public-IP.png)
5. Introduza o nome do endereço público, escolha as opções predefinidas para **SKU** e **atribuição**, em seguida, clique em **OK**.
6. Agora, para guardar as alterações feitas, clique em **guardar**.
7. Fechar os painéis e navegue para **descrição geral** secção de máquina virtual para ligar/RDP.

## <a name="unable-to-connectrdpssh-to-the-failed-over-virtual-machine-even-though-connect-button-is-available-not-grayed-out-on-the-virtual-machine"></a>Não é possível ligar/RDP/SSH para a falha ao longo do virtual machine embora Connect botão está disponível (não a cinzento) na máquina virtual

Verifique **diagnóstico de arranque** na sua máquina Virtual e verifique a existência de erros, conforme indicado neste artigo.

1. Se a máquina virtual não foi iniciado, tente efetuar a ativação pós-falha para um ponto de recuperação mais antigo.
2. Se a aplicação dentro da máquina virtual não está a funcionar, tente falhar através de um ponto de recuperação consistentes da aplicação.
3. Se a máquina virtual está associado a um domínio, em seguida, certifique-se que esse controlador de domínio está a funcionar com exatidão. Isto pode ser feito ao seguir o abaixo indicado passos.
    a. criar uma nova máquina virtual na mesma rede

    b.  Certifique-se de que é capaz de aderir ao mesmo domínio em que a ativação pós-falha da máquina virtual deverá detetar um.

    c. Se o controlador de domínio é **não** funcionar com precisão, em seguida, tente o registo para a ativação pós-falha da máquina virtual utilizando uma conta de administrador local
4. Se estiver a utilizar um servidor DNS personalizado, em seguida, certifique-se de que está acessível. Isto pode ser feito ao seguir o abaixo indicado passos.
    a. Crie uma nova máquina virtual na mesma rede e b. Verifique se a máquina virtual é capaz de a utilizar o servidor DNS personalizado de resolução de nomes

>[!Note]
>Ativar qualquer definição que não seja o diagnóstico de arranque precisaria de agente da VM do Azure ser instalada na máquina virtual antes da ativação pós-falha

## <a name="next-steps"></a>Passos Seguintes

Se precisar de mais ajuda, em seguida, publique a consulta no [fórum de recuperação de Site](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr) ou deixe um comentário no final deste documento. Temos uma Comunidade de Active Directory que deve ser capaz de ajudá-lo.
