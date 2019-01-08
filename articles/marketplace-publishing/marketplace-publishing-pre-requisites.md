---
title: Pré-requisitos não-técnicos para a criação de uma oferta para o Azure Marketplace | Documentos da Microsoft
description: Compreenda os requisitos para criar e implementar uma oferta no Azure Marketplace para outras pessoas para comprar.
services: marketplace-publishing
documentationcenter: ''
author: v-miclar
manager: hascipio
editor: ''
ms.assetid: 3dae463b-8f48-4f52-8fa8-4e3975f09f43
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 08/18/2016
ms.author: hascipio
ROBOTS: NOINDEX
ms.openlocfilehash: 4b925522186d2d9ae537431c1d96d39b107ad967
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54073174"
---
# <a name="general-prerequisites-for-creating-an-offer-for-the-azure-marketplace"></a>Pré-requisitos gerais para a criação de uma oferta para o Azure Marketplace
Compreenda os pré-requisitos gerais e de negócios-centrado no processo que são necessários para seguir em frente com o processo de criação da oferta.

## <a name="ensure-that-you-are-registered-as-a-seller-with-microsoft"></a>Certifique-se de que está registado como um vendedor com a Microsoft
Para obter instruções detalhadas sobre o registo de uma conta de vendedor com a Microsoft, aceda a [criação e registo de conta](marketplace-publishing-accounts-creation-registration.md).

* Se sua empresa já está registrada como um vendedor no Centro de desenvolvimento e pretende criar uma nova oferta, em seguida, iniciar sessão para a publicação de portal com o mesmo ID de e-mail com o Centro de desenvolvimento do registo é feito. Este passo é necessário para que o portal do Centro de desenvolvimento e a publicação estão ligados entre si.
* Se sua empresa já está registrada como um vendedor no Centro de desenvolvimento e que pretende editar uma oferta existente, em seguida, inicie sessão para a publicação portal com a conta de administrador ou com uma conta que é adicionada como um coadmin na publicação portal. Passos para adicionar uma conta de coadmin é indicada abaixo.

## <a name="steps-to-add-a-coadmin-in-the-publishing-portal"></a>Passos para adicionar um coadmin no Portal de publicação
Os administradores de publicação portal pode adicionar os outros membros da empresa, que estão a trabalhar no aplicativo, como um coadmin na publicação portal. **Partindo do princípio que é o administrador,** indicada abaixo estão os passos para adicionar um coadmin.

> [!NOTE]
> Para usuários novos, antes de adicionar um coadministrador na publicação portal, certifique-se de que criou, pelo menos, um aplicativo na publicação portal. Isto é necessário como o **os PUBLICADORES** separador aparecer apenas depois de criar pelo menos uma aplicação na publicação portal.
> 
> 

1. Certifique-se de que o ID de e-mail coadmin é um account(MSA) de Microsoft. Se não estiver, registrá-la como uma MSA através desta [link](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1).
2. Certifique-se de que existe, pelo menos, um aplicativo sob a conta de administrador antes de tentar adicionar um coadmin.
3. Depois de terminar os passos acima, inicie sessão em para a publicação de portal com o coadmin ID de e-mail e, em seguida, terminar a sessão.
4. Agora iniciar sessão para a publicação de portal com o ID de e-mail de administrador.
5. Navegue para os editores -> selecione a sua conta -> administradores -> adicionar o coadmin (captura de ecrã indicada abaixo)
   
    ![desenho](media/marketplace-publishing-pre-requisites/imgAddAdmin_05.png)
6. Certifique-se de que os ids de e-mail fornecidos em vários estágios do processo de publicação (por exemplo, Dev Center, portal de publicação) são monitorizados para qualquer comunicação da Microsoft.
7. Para um registo de centro de desenvolvimento e evite o uso de uma conta associada a uma única pessoa. Esta sugestão remove a dependência de uma única pessoa.
8. Se tiver quaisquer problemas com o registo de centro de desenvolvimento, em seguida, emitir um pedido de suporte através desta [link](https://developer.microsoft.com/en-us/windows/support).

## <a name="steps-to-delete-a-coadmin-in-the-publishing-portal"></a>Passos para eliminar um coadmin na publicação portal
**Partindo do princípio que é o administrador,** indicada abaixo estão os passos para eliminar um coadmin.

1. Inicie sessão para a publicação de portal com o ID de e-mail de administrador.
2. Navegue para **os publicadores** -> selecione a sua conta -> **administradores** -> **Coadministradores**.
3. Clique nas **X** junto a coadmin que pretende eliminar de tot (captura de ecrã indicada abaixo).
   
    ![desenho](media/marketplace-publishing-pre-requisites/imgDeleteAdmin_03.png)

> [!IMPORTANT]
> Não é necessário que concluir a informações fiscais e bancárias da empresa, se estiver a planear publicar ofertas gratuitas apenas (ou traga a sua própria licença).
> 
> O registo de empresa deve ser concluído para começar a utilizar. No entanto, enquanto a sua empresa trabalha nas informações fiscais e bancárias na conta do Microsoft Developer, os desenvolvedores começar a trabalhar na criação da imagem de máquina virtual na [Portal de publicação](https://publish.windowsazure.com), obter certificá-la e teste ele é no ambiente de teste do Azure. Terá a aprovação de conta de vendedor completa apenas para a etapa final de publicar a sua oferta no Azure Marketplace.
> 
> 

## <a name="acquire-an-azure-pay-as-you-go-subscription"></a>Adquirir uma subscrição "pay as you go" do Azure
Este valor é a subscrição que irá utilizar para criar as imagens VM e passar as imagens para o [do Azure Marketplace](https://azure.microsoft.com/marketplace/). Se não tiver uma subscrição existente, em seguida, inscreva-se em https://account.windowsazure.com/signup?offer=ms-azr-0003p.

## <a name="sell-from-countries"></a>Países de "Destino de venda de entrada"
> [!WARNING]
> Para vender os seus serviços no Azure Marketplace, deve certificar-se de que a sua entidade registada a partir de um dos países aprovados "destino de venda-de". Esta restrição é por motivos de dividendos e tributação. Estamos ativamente à procura para expandir essa lista de países em breve, portanto, fique atento. Para obter a lista completa, consulte 1b da secção do [políticas de participação do Azure Marketplace](https://go.microsoft.com/fwlink/?LinkID=526833).
> 
> 

## <a name="next-steps"></a>Passos Seguintes
Assim que os pré-requisitos não técnicos são cumpridos, em seguida, estão os pré-requisitos técnicos específicos da oferta. Clique na ligação para o artigo para o tipo de oferta respectivos que gostaria de criar para o Azure Marketplace.

* [Pré-requisitos técnicos de VM](marketplace-publishing-vm-image-creation-prerequisites.md)
* [Solução modelo pré-requisitos técnicos](marketplace-publishing-solution-template-creation-prerequisites.md)

## <a name="see-also"></a>Consulte também
* [Introdução: Como publicar uma oferta no Azure Marketplace](marketplace-publishing-getting-started.md)

