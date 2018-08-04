---
title: Implementar o Azure Blockchain Workbench
description: Como implementar o Azure Blockchain Workbench
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 7/13/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: b65b6977dfe2043e260fd9c0eefd6038ca436d50
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39505819"
---
# <a name="deploy-azure-blockchain-workbench"></a>Implementar o Azure Blockchain Workbench

O Azure Blockchain Workbench é implementado com um modelo de solução no Azure Marketplace. O modelo simplifica a implementação de componentes necessários para criar aplicações de blockchain. Uma vez implantado, o Blockchain Workbench fornece acesso a aplicações de cliente para criar e gerir utilizadores e aplicações de blockchain.

Para obter mais informações sobre os componentes de Blockchain Workbench, consulte [arquitetura de Azure Blockchain Workbench](blockchain-workbench-architecture.md).

## <a name="prepare-for-deployment"></a>Preparar para a implementação

Blockchain Workbench permite-lhe implementar um livro razão de blockchain, juntamente com um conjunto de serviços do Azure relevantes mais frequentemente utilizada para criar uma aplicação baseada em blockchain. Implantar o Blockchain Workbench resulta nos seguintes serviços do Azure a ser aprovisionados num grupo de recursos na sua subscrição do Azure.

* Tópico do Event Grid 1 evento
* O espaço de nomes do 1 do Service Bus
* 1 application Insights
* 1 banco de dados do SQL (Standard S0)
* 2 serviços de aplicações (Standard)
* 2 cofres de chaves do azure
* 2 contas de armazenamento do azure (Standard LRS)
* 2 conjuntos de dimensionamento de máquinas virtuais (para nós de trabalho e a validação do)
* 2 redes virtuais (incluindo o Balanceador de carga, o grupo de segurança de rede e o endereço IP público para cada rede virtual)
* Opcional: Monitor do Azure

Segue-se um exemplo de implementação criado num **myblockchain** grupo de recursos.

![Exemplo de implementação](media/blockchain-workbench-deploy/example-deployment.png)

