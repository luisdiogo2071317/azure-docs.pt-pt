---
title: "CENC com múltipla DRM e controlo de acesso: uma estrutura de referência e a implementação no Azure e os Media Services do Azure | Microsoft Docs"
description: "Saiba mais sobre como o Microsoft uniforme de transmissão em fluxo cliente transferências Kit de licença."
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.assetid: 7814739b-cea9-4b9b-8370-538702e5c615
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: willzhan;kilroyh;yanmf;juliako
ms.openlocfilehash: cac1513d805e01f2c9633b3b318ad6808027904e
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/22/2017
---
# <a name="cenc-with-multi-drm-and-access-control-a-reference-design-and-implementation-on-azure-and-azure-media-services"></a>CENC com múltipla DRM e controlo de acesso: uma estrutura de referência e a implementação no Azure e os Media Services do Azure
 
## <a name="introduction"></a>Introdução
Conceber e criar um subsistema de direitos digitais (DRM) de gestão para uma ativação pós-falha-a-superior (OTT) online transmissão em fluxo ou solução é uma tarefa complexa. Os operadores online fornecedores de vídeos normalmente outsource esta tarefa para fornecedores de serviços DRM especializados. O objetivo deste documento é apresentar uma estrutura de referência e a implementação de um subsistema DRM ponto a ponto numa OTT ou solução de transmissão em fluxo online.

Os leitores de destino para este documento são engenheiros que trabalham no subsistemas DRM de OTT ou soluções de transmissão em fluxo/multiscreen online ou leitores que estejam interessados em subsistemas DRM. Pressuposto é que os leitores encontram familiarizados com pelo menos uma das tecnologias DRM no mercado, tais como PlayReady, Widevine, do FairPlay ou Adobe acesso.

Neste debate do DRM, também podemos incluir encriptação comum (CENC) com múltipla DRM. Uma tendência principais na transmissão em fluxo online e o setor OTT consiste em utilizar CENC com múltipla nativo DRM em várias plataformas de cliente. Esta tendência é um bit da anterior que é utilizado um único DRM e o cliente SDK para várias plataformas de cliente. Quando utilizar CENC com múltiplos nativo DRM tanto PlayReady como Widevine são encriptados de acordo com o [encriptação comum (ISO/IEC 23001 7 CENC)](http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=65271/) especificação.

Os benefícios da CENC com múltipla DRM são que as TI:

* Reduz o custo de encriptação como um processo de encriptação único é utilizado para plataformas diferentes com o respetivos DRMs nativas de destino.
* Reduz o custo de gerir recursos encriptados porque é necessário apenas uma única cópia dos recursos encriptados.
* Elimina o custo de licenciamento, porque o cliente DRM nativo é normalmente livre na respetiva plataforma nativa de clientes DRM.

Microsoft é um promoter Active Directory de DASH e CENC juntamente com alguns jogadores principais da indústria. Media Services do Azure fornece suporte para DASH e CENC. Para anúncios recentes, consulte os seguintes blogues:

*  [Anunciar os serviços de entrega de licença Widevine da Google nos Serviços de Multimédia do Azure](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)
* [Media Services do Azure adiciona empacotamento Widevine da Google para entrega de um fluxo de múltipla DRM](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/)  

### <a name="overview-of-this-article"></a>Descrição geral deste artigo
Os objetivos deste artigo são:

* Forneça uma estrutura de referência de um subsistema de DRM utiliza CENC com múltipla DRM.
* Forneça uma implementação de referência numa plataforma Azure/Media Services.
* Aborda alguns tópicos de design e implementação.

O artigo, o termo "múltipla DRM" abrange os seguintes produtos:

* Microsoft PlayReady
* Widevine da Google
* Apple FairPlay 

A tabela seguinte resume a aplicação de plataforma nativo/nativo e browsers suportados por cada DRM.

| **Plataforma de cliente** | **Suporte DRM nativo** | **Browser/aplicação** | **Formatos de transmissão em fluxo** |
| --- | --- | --- | --- |
| **TVs inteligentes, o operador STBs, OTT STBs** |PlayReady principalmente, e/ou Widevine e/ou outras |Linux, Opera, WebKit, outros |Vários formatos |
| **Dispositivos Windows 10 (Windows PC, tablets Windows, Windows Phone, Xbox)** |PlayReady |MS Edge/IE11/EME<br/><br/><br/>Plataforma universal do Windows |TRAÇO (para HLS, PlayReady não é suportado)<br/><br/>DASH, transmissão em fluxo uniforme (para HLS, PlayReady não é suportado) |
| **Dispositivos Android (telemóvel, tablet, TV)** |Widevine |Chrome/EME |DASH, HLS |
| **iOS (iPhone, iPad), os clientes dos X e Apple TV** |FairPlay |Safari 8 + /Safari/Chrome EME |HLS |


Tendo em consideração o estado atual da implementação para cada DRM, um serviço pretende normalmente DRMs duas ou três para se certificar de que todos os tipos de pontos finais de endereços da melhor forma de implementar.

Não há um compromisso entre a complexidade da lógica de serviço e a complexidade do lado do cliente ao atingir um determinado nível de experiência de utilizador nos vários clientes.

