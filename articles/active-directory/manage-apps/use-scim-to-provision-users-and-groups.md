---
title: Automatizar o aprovisionamento de aplicações com SCIM no Azure Active Directory | Documentos da Microsoft
description: O Azure Active Directory podem aprovisionar automaticamente os utilizadores e grupos para qualquer arquivo de identidade ou de aplicação é apoiado por um serviço web com a interface definida na especificação de protocolo SCIM
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: barbkess
ms.reviewer: asmalser
ms.custom: aaddev;it-pro;seohack1
ms.openlocfilehash: 04287d286aed872a2b951c47e0f67a93bd19c7b3
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/18/2018
ms.locfileid: "53583481"
---
# <a name="using-system-for-cross-domain-identity-management-scim-to-automatically-provision-users-and-groups-from-azure-active-directory-to-applications"></a>Sistema de gestão de identidade entre domínios (SCIM) a utilizar para aprovisionar automaticamente os utilizadores e grupos do Azure Active Directory a aplicações

## <a name="overview"></a>Descrição geral
Azure Active Directory (Azure AD), pode aprovisionar automaticamente os utilizadores e grupos a nenhum repositório de identidade ou de aplicação que é apoiado por um serviço da web com a interface definidos no [sistema para o protocolo de gestão de identidade entre domínios (SCIM) 2.0 especificação](https://tools.ietf.org/html/draft-ietf-scim-api-19). O Azure Active Directory pode enviar pedidos para criar, modificar ou eliminar atribuídos a utilizadores e grupos para o serviço web. O serviço web, em seguida, pode converter essas solicitações em operações no arquivo de identidade de destino. 

>[!IMPORTANT]
>O comportamento da implementação do Azure AD SCIM foi atualizado pela última vez 18 de Dezembro de 2018. Para obter informações sobre o que foi alterado, consulte [SCIM 2.0 compatibilidade de protocolo do serviço aprovisionamento de utilizador do Azure AD](application-provisioning-config-problem-scim-compatibility.md).

![][0]
*Figura 1: Aprovisionamento do Azure Active Directory para um armazenamento de identidade através de um serviço web*

Esse recurso pode ser usado em conjunto com a capacidade de "traga a sua própria aplicação" no Azure AD. Esta capacidade permite o início de sessão único e automático de utilizadores para aplicações que são apoiadas por um serviço da web SCIM de aprovisionamento.

Existem dois casos de utilização para utilizar o SCIM no Azure Active Directory:

* **Aprovisionamento de utilizadores e grupos para aplicações que suportam SCIM** -aplicações que suportam SCIM 2.0 e utilizam tokens de portador do OAuth para autenticação funciona com o Azure AD sem configuração.
  
* **Criar sua própria solução de configuração para aplicações que suportam a outros aprovisionamento baseado na API** -para aplicações não SCIM, pode criar um ponto de extremidade SCIM para traduzir entre o ponto de final do Azure AD SCIM e qualquer API, o aplicativo oferece suporte para Aprovisionamento de utilizadores. Para ajudar a desenvolver um ponto de extremidade SCIM, há bibliotecas de Common Language Infrastructure (CLI), juntamente com os exemplos de código que mostram como para fornecer um ponto de extremidade SCIM e traduzir mensagens SCIM.  

## <a name="provisioning-users-and-groups-to-applications-that-support-scim"></a>Aprovisionamento de utilizadores e grupos para aplicações que suportam SCIM
O Azure AD pode ser configurado para automaticamente os utilizadores de aprovisionamento atribuído e grupos para aplicativos que implementam uma [sistema para a gestão de identidade entre domínios 2 (SCIM)](https://tools.ietf.org/html/draft-ietf-scim-api-19) serviço web e aceitar tokens de portador do OAuth para autenticação. Dentro da especificação de SCIM 2.0, aplicativos têm de cumprir estes requisitos:

* Suporta a criação de utilizadores e/ou grupos, de acordo com a seção 3.3 do protocolo SCIM.  
* Suporta a modificação de utilizadores e/ou grupos com os pedidos de patch de acordo com a seção 3.5.2 do protocolo SCIM.  
* Suporta a recuperação de um recurso conhecido de acordo com a seção 3.4.1 do protocolo SCIM.  
* Suporta a consulta de utilizadores e/ou grupos, de acordo com a seção 3.4.2 do protocolo SCIM.  Por predefinição, os utilizadores são consultados por externalId e grupos são consultados por displayName.  
* Suporta a consulta de utilizador por ID e pelo Gestor de acordo com a seção 3.4.2 do protocolo SCIM.  
* Suporta a consulta de grupos por ID e por membro de acordo com a seção 3.4.2 do protocolo SCIM.  
* Aceita tokens de portador do OAuth para autorização de acordo com a seção 2.1 do protocolo SCIM.

Contacte o seu fornecedor do aplicativo, ou na documentação do seu fornecedor de aplicações para declarações de estado de compatibilidade com esses requisitos.

### <a name="getting-started"></a>Introdução
Aplicações que suportam o perfil SCIM descrito neste artigo podem ser ligadas ao Azure Active Directory usando o recurso de "aplicativo externas à Galeria" na Galeria de aplicações do Azure AD. Assim que estiver ligado, do Azure AD é executado um processo de sincronização de todos os 40 minutos em que ele consulta o ponto final SCIM do aplicativo para os utilizadores atribuídos e grupos e cria ou modifica-los, de acordo com os detalhes de atribuição.

**Para ligar uma aplicação que suporte SCIM:**

1. Inicie sessão no [do portal do Azure](https://portal.azure.com). 
2. Navegue até **do Azure Active Directory > aplicações empresariais**e selecione **nova aplicação > todos os > aplicação da Galeria não**.
3. Introduza um nome para a sua aplicação e clique em **adicionar** ícone para criar um objeto de aplicação.
    
   ![][1]
   *Figura 2: Galeria de aplicações do Azure AD*
    
4. No ecrã resultante, selecione o **aprovisionamento** separador na coluna esquerda.
5. Na **modo de aprovisionamento** menu, selecione **automática**.
    
   ![][2]
   *Figura 3: Configurar o aprovisionamento no portal do Azure*
    
6. Na **URL de inquilino** campo, introduza o URL do ponto final SCIM do aplicativo. Exemplo: https://api.contoso.com/scim/v2/
7. Se o ponto de extremidade SCIM exige um token de portador do OAuth a partir de um emissor diferente do Azure AD, em seguida, copie o token de portador do OAuth necessário para o opcional **segredo de Token** campo. Se este campo for deixado em branco, o Azure AD incluído um token de portador do OAuth emitido a partir do Azure AD com cada solicitação. Aplicações que utilizam o Azure AD como fornecedor de identidade pode validar este Azure AD-emitiu o token.
8. Clique nas **Testar ligação** botão para que o Azure Active Directory tentar estabelecer ligação ao ponto final do SCIM. Se as tentativas de falharem, são apresentadas informações de erro.  

    >[!NOTE]
    >**Testar ligação** consulta o ponto de extremidade SCIM para um utilizador que não existe, usando um GUID aleatório como a propriedade correspondente selecionada na configuração do Azure AD. A resposta correta esperada é HTTP 200 OK, com uma mensagem de SCIM ListResponse vazia. 

9. Se as tentativas de estabelecer ligação com êxito a aplicação, em seguida, clique em **guardar** para guardar as credenciais de administrador.
10. Na **mapeamentos** secção, existem dois conjuntos selecionáveis de mapeamentos de atributos: um para objetos de utilizador e uma para objetos de grupo. Selecione cada um para rever os atributos que são sincronizados a partir do Azure Active Directory para a sua aplicação. Os atributos selecionados como **correspondência** propriedades são usadas para fazer corresponder os utilizadores e grupos na sua aplicação para operações de atualização. Selecione o botão Guardar para consolidar as alterações.

    >[!NOTE]
    >Opcionalmente, pode desativar a sincronização de objetos de grupo, desativando os mapeamento de "grupos". 

11. Sob **configurações**, o **âmbito** campo define quais os utilizadores e ou grupos são sincronizados. Selecionar "Sincronização apenas atribuído aos utilizadores e grupos" (recomendado) só irá sincronizar os utilizadores e grupos atribuídos na **utilizadores e grupos** separador.
12. Assim que a configuração estiver concluída, alterar os **estado de aprovisionamento** para **no**.
13. Clique em **guardar** para iniciar o serviço de aprovisionamento do AD do Azure. 
14. Se a sincronização atribuído apenas a utilizadores e grupos (recomendados), certifique-se de que selecione os **utilizadores e grupos** separador e atribua os utilizadores e/ou grupos que pretende sincronizar.

Assim que tiver iniciado a sincronização inicial, pode utilizar o **registos de auditoria** separador para monitorize o progresso, que mostra todas as ações executadas pelo serviço de aprovisionamento na sua aplicação. Para obter mais informações sobre como ler o registos de aprovisionamento do AD do Azure, consulte [relatórios sobre o aprovisionamento de contas de utilizadores automático](check-status-user-account-provisioning.md).

> [!NOTE]
> A sincronização inicial demora mais tempo a serem executados do que as sincronizações subsequentes, o que ocorrer aproximadamente a cada 40 minutos, desde que o serviço está em execução. 


## <a name="building-your-own-provisioning-solution-for-any-application"></a>Criar a sua própria solução de aprovisionamento para qualquer aplicação
Ao criar um serviço da web SCIM que faz interface com o Azure Active Directory, pode ativar o aprovisionamento do utilizador de início de sessão e automática único para praticamente qualquer aplicativo que fornece uma API de aprovisionamento de utilizadores REST ou SOAP.

Eis como funciona:

1. O Azure AD fornece uma biblioteca de infraestrutura de linguagem comum com o nome [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/). Integradores de sistemas e os programadores podem utilizar esta biblioteca para criar e implementar um web baseados em SCIM ponto final de serviço capaz de estabelecer a ligação do Azure AD para o armazenamento de identidade de qualquer aplicativo.
2. Mapeamentos são implementados no serviço web para mapear o esquema de usuário padronizado para o esquema de utilizador e o protocolo necessário para a aplicação.
3. O URL de ponto final está registado no Azure AD como parte de uma aplicação personalizada na Galeria de aplicações.
4. Utilizadores e grupos são atribuídos a esta aplicação no Azure AD. Após a atribuição, eles são colocados em fila para ser sincronizados com o aplicativo de destino. O processo de sincronização lidar com a fila é executado em 40 minutos.

### <a name="code-samples"></a>Exemplos de Código
Para facilitar este processo, [exemplos de código](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) são fornecidas que criar um ponto de extremidade de serviço da web SCIM e demonstrar o aprovisionamento automático. É um exemplo de um provedor que mantém um ficheiro com linhas de valores separados por vírgulas que representam os utilizadores e grupos.  O outro é de um fornecedor que funciona com o serviço de gestão de acesso e identidade do Amazon Web Services.  

**Pré-requisitos**

* Visual Studio 2013 ou posterior
* [Azure SDK for .NET](https://azure.microsoft.com/downloads/) (Azure SDK para .NET)
* Windows máquina que suporta a estrutura do ASP.NET 4.5 para ser utilizado como o ponto de extremidade SCIM. Essa máquina deve ser acessível a partir da cloud
* [Uma subscrição do Azure com uma versão de avaliação ou licenciada do Azure AD Premium](https://azure.microsoft.com/services/active-directory/)

### <a name="getting-started"></a>Introdução
É a forma mais fácil de implementar um ponto de extremidade SCIM que pode aceitar pedidos de aprovisionamento do Azure AD criar e implementar o exemplo de código que produz os utilizadores aprovisionados para um ficheiro de valores separados por vírgulas (CSV).

**Para criar um ponto de extremidade SCIM de exemplo:**

1. Transferir o pacote de exemplo de código em [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)
2. Deszipe o pacote e coloque-o no seu computador Windows num local, como C:\AzureAD-BYOA-Provisioning-Samples\.
3. Nesta pasta, inicie o projeto de FileProvisioning\Host\FileProvisioningService.csproj no Visual Studio.
4. Selecione **ferramentas > Gestor de pacotes NuGet > consola do Gestor de pacotes**e execute os seguintes comandos para o projeto de FileProvisioningService resolver as referências de solução:

   ```
    Update-Package -Reinstall
   ```

5. Crie o projeto de FileProvisioningService.
6. Iniciar o aplicativo de linha de comandos no Windows (como administrador) e utilizar o **cd** comandos para alterar o diretório para sua **\AzureAD-BYOA-Provisioning-Samples\FileProvisioning\Host\bin\Debug**pasta.
7. Execute o seguinte comando, substituindo < endereço ip > com o nome de domínio ou endereço IP da máquina Windows:

   ```
    FileSvc.exe http://<ip-address>:9000 TargetFile.csv
   ```

8. No Windows sob **definições do Windows > rede & definições da Internet**, selecione a **Firewall do Windows > Definições Avançadas**e criar um **regras de entrada** que permite o acesso de entrada para porta 9000.
9. Se a máquina do Windows estiver atrás de um roteador, o roteador tem de ser configurado para efetuar a tradução de acesso de rede entre a sua porta 9000 que é exposto à internet e a porta 9000 na máquina do Windows. Esta configuração é necessária para o Azure AD poder aceder a este ponto final na cloud.

**Registar o ponto de final SCIM de exemplo no Azure AD:**

1. Inicie sessão no [do portal do Azure](https://portal.azure.com). 
2. Navegue até **do Azure Active Directory > aplicações empresariais**e selecione **nova aplicação > todos os > aplicação da Galeria não**.
3. Introduza um nome para a sua aplicação e clique em **adicionar** ícone para criar um objeto de aplicação. O objeto de aplicativo criado destina-se para representar a aplicação de destino que seria o aprovisionamento e implementação de início de sessão único para e não apenas o ponto final SCIM.
4. No ecrã resultante, selecione o **aprovisionamento** separador na coluna esquerda.
5. Na **modo de aprovisionamento** menu, selecione **automática**.
    
   ![][2]
   *Figura 4: Configurar o aprovisionamento no portal do Azure*
    
6. Na **URL de inquilino** , insira o URL e a porta do ponto final do SCIM exposta de internet. A entrada é algo como http://testmachine.contoso.com:9000 ou endereço IP de exposto http://<ip-address>:9000/, em que < endereço ip > é a internet.  
7. Se o ponto de extremidade SCIM exige um token de portador do OAuth a partir de um emissor diferente do Azure AD, em seguida, copie o token de portador do OAuth necessário para o opcional **segredo de Token** campo. Se este campo for deixado em branco, do Azure AD irá incluir um token de portador do OAuth emitido a partir do Azure AD com cada solicitação. Aplicações que utilizam o Azure AD como fornecedor de identidade pode validar este Azure AD-emitiu o token.
8. Clique nas **Testar ligação** botão para que o Azure Active Directory tentar estabelecer ligação ao ponto final do SCIM. Se as tentativas de falharem, são apresentadas informações de erro.  

    >[!NOTE]
    >**Testar ligação** consulta o ponto de extremidade SCIM para um utilizador que não existe, usando um GUID aleatório como a propriedade correspondente selecionada na configuração do Azure AD. A resposta correta esperada é HTTP 200 OK, com uma mensagem de SCIM ListResponse vazia

9. Se as tentativas de estabelecer ligação com êxito a aplicação, em seguida, clique em **guardar** para guardar as credenciais de administrador.
10. Na **mapeamentos** secção, existem dois conjuntos selecionáveis de mapeamentos de atributos: um para objetos de utilizador e uma para objetos de grupo. Selecione cada um para rever os atributos que são sincronizados a partir do Azure Active Directory para a sua aplicação. Os atributos selecionados como **correspondência** propriedades são usadas para fazer corresponder os utilizadores e grupos na sua aplicação para operações de atualização. Selecione o botão Guardar para consolidar as alterações.
11. Sob **configurações**, o **âmbito** campo define quais os utilizadores e ou grupos são sincronizados. Selecionar "Sincronização apenas atribuído aos utilizadores e grupos" (recomendado) só irá sincronizar os utilizadores e grupos atribuídos na **utilizadores e grupos** separador.
12. Assim que a configuração estiver concluída, alterar os **estado de aprovisionamento** para **no**.
13. Clique em **guardar** para iniciar o serviço de aprovisionamento do AD do Azure. 
14. Se a sincronização atribuído apenas a utilizadores e grupos (recomendados), certifique-se de que selecione os **utilizadores e grupos** separador e atribua os utilizadores e/ou grupos que pretende sincronizar.

Assim que tiver iniciado a sincronização inicial, pode utilizar o **registos de auditoria** separador para monitorize o progresso, que mostra todas as ações executadas pelo serviço de aprovisionamento na sua aplicação. Para obter mais informações sobre como ler o registos de aprovisionamento do AD do Azure, consulte [relatórios sobre o aprovisionamento de contas de utilizadores automático](check-status-user-account-provisioning.md).

A verificar o exemplo a etapa final é abrir o ficheiro de TargetFile.csv na pasta \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug no seu computador Windows. Assim que o processo de aprovisionamento é executado, este ficheiro mostra os detalhes de todos os atribuído e aprovisionamento de utilizadores e grupos.

### <a name="development-libraries"></a>Bibliotecas de programação
Para desenvolver seu próprio serviço da web que está em conformidade com a especificação de SCIM, primeiro familiarize-se com as seguintes bibliotecas fornecidas pela Microsoft para ajudar a acelerar o processo de desenvolvimento: 

1. Bibliotecas do Common Language Infrastructure (CLI) são oferecidas para utilização com linguagens com base em que a infraestrutura, como c#. Uma dessas bibliotecas, [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), declara uma interface, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, mostrada na ilustração seguinte:  Um desenvolvedor que usa as bibliotecas seria implementar a interface com uma classe que pode ser direcionada para, mais genericamente, como um fornecedor. As bibliotecas permitem aos programadores implementar um serviço web que está em conformidade com a especificação de SCIM. O serviço web pode ser hospedado em serviços de informação Internet ou qualquer executável assembly de infra-estrutura de linguagem comum. Pedido é convertido em chamadas para métodos do provedor, que poderiam ser programados pelo desenvolvedor para operar num armazenamento de identidade.
  
   ![][3]
  
2. [Express manipuladores de rotas](https://expressjs.com/guide/routing.html) são feitas disponíveis para análise de objetos de solicitação de node. js que representam chamadas (conforme definido pela especificação SCIM), a um serviço web do node. js.   

### <a name="building-a-custom-scim-endpoint"></a>Criação de um ponto de extremidade personalizado SCIM
Utilizar as bibliotecas CLI, os desenvolvedores que usam essas bibliotecas podem alojar os seus serviços em qualquer assembly executável de infra-estrutura de linguagem comum ou em serviços de informação Internet. Eis o código de exemplo para hospedar um serviço num assembly executável, no endereço http://localhost:9000: 

    private static void Main(string[] arguments)
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IProvider and 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
      new DevelopersMonitor();
    Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
      new DevelopersProvider(arguments[1]);
    Microsoft.SystemForCrossDomainIdentityManagement.Service webService = null;
    try
    {
        webService = new WebService(monitor, provider);
        webService.Start("http://localhost:9000");

        Console.ReadKey(true);
    }
    finally
    {
        if (webService != null)
        {
            webService.Dispose();
            webService = null;
        }
    }
    }

    public class WebService : Microsoft.SystemForCrossDomainIdentityManagement.Service
    {
    private Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor;
    private Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider;

    public WebService(
      Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitoringBehavior, 
      Microsoft.SystemForCrossDomainIdentityManagement.IProvider providerBehavior)
    {
        this.monitor = monitoringBehavior;
        this.provider = providerBehavior;
    }

    public override IMonitor MonitoringBehavior
    {
        get
        {
            return this.monitor;
        }

        set
        {
            this.monitor = value;
        }
    }

    public override IProvider ProviderBehavior
    {
        get
        {
            return this.provider;
        }

        set
        {
            this.provider = value;
        }
    }
    }

Este serviço tem de ter um HTTP endereço e o servidor de certificado de autenticação dos quais a autoridade de certificação de raiz é um dos seguintes nomes: 

* CNNIC
* Comodo
* CyberTrust
* DigiCert
* GeoTrust
* GlobalSign
* Go Daddy
* VeriSign
* WoSign

Um certificado de autenticação de servidor pode ser associado a uma porta num anfitrião Windows usando o utilitário de shell de rede: 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  

Aqui, o valor fornecido para o argumento de certhash é o thumbprint do certificado, enquanto o valor fornecido para o argumento de appid é um identificador exclusivo global arbitrário.  

Para hospedar o serviço dentro de serviços de informação Internet, um desenvolvedor criaria um assembly de biblioteca de código do CLA com uma classe chamada Startup no espaço de nomes padrão do assembly.  Eis um exemplo dessa classe: 

    public class Startup
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor and  
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter starter;

    public Startup()
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
          new DevelopersMonitor();
        Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
          new DevelopersProvider();
        this.starter = 
          new Microsoft.SystemForCrossDomainIdentityManagement.WebApplicationStarter(
            provider, 
            monitor);
    }

    public void Configuration(
      Owin.IAppBuilder builder) // Defined in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

### <a name="handling-endpoint-authentication"></a>Processar a autenticação de ponto final
Pedidos do Azure Active Directory incluem um token de portador do OAuth 2.0.   Qualquer serviço que o pedido a receber deve autenticar o emissor como sendo o Azure Active Directory em nome de inquilino do Azure Active Directory esperado, para acesso ao serviço da web do Azure Active Directory Graph.  No token, o emissor é identificado por uma afirmação de iss, como "iss": "https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".  Neste exemplo, o endereço base do valor de afirmação, https://sts.windows.net, identifica o Azure Active Directory como o emissor, enquanto o caminho relativo de segmento, cbb1a5ac-f33b-45fa-9bf5-f37db0fed422 endereços, que é um identificador exclusivo do inquilino do Azure Active Directory no nome que o token foi emitido.  Se o token foi emitido para acessar o serviço web do Azure Active Directory Graph, em seguida, o identificador do serviço, 00000002-0000-0000-c000-000000000000, deve estar no valor de afirmação de aud o token.  

Os desenvolvedores que usam as bibliotecas do CLA fornecidas pela Microsoft para a criação de um serviço SCIM podem autenticar os pedidos do Azure Active Directory utilizando o pacote de Microsoft.Owin.Security.ActiveDirectory seguindo estes passos: 

1. Num provedor, implemente a propriedade Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior fazendo com que ele retorne um método a ser chamado sempre que o serviço é iniciado: 

   ```
     public override Action\<Owin.IAppBuilder, System.Web.Http.HttpConfiguration.HttpConfiguration\> StartupBehavior
     {
       get
       {
         return this.OnServiceStartup;
       }
     }

     private void OnServiceStartup(
       Owin.IAppBuilder applicationBuilder,  // Defined in Owin.dll.  
       System.Web.Http.HttpConfiguration configuration)  // Defined in System.Web.Http.dll.  
     {
     }
   ```

2. Adicione o seguinte código para esse método para que qualquer pedido para qualquer um dos pontos de extremidade do serviço autenticados como tendo um token emitido pelo Azure Active Directory em nome de um inquilino especificado, para acesso ao serviço da web do Azure AD Graph: 

   ```
     private void OnServiceStartup(
       Owin.IAppBuilder applicationBuilder IAppBuilder applicationBuilder, 
       System.Web.Http.HttpConfiguration HttpConfiguration configuration)
     {
       // IFilter is defined in System.Web.Http.dll.  
       System.Web.Http.Filters.IFilter authorizationFilter = 
         new System.Web.Http.AuthorizeAttribute(); // Defined in System.Web.Http.dll.configuration.Filters.Add(authorizationFilter);

       // SystemIdentityModel.Tokens.TokenValidationParameters is defined in    
       // System.IdentityModel.Token.Jwt.dll.
       SystemIdentityModel.Tokens.TokenValidationParameters tokenValidationParameters =     
         new TokenValidationParameters()
         {
           ValidAudience = "00000002-0000-0000-c000-000000000000"
         };

       // WindowsAzureActiveDirectoryBearerAuthenticationOptions is defined in 
       // Microsoft.Owin.Security.ActiveDirectory.dll
       Microsoft.Owin.Security.ActiveDirectory.
       WindowsAzureActiveDirectoryBearerAuthenticationOptions authenticationOptions =
         new WindowsAzureActiveDirectoryBearerAuthenticationOptions()    {
         TokenValidationParameters = tokenValidationParameters,
         Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute the appropriate tenant’s 
                                                       // identifier for this one.  
       };

       applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
     }
   ```


## <a name="user-and-group-schema"></a>Esquema de utilizador e grupo
O Azure Active Directory pode aprovisionar dois tipos de recursos para serviços da web SCIM.  Esses tipos de recursos são utilizadores e grupos.  

Recursos de utilizador são identificados pelo identificador de esquema, "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User", que está incluído nessa especificação de protocolo: http://tools.ietf.org/html/draft-ietf-scim-core-schema.  O mapeamento predefinido de atributos de utilizadores no Azure Active Directory para os atributos de recursos "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User" é apresentado na tabela 1, abaixo.  

Grupo de recursos é identificados pelo identificador de esquema, http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  Tabela 2, abaixo, mostra o mapeamento predefinido de atributos de grupos no Azure Active Directory para os atributos do http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group recursos.  

### <a name="table-1-default-user-attribute-mapping"></a>Tabela 1: Mapeamento de atributo de utilizador padrão

| Utilizador do Active Directory do Azure | "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User" |
| --- | --- |
| IsSoftDeleted |ativo |
| displayName |displayName |
| TelephoneNumber de fax |. Value de phoneNumbers [tipo eq "fax"] |
| givenName |name.givenName |
| jobTitle |título |
| mail |. Value de e-mails [tipo eq "trabalho"] |
| mailNickname |externalId |
| gestor |gestor |
| móvel |. Value de phoneNumbers [tipo eq "móvel"] |
| objectId |ID |
| postalCode |.postalCode endereços [tipo eq "trabalho"] |
| Endereços de proxy |e-mails de [tipo é igual a "other"]. Valor |
| physical-Delivery-OfficeName |endereços [tipo é igual a "other"]. Formatado |
| streetAddress |.streetAddress endereços [tipo eq "trabalho"] |
| Apelido |name.familyName |
| Número de telefone |. Value de phoneNumbers [tipo eq "trabalho"] |
| utilizador PrincipalName |userName |

### <a name="table-2-default-group-attribute-mapping"></a>Tabela 2: Mapeamento de atributo de grupo predefinido

| Grupo do Azure Active Directory | http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group |
| --- | --- |
| displayName |externalId |
| mail |. Value de e-mails [tipo eq "trabalho"] |
| mailNickname |displayName |
| membros |membros |
| objectId |ID |
| proxyAddresses |e-mails de [tipo é igual a "other"]. Valor |

## <a name="user-provisioning-and-de-provisioning"></a>Utilizador provisionamento e desprovisionamento
A ilustração seguinte mostra as mensagens que o Azure Active Directory envia para um serviço SCIM para gerir o ciclo de vida de um utilizador no outro armazenamento de identidade. O diagrama também mostra como um serviço SCIM implementado usando as bibliotecas CLI fornecido pela Microsoft para criar que esses serviços de convertem esses pedidos em chamadas para os métodos de um fornecedor.  

![][4]
*Figura 5: Utilizador provisionamento e desprovisionamento sequência*

1. O Azure Active Directory consulta o serviço para um utilizador com um valor de atributo externalId correspondência o valor do atributo mailNickname de um utilizador no Azure AD. A consulta é expressa como um pedido de protocolo HTTP (Hypertext Transfer), como neste exemplo, na qual jyoung é um exemplo de um mailNickname de um utilizador no Azure Active Directory: 

   ```
     GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
     Authorization: Bearer ...
   ```

   Se o serviço foi criado com as bibliotecas de infra-estrutura de linguagem comum fornecidas pela Microsoft para implementar os serviços SCIM, em seguida, o pedido é convertido numa chamada para o método de consulta do fornecedor do serviço.  Esta é a assinatura desse método: 

   ```
     // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
     // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
     // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
     // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
     // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  
 
     System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]>  Query(
       Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
       string correlationIdentifier);
   ````

   Esta é a definição da Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters interface: 

   ```
     public interface IQueryParameters: 
       Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
     {
         System.Collections.Generic.IReadOnlyCollection  <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
         { get; }
     }

     public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
     {
       system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
       { get; }
       System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
       { get; }
       string SchemaIdentifier 
       { get; }
     }

     public interface Microsoft.SystemForCrossDomainIdentityManagement.IFilter
     {
         Microsoft.SystemForCrossDomainIdentityManagement.IFilter AdditionalFilter 
           { get; set; }
         string AttributePath 
           { get; } 
         Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator FilterOperator 
           { get; }
         string ComparisonValue 
           { get; }
     }

     public enum Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator
     {
         Equals
     }
   ```

   No seguinte exemplo de uma consulta para um utilizador com um valor especificado para o atributo externalId, valores de argumentos transmitidos para o método de consulta são: 
   * parâmetros. AlternateFilters.Count: 1
   * parâmetros. AlternateFilters.ElementAt(0). AttributePath: externalId""
   * parâmetros. AlternateFilters.ElementAt(0). ComparisonOperator: ComparisonOperator.Equals
   * parâmetros. AlternateFilter.ElementAt(0). ComparisonValue: "jyoung"
   * correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin. RequestId"] 

2. Se a resposta a uma consulta para o serviço web para um utilizador com um valor de atributo externalId que corresponde ao valor de atributo mailNickname de um utilizador não devolver quaisquer utilizadores, em seguida, Azure Active Directory solicita que o serviço aprovisionamento de um utilizador correspondente ao no Azure Active Directory.  Eis um exemplo de pedido: 

   ```
     POST https://.../scim/Users HTTP/1.1
     Authorization: Bearer ...
     Content-type: application/scim+json
     {
       "schemas":
       [
         "urn:ietf:params:scim:schemas:core:2.0:User",
         "urn:ietf:params:scim:schemas:extension:enterprise:2.0User"],
       "externalId":"jyoung",
       "userName":"jyoung",
       "active":true,
       "addresses":null,
       "displayName":"Joy Young",
       "emails": [
         {
           "type":"work",
           "value":"jyoung@Contoso.com",
           "primary":true}],
       "meta": {
         "resourceType":"User"},
        "name":{
         "familyName":"Young",
         "givenName":"Joy"},
       "phoneNumbers":null,
       "preferredLanguage":null,
       "title":null,
       "department":null,
       "manager":null}
   ```

   As bibliotecas de infra-estrutura de linguagem comum fornecidas pela Microsoft para implementar os serviços SCIM converte esse pedido numa chamada para o método de criação do fornecedor do serviço.  O método Create tem esta assinatura: 

   ```
     // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
     // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
     // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
 
     System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create (
       Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
       string correlationIdentifier);
   ```

   Num pedido de aprovisionamento de um utilizador, o valor do argumento de recursos é uma instância do Microsoft.SystemForCrossDomainIdentityManagement. Classe de Core2EnterpriseUser, definida na biblioteca do Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  Se o pedido para aprovisionar o utilizador for bem-sucedida, a implementação do método é esperada para retornar uma instância do Microsoft.SystemForCrossDomainIdentityManagement. Classe de Core2EnterpriseUser, com o valor da propriedade Identificador definido como o identificador exclusivo do utilizador recentemente aprovisionado.  

3. Para atualizar um utilizador sabe existirem num arquivo de identidade apoiado por um SCIM, o Azure Active Directory continua ao pedir o estado atual do que o utilizador do serviço com um pedido, tais como: 

   ```
     GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
     Authorization: Bearer ...
   ```

   Num serviço criado com as bibliotecas de infra-estrutura de linguagem comum fornecidas pela Microsoft para implementar os serviços SCIM, o pedido é convertido numa chamada para o método de recuperação do fornecedor do serviço.  Esta é a assinatura do método obter: 

    ```
     // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
     // Microsoft.SystemForCrossDomainIdentityManagement.Resource and 
     // Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
     // are defined in Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
     System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> 
        Retrieve(
          Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
            parameters, 
            string correlationIdentifier);
 
     public interface 
       Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
         IRetrievalParameters
         {
           Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
             ResourceIdentifier 
               { get; }
     }
     public interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier
     {
         string Identifier 
           { get; set; }
         string Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier 
           { get; set; }
     }
   ```

   No exemplo de um pedido para obter o estado atual de um utilizador, os valores das propriedades do objeto fornecido como o valor do argumento parâmetros são os seguintes: 
  
   * Identificador: "54D382A4-2050-4C03-94D1-E769F1D15682"
   * SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

4. Se um atributo de referência está a ser atualizado, em seguida, o Azure Active Directory consulta o serviço para determinar se é ou não o valor atual do atributo de referência no arquivo de identidade apoiado pelo serviço já corresponde ao valor desse atributo no Active Directory do Azure Diretório. Para os utilizadores, o único atributo que o valor atual é consultado dessa maneira é o atributo de gestor. Eis um exemplo de um pedido para determinar se o atributo de Gestor de um objeto de utilizador em particular tem atualmente um determinado valor: 

   ```
     GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq  2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
     Authorization: Bearer ...
   ```

   O valor do parâmetro de consulta de atributos, "ID", significa que, se um objeto de utilizador existe que satisfaz a expressão fornecida como o valor do parâmetro de consulta de filtro, em seguida, espera-se que o serviço responder com uma "urn: ietf:params:scim:schemas:core:2.0: Recurso de utilizador"ou"urn: ietf:params:scim:schemas:extension:enterprise:2.0:User", incluindo apenas o valor do atributo de"ID"desse recurso.  O valor do **ID** atributo é conhecido para o requerente. Está incluído no valor do parâmetro de consulta de filtro; o objetivo de pedir para ele, na verdade, é pedir uma representação de um mínimo de um recurso que que satisfaça a expressão de filtro como uma indicação de se pretende ou não existe a qualquer objeto desse tipo.   

   Se o serviço foi criado com as bibliotecas de infra-estrutura de linguagem comum fornecidas pela Microsoft para implementar os serviços SCIM, em seguida, o pedido é convertido numa chamada para o método de consulta do fornecedor do serviço. O valor das propriedades do objeto fornecido como o valor do argumento parâmetros são os seguintes: 
  
   * parâmetros. AlternateFilters.Count: 2
   * parâmetros. AlternateFilters.ElementAt(x). AttributePath: "ID"
   * parâmetros. AlternateFilters.ElementAt(x). ComparisonOperator: ComparisonOperator.Equals
   * parâmetros. AlternateFilter.ElementAt(x). ComparisonValue:  "54D382A4-2050-4C03-94D1-E769F1D15682"
   * parâmetros. AlternateFilters.ElementAt(y). AttributePath: "manager"
   * parâmetros. AlternateFilters.ElementAt(y). ComparisonOperator: ComparisonOperator.Equals
   * parâmetros. AlternateFilter.ElementAt(y). ComparisonValue:  "2819c223-7f76-453a-919d-413861904646"
   * parâmetros. RequestedAttributePaths.ElementAt(0): "ID"
   * parâmetros. SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

   Aqui, o valor do índice x pode ser 0 e o valor de y o índice pode ser 1, ou o valor de x pode ser 1 e o valor de y pode ser 0, dependendo da ordem das expressões de parâmetro de consulta de filtro.   

5. Eis um exemplo de um pedido do Azure Active Directory a um serviço SCIM para atualizar um utilizador: 

   ```
     PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
     Authorization: Bearer ...
     Content-type: application/scim+json
     {
       "schemas": 
       [
         "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
       "Operations":
       [
         {
           "op":"Add",
           "path":"manager",
           "value":
             [
               {
                 "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
                 "value":"2819c223-7f76-453a-919d-413861904646"}]}]}
   ```

   As bibliotecas de infra-estrutura de linguagem comum do Microsoft para implementar os serviços SCIM converte o pedido numa chamada para o método de atualização do fornecedor do serviço. Esta é a assinatura do método de atualização: 

   ```
     // System.Threading.Tasks.Tasks and 
     // System.Collections.Generic.IReadOnlyCollection<T>
     // are defined in mscorlib.dll.  
     // Microsoft.SystemForCrossDomainIdentityManagement.IPatch, 
     // Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, 
     // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
     // Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, 
     // Microsoft.SystemForCrossDomainIdentityManagement.OperationName, 
     // Microsoft.SystemForCrossDomainIdentityManagement.IPath and 
     // Microsoft.SystemForCrossDomainIdentityManagement.OperationValue 
     // are all defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

     System.Threading.Tasks.Task Update(
       Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, 
       string correlationIdentifier);

     public interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch
     {
     Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase 
       PatchRequest 
         { get; set; }
     Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
       ResourceIdentifier 
         { get; set; }        
     }

     public class PatchRequest2: 
       Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase
     {
     public System.Collections.Generic.IReadOnlyCollection
       <Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation> 
         Operations
         { get;}

     public void AddOperation(
       Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation operation);
     }

     public class PatchOperation
     {
     public Microsoft.SystemForCrossDomainIdentityManagement.OperationName 
       Name
       { get; set; }

     public Microsoft.SystemForCrossDomainIdentityManagement.IPath 
       Path
       { get; set; }

     public System.Collections.Generic.IReadOnlyCollection
       <Microsoft.SystemForCrossDomainIdentityManagement.OperationValue> Value
       { get; }

     public void AddValue(
       Microsoft.SystemForCrossDomainIdentityManagement.OperationValue value);
     }

     public enum OperationName
     {
       Add,
       Remove,
       Replace
     }

     public interface IPath
     {
       string AttributePath { get; }
       System.Collections.Generic.IReadOnlyCollection<IFilter> SubAttributes { get; }
       Microsoft.SystemForCrossDomainIdentityManagement.IPath ValuePath { get; }
     }

     public class OperationValue
     {
       public string Reference
       { get; set; }

       public string Value
       { get; set; }
     }
   ```

    No exemplo de um pedido para atualizar um utilizador, o objeto fornecido como o valor do argumento de patch tem estes valores de propriedade: 
  
   * ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
   * ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"
   * (PatchRequest como PatchRequest2). Operations.Count: 1
   * (PatchRequest como PatchRequest2). Operations.ElementAt(0). OperationName: OperationName.Add
   * (PatchRequest como PatchRequest2). Operations.ElementAt(0). Path.AttributePath: "manager"
   * (PatchRequest como PatchRequest2). Operations.ElementAt(0). Value.Count: 1
   * (PatchRequest como PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Referência: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646
   * (PatchRequest como PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Valor: 2819c223-7f76-453A-919d-413861904646

6. Desaprovisionar um utilizador de um arquivo de identidade apoiado por um serviço SCIM, do Azure AD envia um pedido, tais como: 

   ```
     DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
     Authorization: Bearer ...
   ```

   Se o serviço foi criado com as bibliotecas de infra-estrutura de linguagem comum fornecidas pela Microsoft para implementar os serviços SCIM, em seguida, o pedido é convertido numa chamada para o método Delete do fornecedor do serviço.   Esse método tem esta assinatura: 

   ```
     // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
     // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
     // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
     System.Threading.Tasks.Task Delete(
       Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
         resourceIdentifier, 
       string correlationIdentifier);
   ```

   O objeto fornecido como o valor do argumento resourceIdentifier tem estes valores de propriedade no exemplo de um pedido para desaprovisionar um utilizador: 

   * ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
   * ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

## <a name="group-provisioning-and-de-provisioning"></a>Grupo de provisionamento e desprovisionamento
A ilustração seguinte mostra as mensagens que Azure AcD envia a um serviço SCIM para gerir o ciclo de vida de um grupo no outro armazenamento de identidade.  Essas mensagens são diferentes de mensagens relativas aos utilizadores de três formas: 

* O esquema de um recurso de grupo é identificado como `http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group`.  
* Pedidos para obter os grupos de estipular que o atributo de membros é a excluir de qualquer recurso fornecido em resposta ao pedido.  
* Os pedidos para determinar se um atributo de referência tem um determinado valor são pedidos sobre o atributo de membros.  

![][5]
*Figura 6: Grupo de provisionamento e desprovisionamento sequência*

## <a name="related-articles"></a>Artigos relacionados
* [Automatizar o utilizador aprovisionamento/desaprovisionamento às aplicações SaaS](user-provisioning.md)
* [Personalizar mapeamentos de atributos para o aprovisionamento do utilizador](customize-application-attributes.md)
* [Escrever expressões para mapeamentos de atributos](functions-for-customizing-application-data.md)
* [Filtros de âmbito para o aprovisionamento do utilizador](define-conditional-rules-for-provisioning-user-accounts.md)
* [Notificações do aprovisionamento de contas](user-provisioning.md)
* [Lista de tutoriais sobre como integrar aplicações SaaS](../saas-apps/tutorial-list.md)

<!--Image references-->
[0]: ./media/use-scim-to-provision-users-and-groups/scim-figure-1.png
[1]: ./media/use-scim-to-provision-users-and-groups/scim-figure-2a.png
[2]: ./media/use-scim-to-provision-users-and-groups/scim-figure-2b.png
[3]: ./media/use-scim-to-provision-users-and-groups/scim-figure-3.png
[4]: ./media/use-scim-to-provision-users-and-groups/scim-figure-4.png
[5]: ./media/use-scim-to-provision-users-and-groups/scim-figure-5.png
