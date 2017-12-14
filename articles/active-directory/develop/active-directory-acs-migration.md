---
title: "Migrar a partir do serviço de controlo de acesso do Azure | Microsoft Docs"
description: "Opções para mover as aplicações e serviços a partir do serviço de controlo de acesso do Azure"
services: active-directory
documentationcenter: dev-center-name
author: dstrockis
manager: mtillman
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/14/2017
ms.author: dastrock
ms.openlocfilehash: f3de9016fe29a51ab2c7fb9e93fcd33af0f0e871
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2017
---
# <a name="migrate-from-the-azure-access-control-service"></a>Migrar a partir do serviço de controlo de acesso do Azure

Controlo de acesso do Azure, um serviço do Azure Active Directory (Azure AD), será descontinuado dentro de Novembro de 2018. Aplicações e serviços que utilizam o controlo de acesso atualmente têm de ser totalmente migrados para um mecanismo de autenticação diferentes até lá. Este artigo descreve as recomendações para os clientes atuais, conforme pretender preterir a utilização do controlo de acesso. Se não utilizar atualmente o controlo de acesso, não precisa de efetuar qualquer ação.


## <a name="overview"></a>Descrição geral

Controlo de acesso é um serviço de autenticação em nuvem oferece uma forma fácil de autenticar e autorizar utilizadores para o acesso aos seus serviços e aplicações web. Permite que várias funcionalidades de autenticação e autorização para ser factored fora do seu código. Controlo de acesso é principalmente utilizado pelos programadores e arquitetos de clientes do Microsoft .NET, aplicações web ASP.NET e serviços web do Windows Communication Foundation (WCF).

Casos de utilização para o controlo de acesso podem ser divididos em três categorias principais:

- Autenticar determinadas cloud Services da Microsoft, incluindo o Service Bus do Azure e o Dynamics CRM. As aplicações cliente obter os tokens de controlo de acesso para se autenticar estes serviços para efetuar diversas ações.
- Adicionar autenticação para aplicações web, personalizadas e prepackaged (como o SharePoint). Ao utilizar a autenticação "passiva" controlo de acesso, aplicações web podem suportar o início de sessão com uma conta Microsoft (anteriormente Live ID) e com as contas do Facebook, Google, do Azure AD, e Yahoo e serviços de Federação do Active Directory (AD FS).
- Proteger os serviços web personalizados com tokens emitidos pelo controlo de acesso. Ao utilizar a autenticação "ativa", serviços web podem certificar-se de que permitem acesso apenas aos clientes conhecidos que tenham autenticadas com controlo de acesso.

Cada um destes utilizar casos e a respetiva migração recomendada estratégias são abordadas nas secções seguintes. 

> [!WARNING]
> Na maioria dos casos, as alterações significativas de código são necessários para migrar serviços e aplicações existentes para tecnologias mais recentes. Recomendamos que imediatamente comece a planear e executar a migração para evitar quaisquer potenciais falhas ou período de indisponibilidade.

Controlo de acesso tem os seguintes componentes:

- Um token serviço seguro (STS), que recebe pedidos de autenticação e emite tokens de segurança no return.
- Portal clássico do Azure, onde pode cria, elimina e ativar e desativar espaços de nomes do controlo de acesso.
- Um controlo de acesso management portal separado, onde personalizar e configurar os espaços de nomes do controlo de acesso.
- Um serviço de gestão, que pode utilizar para automatizar as funções dos portais.
- Um motor de regra de transformação token, que pode utilizar para definir lógica complexa para manipular o formato de saída de tokens que emite o controlo de acesso.

Para utilizar estes componentes, tem de criar um ou mais espaços de nomes de controlo de acesso. A *espaço de nomes* é uma instância dedicada do controlo de acesso a aplicações e serviços comunicam com. Um espaço de nomes está isolado de todos os outros clientes de controlo de acesso. Outros clientes de controlo de acesso utilizam os seus próprios espaços de nomes. Um espaço de nomes no controlo de acesso tem um URL dedicado que tem este aspeto:

```HTTP
https://<mynamespace>.accesscontrol.windows.net
```