Para tornar a sua seleção, tenha em consideração:

* PlayReady nativamente é implementado em cada dispositivo de Windows, em alguns dispositivos Android e disponível através do software SDKs em praticamente qualquer plataforma.
* Widevine nativamente é implementada em todos os dispositivos Android, no Chrome e outros dispositivos.
* FairPlay está disponível apenas em iOS e os clientes Mac OS ou através do iTunes.

Existem duas opções para um múltipla DRM típica:

* PlayReady e Widevine
* PlayReady, Widevine e FairPlay

## <a name="a-reference-design"></a>Um design de referência
Esta secção apresenta uma estrutura de referência que é agnóstico relativamente às tecnologias utilizadas para implementá-las.

Um subsistema DRM pode conter os seguintes componentes:

* Gestão de chaves
* Empacotamento DRM
* Entrega de licença de DRM
* Verificação de elegibilidade
* Autenticação/autorização
* Leitor
* Rede de entrega de origem/conteúdos (CDN)

O diagrama seguinte ilustra a interação entre os componentes de um subsistema DRM alto nível:

![Subsistema com CENC DRM](./media/media-services-cenc-with-multidrm-access-control/media-services-generic-drm-subsystem-with-cenc.png)

A estrutura tem três camadas básicas:

* Uma camada de back-office (preta) não está exposta externamente.
* Uma camada de rede de Perímetro (blue escuro) contém todos os pontos finais que enfrentam público.
* Uma camada de internet pública (blue leve) contém o CDN e jogadores com o tráfego através da internet pública.

Também deve haver uma ferramenta de gestão de conteúdo para o controlo de proteção DRM, independentemente de ser encriptação estática ou dinâmica. As entradas para a encriptação de DRM incluem:

* Conteúdo de vídeo MBR
* Chave de conteúdo
* URL de aquisição de licença

Durante o período de reprodução, eis o fluxo de alto nível:

* O utilizador ser autenticado.
* É criado um token de autorização para o utilizador.
* Conteúdo DRM protegido (manifesto) é transferido para o leitor.
* O leitor envia um pedido de aquisição de licença para servidores de licenças, juntamente com uma chave de ID e um token de autorização.

A secção seguinte explica a estrutura da gestão de chaves.

| **Recurso de ContentKey** | **Cenário** |
| --- | --- |
| 1-a-1 |O cenário mais simples. Fornece o controlo finest. Mas, esta organização resulta, geralmente, o custo de entrega de licença mais elevado. No mínimo, um pedido de licença é necessário para cada recurso protegido. |
| 1-para-muitos |Pode utilizar a mesma chave de conteúdo para vários recursos. Por exemplo, para todos os recursos num grupo lógico, tais como um género ou o subconjunto de um género (ou gene de filmes), pode utilizar uma única chave de conteúdo. |
| Muitos-para-1 |Várias chaves de conteúdo são necessárias para cada recurso. <br/><br/>Por exemplo, se precisar de aplicar dinâmica proteção CENC com múltipla DRM para MPEG-DASH e encriptação AES-128 dinâmica para HLS, tem duas chaves de conteúdo separadas. Cada chave de conteúdo tem da suas próprias ContentKeyType. (Para a chave de conteúdo utilizada para proteção de CENC dinâmica, utilize ContentKeyType.CommonEncryption. Para a chave de conteúdo utilizada para encriptação AES-128 dinâmica, utilizar ContentKeyType.EnvelopeEncryption.)<br/><br/>Outro exemplo: proteção CENC do conteúdo DASH, em teoria, pode utilizar uma chave de conteúdo para proteger o fluxo de vídeo e outro chave de conteúdo para proteger a sequência de áudio. |
| Muitos-para-muitos |Combinação dos dois cenários anteriores. Um conjunto de chaves de conteúdo é utilizado para cada um dos vários recursos no mesmo grupo de recurso. |

Outro fator importante a ter em consideração é a utilização de licenças nonpersistent e persistentes.

Por que razão são estas considerações importantes?

Se utilizar uma nuvem pública para a entrega de licença, licenças persistentes e nonpersistent tem um impacto direto no custo de entrega de licença. Cenários de design diferentes de dois seguintes servem para ilustrar:

* Uma subscrição mensal: utilizar uma licença persistente e o mapeamento de recurso de chave de conteúdo de 1-para-muitos. Por exemplo, para filmes dos menores, utilizamos uma única chave de conteúdo para a encriptação. Neste caso:

    Número total de licenças pedida para filmes/dispositivo todos os menores = 1

* Uma subscrição mensal: utilizar uma licença nonpersistent e mapeamento de 1 para 1 entre os recursos e a chave de conteúdo. Neste caso:

    Número total de licenças pedida para filmes/dispositivo todos os menores = [número de filmes observada] x [número de sessões]

As estruturas diferentes dois resultam em padrões de pedido de licença muito diferentes. Os padrões diferentes resultam em custos se o serviço de entrega de licença é fornecido por uma nuvem pública como os serviços de suporte de dados de entrega de licença diferentes.

