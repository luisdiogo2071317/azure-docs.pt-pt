---
title: Testar a sua oferta VM para o Marketplace | Documentos da Microsoft
description: Compreenda como testar a imagem de VM para o Azure Marketplace.
services: marketplace-publishing
documentationcenter: ''
author: v-miclar
manager: hascipio
editor: ''
ms.assetid: 7a41c3c6-625c-4478-b804-e124dee89040
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2016
ms.author: hascipio
ROBOTS: NOINDEX
ms.openlocfilehash: b90353dbbc5d019897735cfc05caa3ee094dfedc
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54078818"
---
# <a name="test-your-vm-offer-for-the-azure-marketplace-in-staging"></a>Testar a sua oferta VM para o Azure Marketplace para efeitos de teste
Fase de testes significa implementar o SKU numa "sandbox" privada "onde pode testar e validar sua funcionalidade antes de o implementar no Marketplace. O SKU é apresentada no teste tal como faria para um cliente que tenha implementado. Deve certificar a sua imagem VM antes de ele é emitido para teste.

## <a name="step-1-push-your-offer-to-staging"></a>Passo 1: Emitir a oferta para teste
1. Sobre o **Publish** separador, clique em **emitir para teste**.
   
    ![desenho](media/marketplace-publishing-vm-image-test-in-staging/vm-image-push-to-staging.png)
2. Se o Portal de publicação notifica-o de quaisquer erros, corrija-os.
3. Na **quem pode aceder a sua oferta em etapas?** caixa de diálogo, introduza a lista de subscrições do Azure que irá utilizar para pré-visualizar a oferta no [portal do Azure](https://portal.azure.com).
   
   > [!NOTE]
   > Em caso de máquinas virtuais e modelos de soluções, volte **não** subscrições de lista aprovada de tipo CSP, DreamSpark ou do Azure no Open.
   > 
   > 
   >
   > Em caso de máquinas virtuais, ao clicar no botão **fase emitir para teste**, os passos seguintes são executados nos bastidores. Poderá ver o progresso de cada passo no separador Publicar na publicação portal. Tem de verificar esta página em intervalos regulares (até que o estado é apresentado em ESTÁGIOS) para qualquer informação de falha que precisam de correção da sua parte.

   > - Em primeiro lugar, o seu pedido de teste vai para a equipe de certificação que validam o vhd. No entanto, se o pedido tem apenas alterações de marketing, em seguida, o passo de certificação será ignorado.
   > - Depois de concluída a certificação, da oferta de início da replicação em todos os datacenters do Azure. Ele geralmente usa 24-48hours para a conclusão da replicação, mas poderá demorar até uma semana, dependendo do tamanho do vhd. No entanto, se o pedido tem apenas alterações de marketing, em seguida, a replicação é mais rápida.
   > - Quando a replicação estiver concluída, em seguida, a oferta estará disponível na [portal do Azure](http:/portal.azure.com). Nessa época, o estado se tornar TRANSITADOS para a publicação portal. Uma oferta em etapas está visível no [portal do Azure](http:/portal.azure.com) usando apenas o IDs de e-mail associado à subscrição com a qual a oferta é testada.

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) ao utilizar uma das subscrições Azure listadas no passo anterior.
2. Localizar a sua oferta e validar seus pontos de imagem VM:
   
   * Certifique-se de que conteúdo de marketing aparece corretamente no Marketplace.
   * Implementação de ponto-a-ponto da imagem de VM.
     
      ![img-map-portal](media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg)

> [!IMPORTANT]
> A oferta permanecerá na fase de testes até notificar a Microsoft através do Portal de publicação [**Publish** separador > clique no botão **"Solicitar aprovação para Push para produção"**] que está pronto para enviar para produção. Esta é a altura ideal para ter todos os membros da sua equipa que verifiquem tudo o que na preparação da sua oferta listadas.
> 
> A plataforma de teste foi concebida para a oferta de teste num modo de pré-visualização pelo editor. Podemos desencorajar vivamente utilizar este Multiplataforma para fins de commerical.
> 
> 

## <a name="next-steps"></a>Passos Seguintes
Agora que a oferta é "teste" e testou a sua funcionalidade e conteúdo de marketing, pode avançar para a fase final de publicação, **passo 4**: [Implementar a sua oferta no Marketplace](marketplace-publishing-push-to-production.md).

## <a name="see-also"></a>Consulte também
* [Introdução: Como publicar uma oferta no Azure Marketplace](marketplace-publishing-getting-started.md)

