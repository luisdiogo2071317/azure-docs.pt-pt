---
title: 'Azure Active Directory Domain Services: Atualizar as definições de DNS para a Azure Virtual Network | Microsoft Docs'
description: Introdução aos Serviços de Domínio do Azure Active Directory
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: d4f3e82c-6807-4690-b298-4eabad2b7927
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: maheshu
ms.openlocfilehash: f683eeee05f264ca239b8f1da2bc5078e0146a17
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39503490"
---
# <a name="enable-azure-active-directory-domain-services"></a>Ativar o Azure Active Directory Domain Services

## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Tarefa 4: Atualizar as definições de DNS para a rede virtual do Azure
Nas tarefas de configuração anteriores, ativou com êxito o Azure Active Directory Domain Services no seu diretório. Em seguida permita que os computadores na rede virtual se liguem e consumam estes serviços. Neste artigo, atualiza as definições do servidor DNS da sua rede virtual para que apontem para os dois endereços IP nos quais o Azure Active Directory Domain Services está disponível na rede virtual.

Para atualizar as definições do servidor DNS para a rede virtual em que tenha ativado o Azure Active Directory Domain Services, faça o seguinte:


1. O separador **Descrição geral** lista um conjunto de **Passos de configuração necessários** para serem executados depois dee o domínio gerido estar totalmente aprovisionado. O primeiro passo de configuração é **Definições do servidor DNS para a sua rede virtual**.

    ![Serviços de Domínio - separador Descrição geral](./media/getting-started/domain-services-provisioned-overview.png)

    > [!TIP]
    > Não vê este passo de configuração? Se as definições do servidor DNS na sua rede virtual estiverem atualizadas, não verá as definições no mosaico "Atualizar definições do servidor DNS na sua rede virtual" no separador Descrição geral.
    >
    >

2. Clique no botão **Configurar** para atualizar as definições do servidor DNS para a rede virtual.

> [!NOTE]
> As máquinas virtuais na rede só obtêm as novas definições de DNS após uma reinicialização. Se precisar de atualizar imediatamente as definições do DNS, ative a reinicialização no portal, no PowerShell ou na CLI.
>
>

## <a name="next-step"></a>Passo seguinte
[Tarefa 5: ativar a sincronização de hash de palavras-passe com o Azure Active Directory Domain Services](active-directory-ds-getting-started-password-sync.md)
