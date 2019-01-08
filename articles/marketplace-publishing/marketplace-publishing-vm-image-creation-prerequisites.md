---
title: Pré-requisitos técnicos para a criação de uma imagem de máquina virtual para o Azure Marketplace | Documentos da Microsoft
description: Compreenda os requisitos para criar e implementar uma imagem de máquina virtual no Azure Marketplace para outras pessoas para comprar.
services: marketplace-publishing
documentationcenter: ''
author: v-miclar
manager: hascipio
editor: ''
ms.assetid: 63c16966-0304-4b17-a715-368a0a5ccb2c
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 04/29/2016
ms.author: hascipio; v-divte
ROBOTS: NOINDEX
ms.openlocfilehash: 1fdf945645b2d5c89d7f3750ec836595adb90b06
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54077084"
---
# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-the-azure-marketplace"></a>Pré-requisitos técnicos para a criação de uma imagem de máquina virtual para o Azure Marketplace
Leia o processo cuidadosamente antes de começar e compreender onde e por que cada passo é realizado. Tanto quanto possível, deve preparar informações da sua empresa e outros dados, transferir as ferramentas necessárias, e/ou criar componentes técnicos antes de iniciar o processo de criação da oferta. Estes itens devem ficar claros da revisão deste artigo.  

## <a name="download-needed-tools--applications"></a>Baixe as ferramentas necessárias e aplicações
Deve ter os seguintes itens prontos antes de iniciar o processo:

* Dependendo do sistema operativo tiver como objetivo, instalar o [cmdlets do Azure PowerShell](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) ou [ferramenta de interface de linha de comandos do Linux](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) do [transferências do Azure](https://azure.microsoft.com/downloads/) página.
* Instale o Explorador de armazenamento do Azure do CodePlex.
* Transfira e instale a ferramenta de teste de certificação para o Azure Certified:
  * [http://go.microsoft.com/fwlink/?LinkID=526913](https://go.microsoft.com/fwlink/?LinkID=526913). Precisa de um computador baseado no Windows para executar a ferramenta de certificação. Se não tiver um computador baseado em Windows disponível, pode executar a ferramenta com uma VM baseada no Windows no Azure.

## <a name="platforms-supported"></a>Plataformas suportadas
Pode desenvolver VMs baseadas no Azure no Windows ou Linux. Alguns elementos do processo de publicação – por exemplo, criar um compatível com o Azure disco rígido virtual (VHD) - utilizar diferentes ferramentas e os passos, dependendo do sistema operativo que está a utilizar:  

* Se estiver a utilizar o Linux, consulte a secção "Criar um VHD compatível com o Azure (baseado em Linux)" do [guia de publicação de imagem de Máquina Virtual](marketplace-publishing-vm-image-creation.md).
* Se estiver a utilizar o Windows, consulte a secção "Criar um VHD compatível com o Azure (baseados em Windows)" do [guia de publicação de imagem de Máquina Virtual](marketplace-publishing-vm-image-creation.md).

> [!NOTE]
> Precisa de acesso a uma máquina com base no Windows para:
> 
> * Execute a ferramenta de validação de certificação.
> * Crie o URL de assinatura de acesso VHD partilhado para o envio de certificação do VHD.
> 
> 

## <a name="develop-your-vhd"></a>Desenvolva o VHD
Pode desenvolver VHDs do Azure na cloud ou no local:

* Desenvolvimento com base na cloud significa que todas as etapas de desenvolvimento são executadas remotamente num VHD residente no Azure.
* Desenvolvimento no local requer um VHD de baixar e desenvolver com a infraestrutura no local. Embora essa abordagem seja possível, não o recomendamos. Desenvolvimento para Windows ou SQL no local requer que tenha as chaves de licença relevantes no local. Não pode incluir ou instalar o SQL Server depois de criar uma VM. Também deve basear a sua oferta numa imagem aprovada SQL no portal do Azure. Se optar por desenvolver no local, tem de efetuar alguns passos de forma diferente do que se estivesse desenvolvendo na cloud. Pode encontrar informações relevantes [criar uma imagem VM no local](marketplace-publishing-vm-image-creation-on-premise.md).

[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
