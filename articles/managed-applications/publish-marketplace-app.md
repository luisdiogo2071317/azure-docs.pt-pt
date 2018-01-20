---
title: "As aplicações no mercado de gerido do Azure | Microsoft Docs"
description: "Descreve Azure as aplicações que estão disponíveis através do Marketplace gerido."
services: azure-resource-manager
author: tfitzmac
manager: timlt
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/18/2018
ms.author: tomfitz
ms.openlocfilehash: fccc2dbb7623f4ceb0d3decc7037f75a05858910
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="azure-managed-applications-in-the-marketplace"></a>Aplicações geridas do Azure no Marketplace

Os fornecedores podem utilizar o Azure geridos aplicações para oferecer as soluções para todos os clientes do Azure Marketplace. Os fornecedores podem incluir fornecedores de serviços geridos (MSPs), os fabricantes independentes de software (ISV) e integradores de sistema (SIs). Aplicações geridas reduzem a manutenção e a sobrecarga de manutenção para os clientes. Os fornecedores propor infraestrutura e de software através do marketplace. Pode anexar os serviços e operacional suporte para aplicações geridas. Para obter mais informações, consulte [descrição geral de aplicações gerido](overview.md).

Este artigo explica como pode publicar uma aplicação no Marketplace e que fiquem amplamente disponíveis aos clientes.

## <a name="prerequisites-for-publishing-a-managed-application"></a>Pré-requisitos para publicar uma aplicação gerida

Para concluir este artigo, já tem de ter o ficheiro. zip para a sua definição de aplicações geridas. Para obter mais informações, consulte [Criar aplicação do catálogo de serviço](publish-service-catalog-app.md).

Além disso, existem vários pré-requisitos de negócio. São:

* Da sua empresa ou a sua subsidiária tem de estar localizada num país/região onde vendas são suportadas no marketplace.
* O produto têm de estar licenciado de uma forma que é compatível com modelos de faturação suportados no marketplace.
* Disponibilizar suporte técnico para clientes de forma comercialmente razoável. O suporte pode ser livre, paga, ou através da Comunidade suportar.
* Licença do software e as dependências de software de terceiros.
* Forneça o conteúdo que cumpra os critérios para a sua oferta seja apresentado no Marketplace e no portal do Azure.
* Aceite os termos do contrato do publicador e políticas de participação do Azure Marketplace.
* Concorda em estar em conformidade com os termos de utilização, declaração de privacidade da Microsoft e contrato de programa de certificados do Microsoft Azure.

## <a name="become-a-publisher"></a>Tornar-se de um fabricante

Para se tornar um publicador no Azure Marketplace, tem de:

