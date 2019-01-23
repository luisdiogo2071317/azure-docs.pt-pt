---
title: Publicar uma aplicação gerida do Azure para o Azure Marketplace
description: Publicar uma aplicação gerida do Azure para o Azure Marketplace
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: qianw211
manager: pbutlerm
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 6e5bcd6d9923b4051d44d51ff1a2534bc5e02f41
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54445665"
---
<a name="publish-an-azure-managed-application-to-azure-marketplace"></a>Publicar uma aplicação gerida pelo Azure para o Azure Marketplace 
========================================================

Este artigo lista as diversas etapas envolvidas na publicação de uma oferta de aplicação gerida no Azure Marketplace.

<a name="pre-requisites-for-publishing-a-managed-application"></a>Pré-requisitos para a publicação de uma aplicação gerida 
---------------------------------------------------

Pré-requisitos para a listagem no Azure Marketplace

1.  Técnico

    -   [Criar modelos do Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)

    -   Modelos de início rápido do Azure:

        -   <https://azure.microsoft.com/documentation/templates/>

        -   <https://github.com/azure/azure-quickstart-templates>

    -   Criar definição de interface do Usuário

        -   [Criar ficheiro de definição de interface de utilizador](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview)

2.  Não técnicas (requisitos de negócios)

    -   A sua empresa (ou respetiva subsidiária) tem de estar localizada num país de origem de venda suportado pelo Azure Marketplace

    -   Seu produto têm de estar licenciado de forma que seja compatível com modelos de faturação suportados pelo Azure Marketplace

    -   É responsável por fornecer suporte técnico para os seus clientes: gratuito, pago ou por meio do suporte da Comunidade.

    -   É responsável por licenciamento seu software e dependências no software de terceiros.

    -   Tem de fornecer conteúdo que cumpre os critérios para a sua oferta seja incluída no Azure Marketplace e no Portal de gestão do Azure

    -   Tem de concordar com os termos do contrato de publicador e políticas de participação do Azure Marketplace

    -   Tem de concordar com estar em conformidade com os termos de utilização, declaração de privacidade da Microsoft e contrato de programa de certificação do Microsoft Azure.

<a name="how-to-create-a-new-azure-application-offer"></a>Como criar uma nova oferta de aplicação do Azure 
-------------------------------------------

Depois de todos os pré-requisitos tiverem sido cumpridos, está pronto para começar a criar a sua oferta de aplicação gerida. Antes do começamos, uma visão geral de uma oferta e um SKU

**Oferta**

Uma oferta de aplicação do Azure corresponde a uma classe de produto, oferta do publicador. Se tiver uma nova solução/aplicação para ser publicado no Azure Marketplace, uma nova oferta é a melhor opção. Uma oferta é uma coleção de SKUs. Cada oferta é apresentada como uma entidade própria no Azure Marketplace.

**SKU**

Um SKU é uma oferta com a menor unidade que podem ser comprada. Enquanto na mesma classe de produto (oferta), o SKUs permitem-lhe distinguir entre diferentes funcionalidades suportadas, se a oferta é gerido ou não geridos e modelos de faturação.

Adicionar várias SKUs, se desejar oferecer suporte a diferentes modelos de faturação: como traga a sua própria licença (BYOL), Pay as you Go (PAYG), etc. 

Adicione várias SKUs quando cada SKU suporta uma funcionalidade diferente, defina e cobradas de forma diferente.

Um SKU aparece sob a oferta principal no Azure Marketplace enquanto ele aparece como sua própria entidade que podem ser comprada em azure.com.