## <a name="map-design-to-technology-for-implementation"></a>Design de mapa para a tecnologia de implementação
Em seguida, a estrutura genérica está mapeada para as tecnologias de plataforma do Azure/Media Services, especificando que tecnologia a utilizar para cada bloco modular.

A tabela seguinte mostra o mapeamento.

| **Bloco modular** | **Tecnologia** |
| --- | --- |
| **Leitor** |[Media Player do Azure](https://azure.microsoft.com/services/media-services/media-player/) |
| **Fornecedor de identidade (IDP)** |Azure Active Directory (Azure AD) |
| **Serviço de tokens de segurança (STS)** |Azure AD |
| **Fluxo de trabalho de proteção de DRM** |Proteção de dinâmica de Media Services |
| **Entrega de licença DRM** |* Os Media Services licença entrega (PlayReady, Widevine, do FairPlay) <br/>* Servidor de licenças Axinom <br/>* Servidor de licenças PlayReady personalizado |
| **Origem** |Ponto final de transmissão em fluxo de Media Services |
| **Gestão de chaves** |Não é necessária para a implementação de referência |
| **Gestão de conteúdos** |Uma aplicação de consola c# |

Por outras palavras, IDP e STS são utilizadas com o Azure AD. O [API do Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/) é utilizado para o leitor. Os Media Services e Media Player suportam DASH e CENC com múltipla DRM.

O diagrama seguinte mostra a estrutura e fluxo com o mapeamento de tecnologia anterior geral:

![CENC nos serviços de suporte de dados](./media/media-services-cenc-with-multidrm-access-control/media-services-cenc-subsystem-on-AMS-platform.png)

Para configurar a encriptação de CENC dinâmica, a ferramenta de gestão de conteúdos utiliza as seguintes entradas:

* Abrir conteúdo
* Chave de conteúdo da chave geração/gestão
* URL de aquisição de licença
* Uma lista de informações do Azure AD

Segue-se a saída da ferramenta de gestão de conteúdos:

* ContentKeyAuthorizationPolicy contém a especificação na forma como a entrega de licença verifica um Token Web JSON (JWT) e as especificações de licença DRM.
* AssetDeliveryPolicy contém especificações na transmissão em fluxo formato, a proteção de DRM e o URL de aquisição de licença.

Eis o fluxo durante o tempo de execução:

* Após a autenticação de utilizador, é gerado um JWT.
* Uma das afirmações incluídas no JWT é uma afirmação de grupos que contém o objeto de grupo ID EntitledUserGroup. Esta afirmação é utilizada para passaram a verificação de elegibilidade.
* O leitor transfere o manifesto de cliente de conteúdo protegido por CENC e identifica o seguinte:
   * ID de chave.
   * O conteúdo é CENC protegido.
   * URL de aquisição de licença.
* O leitor faz um pedido de aquisição de licença com base no browser/DRM suportado. A licença aquisição pedido, a chave de que ID e a JWT, também são enviados. O serviço de entrega de licença verifica o JWT e afirmações incluídas antes que emite a licença necessária.

## <a name="implementation"></a>Implementação
### <a name="implementation-procedures"></a>Procedimentos de implementação
Implementação inclui os seguintes passos:

1. Prepare os recursos de teste. Codificar/pacote um vídeo de teste para o MP4 de transmissão múltipla fragmentada nos Media Services. Este recurso é *não* DRM protegido. Proteção de DRM é efetuada pelo dinâmica proteção mais tarde.

2. Crie uma chave de ID e uma chave de conteúdo (opcionalmente a partir de uma chave seed). Neste caso, o sistema de gestão de chaves não é necessária, porque apenas uma única chave ID e a chave de conteúdo são necessários para alguns dos recursos de teste.

3. Utilize a API de serviços de suporte de dados para configurar os serviços de entrega de licença de múltipla DRM para o recurso de teste. Se utilizar servidores de licenças personalizados pela sua empresa ou fornecedores da sua empresa em vez de serviços de licenciamento nos Media Services, pode ignorar este passo. Pode especificar os URLs de aquisição de licença no passo quando configurar a entrega de licença. A API de serviços de suporte de dados é necessário especificar algumas configurações de detalhado, tais como restrição de política de autorização e modelos de resposta de licença para diferentes serviços de licença da DRM. Neste momento, o portal do Azure não fornece a IU necessária para esta configuração. Para informações de nível da API e código de exemplo, consulte [utilize PlayReady e/ou Widevine a encriptação comum dinâmica](media-services-protect-with-playready-widevine.md).

4. Utilize a API de serviços de suporte de dados para configurar a política de entrega de recurso para o recurso de teste. Para informações de nível da API e código de exemplo, consulte [utilize PlayReady e/ou Widevine a encriptação comum dinâmica](media-services-protect-with-playready-widevine.md).

5. Criar e configurar um inquilino do Azure AD no Azure.

6. Crie alguns grupos e contas de utilizador no inquilino do Azure AD. Crie, pelo menos, um grupo de "Utilizador designada por" e adicionar um utilizador a este grupo. Os utilizadores deste grupo passaram a verificação de elegibilidade na aquisição de licença. Os utilizadores não deste grupo não passaram a verificação de autenticação e não é possível adquirir uma licença. A associação a este grupo de "Utilizador designada por" é uma afirmação de necessário nos grupos no JWT emitido pelo Azure AD. Especifique este requisito de afirmação no passo quando configurar serviços de entrega de licença de múltipla DRM.

7. Crie uma aplicação ASP.NET MVC para alojar o seu leitor de vídeo. Esta aplicação ASP.NET está protegida com autenticação de utilizador no inquilino do Azure AD. Afirmações adequadas estão incluídas os tokens de acesso obtidos após a autenticação de utilizador. Recomendamos que OpenID Connect API para este passo. Instale os pacotes de NuGet seguintes:

   * Pacote de instalação Microsoft.Azure.ActiveDirectory.GraphClient
   * Pacote de instalação Microsoft.Owin.Security.OpenIdConnect
   * Pacote de instalação Microsoft.Owin.Security.Cookies
   * Pacote de instalação Microsoft.Owin.Host.SystemWeb
   * Pacote de instalação Microsoft.IdentityModel.Clients.ActiveDirectory

8. Criar um leitor, utilizando o [API do Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/). Utilize o [API do Azure Media Player ProtectionInfo](http://amp.azure.net/libs/amp/latest/docs/) para especificar que tecnologia DRM a utilizar em diferentes plataformas DRM.

9. A tabela seguinte mostra a matriz de teste.

    | **DRM** | **Browser** | **Resultado para o utilizador realizada** | **Resultado para o utilizador unentitled** |
    | --- | --- | --- | --- |
    | **PlayReady** |Microsoft Edge ou o Internet Explorer 11 no Windows 10 |Com Êxito |Falha |
    | **Widevine** |Chrome no Windows 10 |Com Êxito |Falha |
    | **FairPlay** |TBD | | |

Para obter informações sobre como configurar o Azure AD para uma aplicação de leitor de MVC do ASP.NET, consulte [integrar uma aplicação baseada em MVC de OWIN de serviços de suporte de dados do Azure com o Azure Active Directory e restringir a entrega de chave de conteúdo com base em afirmações JWT](http://gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

Para obter mais informações, consulte [autenticação de token JWT nos Media Services do Azure e a encriptação dinâmica](http://gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).  

Para obter informações sobre o Azure AD:

* Pode encontrar informações de Programador no [guia para programadores do Azure Active Directory](../active-directory/active-directory-developers-guide.md).
* Pode encontrar informações de administrador no [administrar o diretório de inquilino do Azure AD](../active-directory/active-directory-administer.md).

### <a name="some-issues-in-implementation"></a>Alguns problemas na implementação
Utilize as seguintes informações de resolução de problemas para obter ajuda com problemas de implementação.

* O emissor URL tem de terminar com "/". O público-alvo tem de ser o ID de cliente de aplicação do leitor. Além disso, adicione "/" no final do URL do emissor.

        <add key="ida:audience" value="[Application Client ID GUID]" />
        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" />

    No [JWT descodificador](http://jwt.calebb.net/), verá **aud** e **iss**, conforme apresentado no JWT:

    ![JWT](./media/media-services-cenc-with-multidrm-access-control/media-services-1st-gotcha.png)

* Adicionar permissões para a aplicação no Azure AD no **configurar** separador da aplicação. As permissões são necessárias para cada aplicação, as versões de locais e implementadas.

    ![Permissões](./media/media-services-cenc-with-multidrm-access-control/media-services-perms-to-other-apps.png)

* Utilize o emissor correto quando configurar a proteção de CENC dinâmica.

        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/"/>

    Não utilizar o seguinte:

        <add key="ida:issuer" value="https://willzhanad.onmicrosoft.com/" />

    O GUID é o ID do inquilino do Azure AD. Pode encontrar o GUID de **pontos finais** menu de pop-up no portal do Azure.

* Associação ao grupo de conceder privilégios de afirmações. Certifique-se de que é o seguinte no ficheiro de manifesto da aplicação do Azure AD: 

    "groupMembershipClaims": "All" (o valor predefinido é nulo)

* Defina o TokenType adequada ao criar requisitos de restrição.

        objTokenRestrictionTemplate.TokenType = TokenType.JWT;

    Como adicionar suporte para JWT (Azure AD) para além de SWT (ACS), a predefinição TokenType é TokenType.JWT. Se utilizar SWT/ACS, tem de definir o token para TokenType.SWT.

## <a name="additional-topics-for-implementation"></a>Tópicos adicionais para a implementação
Esta secção descreve alguns tópicos adicionais na conceção e implementação.

### <a name="http-or-https"></a>HTTP ou HTTPS?
A aplicação de leitor de MVC do ASP.NET tem de suportar o seguinte:

* Autenticação de utilizador através do Azure AD, que está abaixo do HTTPS.
* Exchange JWT entre o cliente e o Azure AD, que está abaixo do HTTPS.
* Aquisição de licença da DRM pelo cliente, que tem de estar sob HTTPS se a entrega de licença é fornecida pelos serviços de suporte de dados. O conjunto de produto PlayReady não mandatar HTTPS para entrega de licença. Se o servidor de licenças PlayReady está fora dos Media Services, pode utilizar HTTP ou HTTPS.

A aplicação de leitor ASP.NET utiliza HTTPS como uma melhor prática, para que seja Media Player numa página em HTTPS. No entanto, HTTP é preferido para a transmissão em fluxo, por isso terá de considerar o problema de conteúdo misto.

* O browser não permite o conteúdo misto. Mas permitir plug-ins como Silverlight OSMF Plug-in para uniforme e TRAÇO-lo. O conteúdo misto é um problema de segurança devido a ameaça de capacidade para injetar JavaScript malicioso, que pode fazer com que os dados de cliente estar em risco. Browsers bloquear esta capacidade, por predefinição. É a única forma de contorná-lo no lado do servidor (origem) ao permitir que todos os domínios (independentemente de HTTPS ou HTTP). Isto é provavelmente não é uma boa ideia.
* Evite conteúdo misto. A aplicação de leitor e o Media Player devem utilizar HTTP ou HTTPS. Quando a reprodução do conteúdo misto, o técnico de silverlightSS requer limpar um aviso de conteúdo misto. O técnico de flashSS processa conteúdo misto sem um aviso de conteúdo misto.
* Se o ponto final de transmissão em fluxo foi criado antes de 2014 de Agosto, que não suporta HTTPS. Neste caso, criar e utilizar um novo ponto de final de transmissão em fluxo para HTTPS.

Na implementação de referência para o conteúdo DRM protegido, a aplicação e a transmissão em fluxo estão em HTTPS. Para abrir conteúdos, o leitor não necessita de autenticação ou uma licença, pelo que pode utilizar HTTP ou HTTPS.

### <a name="azure-active-directory-signing-key-rollover"></a>Azure Active Directory que o rollover da chave de assinatura
Assinar rollover da chave é um ponto importante a ter em consideração na sua implementação. Se é ignorá-lo, o sistema terminar deixa eventualmente de funcionar, no prazo de semanas seis no máximo.

Azure AD utiliza as normas do sector para estabelecer fidedignidade entre si próprio e as aplicações que utilizam o Azure AD. Especificamente, o Azure AD utiliza uma chave de assinatura que é composta por um par de chaves público e privado. Quando o Azure AD cria um token de segurança que contenha informações sobre o utilizador, está assinada pelo Azure AD com uma chave privada antes de ser enviada para a aplicação. Para verificar que o token é válido e de origem do Azure AD, a aplicação tem de validar a assinatura do token. A aplicação utiliza a chave pública exposta pelo Azure AD que está contido no documento de metadados de Federação do inquilino. Esta chave pública e a chave de assinatura partir da qual deriva, é o mesmo utilizada para todos os inquilinos no Azure AD.

Para obter mais informações sobre o rollover da chave do Azure AD, consulte [informações importantes sobre o rollover da chave de assinatura no Azure AD](../active-directory/active-directory-signing-key-rollover.md).

Entre o [par de chaves públicas-privadas](https://login.microsoftonline.com/common/discovery/keys/):

* A chave privada é utilizada pelo Azure AD para gerar um JWT.
* A chave pública é utilizada por uma aplicação, tais como serviços de entrega de licença DRM nos Media Services para verificar o JWT.

Por motivos de segurança do Azure AD roda periodicamente o certificado (a cada seis semanas). No caso de falhas de segurança, o rollover da chave pode ocorrer a qualquer altura. Por conseguinte, os serviços de entrega de licença nos serviços de suporte de dados tem de atualizar a chave pública utilizada como o Azure AD roda o par de chaves. Caso contrário, a falha de autenticação de token nos Media Services e nenhuma licença é emitida.

Para configurar este serviço, defina TokenRestrictionTemplate.OpenIdConnectDiscoveryDocument quando configurar serviços de entrega de licença DRM.

Eis o fluxo JWT:

* O Azure AD emite o JWT com a chave privada atual para um utilizador autenticado.
* Quando um leitor vê um CENC com conteúdo de múltipla DRM protegido, primeiro localiza JWT emitido pelo Azure AD.
* O leitor envia um pedido de aquisição de licença com o JWT aos serviços de entrega de licença nos serviços de suporte de dados.
* Os serviços de entrega de licença nos Media Services utilizam a chave pública atual válido do Azure AD para verificar o JWT antes de emitir licenças.

Serviços de entrega de licença DRM sempre a existência de para a chave pública atual válido do Azure AD. A chave pública apresentada pelo Azure AD é a chave utilizada para verificar um JWT emitido pelo Azure AD.

E se o rollover da chave acontece depois do Azure AD gera um JWT, mas antes do JWT é enviado pelo jogadores aos serviços de entrega de licença DRM nos serviços de suporte de dados para verificação?

Porque uma chave pode ser feita qualquer momento, a mais do que uma chave pública válida está sempre disponível no documento de metadados de Federação. Entrega de licença de Media Services, pode utilizar qualquer uma das chaves especificadas no documento. Porque uma chave poderá ser revertida em breve, outra poderá ser a sua substituição e assim sucessivamente.

### <a name="where-is-the-access-token"></a>Onde está o token de acesso?
Se observar como uma aplicação web chama uma aplicação de API em [identidade da aplicação com a concessão de credenciais de cliente OAuth 2.0](../active-directory/develop/active-directory-authentication-scenarios.md#web-application-to-web-api), o fluxo de autenticação é o seguinte:

* Um utilizador inicia sessão com o Azure AD na aplicação web. Para obter mais informações, consulte [browser para a aplicação web](../active-directory/develop/active-directory-authentication-scenarios.md#web-browser-to-web-application).
* O ponto final de autorização de AD do Azure redireciona o agente de utilizador para a aplicação de cliente com um código de autorização. O agente de utilizador devolve o código de autorização para o URI de redirecionamento da aplicação cliente.
* A aplicação web tem de adquirir um token de acesso para que possa autenticar para a API web e obter o recurso pretendido. Este faz um pedido para o ponto de final de token do Azure AD e fornece as credenciais, ID de cliente e URI de ID da aplicação de web da API. Apresente o código de autorização para provar que o utilizador autorizado.
* Azure AD autentica a aplicação e devolve um token de acesso JWT que é utilizado para chamar a API web.
* Através de HTTPS, a aplicação web utiliza o token de acesso JWT devolvido para adicionar a cadeia JWT com uma designação de "Portador" o cabeçalho "Authorization" do pedido para a API web. A API web, em seguida, valida o JWT. Se a validação for bem sucedida, devolve o recurso pretendido.

Neste fluxo de identidade da aplicação, a API web confianças de entidades que a aplicação web autenticar o utilizador. Por este motivo, este padrão é chamado um subsistema fidedigno. O [diagrama de fluxo de autorização](https://docs.microsoft.com/azure/active-directory/active-directory-protocols-oauth-code) descreve como autorização--concessão do código de fluxo funciona.

Aquisição de licença com restrição de token acompanha o mesmo padrão do subsistema fidedigna. O serviço de entrega de licença nos Media Services é recursos de API web ou o "recurso back-end" que necessita de uma aplicação web para aceder. Por isso, onde está o token de acesso?

O token de acesso é obtido a partir do Azure AD. Após a autenticação com êxito do utilizador, é devolvido um código de autorização. O código de autorização é utilizado, juntamente com o ID de cliente e a chave de aplicação, para o token de acesso do exchange. O token de acesso é utilizado para aceder a uma aplicação de "ponteiro" que aponte para ou que representa o serviço de entrega de licença de Media Services.

Para registar e configurar a aplicação de ponteiro no Azure AD, siga os passos seguintes:

1. No inquilino do Azure AD:

   * Adicione uma aplicação (recurso) com o início de sessão URL https://[resource_name].azurewebsites.net/. 
   * Adicione um ID de aplicação com o https://[aad_tenant_name].onmicrosoft.com/[resource_name URL].

2. Adicione uma nova chave para a aplicação de recursos.

3. Atualizar o ficheiro de manifesto de aplicação para que a propriedade groupMembershipClaims tem o valor "groupMembershipClaims": "All".

4. Na aplicação do Azure AD que aponta para a aplicação web player, na secção **permissões para outras aplicações**, adicionar a aplicação de recurso que foi adicionada no passo 1. Em **delegado permissão**, selecione **acesso [resource_name]**. Esta opção fornece a permissão da aplicação web para criar tokens de acesso que aceder a recursos de aplicação. Fazê-lo para a versão local e implementada da aplicação web se desenvolver com o Visual Studio e a aplicação web do Azure.

JWT emitido pelo Azure AD é o token de acesso utilizado para aceder o recursos de ponteiro.

### <a name="what-about-live-streaming"></a>E vai transmissão em direto?
O debate anterior concentra-se em recursos a pedido. E vai transmissão em direto?

Pode utilizar exatamente a mesma estrutura e implementação para proteger a transmissão em fluxo em direto nos serviços de suporte de dados por encará-los o elemento associado um programa como um recurso VOD.

Especificamente, a transmissão em fluxo nos Media Services, terá de criar um canal e, em seguida, criar um programa no canal. Para criar o programa, tem de criar um elemento que contenha o arquivo em direto para o programa. Para fornecer CENC com proteção múltipla DRM do conteúdo em direto, aplicam-se a mesmo configuração/processamento do ativo como se fosse um recurso VOD antes de iniciar o programa.

### <a name="what-about-license-servers-outside-media-services"></a>O que sobre servidores de licenças fora dos Media Services?
Muitas vezes, os clientes investiu num farm de servidores de licença encontra-se no seu próprio Centro de dados ou um alojadas por fornecedores de serviços DRM. Com a proteção de conteúdos de serviços de suporte de dados, pode funcionar em modo de híbrida. Conteúdo pode ser alojado e dinamicamente protegido nos Media Services, enquanto licenças DRM são fornecidas pelos servidores fora dos Media Services. Neste caso, considere as seguintes alterações:

* STS tem de emitir tokens que são aceitáveis e podem ser verificados pelo farm de servidores de licenciamento. Por exemplo, os servidores de licença Widevine fornecidos pelo Axinom requerem um JWT específico que contém uma mensagem de elegibilidade. Por conseguinte, tem de ter um STS para emitir um JWT deste tipo. Para obter informações sobre este tipo de implementação, vá para o [Centro de documentação do Azure](https://azure.microsoft.com/documentation/) e ver [Axinom de utilização para fornecer licenças Widevine aos Media Services do Azure](media-services-axinom-integration.md).
* Já não precisam de configurar o serviço de entrega de licença (ContentKeyAuthorizationPolicy) nos serviços de suporte de dados. Terá de fornecer a licença de aquisição URLs (PlayReady, Widevine e do FairPlay) quando configurar AssetDeliveryPolicy configurar CENC com múltipla DRM.

### <a name="what-if-i-want-to-use-a-custom-sts"></a>E se pretender utilizar um STS personalizado?
Um cliente pode optar por utilizar um STS personalizado para fornecer JWTs. Por motivos incluem:

* O IDP utilizado pelo cliente não suporta STS. Neste caso, um STS personalizado podem ser uma opção.
* O cliente pode ter mais flexível ou mais rígido controlo integrar STS de subscritor do cliente, sistema de faturação. Por exemplo, um operador MVPD poderá oferecem vários pacotes de subscritor OTT, tais como premium e básica e desporto. O operador querer corresponder as afirmações num token com o pacote de um subscritor para que apenas o conteúdo num pacote específico ficam disponível. Neste caso, um STS personalizado fornece a flexibilidade necessária e o controlo.

Quando utilizar um STS personalizado, têm de ser efetuadas duas alterações:

* Quando configura o serviço de entrega de licença para um recurso, tem de especificar a chave de segurança utilizada para a verificação pelo STS personalizado em vez da chave atual do Azure AD. (Siga mais detalhes.) 
* Quando é gerado um token JTW, foi especificada uma chave de segurança em vez da chave privada de X509 atual certificado no Azure AD.

Existem dois tipos de chaves de segurança:

* A chave simétrica: A mesma chave é utilizada para gerar e verificar um JWT.
* Chave assimétrica: um par de chaves públicas-privadas um X509 certificado é utilizado com uma chave privada para encriptar/gerar um JWT e com a chave pública para verificar o token.

> [!NOTE]
> Se utilizar o .NET Framework / c# como a sua plataforma de desenvolvimento, a X509 certificado utilizado para uma chave assimétrica segurança tem de ter um comprimento de chave de, pelo menos, 2048. Este é um requisito da classe System.IdentityModel.Tokens.X509AsymmetricSecurityKey no .NET Framework. Caso contrário, é emitida a seguinte exceção:

> IDX10630: 'System.IdentityModel.Tokens.X509AsymmetricSecurityKey' para a assinatura não pode ser menor que '2048' bits.

## <a name="the-completed-system-and-test"></a>O sistema concluído e teste
Esta secção explica os seguintes cenários no sistema ponto-a-ponto foi concluído, para que pode ter uma imagem básica do comportamento antes de obter uma conta de início de sessão:

* Se precisar de um cenário não integrada:

    * Para recursos de vídeos alojados nos serviços de suporte de dados que são um desprotegidos ou DRM protegido, mas sem autenticação com token (uma licença de emissão para quem pedida-lo), pode testá-la sem iniciar sessão. Mudar para HTTP esteja as vídeo de transmissão em fluxo através de HTTP.

* Se precisar de um cenário de integrada ponto a ponto:

    * Para recursos de vídeos dinâmica proteção de DRM nos Media Services, com a autenticação de token e JWT gerado pelo Azure AD, terá de iniciar sessão.

Para a aplicação web de leitor e o início de sessão, consulte [este Web site](https://openidconnectweb.azurewebsites.net/).

### <a name="user-sign-in"></a>Início de sessão do utilizador
Para testar o sistema DRM integrado ponto-a-ponto, tem de ter uma conta criada ou adicionado.

Que conta?

Embora o Azure permitisse apenas o acesso os utilizadores da conta Microsoft, é agora permitido o acesso por utilizadores de ambos os sistemas. Todas as propriedades do Azure agora confiam do Azure AD para autenticação e o Azure AD autentica utilizadores organizacionais. Foi criada uma relação de Federação em que o Azure AD confia o sistema de identidade do consumidor de conta Microsoft para autenticar os utilizadores. Como resultado, do Azure AD consegue autenticar contas de contas do Azure AD, bem como nativo do convidado Microsoft.

Porque o Azure AD confia o domínio da conta Microsoft, pode adicionar as contas de qualquer um dos seguintes domínios para o Azure AD personalizada inquilino e utilizam a conta para iniciar sessão:

| **Nome de domínio** | **Domínio** |
| --- | --- |
| **Domínio de inquilino personalizada do Azure AD** |somename.onmicrosoft.com |
| **Domínio empresarial** |Microsoft.com |
| **Domínio da conta Microsoft** |Outlook.com, live.com, hotmail.com |

Pode contactar a qualquer um dos autores ter uma conta criada ou adicionadas por si.

As capturas de ecrã seguintes mostram as páginas de início de sessão diferentes utilizadas pelas contas de domínio diferente:

**Conta de domínio de inquilino do Azure AD personalizado**: A página personalizada à início de sessão personalizada do Azure AD de domínio de inquilino.

![Conta de domínio do inquilino personalizada do Azure AD](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain1.png)

**Conta de domínio da Microsoft com smart card**: A página de início de sessão personalizada ao Microsoft empresarial IT com a autenticação de dois fatores.

![Conta de domínio do inquilino personalizada do Azure AD](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain2.png)

**Conta Microsoft**: A página de início de sessão da conta Microsoft para os consumidores.

![Conta de domínio do inquilino personalizada do Azure AD](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain3.png)

### <a name="use-encrypted-media-extensions-for-playready"></a>Utilização de extensões de suporte de dados encriptados para PlayReady
Num browser moderno com extensões de suporte de dados encriptados (EME) para obter suporte PlayReady, como o Internet Explorer 11 no Windows 8.1 ou posterior e o browser Microsoft Edge no Windows 10, PlayReady é o DRM subjacente para EME.

![Utilizar EME para PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready1.png)

A área de leitor escuro é porque a proteção de PlayReady impede-o de efetuar uma captura de ecrã de vídeo protegida.

A seguinte captura de ecrã mostra o leitor. o plug-ins e Essentials de segurança da Microsoft (MSE) / EME suporta:

![Leitor. o plug-ins para PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready2.png)

Permite EME no Microsoft Edge e no Windows 10 do Internet Explorer 11 [PlayReady SL3000](https://www.microsoft.com/playready/features/EnhancedContentProtection.aspx/) para ser invocado em dispositivos Windows 10 que o suportam. PlayReady SL3000 desbloqueia o fluxo de conteúdo avançado premium (4K, HDR) e novos modelos de entrega (para o conteúdo avançado) de conteúdo.

Para se focarem em dispositivos Windows, PlayReady é a única DRM no hardware disponível nos dispositivos do Windows (PlayReady SL3000). Um serviço de transmissão em fluxo pode utilizar PlayReady, através de EME ou através de uma aplicação plataforma Universal do Windows e oferecem uma qualidade do vídeo superior utilizando PlayReady SL3000 que outro DRM. Normalmente, conteúdo até 2 mil flui através do Chrome ou Firefox e conteúdo cópias de segurança aos fluxos de 4K através do Microsoft Edge/Internet Explorer 11 ou uma aplicação plataforma Universal do Windows no mesmo dispositivo. O depende das definições do serviço e a implementação.

#### <a name="use-eme-for-widevine"></a>Utilizar EME para Widevine
Num browser moderno com suporte para EME/Widevine, por exemplo, o Chrome 41 + no Windows 10, Windows 8.1, Mac OSX Yosemite e o Chrome em Android 4.4.4, Widevine da Google é DRM atrás EME.

![Utilizar EME para Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine1.png)

Widevine não impede a efetuar uma captura de ecrã de vídeo protegida.

![Leitor. o plug-ins para Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine2.png)

### <a name="unentitled-users"></a>Utilizadores unentitled
Se um utilizador não for um membro do grupo "Utilizadores denominada", o utilizador não passaram a verificação de elegibilidade. O serviço de licença de múltipla DRM recusa-se, em seguida emitir a pedido de licença, conforme mostrado. A descrição de detalhado é "adquirir licenças falhou," que é que concebida.

![Utilizadores unentitled](./media/media-services-cenc-with-multidrm-access-control/media-services-unentitledusers.png)

### <a name="run-a-custom-security-token-service"></a>Executar um serviço de token de segurança personalizadas
Se utilizar um STS personalizado, o JWT é emitido pelo STS personalizado utilizando um simétrico ou uma chave assimétrica.

A seguinte captura de ecrã mostra um cenário que utiliza uma chave simétrica (utilizando o Chrome):

![STS personalizados com uma chave simétrica](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts1.png)

A seguinte captura de ecrã mostra um cenário que utiliza uma chave assimétrica através de um X509 do certificado (utilizando um browser moderno Microsoft):

![STS personalizados com uma chave assimétrica](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts2.png)

Em ambos os casos anteriores, a autenticação do utilizador permanece igual. -Ocorre através do Azure AD. A única diferença é que JWTs são emitidos pelo STS personalizado em vez do Azure AD. Ao configurar a proteção de CENC dinâmica, a restrição de serviço de entrega de licença Especifica o tipo de JWT, um simétrico ou uma chave assimétrica.

## <a name="summary"></a>Resumo
Neste documento abordados CENC com múltipla nativo DRM e controlo de acesso através de autenticação com token, a respetiva estrutura e a respectiva implementação através da utilização do Azure, os Media Services e Media Player.

* Um design de referência foi apresentado que contém todos os componentes necessários num subsistema DRM/CENC.
* Uma implementação de referência foi introduzida no Azure, os Media Services e Media Player.
* Alguns tópicos diretamente envolvido na conceção e implementação também foram abordados.

## <a name="media-services-learning-paths"></a>Percursos de aprendizagem dos Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Enviar comentários
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
 
