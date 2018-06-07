---
title: Extensões de máquina virtual do Azure e funcionalidades | Microsoft Docs
description: Saiba que anre de extensões de VM do Azure como utilizá-las com máquinas virtuais do Azure
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/30/2018
ms.author: danis
ms.openlocfilehash: 04f6d68feccf4a9b2bf2fa0f03ad8bd978cf5f17
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34653268"
---
# <a name="azure-virtual-machine-extensions-and-features"></a>Extensões de máquina virtual do Azure e funcionalidades
Extensões de máquina virtual do Azure (VM) são aplicações de pequenas a indicar tarefas de configuração e a automatização de pós-implementação em VMs do Azure, pode utilizar imagens existentes e, em seguida, personalize-os como parte das implementações, obter, fora da empresa de personalizado criação da imagem.

A plataforma do Azure aloja várias extensões que variam entre a configuração de VM, monitorização, segurança e aplicações de utilitário. Os publicadores colocar uma aplicação, em seguida, moldam-la para uma extensão e simplificam a instalação, pelo que tudo o que precisa de fazer é fornecer os parâmetros obrigatórios. 

 Há uma grande escolha de primeiro e extensões de terceiros, se a aplicação no repositório de extensão não existe, em seguida, pode utilizar a extensão de Script personalizado e configurar a VM com os seus próprios comandos e scripts.

Exemplos de cenários-chave que extensões são utilizadas para:
* Configuração de VM, pode utilizar (configuração de estado pretendido) do Powershell DSC, Chef, Puppet e extensões de Script personalizado para instalar agentes de configuração de VM e configurar a VM. 
* Produtos de AV, como da Symantec, ESET.
* Ferramenta de vulnerabilidade VM, tais como Qualys, Rapid7, HPE.
* VM e ferramentas, como DynaTrace, observador de rede do Azure, Site24x7 e Stackify de monitorização de aplicações.

As extensões podem ser incluídas com uma nova implementação de VM. Por exemplo, pode fazer parte de uma maior implementação, a configuração de aplicações no aprovisionamento de VM, ou executar qualquer implementação de post de sistemas de extensão suportados operado.

## <a name="how-can-i-find-what-extensions-are-available"></a>Como posso determinar que extensões estão disponíveis?
Pode ver as extensões disponíveis no painel no Portal, em extensões de VM, isto representa apenas uma pequena quantidade, para obter uma lista completa, pode utilizar as ferramentas da CLI, consulte [detetar extensões de VM do Linux](features-linux.md) e [ Detetar as extensões de VM para o Windows](features-windows.md).

## <a name="how-can-i-install-an-extension"></a>Como instalar uma extensão?
Extensões VM do Azure podem ser geridas utilizando o 2.0 CLI do Azure, Azure PowerShell, os modelos Azure Resource Manager e o portal do Azure. Para experimentar uma extensão, pode aceda ao portal do Azure, selecione a extensão de Script personalizado, em seguida, transmita um comando / script e executar as extensões.

Se pretender que a mesma extensão que adicionou no portal pelo modelo CLI ou o Gestor de recursos, consulte a documentação de outra extensão, tal como [extensão de Script do Windows personalizado](custom-script-windows.md) e [extensão de Script personalizado Linux](custom-script-linux.md).

## <a name="how-do-i-manage-extension-application-lifecycle"></a>Como gerir o ciclo de vida de aplicação de extensão
Não é necessário ligar a uma VM diretamente para instalar ou eliminar a extensão. Como o ciclo de vida de aplicação de extensão do Azure é gerido fora da VM e integrado de plataforma do Azure, também obter integrado estado da extensão.

## <a name="anything-else-i-should-be-thinking-about-for-extensions"></a>Tudo o resto posso deve ser pensar sobre para as extensões?
Extensões instalar aplicações, como a quaisquer aplicações, existem alguns requisitos, para as extensões de existir se uma lista de sos de Linux e Windows suportados e terá de instalar os agentes de VM do Azure. Algumas aplicações de extensão VM individuais podem ter os seus próprios pré-requisitos ambientais, como o acesso a um ponto final.

## <a name="next-steps"></a>Passos Seguintes
* Para obter mais informações sobre como funcionam as extensões e agente Linux, consulte [extensões de VM do Azure e funcionalidades para Linux](features-linux.md).
* Para obter mais informações sobre como funcionam as extensões e o agente de convidados do Windows, consulte [extensões de VM do Azure e funcionalidades para o Windows](features-windows.md).  
* Para instalar o agente convidado do Windows, consulte [descrição geral de agente do Azure Windows Máquina Virtual ](agent-windows.md).  
* Para instalar o agente do Linux, consulte [descrição geral do agente de Máquina Virtual de Linux do Azure ](agent-linux.md).  

