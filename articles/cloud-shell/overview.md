---
title: "Descrição geral de Shell de nuvem do Azure | Microsoft Docs"
description: "Descrição geral da Shell em nuvem do Azure."
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: juluk
ms.openlocfilehash: b710c324f72fa56a2ebad0d1b35052639611d30d
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="overview-of-azure-cloud-shell"></a>Descrição geral da Shell de nuvem do Azure
Shell de nuvem do Azure é uma shell interativa e acessíveis para o browser para a gestão de recursos do Azure.
Fornece a flexibilidade de escolher a experiência de shell que melhor se adapta da forma que trabalha.
Os utilizadores de Linux podem optar por uma experiência de Bash, ao passo que os utilizadores de Windows podem escolher o PowerShell.

Tente do shell.azure.com utilizando este botão.

[![](https://shell.azure.com/images/launchcloudshell.png "Inicie o Shell de nuvem do Azure")](https://shell.azure.com)

Tente do portal do Azure utilizando o ícone de Shell de nuvem.

![Início do portal](media/overview/portal-launch-icon.png)

## <a name="features"></a>Funcionalidades
### <a name="browser-based-shell-experience"></a>Experiência de shell baseada no browser
Shell de nuvem permite o acesso a uma experiência da linha de comandos baseada no browser criada com as tarefas de gestão do Azure em mente.
Pode fornecer a Shell de nuvem tire partido untethered a partir de uma máquina local de trabalho de forma apenas a nuvem.

### <a name="choice-of-preferred-shell-experience"></a>Opção de experiência de shell preferencial
Os utilizadores do Linux podem utilizar Bash na Shell de nuvem, enquanto utilizadores do Windows podem utilizar o PowerShell na Shell de nuvem (pré-visualização) na lista pendente shell.

![Bash na Shell de nuvem](media/overview/overview-bash-pic.png)

![PowerShell na Shell de nuvem (pré-visualização)](media/overview/overview-ps-pic.png)

### <a name="authenticated-and-configured-azure-workstation"></a>Autenticado e configurada estação de trabalho do Azure
Shell de nuvem é gerida pela Microsoft, de modo que vem com suporte de idiomas e ferramentas de linha de comandos populares. Shell de nuvem segura também efetua a autenticação automaticamente para instantânea acesso aos recursos através dos cmdlets do Azure CLI 2.0 ou o Azure PowerShell.

Ver a lista completa de ferramentas para o [Bash experiência](features.md#tools) e [experiência com o PowerShell (pré-visualização).](features-powershell.md#tools)

### <a name="multiple-access-points"></a>Vários pontos de acesso
Shell de nuvem é uma ferramenta flexível que pode ser utilizada a partir de:
* [portal.azure.com](https://portal.azure.com)
* [shell.azure.com](https://shell.azure.com)
* [Documentação de "Experimente" 2.0 da CLI do Azure](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest)
* [Aplicação móvel do Azure](https://azure.microsoft.com/features/azure-portal/mobile-app/)
* [Extensão de conta do Azure de código de VS](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)

### <a name="connect-your-microsoft-azure-files-storage"></a>Ligar o armazenamento de ficheiros do Microsoft Azure
Nuvem Shell máquinas são temporárias e necessitam de uma partilha de ficheiros do Azure para possível montá-la como `clouddrive` para manter os ficheiros.

Na primeira execução que nuvem Shell pede-lhe criar um recurso de grupo, a conta de armazenamento e ficheiros do Azure partilham em seu nome. Este é um passo única e será anexado automaticamente todas as sessões. Uma partilha de ficheiros única pode ser mapeada e será utilizada pelo Bash e do PowerShell na Shell de nuvem (pré-visualização).

#### <a name="create-new-storage"></a>Criar o novo armazenamento
![](media/overview/basic-storage.png)

Uma conta de armazenamento localmente redundante (LRS) e a partilha de ficheiros do Azure podem ser criados em seu nome. Se optar por utilizar ambas a partilha de ficheiros do Azure será utilizada para ambientes de Bash e PowerShell. Aplicam os custos de armazenamento normal.

Três recursos serão criados em seu nome:
1. Grupo de recursos com o nome:`cloud-shell-storage-<region>`
2. Conta de armazenamento com o nome:`cs<uniqueGuid>`
3. Partilha de ficheiros com o nome:`cs-<user>-<domain>-com-<uniqueGuid>`

> [!Note]
> Bash na Shell de nuvem também cria uma imagem de disco de 5 GB predefinido para manter `$Home`. Todos os ficheiros no seu diretório $Home tal como chaves SSH são mantidos da imagem de disco do utilizador armazenada numa partilha de ficheiros do Azure montados de. Aplique as melhores práticas ao guardar os ficheiros no diretório $Home e partilha de ficheiros do Azure montada.

#### <a name="use-existing-resources"></a>Utilizar recursos existentes
![](media/overview/advanced-storage.png)

É fornecida uma opção avançada para associar os recursos existentes para a Shell de nuvem.
Na linha de comandos do programa de configuração da armazenamento, clique em "Definições avançadas de mostrar" para mostrar as opções adicionais.

> [!Note]
> As DropDowns são filtradas para a sua região de nuvem Shell previamente atribuído e contas de armazenamento LRS/GRS.

[Saiba mais sobre armazenamento de nuvem Shell, ao atualizar as partilhas de ficheiros do Azure e a carregar/transferência de ficheiros.](persisting-shell-storage.md)

## <a name="concepts"></a>Conceitos
* Nuvem Shell é executada num anfitrião temporário fornecido por-sessão, por utilizador
* Shell de nuvem exceder o tempo limite após 20 minutos sem atividade interativa
* Shell de nuvem requer uma partilha de ficheiros do Azure para ser montados
* Shell de nuvem utiliza a mesma partilha de ficheiros do Azure para Bash e PowerShell
* Shell de nuvem é atribuído uma máquina por conta de utilizador
* Bash persistir $Home utilizando uma imagem de 5 GB contida na partilha de ficheiros
* As permissões estão definidas como um utilizador de Linux normal no Bash

Saiba mais sobre as funcionalidades no [Bash na Shell de nuvem](features.md) e [PowerShell na Shell de nuvem (pré-visualização)](features-powershell.md).

## <a name="pricing"></a>Preços
O computador que aloja a Shell de nuvem é gratuito, com um pré-requisito de uma partilha de ficheiros do Azure montada. Aplicam os custos de armazenamento normal.

## <a name="next-steps"></a>Passos Seguintes
[Bash no início rápido da Shell de nuvem](quickstart.md) <br>
[PowerShell no início rápido da Shell de nuvem (pré-visualização)](quickstart-powershell.md)