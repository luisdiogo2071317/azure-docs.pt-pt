---
title: Gerir o acesso a aplicações na cloud, restringindo os inquilinos - Azure | Documentos da Microsoft
description: Como utilizar as restrições de inquilino para gerir os utilizadores que podem aceder a aplicações com base no seu inquilino do Azure AD.
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
editor: yossib
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: barbkess
ms.reviewer: richagi
ms.openlocfilehash: 90cfb766e98bfa9706cd88342f8374269bc69d76
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2019
ms.locfileid: "54911630"
---
# <a name="use-tenant-restrictions-to-manage-access-to-saas-cloud-applications"></a>Restrições de inquilino de utilização para gerir o acesso ao SaaS na nuvem

Organizações de grandes porte que enfatizam a segurança pretendem mover para serviços como o Office 365 cloud, mas precisa de saber que os utilizadores só podem aceder a aprovados recursos. Tradicionalmente, empresas restringem os nomes de domínio ou endereços IP, quando desejam gerir o acesso. Essa abordagem falha num mundo onde as aplicações SaaS estão alojadas numa cloud pública, em execução em nomes de domínio compartilhado como outlook.office.com e login.microsoftonline.com. Bloquear estes endereços seria manter os utilizadores acedam ao Outlook na web totalmente, em vez de simplesmente restringi-los a identidades aprovadas e recursos.

Solução de Azure do Active Directory para esse desafio é um recurso chamado restrições de inquilino. Restrições de inquilino permite às organizações controlar o acesso a aplicações de cloud de SaaS, com base no inquilino do Azure AD, os aplicativos utilizam para início de sessão único. Por exemplo, pode querer permitir o acesso a aplicações do Office 365 da sua organização, impedindo o acesso a instâncias de outras organizações dessas mesmas aplicações.  

Restrições de inquilino proporciona às organizações a capacidade de especificar a lista de inquilinos que seus usuários têm permissão de acesso. O Azure AD, em seguida, só concede acesso a estes permitido inquilinos.

Este artigo se concentra nas restrições de inquilino do Office 365, mas o recurso deve funcionar com qualquer aplicação na cloud de SaaS que utiliza protocolos de autenticação moderna com o Azure AD para início de sessão único. Se utilizar aplicações com um diferente do Azure AD de inquilino do inquilino utilizado pelo Office 365 de SaaS, certifique-se de que todas as necessárias de inquilinos são permitidos. Para obter mais informações sobre as aplicações de cloud de SaaS, consulte a [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/).

## <a name="how-it-works"></a>Como funciona

A solução geral inclui os seguintes componentes: 

1. **O Azure AD** – se o `Restrict-Access-To-Tenants: <permitted tenant list>` está presentes, o Azure AD apenas problemas tokens de segurança para os inquilinos permitidos. 

2. **Infraestrutura de servidor de proxy no local** – um dispositivo de proxy capaz de inspeção de SSL, configurada para inserir o cabeçalho que contém a lista de permitidos inquilinos no tráfego destinado para o Azure AD. 

3. **Software de cliente** – para suportar restrições de inquilino, software de cliente tem de pedir tokens diretamente a partir do Azure AD, para que o tráfego pode ser intercetado pela infraestrutura do proxy. Restrições de inquilino é atualmente suportada pelo baseada no browser e aplicativos do Office 365 e pelos clientes do Office quando é utilizada a autenticação moderna (como o OAuth 2.0). 

4. **Autenticação moderna** – serviços em nuvem tem de utilizar autenticação moderna para utilizar as restrições de inquilino e bloquear o acesso a todos os inquilinos não permitido. Serviços do Office 365 cloud devem ser configurados para utilizar protocolos de autenticação moderna, por predefinição. Para obter as informações mais recentes sobre o suporte do Office 365 para autenticação moderna, leia [autenticação moderna do Office 365 atualizado](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/).

O diagrama seguinte ilustra o fluxo de tráfego de alto nível. Inspeção do SSL é necessário apenas no tráfego do Azure AD, não para os serviços em nuvem do Office 365. Essa distinção é importante porque o volume de tráfego para a autenticação para o Azure AD é normalmente muito menor do que o volume de tráfego para aplicações de SaaS, como o Exchange Online e SharePoint Online.

![Fluxo de tráfego de restrições de inquilino - diagrama](./media/tenant-restrictions/traffic-flow.png)

## <a name="set-up-tenant-restrictions"></a>Configurar restrições de inquilino

Existem dois passos para começar com restrições de inquilino. A primeira etapa é certificar-se de que os seus clientes podem ligar-se para os endereços corretos. O segundo consiste em configurar a sua infraestrutura de proxy.

### <a name="urls-and-ip-addresses"></a>URLs e endereços IP

