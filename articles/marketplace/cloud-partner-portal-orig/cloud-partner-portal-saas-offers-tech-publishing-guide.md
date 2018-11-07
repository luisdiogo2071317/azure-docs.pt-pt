---
title: Técnico de aplicação SaaS publicação guia | Documentos da Microsoft
description: Explica como publicar aplicações SaaS ofertas no marketplace apropriado.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: pbutlerm
ms.openlocfilehash: 7430e57ceaec8a280c17f1276d503919a9703182
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51228900"
---
<a name="saas-application-technical-publishing-guide"></a>Técnico de aplicação SaaS guia de publicação
===========================================

Bem-vindo à técnica de aplicações de SaaS guia de publicação! Este guia foi concebido para ajudar a Release candidate e publicadores existentes para listar os seus serviços e aplicações no AppSource ou utilizar as aplicações de SaaS a oferta do Azure Marketplace.

Para uma descrição geral de todas as outras ofertas do Marketplace, consulte a [guia de publicador de Marketplace](https://aka.ms/sellerguide).


<a name="what-are-pre-requisites-for-publishing-a-saas-app"></a>Quais são os pré-requisitos para a publicação de uma aplicação SaaS?
-------------------------------------------------

Portal de publicação fornece acesso baseado em funções no portal, permitindo que várias pessoas colaborar em direção à publicação de uma oferta. Para obter mais informações, consulte [gerir utilizadores](./cloud-partner-portal-manage-users.md). 

Antes de uma oferta pode ser publicada em nome de um fabricante de contas, uma das pessoas com *proprietário* função precisam concorda em estar em conformidade com o [termos de utilização](https://azure.microsoft.com/support/legal/website-terms-of-use/), [Microsoft PrivacyStatement](https://www.microsoft.com/privacystatement/default.aspx), e [ProgramAgreement do Microsoft Azure Certified](https://azure.microsoft.com/support/legal/marketplace/certified-program-agreement/).


<a name="creating-an-offer"></a>Criar uma oferta
-----------------

Esta secção descreve o processo de como criar uma nova oferta de aplicação SaaS.

![Descrição geral da oferta SaaS](media/cpp-creating-saas-offers/saas-offer-overview.png)

A oferta da aplicação SaaS consiste em cinco seções, descritas na tabela a seguir:

| **Seção de ofertas**  | **Descrição**                                                                                                     |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------|
| Definições da oferta     | Ajuda a definir um nome exclusivo e o ID da aplicação SaaS                                                                         |
| Informações técnicas     | Ajuda a configurar o tipo de solução de SaaS e forneça os detalhes de ligação para a sua aplicação                            |
| Versão de Teste         | Secção opcional que permite-lhe definir um serviço que permitirá aos clientes testar a sua oferta antes de eles adquirem-la. |
| Vitrine detalhes | Contém o marketing, legal, gestão de oportunidades potenciais e seções de listagem:   <br/> -A seção de Marketing permite-lhe introduzir a descrição e logotipos necessários para a oferta para ser apresentado corretamente no portal de utilizador do marketplace.  <br/> -A gestão de oportunidades potenciais permite-lhe definir um local para onde redirecionar as oportunidades potenciais de utilizador final geradas no portal de utilizador final do Azure Marketplace.  <br/> -A seção Legal permite-lhe introduzir a política de privacidade e termos de uso documentos legais.  |
| Contacto            | Permite-lhe introduzir informações de contacto de suporte da oferta.                                                                  |
|  |  |


### <a name="creating-a-new-offer"></a>Criar uma nova oferta

Depois de iniciar sessão Portal de parceiros de nuvem, selecione o **nova oferta** item na barra de menu à esquerda, que apresenta um menu de ofertas disponíveis. A imagem seguinte mostra um exemplo dessas ofertas:

![Nova oferta de SaaS](media/cpp-creating-saas-offers/saas-new-offer.png)

Selecione a oferta que foi aprovou para a partir da lista e abrirá um novo formulário de oferta.

![Novo formulário de oferta SaaS](media/cpp-creating-saas-offers/saas-new-offer-2.png)

A tabela seguinte descreve os campos de oferta:

| **Campos de oferta** | **Descrição**                                                                                            |
|------------------|----------------------------------------------------------------------------------------------------------- |
| ID de Oferta         | Um identificador exclusivo para a oferta de um perfil do publicador. Este ID será visível em URLs de produto e os relatórios de faturas. Apenas pode ser constituído por carateres alfanuméricos em minúsculas ou hífenes (-). O ID não pode terminar com um travessão e pode ter um máximo de 50 carateres. Tenha em atenção que este campo está bloqueado assim que uma oferta entra no ar. Por exemplo, se um publicador, a Contoso, publica uma oferta com o ID de oferta exemplo-vm, ela aparecerá no Azure marketplace, como: [https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-vm?tab=Overview](https://azuremarketplace.microsoft.com/) |
| ID de publicador     | O ID de publicador é o identificador exclusivo no Marketplace. Todas as suas ofertas devem ser ligado seu ID de publicador. Não é possível modificar o ID de publicador assim que a oferta é guardada.                                                                                       |
| Nome             | Este é o nome a apresentar para a sua oferta. Este é o nome que irá aparecer no Azure Marketplace e no Portal do Azure. Pode ter um máximo de 50 carateres. Documentação de orientação aqui é incluir um nome de marca reconhecível para o seu produto. Não inclua o nome da sua empresa aqui, a menos que o que é a forma como é comercializada. Se estiver marketing esta oferta em seu próprio Web site, certifique-se de que o nome é exatamente como ele aparece no seu Web site.             |
|  |  |

Clique em **guardar** para guardar o seu progresso. A secção seguinte descreve os planos de adicionar a sua oferta.


### <a name="technical-information"></a>Informações técnicas

A secção de informações técnicas permite-lhe introduzir as seguintes informações:

![SaaS oferecem informações técnicas](media/cpp-creating-saas-offers/saas-new-offer-technical-info.png)

A decisão mais importante é se for para ter uma listagem de SaaS ou ter commerce ativada. Se uma listagem de SaaS, tem de selecionar entre:

-   Livre - se de fornecer um URL da aplicação SaaS em que os clientes podem obter a sua aplicação.
-   Avaliação gratuita - indique um URL da aplicação SaaS em que os clientes podem executar a versão de avaliação antes de eles a oferta de compra.
-   Contacte-me - relevante apenas se tiver um sistema de gestão de oportunidades potenciais ligado, esta opção permite que os clientes pedem para ser contatado, e uma oportunidade potencial for partilhada consigo.

Se for uma aplicação SaaS que está no Azure Marketplace e pretende permitir commerce por meio de transações da Microsoft, selecionadas **venda através do Azure**.  
Para obter mais informações sobre como ligar a sua aplicação SaaS, veja [SaaS - venda através do Azure](./cloud-partner-portal-saas-offer-subscriptions.md).


### <a name="test-drive"></a>Versão de Teste

A criação de uma experiência de avaliação para os seus clientes é uma prática recomendada, certifique-se de que podem comprar com confiança. As opções de avaliação disponíveis, Test-Drive é leva a mais uma forma eficaz de geração de alta qualidade e leva de conversão maior deles.

Fornece aos clientes com uma avaliação autodidata prática dos principais recursos e benefícios, demonstrados num cenário de implementação em tempo real de seu produto.

![Oferta de SaaS Test-Drive](media/cpp-creating-saas-offers/saas-new-offer-test-drive.png)


#### <a name="how-a-test-drive-works"></a>Como funciona um Test Drive

Um cliente potencial pesquisa e descobre a sua aplicação no Marketplace. O cliente inicia sessão e se concorda com os termos de utilização. Neste momento, o cliente recebe o seu ambiente previamente configurada para experimentar para um número fixo de horas, enquanto recebe uma oportunidade potencial altamente qualificada para dar seguimento com.

![Teste 2 de unidade da oferta SaaS](media/cpp-creating-saas-offers/saas-new-offer-test-drive-2.png)

Independentemente da complexidade é de seu aplicativo, a sua versão de teste da Microsoft ajuda-o a traga o seu produto para a vida do cliente. Existem três tipos diferentes de versões de teste disponíveis, cada um com base no tipo de produto, o cenário e o marketplace que estiver a utilizar.

-   **O Azure Resource Manager** -versão de um teste de ARM do Azure é um modelo de implementação que contém todos os recursos do Azure que compõem uma solução que está a ser criada pelo editor. Produtos de acordo com esse tipo de versão de teste são aqueles que utilizam apenas os recursos do Azure.
-   **Aplicação lógica** -teste de aplicação de lógica de uma unidade é um modelo de implementação que se destina a abranger todas as arquiteturas de solução complexas. Todas as aplicações do Dynamics ou produtos personalizados, devem utilizar este tipo de versão de teste.
-   **Power BI** -unidade de um teste do Power BI consiste num link incorporado a um dashboard personalizados internos. Qualquer produto que pretende demonstrar que uma interactive visual do Power BI deve utilizar este tipo de versão de teste. É tudo o que precisa para carregar o URL de BI de energia incorporados.

Os passos de publicação principais para adicionar um teste de unidade são:

1.  Definir o seu cenário de teste de unidade
2.  Criar e/ou modificar o modelo do Resource Manager
3.  Criar o manual passo a passo do teste de unidade
4.  Voltar a publicar a oferta

Para obter mais informações, consulte [Test-Drive](./what-is-test-drive.md).


### <a name="storefront-details"></a>Vitrine detalhes

Aplicações de SaaS tem as duas primeiras secções de um resumo e uma descrição para ser fornecido sobre a sua aplicação.

![Vitrine detalhes da oferta de SaaS](media/cpp-creating-saas-offers/saas-new-offer-storefront-details.png)

A tabela seguinte descreve a oferta\'s Vitrine detalhes:

| **Campos de oferta**        | **Descrição**                                                                                          |
|-------------------------| ---------------------------------------------------------------------------------------------------------|
| Resumo da oferta           | Resumo da proposta de valor da sua oferta. Ele será exibido na página de pesquisa da sua oferta. Deve ser um máximo de 100 carateres.   |
| Descrição da oferta       | Descrição que aparecerá na página de detalhes da sua aplicação. Máximo permitido é de carateres de 1300 *nota* este campo aceita HTML conteúdo com etiquetas como & ltp\>, & lth1\>, & lth2\>, & ltli\>, etc., que lhe permite torne o conteúdo apresentável muito mais. Equipa do portal de publicação está funcionando sobre como adicionar um recurso para permitir que um deles ver uma pré-visualização dos respetivos detalhes vitrine para tornar o conteúdo mais apresentável iterativamente – enquanto isso, pode usar qualquer pode utilizar quaisquer ferramentas online de HTML em tempo real, como http://htmledit.squarefree.com para ver como seria sua descrição. |
| Setores              | Selecione os setores melhor alinhado à sua oferta. Se o aplicativo se relaciona para vários setores, pode selecionar um máximo de dois. |
| Categorias sugeridas    | Selecione as categorias de melhor alinhado à sua oferta. Pode selecionar um máximo de três categorias.     |
| Versão da aplicação     | Introduza o número de versão da sua aplicação                                                                |
| Palavras a procurar (máx. 3) | Introduza as palavras a procurar até três que os clientes podem utilizar para localizar seu aplicativo no Web site vitrine do Marketplace. |
|  |  |


### <a name="marketing-artifacts"></a>Artefactos de marketing

A secção de artefactos de marketing permite-lhe definir o marketing ativos como logótipos, vídeos, capturas de ecrã e documentos do Azure Marketplace:

![SaaS oferecem artefactos de marketing](media/cpp-creating-saas-offers/saas-new-offer-marketing-artifacts.png)

A tabela seguinte descreve os campos de Marketing:

| **Campos de oferta** | **Descrição**                                                                                                          |
|------------------| ------------------------------------------------------------------------------------------------------------------------ |
| Logótipos            | Se for um **venda através do Azure** aplicação SaaS, deve fornecer todas as imagens de logótipo. Se estiver apenas uma listagem, logótipos apenas 2 são necessários. Todos os logótipos carregados no Portal de parceiros de nuvem devem utilizar as seguintes diretrizes:     <br/> -Mantenha o número de principal e secundário cores seu logótipo baixa. O design do Azure tem uma paleta de cores simples.     <br/> -Evite o uso de preto e branco como a cor de fundo do seu logótipo. As cores de tema do Portal do Azure são preto e branco. Em alternativa, utilize uma cor que faria o logótipo proeminente no Portal do Azure. Recomendamos cores primárias simples. Se estiver a utilizar um plano de fundo transparente, em seguida, certifique-se de que o logótipo e o texto não são azul, preto ou branco.     <br/> -Não utilize um plano de fundo de gradação o logótipo.     <br/> -Evite colocar o texto, até mesmo sua empresa ou o nome da marca, o logótipo. O aspeto e funcionalidade do seu logótipo deve ser "simples" e deve evitar gradientes.    <br/> -Não deve ser esticada a imagem do logótipo.                   |
| Vídeos           | Permite-lhe adicionar hiperligações de vídeos da sua oferta. Pode usar links para vídeos do YouTube e/ou Vimeo, que são apresentados, juntamente com a sua oferta aos clientes. Também terá de introduzir uma imagem em miniatura do vídeo, com uma imagem png de 1280 x 720 pixels. Pode ter um máximo de quatro vídeos por oferta. |
| Documentos        | Permite-lhe adicionar documentos de marketing para sua oferta. Todos os documentos tem de estar no formato PDF e pode ter um máximo de três documentos por oferta.                                                                                                                                                      |
| Capturas de ecrã      | Pode adicionar capturas de ecrã da sua oferta. Há um máximo de cinco capturas de ecrã que podem ser adicionados por oferta. O tamanho da imagem máximo é de pixels de 1280 x 720.                                                                                                                                             |
| Ligações Úteis     | Permite-lhe adicionar URLs externos para a sua oferta para o ajudar a ponto para diagramas de arquitetura ou em outros sites que um cliente gostaria de ver.                                                                                                                                                              |
|  |  |

A imagem seguinte mostra como as informações são apresentadas num resultado da pesquisa Marketplace:

![SaaS oferecem informações do publicador marketing](media/cpp-creating-saas-offers/saas-new-offer-marketing-publisher-info.png)

A imagem seguinte mostra como a oferta é exibida no Marketplace após um cliente clica no mosaico de oferta menor:

![SaaS oferecer marketing info2 de publicador](media/cpp-creating-saas-offers/saas-new-offer-marketing-publisher-info-2.png)


### <a name="lead-management-and-legal-information"></a>Levar a gestão e informações legais

A secção Legal permite-lhe definir a documentação de legais da oferta.
Existem dois documentos legais necessários para cada oferta de aplicação SaaS: política de privacidade e termos de utilização. Para obter mais informações, veja  
[Configurar as oportunidades potenciais](./cloud-partner-portal-get-customer-leads.md).

![SaaS oferecem informações legais de marketing](media/cpp-creating-saas-offers/saas-new-offer-legal-info.png)

A tabela seguinte descreve as propriedades de seção Legal:

| **Campos de oferta**   | **Descrição**                                                                                 |
|--------------------| ----------------------------------------------------------------------------------------------- |
| URL de política de privacidade | O URL para a política de privacidade da sua empresa.                                                       |
| Termos de Utilização       | Os termos de utilização para a sua oferta. Escreva ou copie e cole os termos de utilização aqui. Utilização básica de HTML é permitida, por exemplo, o que este campo demora html conteúdo com etiquetas como & ltp\>, & lth1\>, & lth2\>, & ltli\>, etc. *Importante* Recomendamos vivamente que reveja e o HTML que criou num browser antes de submeter a oferta de pré-visualização. |
|  |  |


### <a name="contact-information"></a>Informações de Contacto

Nesta secção, irá introduzir os contactos de suporte da sua empresa que serão responsável por suporte de clientes a utilizar esta oferta.
Existem três áreas principais: contacto de engenharia, contacto de suporte e URLs de suporte:

![SaaS oferecem informações de contacto de marketing](media/cpp-creating-saas-offers/saas-new-offer-contact-info.png)


| **Contact**         | **Descrição**                                                                                                                          |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| Contacto de engenharia | Forneça o nome, e-mail e número de telefone de um contacto de engenharia que a Microsoft pode contactar para quaisquer problemas de suporte e de negócios. |
| Contacto de Suporte     | Forneça o nome, e-mail, número de telefone e URL que os clientes podem contactar quando têm pedidos de suporte de suporte.                  |
|  |  |


Assim que a sua oferta está pronta e publicar, a oferta passa pelo certificação. Podemos testar a sua aplicação SaaS através de uma verificação manual em torno de seu URL de teste, se tiver uma lista ou pontos finais têm de vender seu através do Azure selecionada. Durante esta aprovação manual, decidimos também loja adequada que a aplicação deve aparecer em (AppSource, Azure Marketplace ou ambos).

<a name="updating-the-offer"></a>A atualizar a oferta
------------------

Existem vários tipos de atualizações que pode querer fazer sua oferta depois foi publicado e está em direto. Qualquer alteração feita para a nova versão da sua oferta deve ser guardada e republicada tê-lo a refletir no Marketplace.

<a name="deleting-an-existing-offer"></a>A eliminar uma oferta existente
--------------------------

Pode optar por remover a oferta do Marketplace. Remoção da oferta que impede que novos clientes já não podem comprar ou implementar a sua oferta, no entanto, ele não tem impacto sobre os clientes existentes. A Extinção da Oferta é o processo de extinção do serviço e/ou contrato de licenciamento entre si e os seus clientes existentes.

Documentação de orientação e as políticas relacionadas com a remoção e extinção da oferta são regidas pelo contrato de publicador do Microsoft Marketplace (consultar secção 7) e as políticas de participação (consulte a secção 6.2). Esta seção fornece informações sobre os cenários suportados de exclusão diferente e os passos que pode tomar para eles.

### <a name="delete-the-live-offer"></a>Eliminar a oferta em direto

Existem vários aspectos que têm de ser resolvido se um pedido para remover uma oferta em direto. Siga os passos abaixo para obter documentação de orientação da equipa do suporte para remover uma oferta em direto do Azure Marketplace:

1.  Emitir um pedido de suporte utilizando a seguinte hiperligação
2.  Na lista Tipo de problema, selecione **gestão de ofertas**e na lista de categoria, selecione **modificar uma oferta de e/ou o SKU já na produção**.
3.  Submeter o pedido

A equipa de suporte irá guiá-lo no processo de eliminação de oferta.

> [!NOTE] 
> A eliminar uma oferta não afeta a compra atual dessa oferta. Essas compras do cliente irão continuar a trabalhar como antes.
No entanto, a oferta não estarão disponível para qualquer novas compras após a eliminação foi concluída.
