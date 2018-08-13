---
title: Gerir a sua imagem de máquina virtual no Azure Marketplace | Documentos da Microsoft
description: Guia detalhado sobre como gerir a sua imagem de máquina virtual no Azure Marketplace após a publicação inicial
services: Azure Marketplace
documentationcenter: ''
author: HannibalSII
manager: hascipio
editor: ''
ms.assetid: cc8648d4-59c2-4678-b47d-992300677537
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 08/03/2016
ms.author: hascipio;
ms.openlocfilehash: e1f90650e71345957c2d353774cb8bef62c1868b
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39715020"
---
# <a name="post-production-guide-for-virtual-machine-offers-in-the-azure-marketplace"></a>Guia de pós-produção das ofertas de máquina virtual no Azure Marketplace
Este artigo explica como pode atualizar uma oferta de máquina de virtual em direto no Azure Marketplace. Ele orienta pelo processo de adicionar um ou mais novos SKUs para uma oferta existente. Ele também o orienta pelo processo de remoção de uma oferta de máquina de virtual em direto ou SKU do Marketplace.

Depois de um oferta/SKU é transitado para o [portal do Azure](http://portal.azure.com), não é possível alterar as seguintes caixas de texto:

* **Identificador da oferta**: no portal de publicação do, aceda a **máquinas virtuais** e selecione a sua oferta. Em seguida, clique em **imagens de VM** > **oferecem identificador**.
* **Identificador do SKU**: no portal de publicação do, aceda a **máquinas virtuais** e selecione a sua oferta. Em seguida, clique em **SKUS** > **adicione um SKU**.
* **Espaço de nomes do publicador**: no portal de publicação do, aceda a **máquinas virtuais** > **passo a passo** > **diga-nos sobre a sua empresa**(encontrado em "Passo 2 registar a sua empresa") > **espaço de nomes do publicador** > **espaço de nomes**.

Depois da oferta/SKU está listado na [Marketplace](http://azure.microsoft.com/marketplace), não é possível alterar as seguintes caixas de texto:

* **Identificador da oferta**: no portal de publicação do, aceda a **máquinas virtuais** e selecione a sua oferta. Em seguida, clique em **imagens de VM** > **oferecem identificador**.
* **Identificador do SKU**: no portal de publicação do, aceda a **máquinas virtuais** e selecione a sua oferta. Em seguida, clique em **SKUS** > **adicione um SKU**.
* **Espaço de nomes do publicador**: no portal de publicação do, aceda a **máquinas virtuais** > **passo a passo** > **diga-nos sobre a sua empresa**(encontrado em "Passo 2 registar") **espaço de nomes do publicador** > **espaço de nomes**.
* **Portas**: no portal de publicação do, aceda a **máquinas virtuais** e selecione a sua oferta. Em seguida, clique em **imagens de VM** > **portas abertas**.
* **Alteração de SKU (s) listados de preço**
* **Alteração do modelo de faturação dos SKU (s) listados**
* **Remoção de regiões de SKU (s) listados de faturação**
* **Alterar o número de discos de dados de SKU (s) listados**

## <a name="update-the-technical-details-of-a-sku"></a>Atualizar os detalhes técnicos de um SKU
Para adicionar uma nova versão para o SKU listado e voltar a publicar a oferta, siga estes passos:

1. Inicie sessão para o [portal de publicação](https://publish.windowsazure.com).
2. Vá para o **máquinas virtuais** separador e selecione a sua oferta.
3. No menu à esquerda, clique nas **imagens de VM** separador.
4. Na **SKUs** secção, localize o SKU que pretende atualizar.
5. Adicionar um novo número de versão para o SKU e clique nas **+** botão. A nova versão deve estar num formato x.y. z, em que X, Y e Z são números inteiros. Alterações de versão só devem ser incrementais.
6. Na **URL de VHD do SO** caixa, introduza a URI criado para o sistema de operativo VHD de assinatura de acesso partilhado e guardar as alterações.

   > [!IMPORTANT]
   > Não é possível incremento/diminuir o número de discos de dados de um SKU listado. Tem de criar um novo SKU neste caso. Para obter instruções detalhadas, consulte a seção [adicionar um novo SKU numa oferta listada](#add-a-new-sku-under-a-listed-offer).
   >
   >
7. Vá para o **PUBLISH** separador e clique em **fase emitir para teste**. Para obter instruções detalhadas sobre como testar a sua oferta no ambiente de teste, consulte [testar a sua oferta VM para o Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. Depois de o testar sua oferta para efeitos de teste, vá para o **publicar** separador na publicação portal. Clique em **solicitar aprovação para enviar por PUSH para produção** voltar a publicar a sua oferta no Marketplace.

    ![Imagens de VM](media/marketplace-publishing-vm-image-post-publishing/img01_07.png)

## <a name="update-the-nontechnical-details-of-an-offer-or-a-sku"></a>Atualizar os detalhes não técnicos de uma oferta ou um SKU
Pode atualizar o não técnicos (marketing, legal, o suporte, categorias) detalhes da sua oferta em direto ou SKU no Marketplace.

### <a name="update-the-offer-description-and-logos"></a>Atualizar a descrição da oferta e logotipos
Para atualizar os detalhes da oferta e voltar a publicar a oferta, siga estes passos:

1. Inicie sessão para o [portal de publicação](https://publish.windowsazure.com).
2. Vá para o **máquinas virtuais** separador e selecione a sua oferta.
3. No menu à esquerda, clique nas **MARKETING** separador.
4. Clique em **inglês (EUA)**.
5. Clique nas **detalhes** separador. Na **Descrição** secção, atualizar a oferta **título**, **resumo**, e **resumo EXTENSO** e guarde as alterações.

   > [!NOTE]
   > Quando atualizar os detalhes SKU, tenha em atenção estas restrições: 
   * Não introduza texto duplicado para a descrição da oferta e a descrição de SKU.
   * Não introduza texto duplicado para o título SKU e a oferta resumo há muito tempo. 
   * Não introduza texto duplicado para o título SKU e o resumo de oferta.
   >
   >

    ![Detalhes](media/marketplace-publishing-vm-image-post-publishing/img02.1_05.png)
6. No **LOGÓTIPOS** secção a **detalhes** separador, atualize os logótipos. Certifique-se de que os logótipos seguem os [diretrizes do Azure Marketplace](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).

   > [!NOTE]
   > Um ícone de hero é opcional. Pode optar por não carregue um ícone de hero. No entanto, após o carregamento de um ícone de hero, não há nenhuma provisão para eliminá-la a publicação portal. Siga os [diretrizes de ícone de hero](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
   >
   >
7. Vá para o **PUBLISH** separador e clique em **fase emitir para teste**. Para obter instruções detalhadas sobre como testar a sua oferta no ambiente de teste, consulte [testar a sua oferta VM para o Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. Depois de o testar sua oferta para efeitos de teste, vá para o **publicar** separador na publicação portal. Clique em **solicitar aprovação para enviar por PUSH para produção** voltar a publicar a sua oferta no Marketplace.

    ![Logótipos](media/marketplace-publishing-vm-image-post-publishing/img02.1_08.png)

### <a name="update-the-sku-description"></a>Atualizar a descrição de SKU
Para atualizar os detalhes SKU e voltar a publicar a oferta, siga estes passos:

1. Inicie sessão para o [portal de publicação](https://publish.windowsazure.com).
2. Vá para o **máquinas virtuais** separador e selecione a sua oferta.
3. No menu à esquerda, clique nas **MARKETING** separador.
4. Clique em **inglês (EUA)**.
5. Clique nas **planos** separador. Na **SKUs** secção, atualize o SKU **título**, **resumo**, e **Descrição** e guarde as alterações.

   > [!NOTE]
   > Quando atualizar os detalhes SKU, tenha em atenção estas restrições: 
   * Não introduza texto duplicado para a descrição da oferta e a descrição de SKU. 
   * Não introduza texto duplicado para o título SKU e a oferta resumo há muito tempo. 
   * Não introduza texto duplicado para o título SKU e o resumo de oferta.
   >
   >
6. Vá para o **PUBLISH** separador e clique em **fase emitir para teste**. Para obter instruções detalhadas sobre como testar a sua oferta no ambiente de teste, consulte [testar a sua oferta VM para o Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
7. Depois de o testar sua oferta para efeitos de teste, vá para o **publicar** separador na publicação portal. Clique em **solicitar aprovação para enviar por PUSH para produção** voltar a publicar a sua oferta no Marketplace.

    ![Planos](media/marketplace-publishing-vm-image-post-publishing/img02.2_07.png)

### <a name="change-existing-links-or-add-new-links"></a>Alterar o link já existente ou adicionar novas ligações
Para alterar as ligações existentes ou adicionar novas ligações e, em seguida, voltar a publicar a sua oferta, siga estes passos:

1. Inicie sessão para o [portal de publicação](https://publish.windowsazure.com).
2. Vá para o **máquinas virtuais** separador e selecione a sua oferta.
3. No menu à esquerda, clique nas **MARKETING** separador.
4. Clique em **inglês (EUA)**.
5. Clique nas **LINKS** separador.
6. Adicionar uma nova ligação, o **Links** secção, clique em **+ adicionar ligação**. Na **adicionar ligação** caixa de diálogo, introduza a ligação **título** e **URL** e guarde as alterações. Pode introduzir qualquer ligação que contém informações que podem ajudar os clientes.
7. Para atualizar ou eliminar uma ligação existente, selecione a ligação e clique nas **edite** botão ou o **eliminar** botão.

   > [!NOTE]
   > Certifique-se de que as ligações que introduzir nesta secção estão a funcionar corretamente, uma vez que estas ligações, seja validadas durante o processo de pedido de produção.
   >
   >
8. Vá para o **PUBLISH** separador e clique em **fase emitir para teste**. Para obter instruções detalhadas sobre como testar a sua oferta no ambiente de teste, consulte [testar a sua oferta VM para o Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
9. Depois de o testar sua oferta para efeitos de teste, vá para o **publicar** separador na publicação portal. Clique em **solicitar aprovação para enviar por PUSH para produção** voltar a publicar a sua oferta no Marketplace.

    ![Ligações](media/marketplace-publishing-vm-image-post-publishing/img02.3_09-01.png)

    ![Adicionar ligação](media/marketplace-publishing-vm-image-post-publishing/img02.3-2.png)

### <a name="change-an-existing-sample-image-or-add-a-new-sample-image"></a>Alterar uma imagem de exemplo existente ou adicionar uma nova imagem de exemplo
Para alterar uma imagem de exemplo existente ou adicionar novas imagens de exemplo e, em seguida, voltar a publicar a sua oferta, siga estes passos:

> [!NOTE]
> Imagem de apenas um exemplo é apresentada na [portal do Azure](https://portal.azure.com).
>
>

1. Inicie sessão para o [portal de publicação](https://publish.windowsazure.com).
2. Vá para o **máquinas virtuais** separador e selecione a sua oferta.
3. No menu à esquerda, clique nas **MARKETING** separador.
4. Clique em **inglês (EUA)**.
5. Clique nas **imagens de exemplo** separador.
6. Adicionar uma nova imagem de exemplo, o **imagens de exemplo** secção, clique em **carregar uma nova imagem** e guarde as alterações.

   > [!NOTE]
   > A inclusão de uma imagem de exemplo é opcional.
   >
7. Vá para o **PUBLISH** separador e clique em **fase emitir para teste**. Para obter instruções detalhadas sobre como testar a sua oferta no ambiente de teste, consulte [testar a sua oferta VM para o Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. Depois de o testar sua oferta para efeitos de teste, vá para o **publicar** separador na publicação portal. Clique em **solicitar aprovação para enviar por PUSH para produção** voltar a publicar a sua oferta no Marketplace.

    ![Imagens de exemplo](media/marketplace-publishing-vm-image-post-publishing/img02.4_09.png)

### <a name="update-the-legal-content"></a>Atualizar o conteúdo legal
Para atualizar o conteúdo legal e voltar a publicar a oferta, siga estes passos:

1. Inicie sessão para o [portal de publicação](https://publish.windowsazure.com).
2. Vá para o **máquinas virtuais** separador e selecione a sua oferta.
3. No menu à esquerda, clique nas **MARKETING** separador.
4. Clique em **inglês (EUA)**.
5. Clique nas **legais** separador. Na **legais** secção, Atualize as suas políticas/termos de utilização. Introduza ou cole políticas/termos no **termos de utilização** caixa e guarde as alterações.
6. O limite de caracteres para os termos legais de utilização é 1 milhão de carateres.
7. Vá para o **PUBLISH** separador e clique em **fase emitir para teste**. Para obter instruções detalhadas sobre como testar a sua oferta no ambiente de teste, consulte [testar a sua oferta VM para o Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. Depois de o testar sua oferta para efeitos de teste, vá para o **publicar** separador na publicação portal. Clique em **solicitar aprovação para enviar por PUSH para produção** voltar a publicar a sua oferta no Marketplace.

    ![Informações Legais](media/marketplace-publishing-vm-image-post-publishing/img02.5_08.png)

### <a name="update-the-support-information"></a>Atualizar as informações de suporte
Para atualizar as informações de suporte e voltar a publicar a oferta, siga estes passos:

1. Inicie sessão para o [portal de publicação](https://publish.windowsazure.com).
2. Vá para o **máquinas virtuais** separador e selecione a sua oferta.
3. No menu à esquerda, clique nas **suporte** separador.
4. Na **contacte Engineering** secção, atualizar os detalhes de contactos de engenharia. Esses detalhes são utilizados para comunicação interna entre os parceiros e a Microsoft apenas.
5. Na **suporte ao cliente** secção, atualizar os detalhes de contacto de suporte e o **URL de suporte**. Esses detalhes são utilizados para comunicação interna entre os parceiros e a Microsoft apenas.

   > [!NOTE]
   > Se quiser fornecer apenas suporte de e-mail, introduza um número de telefone fictício no **suporte ao cliente** secção. Neste caso, é utilizado em vez disso, o e-mail fornecido.
   >
   >
6. Vá para o **PUBLISH** separador e clique em **fase emitir para teste**. Para obter instruções detalhadas sobre como testar a sua oferta no ambiente de teste, consulte [testar a sua oferta VM para o Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
7. Depois de o testar sua oferta para efeitos de teste, vá para o **publicar** separador na publicação portal. Clique em **solicitar aprovação para enviar por PUSH para produção** voltar a publicar a sua oferta no Marketplace.

    ![Suporte](media/marketplace-publishing-vm-image-post-publishing/img02.6_07.png)

### <a name="update-the-categories"></a>As categorias de atualizações
Atualize a secção de categorias para a sua oferta e voltar a publicar a oferta, siga estes passos:

1. Inicie sessão para o [portal de publicação](https://publish.windowsazure.com).
2. Vá para o **máquinas virtuais** separador e selecione a sua oferta.
3. No menu à esquerda, clique nas **categorias** separador.
4. Na **categorias** secção, as categorias para a sua oferta de atualização e guardar as alterações. Pode escolher até cinco categorias para a galeria do Azure Marketplace.
5. Vá para o **PUBLISH** separador e clique em **fase emitir para teste**. Para obter instruções detalhadas sobre como testar a sua oferta no ambiente de teste, consulte [testar a sua oferta VM para o Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
6. Depois de o testar sua oferta para efeitos de teste, vá para o **publicar** separador na publicação portal. Clique em **solicitar aprovação para enviar por PUSH para produção** voltar a publicar a sua oferta no Marketplace.

    ![Categorias](media/marketplace-publishing-vm-image-post-publishing/img02.7_06.png)

## <a name="add-a-new-sku-under-a-listed-offer"></a>Adicionar um novo SKU numa oferta listada
Para adicionar um novo SKU na oferta em direto, siga estes passos:

1. Inicie sessão para o [portal de publicação](https://publish.windowsazure.com).
2. Vá para o **máquinas virtuais** separador e selecione a sua oferta.
3. No menu à esquerda, clique nas **SKUS** separador. Em seguida, clique em **adicione um SKU**. 
4. Na caixa de diálogo, introduza um **identificador do SKU** em minúsculas. Selecione o **traga seu próprio modelo de faturação de licença (BYOL)** caixa de verificação se pretender publicar o novo SKU com um modelo de faturação de BYOL. Caso contrário, desmarque a caixa de verificação. Clique na marca de escala para criar um novo SKU. Se não escolher o modelo de faturação de BYOL, o modelo de faturação é automaticamente definido para a cada hora. Se pretender que a versão de avaliação gratuita de 30 dias para o modelo de faturação por hora, selecione **um mês** para **é uma avaliação gratuita disponível?** Caso contrário, selecione **versão de avaliação não**. (**é uma avaliação gratuita disponível? ** aparece apenas se não selecionou BYOL ao criar o novo SKU.)

   > [!IMPORTANT]
   > **Ocultar este SKU do Marketplace, uma vez que sempre deve ser adquirido através de um modelo de solução** deve ser **Sim** *apenas* se está aprovada para publicar um modelo de solução. Caso contrário, esta opção deve ser sempre **não**.
   >
   >
4. No menu à esquerda, clique nas **imagens de VM** separador e descubra o novo SKU que criou.
5. Para configurar o novo SKU, consulte [obter certificação para sua imagem VM](marketplace-publishing-vm-image-creation.md#5-obtain-certification-for-your-vm-image) para obter orientações.
6. Para adicionar o material de marketing para o novo SKU, consulte [Marketplace fornecer conteúdo de marketing](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
7. Para adicionar informações de preços para o novo SKU, consulte [definir os preços](marketplace-publishing-push-to-staging.md#step-2-set-your-prices).
8. Vá para o **PUBLISH** separador e clique em **fase emitir para teste**. Para obter instruções detalhadas sobre como testar a sua oferta no ambiente de teste, consulte [testar a sua oferta VM para o Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
9. Depois de o testar sua oferta para efeitos de teste, vá para o **publicar** separador na publicação portal. Clique em **solicitar aprovação para enviar por PUSH para produção** voltar a publicar a sua oferta no Marketplace.

    ![SKUs](media/marketplace-publishing-vm-image-post-publishing/img03_09-01.png)

    ![Adicione um SKU](media/marketplace-publishing-vm-image-post-publishing/img03_09-02.png)

## <a name="change-the-data-disk-count-for-a-listed-sku"></a>Alterar o número de discos de dados para uma SKU listada
Não é possível incremento/diminuir o número de discos de dados de um SKU listado. Tem de criar um novo SKU neste caso. Para obter instruções detalhadas, consulte a seção [adicionar um novo SKU numa oferta listada](#add-a-new-sku-under-a-listed-offer).

## <a name="delete-a-listed-offer-from-the-marketplace"></a>Eliminar uma oferta listada no Marketplace
Vários aspectos tem de ser resolvido no caso de um pedido para remover uma oferta em direto. Para obter documentação de orientação da equipe de suporte para remover uma oferta listada no Marketplace, siga estes passos:

1. Emitir um pedido de suporte sobre o [criar um incidente](https://support.microsoft.com/en-us/getsupport?wf=0&tenant=ClassicCommercial&oaspworkflow=start_1.0.0.0&locale=en-us&supportregion=en-us&pesid=15635&ccsid=635993707583706681) página.

2. Selecione **tipo de problema** como **gestão de ofertas**e selecione **categoria** como **modificar uma oferta de e/ou o SKU já na produção**.
3. Submeta o pedido.

A equipa de suporte orienta-o ao longo do processo de eliminação de oferta/SKU.

> [!NOTE]
> Sempre é possível eliminar a oferta enquanto está em estado Rascunho (mas não de teste ou produção). Sobre o **histórico** separador, clique em **DESCARTAR rascunho**.
>
>

## <a name="delete-a-listed-sku-from-the-marketplace"></a>Eliminar um SKU listado no Marketplace
Para eliminar um SKU listado no Marketplace, siga estes passos:

1. Inicie sessão para o [portal de publicação](https://publish.windowsazure.com).

2. Vá para o **máquinas virtuais** separador e selecione a sua oferta.
3. No painel à esquerda, clique nas **SKUS** separador.
4. Selecione o SKU que pretende eliminar e clique nas **eliminar** botão.
5. Vá para o **publicar** separador na publicação portal. Clique em **solicitar aprovação para enviar por PUSH para produção** para voltar a publicar a oferta no Marketplace.
6. Depois da oferta será replicada no Marketplace, o SKU é eliminado do Marketplace e o portal do Azure.

## <a name="delete-the-current-version-of-a-listed-sku-from-the-marketplace"></a>Eliminar a versão atual de um SKU listada no Marketplace
Para eliminar a versão atual de um SKU listada no Marketplace, siga estes passos: 

1. Inicie sessão para o [portal de publicação](https://publish.windowsazure.com).

2. Vá para o **máquinas virtuais** separador e selecione a sua oferta.
3. No menu à esquerda, clique nas **imagens de VM** separador.
4. Selecione o SKU cuja versão atual que pretende eliminar e clique nas **eliminar** botão.
5. Vá para o **publicar** separador na publicação portal. Clique em **solicitar aprovação para enviar por PUSH para produção** para voltar a publicar a oferta no Marketplace.
6. Depois da oferta é replicada em Marketplace, a versão atual do SKU listado é eliminada do Marketplace e o portal do Azure. O SKU, em seguida, é revertido para a versão anterior.

## <a name="revert-the-listing-price-to-production-values"></a>Reverter o preço de listagem com valores de produção
Para reverter o preço de listagem com valores de produção, siga estes passos:

1. Inicie sessão para o [portal de publicação](https://publish.windowsazure.com).
2. Vá para o **máquinas virtuais** separador e selecione a sua oferta.
3. No menu à esquerda, clique nas **preços** separador.
4. Selecione uma região cujos preços que pretende repor.

    ![Regiões de preços](media/marketplace-publishing-vm-image-post-publishing/img08-04.png)
5. Para SKUs com um modelo de faturação por hora, repor os preços para todos os núcleos, como eles estão em produção para a região selecionada. Relativamente aos SKUs com um modelo de faturação de BYOL, tornar o SKU disponível na região, selecionando a caixa de verificação para o SKU no **EXTERNALLY-LICENSED (BYOL) SKU disponibilidade** secção.

    ![Modelos de preços](media/marketplace-publishing-vm-image-post-publishing/img08-05.png)
6. Clique em **AUTOPRICE outros mercados com base nos preços nos Estados Unidos**.

   > [!NOTE]
   > Etiqueta do botão pode ser diferente consoante a região que selecionou. Uma vez que selecionamos dos Estados Unidos, o nome do botão é **AUTOPRICE outros mercados com base no preços IN UNITED Estados**.
   >
   >

    ![Autoprice](media/marketplace-publishing-vm-image-post-publishing/img08-06.png)
7. Na página 1 do assistente Autoprice, escolha o mercado de base e clique nas **seta** botão.

    ![Mercado de base](media/marketplace-publishing-vm-image-post-publishing/img08-07.png)
8. Na página 2, escolha os planos de serviço e medidores (núcleos) e clique nas **seta** botão.

    ![Planos de serviços e medidores](media/marketplace-publishing-vm-image-post-publishing/img08-08.png)
9. Na página 3, clique em **todos os ativar/desativar** para selecionar todas as regiões. Ou pode selecionar manualmente a caixas de verificação para regiões específicas. Clique nas **seta** botão.

    ![Escolha mercados](media/marketplace-publishing-vm-image-post-publishing/img08-09.png)
10. Na página 4, reveja as taxas de câmbio e clique em **concluir**. O assistente repõe os preços, de acordo com as suas seleções.

11. Sobre o **preços** separador, clique em **vista de resumo e alterações**.
    Para **versão do modo de exibição**, selecione **rascunho**e para **comparar com**, selecione **produção**. Não se vir nenhuma diferença de preços, os preços revertido com êxito para os valores de produção.

    ![Ver resumo e alterações](media/marketplace-publishing-vm-image-post-publishing/img08-11.png)
12. Vá para o **PUBLISH** separador e clique em **fase emitir para teste**. Para obter instruções detalhadas sobre como testar a sua oferta no ambiente de teste, consulte [testar a sua oferta VM para o Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
13. Depois de o testar sua oferta para efeitos de teste, vá para o **publicar** separador na publicação portal. Clique em **solicitar aprovação para enviar por PUSH para produção** voltar a publicar a sua oferta no Marketplace.

## <a name="revert-the-billing-model-to-production-values"></a>Reverter o modelo de faturação para valores de produção
Para reverter o modelo de faturação para valores de produção, siga estes passos:

1. Inicie sessão para o [portal de publicação](https://publish.windowsazure.com).

2. Vá para o **máquinas virtuais** separador e selecione a sua oferta.
3. No menu à esquerda, clique nas **SKUS** separador.
4. Clique nas **editar** botão para reverter o modelo de faturação. Na janela que se abre, selecione ou desmarque os **a faturação e licenciamento é feito externamente do Azure (também conhecido como traga a sua própria licença)** caixa de verificação.

    ![Editar faturação](media/marketplace-publishing-vm-image-post-publishing/img09-04.png)
5. Siga os passos em "Revert o preço de listagem com valores de produção" neste artigo.
6. Vá para o **PUBLISH** separador e clique em **fase emitir para teste**. Para obter instruções detalhadas sobre como testar a sua oferta no ambiente de teste, consulte [testar a sua oferta VM para o Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
7. Depois de o testar sua oferta para efeitos de teste, vá para o **publicar** separador na publicação portal. Clique em **solicitar aprovação para enviar por PUSH para produção** voltar a publicar a sua oferta no Marketplace.

## <a name="revert-the-visibility-setting-of-a-listed-sku-to-the-production-value"></a>Reverter a definição de visibilidade de um SKU listada para o valor de produção
Para reverter a definição de visibilidade de um SKU listada para o valor de produção, siga estes passos:

1. Inicie sessão para o [portal de publicação](https://publish.windowsazure.com).

2. Vá para o **máquinas virtuais** separador e selecione a sua oferta.
3. No menu à esquerda, clique nas **SKUS** separador.
4. Selecione o SKU e reverter a definição de visibilidade do SKU para o valor de produção.

    ![Visibilidade](media/marketplace-publishing-vm-image-post-publishing/img10-04.png)
5. Depois de terminar com as alterações, clique em **solicitar aprovação para enviar por PUSH para produção** voltar a publicar a sua oferta no Marketplace.

## <a name="see-also"></a>Consulte também
* [Introdução ao: Publicar uma oferta no Azure Marketplace](marketplace-publishing-getting-started.md)
* [Compreender a geração de relatórios de dividendos](marketplace-publishing-report-payout.md)
* [Alterar o incentivo de revendedor de fornecedor de soluções Cloud](marketplace-publishing-csp-incentive.md)
* [Resolver problemas comuns de publicação no Marketplace](marketplace-publishing-support-common-issues.md)
* [Obtenha suporte como um publicador](marketplace-publishing-get-publisher-support.md)
* [Criar uma imagem de VM no local](marketplace-publishing-vm-image-creation-on-premise.md)
* [Criar uma máquina virtual com Windows no portal de pré-visualização do Azure](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