Para utilizar as restrições de inquilino, os clientes tem de ser possível estabelecer ligação com os seguintes URLs de AD do Azure para autenticar: login.microsoftonline.com login.microsoft.com e login.windows.net. Além disso, para aceder ao Office 365, os clientes também devem ser capazes de ligar para os FQDNs/URLs e endereços IP definidos na [intervalos de endereços IP e URLs do Office 365](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2). 

### <a name="proxy-configuration-and-requirements"></a>Configuração de proxy e de requisitos

A seguinte configuração é necessária para ativar restrições de inquilino através de sua infraestrutura de proxy. Esta orientação é genérica, portanto, deve consultar a documentação do seu fornecedor de proxy para obter os passos de implementação específica.

#### <a name="prerequisites"></a>Pré-requisitos

- O proxy deve ser capaz de realizar a interceptação de SSL, inserção de cabeçalho HTTP e filtrar destinos utilizando FQDNs/URLs. 

- Os clientes têm de confiar a cadeia de certificados apresentada pelo proxy para comunicações SSL. Por exemplo, se forem utilizados certificados a partir de uma PKI interna, o interno emissora certificado certificado de autoridade tem de ser fidedigno.

- Esta funcionalidade está incluída nas subscrições do Office 365, mas se pretender utilizar as restrições de inquilino para controlar o acesso a outras aplicações SaaS, em seguida, licenças do Azure AD Premium 1 são necessárias.

#### <a name="configuration"></a>Configuração

Para cada pedido recebido login.microsoftonline.com, login.microsoft.com e login.windows.net, insira dois cabeçalhos HTTP: *Restringir-Access-para-inquilinos* e *restringir acesso-contextual*.

Os cabeçalhos devem incluir os seguintes elementos: 
- Para *restringir acesso para inquilinos*, um valor de \<permitido a lista de inquilino\>, que é uma lista separada por vírgulas de inquilinos para permitir que os usuários acessem. Qualquer domínio que está registado com um inquilino pode ser utilizado para identificar o inquilino nesta lista. Por exemplo, para permitir o acesso aos inquilinos Contoso e Fabrikam, o par nome/valor é semelhante a:  `Restrict-Access-To-Tenants: contoso.onmicrosoft.com,fabrikam.onmicrosoft.com` 
- Para *restringir acesso-contextual*, um valor de um ID de diretório único, declarando a qual o inquilino é definir as restrições de inquilino. Por exemplo, para declarar Contoso como o inquilino que definir a política de restrições de inquilino, o par nome/valor é semelhante a: `Restrict-Access-Context: 456ff232-35l2-5h23-b3b3-3236w0826f3d`  

