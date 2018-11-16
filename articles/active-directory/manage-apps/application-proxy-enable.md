---
title: Começar a utilizar o Proxy de aplicações do Azure AD - instalar o conector | Documentos da Microsoft
description: Ativar o Proxy de aplicações no portal do Azure e instalar os conectores para o proxy inverso.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 11/14/2018
ms.author: barbkess
ms.reviewer: japere
ms.custom: it-pro
ms.openlocfilehash: 9a869055613da6465a9beda9b8edc1bf812b6dfe
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51712114"
---
# <a name="get-started-with-application-proxy-and-install-the-connector"></a>Introdução ao Proxy da aplicação e instalar o conector
Este artigo orienta-o pelos passos para ativar o Proxy da aplicação no Azure Active Directory (Azure AD).

Se não tiver ainda em consideração os benefícios de segurança e produtividade do Proxy de aplicações traz para a sua organização, saiba mais sobre [como fornecer acesso remoto seguro a aplicações no local](application-proxy.md).

## <a name="prerequisites"></a>Pré-requisitos
Para ativar o Proxy de aplicações, terá de:

* R [subscrição do Microsoft Azure AD básica ou premium](https://azure.microsoft.com/pricing/details/active-directory). 
* Uma conta de administrador do aplicativo.

### <a name="windows-server"></a>Servidor do Windows
É necessário um servidor com o Windows Server 2012 R2 ou posterior, no qual pode instalar o conector de Proxy de aplicações. O servidor tem de ligar a serviços de Proxy de aplicações no Azure e as aplicações no local que está a publicar.

O windows server tem de ter o TLS 1.2, ativado antes de instalar o conector do Proxy de aplicações. Os conectores existentes com versões anteriores ao 1.5.612.0 irão continuar a trabalhar em versões anteriores do TLS até indicações em contrário. Para ativar o TLS 1.2:

1. Defina as seguintes chaves de registo:
    
    ```
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319] "SchUseStrongCrypto"=dword:00000001
    ```

2. Reinicie o servidor

Para o início de sessão único para aplicativos que usam o Kerberos Contrained Delegation (KCD), o servidor do Windows e as aplicações que está a publicar devem estar no mesmo domínio do Active Directory. Para obter mais informações, consulte [KCD para início de sessão único com o Proxy de aplicações](application-proxy-configure-single-sign-on-with-kcd.md).
  
### <a name="proxy-servers"></a>Servidores de proxy

Se a sua organização utilizar servidores proxy para estabelecer ligação à internet, terá de configurá-los para o Proxy de aplicações.  Para obter mais informações, consulte [funcionam com o existente no local servidores proxy](application-proxy-configure-connectors-with-proxy-servers.md). 



## <a name="open-your-ports"></a>Abrir as portas

Para preparar o ambiente para o Proxy de aplicações do Azure AD, primeiro tem de ativar a comunicação a centros de dados do Azure. Se estiver colocada uma firewall no caminho, certifique-se de que está aberta, para que o Conector possa fazer pedidos HTTPS (TCP) para a Proxy da Aplicação.

1. Abrir as seguintes portas **saída** tráfego:

   | Número da porta | Como são utilizadas |
   | --- | --- |
   | 80 | Baixar a revogação de certificados apresenta uma lista (CRL) ao validar o certificado SSL |
   | 443 | Todas as comunicações de saída com o serviço de Proxy de aplicações |

   Se a firewall impuser tráfego de acordo com os utilizadores de origem, abra estas portas para o tráfego dos serviços do Windows que são executados como um serviço de rede.

   > [!IMPORTANT]
   > A tabela reflete os requisitos de porta para versões de conector 1.5.132.0 e mais recentes. Se ainda terá uma versão mais antiga do conector, também tem de ativar as seguintes portas para além de 80 e 443: 5671, 8080, 9090-9091, 9350, 9352, 10100 – 10120.
   >
   >Para obter informações sobre a atualização de seus conectores para a versão mais recente, consulte [conectores de Proxy de aplicações do AD Azure compreender](application-proxy-connectors.md#automatic-updates).

2. Se a sua firewall ou proxy permite que listas de permissões de DNS, pode-se ligações de lista de permissões para msappproxy.net e servicebus.windows.net. Se não, precisa permitir o acesso para o [intervalos de IP de DataCenter do Azure](https://www.microsoft.com/download/details.aspx?id=41653), que é atualizado a cada semana.

3. A Microsoft utiliza os endereços de quatro para verificar os certificados. Permitir o acesso aos seguintes URLs, se ainda não o fez para outros produtos:
   * mscrl.microsoft.com:80
   * crl.microsoft.com:80
   * ocsp.msocsp.com:80
   * www.microsoft.com:80

4. O conector precisa de aceder a login.windows.net e login.microsoftonline.com para o processo de registo.


## <a name="install-and-register-a-connector"></a>Instalar e registar um conector
1. Inicie sessão como administrador no [portal do Azure](https://portal.azure.com/).
2. O diretório atual é apresentado em seu nome de utilizador no canto superior direito. Se precisar de mude de diretório, selecione esse ícone.
3. Aceda a **do Azure Active Directory** > **Proxy de aplicações**.

   ![Navegue para o Proxy de aplicações](./media/application-proxy-enable/app_proxy_navigate.png)

4. Selecione **transferir conector**.

   ![Transferir Conector](./media/application-proxy-enable/download_connector.png)

5. Execute **AADApplicationProxyConnectorInstaller.exe** no servidor que preparou de acordo com os pré-requisitos.
6. Siga as instruções do assistente de instalação. Durante a instalação, é solicitado que registe o conector com o Proxy de aplicações de inquilino do Azure AD.

   * Indique as suas credenciais de administrador global do Azure AD. O inquilino do administrador global pode ser diferente das suas credenciais do Microsoft Azure.
   * Certifique-se o administrador que regista o conetor está no mesmo diretório onde ativou o serviço da Proxy da Aplicação. Por exemplo, se o domínio de inquilino for contoso.com, o administrador deve ser admin@contoso.com ou qualquer outro alias nesse domínio.
   * Se **configuração de segurança avançada do IE** está definida como **no** no servidor onde está a instalar o conector, poderá não ver o ecrã de registo. Para obter acesso, siga as instruções na mensagem de erro. Certifique-se de que a Segurança Avançada do Internet Explorer está desativada.

Para fins de elevada disponibilidade, deve implementar pelo menos dois conetores. Cada conetor tem de ser registado separadamente.

## <a name="test-that-the-connector-installed-correctly"></a>Teste o conector foi instalado corretamente

Pode confirmar que um novo conector instalado corretamente ao verificar para o mesmo no portal do Azure ou no seu servidor. 

No portal do Azure, inicie sessão no seu inquilino e navegue para **do Azure Active Directory** > **Proxy de aplicações**. Todos os seus conectores e os grupos de conexão são apresentados nesta página. Selecione um conector para ver os respetivos detalhes ou movê-la para um grupo de conectores diferentes. 

No seu servidor, verifique a lista de serviços do Active Directory para o conector e o atualizador do conector. Os dois serviços devem começar a executar imediatamente, mas se não estiver, ativá-los: 

   * O **Conector do Proxy da Aplicação do Microsoft AAD** ativa a conectividade

   * **Atualizador de conector do Proxy de aplicação do Microsoft AAD** é um serviço de atualização automática. O atualizador verifica a existência de novas versões do conector e atualiza o conector conforme necessário.

   ![Serviços do Conector do Proxy da Aplicação – captura de ecrã](./media/application-proxy-enable/app_proxy_services.png)

Para obter informações sobre conectores e como eles se manter atualizados, consulte [conectores de Proxy de aplicações do AD Azure compreender](application-proxy-connectors.md).


## <a name="next-steps"></a>Passos Seguintes
Está agora pronto para [Publicar aplicações com o Proxy da Aplicação](application-proxy-publish-azure-portal.md).

Se tiver aplicações que estão em redes separadas ou em diferentes localizações, utilize grupos de conetor para organizar os diferentes conetores em unidades lógicas. Saiba mais sobre [Trabalhar com conetores da Proxy da Aplicação](application-proxy-connector-groups.md).