1. Criar um ID de Microsoft - criar a sua conta Microsoft com um endereço de e-mail que pertence ao domínio da sua empresa, mas não para um único individuais. Este endereço de correio eletrónico é utilizado para a Microsoft Developer Center e o Portal de parceiros de nuvem. Para obter mais informações, consulte [guia de publicador do Azure Marketplace](https://aka.ms/sellerguide).
1. Submeter [Azure Marketplace Nomination formulário](https://aka.ms/ampnomination) - para **solução que pretende publicar?**, selecione **aplicações geridas**. Depois do formulário é submetido, a equipa de integração de Marketplace revê a aplicação e valida o pedido. O processo de aprovação pode demorar uma a três dias. Quando o seu nomination é aprovado, receberá um código promocional para waive a taxa de registo para o Centro de programadores. Se o fizer **não** preencher o formulário de Nomination do Marketplace, é-lhe perguntado aos quais deve prestar uma taxa de registo $99.
1. Registar no [Centro para programadores](https://developer.microsoft.com) -Microsoft valida que a sua organização é uma entidade legal válida com um ID de dedução dos impostos válido para o país/região na qual está registado. O processo de aprovação pode demorar 5 a 10 dias. Para evitar a taxa de registo, utilize o código promocional que recebeu por correio eletrónico do processo de nomination. Para obter mais informações, consulte [guia de publicador do Azure Marketplace](https://aka.ms/sellerguide).
1. Inicie sessão no [Portal de parceiros de nuvem](https://cloudpartner.azure.com) - no perfil do publicador, associar a sua conta do Centro de programadores com o perfil de publicador do Marketplace. Para obter mais informações, consulte [guia de publicador do Azure Marketplace](https://aka.ms/sellerguide).

## <a name="create-a-new-azure-application-offer"></a>Criar uma nova oferta de aplicação do Azure

Depois de criar a sua conta do portal de parceiros, está pronto para criar a sua oferta da aplicação gerida.

### <a name="set-up-an-offer"></a>Configurar uma oferta

A oferta para uma aplicação gerida corresponde a uma classe de produto de um fabricante da oferta. Se tiver um novo tipo de aplicação que pretende tornar disponível no mercado, pode configurá-lo como uma nova oferta. Uma oferta é uma coleção de SKUs. Cada oferta aparece como o seu próprio entidade no marketplace.

1. Iniciar sessão para o [portal de parceiros de nuvem](https://cloudpartner.azure.com/).

1. No painel de navegação à esquerda, selecione **+ nova oferta** > **aplicações do Azure**.

1. No **Editor** vista, consulte os formulários necessários. Cada formulário é descrito mais à frente neste artigo.

## <a name="offer-settings-form"></a>Formulário de definições da oferta

Os campos para o **oferecem definições** formulário são:

* **Oferecer ID**: este identificador exclusivo identifica a oferta dentro de um perfil do publicador. Este ID é visível no produto URLs, modelos do Resource Manager, e relatórios de faturação. Só podem ser composta de carateres alfanuméricos minúsculos ou traços (-). O ID não pode terminar com um travessão. De limitada a um máximo de 50 carateres. Depois de uma oferta fica em direto, este campo está bloqueado.
* **ID de publicador**: Utilize esta lista pendente para escolher o perfil de publicador que pretende publicar esta oferta em. Depois de uma oferta fica em direto, este campo está bloqueado.
* **Nome**: este nome a apresentar para a sua oferta é apresentada no Marketplace e através do portal. Pode ter um máximo de 50 carateres. Incluem um nome de marca reconhecível para o produto. Não inclua o nome da sua empresa aqui, a menos que seja como for marketed. Se estiver a marketing esta oferta no seu próprio site, certifique-se de que o nome exatamente como é apresentado no seu Web site.

Quando terminar, selecione **guardar** para guardar o seu progresso.

## <a name="skus-form"></a>SKUs de formulário

O passo seguinte consiste em Adicionar SKUs para a sua oferta.

Um SKU é a unidade purchasable mais pequeno de uma oferta. Pode utilizar um SKU dentro da mesma classe de produto (oferta) para diferenciar entre:

* Funcionalidades diferentes que são suportadas
* Indica se a oferta é gerida ou não gerida
* Modelos de faturação que são suportados

Um SKU aparece sob a oferta de principal no marketplace. É apresentado como o seu próprio entidade purchasable no portal do Azure.

1. Selecione **SKUs** > **SKU novo**.

1. Introduza um **ID do SKU**. Um ID de SKU é um identificador exclusivo para o SKU dentro de uma oferta. Este ID é visível no produto URLs, modelos do Resource Manager, e relatórios de faturação. Só podem ser composta de carateres alfanuméricos minúsculos ou traços (-). O ID não pode terminar com um travessão e de limitada a um máximo de 50 carateres. Depois de uma oferta fica em direto, este campo está bloqueado. Pode ter vários SKUs dentro de uma oferta. É necessário um SKU de cada imagem que pretende publicar.

1. Preencha o **detalhes de SKU** secção no seguinte formato:

   Preencha os campos seguintes:

   * **Título**: introduza um título para este SKU. Este título é apresentado na Galeria para este item.
   * **Resumo**: introduza um breve resumo para esta SKU. Este texto é apresentado sob o título.
   * **Descrição**: introduza uma descrição detalhada sobre o SKU.
   * **Tipo de SKU**: os valores permitidos são *aplicações geridas* e *modelos de solução*. Neste caso, selecione *aplicações geridas*.
   * **Disponibilidade do país/região**: selecione países/regiões onde a aplicação gerida está disponível.
   * **Preços**: forneça um preço para a gestão da aplicação. Selecione os países disponíveis antes de definir o preço.

1. Adicione um novo pacote. Preencha o **detalhes do pacote** secção no seguinte formato:

   Preencha os campos seguintes:

   * **Versão atual**: introduza uma versão para o pacote que carrega. Deverá estar no formato `{number}.{number}.{number}{number}`.
   * **Selecione um ficheiro de pacote**: Este pacote contém dois ficheiros necessários comprimidos para um pacote. zip. Um ficheiro é um modelo do Resource Manager que define os recursos a implementar a aplicação gerida. O ficheiro de outro define o [interface de utilizador](create-uidefinition-overview.md) para implementar a aplicação gerida através do portal de consumidores. Na interface de utilizador, especifique os elementos que permitem que os consumidores fornecer valores de parâmetros.
   * **PrincipalId**: Esta propriedade é o identificador do Azure Active Directory (Azure AD) de um utilizador, o grupo de utilizadores ou a aplicação que é concedida acesso aos recursos na subscrição do cliente. A definição de função descreve as permissões.
   * **Definição de função**: Esta propriedade é uma lista de todos os as controlo de acesso baseado em funções (RBAC) funções incorporadas fornecidas pelo Azure AD. Pode selecionar a função que é mais adequada utilizar para gerir os recursos em nome do cliente.

Pode adicionar vários autorizações. Recomendamos que crie um grupo de utilizadores do AD e especificar o respetivo ID em **PrincipalId**. Desta forma, pode adicionar mais utilizadores para o grupo de utilizadores sem a necessidade de atualizar o SKU.

Para obter mais informações sobre o RBAC, consulte [começar a utilizar o RBAC no portal do Azure](../active-directory/role-based-access-control-what-is.md).

## <a name="marketplace-form"></a>Formulário do Marketplace

O formulário de Marketplace pede-lhe campos que apareçam no [Azure Marketplace](https://azuremarketplace.microsoft.com) e o [portal do Azure](https://portal.azure.com/).

### <a name="preview-subscription-ids"></a>IDs de subscrição de pré-visualização

Introduza uma lista de IDs, que pode aceder à oferta depois da sua publicação de subscrição do Azure. Pode utilizar estas subscrições listados em branco para testar a oferta previewed antes de torná-lo em direto. Pode compilar uma lista de permissão de até 100 subscrições no portal de parceiros.

### <a name="suggested-categories"></a>Categorias sugeridas

Selecione até cinco categorias da lista que oferta pode ser melhor associada. Estas categorias são utilizadas para mapear a oferta para as categorias de produtos que estão disponíveis no [Azure Marketplace](https://azuremarketplace.microsoft.com) e [portal do Azure](https://portal.azure.com/).

#### <a name="azure-marketplace"></a>Azure Marketplace

O resumo da sua aplicação gerida apresenta os seguintes campos:

![Resumo do Marketplace](./media/publish-marketplace-app/publishvm10.png)

O **descrição geral** separador para a sua aplicação gerida apresenta os seguintes campos:

![Descrição geral do Marketplace](./media/publish-marketplace-app/publishvm11.png)

O **planos + preços** separador para a sua aplicação gerida apresenta os seguintes campos:

![Planos de Marketplace](./media/publish-marketplace-app/publishvm15.png)

#### <a name="azure-portal"></a>Portal do Azure

O resumo da sua aplicação gerida apresenta os seguintes campos:

![Resumo do portal](./media/publish-marketplace-app/publishvm12.png)

A descrição geral da sua aplicação gerida apresenta os seguintes campos:

![Descrição geral do portal](./media/publish-marketplace-app/publishvm13.png)

#### <a name="logo-guidelines"></a>Diretrizes de logótipo

Siga estas diretrizes para qualquer logótipo que carregar no portal de parceiros de Cloud:

*   A estrutura do Azure tem uma paleta de cores simples. Limite o número de principal e secundário cores no seu logótipo.
*   As cores de tema do portal são brancos e marcar a negrito. Não utilize estes cores como a cor de fundo para o logótipo. Utilize uma cor que faz com que o seu logótipo prominent no portal. Recomendamos simples de principal de cores. *Se utilizar um fundo transparente, certifique-se de que o logótipo e o texto não são brancos, negra ou azul.*
*   Não utilize um fundo gradação no logótipo.
*   Não coloque o texto no logótipo, nem mesmo a empresa ou o nome de marca. O aspeto e funcionalidade do logótipo deve ser simples e evitar gradações.
*   Certifique-se que o logótipo não se encontra stretched (ampliada).

#### <a name="hero-logo"></a>Logótipo de heroína

O logótipo heroína é opcional. O fabricante pode optar por não carregar um logótipo de heroína. Depois do ícone de heroína é carregado, não pode ser eliminada. Nessa altura, o parceiro tem de seguir as diretrizes de Marketplace para ícones heroína.

Siga estas diretrizes para o ícone de logótipo heroína:

*   O nome de apresentação do publicador, o título do plano e a oferta longa resumo são apresentados em branco. Por conseguinte, não utilize uma cor leve para a imagem de fundo do ícone heroína. Não é permitido um fundo branco, preto ou transparente para ícones heroína.
*   Depois da oferta é listada, elementos são incorporados programaticamente dentro o logótipo heroína. Os elementos incorporados incluem o nome de apresentação do publicador, o título do plano, a oferta longa resumo e o **criar** botão. Por conseguinte, não introduza quaisquer texto ao estruturar o logótipo heroína. Deixe o espaço em branco à direita porque o texto é através de programação desse espaço. O espaço em branco para o texto deve ser 415 x 100 pixéis à direita. É de deslocamento em 370 pixéis da esquerda.

    ![Exemplo de logótipo heroína](./media/publish-marketplace-app/publishvm14.png)

## <a name="support-form"></a>Formulário de suporte

Preencha o **suporta** contacta de formulário com o suporte da sua empresa. Estas informações podem ser engenharia contactos e contactos de suporte ao cliente.

## <a name="publish-an-offer"></a>Publicar uma oferta

Depois de preencher todas as secções, selecione **publicar** para iniciar o processo que disponibiliza a oferta para clientes.

## <a name="next-steps"></a>Passos Seguintes

* Para obter uma introdução às aplicações geridas, veja [Descrição geral das aplicações geridas](overview.md).
* Para obter informações sobre como publicar uma aplicação do catálogo de serviço geridas, consulte [criar e publicar uma aplicação do catálogo de serviço geridas](publish-service-catalog-app.md).