1.  Inicie sessão para o [Portal de parceiros da Cloud](http://cloudpartner.azure.com).

2.  Na barra de navegação esquerdo, clique em \"+ nova oferta\" e selecione \"aplicativos do Azure\".

    ![Nova oferta](./media/cloud-partner-portal-publish-managed-app/newOffer.png)

3.  Uma nova oferta \"Editor\" Vista agora é aberta e pode começar a criar.

4.  O \"formulários\" que têm de ser preenchidos são visíveis no lado esquerdo dentro os \"Editor\" vista. Cada \"formulário\" consiste num conjunto de campos que devem ser preenchidos. Necessário campos são marcados com um asterisco vermelho (\*).

    > Existem 4 formulários principais para a criação de uma aplicação gerida

    -   Definições da oferta

    -   SKUs

    -   Marketplace

    -   Suporte

<a name="how-to-fill-out-the-offer-settings-form"></a>Como preencher o formulário de definições da oferta 
---------------------------------------

O formulário de definições da oferta é um formulário básico para especificar as definições da oferta.
Os diferentes campos são descritos abaixo.

**ID de oferta**

Um identificador exclusivo para a oferta de um perfil do publicador.
Ela estará visível no produto URLs, modelos do Resource Manager, e relatórios de faturação. Minúscula apenas carateres alfanuméricos ou são permitidos travessões (-). Ela não pode terminar com um travessão ou ter mais de um máximo de 50 carateres. Este campo está bloqueado assim que uma oferta entra no ar.

**ID de publicador**

Essa área suspensa permite-lhe escolher o perfil do publicador que pretende publicar esta oferta em. Este campo está bloqueado assim que uma oferta entra no ar.

**Nome**

O nome a apresentar para a sua oferta. O nome que aparecerá no Azure Marketplace e no portal do Azure. Pode ter um máximo de 50 carateres. Documentação de orientação aqui é incluir um nome de marca reconhecível para o seu produto. Não inclua o nome da empresa aqui, a menos que o que é a forma como é comercializada. Se estiver marketing esta oferta em seu próprio Web site, certifique-se de que o nome é exatamente como ele aparece no seu Web site.

Clique em \"guardar\" para guardar o seu progresso. Próxima etapa seria adicionar SKUs para a sua oferta.

<a name="how-to-create-skus"></a>Como criar SKUs 
------------------

Clique nas \"SKUs\" formulário. Aqui pode ver uma opção para \"adicione um SKU\" clicar em que irá permitir-lhe para introduzir uma \"ID do SKU\".

![nova oferta SKUs](./media/cloud-partner-portal-publish-managed-app/newOffer_skus.png)

\"ID do SKU\" é um identificador exclusivo para o SKU dentro de uma oferta. Este ID será visível no produto URLs, modelos ARM, e relatórios de faturação. Apenas pode ser constituído por carateres alfanuméricos em minúsculas ou hífenes (-).
Ele não pode terminar com um travessão e pode ter um máximo de 50 carateres. Este campo está bloqueado assim que uma oferta entra no ar. Pode ter vários SKUs numa oferta. Precisa de um SKU para cada imagem que pretende publicar.

Assim que tiver sido adicionado um SKU, irá aparecer na lista de SKUs dentro de \"SKUs\" formulário. Clique no nome do SKU para obter os detalhes de SKU em questão específica. Aqui estão alguns detalhes para alguns dos campos.

Depois de clicar no \"novo SKU\", terá de preencher o formulário seguinte:

![Nova oferta - novo SKU](./media/cloud-partner-portal-publish-managed-app/newOffer_newsku.png)

### <a name="how-to-fill-sku-details-section"></a>Como preencher a secção de detalhes de Sku 

**Título** -fornece um título para este Sku. Este é o que irá aparecer na Galeria para este item.

**Resumo** -fornece um breve resumo para este sku. Este texto irá aparecer diretamente sob o título.

**Descrição** -fornece uma descrição detalhada sobre o SKU.

**Tipo de SKU** -os valores permitidos são \"Managed Application\" e \"modelos de soluções\". Para esse caso, selecionará \"Managed Application\".

### <a name="how-to-fill-package-details-section"></a>Como preencher a secção de detalhes do pacote 

A secção de pacote tem os seguintes campos que têm de ser preenchidos

![Nova oferta - novo pacote SKU](./media/cloud-partner-portal-publish-managed-app/newOffer_newsku_package.png)

**Versão atual** -fornece uma versão para o pacote irá carregar.
Ele deve estar no formato.

**Ficheiro do pacote** -o pacote contém os seguintes ficheiros que são compactados num arquivo. zip.

applianceMainTemplate.json - o ficheiro de modelo de implementação que é utilizado para implementar a solução/aplicação e criar os recursos definidos no mesmo. Pode encontrar mais detalhes sobre como arquivos de modelo de implementação de autor aqui- <https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template>

applianceCreateUIDefinition.json - este ficheiro é utilizado pelo Portal no azure.com para gerar a interface do usuário para o aprovisionamento desta solução/aplicação. Pode encontrar mais detalhes sobre como criar este arquivo aqui- <https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview>

maintemplate. JSON - o ficheiro de modelo que contém apenas o recurso de Microsoft.Solution/appliances. As propriedades chave deste recurso ter em consideração são os seguintes:

-   \"tipo\" -o valor deve ser \"Marketplace\" no caso de cenário de aplicação gerida por mercado
-   \"ManagedResourceGroupId\": o grupo de recursos no cliente\'subscrição s onde todos os recursos definidos no applianceMainTemplate.json serão implementados.
-   \"PublisherPackageId\": a cadeia de caracteres que identifica exclusivamente o pacote. 

O valor tem de ser construída da seguinte forma - é uma concatenação das \[publisherId\].\[ OfferId\]-pré-visualização\[SKUID\].\[ PackageVersion\].
Cada um destes valores pode ser obtida como mostrado abaixo.

Este pacote deve conter todos os outros modelos aninhados ou scripts necessários para aprovisionar com êxito esta aplicação. O maintemplate. JSON, applianceMainTemplate.json e applianceCreateUIDefinition.json tem de estar presentes na pasta raiz.

**As autorizações** -esta propriedade define que dispõe do acesso e o nível de acesso aos recursos em subscrições de clientes. Isto permite que o publicador gerir a aplicação em nome do cliente.

-   PrincipalId - o identificador do Azure Active directory de um utilizador, o grupo de utilizadores ou a aplicação que vai ser concedida determinadas permissões (conforme descrito pela definição de função) nos recursos na subscrição de cliente.

-   Definição de função - uma lista de todas as funções RBAC incorporadas fornecidas pelo Azure AD. Pode selecionar a função mais apropriada, que irá permitir-lhe gerir os recursos em nome do cliente.

Podem ser adicionadas vários autorizações. No entanto, é recomendado para criar um grupo de utilizadores do Active Directory e especificar sua ID na \"PrincipalId\."  Isto irá permitir a adição de mais usuários ao grupo de utilizadores sem ter de atualizar o SKU.

Obter mais detalhes sobre o RBAC podem ser encontrados aqui- <https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is>

<a name="marketplace-form"></a>Formulário do Marketplace
----------------

O formulário de marketplace dentro de uma oferta de aplicação do azure pede-lhe campos que serão apresentados na [do Azure Marketplace](https://azuremarketplace.microsoft.com) e, no [Portal do Azure](https://portal.azure.com/). Aqui estão alguns detalhes para alguns dos campos.

#### <a name="preview-subscription-ids"></a>IDs de subscrição de pré-visualização

Introduza aqui uma lista de IDs de subscrição do Azure que gostaria de ter acesso para a oferta assim que for publicado. Estas subscrições listadas em branco, poderá testar a oferta visualizada antes de ficar em direto. O portal de parceiros permite-lhe a lista aprovada até 100 subscrições.

#### <a name="suggested-categories"></a>Categorias sugeridas

Selecione até cinco categorias na lista fornecida que sua oferta pode ser melhor associada. Categorias selecionadas serão utilizadas para mapear a sua oferta para as categorias de produtos disponíveis no [do Azure Marketplace](https://azuremarketplace.microsoft.com) e [Portal do Azure](https://portal.azure.com/).

Aqui estão alguns dos locais que os dados fornecidas neste formulário é exibido na.

##### <a name="azure-marketplace"></a>Azure Marketplace

![publishvm10](./media/cloud-partner-portal-publish-managed-app/publishvm10.png)

![publishvm11](./media/cloud-partner-portal-publish-managed-app/publishvm11.png)

![publishvm15](./media/cloud-partner-portal-publish-managed-app/publishvm15.png)

##### <a name="portal-at-azurecom"></a>Portal at azure.com

![publishvm12](./media/cloud-partner-portal-publish-managed-app/publishvm12.png)

![publishvm13](./media/cloud-partner-portal-publish-managed-app/publishvm13.png)

##### <a name="logo-guidelines"></a>Diretrizes de logótipo

Todos os logótipos carregados no Portal de parceiros de nuvem devem seguir as diretrizes abaixo:

-   O design do Azure tem uma paleta de cores simples. Mantenha o número de principal e secundário cores seu logótipo baixa.

-   As cores de tema do portal em azure.com são brancas e preto. Evite utilizar essas cores como a cor de fundo de suas logótipos. Utilize uma cor que faria os logótipos proeminente no portal em azure.com.
    Recomendamos cores primárias simples. **Se estiver a utilizar o fundo transparente, em seguida, certifique-se de que o logótipos e o texto não são azul, preto ou branco.**

-   Não utilize um plano de fundo de gradação o logótipo.

-   Evite colocar texto, até mesmo a sua empresa, ou sua marca nome, o logótipo.
    Deve ser o aspeto e funcionalidade do seu logótipo \'simples\' e deve evitar gradientes.

-   Evite alongamento o logótipo.

##### <a name="hero-logo"></a>Logótipo de Hero

O logótipo de Hero é opcional. O publicador pode optar por não carregue um logótipo de Hero. No entanto uma vez carregado não é possível eliminar o ícone de hero. Nesse caso, o parceiro tem de seguir as diretrizes do Azure Marketplace para os ícones de Hero.

###### <a name="guidelines-for-the-hero-logo-icon"></a>Diretrizes para o ícone de logótipo de Hero

-   O nome de exibição do publicador, título de plano e a oferta longa resumo são apresentados na cor da fonte branco. Evite manter qualquer cor leve no plano de fundo do ícone Hero. Planos de fundo transparentes, pretas e brancas não são permitidos para os ícones de Hero.

-   Quando a oferta é listada, o nome de exibição do publicador, título do plano, oferta resumo há muito tempo e no botão Criar programaticamente incorporados dentro o logótipo de Hero. Não precisa de introduzir qualquer texto durante a conceção o logótipo de Hero. Deixe espaços vazios no lado direito para o publicador apresentam o nome, a título de plano, o resumo extenso da oferta e etc. Eles estão incluídos por meio de programação.
    Esvaziar espaços para o texto deve ser 415 x 100 à direita e compensada pela 370 px da esquerda.

![publishvm14](./media/cloud-partner-portal-publish-managed-app/publishvm14.png)

<a name="support-form"></a>Formulário de suporte
------------

O formulário seguinte para preencher é a forma de suporte. Este formulário solicita contactos de suporte da sua empresa.  Alguns exemplos são as suas informações de contacto de engenharia e informações de contacto de suporte ao cliente.

<a name="how-to-publish-an-offer"></a>Como publicar uma oferta
-----------------------

Agora que a oferta é escrita, a próxima etapa é publicar a oferta no Azure Marketplace. Clique nas \"publicar\" botão Iniciar o processo de tornar a sua oferta em direto.
