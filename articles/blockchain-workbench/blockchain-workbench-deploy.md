---
title: Implementar o Azure Blockchain Workbench
description: Como implementar o Azure Blockchain Workbench
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 4/22/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 85a627678f862d783d47013d82bae8b485d7d4e9
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="deploy-azure-blockchain-workbench"></a>Implementar o Azure Blockchain Workbench

Azure Blockchain Workbench é implementada através de um modelo de solução no Azure Marketplace. O modelo simplifica a implementação dos componentes necessários para criar aplicações blockchain. Depois de implementada, o Blockchain Workbench fornece acesso a aplicações de cliente para criar e gerir utilizadores e aplicações blockchain.

Para obter mais informações sobre os componentes do Blockchain Workbench, consulte [arquitetura Azure Blockchain Workbench](blockchain-workbench-architecture.md).

## <a name="prepare-for-deployment"></a>Preparar para a implementação

Azure Blockchain Workbench requer vários pré-requisitos antes da implementação. Os pré-requisitos incluem registos de configuração e a aplicação do Azure AD.

### <a name="blockchain-workbench-api-app-registration"></a>Registo de aplicação Blockchain Workbench API

Implementação de Blockchain Workbench necessita de registo de uma aplicação do Azure AD. Precisa de um inquilino do Azure Active Directory (Azure AD) para registar a aplicação. Pode utilizar um inquilino existente ou criar um novo inquilino. Se estiver a utilizar um inquilino do Azure AD existente, necessita de permissões suficientes para registar a aplicações dentro de um inquilino do Azure AD. Os registos de aplicações têm de ser no inquilino do administrador de subscrição da subscrição em que é implementado o Workbench. Para obter mais informações sobre inquilinos do Azure AD, consulte [como obter um inquilino do Active Directory](../active-directory/develop/active-directory-howto-tenant.md) e [integrar aplicações com o Azure Active Directory](../active-directory/develop/active-directory-integrating-applications.md).

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione a conta na parte superior direita canto e mudar para o Azure AD pretendido de inquilino. O inquilino deve ser o inquilino do administrador de subscrição da subscrição onde Workbench é implementada e tem permissões suficientes para registar as aplicações.
3. No painel de navegação esquerdo, selecione o **do Azure Active Directory** serviço. Selecione **registos de aplicação** > **novo registo de aplicação**.

    ![Registo de aplicações](media/blockchain-workbench-deploy/app-registration.png)

4. Forneça um **nome** e **URL de início de sessão** para a aplicação. Pode utilizar valores de marcador de posição, uma vez que os valores são alterados durante a implementação. 

    ![Criar registo de aplicação](media/blockchain-workbench-deploy/app-registration-create.png)

    |Definição  | Valor  |
    |---------|---------|
    |Nome | `Blockchain API` |
    |Tipo de aplicação |Aplicação/API Web|
    |URL de início de sessão | `https://blockchainapi` |

5. Selecione **criar** para registar a aplicação do Azure AD.

### <a name="modify-application-manifest"></a>Modificar o manifesto da aplicação

Em seguida, terá de modificar o manifesto da aplicação para utilizar funções da aplicação no Azure AD para especificar administradores Blockchain Workbench.  Para mais informações sobre os manifestos da aplicação, consulte [manifesto da aplicação do Azure Active Directory](../active-directory/develop/active-directory-application-manifest.md).

1. Para a aplicação, registado, selecione **manifesto** no painel de detalhes de aplicação registada.
2. Gere um GUID. Pode utilizar o comando do PowerShell `[guid]::NewGuid()` ou ferramentas de online para gerar um GUID. 
3. Vai atualizar o **appRoles** secção do manifesto. No painel de manifesto de edição, selecione **editar** e substitua `"appRoles": []` com o JSON fornecido. Não se esqueça de substituir o valor para o **id** campo com o GUID é gerado. 

    ``` json
    "appRoles": [
         {
           "allowedMemberTypes": [
             "User",
             "Application"
           ],
           "displayName": "Administrator",
           "id": "<A unique GUID>",
           "isEnabled": true,
           "description": "Blockchain Workbench administrator role allows creation of applications, user to role assignments, etc.",
           "value": "Administrator"
         }
       ],
    ```

    ![Editar manifesto](media/blockchain-workbench-deploy/edit-manifest.png)

    > [!IMPORTANT]
    > O valor **administrador** é necessária para identificar os administradores de Blockchain Workbench.

