---
title: Oferta de máquina virtual de autor | Documentos da Microsoft
description: Publica uma máquina virtual no Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: pbutlerm
ms.openlocfilehash: e01fc839e901b39618990946a11ee8883dbe5f74
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48810831"
---
# <a name="publish-a-virtual-machine-to-azure-marketplace"></a>Publicar uma máquina virtual no Azure Marketplace

Este artigo fornece os passos que precisa para publicar uma oferta de máquina virtual no Azure Marketplace.

## <a name="prerequisites"></a>Pré-requisitos

Os seguintes pré-requisitos de técnicos e aplicam a publicação de máquinas virtuais no Azure Marketplace

### <a name="technical"></a>Técnico

-   [Pré-requisitos técnicos para a criação de uma imagem de máquina virtual para o Azure Marketplace](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation-prerequisites)

-   [Criar e carregar um VHD do Linux](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-create-upload-generic?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

-   [Criar e testar uma VM do Linux a partir de uma imagem](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-upload-vhd)

-   [Criar e carregar um VHD do Windows ](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-prepare-for-upload-vhd-image?toc=/azure/virtual-machines/windows/toc.json)

-   [Criar e testar uma VM do Windows a partir de uma imagem](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-create-vm-generalized-managed?toc=/azure/virtual-machines/windows/toc.json)

-   [Como resolver problemas comuns encontrados durante a criação do VHD](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation-troubleshooting)

-   [Recomendações de segurança para imagens do Azure Marketplace](https://docs.microsoft.com/en-us/azure/security/security-recommendations-azure-marketplace-images)


### <a name="non-technical-business-requirements"></a>Não técnicas (requisitos de negócios)

 -   A sua empresa (ou respetiva subsidiária) está localizada num país de origem de venda suportado pelo Azure Marketplace

-   Seu produto têm de estar licenciado de forma que seja compatível com os modelos de faturação suportados pelo Azure Marketplace

-   É responsável por disponibilizar suporte técnico aos clientes de forma que sejam comercialmente razoável. Esse suporte pode ser gratuito, pago ou através da Comunidade de suporte.

-   É responsável pelo licenciamento do software e as dependências de software de terceiros.

-   Forneça o conteúdo que satisfaça os critérios para a sua oferta seja incluída no Azure Marketplace e no Portal de gestão do Azure.

-   Aceita os termos do contrato de publicador e políticas de participação do Azure Marketplace.

-   Concorda em estar em conformidade com o [termos de utilização](https://azure.microsoft.com/support/legal/website-terms-of-use/) , [declaração de privacidade da Microsoft](http://www.microsoft.com/privacystatement/default.aspx), e [contrato do Microsoft Azure Certified programa](https://azure.microsoft.com/support/legal/marketplace/certified-program-agreement/).

## <a name="before-you-begin"></a>Antes de começar

Depois de atender a todos os pré-requisitos, pode começar a sua oferta de modelo de solução de criação. Antes de começar, reveja a seguinte oferta e informações do SKU.

**Oferta**

Uma oferta de aplicação do Azure corresponde a uma classe de produto, oferta do publicador. Se tiver um novo tipo de solução/aplicação que gostaria de estar disponíveis no Azure Marketplace, uma nova oferta é a melhor abordagem. Uma oferta é uma coleção de SKUs. Cada oferta é apresentada como uma entidade própria no Azure Marketplace.

**SKU**

Um SKU é a unidade de compra mais pequena de uma oferta. Enquanto na mesma classe de produto (oferta), o SKUs permitem-lhe distinguir entre diferentes funcionalidades suportadas. Por exemplo, a oferta é gerenciada ou modelos de faturação diferentes e não são suportados.

Adicione várias SKUs nos seguintes cenários:
- Pretende suportar diferentes modelos de faturação, como traga a sua própria licença (BYOL) ou Pay as you Go (PAYG).
- Cada SKU suporta um conjunto de recursos diferentes e cada conjunto de recursos é um preço diferente.

Um SKU aparece na oferta principal no Azure Marketplace e é exibido como sua própria entidade que podem ser comprada no portal do Azure.

## <a name="to-create-a-new-offer"></a>Para criar uma nova oferta

1.  Inicie sessão para o [portal do Cloud partner](http://cloudpartner.azure.com/).

2.  Na barra de navegação esquerdo, selecione **+ nova oferta**e, em seguida, selecione **máquinas virtuais**.

    ![Nova oferta para máquinas virtuais](./media/cloud-partner-portal-publish-virtual-machine/publishvm1.png)

3.  Sob **nova oferta**, selecione **Editor**.

![Nova oferta do Editor](./media/cloud-partner-portal-publish-virtual-machine/publishvm2.png)

4.  Sob **Editor**, irá fornecer informações nas seguintes vistas:
    - Definições da oferta
    - SKUs
    - Marketplace
    - Suporte a que cada vista contém um conjunto de campos para que possa preencher. Necessário campos são indicados com um asterisco vermelho (\*)

## <a name="to-configure-offer-settings"></a>Para configurar as definições da oferta

1. Configure as seguintes opções **oferecer identidade** campos nas definições da oferta.

    **ID de oferta**

     Um identificador exclusivo para a oferta de um perfil do publicador. Este ID está visível no produto URLs, modelos do Azure Resource Manager, e relatórios de faturação. Pode utilizar apenas carateres alfanuméricos em minúsculas ou traços (-). O ID não pode terminar com um travessão e pode ter um máximo de 50 carateres. Por exemplo, se um publicador **contoso** publica uma oferta com o ID de oferta **exemplo-vm**, ele aparece no Azure marketplace como **https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-vm?tab=Overview**  
    >[!Note]
    >Este campo é bloqueado quando uma oferta entra no ar.

    **ID de publicador**

    Lista pendente para o perfil do publicador. Escolha o perfil que pretende publicar a oferta em. 
    >[!Note]
    >Este campo é bloqueado quando uma oferta entra no ar.

    **Nome**

    O nome a apresentar para a sua oferta. Este nome é apresentado no Azure Marketplace e no Portal do Azure. Pode ter um máximo de 50 carateres. Utilize as seguintes orientações para o nome da oferta:
    -  Inclua um nome de marca reconhecível para o produto. 
    - Não inclua o nome da sua empresa aqui, a menos que essa é a forma como a oferta é comercializada.
    - Se estiver marketing esta oferta em seu próprio Web site, certifique-se de que o nome é idêntico ao nome no seu Web site.

2. Selecione **guardar** para concluir as definições da oferta.

## <a name="to-create-a-sku"></a>Para criar um SKU

1. Selecione **SKUs**. 
2. Selecione **adicione um SKU** para introduzir um ID de SKU. O ID de SKU é um identificador exclusivo para o SKU dentro de uma oferta. Este ID está visível no produto URLs, modelos do Azure Resource Manager, e relatórios de faturação. O ID de SKU:
    - Só pode ter um máximo de 50 carateres.
    - Só pode ser composto por carateres alfanuméricos em minúsculas ou traços (-).
    - O ID não pode terminar com um hífen.

    ![Adicione um SKU](./media/cloud-partner-portal-publish-virtual-machine/publishvm4.png)


## <a name="configure-sku-details"></a>Configure os detalhes SKU

Depois de adicionar um SKU, aparece na lista de SKUs na vista de SKUs. Para ver os detalhes SKU, selecione o nome SKU. Pode utilizar a vista de detalhes para adicionar informações nos campos a seguir.

### <a name="hide-this-sku"></a>Ocultar este SKU

Utilize esta definição para gerir a visibilidade SKU. Se "Ocultar este SKU" estiver desativado, o SKU é visível na [do Azure Marketplace](https://azuremarketplace.microsoft.com) e, na [Portal do Azure](https://portal.azure.com/) aos clientes. Pode querer ocultar o SKU, se quiser apenas disponível através de modelos de soluções e não para compra individualmente.

### <a name="cloud-availability"></a>Disponibilidade da cloud

Este campo permite-lhe definir a disponibilidade do SKU nas Clouds do Azure diferente.

**Azure público**

Este SKU pode ser implementado para os clientes em todas as regiões públicas do Azure com integração do Marketplace.

**O Azure Government Cloud**

Este SKU pode ser implementado na Cloud do Azure Government. Antes de publicação para [do Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners), recomendamos que os publicadores testar e validar a sua solução funciona conforme esperado. Para a fase e de teste, [pedir uma conta de avaliação](https://azure.microsoft.com/offers/ms-azr-usgov-0044p). 

>[!Note]
>O Microsoft Azure Government é uma cloud de Comunidade governamental com acesso controlado para clientes do Federal, estado, local ou tribal, elegíveis de parceiros de e para atender a essas entidades.

### <a name="countryregion-availability"></a>Disponibilidade de país/região

Este campo identifica as regiões em que o SKU vai estar disponível para compra. Precisa considerar cuidadosamente qual realizar suas SKUs disponíveis. Alguns países estão classificados como "Microsoft imposto Remitted país".

-   Em "Microsoft imposto Remitted país", a Microsoft cobra impostos de clientes e paga impostos de (remits) para o Governo.

-   Em outros países, os parceiros são responsáveis por coletar os clientes de impostos e pagar impostos ao. Se escolher vender nestes países, tem de ter a capacidade de calcular e pagar impostos nos mesmos.

![Selecionar país/região de disponibilidade](./media/cloud-partner-portal-publish-virtual-machine/publishvm5.png)

### <a name="pricing"></a>Preços

Atualmente são suportados dois modelos de preços.

#### <a name="bring-your-own-license-byol"></a>Bring-Your-própria licença (BYOL)

Gerir o licenciamento do software em execução na VM. A Microsoft cobra apenas os custos de infraestrutura.

#### <a name="usage-based-monthly-billed-sku"></a>SKU de faturação mensal com base de utilização

Os clientes serão cobrados numa base por hora com base nas tarifas definidas pelos publicadores sobre os tamanhos VM. No caso de **faturação à hora** modelo dos SKUs, o preço total será a soma do custo do software cobrado pelo fabricante e o custo de infraestrutura cobrada pela Microsoft. Esse custo total será apresentado ao cliente como um preço por hora e mensal quando estiver a considerar a compra. A faturação nesse caso será mensalmente.

Dentro do modelo de utilização com base, existem outras definições necessárias por si.

**Versão de avaliação gratuita**

Pode especificar se pretende fornecer uma avaliação gratuita para os seus clientes.
Aqui o cliente não de pagar o custo de software durante os primeiros dias de 30/90 (consoante a seleção) após a implementação da VM. Depois de decorrido o período de avaliação gratuita eles cobrada numa base por hora com base nas tarifas definidas pelos publicadores no modelo por hora.

**Preço de núcleo por**

Pode definir o preço por núcleo para o SKU. Para isso, apenas tem de introduzir um preço base para um único núcleo e podemos auto-compute os preços para o resto dos núcleos. Introduza os preços em dólares americanos no portal e podemos auto-calculará os preços para outras regiões. Pode verificar os preços noutras regiões com **exportar dados de preços**

![Preço de núcleo por](./media/cloud-partner-portal-publish-virtual-machine/publishvm6.png)


**Preços discretos**

Pode definir os preços de cada conjuntos de núcleos individualmente se desejar colocar o preço separadamente em cada núcleo.

![Preços discretos](./media/cloud-partner-portal-publish-virtual-machine/publishvm7.png)

**Preços de importação-exportação**

Tem a flexibilidade para exportar os preços que tenha sido configurada através do portal para fazer alterações através da interface do excel. Isto também permite-lhe verificar os preços por região e preços em moedas locais.
Clicar em **preços de exportação** irá transferir um ficheiro do excel com pré-preenchido de detalhes dos preços. Pode editá-las dentro do excel e, em seguida, usar **preço de importação** para importar as alterações efetuadas.
Os preços importados refletirá o portal também.

No excel preço, os preços para as diferentes regiões estão listados na moeda local. A taxa de câmbio que utilizamos é atualizada diariamente.

![Exportar-importar preços com exemplos de taxa de câmbio](./media/cloud-partner-portal-publish-virtual-machine/publishvm8.png)

>[!IMPORTANT]
>-   Os preços não podem ser alterados depois de uma oferta entra no ar. No entanto, ainda poderá adicionar ou remover regiões suportadas.
>-   Este preço é cobrado ao utilizador para além [Azure\'preços das máquinas virtuais de s](http://aka.ms/vmpricingdetails).
>-   Os preços são definidos para todas as regiões na moeda local, utilizando as taxas de moeda disponíveis no momento da definição dos preços.
>-   Para definir ou ver o preço de cada região individualmente, a folha de cálculo do preço de exportação e importação com preços personalizados.

## <a name="vm-images"></a>Imagens de VM

A secção seguinte para concluir será a secção de imagens de VM. Antes de ir para nesta secção, tem de ter o VHD que pretende publicar pronto. Aqui estão alguns links, ajudando-o a criar o VHD:

-   [Pré-requisitos técnicos para a criação de uma imagem de máquina virtual para o Azure Marketplace](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation-prerequisites)

-   [Criar e carregar um VHD do Linux](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-create-upload-generic?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

-   [Criar e testar uma VM do Linux a partir de uma imagem](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-upload-vhd)

-   [Criar e carregar um VHD do Windows ](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-prepare-for-upload-vhd-image?toc=/azure/virtual-machines/windows/toc.json)

-   [Criar e testar uma VM do Windows a partir de uma imagem](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-create-vm-generalized-managed?toc=/azure/virtual-machines/windows/toc.json)

-   [Como resolver problemas comuns encontrados durante a criação do VHD](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation-troubleshooting)

Assim que tiver o VHD estiver pronto, pode começar a preencher nesta secção.
Aqui estão alguns detalhes para alguns dos campos.

### <a name="recommended-vm-sizes"></a>Recomendado tamanhos de VM

Selecione até seis tamanhos de máquinas virtuais recomendados. Estas são recomendações apresentadas para o cliente no Azure Marketplace e o painel de escalão de preço no Portal do Azure quando este decide comprar e implementar a sua imagem. **Estas são apenas recomendações. O cliente é capaz de selecionar qualquer tamanho VM que englobe os discos especificados na sua imagem.**  Captura de ecrã seguinte mostra os tamanhos VM recomendados que os utilizadores verão num cliente no Portal do Azure.


![Tamanhos de VM recomendados](./media/cloud-partner-portal-publish-virtual-machine/publishvm9.png)


### <a name="open-ports"></a>Portas abertas

Especifica as portas que gostaria de ter feitas aberta e disponível. Estas portas são abertas durante esta implementação de VMs.

### <a name="adding-vm-images"></a>Adicionar imagens de VM

A próxima etapa é adicionar uma imagem de VM para o SKU. Pode adicionar até 8 versões de disco por SKU. Apenas o maior disco número da versão para uma SKU particular aparecerá no Azure Marketplace. Outros serão visíveis através de APIs.

Sob **versão de disco**, selecione **+ nova versão**. Isto mostra os seguintes campos que precisa para preencher.

#### <a name="vm-image-version"></a>Versão da imagem VM

A versão da imagem VM tem de cumprir os [versão semântica](http://semver.org/) formato. Versões devem ter o formato x.y. z, em que X, Y e Z são números inteiros. Imagens em SKUs diferentes podem ter diferentes versões principais e secundárias. Versões dentro de um SKU só devem ser as alterações incrementais, aumentam a versão de patch (Z do x.y. z).

#### <a name="os-vhd-url"></a>URL DE VHD DO SO

Introduza o [URI de assinatura de acesso partilhado](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#52-get-the-shared-access-signature-uri-for-your-vm-images) criado para o sistema de operativo VHD.

Se existirem discos de dados associados este SKU, pode optar por adicionar estes discos, selecionando o **+ novo disco de dados** ligação. Esta ação apresenta os campos adicionais para que possa preencher.

#### <a name="lun-vhd-url"></a>URL DE VHD LUN

Introduza o [URI de assinatura de acesso partilhado](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#52-get-the-shared-access-signature-uri-for-your-vm-images) para o seu disco de dados.

#### <a name="lun-number"></a>Número do LUN

Atribua este LUN um número. Esse número irá ser reservado para este disco de dados neste SKU.

>[!Note]
>Depois de publicar um SKU com uma determinada versão VM e os discos de dados, estas fica bloqueadas e não podem ser alteradas. Para as versões VM adicionais que são adicionadas ao SKU, não é possível alterar o número de discos de dados que tem de suportar.

#### <a name="common-sas-url-issues--fixes"></a>URL de SAS problemas comuns & de correções

| Problema                                                                 | Mensagem                                                                           | Corrigir                                                           |  Ligação para a documentação                                                                                |
|---------------------------------------------------------------------  |-------------------------------------------------------------------------------    |-----------------------------------------------------------    |---------------------------------------------------------------------------------------------------    |
| Falha na cópia dos imagens - "?" não for encontrada no url SAS                | Falha: Copiar as imagens. Não é possível transferir o blob com fornecida a Uri de SAS.       | Atualizar o Url de SAS com recomendado de ferramentas                    | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |
| Imagens de falha em Copiar - parâmetros "st" e "se" não está no url de SAS   | Falha: Copiar as imagens. Não é possível transferir o blob com fornecida a Uri de SAS.        | Atualize o Url de SAS com datas de início e fim no mesmo             | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |
| Falha na cópia dos imagens - "sp = rl" não está no url de SAS                    | Falha: Copiar as imagens. Não é possível transferir o blob com fornecida a Uri de SAS         | Atualizar o Url de SAS com as permissões definidas como "Leitura" & "List     | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |
| Falha na cópia dos imagens - url de SAS tem espaços em branco no nome do vhd     | Falha: Copiar as imagens. Não é possível transferir o blob com fornecida a Uri de SAS.        | Atualizar o Url de SAS, sem espaços em branco                       | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |
| Falha na cópia dos imagens – erro de autorização de Url de SAS               | Falha: Copiar as imagens. Não é possível transferir blob devido a erro de autorização     | Voltar a gerar o Url de SAS                                        | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |


## <a name="to-configure-the-marketplace"></a>Para configurar o Marketplace

Utilize a vista do Marketplace para configurar os campos que são apresentados para a oferta no [do Azure Marketplace](https://azuremarketplace.microsoft.com) e, no [Portal do Azure](https://portal.azure.com/).

### <a name="preview-subscription-ids"></a>Ids de subscrição de pré-visualização

A lista de IDs de subscrição do Azure que gostaria de ter acesso para a oferta quando é publicada a oferta. Estas subscrições listadas em branco lhe permitem testar a oferta visualizada antes de ficar em direto. O portal de parceiros permite-lhe a lista branca até 100 subscrições.

### <a name="suggested-categories"></a>Categorias sugeridas

Selecione até 5 categorias na lista fornecida que sua oferta pode ser melhor associada. Categorias selecionadas serão utilizadas para mapear a sua oferta para as categorias de produtos disponíveis no [do Azure Marketplace](https://azuremarketplace.microsoft.com) e [Portal do Azure](https://portal.azure.com/).

Os exemplos seguintes mostram a informação do marketplace no Azure Marketplace e o Portal do Azure.

**O Azure Marketplace**


![publishvm10](./media/cloud-partner-portal-publish-virtual-machine/publishvm10.png)


![publishvm11](./media/cloud-partner-portal-publish-virtual-machine/publishvm11.png)


![publishvm15](./media/cloud-partner-portal-publish-virtual-machine/publishvm15.png)


**Portal do Azure**


![publishvm12](./media/cloud-partner-portal-publish-virtual-machine/publishvm12.png)



![publishvm13](./media/cloud-partner-portal-publish-virtual-machine/publishvm13.png)


### <a name="logo-guidelines"></a>Diretrizes de logótipo

Siga estas diretrizes para logótipos carregados para o Portal de parceiro de Cloud:

-   O design do Azure tem uma paleta de cores simples. Mantenha o número de principal e secundário cores seu logótipo baixa.

-   As cores de tema do portal do Azure são brancas e preto. Evite utilizar essas cores como a cor de fundo de suas logótipos. Utilize uma cor que tornaria os logótipos proeminente no portal do Azure. Recomendamos cores primárias simples.

    >[!Note] 
    >Se estiver a utilizar um plano de fundo transparente, em seguida, certifique-se de que o texto/logótipos não são branco, preto ou azul.

-   Não utilize um fundo de gradação no logótipo.

-   Evite colocar texto no logótipo. Isto inclui a sua empresa ou o nome da marca. Deve ser o aspeto e funcionalidade do seu logótipo *simples* e deve evitar gradientes.

-   O logótipo não deve ser ampliado.

#### <a name="hero-logo"></a>Logótipo de Hero

O logótipo de Hero é opcional. O publicador pode optar por não carregue um logótipo de Hero. No entanto, após o carregamento, o logótipo não pode ser eliminada. O parceiro tem de seguir as diretrizes do Azure Marketplace para os ícones de Hero.

#### <a name="guidelines-for-the-hero-logo-icon"></a>Diretrizes para o ícone de logótipo de Hero

-   O nome de exibição do publicador, título de plano e resumo completo da oferta são exibidos usando uma fonte colorida branca. Evite usar qualquer cor leve em segundo plano. Preto, planos de fundo brancas e transparentes não são permitidos para os ícones de Hero.

-   Nome de exibição publicador, título, a oferta resumo há muito tempo e no botão Criar incorporados por meio de programação dentro o logótipo de Hero quando listados da oferta de plano. Não introduza qualquer texto quando estiver a conceber o logótipo de Hero. Deixe um espaço em branco à direita do logótipo de. Este espaço deve ser 415 x 100 pixéis e é contrabalançado por 370 px da esquerda.

![Exemplo de logótipo de destaque](./media/cloud-partner-portal-publish-virtual-machine/publishvm14.png)

### <a name="lead-management"></a>Numa gestão

Para configurar as definições de gestão de oportunidades potenciais da oferta, siga [estas instruções](./cloud-partner-portal-get-customer-leads.md).

## <a name="to-configure-support"></a>Configurar o suporte

Utilize a vista de suporte para fornecer as seguintes informações:

- Contactos de sua empresa, como a engenharia de suporte.
- Contactos de suporte ao cliente.

## <a name="to-publish-the-offer"></a>Para publicar a oferta

A etapa final é publicar a oferta. Siga [estas instruções para a ativação online com a oferta](./cloud-partner-portal-make-offer-live-on-azure-marketplace.md).
