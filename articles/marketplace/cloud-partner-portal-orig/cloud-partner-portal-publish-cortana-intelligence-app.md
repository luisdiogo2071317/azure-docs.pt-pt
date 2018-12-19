---
title: Cortana intelligence | Documentos da Microsoft
description: Publicar uma oferta do Cortana intelligence.
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
ms.openlocfilehash: 74cde720af012b3355b813cf6da2b2bdf10b9b8e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51257212"
---
# <a name="publish-a-cortana-intelligence-offer-using-the-cloud-partner-portal"></a>Publicar uma oferta do Cortana Intelligence com o Portal de parceiro de Cloud

Este artigo descreve como publicar uma oferta do Cortana Intelligence com o Portal de parceiro de Cloud.

## <a name="prerequisites"></a>Pré-requisitos

O Portal de parceiros da Cloud suporta o acesso baseado em funções para o portal, que permite que os colaboradores colaborar em publicar uma oferta. Para obter mais informações, consulte [gerir utilizadores do Portal Cloud](./cloud-partner-portal-manage-users.md).

Antes de uma oferta pode ser publicada em nome de um fabricante de contas, uma das pessoas com \"proprietário\" função precisam concorda em estar em conformidade com o [termos de utilização](https://azure.microsoft.com/support/legal/website-terms-of-use/), [declaraçãodeprivacidadedaMicrosoft](https://www.microsoft.com/privacystatement/default.aspx), e [contrato do programa do Microsoft Azure Certified](https://azure.microsoft.com/support/legal/marketplace/certified-program-agreement/).

## <a name="to-start-creating-a-cortana-inteligence-offer"></a>Para começar a criar uma oferta de Cortana Inteligence

Depois de todos os pré-requisitos tiverem sido cumpridos, está pronto para começar a criar a sua oferta de Cortana Inteligence.

