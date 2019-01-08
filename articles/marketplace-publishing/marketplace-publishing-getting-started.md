---
title: Descrição geral de como criar e implementar uma oferta no Marketplace | Documentos da Microsoft
description: Compreender os passos necessários para se tornar um programador da Microsoft aprovado e criar e implementar uma imagem de máquina virtual, modelo, o serviço de dados ou serviço de Programador no Azure Marketplace
services: marketplace-publishing
documentationcenter: ''
author: v-miclar
manager: hascipio
editor: ''
ms.assetid: 5343bd26-c6e4-4589-85b7-4a2c00bba8ab
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/05/2017
ms.author: hascipio
ROBOTS: NOINDEX
ms.openlocfilehash: 7c2f33755628d6ee4880f1965545cde019b72c5e
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54073838"
---
# <a name="publish-and-manage-an-offer-in-the-azure-marketplace"></a>Publicar e gerir uma oferta no Azure Marketplace

> [!NOTE]
> Esta documentação já não está disponível e não é precisa. Em vez disso, vá para o Azure Marketplace [guia de vendedor](https://docs.microsoft.com/azure/marketplace/seller-guide/cloud-partner-portal-seller-guide) para obter orientações sobre a publicação de uma oferta no Azure Marketplace.

Este artigo é fornecido para ajudar os desenvolvedores a criar, implementar e gerir as suas soluções listadas no Azure Marketplace para outros clientes do Azure e parceiros adquirir e utilizar.

## <a name="marketplace-publishing"></a>Publicação do Marketplace
Como um publicador do Azure, pode distribuir e vender a sua solução inovadora ou serviço para outros desenvolvedores, ISVs e os profissionais de TI no Marketplace. Através do Marketplace, pode chegar aos clientes que pretendem desenvolver rapidamente seus aplicativos baseados na nuvem e soluções móveis. Se a sua solução destina-se os utilizadores empresariais, convém considerar a [AppSource](https://appsource.microsoft.com) marketplace.


## <a name="supported-types-of-solutions"></a>Suportados tipos de soluções
A primeira coisa que deseja fazer como um publicador é definir o tipo de solução da sua empresa está oferecendo. O Marketplace suporta os seguintes tipos de ofertas:

|Tipo de solução|Máquina virtual|Modelo de solução|
|---|---|---|
|**Definição**|Obter imagens pré-configuradas com um sistema operativo totalmente instalado e uma ou mais aplicações. Uma imagem de máquina virtual fornece as informações necessárias para criar e implementar máquinas virtuais no serviço máquinas virtuais do Azure.|Uma estrutura de dados que pode fazer referência a um ou mais serviços do Azure distintos, incluindo serviços publicados por outros vendedores. Os subscritores do Azure podem utilizá-lo para implementar uma ou mais ofertas de forma única e coordenada.|
|**Exemplo**|Como um publicador do Azure, criou e validados de uma VM com um serviço de base de dados inovadoras. Outros subscritores do Azure querem comprar e implementar esta VM em seus ambientes de serviço cloud.|Como um publicador do Azure, tiver fornecido um conjunto de serviços em todo o Azure torna rápido implementar serviços em nuvem com balanceamento de carga, maior segurança e elevada disponibilidade. Outros subscritores do Azure podem poupar tempo ao adquirir o modelo de solução que atenda às suas objetivo. Eles não têm manualmente, localizar, comprar, implementar e configurar os serviços do Azure idênticos ou semelhantes.|

> [!NOTE]
> Alguns passos são partilhados entre os diferentes tipos de soluções, e outras são distintas para o respetivo tipo de solução. Este artigo fornece uma breve visão geral dos passos que necessários para qualquer tipo de solução.

## <a name="publish-a-solution"></a>Publicar uma solução
![Nomeie os, registar, publicar](media/marketplace-publishing-getting-started/img01.png)

### <a name="nominate-your-solution-for-pre-approval"></a>Nomeie a sua solução de pré-aprovação
Para publicar uma máquina virtual [solução](https://createopportunity.azurewebsites.net) no Marketplace, conclua o Microsoft Azure Certified **formulário de nomeação de solução**.

>[!NOTE]
> Se estiver a trabalhar com um Gestor de conta de parceiro ou um Gestor de parceiro de DX, peça-lhe para nomear sua solução para o programa Azure Certified. Também pode ir para o [Microsoft Azure Certified](http://createopportunity.azurewebsites.net) página da Web e preencha o formulário de aplicação. Introduza o e-mail do seu Gestor de conta de parceiro ou o Gestor de parceiros DX no **contacto da Microsoft patrocinador** caixa.

Se cumprir os critérios de elegibilidade do [políticas de participação do Azure Marketplace](https://go.microsoft.com/fwlink/?LinkID=526833) e a sua aplicação é aprovada, podemos começar a trabalhar consigo para carregar sua solução no Marketplace.

### <a name="register-your-account-as-a-microsoft-seller"></a>Registar a sua conta como um vendedor da Microsoft
Registe-se a sua conta Microsoft como um [conta do Microsoft Developer](marketplace-publishing-accounts-creation-registration.md).

### <a name="publish-your-solution"></a>Publicar a sua solução
Para publicar uma solução no Marketplace, siga estes passos:
1. Cumprir os requisitos não técnicos.

    a. Atender a [pré-requisitos não técnicos](marketplace-publishing-pre-requisites.md).

    b. Atender a [pré-requisitos técnicos VM](marketplace-publishing-vm-image-creation-prerequisites.md).

    c. Atender a [pré-requisitos técnicos do modelo de solução](marketplace-publishing-solution-template-creation-prerequisites.md).

2. Crie a sua oferta.

    a. Criar uma [máquina virtual](marketplace-publishing-vm-image-creation.md) oferecem.

    b. Criar uma [modelo de solução](marketplace-publishing-solution-template-creation.md) oferecem.

3. Criar a sua oferta [conteúdo de marketing](marketplace-publishing-push-to-staging.md).

4. Teste a sua oferta para efeitos de teste.

    a. Testar a sua oferta VM na [teste](marketplace-publishing-vm-image-test-in-staging.md).

    b. Testar a sua oferta de modelo de solução no [teste](marketplace-publishing-solution-template-test-in-staging.md).

5. Implementar a sua oferta para o [Marketplace](marketplace-publishing-push-to-production.md).


### <a name="create-and-manage-a-virtual-machine-image"></a>Criar e gerir imagens de máquinas virtuais
Criar e gerir uma imagem de VM ao utilizar estes recursos:
* Criar uma imagem de VM [locais](marketplace-publishing-vm-image-creation-on-premise.md).
* Criar uma máquina virtual em execução [Windows no portal do Azure](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Criar uma máquina virtual em execução [Linux no portal do Azure](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Solucionar problemas comuns encontrados durante [criação do VHD](marketplace-publishing-vm-image-creation-troubleshooting.md).

## <a name="manage-your-solution"></a>Gerir a sua solução
Gerir a sua solução com a ajuda dos seguintes recursos:
* [Leia o guia de pós-produção das ofertas de máquina virtual](marketplace-publishing-vm-image-post-publishing.md)
* [Atualizar os detalhes não técnicos de uma oferta ou um SKU](marketplace-publishing-vm-image-post-publishing.md#update-the-nontechnical-details-of-an-offer-or-a-sku)
* [Atualizar os detalhes técnicos de uma oferta ou um SKU](marketplace-publishing-vm-image-post-publishing.md#update-the-technical-details-of-a-sku)
* [Adicionar um novo SKU numa oferta listada](marketplace-publishing-vm-image-post-publishing.md#add-a-new-sku-under-a-listed-offer)
* [Alterar o número de discos de dados para uma SKU listada](marketplace-publishing-vm-image-post-publishing.md#change-the-data-disk-count-for-a-listed-sku)
* [Eliminar uma oferta listada no Marketplace](marketplace-publishing-vm-image-post-publishing.md)
* [Eliminar um SKU listado no Marketplace](marketplace-publishing-vm-image-post-publishing.md#delete-a-listed-sku-from-the-marketplace)
* [Eliminar a versão atual de um SKU listada no Marketplace](marketplace-publishing-vm-image-post-publishing.md#delete-the-current-version-of-a-listed-sku-from-the-marketplace)
* [Reverter o preço de listagem com valores de produção](marketplace-publishing-vm-image-post-publishing.md#revert-the-listing-price-to-production-values)
* [Reverter o modelo de faturação para valores de produção](marketplace-publishing-vm-image-post-publishing.md#revert-the-billing-model-to-production-values)
* [Reverter a definição de visibilidade de um SKU listada para o valor de produção](marketplace-publishing-vm-image-post-publishing.md#revert-the-visibility-setting-of-a-listed-sku-to-the-production-value)

## <a name="additional-resources"></a>Recursos adicionais
[Configurar o Azure PowerShell](marketplace-publishing-powershell-setup.md)