> [!TIP]
> Pode encontrar o seu ID de diretório no [portal do Azure](https://portal.azure.com). Inicie sessão como administrador, selecione **do Azure Active Directory**, em seguida, selecione **propriedades**.

Para impedir que os usuários a inserir seu próprio cabeçalho HTTP com inquilinos não aprovado, o proxy tem de substituir o cabeçalho de restringir acesso para inquilinos se já estiver presente no pedido de entrada. 

Os clientes devem ser forçados a utilizar o proxy para todos os pedidos para login.microsoftonline.com login.microsoft.com e login.windows.net. Por exemplo, se os ficheiros PAC são utilizados para direcionar clientes para utilizar o proxy, os utilizadores finais devem não ser capazes de editar ou desativar os ficheiros PAC.

## <a name="the-user-experience"></a>A experiência do usuário

Esta secção mostra a experiência para os utilizadores finais e os administradores.

### <a name="end-user-experience"></a>Experiência do utilizador final

Um utilizador de exemplo é na rede da Contoso, mas está a tentar aceder à instância da Fabrikam de SaaS aplicação partilhada, como o Outlook online. Se Fabricam é um inquilino não é permitido para a instância de Contoso, este verá a seguinte página:

![Acesso negado a página para os utilizadores nos inquilinos não permitido](./media/tenant-restrictions/end-user-denied.png)

### <a name="admin-experience"></a>Experiência de administrador

Enquanto a configuração de restrições de inquilino é feita na infraestrutura de proxy empresarial, os administradores podem acessar os relatórios de restrições de inquilino no portal do Azure diretamente. Para ver os relatórios, vá para a página de descrição geral de diretório Active Directory do Azure, em seguida, procure em 'Outras capacidades'.

ID de administrador para o inquilino especificado como o inquilino do contexto de acesso de restrito pode utilizar este relatório para ver os inícios de sessão bloqueados devido à política de restrições de inquilino, incluindo a identidade utilizada e o diretório de destino. Inícios de sessão estão incluídos, se a definição de restrição de inquilino é o inquilino do utilizador ou um inquilino de recursos para o início de sessão.

![Utilizar o portal do Azure para ver a tentativas de início de sessão restritas](./media/tenant-restrictions/portal-report.png)

Como outros relatórios no portal do Azure, pode utilizar filtros para especificar o âmbito do seu relatório. Pode filtrar um utilizador específico, o aplicativo, o cliente ou o intervalo de tempo.

## <a name="office-365-support"></a>Suporte do Office 365

Aplicativos do Office 365 tem de cumprir dois critérios para suportar totalmente as restrições de inquilino:

1. O cliente utilizado suporta a autenticação moderna
2. Autenticação moderna está ativada como o protocolo de autenticação predefinido para o serviço cloud.

Consulte a [autenticação moderna do Office 365 atualizado](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/) para as informações mais recentes no Office que os clientes suportam atualmente autenticação moderna. Essa página também inclui ligações para instruções para habilitar a autenticação moderna no específicos Exchange Online e Skype para empresas Online inquilinos. Autenticação moderna já está ativada por predefinição no SharePoint Online.

Restrições de inquilino é, atualmente, suportado por aplicações baseadas no browser do Office 365 (o SharePoint do Portal do Office, Yammer, sites, Outlook para a Web, etc.). Para clientes espessos (Outlook, Skype para empresas, Word, Excel, PowerPoint, etc.) Restrições de inquilino só podem ser impostas quando é utilizada a autenticação moderna.  

Outlook e o Skype para clientes de empresas que oferecem suporte a autenticação moderna podem ainda podem utilizar protocolos herdados em inquilinos em que a autenticação moderna não está ativada, ignorando efetivamente restrições de inquilino. Aplicativos que usam protocolos legados podem estar bloqueados por restrições de inquilino se eles contactar login.microsoftonline.com, login.microsoft.com ou login.windows.net durante a autenticação.

Para o Outlook no Windows, os clientes podem optar por implementar restrições de impedir que os utilizadores finais adicionando contas de correio não aprovados a seus perfis. Por exemplo, consulte a [impedir a adição de contas do Exchange não predefinidas](https://gpsearch.azurewebsites.net/default.aspx?ref=1) definição de política de grupo.

## <a name="testing"></a>Testes

Se quiser experimentar as restrições de inquilino antes de a implementar para toda a organização, existem duas opções: uma abordagem baseada em host usando uma ferramenta como o Fiddler ou uma implementação faseada de configurações de proxy.

### <a name="fiddler-for-a-host-based-approach"></a>Fiddler para uma abordagem baseada no anfitrião

Fiddler é um proxy que pode ser usado para capturar e modificar o tráfego HTTP/HTTPS, incluindo a inserção de cabeçalhos HTTP de depuração de web gratuito. Para configurar o Fiddler para testar as restrições de inquilino, execute os seguintes passos:

1.  [Transferir e instalar o Fiddler](https://www.telerik.com/fiddler).
2.  Configurar o Fiddler para desencriptar o tráfego HTTPS, por [documentação de ajuda do Fiddler](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).
3.  Configurar o Fiddler para inserir o *restringir acesso para inquilinos* e *contexto de restringir acesso* cabeçalhos usando regras personalizadas:
    1. Na ferramenta de depurador da Web Fiddler, selecione o **regras** menu e selecione **personalizar regras...** Para abrir o ficheiro de CustomRules.
    2. Adicione as seguintes linhas no início da *OnBeforeRequest* função. Substitua \<domínio de inquilino\> com um domínio registar-se com o seu inquilino, por exemplo, contoso.onmicrosoft.com. Substitua \<ID de diretório\> com o identificador de GUID do Azure AD do seu inquilino.

    ```
    if (oSession.HostnameIs("login.microsoftonline.com") || oSession.HostnameIs("login.microsoft.com") || oSession.HostnameIs("login.windows.net")){      oSession.oRequest["Restrict-Access-To-Tenants"] = "<tenant domain>";      oSession.oRequest["Restrict-Access-Context"] = "<directory ID>";}
    ```

    Se tiver de permitir que vários inquilinos, utilize uma vírgula para separar os nomes de inquilino. Por exemplo:

    ```
    oSession.oRequest["Restrict-Access-To-Tenants"] = "contoso.onmicrosoft.com,fabrikam.onmicrosoft.com";
    ```

4. Guarde e feche o ficheiro de CustomRules.

Depois de configurar o Fiddler, pode capturar o tráfego ao aceder a **arquivo** menu e selecionando **capturar tráfego**.

### <a name="staged-rollout-of-proxy-settings"></a>Distribuição em etapas das definições de proxy

Consoante as capacidades da sua infraestrutura de proxy, poderá testar a implementação das definições para os seus utilizadores. Aqui estão algumas opções de alto nível para consideração:

1.  Utilize ficheiros PAC para apontar os utilizadores de teste para uma infraestrutura de proxy de teste, embora usuários normais continuam a utilizar a infraestrutura do proxy de produção.
2.  Alguns servidores de proxy deve dar suporte a utilização de grupos de configurações diferentes.

Consulte a documentação do servidor proxy para obter detalhes específicos.

## <a name="next-steps"></a>Passos Seguintes

- Leia sobre [autenticação moderna do Office 365 atualizado](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/)

- Reveja o [intervalos de endereços IP e URLs do Office 365](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
