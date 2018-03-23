---
title: Migrar aplicações no local do AD FS para o Azure. | Microsoft Docs
description: Este documento destina-se a ajudar as organizações a compreender como migrar as aplicações no local para o Azure AD, com especial destaque para as aplicações SaaS federadas.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/02/2018
ms.author: billmath
ms.openlocfilehash: f1256e2fefe1ad3d37403f37804888ab9dcf6e99
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="migrate-ad-fs-on-premises-apps-to-azure"></a>Migrar aplicações no local do AD FS para o Azure 

Este documento destina-se a ajudar as organizações a compreender como migrar as aplicações no local para o Azure AD.  Aborda em especial as aplicações SaaS federadas.  Este documento não é uma orientação passo a passo.  Disponibiliza orientações conceptuais para o ajudar a concretizar a migração ao explicar de que forma é que as configurações no local se traduzem no Azure AD. Também abrange os requisitos necessários para os cenários mais comuns.

## <a name="introduction"></a>Introdução

Se tiver um diretório no local que contenha as contas de utilizador, é provável que tenha, pelo menos, uma ou duas aplicações.  E estas aplicações estão configuradas para os utilizadores acederem mediante o início de sessão com essas identidades.

E se a sua organização for igual à maioria, o mais provável é que já esteja a meio caminho para adotar as aplicações e identidades na cloud.  Se calhar, está a utilizar o Office 365 e o Azure AD Connect.  Talvez tenha configurado aplicações SaaS baseadas na cloud para algumas cargas de trabalho importantes, mas não para todas.  

Muitas organizações têm aplicações SaaS ou de linha de negócio (LoB) personalizadas federadas diretamente num serviço de início de sessão no local, como o Active Directory Federation Service (AD FS), juntamente com aplicações baseadas no Office 365 e no Azure AD.  Este guia de migração descreve como e porquê migrar aplicações no local para o Azure AD.

>[!NOTE]
>Este guia disponibiliza informações detalhadas sobre a configuração e migração de aplicações SaaS, com informações de alto nível relativamente a aplicações LoB personalizadas.  Está prevista a disponibilização de mais orientações detalhadas para aplicações LoB no futuro.

Figura 1: aplicações ligadas diretamente no local ![no local](media/migrate-adfs-apps-to-azure/migrate1.png)

Figura 2: aplicações federadas através do Azure AD ![Azure](media/migrate-adfs-apps-to-azure/migrate2.png)

## <a name="why-migrate-apps-to-azure-ad"></a>Porquê migrar aplicações para o Azure AD?

Às organizações que já utilizam o AD FS, o Ping ou outro fornecedor de autenticação no local, a migração das aplicações para o Azure AD oferece as vantagens seguintes:

**Acesso mais seguro**
- Configurar controlos de acesso granulares por aplicação, incluindo a Multi-Factor Authentication (MFA), com o [Acesso Condicional do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).  Estas políticas podem ser aplicadas a aplicações SaaS e personalizadas tal e qual como faz atualmente para o Office 365
- Para detetar ameaças e proteger o início de sessão com base em machine learning e heurística que identificam o tráfego de risco, tire partido das capacidades incorporadas e em constante evolução do Azure AD com o [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)

**Colaboração B2B do Azure AD**
- Quando o início de sessão único nas aplicações SaaS se basear no Azure AD, pode dar aos parceiros acesso a recursos na cloud com a [colaboração B2B do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).

**Experiência de administração mais fácil e capacidades adicionais do Azure Ad**
- Enquanto fornecedor de identidades para aplicações SaaS, o Azure AD suporta capacidades adicionais, como certificados de assinatura de tokens por aplicação, [datas de expiração de certificados configuráveis](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs) e [aprovisionamento automatizado](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning) de contas de utilizador (em aplicações importantes da Galeria) com base em identidades do Azure AD.

**Manter as vantagens de um fornecedor de identidades no local**
- Ao mesmo tempo que beneficia das vantagens do Azure AD, pode continuar a utilizar a sua solução no local de identidades, de modo a preservar benefícios como Multi-Factor Authentication (MFA) no local, registo e auditoria. 

**Aprender a extinguir rapidamente o fornecedor de identidades no local**
- Para as organizações que pretendem extinguir o produto de autenticação no loca, a migração das aplicações para o Azure AD permite uma transição mais fácil, ao remover parte do trabalho 

