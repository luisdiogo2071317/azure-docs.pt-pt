---
title: Criar uma oferta de módulo do IoT Edge | Documentos da Microsoft
description: Como publicar um novo módulo do IoT Edge para o Marketplace.
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
ms.date: 09/19/2018
ms.author: pbutlerm
ms.openlocfilehash: d3cc1a09963c5f7fee613af24c63fd15b1cfffee
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48810272"
---
# <a name="how-to-publish-a-new-iot-edge-module-in-the-cloud-partner-portal"></a>Como publicar um novo módulo do IoT Edge no Portal de parceiros de nuvem

Este artigo descreve os passos para publicar uma nova oferta de módulo do IoT Edge.

## <a name="prerequisites"></a>Pré-requisitos

Os seguintes pré-requisitos aplicam-se para a publicação de um módulo do IoT Edge para o Azure Marketplace.

-   Acesso para o [Cloud Partner Portal (CPP)](https://cloudpartner.azure.com/#alloffers). Para obter mais informações, consulte a [guia de publicação](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).

-   Ter o módulo do IoT Edge alojado no Azure Container Registry.

-   Ter seus metadados do módulo do IoT Edge pronto (incluindo lista parcial):

    -   Cargo

    -   Descrição (formato HTML básico)

    -   Um logótipo no formato de imagem png e no tamanho dos seguintes: pixels de 40 pixels x 40, pixels de 90 pixels x 90, 115 pixels x 115 píxeis, em pixels de 255 pixels x 115.

    -   Termos de política de privacidade e de utilização

<a name="prepare-your-iot-edge-module-listing-in-cpp"></a>Preparar a sua listagem do módulo do IoT Edge no CPP
-------------------------------------------

### <a name="create-a-new-offer-of-the-type-iot-edge-module"></a>Criar uma nova oferta do tipo de módulo do IoT Edge 

Siga estes passos para preparar sua listagem do módulo do IoT Edge:

-   Inicie sessão sua [conta CPP](https://cloudpartner.azure.com/).

>[!Note]
>Para obter informações gerais sobre o Portal de parceiros da Cloud, pode verificar o [saber documentação](https://cloudpartner.azure.com/#learn)

-   Selecione **nova oferta**e, em seguida, selecione **módulo do IoT Edge**.

>[!NOTE]
>Um módulo do IoT Edge é um contentor que é efetuado especificamente para ser executado no IoT Edge. Os cenários abordados pelo módulo do IoT Edge tem sentido num contexto de IoT Edge. Ele também inclui as predefinições de configuração para tornar a implantação para uma IoT Edge dispositivo direta. O contentor pode também incluir o SDK de módulo do IoT Edge para permitir a comunicação com o edgeHub e o IoT Hub.

### <a name="define-your-offer-settings"></a>Definir as definições da oferta

No separador Definições da oferta, introduza as informações da sua oferta.

![Identidade de oferta](./media/cloud-partner-portal-create-iot-edge-module-offer/offer-identity.png)


-   O **ID da oferta** exclusivamente identifica a sua oferta no CPP e pode ser usado em URLS do lado do cliente.

-   O **nome** só é visível por si para referenciar a esta oferta no CPP.

### <a name="create-one-or-more-skus"></a>Criar um ou mais SKUs

Cada SKU corresponde a uma imagem de contentor. Tem de ter, pelo menos, um SKU e pode adicionar mais do que um. Existem duas partes para um SKU:

-   Metadados SKU

-   Metadados do contentor

**Para criar um SKU:**

Selecione o **SKUs** separador e, em seguida, selecione **novo SKU**.

![Novo SKU](./media/cloud-partner-portal-create-iot-edge-module-offer/SKUs.png)

Os metadados SKU contém os seguintes campos, o que são necessários:
- ID de SKU - um identificador exclusivo.
- Título – o título SKU, até 50 carateres.
- Resumo - uma descrição breve, até 100 carateres.
- Descrição – uma descrição longa.
- Ocultar este SKU – é a predefinição **não**.
   
![Detalhes SKU](./media/cloud-partner-portal-create-iot-edge-module-offer/SKU-settings.png)

#### <a name="iot-edge-module-metadata-and-the-container-registry"></a>Metadados de módulo do IoT Edge e o registo de contentor

Metadados do módulo do IoT Edge tem informações de referência de imagem que estão armazenadas no Azure container registry (ACR). O Azure Marketplace copia a imagem para o registo de marketplace público e está disponível para clientes após a certificação. Todos os pedidos de utilizador para consumir uma imagem de módulo do IoT Edge são servidos a partir do registo de contentor do Marketplace.

![Imagens do contentor](./media/cloud-partner-portal-create-iot-edge-module-offer/container-images.png)

**Imagens de contentor**

Os detalhes do repositório de imagens tem os seguintes campos obrigatórios:

-   **ID de subscrição** id de subscrição do Azure onde o registo do ACR está presente.

-   **Nome do grupo de recursos** – o nome do grupo de recursos do registo ACR.

-   **Nome do registo** – nome do registo do ACR.

-   **Nome do repositório** – o nome do repositório. Uma vez definido, este valor não é possível alterar mais tarde. O nome deve ser exclusivo para que nenhuma outra oferta na sua conta tem o mesmo nome.

-   **Nome de utilizador** – o nome de utilizador associada com o ACR (nome de utilizador do administrador).

-   **Palavra-passe** – a palavra-passe associada com o ACR.

    >[!Note]
    >O nome de utilizador e palavra-passe são necessários para assegurar que os parceiros têm acesso para o ACR descrito no processo de publicação.

Quando está publicando uma imagem de módulo do IoT Edge, pode fornecer uma ou mais etiquetas de imagem. Certifique-se de que adicionar uma etiqueta de "mais recente" (predefinição) para o seu módulo, para que possa identificar facilmente o módulo durante o teste.

Também pode especificar as seguintes especificações de módulo do IoT Edge:

-   **createOptions** -createOptions o padrão para passar para este módulo do IoT Edge pode ser iniciado de imediato.

-   **duplo:** -o duplo predefinido para passar para este módulo do IoT Edge pode ser iniciado prontos a utilizar ao utilizar o SDK do módulo de IoT.

-   **rotas:** -as rotas predefinidas para passar para que este módulo do IoT Edge pode ser iniciado de imediato, se utilizar o SDK do módulo de IoT.

### <a name="describe-your-iot-edge-module-for-your-customers"></a>Descreva o seu módulo do IoT Edge para seus clientes

No separador de Marketplace, adicione o seu conteúdo de marketing específico. Esta informação é o que estarão publicamente visíveis e listados no Azure Marketplace.

![Descrição geral de módulo do IoT Edge](./media/cloud-partner-portal-create-iot-edge-module-offer/overview.png)

-   **Título** -o título do seu módulo do IoT Edge que é destinado ao público.

-   **Resumo** -o resumo do seu módulo do IoT Edge que é destinado ao público na maioria das páginas, como a navegação de páginas.

-   **Há muito tempo resumo** -o resumo do seu módulo do IoT Edge que é destinado ao público quando o módulo em destaque. 

-   **Descrição** -a descrição do seu módulo do IoT Edge que é destinado ao público na página de detalhes do produto. (Pode utilizar tags de HTML básicos para formatar sua descrição.)

-   **Identificador de marketing** -um identificador exclusivo que é utilizado para criar o URL do seu produto. Este URL é o seguinte formato: *azuremarketplace.microsoft.com/enus/marketplace/apps/yourpublisherid.youriotedgemodulemarketingidentifier*.

-   **Ids de subscrição de pré-visualização** - os utilizadores que têm acesso a estas subscrições serão capazes de ver o módulo do IoT Edge após o passo de certificação e antes de entrar no ar.

-   **Ligações úteis** -pode adicionar até 10 ligações que serão apresentadas na sua página de detalhes do produto.

-   **Sugestões de categorias** -escolher até cinco categorias. Serão apresentados na sua página de detalhes do produto. Atualmente, todos os módulos do IoT Edge são apresentados sob a *Internet das coisas \> módulo do IoT Edge* categorias nas páginas de procura.

-   **Logótipos** -carregar as suas imagens de logótipo do módulo do IoT Edge no formato PNG. Utilize os seguintes tamanhos: e exatamente no tamanho dos seguintes: pixels de 40 pixels x 40, pixels de 90 pixels x 90, 115 pixels x 115 píxeis, em pixels de 255 pixels x 115.

-   **Capturas de ecrã** -capturas de ecrã são apresentados na sua página de detalhes do produto. Eles são uma boa forma de comunicar visualmente o que faz o módulo do IoT Edge e como ela funciona. Pode mostrar diagramas de arquitetura ou utilizar as maiúsculas ilustrações por exemplo.

-   **Vídeos** -vídeos são apresentados na sua página de detalhes do produto. Eles são uma boa forma de comunicar visualmente o que faz o módulo do IoT Edge e como ela funciona.

-   **Numa gestão** -pode escolher um sistema para recolher todos os clientes potenciais que mostram o interesse no seu produto.

-   **Privacidade** -tem de ter uma URL apontando para a política de privacidade.

-   **Termos de utilização** -tem de ter termos de utilização. Pode utilizar tags de HTML para formatar esta página ou de apontar para uma das suas outras páginas.

### <a name="enter-your-support-contact-information"></a>Introduza as suas informações de contacto de suporte

No separador de suporte, forneça **contacto de engenharia** e **suporte ao cliente** informações.

![Contactos de suporte](./media/cloud-partner-portal-create-iot-edge-module-offer/support-contact-info.png)


## <a name="certify-your-iot-edge-module"></a>Certificar o módulo do IoT Edge

Depois de fornecer todas as informações necessárias, selecione **publicar** para enviar o seu módulo do IoT Edge para certificação. Verá uma linha do tempo que mostram 's os passos no processo de certificação.

**Verificação de módulo**

O módulo é verificado pela nossa equipa de certificação. Depois do módulo está certificado, terá uma ligação privada para testar seu módulo. Se precisar de efetuar alterações após os testes, editar os metadados da sua oferta e envie novamente o módulo para a equipe de certificação. 

## <a name="publish-your-iot-edge-module"></a>Publicar o seu módulo do IoT Edge

Depois de concluir o teste e estão prontos para publicar, selecione **Go Live** para publicar seu módulo do IoT Edge.

>[!Important]
>Se quiser ter o módulo do IoT Edge, anunciado no evento do Ignite, seu módulo tem de ser público por 23/09/2018.
