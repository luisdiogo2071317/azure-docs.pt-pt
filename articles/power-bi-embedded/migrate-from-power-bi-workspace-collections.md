---
title: Como migrar conteúdos da coleção de área de trabalho do Power BI para o Power BI Embedded | Documentos da Microsoft
description: Saiba como migrar de coleções de área de trabalho do Power BI para o Power BI Embedded e tire partido dos avanços de incorporação de conteúdos nas aplicações.
services: power-bi-embedded
documentationcenter: ''
author: markingmyname
manager: kfile
editor: ''
tags: ''
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/28/2017
ms.author: maghan
ms.openlocfilehash: de20d532112ca73f34f7cb603d043579c28179d6
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39071237"
---
# <a name="how-to-migrate-power-bi-workspace-collection-content-to-power-bi-embedded"></a>Como migrar conteúdos da coleção de área de trabalho do Power BI para o Power BI Embedded

Saiba como migrar de coleções de área de trabalho do Power BI para o Power BI Embedded. Este artigo fornece orientações para migrar de coleções de área de trabalho do Azure Power BI para o Power BI Embedded. Vamos também ver o que esperar para que as alterações de aplicação.

O recurso de coleções de área de trabalho do Power BI continua a estar disponível durante um período limitado, após o lançamento de disponibilidade geral do Power BI Premium. Os clientes tiverem um contrato Enterprise têm acesso às suas coleções de área de trabalho existentes por meio da expiração dos contratos existentes. Os clientes que adquiriram coleções de área de trabalho do Power BI através de canais diretos ou CSP usufruir do acesso durante um ano de disponibilidade geral do Power BI Premium.

> [!IMPORTANT]
> Enquanto a migração utiliza uma dependência no serviço Power BI, não existe uma dependência no Power BI para os utilizadores da sua aplicação ao utilizar um **token de incorporação**. Não é necessário para se inscrever no Power BI para ver o conteúdo incorporado na sua aplicação. Pode utilizar esta abordagem de incorporação incorpore o Power BI para os seus clientes.

![Fluxo do Power BI Embedded](media/migrate-from-power-bi-workspace-collections/powerbi-embed-flow.png)

## <a name="prepare-for-the-migration"></a>Preparar para a migração

Existem algumas coisas que precisa fazer para se preparar para migrar do serviço de coleções de área de trabalho do Power BI através de para o Power BI Embedded. Precisa de um inquilino disponível, bem como um utilizador que tenha uma licença do Power BI Pro.

