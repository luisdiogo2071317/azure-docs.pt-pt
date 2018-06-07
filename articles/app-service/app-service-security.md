---
title: Segurança no App Service do Azure e as funções do Azure | Microsoft Docs
description: Saiba mais sobre como o serviço de aplicações ajuda segura a aplicação e como lhe pode ainda mais bloquear a aplicação contra ameaças.
keywords: serviço de aplicações do Azure, aplicação web, aplicação móvel, aplicação api, aplicação de função, segurança, segura e protegida, a conformidade, em conformidade, certificado, certificados, https, ftps, tls, a fidedignidade, a encriptação, encriptar, encriptados, restrição de ip, autenticação, autorização, authn, autho, msi, identidade de serviço geridas, os segredos, segredo, a aplicação de patches, patch, patches, versão, isolamento, o isolamento de rede, ddos, mitm
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2018
ms.author: cephalin
ms.openlocfilehash: 2ca1c1518589e60a03570e1c2063381f749ed9aa
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655367"
---
# <a name="security-in-azure-app-service-and-azure-functions"></a>Segurança no App Service do Azure e as funções do Azure

Este artigo mostra como [App Service do Azure](app-service-web-overview.md) ajuda a proteger a sua aplicação web, o back-end da aplicação móvel, a aplicação API, e [das funções do Azure](/azure/azure-functions/). Também mostra como pode proteger ainda mais a aplicação com as funcionalidades do serviço de aplicações incorporadas.

Os componentes de plataforma do App Service, incluindo VMs do Azure, ligações de rede, armazenamento, gestão e a integração de funcionalidades, e estruturas web ativamente estão protegidos e protegidos. Serviço de aplicações passa através de verificações de compatibilidade vigorous numa base contínua para se certificar de que:

- Os recursos de aplicação são [protegidos](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) a partir dos recursos do Azure de outros clientes.
- [Instâncias de VM e tempo de execução software que são atualizadas regularmente](app-service-patch-os-runtime.md) às vulnerabilidades de endereço recentemente detetado. 
- Comunicação de segredos (por exemplo, cadeias de ligação) entre a aplicação e outros recursos do Azure (tais como [base de dados SQL](/services/sql-database/)) permanece no Azure e não cruzada quaisquer limites de rede. Os segredos são sempre encriptados quando armazenada.
- Funcionalidades de todas as comunicações através da conectividade do serviço de aplicações, tais como [da ligação híbrida](app-service-hybrid-connections.md), são encriptados. 
- As ligações com ferramentas de gestão remota do PowerShell do Azure, CLI do Azure, SDKs do Azure, as APIs REST, são todos encriptadas.
- ameaças de 24 horas gestão protege a infraestrutura e plataforma contra software maligno, distribuída denial of service (DDoS) distribuídos, ataques man-in-the-middle (MITM) e outras ameaças.