4.  Clique em **guardar** para guardar as alterações de manifesto de aplicação.

### <a name="add-graph-api-required-permissions"></a>Adicionar permissões da Graph API necessárias

A aplicação de API tem de solicitar permissão ao utilizador para aceder ao diretório. Defina a permissão necessária seguinte para a aplicação de API:

1. No registo de aplicação Blockchain API, selecione **definições > as permissões necessárias > selecionar um API > Microsoft Graph**.

    ![Selecionar uma API](media/blockchain-workbench-deploy/client-app-select-api.png)

    Clique em **Selecionar**.

2. No **ativar o acesso ao** em **permissões de aplicação**, escolha **leia perfis completos de todos os utilizadores**.

    ![Ativar o acesso](media/blockchain-workbench-deploy/client-app-read-perms.png)

    Clique em **selecione** , em seguida, clique em **feito**.

3. No **as permissões necessárias**, selecione **conceder permissões** , em seguida, selecione **Sim** para o pedido de verificação.

   ![Conceder permissões](media/blockchain-workbench-deploy/client-app-grant-permissions.png)

   Conceder permissão permite Blockchain Workbench aceder aos utilizadores no diretório. É necessária a permissão de leitura para procurar e adicionar membros a Blockchain Workbench.

### <a name="add-graph-api-key-to-application"></a>Adicionar chave de API de gráfico à aplicação

Blockchain Workbench utiliza o Azure AD como o sistema de gestão de identidade principal para os utilizadores interajam com blockchain aplicações. Para o Blockchain Workbench para aceder ao Azure AD e obter informações de utilizador, tais como nomes e os e-mails, é necessário adicionar uma chave de acesso. Blockchain Workbench utiliza a chave para autenticar com o Azure AD.

1. Para a aplicação, registado, selecione **definições** no painel de detalhes de aplicação registada.
2. Selecione **Chaves**.
3. Adicionar uma nova chave, especificando uma chave **Descrição** e escolher **expira** um valor de duração. 

    ![Criar chave](media/blockchain-workbench-deploy/app-key-create.png)

    |Definição  | Valor  |
    |---------|---------|
    | Descrição | `Service` |
    | Expira em | Escolha uma duração de expiração |

4. Selecione **Guardar**. 
5. Copie o valor da chave e guarde-o para utilizar mais tarde. É necessário para a implementação.

    > [!IMPORTANT]
    >  Se não guarde a chave para a implementação, terá de gerar uma nova chave. Não é possível obter o valor da chave do portal mais tarde.

### <a name="get-application-id"></a>Obter ID da aplicação

As informações de inquilino e ID de aplicação são necessários para implementação. Recolher e armazenar as informações de utilização durante a implementação.

1. Para a aplicação, registado, selecione **definições** > **propriedades**.
2.  No **propriedades** painel, copiar e arquivo os seguintes valores para utilização posterior durante a implementação.

    ![Propriedades da aplicação API](media/blockchain-workbench-deploy/app-properties.png)

    | Definição para armazenar  | Utilizar na implementação |
    |------------------|-------------------|
    | ID da aplicação | A configuração do Active Directory do Azure > ID da aplicação |

### <a name="get-tenant-domain-name"></a>Obter nome de domínio de inquilino

Recolher e armazenar o nome de domínio de inquilino do Active Directory onde as aplicações estão registadas. 

No painel de navegação esquerdo, selecione o **do Azure Active Directory** serviço. Selecione **nomes de domínio personalizado**. Copie e armazenar o nome de domínio.

![Nome de domínio](media/blockchain-workbench-deploy/domain-name.png)

## <a name="deploy-blockchain-workbench"></a>Implementar Blockchain Workbench

