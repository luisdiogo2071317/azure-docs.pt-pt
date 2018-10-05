---
title: Publicar um modelo de solução | Documentos da Microsoft
description: Publicar um modelo de solução no Azure Marketplace.
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
ms.date: 09/17/2018
ms.author: pbutlerm
ms.openlocfilehash: ded952ac6418ae3d9916b3ae8b8dbacb0c9c5a84
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48810488"
---
# <a name="publish-a-solution-template-to-azure-marketplace"></a>Publicar um modelo de solução no Azure Marketplace

Este artigo fornece os passos para publicar um modelo de solução da oferta no Azure Marketplace.

## <a name="prerequisites"></a>Pré-requisitos

Os seguintes pré-requisitos de técnicos e aplicam-se para a listagem de um modelo de solução no Azure Marketplace.

### <a name="technical"></a>Técnico

- [Compreender os modelos Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)

- Modelos de início rápido do Azure:

    - [Documentação do modelo de início rápido do Azure](https://azure.microsoft.com/documentation/templates/)

    - [Documentação de início rápido do Azure no GitHub](https://github.com/azure/azure-quickstart-templates)

 - [Criar ficheiro de interface de utilizador do portal do Azure](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview)

### <a name="non-technical-business-requirements"></a>Não técnicas (requisitos de negócios)

-   A sua empresa (ou respetiva subsidiária) tem de estar localizada num destino de venda do país suportado pelo Azure Marketplace.

-   Seu produto têm de estar licenciado de forma que seja compatível com modelos de faturação suportados pelo Azure Marketplace.

-   É responsável para disponibilizar suporte técnico aos clientes de forma que sejam comercialmente razoável, quer gratuito, pago ou através de suporte da Comunidade.

-   É responsável pelo licenciamento do software e as dependências de software de terceiros.

-   Fornece conteúdo de que cumpre os critérios para a sua oferta seja incluída no Azure Marketplace e no Portal de gestão do Azure.

-   Aceite os termos do contrato de publicador e políticas de participação do Azure Marketplace.

-   Aceite cumprir Termos de Utilização, a Declaração de Privacidade da Microsoft e o Contrato do Programa Microsoft Azure Certified.

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

1.  Inicie sessão para o [Portal de parceiros da Cloud](http://cloudpartner.azure.com/).

2.  Na barra de navegação esquerdo, selecione **+ nova oferta**e, em seguida, selecione **aplicativos do Azure**.

    ![Criar uma nova oferta](./media/cloud-partner-portal-publish-managed-app/newOffer.png)

3.  Sob **nova oferta**, selecione **Editor**.

    ![Novo editor de oferta](./media/cloud-partner-portal-publish-managed-app/newOffer_OfferSettings.png)

4.  Sob **Editor**, irá fornecer informações nas seguintes vistas:
    - Definições da oferta
    - SKUs
    - Marketplace
    - Suporte a que cada vista contém um conjunto de campos para que possa preencher. Necessário campos são indicados com um asterisco vermelho (\*)

## <a name="to-configure-offer-settings"></a>Para configurar as definições da oferta

1. Configure as seguintes opções **oferecer identidade** campos nas definições da oferta.

    **ID de oferta**

     Um identificador exclusivo para a oferta de um perfil do publicador. Este ID está visível no produto URLs, modelos ARM, e relatórios de faturação. Pode utilizar apenas carateres alfanuméricos em minúsculas ou traços (-). O ID não pode terminar com um travessão e pode ter um máximo de 50 carateres. 
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
para a sua oferta.

## <a name="to-create-skus"></a>Para criar SKUs
------------------

1. Selecione **SKUs**. 

    ![Novo SKU](./media/cloud-partner-portal-publish-managed-app/newOffer_skus.png)

    ID do SKU é um identificador exclusivo para o SKU dentro de uma oferta. Este ID está visível no produto URLs, modelos ARM, e relatórios de faturação. O ID de SKU:
    - Só pode ter um máximo de 50 carateres.
    - Só pode ser composto por carateres alfanuméricos em minúsculas ou traços (-).
    - O ID não pode terminar com um hífen.

    >[!Note]
    >Depois de um SKU é adicionado, aparece na lista de SKUs na vista de SKUs. Para ver os detalhes SKU, selecione o nome SKU. 

2. Selecione **novo SKU** para fornecer as informações mostradas na captura de ecrã seguinte. 

    ![Detalhes SKU](./media/cloud-partner-portal-publish-managed-app/newOffer_newsku.png)


### <a name="sku-settings"></a>Definições de SKU

Forneça as seguintes definições de SKU.

- **Título** -um título para o SKU. Este título é apresentado na Galeria para este item.

- **Resumo** - uma breve descrição de resumo do SKU. (O comprimento máximo é 100 carateres.)

- **Descrição** - uma descrição do SKU de detalhadas.

- **Tipo de SKU** -uma lista suspensa com estes valores: "Aplicação de gerida (pré-visualização)" e "Modelo de solução". Para este cenário, selecione **modelo de solução**.

- **Disponibilidade da cloud** -a localização do SKU. A predefinição é público do Azure.

### <a name="package-details"></a>Detalhes do pacote

Depois de concluir as definições de SKU, forneça os seguintes detalhes do pacote.

![Detalhes do pacote](./media/cloud-partner-portal-publish-managed-app/newOffer_newsku_ST_package.png)

- **Versão atual** -a versão do pacote que irá carregar. Deve estar no formato:....

- **Ficheiro do pacote** -este pacote contém os seguintes arquivos, que são salvas num arquivo. zip.

    -   Maintemplate. JSON - o ficheiro de modelo de implementação que é utilizado para implementar a solução/aplicação e criar os recursos definidos para a solução. Para obter mais informações, consulte [como criar arquivos de modelo de implementação](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template)

    -   Createuidefinition - este ficheiro é utilizado pelo Portal do Azure para gerar a interface do usuário para o aprovisionamento desta solução/aplicação. Para obter mais informações, consulte [interface de utilizador do portal do Azure criar para a aplicação gerida](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview)

    >[!IMPORTANT]
    >Este pacote deve conter quaisquer outros modelos aninhados ou scripts que são necessários para aprovisionar esta aplicação. O maintemplate. JSON e Createuidefinition tem de ser na pasta raiz.

## <a name="to-configure-the-marketplace"></a>Para configurar o Marketplace

Utilize a vista do Marketplace para configurar os campos que são apresentados para a oferta no [do Azure Marketplace](https://azuremarketplace.microsoft.com) e, no [Portal do Azure](https://portal.azure.com/).

### <a name="preview-subscription-ids"></a>Ids de subscrição de pré-visualização

A lista de IDs de subscrição do Azure que gostaria de ter acesso para a oferta quando é publicada a oferta. Estas subscrições listadas em branco lhe permitem testar a oferta visualizada antes de ficar em direto. O portal de parceiros permite-lhe a lista branca até 100 subscrições.

### <a name="suggested-categories"></a>Categorias sugeridas

Selecione até 5 categorias na lista fornecida que sua oferta pode ser melhor associada. Categorias selecionadas serão utilizadas para mapear a sua oferta para as categorias de produtos disponíveis no [do Azure Marketplace](https://azuremarketplace.microsoft.com) e [Portal do Azure](https://portal.azure.com/).

Os exemplos seguintes mostram a informação do marketplace no Azure Marketplace e o Portal do Azure.

**O Azure Marketplace**

![publishvm10](./media/cloud-partner-portal-publish-managed-app/publishvm10.png)


![publishvm11](./media/cloud-partner-portal-publish-managed-app/publishvm11.png)


![publishvm15](./media/cloud-partner-portal-publish-managed-app/publishvm15.png)


**Portal do Azure**


![publishvm12](./media/cloud-partner-portal-publish-managed-app/publishvm12.png)


![publishvm13](./media/cloud-partner-portal-publish-managed-app/publishvm13.png)


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

![publishvm14](./media/cloud-partner-portal-publish-managed-app/publishvm14.png)

## <a name="to-configure-support"></a>Configurar o suporte

Utilize a vista de suporte para fornecer as seguintes informações:

- Contactos de sua empresa, como a engenharia de suporte.
- Contactos de suporte ao cliente.

## <a name="to-publish-the-offer"></a>Para publicar a oferta

A etapa final é publicar a oferta. Selecione **Publicar**.