O custo de Blockchain Workbench é um agregado do custo dos serviços do Azure subjacentes. Informações sobre preços do serviços do Azure podem ser calculados utilizando a [Calculadora de preços](https://azure.microsoft.com/pricing/calculator/).

O Azure Blockchain Workbench requer vários pré-requisitos antes da implantação. Os pré-requisitos incluem registos de configuração e a aplicação do Azure AD.

### <a name="blockchain-workbench-api-app-registration"></a>Registo da aplicação API da bancada de trabalho de Blockchain

Implementação de Blockchain Workbench requer o registo de uma aplicação do Azure AD. Precisa de um inquilino do Azure Active Directory (Azure AD) para registar a aplicação. Pode utilizar um inquilino existente ou criar um novo inquilino. Se estiver a utilizar um inquilino do Azure AD existente, tem permissões suficientes para registar aplicações e conceder permissões da Graph API no inquilino do Azure AD. Se não tiver permissões suficientes no inquilino do Azure AD existente, crie um novo inquilino. 

> [!IMPORTANT]
> Bancada de trabalho não tem de ser implementado no mesmo inquilino que está a utilizar para registar uma aplicação do Azure AD. Bancada de trabalho tem de ser implementada num inquilino em que tem permissões suficientes para implementar recursos. Para obter mais informações sobre os inquilinos do Azure AD, consulte [como obter um inquilino do Active Directory](../active-directory/develop/quickstart-create-new-tenant.md) e [integrar aplicações com o Azure Active Directory](../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md).

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione a sua conta no canto superior direito canto e mude para o Azure AD pretendido de inquilino. O inquilino deve ser o inquilino do administrador de subscrição da subscrição em que a Bancada de trabalho é implementada e tem permissões suficientes para registar aplicações.
3. No painel de navegação do lado esquerdo, selecione o **do Azure Active Directory** serviço. Selecione **registos de aplicações** > **novo registo de aplicação**.

    ![Registo de aplicações](media/blockchain-workbench-deploy/app-registration.png)

4. Fornecer um **Name** e **URL de início de sessão** para a aplicação. Pode utilizar valores de marcador de posição, uma vez que os valores são alterados durante a implantação. 

    ![Criar registo de aplicações](media/blockchain-workbench-deploy/app-registration-create.png)

    |Definição  | Valor  |
    |---------|---------|
    |Nome | `Blockchain API` |
    |Tipo de aplicação |Aplicação/API Web|
    |URL de início de sessão | `https://blockchainapi` |

5. Selecione **criar** para registar a aplicação do Azure AD.

### <a name="modify-application-manifest"></a>Modificar o manifesto da aplicação

Em seguida, terá de modificar o manifesto de aplicação para utilizar funções de aplicação no Azure AD para especificar administradores do Blockchain Workbench.  Para obter mais informações sobre manifestos da aplicação, consulte [manifesto de aplicação do Azure Active Directory](../active-directory/develop/active-directory-application-manifest.md).

1. Para a aplicação que registou, selecione **manifesto** no painel de detalhes de aplicação registada.
2. Gere um GUID. Pode gerar um GUID usando o comando do PowerShell [guid]:: Novo_guid () ou o cmdlet New-GUID. Outra opção consiste em utilizar um Web site de gerador GUID.
3. Pretender atualizar o **appRoles** secção do manifesto. No painel de manifesto de edição, selecione **edite** e substitua `"appRoles": []` com o JSON fornecido. Certifique-se de que substitua o valor para o **id** campo com o GUID é gerado. 

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
    > O valor **administrador** é necessário para identificar os administradores de Blockchain Workbench.

4.  Clique em **guardar** para guardar as alterações de manifesto do aplicativo.

### <a name="add-graph-api-required-permissions"></a>Adicionar permissões da Graph API necessárias

A aplicação de API tem de pedir permissão ao utilizador para aceder ao diretório. Defina a seguinte permissão necessária para a aplicação de API:

1. No registo de aplicações API do Blockchain, selecione **definições > permissões obrigatórias > selecionar uma API > Microsoft Graph**.

    ![Selecionar uma API](media/blockchain-workbench-deploy/client-app-select-api.png)

    Clique em **Selecionar**.

2. Na **ativar o acesso ao** sob **permissões de aplicação**, escolha **ler perfis completos de todos os utilizadores**.

    ![Ativar o acesso](media/blockchain-workbench-deploy/client-app-read-perms.png)

    Clique em **selecionar** , em seguida, clique em **feito**.

3. Na **permissões obrigatórias**, selecione **conceder permissões** , em seguida, selecione **Sim** para o pedido de verificação.

   ![Conceder permissões](media/blockchain-workbench-deploy/client-app-grant-permissions.png)

   Concessão de permissão permite a Bancada de trabalho de Blockchain aceder aos utilizadores no diretório. A permissão de leitura é necessária para procurar e adicionar membros a Bancada de trabalho de Blockchain.

### <a name="add-graph-api-key-to-application"></a>Adicionar chave de API de gráfico à aplicação

Blockchain Workbench utiliza o Azure AD como o sistema de gestão de identidade principais para utilizadores que interagem com aplicações de blockchain. Para que o Blockchain Workbench aceder ao Azure AD e obter informações de utilizador, tais como nomes e os e-mails, terá de adicionar uma chave de acesso. Blockchain Workbench utiliza a chave para autenticar com o Azure AD.

1. Para a aplicação que registou, selecione **definições** no painel de detalhes de aplicação registada.
2. Selecione **Chaves**.
3. Adicione uma nova chave ao especificar uma chave **Descrição** e selecionando **expira** valor de duração. 

    ![Criar chave](media/blockchain-workbench-deploy/app-key-create.png)

    |Definição  | Valor  |
    |---------|---------|
    | Descrição | `Service` |
    | Expira em | Escolha uma duração de expiração |

4. Selecione **Guardar**. 
5. Copie o valor da chave e armazene-o para utilizar mais tarde. Precisa para a implementação.

    > [!IMPORTANT]
    >  Se não as guardar a chave para a implementação, terá de gerar uma nova chave. Não é possível obter o valor da chave mais tarde no portal.

### <a name="get-application-id"></a>Obter ID da aplicação

As informações de inquilino e ID de aplicação são necessários para a implementação. Recolher e armazenar as informações para utilização durante a implementação.

1. Para a aplicação que registou, selecione **configurações** > **propriedades**.
2.  Na **propriedades** painel, cópia e arquivo, os seguintes valores para utilizar mais tarde durante a implementação.

    ![Propriedades da aplicação API](media/blockchain-workbench-deploy/app-properties.png)

    | Definição para armazenar  | Utilizar na implementação |
    |------------------|-------------------|
    | ID da aplicação | A configuração do Azure Active Directory > ID de aplicação |

### <a name="get-tenant-domain-name"></a>Obter nome de domínio de inquilino

Recolher e armazenar o nome de domínio de inquilino do Active Directory onde as aplicações estão registadas. 

No painel de navegação do lado esquerdo, selecione o **do Azure Active Directory** serviço. Selecione **Nomes de domínio personalizados**. Copie e armazene o nome de domínio.

![Nome de domínio](media/blockchain-workbench-deploy/domain-name.png)

## <a name="deploy-blockchain-workbench"></a>Implementar o Blockchain Workbench

Depois dos passos de pré-requisitos foram concluídos, está pronto para implementar o Blockchain Workbench. As secções seguintes descrevem como implantar o framework.

1.  Inicie sessão no [portal do Azure](https://portal.azure.com).
2.  Selecione a sua conta no canto superior direito de canto e mude para o Azure AD pretendido do inquilino onde pretende implementar o Azure Blockchain Workbench.
3.  No painel esquerdo, selecione **criar um recurso**. Procure `Azure Blockchain Workbench` no **pesquisar no Marketplace** barra de pesquisa. 

    ![Barra de pesquisa do Marketplace](media/blockchain-workbench-deploy/marketplace-search-bar.png)

4.  Selecione **o Azure Blockchain Workbench**.

    ![Resultados da pesquisa de mercado](media/blockchain-workbench-deploy/marketplace-search-results.png)

4.  Selecione **Criar**.
5.  Conclua as definições básicas.

    ![Criar o Azure Blockchain Workbench](media/blockchain-workbench-deploy/blockchain-workbench-settings-basic.png)

    | Definição | Descrição  |
    |---------|--------------|
    | Prefixo de recursos | Identificador exclusivo curto para a sua implementação. Este valor é utilizado como base para atribuir nomes a recursos. |
    | Nome de utilizador VM | O nome de utilizador é utilizado como administrador para todas as máquinas virtuais (VM). |
    | Tipo de autenticação | Selecione se pretende utilizar uma palavra-passe ou a chave para ligar a VMs. |
    | Palavra-passe | A palavra-passe é utilizada para ligar a VMs. |
    | SSH | Utilizar uma chave pública RSA no início com o formato de linha única **ssh-rsa** ou utilize o formato PEM com várias linha. Pode gerar chaves SSH com `ssh-keygen` em Linux e OS X ou PuTTYGen no Windows. Obter mais informações sobre chaves SSH, veja [chaves de como utilizar SSH com Windows no Azure](../virtual-machines/linux/ssh-from-windows.md). |
    | Palavra-passe de base de dados / Confirmar palavra-passe de base de dados | Especifique a palavra-passe a utilizar para acessar o banco de dados criado como parte da implementação. |
    | Região de implementação | Especifique onde pretende implementar recursos de Blockchain Workbench. Para uma melhor disponibilidade, isto deve corresponder a **localização** definição. |
    | Subscrição | Especifique a subscrição do Azure que pretende utilizar para a sua implementação. |
    | Grupos de recursos | Criar um novo grupo de recursos, selecionando **criar novo** e especifique um nome de grupo de recursos exclusivo. |
    | Localização | Especifica a região que pretende implementar a estrutura. |

6.  Selecione **OK** para concluir a seção de configuração da definição básico.

7.  Concluir o **configuração do Azure Active Directory**.

    ![Configuração do Azure AD](media/blockchain-workbench-deploy/blockchain-workbench-settings-aad.png)

    | Definição | Descrição  |
    |---------|--------------|
    | Nome do Domínio | Inquilino de utilização do Azure AD recolhidos no [nome de domínio de inquilino de Get](#get-tenant-domain-name) secção pré-requisitos. |
    | ID da aplicação | Utilize o ID de aplicação do registo de aplicações de cliente de Blockchain recolhidos no [obter ID da aplicação](#get-application-id) secção pré-requisitos. |
    | Chave da Aplicação | Utilizar a chave da aplicação de registo de aplicações de cliente Blockchain recolhido no [chave de API de gráfico de adicionar a aplicação](#add-graph-api-key-to-application) secção pré-requisitos. |


8.  Clique em **OK** para concluir a seção de configuração de parâmetros do Azure AD.

9.  Na **definições de rede e desempenho**, escolha se pretende criar uma nova rede de blockchain ou utilizar uma rede de uma prova da autoridade blockchain existente.

    Para **criar novo**:

    O *criar um novo* opção cria um conjunto de nós de prova de Ethereum autoridade (PoA) numa subscrição de um único membro. 

    ![Definições de rede e desempenho](media/blockchain-workbench-deploy/blockchain-workbench-settings-network-new.png)

    | Definição | Descrição  |
    |---------|--------------|
    | Número de nós de blockchain | Escolha o número de Ethereum PoA nós de validador para ser implantado em sua rede. |
    | Desempenho de armazenamento | Escolha o desempenho de armazenamento VM preferencial para a sua rede de blockchain. |
    | Tamanho da máquina virtual | Escolha o tamanho da VM preferencial para a sua rede de blockchain. |

    Para **utilizar existente**:

    O *utilizar existente* opção permite-lhe especificar uma rede de blockchain Ethereum prova-of-Authority (PoA). Pontos finais têm os seguintes requisitos.

    * O ponto final tem de ser uma rede de blockchain Ethereum prova-of-Authority (PoA).
    * O ponto final tem de ser publicamente acessível através da rede.
    * A rede de blockchain PoA deve ser configurada para ter o preço de gás definido para zero (Nota: não são financiadas a contas de Blockchain Workbench. Se os fundos são necessários, as transações falharem).

    ![Definições de rede e desempenho](media/blockchain-workbench-deploy/blockchain-workbench-settings-network-existing.png)

    | Definição | Descrição  |
    |---------|--------------|
    | Ponto de extremidade Ethereum RPC | Forneça o ponto de extremidade RPC de uma rede de blockchain PoA existente. O ponto final começa com http:// e termina com um número de porta. Por exemplo, `http://contoso-chain.onmicrosoft.com:8545` |
    | Desempenho de armazenamento | Escolha o desempenho de armazenamento VM preferencial para a sua rede de blockchain. |
    | Tamanho da máquina virtual | Escolha o tamanho da VM preferencial para a sua rede de blockchain. |

10. Selecione **OK** para concluir as definições de rede e o desempenho.

11. Concluir o **do Azure Monitor** definições.

    ![Azure Monitor](media/blockchain-workbench-deploy/blockchain-workbench-settings-oms.png)

    | Definição | Descrição  |
    |---------|--------------|
    | Monitorização | Escolha se pretende ativar o Azure Monitor para monitorizar a sua rede de blockchain |
    | Ligar à instância existente do Log Analytics | Escolha se pretende utilizar um existente do Log Analytics de instância ou criar um novo. Se utilizar uma instância existente, introduza o seu ID de área de trabalho e a chave primária. |

12. Clique em **OK** para concluir a secção de Azure Monitor.

13. Reveja o resumo para verificar que os parâmetros são precisos.

    ![Resumo](media/blockchain-workbench-deploy/blockchain-workbench-summary.png)

14. Selecione **criar** para aceitar os termos e implementar o Azure Blockchain Workbench.

A implementação pode demorar até 90 minutos. Pode utilizar o portal do Azure para monitorizar o progresso. No grupo de recursos criado recentemente, selecione **implementações > Descrição geral** para ver o estado dos artefatos implementados.

## <a name="blockchain-workbench-web-url"></a>URL de Web de Blockchain Workbench

Depois de concluída a implementação da bancada de trabalho de Blockchain, um novo grupo de recursos contém recursos do Blockchain Workbench. Serviços de Blockchain Workbench são acedidos através de um URL de web. Os passos seguintes mostram como obter o URL de web do framework implantado.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No painel de navegação do lado esquerdo, selecione **grupos de recursos**
3. Escolha o nome do grupo de recursos que especificou ao implementar o Blockchain Workbench.
4. Clique nas **tipo** cabeçalho de coluna para ordenar a lista por ordem alfabética por tipo.
5. Existem dois recursos com tipo **serviço de aplicações**. Selecione o tipo de recurso **serviço de aplicações** *sem* o "-api" sufixo.

    ![Lista de aplicações do serviço](media/blockchain-workbench-deploy/resource-group-list.png)

6.  No serviço de aplicações **Essentials** secção, copie a **URL** valor, que representa o URL da web para o seu Blockchain Workbench implementado.

    ![Princípios básicos do serviço de aplicações](media/blockchain-workbench-deploy/app-service.png)

Para associar um nome de domínio personalizado com o Blockchain Workbench, veja [configurar um nome de domínio personalizado para uma aplicação web no serviço de aplicações do Azure utilizando o Gestor de tráfego](../app-service/web-sites-traffic-manager-custom-domain-name.md).

## <a name="configuring-the-reply-url"></a>Configurar o URL de resposta

Depois de ter sido implementado o Azure Blockchain Workbench, a próxima etapa é certificar-se de que a aplicação de cliente do Azure Active Directory (Azure AD) está registada para o correto **URL de resposta** da bancada de trabalho de Blockchain implementado URL da web.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Certifique-se de que está no inquilino onde que registou a aplicação de cliente do Azure AD.
3. No painel de navegação do lado esquerdo, selecione o **do Azure Active Directory** serviço. Selecione **Registos das aplicações**.
4. Selecione a aplicação de cliente do Azure AD que registou na secção de pré-requisitos.
5. Selecione **definições > URLs de resposta**.
6. Especifique o URL da web principal da implementação do Azure Blockchain Workbench, que obteve na **obter o URL de Web do Azure Blockchain Workbench** secção. O URL de resposta é o prefixo `https://`. Por exemplo, `https://myblockchain2-7v75.azurewebsites.net`

    ![URLs de Resposta](media/blockchain-workbench-deploy/configure-reply-url.png)

7. Selecione **guardar** para atualizar o registo de cliente.

## <a name="remove-a-deployment"></a>Remover uma implementação

Quando já não é necessária uma implementação, pode remover uma implementação ao eliminar o grupo de recursos de Blockchain Workbench.

1. No portal do Azure, navegue até **grupo de recursos** no painel de navegação esquerda e selecione o grupo de recursos que pretende eliminar. 
2. Selecione **Eliminar grupo de recursos**. Verifique a eliminação ao introduzir o nome do grupo de recursos e selecione **eliminar**.

    ![Eliminar grupo de recursos](media/blockchain-workbench-deploy/delete-resource-group.png)

## <a name="next-steps"></a>Passos Seguintes

Neste artigo de procedimentos, implementou o Azure Blockchain Workbench. Para saber como criar uma aplicação de blockchain, avance para o próximo artigo de procedimento.

> [!div class="nextstepaction"]
> [Criar uma aplicação de blockchain no Azure Blockchain Workbench](blockchain-workbench-create-app.md)