Para obter mais informações sobre a segurança da infraestrutura e plataforma no Azure, consulte [Centro de fidedignidade do Azure](https://azure.microsoft.com/overview/trusted-cloud/).

As secções seguintes mostram como proteger ainda mais a aplicação de serviço de aplicações de ameaças.

## <a name="https-and-certificates"></a>HTTPS e certificados

Serviço de aplicações permite-lhe proteger as suas aplicações com [HTTPS](https://wikipedia.org/wiki/HTTPS). Quando é criada a sua aplicação, o nome de domínio predefinido (\<APP_NAME>.azurewebsites.NET >. azurewebsites.net) já se encontra acessível através de HTTPS. Se lhe [configurar um domínio personalizado para a sua aplicação](app-service-web-tutorial-custom-domain.md), deve também [Proteja-a com um certificado personalizado](app-service-web-tutorial-custom-ssl.md) , para que os browsers cliente podem fazer as ligações de HTTPS protegidas para o domínio personalizado. Existem duas formas de fazê-lo:

- **Certificado de serviço de aplicações** -criar um certificado diretamente no Azure. O certificado está protegido na [Cofre de chaves do Azure](/azure/key-vault/)e podem ser importados para a aplicação de serviço de aplicações. Para obter mais informações, consulte [comprar e configurar um certificado SSL para o serviço de aplicações do Azure](web-sites-purchase-ssl-web-site.md).
- **Certificados de terceiros** - carregar um certificado SSL personalizado comprado numa autoridade de certificação fidedigna e vinculá-lo à sua aplicação de serviço de aplicações. Serviço de aplicações suporta certificados de domínio único e certificados de caráter universal. Também suporta certificados autoassinados para fins de teste. Para obter mais informações, consulte [vincular um certificado SSL personalizado existente para Web Apps do Azure](app-service-web-tutorial-custom-ssl.md).

## <a name="insecure-protocols-http-tls-10-ftp"></a>Inseguras protocolos HTTP, TLS 1.0, o protocolo FTP)

Para proteger a sua aplicação em relação a todas as ligações de (protocolo HTTP) não encriptadas, o App Service fornece configuração de um clique para impor o HTTPS. Pedidos protegidos estão ativados ausente antes de atingirem mesmo código da aplicação. Para obter mais informações, consulte [impor HTTPS](app-service-web-tutorial-custom-ssl.md#enforce-https).

[TLS](https://wikipedia.org/wiki/Transport_Layer_Security) 1.0 é já não é considerado seguro pelas normas do sector, como [PCI DSS](https://wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard). Serviço de aplicações permite-lhe desativar os protocolos desatualizados por [impor o TLS 1.1/1.2](app-service-web-tutorial-custom-ssl.md#enforce-tls-1112).

Serviço de aplicações suporta FTP e FTPS para implementar os seus ficheiros. No entanto, FTPS deve ser utilizada em vez de FTP, se, todos os possível. Quando um ou ambos estes protocolos não estão em utilização, deverá [desativá-las](app-service-deploy-ftp.md#enforce-ftps).

## <a name="static-ip-restrictions"></a>Restrições de IP estáticas

Por predefinição, a aplicação de serviço de aplicações aceita os pedidos de todos os endereços IP da internet, mas poderá limitar a que o acesso a um pequeno subconjunto de endereços IP. Serviço de aplicações no Windows permite-lhe definir uma lista de endereços IP que estão autorizados a aceder à sua aplicação. Lista de permitidos pode incluir os endereços IP individuais ou um intervalo de endereços IP definidos por uma máscara de sub-rede. Para obter mais informações, consulte [restrições de IP do Azure App Service estático](app-service-ip-restrictions.md).

Para o serviço de aplicações no Windows, também pode restringir os endereços IP dinamicamente configurando o _Web. config_. Para obter mais informações, consulte [de segurança de IP dinâmico <dynamicIpSecurity> ](https://docs.microsoft.com/iis/configuration/system.webServer/security/dynamicIpSecurity/).

## <a name="client-authentication-and-authorization"></a>Autorização e autenticação de cliente

App Service do Azure fornece ative a chave de autenticação e autorização de utilizadores ou cliente aplicações. Quando ativada, pode iniciar sessão em utilizadores e aplicações de cliente com pouca ou nenhuma código da aplicação. Pode implementar a sua própria solução de autenticação e autorização, ou permitir que o serviço de aplicação processá-lo por si. O módulo de autenticação e autorização processa pedidos web antes de manipulação-los desativado para o código da aplicação e nega pedidos não autorizados antes de atingirem o seu código.

Autorização e autenticação do serviço de aplicações suportam vários fornecedores de autenticação, incluindo o Azure Active Directory, as contas de Microsoft, Facebook, Google e Twitter. Para obter mais informações, consulte [autenticação e autorização no serviço de aplicações do Azure](app-service-authentication-overview.md).

## <a name="service-to-service-authentication"></a>Autenticação serviço a serviço

Quando autenticação relativamente do serviço de back-end, o serviço de aplicações fornece dois mecanismos diferentes consoante a sua necessidade:

- **Identidade de serviço** -iniciar sessão para o recurso remoto utilizando a identidade da aplicação em si. Serviço de aplicações permite-lhe criar facilmente uma [identidade do serviço gerido](app-service-managed-service-identity.md), que pode ser utilizado para autenticar com outros serviços, tais como [SQL Database do Azure](/azure/sql-database/) ou [Cofre de chaves do Azure](/azure/key-vault/). Para um tutorial ponto-a-ponto desta abordagem, consulte [ligação de proteger a base de dados de SQL do Azure do serviço de aplicações através de identidade do serviço gerido](app-service-web-tutorial-connect-msi.md).
- **Em-nome-de (OBO)** -efetuar o acesso delegado a recursos remotos em nome do utilizador. Com o Azure Active Directory como o fornecedor de autenticação, a aplicação de serviço de aplicações pode efetuar delegado início de sessão para um serviço remoto, tais como [Active Directory Graph API do Azure](../active-directory/develop/active-directory-graph-api.md) ou uma aplicação API remota no App Service. Para um tutorial ponto-a-ponto desta abordagem, consulte [autenticar e autorizar utilizadores ponto-a-ponto no App Service do Azure](app-service-web-tutorial-auth-aad.md).

## <a name="connectivity-to-remote-resources"></a>Conectividade a recursos remotos

Existem três tipos de recursos remotos, que a aplicação poderá ter de aceder a: 

- [Recursos do Azure](#azure-resources)
- [Recursos dentro de uma rede Virtual do Azure](#resources-inside-an-azure-virtual-network)
- [Recursos no local](#on-premises-resources)

Em cada um nestes casos, o App Service fornece uma forma de fazer ligações seguras, mas deve observar ainda melhores práticas de segurança. Por exemplo, utilize sempre ligações encriptadas, mesmo que o recurso de back-end permite ligações não encriptadas. Além disso, certifique-se de que o serviço do Azure de back-end permite que o conjunto mínimo de endereços IP. Pode encontrar os endereços IP de saída para a sua aplicação em [de entrada e saídos endereços IP no App Service do Azure](app-service-ip-addresses.md).

### <a name="azure-resources"></a>Recursos do Azure

Quando a aplicação liga a recursos do Azure, tal como [base de dados SQL](/services/sql-database/) e [Storage do Azure](/azure/storage/), a ligação permanece no Azure e não cruzada quaisquer limites de rede. No entanto, a ligação atravessa a rede partilhada no Azure, sempre, por isso, certifique-se de que a ligação está encriptada. 

Se a aplicação estiver alojada num [ambiente de serviço de aplicações](environment/intro.md), deve [ligar a serviços do Azure através de pontos finais do serviço de rede Virtual suportados pelo](../virtual-network/virtual-network-service-endpoints-overview.md).

### <a name="resources-inside-an-azure-virtual-network"></a>Recursos dentro de uma rede Virtual do Azure

A aplicação pode aceder a recursos num [Azure Virtual Network](/azure/virtual-network/) através de [integração de rede Virtual](web-sites-integrate-with-vnet.md). A integração for estabelecida com uma rede Virtual com uma VPN ponto a site. A aplicação, em seguida, pode aceder aos recursos na rede Virtual com os respetivos endereços IP privados. A ligação de ponto a site, no entanto, ainda passarem por redes partilhadas no Azure. 

Para isolar a conectividade de recurso completamente das redes partilhadas no Azure, criar a sua aplicação num [ambiente de serviço de aplicações](environment/intro.md). Uma vez que um ambiente de serviço de aplicações sempre for implementado para uma rede Virtual dedicado, a conectividade entre a sua aplicação e os recursos dentro da rede Virtual está completamente isolada. Para outros aspetos de segurança de rede num ambiente de serviço de aplicações, consulte [isolamento de rede](#network-isolation).

### <a name="on-premises-resources"></a>Recursos no local

Pode aceder em segurança a recursos no local, como bases de dados, de três formas diferentes: 

- [As ligações híbridas](app-service-hybrid-connections.md) -estabelece uma ligação ponto a ponto para o recurso remoto através de um túnel TCP. O túnel TCP é estabelecido utilizando TLS 1.2 com chaves de assinatura (SAS) de acesso partilhado.
- [Integração de rede virtual](web-sites-integrate-with-vnet.md) com VPN de site-site - conforme descrito em [recursos dentro de uma rede Virtual do Azure](#resources-inside-an-azure-virtual-network), mas a rede Virtual podem ser ligada à sua rede no local através de um [ VPN de site para site](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md). Nesta topologia de rede, a aplicação pode ligar a recursos no local, como a outros recursos na rede Virtual.
- [Ambiente de serviço de aplicações](environment/intro.md) com VPN de site-site - conforme descrito em [recursos dentro de uma rede Virtual do Azure](#resources-inside-an-azure-virtual-network), mas a rede Virtual podem ser ligada à sua rede no local através de um [site a site VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md). Nesta topologia de rede, a aplicação pode ligar a recursos no local, como a outros recursos na rede Virtual.

## <a name="application-secrets"></a>Segredos de aplicação

Não armazene segredos de aplicação, tais como as credenciais da base de dados, tokens de API e as chaves privadas nos seus ficheiros de configuração ou de código. A abordagem geralmente aceite está a aceder aos mesmos como [variáveis de ambiente](https://wikipedia.org/wiki/Environment_variable) através do padrão de padrão no seu idioma à escolha. No App Service, a forma de definir as variáveis de ambiente é através de [as definições de aplicação](web-sites-configure.md#app-settings) (e, especialmente para aplicações de .NET, [cadeias de ligação](web-sites-configure.md#connection-strings)). As definições de aplicação e cadeias de ligação são armazenadas de forma encriptada no Azure e está a ser desencriptados apenas antes de a ser injetado no memória de processo da sua aplicação quando a aplicação for iniciada. As chaves de encriptação rodam regularmente.

Em alternativa, pode integrar a aplicação de serviço de aplicações com [Cofre de chaves do Azure](/azure/key-vault/) para a gestão de segredos avançadas. Por [aceder ao Cofre de chaves com uma identidade de serviço geridas](../key-vault/tutorial-web-application-keyvault.md), a aplicação do app Service pode aceder em segurança os segredos precisa.

## <a name="network-isolation"></a>Isolamento de rede

Exceto para o **Isolated** escalão de preço, todos os escalões executar as suas aplicações na infraestrutura de rede partilhado no App Service. Por exemplo, os endereços IP públicos e Balanceadores de carga de front-end são partilhados com outros inquilinos. O **Isolated** camada dá-lhe o isolamento de rede completa, executando as suas aplicações dentro de um dedicado [ambiente de serviço de aplicações](environment/intro.md). Um ambiente de serviço de aplicação é executada na sua própria instância do [Azure Virtual Network](/azure/virtual-network/). Permite-lhe: 

- Restringir o acesso de rede com [grupos de segurança de rede](../virtual-network/virtual-networks-nsg.md). 
- Servir as suas aplicações através de um ponto final público dedicado, com extremidades front-dedicadas.
- Servir aplicação interna com um balanceador de carga interno (ILB), que permite o acesso apenas a partir de dentro da sua rede Virtual do Azure. O ILB tem um endereço IP do seu sub-rede privada, o que fornece isolamento total das suas aplicações a partir da internet.
- [Utilize um ILB atrás de uma firewall de aplicação web (WAF)](environment/integrate-with-application-gateway.md). O WAF proporciona proteção de nível empresarial às suas aplicações destinado ao público, como proteção DDoS, filtragem de URI e prevenção de injeção de SQL.

Para obter mais informações, consulte [introdução para ambientes do App Service do Azure](environment/intro.md).