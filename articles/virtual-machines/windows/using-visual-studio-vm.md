---
title: "Com o Visual Studio numa máquina Virtual do Azure | Microsoft Docs"
description: "Com o Visual Studio numa máquina Virtual do Azure."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: PhilLee-MSFT
manager: sacalla
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.prod: vs-devops-alm
ms.date: 01/30/2018
ms.author: phillee
keywords: visualstudio
ms.openlocfilehash: 813022f1778e2c7f3174e11192b845c2c33ad219
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a id="top"></a> Imagens do visual Studio no Azure
Com o Visual Studio a executar numa pré-configurada do Azure máquina virtual (VM) é a forma mais fácil e mais rápida fazer a partir nada para um ambiente de desenvolvimento de segurança e executar.  Imagens de sistema com configurações diferentes do Visual Studio estão disponíveis no [Azure Marketplace](https://portal.azure.com/). Apenas uma VM de arranque e desativar a aceda.

Novo no Azure? [Crie uma conta gratuita](https://azure.microsoft.com/free).

## <a name="what-configurations-and-versions-are-available"></a>Que configurações e versões estão disponíveis?
No Azure Marketplace localizar imagens para as versões mais recentes principais: 2017 do Visual Studio e o Visual Studio 2015.  Para cada versão principal, verá originalmente lançada (aka ' RTW') versão e as "mais recente" versões atualizadas.  Para cada uma destas versões diferentes, encontrará as edições do Visual Studio Enterprise e o Visual Studio Community.

|               Versão de lançamento              |          Edições            |    Versão do produto    |
|:------------------------------------------:|:----------------------------:|:---------------------:|
| Visual Studio 2017 - mais recente (versão 15.5) |    Enterprise, da Comunidade     |     Versão 15.5.3    |
|         Visual Studio 2017 - RTW           |    Enterprise, da Comunidade     |     Versão 15.0.7    |
|   Visual Studio 2015 - mais recente (atualização 3)   |    Enterprise, da Comunidade     | Versão 14.0.25431.01 |
|         Visual Studio 2015 - RTW           | Nenhum (expirado para manutenção) |          ---          |

> [!NOTE]
> Em conformidade com a política, inicialmente lançada de manutenção do Microsoft (aka ' RTW') versão do Visual Studio 2015 expirou para a manutenção.  Por conseguinte, o Visual Studio 2015 Update 3 é a única versão restante é fornecida para a linha de produto do Visual Studio 2015.

Para obter mais informações, consulte o [Visual Studio manutenção política](https://www.visualstudio.com/en-us/productinfo/vs-servicing-vs).

## <a name="what-features-are-installed"></a>Quais as funcionalidades que estão instaladas?
Cada imagem contém a funcionalidade recomendada definido para que essa edição do Visual Studio.  Geralmente, a instalação inclui:

* Todas as cargas de trabalho disponíveis, incluindo da carga de trabalho recomendado componentes opcionais
* .NET 4.6.2 e .NET 4.7 SDKs, os pacotes de filtragem e ferramentas de programador
* Visual F#
* Extensão de GitHub para Visual Studio
* LINQ para as ferramentas do SQL Server

Esta é a linha de comandos que utilizamos para instalar o Visual Studio ao criar as imagens:

   * vs_enterprise.exe --allWorkloads --includeRecommended --passive ^
   * Adicionar Microsoft.Net.Component.4.7.SDK ^
   * Adicionar Microsoft.Net.Component.4.7.TargetingPack ^ 
   * Adicionar Microsoft.Net.Component.4.6.2.SDK ^
   * Adicionar Microsoft.Net.Component.4.6.2.TargetingPack ^
   * Adicionar Microsoft.Net.ComponentGroup.4.7.DeveloperTools ^
   * add Microsoft.VisualStudio.Component.FSharp ^
   * add Component.GitHub.VisualStudio ^
   * add Microsoft.VisualStudio.Component.LinqToSql

Se as imagens não incluem uma funcionalidade do Visual Studio que precisa, os seus comentários que através da ferramenta de comentários (canto superior direito da página).

## <a name="what-size-vm-should-i-choose"></a>O tamanho da VM devo escolher?
Aprovisionamento de uma nova máquina virtual é fácil e o Azure oferece uma gama completa de tamanhos de máquina virtual.  Tal como acontece com quaisquer aquisição de hardware, que pretende efetuar o balanceamento de desempenho em comparação com o custo.  Uma vez que o Visual Studio é uma aplicação de elevado desempenho, vários threads, quer um tamanho VM que inclui, pelo menos, dois processadores e 7 GB de memória.  Para obter mais informações sobre os tamanhos de máquina mais recentes, consulte [máquinas de virtuais de tamanhos para Windows no Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes).

Com o Azure, não está bloqueada com a sua primeira escolha – pode rebalancear à sua escolha inicial ao redimensionar a VM.  Ou pode aprovisionar uma nova VM com um tamanho mais adequado, ou pode redimensionar a VM existente para um hardware subjacente diferente.  Para obter mais informações, consulte [redimensionar uma VM do Windows](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/resize-vm).

## <a name="after-i-get-the-vm-running-then-what"></a>Após receber a VM em execução, em seguida, o que?
Visual Studio segue o modelo de "traga a própria licença" de mensagens em fila no Azure.  Por isso, da mesma forma para uma instalação de hardware proprietária, um dos passos primeiro é licenciar a instalação do Visual Studio.  Pode desbloquear o Visual Studio, o início de sessão com uma conta Microsoft que está associada a uma subscrição do Visual Studio ou pode desbloquear o Visual Studio com a chave de produto com a compra inicial.  Para obter mais informações, consulte [iniciar sessão no Visual Studio](https://docs.microsoft.com/en-us/visualstudio/ide/signing-in-to-visual-studio) e [como desbloquear o Visual Studio](https://docs.microsoft.com/en-us/visualstudio/ide/how-to-unlock-visual-studio).

## <a name="after-i-build-out-the-dev-vm-how-do-i-save-capture-it-for-future-or-team-use"></a>Depois de compilação terminar o desenvolvimento VM, como posso guardar (captura)-o para o futuro ou equipa utilização?

Espetro dos ambientes de desenvolvimento é grande e existe real custos associados a conceção de ambientes de mais complexas.  No entanto, independentemente da configuração do seu ambiente, Azure faz com que preserva esse investimento fácil capturando guardar/VM perfeitamente configurado como uma 'imagem base' para utilização futura – para si e/ou para outros membros da sua equipa.  Em seguida, quando se reinicia uma nova VM, Aprovisione da imagem base em vez da imagem do Marketplace.

Como um resumo rápido, terá de sysprep e encerramento de VM em execução, em seguida, *captura (figura 1)* VM como uma imagem através da IU do portal do Azure.  Azure irá guardar o `.vhd` ficheiro que contém a imagem na conta de armazenamento à sua escolha.  Em seguida, a nova imagem aparece como um recurso de imagem na lista da sua subscrição de recursos.

<img src="media/using-visual-studio-vm/capture-vm.png" alt="Capture an image through the Azure portal’s UI" style="border:3px solid Silver; display: block; margin: auto;"><center>*(A figura 1) Capture uma imagem através da IU do portal do Azure.*</center>

Para obter mais informações, consulte [capturar uma VM a uma imagem](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/capture-image-resource).

  **O lembrete:** não se esqueça de sysprep a VM!  Se perder esse passo, o Azure não é possível aprovisionar uma VM a partir da imagem.

> [!NOTE]
> Pode ainda implicar alguns custos para armazenamento das imagens, mas esse custo incremental é provável que insignificant em comparação com os custos de manpower reconstruir VM a partir do zero – para cada pessoa na sua equipa que necessita de uma VM.  Por exemplo,-custos alguns utilizados no compromisso para criar e armazenar uma imagem de 127 GB durante um mês que é reutilizável por todos os membros da sua equipa.  No entanto, estes custos são insignificant em comparação com horas que cada empregado invests a criação de saída e validar uma caixa de desenvolvimento está corretamente configurada para utilização individuais.

Além disso, as tarefas de desenvolvimento ou tecnologias poderão ter escala mais – como varieties de configurações de desenvolvimento e configurações com várias máquina.  Pode utilizar o Azure DevTest Labs para criar _receitas_ que automatizar a construção da "imagem dourada' e gerir políticas para a sua equipa de VMs em execução.  [Utilizar o Azure DevTest Labs para programadores](https://docs.microsoft.com/en-us/azure/devtest-lab/devtest-lab-developer-lab) é a origem para obter mais informações sobre DevTest Labs melhor.

## <a name="next-steps"></a>Passos Seguintes
Agora que sabe mais sobre as imagens de Visual Studio previamente configuradas, o passo seguinte consiste em criar uma nova VM:

* [Criar uma VM através do portal do Azure](quick-create-portal.md)
* [Descrição geral de máquinas virtuais do Windows](overview.md)