## <a name="mapping-types-of-apps-on-premises-to-types-of-apps-in-azure-ad"></a>Mapear tipos de aplicações no local para tipos de aplicações no Azure AD
A maioria das aplicações pertence a uma de algumas categorias com base no tipo de início de sessão que utilizam.  Estas categorias determinam a forma como a aplicação é representada no Azure AD.

Resumindo, as aplicações SAML 2.0 podem ser integradas no Azure AD através da galeria de aplicações do Azure AD ou como aplicações externas à galeria.  Do mesmo modo, as aplicações que utilizam OAuth 2.0 ou OpenID Connect podem ser integradas no Azure AD como “registos de aplicações”.  Leia mais detalhes abaixo.

### <a name="federated-saas-apps-vs-custom-lob-apps"></a>Aplicações SaaS vs. aplicações LoB personalizadas federadas
As aplicações federadas incluem aplicações das categorias apresentadas a seguir.

- Aplicações SaaS 
    - Se os seus utilizadores iniciarem sessão nas aplicações SaaS, como Salesforce, ServiceNow, ou Workday, e estiver a integrar num fornecedor de identidades no local, como o AD FS ou o Ping, significa que está a utilizar o início de sessão federado nas aplicações SaaS.
    - Geralmente, as aplicações utilizam o protocolo SAML 2.0 para início de sessão federado.
    - As aplicações que pertencem a esta categoria podem ser integradas no Azure AD como Aplicações Empresariais, a partir da galeria ou como aplicações externas à galeria.
- Aplicações LoB personalizadas
    - Referem-se a aplicações não SaaS, desenvolvidas internamente pela sua organização ou disponíveis como produtos de pacote padrão que são instalados no seu datacenter.  Incluem aplicações do SharePoint e aplicações criadas com base no Windows Identity Foundation (WIF).
    - As aplicações podem utilizar SAML 2.0, WS-Federation, OAuth ou OpenID Connect para o início de sessão federado
    - As aplicações personalizadas que utilizem Oauth 2.0, OpenID Connect ou WS-Federation podem ser integradas no Azure AD como Registos de aplicações e as que utilizem SAML 2.0 ou WS-Federation podem ser integradas como aplicações externas à galeria dentro das Aplicações Empresariais.

### <a name="non-federated-apps"></a>Aplicações não federadas
Além disso, as aplicações não federadas podem ser integradas no Azure AD através do Proxy de Aplicações do Azure AD e de funcionalidades associadas.  Para obter mais informações sobre as capacidades, siga as ligações:
- Aplicações que utilizam Windows Integrated Auth (WIA) diretamente no Active Directory
    - Estas aplicações podem ser integradas no Azure AD através do[Proxy de Aplicações do Azure AD](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal)