Depois dos passos de pré-requisitos foram concluídos, está pronto para implementar o Blockchain Workbench. As secções seguintes descrevem como implementar a estrutura.

1.  Inicie sessão no [portal do Azure](https://portal.azure.com).
2.  Selecione a sua conta no canto superior direito canto e mudar para o Azure AD pretendido inquilino onde pretende implementar o Azure Blockchain Workbench.
3.  No painel esquerdo, selecione **crie um recurso**. Procurar `Azure Blockchain Workbench` no **procurar no Marketplace** barra de pesquisa. 

    ![Barra de pesquisa do Marketplace](media/blockchain-workbench-deploy/marketplace-search-bar.png)

4.  Selecione **Azure Blockchain Workbench**.

    ![Resultados de pesquisa do Marketplace](media/blockchain-workbench-deploy/marketplace-search-results.png)

4.  Selecione **Criar**.
5.  Conclua as definições básicas.

    ![Criar Azure Blockchain Workbench](media/blockchain-workbench-deploy/blockchain-workbench-settings-basic.png)

    | Definição | Descrição  |
    |---------|--------------|
    | Prefixo de recursos | Identificador exclusivo curto para a sua implementação. Este valor é utilizado como base para recursos de atribuição de nomes. |
    | Nome de utilizador VM | O nome de utilizador é utilizado como administrador para todas as máquinas virtuais (VM). |
    | Tipo de autenticação | Selecione se pretende utilizar uma palavra-passe ou chave para ligar a VMs. |
    | Palavra-passe | A palavra-passe é utilizada para ligar a VMs. |
    | SSH | Utilizar uma chave pública RSA no início do formato de linha única com **ssh-rsa** ou utilize o formato PEM com várias linha. Pode gerar chaves SSH utilizando `ssh-keygen` no Linux e OS X ou PuTTYGen no Windows. Obter mais informações sobre chaves SSH, consulte [das chaves de como utilizar o SSH com o Windows no Azure](../virtual-machines/linux/ssh-from-windows.md). |
    | Base de dados de palavra-passe / Confirmar palavra-passe de base de dados | Especifique a palavra-passe Utilize para aceder à base de dados criada como parte da implementação. |
    | Região de implementação | Especifique onde pretende implementar recursos Blockchain Workbench. Para uma melhor disponibilidade, isto deve corresponder a **localização** definição. |
    | Subscrição | Especifique a subscrição do Azure que pretende utilizar para a sua implementação. |
    | Grupos de recursos | Criar um novo grupo de recurso selecionando **criar nova** e especifique um nome de grupo de recursos exclusiva. |
    | Localização | Especificar a região que pretende implementar a estrutura. |

6.  Selecione **OK** para concluir a secção de configuração da definição básico.

7.  Concluir o **a configuração do Azure Active Directory**.

    ![Configuração do Azure AD](media/blockchain-workbench-deploy/blockchain-workbench-settings-aad.png)

    | Definição | Descrição  |
    |---------|--------------|
    | Nome do Domínio | O inquilino de utilizar o Azure AD recolhidos no [nome de domínio do inquilino Get](#get-tenant-domain-name) secção pré-requisitos. |
    | ID da aplicação | Utilize o ID da aplicação do registo de aplicação de cliente Blockchain recolhidos no [obter ID da aplicação](#get-application-id) secção pré-requisitos. |
    | Chave da Aplicação | Utilize a chave de aplicações do registo de aplicação de cliente Blockchain recolhido no [chave de API do gráfico de adicionar a aplicação](#add-graph-api-key-to-application) secção pré-requisitos. |


8.  Clique em **OK** para concluir a secção de configuração de parâmetros do Azure AD.

9.  Concluir o **tamanho e o desempenho de rede** definições.

    ![Definições de rede e de desempenho](media/blockchain-workbench-deploy/blockchain-workbench-settings-network.png)

    | Definição | Descrição  |
    |---------|--------------|
    | Número de nós de blockchain | Escolha o número de Ethereum PoA nós de validação para ser implementada na sua rede. |
    | Desempenho de armazenamento | Escolha o desempenho de armazenamento VM preferido para a sua rede blockchain. |
    | Tamanho da máquina virtual | Escolha o tamanho da VM preferido para a sua rede blockchain. |

10. Selecione **OK** para concluir a secção de tamanho e o desempenho de rede.

11. Concluir o **Azure Monitor** definições.

    ![Azure Monitor](media/blockchain-workbench-deploy/blockchain-workbench-settings-oms.png)

    | Definição | Descrição  |
    |---------|--------------|
    | Monitorização | Escolha se pretende ativar a monitorização do Azure monitorizar a rede blockchain |
    | Ligar à instância de análise de registos existente | Escolha se pretende utilizar um existente análise de registos de instância ou crie um novo. Se utilizar uma instância existente, introduza o seu ID da área de trabalho e a chave primária. |

12. Clique em **OK** para concluir a secção de Monitor do Azure.

13. Reveja o resumo para verificar que os parâmetros estão corretos.

    ![Resumo](media/blockchain-workbench-deploy/blockchain-workbench-summary.png)

14. Selecione **criar** para aceitar os termos e implementar o Blockchain Workbench do Azure.

A implementação pode demorar até 90 minutos. Pode utilizar o portal do Azure para monitorizar o progresso. No grupo de recursos criado de novo, selecione **implementações > Descrição geral** para ver o estado dos artefactos implementados.

## <a name="blockchain-workbench-web-url"></a>URL de Web Blockchain Workbench

Depois de concluída a implementação do Blockchain Workbench, um novo grupo de recursos contém recursos da sua Blockchain Workbench. Serviços de Blockchain Workbench são acedidos através de um URL de web. Os passos seguintes mostram como obter o URL de web do framework implementado.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No painel de navegação esquerdo, selecione **grupos de recursos**
3. Escolha o nome do grupo de recursos que especificou ao implementar Blockchain Workbench.
4. Clique em de **tipo** no cabeçalho de coluna para ordenar a lista por ordem alfabética por tipo.
5. Existem dois recursos com o tipo **do serviço de aplicações**. Selecione o recurso do tipo **do serviço de aplicações** *sem* o "-api" sufixo.

    ![Lista de serviço de aplicações](media/blockchain-workbench-deploy/resource-group-list.png)

6.  No App Service **Essentials** secção, copie o **URL** valor, que representa o URL da web para o Blockchain Workbench implementado.

    ![Essentials do serviço de aplicações](media/blockchain-workbench-deploy/app-service.png)

Para associar um nome de domínio personalizado Blockchain Workbench, consulte [configurar um nome de domínio personalizado para uma aplicação web no serviço de aplicações do Azure utilizando o Gestor de tráfego](../app-service/web-sites-traffic-manager-custom-domain-name.md).

## <a name="configuring-the-reply-url"></a>Configurar o URL de resposta

Depois de ter sido implementado o Blockchain Workbench do Azure, o próximo passo é assegurar-se de que a aplicação de cliente do Azure Active Directory (Azure AD) está registada para o correto **URL de resposta** do Blockchain Workbench implementado URL de web.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Certifique-se de que está no inquilino onde registou a aplicação de cliente do Azure AD.
3. No painel de navegação esquerdo, selecione o **do Azure Active Directory** serviço. Selecione **Registos das aplicações**.
4. Selecione a aplicação de cliente do Azure AD que registou na secção de pré-requisitos.
5. Selecione **definições > responder URLs**.
6. Especifique o URL de web principal da implementação do Azure Blockchain Workbench que obteve no **obter o URL de Web do Azure Blockchain Workbench** secção. O URL de resposta é o prefixo `https://`. Por exemplo, `https://myblockchain2-7v75.azurewebsites.net`

    ![URLs de Resposta](media/blockchain-workbench-deploy/configure-reply-url.png)

7. Selecione **guardar** para atualizar o registo de cliente.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo procedimentos, implementou Azure Blockchain Workbench. Para saber como criar uma aplicação blockchain, avance para o seguinte artigo de procedimentos.

> [!div class="nextstepaction"]
> [Criar uma aplicação de blockchain no Azure Blockchain Workbench](blockchain-workbench-create-app.md)