1. Certifique-se de que tem acesso a um inquilino do Azure Active Directory (Azure AD).

    Qual o inquilino a utilizar?

    * Utilizar o seu inquilino do Power BI empresarial existente?
    * Utilizar um inquilino separado para a sua aplicação?
    * Utilizar um inquilino separado para cada cliente?

    Se optar por criar um novo inquilino para a sua aplicação ou para cada cliente, consulte um dos seguintes:

    * [Criar um inquilino do Azure Active Directory](https://powerbi.microsoft.com/documentation/powerbi-developer-create-an-azure-active-directory-tenant/)
    * [Como obter um inquilino do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant).

2. Crie um utilizador neste novo inquilino, que atua como a sua conta "principal" da aplicação. Que conta tem de se inscrever no Power BI e tem de ter uma licença do Power BI Pro atribuída ao mesmo.

## <a name="accounts-within-azure-ad"></a>Contas no Azure AD

As contas seguintes têm de existir no seu inquilino.

> [!NOTE]
> Estas contas têm de ter licenças do Power BI Pro para utilizar áreas de trabalho de aplicação.

1. Um utilizador de administrador do inquilino.

    Recomenda-se que a área de trabalho de aplicação incorporação tem o administrador de inquilino listado como um membro.

2. Contas para os analistas que criar o conteúdo.

    Estes utilizadores devem ser atribuídos a áreas de trabalho de aplicação conforme necessário.

3. Um aplicativo *mestre* conta de utilizador ou a conta de serviço.

    O back-end de aplicativos armazena as credenciais para esta conta. Utilizar o *mestre* de contas para obter um token do Azure AD para utilização com as APIs de REST do Power BI. Esta conta é utilizada para gerar o token de incorporação para a aplicação. O *mestre* conta tem de ser um administrador das áreas de trabalho de aplicação criadas para incorporar.

    **Esta conta é apenas uma conta de utilizador normal na sua organização que é utilizada para fins de incorporação.**

## <a name="app-registration-and-permissions"></a>Registo da aplicação e permissões

Para fazer chamadas de REST API, registe uma aplicação com o Azure AD. Configuração de adição é aplicada no portal do Microsoft Azure para além da página de registo de aplicação do Power BI. Para obter mais informações, consulte [registar uma aplicação do Azure AD para incorporar conteúdo do Power BI](https://powerbi.microsoft.com/documentation/powerbi-developer-register-app/).

A recomendação é registrar o aplicativo usando o aplicativo **mestre** conta.

## <a name="create-app-workspaces-required"></a>Criar áreas de trabalho de aplicação (obrigatórios)

Se a sua aplicação está a servir vários clientes, pode tirar partido das áreas de trabalho de aplicação para fornecer isolamento melhor. Dashboards e relatórios estariam isolados entre os seus clientes. Em seguida, poderia usar uma conta do Power BI por área de trabalho de aplicação para isolar ainda mais as experiências dos aplicativos entre os clientes, mas apenas pode utilizar uma conta para manter a simplicidade.

> [!IMPORTANT]
> Não é possível utilizar uma área de trabalho pessoal (um "Minha área de trabalho") para tirar partido da incorporação para os seus clientes.

Precisa de um utilizador que tenha uma licença Pro para criar uma área de trabalho de aplicação no Power BI. O utilizador do Power BI que cria a área de trabalho de aplicação é um administrador da área de trabalho por predefinição. **O aplicativo *mestre* conta tem de ser um administrador da área de trabalho.**

## <a name="content-migration"></a>Migração de conteúdo

Migração do conteúdo das suas coleções de área de trabalho para o Power BI Embedded pode ser feita em paralelo para a sua solução atual e não requer qualquer período de inatividade.

R **ferramenta de migração** está disponível para que possa utilizar para ajudar a copiar o conteúdo de coleções de área de trabalho do Power BI para o Power BI Embedded. Especialmente se tem muitos relatórios. Para obter mais informações, consulte [ferramenta de migração do Power BI Embedded](migrate-tool.md).

Migração de conteúdos baseia-se sobretudo em duas APIs.

1. Transferir PBIX – esta API pode transferir os ficheiros PBIX carregados no Power BI depois de Outubro de 2016.
2. Importar PBIX – esta API carrega ficheiros PBIX no Power BI.

Para alguns fragmentos de código relacionados, consulte [fragmentos de código para a migração de conteúdo do Power BI Embedded](migrate-code-snippets.md).

### <a name="report-types"></a>Tipos de relatório

Existem vários tipos de relatórios, cada um a solicitar um fluxo de migração diferentes.

#### <a name="cached-dataset-and-report"></a>Conjunto de dados em cache e relatório

Veja conjuntos de dados em cache para ficheiros PBIX com dados em vez de uma ligação em direto ou a ligação de DirectQuery importados.

**Fluxo**

1. Chame API transferir PBIX a partir de sua área de trabalho de coleção de área de trabalho do Power BI.
2. Guarde o PBIX.
3. Chame importar PBIX para a área de trabalho do Power BI Embedded.

#### <a name="directquery-dataset-and-report"></a>Relatório e conjunto de dados do DirectQuery

**Fluxo**

1. Chame GET `https://api.powerbi.com/v1.0/collections/{collection_id}/workspaces/{wid}/datasets/{dataset_id}/Default.GetBoundGatewayDataSources` e guarde a cadeia de ligação recebida.
2. Chame API transferir PBIX a partir de sua área de trabalho de coleção de área de trabalho do Power BI.
3. Guarde o PBIX.
4. Chame importar PBIX para a área de trabalho do Power BI Embedded.
5. Atualize a cadeia de ligação ao chamar - POST  `https://api.powerbi.com/v1.0/myorg/datasets/{dataset_id}/Default.SetAllConnections`
6. Obtenha o ID de ID de GW e a origem de dados ao chamar - introdução `https://api.powerbi.com/v1.0/myorg/datasets/{dataset_id}/Default.GetBoundGatewayDataSources`
7. Atualize as credenciais do utilizador ao chamar - PATCH `https://api.powerbi.com/v1.0/myorg/gateways/{gateway_id}/datasources/{datasource_id}`

#### <a name="old-dataset-and-reports"></a>Conjunto de dados antigo e relatórios

Relatórios carregados antes de Outubro de 2016 não suportam a funcionalidade de transferir PBIX. 

**Fluxo**

1. Obtenha o PBIX a partir do seu ambiente de desenvolvimento (seu controle de origem interno).
2. Chame importar PBIX para a área de trabalho do Power BI Embedded.

#### <a name="push-dataset-and-report"></a>Relatório e conjunto de dados Push

Transferir PBIX não suporta *Push API* conjuntos de dados. Dados de conjunto de dados da API push não podem ser transferidos a partir de coleções de área de trabalho do Power BI para o Power BI Embedded.

**Fluxo**

1. Chame a API "Criar conjunto de dados" com o conjunto de dados Json para criar o conjunto de dados para a área de trabalho do Power BI Embedded.
2. Reconstrua o relatório para o conjunto de dados criado *.

É possível utilizar algumas soluções para migrar o push relatório da api de coleções de área de trabalho do Power BI para o Power BI Embedded com o seguinte procedimento:

1. Carregue um PBIX fictício para a área de trabalho de coleção de área de trabalho do Power BI.
2. Clonar o push api de relatórios e vincule-ao PBIX fictício do passo 1.
3. Transferir o relatório da API com o PBIX fictício push.
4. Carregue o PBIX fictício para a área de trabalho do Power BI Embedded.
5. Crie conjunto de dados push na sua área de trabalho do Power BI Embedded.
6. Rebind relatório para o conjunto de dados da api push.

## <a name="create-and-upload-new-reports"></a>Criar e carregar novos relatórios

Além do conteúdo migrado a partir de coleções de área de trabalho do Power BI, pode criar os seus relatórios e conjuntos de dados com o Power BI Desktop e, em seguida, publicar esses relatórios numa área de trabalho de aplicação. O utilizador final, os relatórios de publicação tem de ter uma licença do Power BI Pro para poder publicar numa área de trabalho de aplicação.

## <a name="rebuild-your-application"></a>Reconstruir a aplicação

1. Modificar a sua aplicação para utilizar as APIs de REST do Power BI e a localização de relatórios no powerbi.com.

2. Reconstrua a sua autenticação AuthN/AuthZ com o *mestre* de conta para a sua aplicação. Pode tirar partido da utilização de um [token de incorporação](https://msdn.microsoft.com/library/mt784614.aspx) para permitir que este utilizador aja em nome de outros utilizadores.

3. Incorpore os relatórios do Power BI Embedded na sua aplicação. Para obter mais informações, consulte [como incorporar os dashboards do Power BI, relatórios e mosaicos](https://powerbi.microsoft.com/documentation/powerbi-developer-embedding-content/).

## <a name="map-your-users-to-a-power-bi-user"></a>Mapear os utilizadores para um utilizador do Power BI

Na sua aplicação, é mapear os utilizadores que gere na aplicação para um *mestre* credencial do Power BI para os objetivos da sua aplicação. As credenciais para esta Power BI *mestre* conta são armazenadas na sua aplicação e ser utilizado para criar tokens de incorporação.

## <a name="what-to-do-when-you-are-ready-for-production"></a>O que fazer quando estiver pronto para produção

Quando estiver pronto para mudar para produção, terá de fazer o seguinte:

- Se estiver a utilizar um inquilino separado para o desenvolvimento, em seguida, terá de certificar-se de que as áreas de trabalho de aplicação, juntamente com dashboards e relatórios, estão disponíveis no seu ambiente de produção. Certifique-se de que criou a aplicação no Azure AD para o seu inquilino de produção e atribuídas as permissões de aplicação corretas, conforme indicado no passo 1.

- Compre uma capacidade adequada às suas necessidades. Pode utilizar o [documento técnico de planeamento da capacidade de análise incorporada](https://aka.ms/pbiewhitepaper) para ajudar a compreender o que poderá ter. Quando estiver pronto para comprar, pode comprar um recurso do Power BI Embedded no portal do Azure.

- Editar a área de trabalho de aplicação e atribua-a uma capacidade em Avançadas.

    ![Atribuir a área de trabalho de aplicação a uma capacidade em powerbi.com](media/migrate-from-power-bi-workspace-collections/embedded-capacity.png)

- Envie a sua aplicação atualizada para produção e comece a incorporar relatórios do Power BI Embedded.

## <a name="after-migration"></a>Após a migração

Uma limpeza é necessária em coleções de área de trabalho do Power BI.

- Remova todas as áreas de trabalho da solução implementada no serviço do Azure de coleções de área de trabalho do Power BI.
- Elimine as coleções de área de trabalho existentes no Azure.

## <a name="next-steps"></a>Passos Seguintes

Parabéns! A aplicação foi agora migrada para o Power BI Embedded. Para obter informações sobre como incorporar os dashboards do Power BI, relatórios e conjuntos de dados, consulte [como incorporar os dashboards do Power BI, relatórios e mosaicos](https://powerbi.microsoft.com/documentation/powerbi-developer-embedding-content/).

Mais perguntas? [Experimente perguntar à Comunidade do Power BI](http://community.powerbi.com/)