Todas as comunicações com o STS e operações de gestão são efetuadas neste URL. Utilize caminhos diferentes para diferentes fins. Para determinar se a aplicações ou serviços utilizam o controlo de acesso, monitorizar o tráfego para https://\<espaço de nomes\>. accesscontrol.windows.net. Qualquer tráfego para este URL é processado pelo controlo de acesso e tem de ser descontinuada. 

A exceção é qualquer tráfego https://accounts.accesscontrol.windows.net. O tráfego para este URL já está a ser processado por um serviço diferente e não é afetado pela descontinuação do controlo de acesso. 

Também deve iniciar sessão para o portal clássico do Azure e verifique a existência de quaisquer espaços de nomes do controlo de acesso nas subscrições que é proprietário. Espaços de nomes de controlo de acesso são listados no **espaços de nomes de controlo de acesso** separador, sob o **do Active Directory** serviço.

Para obter mais informações sobre o controlo de acesso, consulte [2.0 de serviço de controlo de acesso (arquivado)](https://msdn.microsoft.com/library/hh147631.aspx).

## <a name="retirement-schedule"></a>Agenda de extinção

A partir de Novembro de 2017, todos os componentes de controlo de acesso são totalmente suportadas e operacional. A única restrição é que lhe [não é possível criar novos espaços de nomes do controlo de acesso através do portal clássico do Azure](https://azure.microsoft.com/blog/acs-access-control-service-namespace-creation-restriction/).

Segue-se a agenda para descontinuar a componentes de controlo de acesso:

- **Novembro de 2017**: O administrador do Azure AD no portal clássico do Azure experiência [é retirado](https://blogs.technet.microsoft.com/enterprisemobility/2017/09/18/marching-into-the-future-of-the-azure-ad-admin-experience-retiring-the-azure-classic-portal/). Neste momento, a gestão de espaço de nomes para controlo de acesso está disponível um URL dedicado, novo: http://manage.windowsazure.com?restoreClassic=true. Utilize este URl para ver os espaços de nomes existentes, ativar e desativar espaços de nomes e eliminar os espaços de nomes, se optar por.
- **De 2018 Abril**: gestão de espaço de nomes do controlo de acesso já não está disponível no URL http://manage.windowsazure.com?restoreClassic=true dedicado. Neste momento, não é possível desativar ou ativar, eliminar ou enumerar os espaços de nomes do controlo de acesso. No entanto, o portal de gestão do controlo de acesso será totalmente funcional e localizado em https://\<espaço de nomes\>. accesscontrol.windows.net. Todos os outros componentes de controlo de acesso continuam a funcionar normalmente.
- **De 2018 Novembro**: controlo de acesso todos os componentes é encerrado permanentemente. Isto inclui o portal de gestão do controlo de acesso, o serviço de gestão, STS e o motor de regra de transformação de token. Neste momento, quaisquer pedidos enviados para o controlo de acesso (localizado em \<espaço de nomes\>. accesscontrol.windows.net) falhar. Deve ter migrou todas as aplicações e serviços existentes para outras tecnologias bem antes desta hora.


## <a name="migration-strategies"></a>Estratégias de migração

As secções seguintes descrevem recomendações de alto nível para migrar de controlo de acesso para outras tecnologias da Microsoft.

### <a name="clients-of-microsoft-cloud-services"></a>Clientes do cloud services da Microsoft

Cada serviço em nuvem da Microsoft que aceita tokens que são emitidos pelo controlo de acesso agora suporta, pelo menos, uma forma alternativa de autenticação. Varia consoante o mecanismo de autenticação correta para cada serviço. Recomendamos que consulte a documentação específica para cada serviço para obter orientações sobre oficial. Para sua comodidade, cada conjunto de documentação é fornecido aqui:

| Serviço | Orientação |
| ------- | -------- |
| Service Bus do Azure | [Migrar para assinaturas de acesso partilhado](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas) |
| Reencaminhamento de barramento de serviço do Azure | [Migrar para assinaturas de acesso partilhado](https://docs.microsoft.com/azure/service-bus-relay/relay-migrate-acs-sas) |
| Cache gerida do Azure | [Migrar para a Cache de Redis do Azure](https://docs.microsoft.com/azure/redis-cache/cache-faq#which-azure-cache-offering-is-right-for-me) |
| Azure DataMarket | [Migrar para as APIs de serviços cognitivos](https://docs.microsoft.com/azure/machine-learning/studio/datamarket-deprecation) |
| Serviços BizTalk | [Migrar para a funcionalidade de Logic Apps do App Service do Azure](https://docs.microsoft.com/azure/machine-learning/studio/datamarket-deprecation) |
| Os Media Services do Azure | [Migrar para a autenticação do Azure AD](https://azure.microsoft.com/blog/azure-media-service-aad-auth-and-acs-deprecation/) |
| Azure Backup | [Atualize o agente de cópia de segurança do Azure](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq) |

<!-- Dynamics CRM: Migrate to new SDK, Dynamics team handling privately -->
<!-- Azure RemoteApp deprecated in favor of Citrix: http://www.zdnet.com/article/microsoft-to-drop-azure-remoteapp-in-favor-of-citrix-remoting-technologies/ -->
<!-- Exchange push notifications are moving, customers don't need to move -->
<!-- Retail federation services are moving, customers don't need to move -->
<!-- Azure StorSimple: TODO -->
<!-- Azure SiteRecovery: TODO -->


### <a name="web-applications-that-use-passive-authentication"></a>Aplicações Web que utilizam autenticação passiva

Para aplicações web que utilizam o controlo de acesso para a autenticação de utilizador, o controlo de acesso fornece as seguintes funcionalidades e capacidades para programadores de aplicações web e arquitetos de TI:

- A integração com o Windows Identity Foundation (WIF).
- Federação com o Google, Facebook, Yahoo, Azure Active Directory, AD FS contas e e contas Microsoft.
- Suporte para os seguintes protocolos de autenticação: 13 de rascunho de OAuth 2.0, WS-Trust e Web dos serviços de Federação (WS-Federation).
- Suporte para os seguintes formatos de token: Token Web JSON (JWT), SAML 1.1, SAML 2.0 e simples Web tokens (SWT).
- Uma experiência de deteção de realm inicial, integrada no WIF, que permite aos utilizadores escolher o tipo de conta que utilizam para iniciar sessão. Esta experiência é alojada pela aplicação web e é totalmente personalizável.
- Token transformação que permite que a personalização avançada de afirmações recebidas pela aplicação web do controlo de acesso, incluindo:
    - Pass-through afirmações a partir de fornecedores de identidade.
    - Adição de afirmações personalizadas adicionais.
    - Lógica depois de se simples para emitir afirmações em determinadas condições.

Infelizmente, não é um serviço que oferece todas estas capacidades equivalentes. Deve avaliar que capacidades de controlo de acesso que precisa e, em seguida, escolha entre utilizar [do Azure Active Directory](https://azure.microsoft.com/develop/identity/signin/), [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) (Azure AD B2C), ou outra autenticação na nuvem serviço.

#### <a name="migrate-to-azure-active-directory"></a>Migrar para o Azure Active Directory

Um caminho a ter em consideração é integrar as suas aplicações e serviços diretamente com o Azure AD. Azure AD é o fornecedor de identidade baseada na nuvem do Microsoft profissional ou escolar contas. Azure AD é o fornecedor de identidade para o Office 365, Azure e muito mais. Fornece semelhante federado capacidades de autenticação para o controlo de acesso, mas não suporta todas as funcionalidades de controlo de acesso. 

O exemplo primário é federação com fornecedores de identidade de redes sociais, como o Facebook, Google e Yahoo. Se os seus utilizadores iniciar sessão com estes tipos de credenciais, do Azure AD não é a solução para si. 

Azure AD também não suporta necessariamente os protocolos de autenticação mesmo exato como controlo de acesso. Por exemplo, embora o controlo de acesso e o Azure AD suportarem OAuth, existem ligeiras diferenças entre cada implementação. Diferentes implementações exigem que modifique o código como parte de uma migração.

No entanto, do Azure AD fornece várias vantagens potenciais para clientes de controlo de acesso. -Nativamente suporta Microsoft escolar ou profissional contas alojadas na nuvem, que são frequentemente utilizadas pelos clientes de controlo de acesso. 

Também pode ser federado um inquilino do Azure AD para uma ou mais instâncias do Active Directory no local através do AD FS. Desta forma, a aplicação pode autenticar utilizadores baseados na nuvem e os utilizadores que estão alojados no local. Também suporta o protocolo WS-Federation, o que torna relativamente simples integrar com uma aplicação web utilizando WIF.

A tabela seguinte compara as funcionalidades do controlo de acesso que são relevantes para as aplicações web com essas funcionalidades que estão disponíveis no Azure AD. 

Um nível elevado, *do Azure Active Directory, provavelmente, é a melhor opção para a migração se permitir que os utilizadores iniciam sessão no apenas com os respetivos Microsoft profissional ou escolar contas*.

| Capacidade | Suporte de controlo de acesso | Suporte do Azure AD |
| ---------- | ----------- | ---------------- |
| **Tipos de contas** | | |
| Microsoft profissional ou escolar contas | Suportado | Suportado |
| Contas do Windows Server Active Directory e o AD FS |-Suportadas através de federação com um inquilino do Azure AD <br />-Suportadas através de Federação direta com o AD FS | Só é suportada através de federação com um inquilino do Azure AD | 
| Contas de outros sistemas de gestão de identidades empresariais |-Possível através de federação com um inquilino do Azure AD <br />-Suportadas através de Federação direta | Possível através de federação com um inquilino do Azure AD |
| Contas Microsoft para utilização pessoal | Suportado | Suportada através do Azure AD v 2.0 protocolo de OAuth, mas não através de quaisquer outros protocolos | 
| Contas do Facebook, Google, Yahoo | Suportado | Não é suportado whatsoever |
| **Protocolos e compatibilidade SDK** | | |
| WIF | Suportado | Estão disponíveis instruções suportadas, mas limitadas |
| WS-Federation | Suportado | Suportado |
| OAuth 2.0 | Suporte para rascunho 13 | Suporte para RFC 6749, a especificação de mais moderna |
| WS-Trust | Suportado | Não suportado |
| **Formatos de token** | | |
| JWT | Suportado na versão Beta | Suportado |
| SAML 1.1 | Suportado | Pré-visualização |
| SAML 2.0 | Suportado | Suportado |
| SWT | Suportado | Não suportado |
| **Personalizações** | | |
| Personalizável realm inicial deteção/conta-diretriz IU | Código transferível, que pode ser incorporado nas aplicações | Não suportado |
| Carregar certificados de assinatura de tokens personalizados | Suportado | Suportado |
| Personalizar afirmações no |-Pass-through afirmações de entrada a partir de fornecedores de identidade<br />-Obtenha acesso token do fornecedor de identidade como uma afirmação<br />-Emitir afirmações de saída com base nos valores de afirmações de entrada<br />-Emitir afirmações de saída com valores constantes |-Não é possível passar afirmações a partir de fornecedores de identidade federada<br />-Não é possível obter acesso token do fornecedor de identidade como uma afirmação<br />-Não é possível emitir afirmações de saída com base nos valores de afirmações de entrada<br />-Pode emitir afirmações de saída com valores constantes<br />-Podem emitir afirmações de saída com base nas propriedades de utilizadores sincronizados com o Azure AD |
| **Automatização** | | |
| Automatizar tarefas de gestão e configuração | Suportadas através do serviço de gestão do controlo de acesso | Suportada através do Microsoft Graph e do Azure AD Graph API |

Se decidir que do Azure AD é o caminho de migração melhor para as suas aplicações e serviços, deve ter conhecimento de duas formas de integrar a sua aplicação com o Azure AD.

Para utilizar o WS-Federation ou WIF para integrar com o Azure AD, recomendamos o seguinte, a abordagem descrita no [configurar federado-início de sessão único para uma aplicação não galeria](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-federated-sso-non-gallery). O artigo refere-se à configuração do Azure AD para baseados em SAML-início de sessão único, mas também funciona para a configuração de WS-Federation. Seguir esta abordagem requer uma licença do Azure AD Premium. Esta abordagem tem duas vantagens:

- Obter a total flexibilidade de personalização de token do Azure AD. Pode personalizar as afirmações que são emitidas pelo Azure AD para corresponder as afirmações que são emitidas pelo controlo de acesso. Isto inclui especialmente a afirmação de identificador de nome ou o ID de utilizador. Para continuar a receber de identificadores de utilizador consistente para os seus utilizadores depois de alterar as tecnologias, certifique-se de que o utilizador IDs emitido por correspondência do Azure AD os emitidos pelo controlo de acesso.
- Pode configurar um certificado de assinatura de tokens de mensagens em fila que é específico para a aplicação e, com uma duração controlado por si.

<!--

Possible nameIdentifiers from Access Control (via AAD or AD FS):
- AD FS - Whatever AD FS is configured to send (email, UPN, employeeID, what have you)
- Default from AAD using App Registrations, or Custom Apps before ClaimsIssuance policy: subject/persistent ID
- Default from AAD using Custom apps nowadays: UPN
- Kusto can't tell us distribution, it's redacted

-->

> [!NOTE]
> Esta abordagem requer uma licença do Azure AD Premium. Se for um cliente de controlo de acesso e requerem uma licença de premium para configurar o início de sessão único para uma aplicação, contacte-nos. Iremos irá todo o gosto fornecer licenças de programador para utilização.

Uma abordagem alternativa passa por seguir [este exemplo de código](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation), que dá instruções ligeiramente diferentes para a configuração de WS-Federation. Este exemplo de código não utiliza WIF, mas em vez disso, o middleware de ASP.NET 4.5 OWIN. No entanto, as instruções para o registo de aplicação são válidas para aplicações que utilizam o WIF e não necessitam de uma licença do Azure AD Premium. 

Se escolher esta abordagem, tem de compreender [rollover da chave de assinatura no Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-signing-key-rollover). Esta abordagem utiliza o Azure AD global de assinatura de tokens de problema a chave. Por predefinição, o WIF não atualiza automaticamente as chaves de assinatura. Quando o Azure AD roda respetivas chaves de assinatura global, a implementação de WIF tem de estar preparado para aceitar as alterações.

Se pode integrar com o Azure AD através de protocolos de OpenID Connect ou OAuth, é recomendável fazê-lo. Temos um vasto conjunto documentação e orientações sobre como integrar o Azure AD na sua aplicação web disponível no nosso [guia para programadores do Azure AD](http://aka.ms/aaddev).

<!-- TODO: If customers ask about authZ, let's put a blurb on role claims here -->

#### <a name="migrate-to-azure-active-directory-b2c"></a>Migrar para o Azure Active Directory B2C

Outro caminho de migração a ter em consideração é o Azure AD B2C. O Azure AD B2C é um serviço em nuvem autenticação, como o controlo de acesso, permite aos programadores outsource os respetivos lógica de gestão de identidade e autenticação para um serviço em nuvem. É um serviço pago (com escalões gratuitos e premium) que foi concebido para aplicações direcionadas para o consumidor que podem ter até milhões de utilizadores ativos.

Como o controlo de acesso, é uma das funcionalidades do Azure AD B2C mais apelativo que suporta vários tipos de contas. Com o Azure AD B2C, pode criar uma sessão de segurança de utilizadores utilizando a respetiva conta Microsoft, ou o Facebook, Google, LinkedIn, o GitHub ou Yahoo contas e muito mais. Azure AD B2C suporta também "contas locais", ou nome de utilizador e palavras-passe que os utilizadores criam especificamente para a sua aplicação. O Azure AD B2C também oferece extensibilidade avançada que pode utilizar para personalizar os fluxos de início de sessão. 

No entanto, Azure AD B2C não suporta o leque de protocolos de autenticação de token de formatos e que os clientes poderão exigir o controlo de acesso. Também é não é possível utilizar Azure AD B2C para obter os tokens e a consulta para obter informações adicionais sobre o utilizador do fornecedor de identidade, Microsoft ou, caso contrário.

A tabela seguinte compara as funcionalidades do controlo de acesso que são relevantes para as aplicações web com as que estão disponíveis no Azure AD B2C. Um nível elevado, *do Azure AD B2C provavelmente é a escolha certa para a migração se consumidor com acesso à sua aplicação, ou se suporta vários tipos de contas.*

| Capacidade | Suporte de controlo de acesso | Suporte do Azure AD B2C |
| ---------- | ----------- | ---------------- |
| **Tipos de contas** | | |
| Microsoft profissional ou escolar contas | Suportado | Suportada através de políticas personalizadas  |
| Contas do Windows Server Active Directory e o AD FS | Suportada através de Federação direta com o AD FS | Suportado através de Federação de SAML utilizando as políticas personalizadas |
| Contas de outros sistemas de gestão de identidades empresariais | Suportada através de Federação direta através do WS-Federation | Suportado através de Federação de SAML utilizando as políticas personalizadas |
| Contas Microsoft para utilização pessoal | Suportado | Suportado | 
| Contas do Facebook, Google, Yahoo | Suportado | Facebook e Google suportado nativamente, Yahoo suportado através de Federação OpenID Connect utilizando as políticas personalizadas |
| **Protocolos e compatibilidade SDK** | | |
| Windows Identity Foundation (WIF) | Suportado | Não suportado |
| WS-Federation | Suportado | Não suportado |
| OAuth 2.0 | Suporte para rascunho 13 | Suporte para RFC 6749, a especificação de mais moderna |
| WS-Trust | Suportado | Não suportado |
| **Formatos de token** | | |
| JWT | Suportado na versão Beta | Suportado |
| SAML 1.1 | Suportado | Não suportado |
| SAML 2.0 | Suportado | Não suportado |
| SWT | Suportado | Não suportado |
| **Personalizações** | | |
| Personalizável realm inicial deteção/conta-diretriz IU | Código transferível, que pode ser incorporado nas aplicações | IU totalmente personalizável através do CSS personalizado |
| Carregar certificados de assinatura de tokens personalizados | Suportado | Assinatura chaves personalizadas, certificados, suportadas através de políticas personalizadas |
| Personalizar afirmações no |-Pass-through afirmações de entrada a partir de fornecedores de identidade<br />-Obtenha acesso token do fornecedor de identidade como uma afirmação<br />-Emitir afirmações de saída com base nos valores de afirmações de entrada<br />-Emitir afirmações de saída com valores constantes |-Pode passar afirmações a partir de fornecedores de identidade políticas personalizadas necessárias para alguns afirmações<br />-Não é possível obter acesso token do fornecedor de identidade como uma afirmação<br />-Podem emitir afirmações de saída com base nos valores de afirmações de entrada através de políticas personalizadas<br />-Pode emitir afirmações de saída com valores constantes através de políticas personalizadas |
| **Automatização** | | |
| Automatizar tarefas de gestão e configuração | Suportadas através do serviço de gestão do controlo de acesso |-Criação de utilizadores permitido através do AD Graph API do Azure<br />-Não é possível criar inquilinos B2C, aplicações ou políticas através de programação |

Se decidir que o Azure AD B2C é o caminho de migração melhor para as suas aplicações e serviços, começar com os seguintes recursos:

- [Documentação do Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)
- [Políticas personalizadas do Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview-custom)
- [Preços do Azure AD B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/)


#### <a name="migrate-to-ping-identity-or-auth0"></a>Migrar a identidade de Ping ou Auth0

Em alguns casos, poderá achar que do Azure AD e o Azure AD B2C não são suficientes para substituir o controlo de acesso nas suas aplicações web sem alterações de código principal. Alguns exemplos comuns poderão incluir:

- Aplicações Web que utilizam o WIF ou WS-Federation para início de sessão com fornecedores de identidade de redes sociais, como o Google ou do Facebook.
- As aplicações Web que efetuar Federação direta de uma empresa identificam fornecedor através do protocolo WS-Federation.
- Aplicações Web que exigem o token de acesso emitido por um fornecedor de identidade sociais (tais como o Google ou do Facebook) como uma afirmação em tokens emitidos pelo controlo de acesso.
- Aplicações Web com complexas transformação token regras do Azure AD ou Azure AD B2C não consegue reproduzir.
- Aplicações web do multi-inquilino que utilizam ACS para gerir centralmente a Federação de vários fornecedores de identidade diferentes

Nestes casos, pode querer considere migrar a sua aplicação web para outro serviço de autenticação em nuvem. Recomendamos a explorar as seguintes opções. Cada uma das seguintes opções oferecem capacidades semelhantes ao controlo de acesso:



|     |     | 
| --- | --- |
| ![Auth0](./media/active-directory-acs-migration/rsz_auth0.png) | [Auth0](https://auth0.com/acs) é um serviço de identidade de nuvem flexível que criou [orientações de migração de elevado nível para clientes de controlo de acesso](https://auth0.com/acs)e suporta quase todas as funcionalidades que faz dos ACS. |
| ![Ping](./media/active-directory-acs-migration/rsz_ping.png) | [Identidade de ping](https://www.pingidentity.com) oferece duas soluções semelhantes às ACS. PingOne é um serviço de identidade de nuvem que suporta muitas das mesmas funcionalidades como ACS e PingFederate é um produto de identidade no local semelhantes que oferece uma maior flexibilidade. Consulte [orientações de extinção de ACS de Ping](https://www.pingidentity.com/company/blog/2017/11/20/migrating_from_microsoft_acs_to_ping_identity.html) para obter mais detalhes sobre como utilizar estes produtos.  |

Nosso objetivo trabalhar com a identidade de Ping e Auth0 é certificar-se de que todos os clientes de controlo de acesso tem um caminho de migração para os serviços e aplicações que minimiza a quantidade de trabalho necessário para mover de controlo de acesso.

<!--

## Sharepoint 2010, 2013, 2016

TODO: Azure AD only, use Azure AD SAML 1.1 tokens, when we bring it back online.
Other IDPs: use Auth0? https://auth0.com/docs/integrations/sharepoint.

-->

### <a name="web-services-that-use-active-authentication"></a>Serviços Web que utilizam a autenticação do Active Directory

Para os serviços web que estão protegidos com tokens emitidos pelo controlo de acesso, o controlo de acesso oferece as seguintes funcionalidades e capacidades:

- Capacidade de registar um ou mais *identidades do serviço* no seu espaço de nomes do controlo de acesso. Identidades de serviço podem ser utilizadas para pedir tokens.
- Suporte para o OAuth MOLDAR e OAuth 2.0 rascunho 13 protocolos para pedir tokens, utilizando os seguintes tipos de credenciais:
    - Uma palavra-passe simple que é criada para a identidade de serviço
    - A sessão iniciada SWT utilizando uma chave simétrica ou X509 certificado
    - Um token SAML emitido por um fornecedor de identidade fidedigna (normalmente, uma instância do AD FS)
- Suporte para os seguintes formatos de token: JWT, SAML 1.1, SAML 2.0 e SWT.
- Regras de transformação de token simples.

Identidades de serviço no controlo de acesso são tipicamente usadas para implementar a autenticação de servidor para servidor.  

#### <a name="migrate-to-azure-active-directory"></a>Migrar para o Azure Active Directory

A nossa recomendação para este tipo de fluxo de autenticação está a migrar para [do Azure Active Directory](https://azure.microsoft.com/develop/identity/signin/). Azure AD é o fornecedor de identidade baseada na nuvem do Microsoft profissional ou escolar contas. Azure AD é o fornecedor de identidade para o Office 365, Azure e muito mais. 

Também pode utilizar o Azure AD para autenticação de servidor para servidor utilizando a implementação do Azure AD de concessão de credenciais de cliente OAuth. A tabela seguinte compara as capacidades do controlo de acesso na autenticação de servidor para servidor com as que estão disponíveis no Azure AD.

| Capacidade | Suporte de controlo de acesso | Suporte do Azure AD |
| ---------- | ----------- | ---------------- |
| Como registar um serviço web | Criar uma entidade confiadora no portal de gestão do controlo de acesso | Criar uma aplicação web do Azure AD no portal do Azure |
| Como registar um cliente | Criar uma identidade de serviço no portal de gestão do controlo de acesso | Criar outra aplicação web do Azure AD no portal do Azure |
| Protocolo utilizado |-Protocolo de moldagem OAuth<br />-As credenciais de cliente OAuth 2.0 rascunho 13 conceder | Conceder credenciais de cliente OAuth 2.0 |
| Métodos de autenticação de cliente |-Palavra-passe simples<br />-SWT assinado<br />-Token SAML de um fornecedor de identidade federada |-Palavra-passe simples<br />-JWT assinado |
| Formatos de token |-JWT<br />-SAML 1.1<br />-SAML 2.0<br />-SWT<br /> | Apenas JWT |
| Transformação de token |-Adicionar afirmações personalizadas<br />-Lógica de emissão de afirmação simples de depois de se | Adicionar afirmações personalizadas | 
| Automatizar tarefas de gestão e configuração | Suportadas através do serviço de gestão do controlo de acesso | Suportada através do Microsoft Graph e do Azure AD Graph API |

Para obter orientações sobre cenários de servidor para servidor de implementação, consulte os seguintes recursos:

- Secção de serviços a [guia para programadores do Azure AD](https://aka.ms/aaddev)
- [Exemplo de código do daemon utilizando as credenciais de cliente de palavra-passe simples](https://github.com/Azure-Samples/active-directory-dotnet-daemon)
- [Exemplo de código do daemon utilizando credenciais de certificado de cliente](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential)

#### <a name="migrate-to-ping-identity-or-auth0"></a>Migrar a identidade de Ping ou Auth0

Em alguns casos, poderá achar que as credenciais do cliente do Azure AD e o OAuth conceder implementação não são suficientes para substituir o controlo de acesso na sua arquitetura sem alterações de código principal. Alguns exemplos comuns poderão incluir:

- Autenticação de servidor para servidor utilizando o token formatos que não sejam JWTs.
- Autenticação de servidor para servidor utilizando um token de entrada fornecido por um fornecedor de identidade externas.
- Autenticação de servidor para servidor com as regras de transformação de token do Azure AD não consegue reproduzir.

Nestes casos, poderá considerar migrar a sua aplicação web para outro serviço de autenticação em nuvem. Recomendamos a explorar as seguintes opções. Cada uma das seguintes opções oferecem capacidades semelhantes ao controlo de acesso:

|     |     | 
| --- | --- |
| ![Auth0](./media/active-directory-acs-migration/rsz_auth0.png) | [Auth0](https://auth0.com/acs) é um serviço de identidade de nuvem flexível que criou [orientações de migração de elevado nível para clientes de controlo de acesso](https://auth0.com/acs)e suporta quase todas as funcionalidades que faz dos ACS. |
| ![Ping](./media/active-directory-acs-migration/rsz_ping.png) | [Identidade de ping](https://www.pingidentity.com) oferece duas soluções semelhantes às ACS. PingOne é um serviço de identidade de nuvem que suporta muitas das mesmas funcionalidades como ACS e PingFederate é um produto de identidade no local semelhantes que oferece uma maior flexibilidade. Consulte [orientações de extinção de ACS de Ping](https://www.pingidentity.com/company/blog/2017/11/20/migrating_from_microsoft_acs_to_ping_identity.html) para obter mais detalhes sobre como utilizar estes produtos.  |

Nosso objetivo trabalhar com a identidade de Ping e Auth0 é certificar-se de que todos os clientes de controlo de acesso tem um caminho de migração para os serviços e aplicações que minimiza a quantidade de trabalho necessário para mover de controlo de acesso.

## <a name="questions-concerns-and-feedback"></a>Perguntas, preocupações e comentários

Compreendemos que muitos clientes de controlo de acesso não irão localizar um caminho de migração limpar depois de ler este artigo. Poderá ter alguns assistência ou orientações para determinar o plano adequado. Se gostaria de abordar os cenários de migração e perguntas, deixe um comentário nesta página.
