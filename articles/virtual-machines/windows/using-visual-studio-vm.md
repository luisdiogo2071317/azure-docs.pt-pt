---
title: "Com o Visual Studio numa máquina virtual do Azure | Microsoft Docs"
description: "Com o Visual Studio numa máquina virtual do Azure."
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
ms.date: 03/02/2018
ms.author: phillee
keywords: visualstudio
ms.openlocfilehash: e5b289b2be982653461c4f5b933e1ec444fa9716
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="visual-studio-images-on-azure"></a>Visual Studio as imagens no Azure
Com o Visual Studio numa pré-configurada do Azure máquina virtual (VM) é uma forma rápida e fácil fazer a partir nada para um ambiente de desenvolvimento de segurança e executar. Imagens de sistema com configurações diferentes do Visual Studio estão disponíveis no [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps?search=%22visual%20studio%202017%22&page=1).

Novo no Azure? [Crie uma conta gratuita](https://azure.microsoft.com/free).

## <a name="what-configurations-and-versions-are-available"></a>Que configurações e versões estão disponíveis?
As imagens para as versões mais recentes principais para Visual Studio 2017 e Visual Studio 2015, podem ser encontradas no Azure Marketplace. Para cada versão principal, consulte a versão de (RTW) originalmente lançamento e as versões atualizadas mais recente. Cada um destas versões oferece o Visual Studio Enterprise e as edições do Visual Studio Community. Estas imagens são atualizadas, pelo menos, todos os meses para incluir as atualizações mais recentes do Visual Studio e o Windows. Enquanto os nomes das imagens permanecerem o mesmo, a descrição de cada imagem inclui a versão de produto instalado e "a partir de" data da imagem.

| Versão de lançamento              | Edições            | Versão do produto     |
|:-----------------------------------------:|:----------------------------:|:-----------------------:|
| Visual Studio 2017: Mais recente (versão 15.5) |    Enterprise, da Comunidade     |      Versão 15.6.0     |
|         Visual Studio 2017: RTW           |    Enterprise, da Comunidade     |      Versão 15.0.10    |
|   Visual Studio 2015: Mais recente (atualização 3)   |    Enterprise, da Comunidade     |  Versão 14.0.25431.01  |
|         Visual Studio 2015: RTW           |              Nenhum            | (Expirou para a manutenção) |

> [!NOTE]
> Em conformidade com a Microsoft a política de manutenção, a versão de (RTW) originalmente lançamento do Visual Studio 2015 expirou para a manutenção. Visual Studio 2015 Update 3 é a única versão restante é fornecida para a linha de produto do Visual Studio 2015.

Para obter mais informações, consulte o [Visual Studio manutenção política](https://www.visualstudio.com/productinfo/vs-servicing-vs).

## <a name="what-features-are-installed"></a>Quais as funcionalidades que estão instaladas?
Cada imagem contém a funcionalidade recomendada definido para que essa edição do Visual Studio. Geralmente, a instalação inclui:

* Todas as cargas de trabalho disponíveis, incluindo cada carga de trabalho recomendado componentes opcionais
* .NET 4.6.2 e .NET 4.7 SDKs, os pacotes de filtragem e ferramentas de programador
* Visual F#
* Extensão de GitHub para Visual Studio
* LINQ para as ferramentas do SQL Server

A linha de comandos utilizada para instalar o Visual Studio quando criar as imagens da seguinte forma:

```
    vs_enterprise.exe --allWorkloads --includeRecommended --passive ^
       add Microsoft.Net.Component.4.7.SDK ^
       add Microsoft.Net.Component.4.7.TargetingPack ^ 
       add Microsoft.Net.Component.4.6.2.SDK ^
       add Microsoft.Net.Component.4.6.2.TargetingPack ^
       add Microsoft.Net.ComponentGroup.4.7.DeveloperTools ^
       add Microsoft.VisualStudio.Component.FSharp ^
       add Component.GitHub.VisualStudio ^
       add Microsoft.VisualStudio.Component.LinqToSql
```

Se as imagens não incluem uma funcionalidade do Visual Studio que precisa, os seus comentários através da ferramenta de comentários no canto superior direito da página.

## <a name="what-size-vm-should-i-choose"></a>O tamanho da VM devo escolher?
O Azure oferece uma gama completa de tamanhos de máquina virtual. Porque o Visual Studio é uma aplicação de elevado desempenho, vários threads, quer um tamanho VM que inclui, pelo menos, dois processadores e 7 GB de memória. Recomendamos os seguintes tamanhos de VM para as imagens do Visual Studio:

   * Standard_D2_v3
   * Standard_D2s_v3
   * Standard_D4_v3
   * Standard_D4s_v3
   * Standard_D2_v2
   * Standard_D2S_v2
   * Standard_D3_v2
    
Para obter mais informações sobre os tamanhos de máquina mais recentes, consulte [máquinas de virtuais de tamanhos para Windows no Azure](/azure/virtual-machines/windows/sizes).

Com o Azure, pode rebalancear à sua escolha inicial ao redimensionar a VM. Pode aprovisionar uma nova VM com um tamanho mais adequado ou redimensionar a VM existente para um hardware subjacente diferente. Para obter mais informações, consulte [redimensionar uma VM do Windows](/azure/virtual-machines/windows/resize-vm).

## <a name="after-the-vm-is-running-whats-next"></a>Depois da VM está em execução, o que é o seguinte?
Visual Studio segue o modelo de "traga a sua própria licença" de mensagens em fila no Azure. Tal como acontece com uma instalação de hardware proprietária, um dos passos primeiro é licenciar a instalação do Visual Studio. Para desbloquear o Visual Studio, está:
- Inicie sessão com uma conta Microsoft que está associada a uma subscrição do Visual Studio 
- Desbloquear o Visual Studio com a chave de produto fornecida na compra inicial

Para obter mais informações, consulte [inicie sessão no Visual Studio](/visualstudio/ide/signing-in-to-visual-studio) e [como desbloquear o Visual Studio](/visualstudio/ide/how-to-unlock-visual-studio).

## <a name="how-do-i-save-the-development-vm-for-future-or-team-use"></a>Como posso guardar o desenvolvimento de VM para o futuro ou equipa utiliza?

Espetro dos ambientes de desenvolvimento é grande e existe real custos associados a conceção de ambientes de mais complexas. Independentemente da configuração do seu ambiente, pode guardar ou capturar, a VM configurada como uma "imagem base" para utilização futura ou para outros membros da sua equipa. Em seguida, quando se reinicia uma nova VM, pode Aprovisiona da imagem de base, em vez da imagem do Azure Marketplace.

Um resumo rápido: utilizar a ferramenta de preparação do sistema (Sysprep) e encerre a VM em execução e, em seguida, capturar *(figura 1)* VM como uma imagem através da IU no portal do Azure. Azure guarda o `.vhd` ficheiro que contém a imagem na conta de armazenamento à sua escolha. A nova imagem, em seguida, apresentado como um recurso de imagem na lista da sua subscrição de recursos.

<img src="media/using-visual-studio-vm/capture-vm.png" alt="Capture an image through the Azure portal UI" style="border:3px solid Silver; display: block; margin: auto;"><center>*(A figura 1) Capture uma imagem através da IU do portal do Azure.*</center>

Para obter mais informações, consulte [criar uma imagem gerida de uma VM generalizada no Azure](/azure/virtual-machines/windows/capture-image-resource).

> [!IMPORTANT]
> Não se esqueça de utilizar o Sysprep para preparar a VM. Se perder esse passo, o Azure não é possível aprovisionar uma VM a partir da imagem.

> [!NOTE]
> Pode ainda implicar alguns custos para armazenamento das imagens, mas que custo incremental pode ser insignificant em comparação com os custos gerais para reconstruir a VM a partir do zero para cada membro de equipa que necessitam de um. Por exemplo,-custos alguns utilizados no compromisso para criar e armazenar uma imagem de 127 GB durante um mês que é reutilizável pela sua equipa de toda. No entanto, estes custos são insignificant em comparação com horas que cada empregado invests a criação de saída e validar uma caixa de desenvolvimento está corretamente configurada para utilização individuais.

Além disso, as tarefas de desenvolvimento ou tecnologias poderão ter mais de escala, como varieties de configurações de desenvolvimento e configurações com várias máquina. Pode utilizar o Azure DevTest Labs para criar _receitas_ que automatizar a construção da "imagem dourada". Também pode utilizar o DevTest Labs para gerir políticas para a de VMs em execução sua equipa. [Utilizar o Azure DevTest Labs para programadores](/azure/devtest-lab/devtest-lab-developer-lab) é a origem para obter mais informações sobre DevTest Labs melhor.

## <a name="next-steps"></a>Passos Seguintes
Agora que sabe mais sobre as imagens pré-configuradas do Visual Studio, o passo seguinte consiste em criar uma nova VM:

* [Criar uma VM através do portal do Azure](quick-create-portal.md)
* [Descrição geral de máquinas virtuais do Windows](overview.md)