- Aplicações que se integram no seu fornecedor de início de sessão único através de um agente e que utilizam cabeçalhos para autorização
    - as aplicações no local que utilizam um agente instalado para início de sessão e a autorização baseada em cabeçalhos podem ser configuradas para o início de sessão baseado no Azure AD através do Proxy de Aplicações do Azure AD com o [Acesso de Ping ao Azure AD](https://blogs.technet.microsoft.com/enterprisemobility/2017/06/15/ping-access-for-azure-ad-is-now-generally-available-ga/)

## <a name="translating-on-premises-federated-apps-to-azure-ad"></a>Traduzir aplicações federadas no local para o Azure AD 
Felizmente, o AD FS e o Azure funcionam de forma semelhante, pelo que os conceitos de configurar os identificadores e os URLs de confiança, início de sessão e fim de sessão aplicam-se em ambos os casos.  No entanto, existem algumas diferenças pequenas que tem de compreender à medida que faz a transição.

Mapeámos várias ideias importantes que o AD FS, o Azure AD e as aplicações SaaS partilham na tabela abaixo, para o ajudar a traduzir. 

### <a name="representing-the-app-in-azure-ad-or-ad-fs"></a>Representar a aplicação no Azure AD ou no AD FS
A migração começa com a avaliação da forma como a aplicação está configurada no local e mapear essa configuração para o Azure AD.  Abaixo pode ver o mapeamento dos elementos de configuração da entidade confiadora do AD FS para os elementos correspondentes no Azure AD.  
- Termo do AD FS: entidade confiadora ou confiança da entidade confiadora
- Termo do Azure AD: aplicação empresarial ou Registo de aplicações (consoante o tipo de aplicação)

|Elemento de Configuração da Aplicação|Descrição|Na configuração do AD FS|Localização correspondente na configuração do Azure AD|Elemento de Token SAML|
|-----|-----|-----|-----|-----|
|URL de início de sessão da aplicação|URL da página de início de sessão desta aplicação. É aonde o utilizador acede para iniciar um início de sessão na aplicação num fluxo SAML iniciado por SP.|N/D|No Azure AD, o URL de início de sessão é configurado no portal do Azure, nas Propriedades de Início de Sessão Único da aplicação, como o URL de início de sessão.</br></br>(poderá ter de clicar em “Mostrar definições de URL avançadas” para ver o URL de início de sessão)||
|URL de Resposta da Aplicação|URL da aplicação na perspetiva do IdP.  É para aqui que o utilizador e o token são enviados depois de o utilizador iniciar sessão no IdP.</br></br>  Por vezes, é denominado “ponto final de consumidor de asserção de SAML”.|Encontrado na Fidedignidade da Entidade Confiadora do AD FS da aplicação.  Clique com o botão direito do rato e selecione "Propriedades" -> separador "Pontos finais".|No Azure AD, o URL de resposta é configurado no portal do Azure, nas Propriedades de Início de Sessão Único da aplicação, como o URL de resposta.</br></br>(poderá ter de clicar em “Mostrar definições de URL avançadas” para ver o URL de resposta)|É mapeado para o elemento Destino no token SAML.</br></br>  Valor de exemplo: https://contoso.my.salesforce.com|
|URL de fim de sessão da aplicação|URL para o qual são enviados os pedidos de “limpeza de fim de sessão” quando o utilizador termina sessão numa aplicação, para terminar sessão em todas as outras aplicações nas quais o IdP tenha iniciado sessão pelo utilizador.|Encontrado na Gestão do AD FS em Fidedignidades da Entidade Confiadora.  Clique com o botão direito do rato em RP e selecione "Propriedades" -> Clique no separador "Pontos finais".|N/D – o Azure AD não suporta o “fim de sessão único”, o que significa terminar sessão em todas as aplicações.  Simplesmente termina a sessão do utilizador do próprio Azure AD.|ND|
|Identificador de Aplicação|O identificador da aplicação na perspetiva do IdP O valor do URL de início de sessão é, muitas vezes, utilizado como o Identificador (mas não sempre)</br></br>  Por vezes, a aplicação refere-se a este identificador como "ID da Entidade".|No AD FS, este é o ID da Entidade Confiadora: clique com o botão direito do rato na fidedignidade da entidade confiadora e selecione "Propriedades" -> Clique no separador "Identificadores"|No Azure AD, o identificador é configurado no portal do Azure, nas Propriedades de Início de Sessão Único da aplicação, como o Identificador em Domínio e URLs (poderá ter de clicar na caixa de verificação “Mostrar definições de URL avançadas”)|Corresponde ao elemento Audiência no token SAML|
|Metadados de Federação da Aplicação|Localização dos metadados de federação da aplicação.  O IdP utiliza-os para atualizar automaticamente definições de configuração específicas, como Pontos Finais ou certificados de encriptação.|O URL dos Metadados de Federação da Aplicação encontra-se na Fidedignidade da Entidade Confiadora do AD FS da aplicação.  Clique com o botão direito do rato na confiança, escolha Propriedades clique no separador Monitorização.|N/D - o Azure AD não suporta o consumo direto dos metadados de federação da aplicação.|ND|
|Identificador de Utilizador / NameID|Atributo que é utilizado para indicar exclusivamente a identidade a partir do utilizador do Azure AD ou do AD FS para a sua aplicação.</br></br>  Normalmente, é o UPN ou o endereço de e-mail do utilizador.|No AD FS, encontra-se como regra de afirmação na entidade confiadora.  Na maioria dos casos, é a regra de afirmação que emite uma afirmação com um tipo que termina em "nameidentifier"|No Azure AD, o Identificador de Utilizador pode ser encontrado no portal do Azure, nas Propriedades de Início de Sessão Único da aplicação, no cabeçalho Atributos do Utilizador.</br></br>Por predefinição, é utilizado o UPN.|Comunicado do IdP para a aplicação como o elemento “NameID” no token SAML.|
|Outras afirmações que vão ser enviadas para a aplicação|Para além do Identificador de Utilizador / NameID, são geralmente enviadas outras informações de afirmações do IdP para a aplicação, como, por exemplo, o nome próprio, o apelido, o endereço de e-mail e grupos dos quais o utilizador seja membro|No AD FS, encontram-se como outras regras de afirmações na entidade confiadora.|No Azure AD, no portal do Azure, nas Propriedades de Início de Sessão Única da aplicação, no cabeçalho Atributos do utilizador, clique em Ver e edite todos os outros atributos de utilizador.|| 

### <a name="representing-azure-ad-as-an-identity-provider-idp-in-a-saas-app"></a>Representar o Azure AD como Fornecedor de Identidade (IdP) em aplicações SaaS
Como parte da migração, a aplicação tem de ser configurada para apontar para o Azure AD (vs. o fornecedor de identidade no local).  Esta secção debruça-se principalmente nas aplicações SaaS que utilizam o protocolo SAML e não nas aplicações LoB/personalizadas. No entanto, os conceitos descritos estendem-se a estas últimas. 

Num nível mais elevado, existem alguns aspetos importantes para apontar uma aplicação SaaS para o Azure AD
- Emissor do Fornecedor de Identidade:https&#58;//sts.windows.net/{tenant-id}/
- URL de Início de Sessão do Fornecedor de Identidade: https&#58;//login.microsoftonline.com/{tenant-id}/saml2
- URL de Fim de Sessão do Fornecedor de Identidade: https&#58;//login.microsoftonline.com/{tenant-id}/saml2 
- Localização dos metadados de federação:  https&#58;//login.windows.net/{tenant-id} <tenant-id>/federationmetadata/2007-06/federationmetadata.xml?appid={<application-id} 

em que {tenant-id} é substituído pelo ID do seu inquilino, que se encontra no portal do Azure em Azure Active Directory -> Propriedades como o “ID do Diretório”, e em que {application-id} é substituído pelo ID da sua aplicação, que se encontra nas Propriedades da aplicação como “ID da Aplicação”

A tabela descreve com maior detalhe os principais elementos de configuração do IdP para configurar as definições de SSO na aplicação e os respetivos valores ou localizações no AD FS e no Azure AD.  A arquitetura de referência da tabela é a aplicação SaaS, que tem de saber para onde enviar os pedidos de autenticação e como validar os tokens recebidos.

|Elemento de configuração|Descrição|AD FS|Azure AD|
|---|---|---|---|
|Metadados </br>início de sessão </br>do IdP|URL de início de sessão do IdP na perspetiva da aplicação (para onde o utilizador é redirecionado para iniciar sessão).|O URL de início de sessão do AD FS é o nome do serviço de federação do AD FS seguido de “/adfs/ls/”, como, por exemplo, https&#58;//fs.contoso.com/adfs/ls/|O valor correspondente para o Azure AD segue o padrão, em que {tenant-id} é substituído pelo ID de inquilino, que se encontra no portal do Azure em Azure Active Directory -> Propriedades como o "ID do Diretório".</br></br>Para as aplicações que utilizam o protocolo SAML-P: https&#58;//login.microsoftonline.com</br>/{tenant-id}/saml2 </br></br>Para as aplicações que utilizam o protocolo WS-Federation https&#58;//login.microsoftonline.com</br>/{tenant-id}/wsfed|
|URL de </br>fim de sessão </br>do IdP|URL de fim de sessão do IdP na perspetiva da aplicação (para onde o utilizador é redirecionado quando escolhe “terminar sessão” na aplicação).|No AD FS, o URL de fim de sessão é igual ao URL de início de sessão ou é o mesmo URL com “wa=wsignout1.0” anexado, como, por exemplo, https&#58;//fs.contoso.com/adfs/ls /?wa=wsignout1.0|O valor correspondente para o Azure AD depende de a aplicação ter suporte para o fim de sessão SAML 2.0.</br></br>Se a aplicação suportar o fim de sessão SAML, o valor segue o padrão, em que o valor de {tenant-id} é substituído pelo ID do inquilino, que se encontra no portal do Azure em Azure Active Directory > Propriedades como o “ID do Diretório”. https&#58;//login.microsoftonline.com</br>/{tenant-id}/saml2</br></br>Se a aplicação não suportar o fim de sessão SAML: https&#58;//login.microsoftonline.com</br>/common /wsfederation?wa=wsignout1.0|
|Certificado de </br>Assinatura de </br>Certificado|O certificado cuja chave privada é utilizada pelo IdP para assinar tokens emitidos.  Verifica se o token provém do mesmo IdP para o qual a aplicação está configurada para confiar.|O certificado de assinatura de tokens do AD FS encontra-se em Gestão do AD FS, em Certificados.|No Azure AD, o certificado de assinatura de tokens pode ser encontrado no portal do Azure, nas Propriedades de Início de Sessão Único da aplicação, no cabeçalho Certificado de Assinatura SAML, onde pode transferir o certificado para carregamento para a aplicação.</br></br>  Se a aplicação tiver mais de um certificado, todos os certificados estão disponíveis no ficheiro xml dos metadados de federação.|
|Identificador / </br>“Emissor”|Identificador do IdP na perspetiva da aplicação (por vezes, denominado “Emissor” ou “ID do Emissor”)</br></br>No token SAML, o valor aparece como o elemento “Emissor”|Geralmente, o identificador no AD FS é o identificador do serviço de federação na Gestão do AD FS, em Serviço -> Editar Propriedades do Serviço de Federação.  Por exemplo: http&#58;//fs.contoso.com/adfs/services/trust|O valor correspondente para o Azure AD segue o padrão, em que o valor de {tenant-id} é substituído pelo ID de inquilino, que se encontra no portal do Azure em Azure Active Directory -> Propriedades como o "ID do Diretório".  https&#58;//sts.windows.net/{tenant-id}/|
|Metadados </br>de Federação </br>Metadados|Localização dos metadados de federação publicamente disponíveis do IdP.  (algumas aplicações utilizam os metadados de federação como alternativa à configuração individual, por parte do administrador, de URLs, do identificador e do certificado de assinatura de tokens)|O URL dos metadados de federação do AD FS encontram-se na Gestão do AD FS, em Serviço -> Pontos Finais -> Metadados -> Tipo: Metadados de Federação, como, por exemplo, https&#58;//fs.contoso.com/ FederationMetadata/2007-06/</br>FederationMetadata.xml|O valor correspondente do Azure AD segue o padrão https&#58;//login.microsoftonline.com</br>/{TenantDomainName}/FederationMetadata/2007-06/</br>FederationMetadata.xml, em que o valor de {TenantDomainName} é substituído pelo nome do seu inquilino no formato “contoso.onmicrosoft.com” </br></br>[Mais informações](https://docs.microsoft.com/azure/active-directory/develop/active-directory-federation-metadata) sobre os metadados de federação no Azure AD.

## <a name="migrating-saas-apps"></a>Migrar aplicações SaaS
Atualmente, a migração de aplicações SaaS do AD FS ou de outros fornecedores de identidade para o Azure AD é um processo manual. Para obter orientações para aplicações específicas, [veja a lista de tutoriais relativos à integração de aplicações SaaS disponíveis na Galeria](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

Os tutoriais de integração partem do princípio de que está a fazer uma integração “green field”.  À medida que planeia, avalia, configura e faz a transição das aplicações, existem alguns conceitos importantes específicos da migração que deverá conhecer.  
- Ao passo que algumas aplicações podem ser migradas facilmente, as aplicações que tenham requisitos mais complexos, como afirmações personalizadas, podem exigir configuração adicional no Azure AD e/ou no Azure AD Connect.
- Nos cenários mais frequentes, as aplicações só precisam da afirmação NameId e de outras afirmações de identificador de utilizador comuns; para determinar se são precisas mais afirmações, veja que afirmações está a emitir no AD FS ou no seu fornecedor de identidade de terceiros.
- Depois de determinar a afirmação ou afirmações adicionais necessárias, tem de confirmar que estão disponíveis no Azure AD.  Tem de verificar a configuração da sincronização do Azure AD Connect, para garantir que um atributo necessário, como, por exemplo, samAccountName, está a ser sincronizado com o Azure AD
- Quando os atributos estiverem disponíveis no Azure AD, tem de adicionar regras de emissão de afirmações ao Azure AD para incluir esses atributos como afirmações nos tokens emitidos.  Isto é feito nas Propriedades de Início de Sessão Único da aplicação no Azure AD.

### <a name="assessing-what-can-be-migrated"></a>Avaliar o que pode ser migrado
As aplicações SAML 2.0 podem ser integradas no Azure AD através da galeria de aplicações do Azure AD ou como aplicações externas à galeria.  

Existem algumas configurações que exigem passos adicionais para configurar no Azure AD e algumas que não são atualmente suportadas.  Para determinar o que pode ser movido, observe a configuração atual de cada uma das suas aplicações, mais concretamente o seguinte:
- Regras de afirmações configuradas (regras de transformação de emissão)
- Formato e atributo NameID de SAML
- Versões do token SAML emitidas
- Outras configurações, como regras de autorização de emissão ou políticas de controlo de acesso e regras de autenticação multifator (autenticação adicional)

#### <a name="what-can-be-migrated-today"></a>O que pode ser migrado atualmente
As aplicações que podem ser migradas facilmente hoje incluem aplicações SAML 2.0 que utilizam o conjunto padrão de elementos de configuração e afirmações.  Estas aplicações podem consistir em
- nome principal de utilizador
- endereço de e-mail
- Nome Próprio
- Apelido
- Atributo alternativo como NameID de SAML, incluindo o atributo de correio do Azure AD, o prefixo de correio, o employeeid, os atributos de extensão 1 – 15 ou SamAccountName no local (veja [Editing the NameIdentifier claim)](./develop/active-directory-saml-claims-customization.md) (Editar a afirmação NameIdentifier)
- Afirmações personalizadas (veja o documento [aqui](active-directory-claims-mapping.md) e [aqui](./develop/active-directory-saml-claims-customization.md) para obter informações sobre os mapeamentos de afirmações suportados)

Além das afirmações personalizadas e dos elementos nameID, as configurações que exigem passos adicionais no Azure AD como parte da migração são:
- Regras de autorização ou MFA personalizadas no AD FS (configuradas com a funcionalidade [acesso condicional do Azure AD](active-directory-conditional-access-azure-portal.md))
- As aplicações que tenham vários pontos finais SAML podem ser configuradas no Azure AD com o PowerShell (esta capacidade não está disponível no portal)
- As aplicações WS-Federation, como as aplicações do SharePoint, que precisam de tokens SAML com a versão 1.1 têm de ser configuradas manualmente através do PowerShell

#### <a name="appsconfigurations-not-supported-in-azure-ad-today"></a>Aplicações/configurações não suportadas atualmente no Azure AD
As aplicações que requerem as capacidades seguintes não podem ser migradas atualmente.  Se tiver aplicações que precisem destas funcionalidades, envie os seus comentários na secção de comentários.
- Capacidades do Protocolo
    - Suporte para Fim de Sessão Único (SLO) de SAML em todas as aplicações com sessão iniciada
    - Suporte para o padrão WS-Trust ActAs
    - Resolução de artefactos SAML 
    - Verificação de assinaturas de pedidos SAML assinados (tenha em conta que os pedidos assinados são aceites, mas a assinatura não é verificada)
 - Capacidades de Tokens 
     - Encriptação de Tokens SAML 
     - Tokens SAML com a versão 1.1 em respostas do protocolo SAML 
- Afirmações nas Capacidades de Tokens
    - Emitir nomes de grupos no local como afirmações
    - Afirmações de arquivos que não o do Azure AD
    - Regras de transformação de emissão de afirmações complexas (obtenha informações sobre os mapeamentos de afirmações suportados neste [documento](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) e neste [documento](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization))
    - Emitir extensões de diretório como afirmações
    - Especificação personalizada do formato NameID
    - Emissão de atributos de valores múltiplos

>[!NOTE]
>O Azure AD evolui continuamente para adicionar mais capacidades nesta área. Atualizamos este documento regularmente. 

### <a name="configuring-azure-ad"></a>Configurar o Azure AD    
#### <a name="configure-single-sign-on-sso-settings-for-the-saas-app"></a>Configurar as definições início de sessão único (SSO) para a aplicação SaaS

No Azure AD, a configuração do início de sessão de SAML conforme exigido pela sua aplicação é feita nas Propriedades de Início de Sessão Único, em Atributos do utilizador, conforme mostrado:

![](media/migrate-adfs-apps-to-azure/migrate3.png)
- Clique em “Ver e editar todos os outros atributos de utilizador” para ver os atributos que vão ser enviados no token de segurança

![](media/migrate-adfs-apps-to-azure/migrate4.png)
- Clique na linha de um atributo específico para editar ou clique em “Adicionar atributo” para criar um atributo novo. 
![](media/migrate-adfs-apps-to-azure/migrate5.png)

#### <a name="assign-users-to-the-app"></a>Atribuir utilizadores à aplicação
Para que os seus utilizadores no Azure AD possam iniciar sessão numa determinada aplicação SaaS, é necessário dar-lhes acesso a partir do mesmo.

Para atribuir utilizadores no portal do Azure AD, navegue para o ecrã da aplicação SaaS no portal e, em seguida, clique em "Utilizadores e grupos", na barra lateral. Para adicionar um utilizador ou um grupo, clique em "Adicionar utilizador", na parte superior do ecrã. 

![](media/migrate-adfs-apps-to-azure/migrate6.png) 

![](media/migrate-adfs-apps-to-azure/migrate7.png) Para verificar o acesso, o utilizador deverá ver essa aplicação SaaS no respetivo [painel de acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) quando inicia sessão, que está disponível em http://myapps.microsoft.com. Por exemplo, foi atribuído com êxito ao utilizador acesso ao Salesforce e ao ServiceNow.

![](media/migrate-adfs-apps-to-azure/migrate8.png)

### <a name="configuring-the-saas-app"></a>Configurar a aplicação SaaS
O processo de transição da federação no local para o Azure AD depende de a aplicação SaaS com que está a trabalhar suportar ou não vários Fornecedores de Identidade (IdPs).  Seguem-se algumas perguntas comuns relativamente ao suporte de vários IdPs:

   **P: O que significa uma aplicação suportar vários IdPs?**
    
   R: As aplicações SaaS que suportam vários IdPs permitem-lhe introduzir todas as informações do IdP novo (no nosso caso, o Azure AD) antes de consolidar a alteração da experiência de início de sessão.  Quando a configuração estiver concluída, pode mudar a configuração de autenticação da aplicação para apontar para o Azure AD.

   P: Por que motivo é relevante a aplicação SaaS suportar vários IdPs?

   R: Se não forem suportados vários IdPs, o administrador terá de reservar um curto período de tempo como falha de serviço ou manutenção para configurar o Azure AD como o IdP novo da aplicação. Durante este período de inatividade, os utilizadores devem ser notificados que não conseguirão iniciar sessão nas respetivas contas.

   Se uma aplicação suportar mais de um IdP, a configuração do IdP adicional pode ser feita com antecedência, de modo a que o administrador possa simplesmente mudar de IdP durante a transição para o Azure.

   Além disso, se a aplicação suportar vários IdPs e quiser ter vários IdPs a processar em simultâneo a autenticação de início de sessão, o utilizador pode escolher que IdP vai utilizar na autenticação na respetiva página de início de sessão.

#### <a name="example-multiple-idp-support"></a>Exemplo: suporte para vários IdP
Por exemplo, no Salesforce, a configuração do IdP está disponível em Settings (Definições) -> Company Settings (Definições da Empresa) -> My Domain (O Meu Domínio) -> Authentication Configuration (Configuração da Autenticação).

![](media/migrate-adfs-apps-to-azure/migrate9.png)

Devido à configuração que criámos anteriormente em Identidade -> Definições de início de sessão único, deverá conseguir alterar o IdP da configuração de autenticação, digamos, do AD FS para o Azure AD. 

![](media/migrate-adfs-apps-to-azure/migrate10.png)

### <a name="optional-configure-user-provisioning-in-azure-ad"></a>Opcional: configurar o aprovisionamento de utilizadores no Azure AD
Opcionalmente, se pretender que o Azure AD processe diretamente o aprovisionamento de utilizadores numa determinada aplicação SaaS, veja este documento sobre a gestão do aprovisionamento de contas de utilizadores para aplicações empresariais no Azure AD.

## <a name="next-steps"></a>Passos seguintes

- [Managing Applications with Azure Active Directory](active-directory-enable-sso-scenario.md) (Gerir Aplicações com o Azure Active Directory)
- [Gerir o acesso a aplicações](active-directory-managing-access-to-apps.md)
- [Azure AD Connect Federation](active-directory-aadconnectfed-whatis.md) (Federação do Azure AD Connect)