1.  Inicie sessão para o [portal do cloud partner](http://cloudpartner.azure.com/).
2.  Na barra de navegação esquerdo, selecione **+ nova oferta**.
3. Selecione **do Cortana Intelligence**.
4. Selecione o **Editor** separador a **nova oferta** vista para configurar as seguintes opções:
    -   Definições da oferta
    -   Informações técnicas
    -   Vitrine detalhes
    -   Contactos

    Cada uma das opções anteriores exibir um formulário que preencher.  Necessário campos para cada formulário são marcados com um asterisco vermelho (\*).

## <a name="to-configure-offer-settings"></a>Para configurar as definições da oferta

As definições da oferta fornece informações básicas sobre a oferta, como um ID de oferta, o ID de publicador e o nome da oferta.

### <a name="offer-id"></a>ID de Oferta

Este é um identificador exclusivo para a oferta de um perfil do publicador.
Este ID está visível no produto URLs. Apenas pode ser constituído por carateres alfanuméricos em minúsculas ou hífenes (-). O ID não pode terminar com um travessão e pode ter um máximo de 50 carateres. 
>[!Note]
>Este campo é bloqueado quando uma oferta entra no ar.

**Exemplo:**

Se o publicador **contoso** cria uma oferta com o ID de oferta *exemplo-Cortana Intelligence*, que será apresentado no AppSource como "https://appsource.microsoft.com/marketplace/apps/**contoso**.*exemplo-Cortana Intelligence* ? separador descrição geral de = ".

### <a name="publisher-id"></a>ID de publicador

Uma lista suspensa de perfis do publicador. Utilize para escolher o perfil do publicador que pretende publicar esta oferta em.

>[!Note]
>Este campo é bloqueado quando uma oferta entra no ar.

### <a name="name"></a>Nome

O nome a apresentar para a sua oferta. Este nome é apresentado em [AppSource](https://appsource.microsoft.com). Pode ter um máximo de 50 carateres.

Quando terminar de fornecer as informações necessárias nas definições da oferta, selecione **guardar** para avançar para a parte de informações técnicas da oferta. opção.

## <a name="to-configure-technical-info"></a>Para configurar informações técnicas

Utilize esta vista para fornecer informações técnicas, que serão apresentadas na página da oferta. Os campos seguintes aplicam-se a esta vista.

### <a name="partner-mpn-id"></a>Id de parceiro da MPN

Se for um parceiro da Microsoft registado, vá para o [Web site de parceiros](https://partners.microsoft.com/) e inicie sessão para obter o ID de organização de parceiro. Introduza esse ID para o **MPN Id de parceiro**.

### <a name="analytics-components-used"></a>Componentes de análise utilizados

Selecione todos os componentes que utiliza a sua oferta. Selecione, pelo menos, 1 componente. Selecione **Microsoft R** apenas se estiver a utilizar qualquer uma das licenças MRS dos serviços do SQL 2016 R, HDInsight Premium, ou MRS em execução em VMs.

### <a name="additional-components-used"></a>Componentes adicionais utilizados

Selecione todos os outros componentes que utiliza a sua solução.

### <a name="customer-name-using-solution"></a>Nome do cliente através de solução

Forneça o nome do cliente que está a utilizar esta solução na produção. 

>[!Note]
>Estas informações não sejam publicadas no AppSource. Só é utilizado para a avaliação da solução.

### <a name="azure-consumption-requirement-met"></a>Foi cumprido um requisito de consumo do Azure?

Indicar se pretende ou não a solução gera, pelo menos, US $1 K por mês por cliente dos componentes do Azure Suite ou se a solução utiliza o Microsoft R.

>[!Note]
>Estas informações não sejam publicadas no AppSource. Só é utilizado para a avaliação da solução.

### <a name="demo-video-url"></a>URL de vídeo de demonstração

Forneça um URL para uma demonstração em vídeo da solução/aplicação que a sua equipa de vendas previamente pode mostrar os clientes. 

>[!Note]
>Estas informações não sejam publicadas no AppSource. Só é utilizado para a avaliação da solução.

#### <a name="demo-video-guidelines"></a>Diretrizes de vídeo de demonstração

- Duração do vídeo deve ser menos de 15 minutos.
- O vídeo deve ser editado de minimamente a gravação da funcionalidade da solução. 
- O vídeo destaca os principais aspectos da funcionalidade destinada ao utilizador e enfoca a integração de análises avançadas. 
- Vídeos de demonstração **não irá** disponibilizada publicamente aos clientes, mas espera-se para ser representativo de como a solução *seria* mostrado para um cliente em potencial. Como tal, eles devem ser passível de repetição e com script.
- Utilize qualquer ferramenta de gravação de ecrã que exporta um formato de vídeo padrão (por exemplo, MPEG) para criar o seu vídeo. 

As instruções seguintes mostram como criar um vídeo com o Skype para empresas. 

1. [Iniciar uma reunião](https://support.office.com/article/Start-a-Skype-for-Business-conference-call-8dc8ac52-91ac-4db9-8672-11551fdaf997)
2. [Compartilhe sua área de trabalho](https://support.office.com/article/Share-your-screen-in-Skype-for-Business-2d436dc9-d092-4ef1-83f1-dd9f7a7cd3fc)
3. [Iniciar gravação](https://support.office.com/article/Share-sites-or-documents-with-people-outside-your-organization-80e49744-e30f-44db-8d51-16661b1d4232)
4. Depois de interromper a gravação, [usar o Gerenciador de gravação para publicar a gravação](https://support.office.com/article/Recording-Manager-save-and-publish-59a3beb7-c700-40cf-ab21-bc82a2b06351)

Carregue o seu vídeo gravado para um serviço que permite gerar um URL partilhado. Por exemplo, um [ligação de convidado no OneDrive ou Sharepoint](https://support.office.com/article/Share-sites-or-documents-with-people-outside-your-organization-80e49744-e30f-44db-8d51-16661b1d4232).

### <a name="supported-regions"></a>Regiões suportadas

Selecione todas as regiões que oferece suporte a sua solução. Selecione, pelo menos, 1 região.

### <a name="power-bi-desktop-file-pbix"></a>Ficheiro do Power BI desktop (. pbix)

Se aplicável, carregue um ficheiro. pbix. Certifique-se de que dados são importados para o ficheiro, não referenciado externamente. Vamos criar o relatório incorporado para.

### <a name="solution-architecture"></a>Arquitetura da solução

Carregar um documento que detalha a arquitetura da solução. Para carregar uma diretriz de diagrama de arquitetura de solução, consulte [modelo de fluxo de trabalho de solução de análise avançada](https://partnersprofilesint.blob.core.windows.net/partner-assets/documents/AppSource%20Solution%20Publishing%20Guide%20Docs/Advanced%20Analytics%20Solution%20Workflow%20Template%20V.2017.3.23.pptx).

>[!Note]
>Estas informações não sejam publicadas no AppSource. Só é utilizado para a avaliação da solução.

### <a name="select-segments"></a>Selecione segmentos

Selecione todos os segmentos de setor relacionadas. Se a aplicação for para qualquer segmento não listado, introduza o nome de segmento na **outros** campo. Está limitado a três palavras do segmento.

### <a name="select-business-processes"></a>Selecione os processos de negócios

Selecione os processos de negócios que melhor descrevem sua solução. Se a aplicação for para todos os processos não listados, introduza o nome do processo a **outros** campo. Está limitado a três palavras para o processo.

### <a name="trial-info"></a>Informações de avaliação

-   **URL de avaliação de SaaS:** introduza o URL para a experiência de avaliação da sua aplicação.
-   **URL de avaliação de unidade de teste:** introduza o URL para a experiência de versão de teste da sua aplicação.

Para obter mais informações sobre as versões de avaliação, consulte **tipo de aplicação** na próxima seção deste artigo.

Quando terminar de fornecer as informações necessárias em informações técnicas, selecione **guardar** para avançar para a parte de loja detalhes da oferta. 

## <a name="to-configure-storefront-details"></a>Para configurar os detalhes de loja

### <a name="offer-summary"></a>Resumo da oferta

Este é um resumo da proposta de valor da sua oferta. Ele será exibido na página de pesquisa da sua oferta. O comprimento máximo do resumo é 100 carateres.

### <a name="offer-description"></a>Descrição da oferta

Esta é a descrição que aparecerá na página de detalhes da sua aplicação.
O comprimento máximo da descrição é 1300 carateres.

Tenha em atenção que este campo leva html conteúdo com etiquetas como \<p\>, \<h1\>, \<h2\>, \<li\>, etc., que permite tornar o conteúdo muito mais apresentável. Equipa do portal de publicação está funcionando sobre como adicionar um recurso para permitir que um deles ver uma pré-visualização dos respetivos detalhes vitrine para tornar o conteúdo mais apresentável iterativamente – enquanto isso, pode usar qualquer pode utilizar quaisquer ferramentas online de html em tempo real, como [ http://htmledit.squarefree.com ](http://htmledit.squarefree.com/) para ver como seria a aparência de sua descrição.

O formato sugerido da descrição é ter dividir o texto em seções secundárias com base no valor propositions, cada um com realçado com um cabeçalho secundárias. Visitantes glance normalmente sobre o campo "Resumo de oferecer" e cabeçalhos secundárias para obter a essência de endereços de que a aplicação e por que eles considerar a aplicação em apenas uma breve análise. Por isso, é importante obter o utilizador\'atenção s, dar-lhes um motivo para continue a ler para obter os detalhes.

#### <a name="partner-examples"></a>Exemplos de parceiro

- [Otimização de inventário de Neal Analytics](https://appsource.microsoft.com/product/web-apps/neal_analytics.8066ad01-1e61-40cd-bd33-9b86c65fa73a?tab=Overview)
- [Personalização de varejo Plexure](https://appsource.microsoft.com/product/web-apps/plexure.c82dc2fc-817b-487e-ae83-1658c1bc8ff2?tab=Overview)
- [Serviços dos cidadãos AvePoint](https://appsource.microsoft.com/product/web-apps/avepoint.7738ac97-fd40-4ed3-aaab-327c3e0fe0b3?tab=Overview)

### <a name="industries"></a>Setores

Selecione o setor melhor alinhado à sua aplicação. Se o aplicativo se relaciona para vários setores, deixe este campo vazio.

### <a name="categories"></a>Categorias

Selecione as categorias que são relevantes para a sua aplicação. Selecione um máximo de duas categorias.

### <a name="app-type"></a>Tipo de aplicação

Selecione o tipo de versão de avaliação que oferecerá suporte a sua aplicação no AppSource. Escolha das avaliações seguintes:
- **Gratuito** significa que a aplicação é gratuita.
- **Versão de avaliação** significa que os clientes podem experimentar seu durante um período especificado.
- **Pedir avaliação** significa que os clientes podem solicitar uma versão de avaliação da aplicação.

Dois tipos de experiências de avaliação podem permitir que parceiros no AppSource.

- O **versão de avaliação** opção, também denominada **versões de avaliação do cliente por um instrutor (CLT)** pode ser um dos seguintes tipos: 
    - Versão de avaliação de SaaS
        - O cliente pode navegar para um Url que ou seu parceiro fornecer. O cliente, passa pelo AAD federado, SSO para têm um tempo demarcado experiência de avaliação.
        - Esta é uma aplicação de SaaS com multi-inquilinos que isola / dados um utilizador de configuração de outros utilizadores. Ou esta experiência fornece apenas um subconjunto que utiliza as operações só de leitura.

        **Exemplos:**

        - [Execução de varejo POP AFS](https://appsource.microsoft.com/product/web-apps/afs.fa9fc926-3bc3-43dd-becd-3ef41b52c10b?tab=Overview)
        - [Os serviços dos cidadãos AvePoint](https://appsource.microsoft.com/product/web-apps/avepoint.7738ac97-fd40-4ed3-aaab-327c3e0fe0b3?tab=Overview). (Esta aplicação fornece uma experiência organizada com caminhos claras para um conjunto de pessoas fidedignas do utilizador selecionado.)

     - Versão de Teste
        - A (ou seu parceiro) solução ou um subconjunto de ele pode ser empacotado com modelos do Azure Resource Manager que pode criar uma instância de AppSource e. AppSource pode gerir a aplicação numa subscrição de parceiro com o tempo de boxing e manter o conjunto de quentes/frias de instâncias, etc.
        - Podemos fornecer modelos e o código de exemplo para ajudá-lo a se escolher esta opção.

        **Exemplos:**

        - [Otimização de inventário de Neal Analytics](https://appsource.microsoft.com/product/web-apps/neal_analytics.8066ad01-1e61-40cd-bd33-9b86c65fa73a?tab=Overview&tag=CISHome)

- O **pedido para versão de avaliação** (**versões de avaliação do parceiro por um instrutor / PLT**) opção exige que os clientes preencher um formulário de informações de contacto para seguimento de parceiro. O parceiro acompanha e fornece uma demonstração ou de avaliação da aplicação. Para obter mais informações, consulte [instruções de experiência de avaliação do AppSource](https://aka.ms/trialexperienceforwebapps) vídeo para obter uma descrição de geral de alto nível.

>[!Note]
>Os dados mostram que **versões de avaliação do cliente por um instrutor** tem uma maior geração de oportunidades potencial que **versões de avaliação do parceiro levou**.


### <a name="help-link-for-your-app"></a>Ligação de ajuda para a sua aplicação

Forneça um URL para uma página com informações de ajuda para a sua aplicação.

### <a name="supported-countriesregions"></a>Países/regiões suportados

Este campo determina os países/regiões onde sua oferta estarão disponível para avaliação.

![Países/regiões suportados](./media/cloud-partner-portal-publish-cortana-intelligence-app/CISScreenshot3.png)

### <a name="supported-languages"></a>Linguagens suportadas

Selecione os idiomas que oferece suporte a sua aplicação. Se seu aplicativo oferecer suporte a idiomas que não estão nesta lista, publicar a sua oferta e enviar-nos para: <appsource@microsoft.com> para nos informar sobre o suporte para outros idiomas.

### <a name="app-version"></a>Versão da aplicação

Introduza o número de versão para a sua aplicação.

### <a name="products-your-app-works-with"></a>Produtos a sua aplicação funciona com

Lista produtos específicos que a sua aplicação funciona com. Pode listar o máximo de 3 produtos. Para listar um produto, selecione o **sinal de adição** (junto ao novo) e um campo de texto em aberto é criado para. Introduza o nome de um produto que a sua aplicação funciona com.

### <a name="hide-key"></a>Ocultar chave

Esta é uma chave combinada com o URL de pré-visualização de oferta para ocultar a oferta da visualização pública. Não é uma palavra-passe. Pode introduzir qualquer cadeia de caracteres de alfanumérica.

### <a name="offer-logo-small"></a>Logótipo da oferta (pequena)

Este logótipo é apresentada na página de pesquisa da sua aplicação. O formato de imagem png é o único formato de que tem permissão. O tamanho da imagem é 48 pixels por 48 pixels.

### <a name="offer-logo-large"></a>Logótipo da oferta (grande)

Este logótipo é apresentada na página de detalhes da sua aplicação. O formato de imagem png é o único formato de que tem permissão. O tamanho da imagem é 48 pixels de pixels x 48.

### <a name="video"></a>Vídeo

Pode carregar o máximo de quatro vídeos. Para cada vídeo que pretende carregar:
- Forneça o nome do vídeo
- Forneça uma URL (YouTube ou Vimeo apenas)
- Fornece uma miniatura para associar o vídeo. A miniatura deve utilizar o formato de imagem png e do tamanho tem de ser pixels de 1280 X 720 pixels. 

Para adicionar novos video(s), selecione **+ novo**, mostrado na captura de ecrã seguinte.

![Adicionar novo vídeo](./media/cloud-partner-portal-publish-cortana-intelligence-app/CISScreenshot4.png)

### <a name="document"></a>Documento

Pode carregar um máximo de três documentos. Cada documento tem de utilizar o formato de ficheiro PDF. Para adicionar um novo documento:

- Selecione **+ novo**
- Introduza um nome de documento
- Selecione **carregar** para carregar um documento.

![Adicionar novo documento](./media/cloud-partner-portal-publish-cortana-intelligence-app/CISScreenshot5.png)

### <a name="privacy-policy"></a>Política de Privacidade

Introduza o URL para a política de privacidade da sua aplicação

### <a name="terms-of-use"></a>Termos de utilização

Introduza os termos de utilização da sua aplicação. Os clientes do AppSource precisam aceitar estes termos antes de eles experimentam a sua aplicação.

>[!Note]
>Este campo aceita conteúdo usando HTML marcas, como o conteúdo de html < p\>, < h1\>, e < li\>. Pode utilizar estas etiquetas para formatar o seu conteúdo. 

### <a name="lead-destination"></a>Líder de destino

Selecione um sistema CRM onde serão armazenados os suas oportunidades potenciais. 

Selecione **tabelas do Azure** se estiver a utilizar um destes sistemas CRM: Microsoft Dynamics CRM, Marketo ou Salesforce. 

Para obter mais informações sobre o sistema CRM que pretende utilizar, selecione um dos links a seguir para sistemas suportados.

-   [Tabela do Azure](./cloud-partner-portal-lead-management-instructions-azure-table.md)
-   [Marketo](./cloud-partner-portal-lead-management-instructions-marketo.md)
-   [Microsoft Dynamics CRM](./cloud-partner-portal-lead-management-instructions-dynamics.md)
-   [Salesforce](./cloud-partner-portal-lead-management-instructions-salesforce.md)
