---
title: "Descrição geral de identidade para a pilha do Azure | Microsoft Docs"
description: Saiba mais sobre os sistemas de identidade que pode utilizar com a pilha do Azure.
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 2/22/2018
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 609ea3300806f3cfed4a3285a096f59be491c684
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/24/2018
---
# <a name="overview-of-identity-for-azure-stack"></a>Descrição geral de identidade para a pilha do Azure

Necessita de pilha do Azure AD do Azure ou através do Active Directory (AD) como um fornecedor de identidade de segurança de serviços de Federated do Active Directory (AD FS). Os seguintes conceitos e detalhes de autorização podem ajudar a tornar a escolha entre fornecedores de identidade. A escolha de um fornecedor é uma decisão Monouso que efetuar quando implementa a pilha de Azure pela primeira vez.  

À sua escolha entre o Azure AD e AD FS poderão ser limitado pelo modo das quais implementa a pilha do Azure: 
- Quando implementar num modo ligado, pode utilizar o Azure AD ou AD FS. 
- Quando implementar num modo desligado sem uma ligação à Internet, é suportado apenas do AD FS.

Para obter mais informações sobre estas opções, veja os artigos seguintes, dependendo do ambiente de pilha do Azure:
- Kit de implementação de pilha do Azure, consulte [considerações de identidade](azure-stack-datacenter-integration.md#identity-considerations).
- Sistemas de pilha integrada do Azure, consulte [decisões sobre a pilha do Azure de planeamento de implementação integrado sistemas](azure-stack-deployment-decisions.md).

 
## <a name="common-concepts-for-identity"></a>Conceitos comuns para a identidade
As secções seguintes abordam conceitos comuns para fornecedores de identidade e a sua utilização na pilha do Azure.
![Terminologia para fornecedores de identidade](media/azure-stack-identity-overview/terminology.png)


### <a name="directories-tenants-and-organizations"></a>Os inquilinos de diretórios e organizações
Um diretório é um contentor que contém informações sobre *utilizadores*, *aplicações*, *grupos*, e *principais de serviço*.
 
Um inquilino de diretório é um *organização*, como o Microsoft ou a sua própria empresa. 
- Azure AD suporta vários inquilinos e pode suportar a várias organizações, cada uma no seu diretório. Se utilizar o Azure AD e tiver vários inquilinos, pode conceder aos utilizadores do acesso de um inquilino outros inquilinos desse mesmo diretório e aplicações.
- AD FS suporta apenas um único inquilino e, por conseguinte, uma única organização. 

### <a name="users-and-groups"></a>Utilizadores e grupos
Contas de utilizador (identidades) são contas padrão que autenticar indivíduos com um ID de utilizador e palavra-passe. Os grupos podem incluir outros grupos ou utilizadores. 

Como criar e gerir utilizadores e grupos depende a solução de identidade que utilizar. 

Na pilha do Azure, as contas de utilizador: 
- São criados no  *username@domain*  formato. Apesar do AD FS mapeia as contas de utilizador para um Active Directory, AD FS não suporta a utilização do  _&lt;domínio >\<alias >_ formato. 
- Pode ser configurado para utilizar a autenticação multifator. 
- Estão limitadas para o diretório onde se primeiro de registar, que é o respetivo diretório organizações.
- Podem ser importados a partir dos diretórios no local. Para obter mais informações, consulte [integrar os diretórios no local ao Azure Active Directory](/azure/active-directory/connect/active-directory-aadconnect) na documentação do Azure.  

Quando inicia sessão no portal de inquilinos a organizações, utilize https://portal.local.azurestack.external. 

### <a name="guest-users"></a>Utilizadores convidados
Os utilizadores convidados são contas de utilizador de outros inquilinos do diretório que tenham sido concedidos acesso aos recursos no seu diretório. Para suportar o utilizador convidado, tem de utilizar o Azure AD e ativar o suporte para vários inquilinos. Quando suportado, pode convidar um utilizador convidado para aceder a recursos no seu inquilino de diretório, permitindo colaboração com organizações fora. 

Para convidar utilizadores convidados, podem utilizar os operadores da nuvem e os utilizadores [colaboração B2B do Azure AD](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Convidou utilizadores obter acesso a documentos, recursos e aplicações do seu diretório, enquanto mantém o controlo sobre os seus próprios recursos e os dados. 

Como um utilizador convidado, podem iniciar sessão para outro inquilino de diretório de organizações. Para tal, tem de acrescentar esse nome de diretório de organizações para o portal de URL.  Por exemplo se tiver de pertencer ao contoso.com, mas pretende iniciar sessão para o diretório da Fabrikam, utilize https://portal.local.azurestack.external/fabrikam.onmicrosoft.com.

### <a name="applications"></a>Aplicações
Pode registar aplicações para o Azure AD ou AD FS e, em seguida, proporciona-lhes para utilizadores na sua organização. 

Aplicações incluem:
- **Aplicação Web** – os exemplos incluem o portal do Azure e o Azure Resource Manager.  Suportam chamadas de Web API. 
- **O Native client** – os exemplos incluem o Azure PowerShell, o Visual Studio e interface de linha de comandos (CLI) do Azure.

As aplicações podem suportar dois tipos de inquilinos: 
- **Único inquilino** as aplicações suportam o utilizadores e serviços apenas a partir do mesmo diretório em que a aplicação está registada. 

  > [!NOTE]    
  > Como o AD FS suporta apenas um único diretório, as aplicações que cria uma topologia do AD FS são por predefinição, as aplicações de inquilino único.
- **Multi-inquilino** utilização de suporte de aplicações por utilizadores e serviços do diretório onde a aplicação é registada e de diretórios do inquilino adicionais.  Com aplicações de multi-inquilinos, os utilizadores de outra podem de diretório (outro inquilino do Azure AD) do inquilino a iniciar sessão para a aplicação.     

  Para obter mais informações sobre vários inquilinos, consulte [ativar vários inquilinos](azure-stack-enable-multitenancy.md).  

  Para obter mais informações sobre como desenvolver uma aplicação multi-inquilino, consulte [aplicações multi-inquilinos](/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview).

Ao registar uma aplicação, são criados dois objetos de:
- **Objeto de aplicação** – o objeto de aplicação é a representação global da aplicação em todos os inquilinos. Esta relação é um para um com a aplicação de software e apenas no diretório onde a aplicação está regista primeiro não existe.

 
   
- **Objeto principal do serviço** – um principal de serviço é uma credencial que é criada para uma aplicação no diretório onde a aplicação primeiro está registada. Um principal de serviço também é criado no diretório de cada inquilino adicional em que é utilizada essa aplicação. Esta relação pode ser um um-para-muitos com a aplicação de software.   

Para saber mais sobre a aplicação e objetos de principal de serviço, consulte o artigo [aplicação e objetos de principal de serviço no Azure Active Directory (Azure AD)](/azure/active-directory/develop/active-directory-application-objects). 

### <a name="service-principals"></a>Principais de serviço 
Um principal de serviço é um conjunto de *credenciais* para uma aplicação ou serviço que concedam acesso a recursos na pilha do Azure. Utilização de um principal de serviço separa as permissões de aplicação de permissões do utilizador que utiliza a aplicação.

Um principal de serviço é criado em cada inquilino onde a aplicação é utilizada. O principal de serviço estabelece uma identidade para início de sessão e acesso a recursos (como os utilizadores) que estão protegidos por esse inquilino.   

- A aplicação de uma único inquilino tem apenas um principal de serviço, no diretório em que é criado pela primeira vez.  Este principal de serviço é criado e permitir a utilizar durante o registo da aplicação. 
- Uma aplicação de Web do multi-inquilino ou a API tem um principal de serviço criado em cada inquilino onde um utilizador esse inquilino permitir a utilização desta aplicação.  

As credenciais de principais de serviço podem ser chave (gerado através do portal do Azure) ou um certificado.  Utilização de um certificado é adequada para a automatização porque são considerados certificados mais seguros do que a utilização de chaves. 


> [!NOTE]    
> Quando utilizar o AD FS com pilha do Azure, apenas o administrador pode criar principais de serviço. Com o AD FS, os principais de serviço necessitam de certificados e são criadas através do ponto final com privilégios (PEP). Para obter mais informações, consulte, [fornecer acesso de aplicações do Azure pilha](azure-stack-create-service-principals.md).

Para mais informações sobre principais de serviço para a pilha do Azure, consulte o artigo [criar principais de serviço](azure-stack-create-service-principals.md).


### <a name="services"></a>Serviços
Serviços na pilha do Azure que interagem com o fornecedor de identidade, estão registados como aplicações com o fornecedor de identidade. Como aplicações, o registo permite um serviço autenticar com o sistema de identidade. 

Todos os serviços do Azure utilizam [OpenID Connect](/azure/active-directory/develop/active-directory-protocols-openid-connect-code) protocolos e [JSON Web Tokens](/azure/active-directory/develop/active-directory-token-and-claims) (JWT) para estabelecer a identidade. Devido à utilização consistente de protocolos pelo Azure AD e AD FS, pode utilizar o Azure [Active Directory Authentication Library](/azure/active-directory/develop/active-directory-authentication-libraries) (ADAL) para autenticar no local ou para o Azure (num cenário ligado). ADAL também permite-lhe utilizar ferramentas como o Azure PowerShell e da CLI para entre a nuvem e gestão de recursos no local. 


### <a name="identities-and-your-identity-system"></a>Identidades e o sistema de identidade 
Identidades de pilha do Azure incluem as contas de utilizador, grupos e principais de serviço. Quando instala a pilha do Azure, vários serviços de aplicações incorporadas e registar automaticamente com o seu fornecedor de identidade no inquilino do diretório. Alguns serviços que registarem são utilizados para a administração. Outros serviços estão disponíveis para os utilizadores. Os registos de predefinição dê identidades de serviços de núcleos que podem interagir entre si e com identidades adicionar mais tarde.
Se configurar o Azure AD com vários inquilinos, algumas aplicações propagação para os diretórios de novo.  

## <a name="authentication-and-authorization"></a>Autenticação e autorização
 

### <a name="authentication-by-applications-and-users"></a>Autenticação por aplicações e os utilizadores
  
![Identidade entre camadas da pilha do Azure](media/azure-stack-identity-overview/identity-layers.png)

Para aplicações e utilizadores, a arquitetura de pilha do Azure é descrita por quatro camadas. Interações entre cada uma destas camadas podem utilizar diferentes tipos de autenticação.


|Camada    |Autenticação entre camadas  |
|---------|---------|
|Ferramentas e os clientes, como o portal de administração     | Para aceder ou modificar um recurso na pilha do Azure, ferramentas e os clientes utilizam um [JSON Web Tokens](/azure/active-directory/develop/active-directory-token-and-claims) (JWT) para colocar uma chamada para o Azure Resource Manager.  <br><br> O Azure Resource Manager valida o JWT e lê a *afirmações* o token emitido para estimar o nível de autorização que utilizador ou principal de serviço tem na pilha do Azure.        |
|O Azure Resource Manager e os respetivos serviços de núcleo     |O Azure Resource Manager comunica com os fornecedores de recursos para a transferência de comunicação dos utilizadores. <br><br> Transferências de utilização *imperativo direto* chamadas ou *declarativa* chama através de [modelos Azure Resource Manager](/azure/azure-stack/user/azure-stack-arm-templates.md).|
|Fornecedores de Recursos     |Chamadas transmitidas para os fornecedores de recursos estão protegidas com autenticação baseada em certificado. <br><br>O Azure Resource Manager e o fornecedor de recursos permanecem na comunicação através de uma API. Para cada chamada recebida a partir do Azure Resource Manager, o fornecedor de recursos valida a chamada com esse certificado.|
|Infraestrutura e lógica de negócio     |Fornecedores de recursos comunicarem com a lógica de negócio e a infraestrutura através da utilização de um modo de autenticação da sua eleição. A predefinição fornecedores de recursos que são enviados juntamente com a pilha do Azure utilizam a autenticação do Windows para proteger esta comunicação.|

![Informações necessárias para autenticação](media/azure-stack-identity-overview/authentication.png)


### <a name="authenticate-to-azure-resource-manager"></a>Autenticar para o Azure Resource Manager
Para autenticar com o fornecedor de identidade e receber um JWT, tem de ter as seguintes informações: 
1.  **URL para o sistema de identidade (autoridade)** – o URL que pode ser contactado o fornecedor de identidade. Por exemplo,  *&lt;https://login.windows.net>*. 
2.  **O URI de ID de aplicação para o Azure Resource Manager** – o identificador exclusivo para o Azure Resource Manager que é registado junto do seu fornecedor de identidade e exclusivo para cada instalação de pilha do Azure.
3.  **Credenciais** – esta credencial estiver a utilizar para autenticar com o fornecedor de identidade.  
4.  **URL para o Azure Resource Manager** – o URL é a localização do serviço do Azure Resource Manager. Por exemplo,  *&lt;https://management.azure.com>* ou  *&lt;https://management.local.azurestack.external>*.

Quando um principal (um cliente, aplicação ou utilizador) faz um pedido de autenticação para aceder a um recurso, esse pedido tem de incluir:
- Credenciais de um principal.
- O URI de ID de aplicação do recurso que pretendem aceder.  

As credenciais serem validadas pelo fornecedor de identidade. O fornecedor de identidade também valida que o URI de ID de aplicação é para uma aplicação registada, e que o principal tem os privilégios corretos para obter um token para esse recurso. Se o pedido for válido, é concedido uma Web Token JSON. 

Em seguida, tem do token de passar no cabeçalho de um pedido para o Azure Resource Manager. O Azure Resource Manager efetua das seguintes validações por nenhuma ordem específica:
- Validar o *emissor* (iss) de afirmação confirmar que o token do fornecedor de identidade correta. 
- Validar o *público-alvo* afirmação (aud) para confirmar se o token foi emitido para o Azure Resource Manager. 
- Confirme que o JWT está assinada com um certificado que é configurado através de OpenID e que é conhecido no Azure Resource Manager. 
- Reveja o *emitido no* (iat) e *expiração* (exp) afirmações para confirmar que o token do Active Directory e podem ser aceites. 

Quando todas as validações estiverem concluídas, o Gestor de recursos do Azure utiliza o *objeto* (oid) e o *grupos* afirmações para tornar uma lista de recursos que o principal pode aceder. 

![Diagrama do protocolo de troca de tokens](media/azure-stack-identity-overview/token-exchange.png)


### <a name="use-role-based-access-control"></a>Utilizar o controlo de acesso baseado em funções  
Baseada em funções controlo de acesso (RBAC) na pilha do Azure é consistente com a implementação no Microsoft Azure.  Pode gerir o acesso aos recursos através da atribuição de função RBAC adequada aos utilizadores, grupos e aplicações. Para obter informações sobre como utilizar o RBAC com pilha do Azure, reveja os artigos seguintes:
- [Introdução ao controlo de acesso baseado em funções no portal do Azure](/azure/active-directory/role-based-access-control-what-is).
- [Utilizar o controlo de acesso baseado em funções para gerir o acesso aos recursos da sua subscrição do Azure](/azure/active-directory/role-based-access-control-configure).
- [Criar funções personalizadas para controlo de acesso em funções do Azure](/azure/active-directory/role-based-access-control-custom-roles).
- [Gerir o controlo de acesso baseado em funções](azure-stack-manage-permissions.md) na pilha do Azure.


### <a name="authenticate-with-azure-powershell"></a>Autenticar com o Azure PowerShell  
Detalhes sobre como utilizar o Azure PowerShell para autenticar com o Azure pilha podem ser encontrados em [configurar o Azure pilha ambiente do utilizador PowerShell](azure-stack-powershell-configure-user.md).

### <a name="authenticate-with-azure-cli"></a>Autenticar com o CLI do Azure
Detalhes sobre como utilizar o Azure PowerShell para autenticar com o Azure pilha podem ser encontrados em [instalar e configurar a CLI para utilização com o Azure pilha](/azure/azure-stack/user/azure-stack-connect-cli.md).

## <a name="next-steps"></a>Passos Seguintes
- [Arquitetura de identidade](azure-stack-identity-architecture.md)   
- [Integração do Centro de dados - identidade](azure-stack-integrate-identity.md)




