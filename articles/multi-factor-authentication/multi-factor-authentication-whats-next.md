---
title: Configurar o multi-factor Authentication do Azure | Microsoft Docs
description: "Este artigo descreve como configurar definições do multi-factor Authentication do Azure para relatórios, alertas de fraude, omissões de uso individual, mensagens de voz personalizadas, a colocação em cache, IPs fidedignos e palavras-passe de aplicação."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: 75af734e-4b12-40de-aba4-b68d91064ae8
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.author: joflore
ms.reviewer: richagi
ms.openlocfilehash: 4421b995e69e115fbb6c7379af79aaef537aed0d
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2018
---
# <a name="configure-azure-multi-factor-authentication-settings"></a>Configurar definições de multi-factor Authentication do Azure

Este artigo ajuda-o a gerir o Azure multi-factor Authentication agora que está em execução. Abrange vários tópicos que ajudam a tirar o máximo partido do Azure multi-factor Authentication. Todas as funcionalidades não estão disponíveis em cada [versão do Azure multi-factor Authentication](/multi-factor-authentication-get-started.md#what-features-do-i-need).

| Funcionalidade | Descrição | 
|:--- |:--- |
| [Bloquear e desbloquear utilizadores](#block-and-unblock-users) |Utilize a funcionalidade de utilizadores bloquear/desbloquear para impedir que os utilizadores receber pedidos de autenticação. |
| [Alerta de fraude](#fraud-alert) |Configure a funcionalidade de alertas de fraude, para que os utilizadores podem comunicar fraudulenta tenta aceder aos respetivos recursos. |
| [Omissão de uso individual](#one-time-bypass) |Utilizar a funcionalidade de omissão de uso individual para permitir que os utilizadores autentiquem uma vez por _ignorando_ multi-factor Authentication. |
| [Mensagens de voz personalizadas](#custom-voice-messages) |Utilize a funcionalidade de mensagens de voz personalizadas para utilizar os seus próprios gravações ou saudações com multi-factor Authentication. |
| [A colocação em cache](#caching-in-azure-multi-factor-authentication) |Utilize a funcionalidade de colocação em cache para definir um período de tempo específico, para que as tentativas de autenticação subsequentes êxito automaticamente. |
| [IPs fidedignos](#trusted-ips) |Os administradores de um inquilino federado ou gerido podem utilizar a funcionalidade de IPs fidedignos para ignorar a verificação de dois passos para os utilizadores que iniciar sessão a partir da intranet da empresa. |
| [Palavras-passe de aplicação](#app-passwords) |Utilize a funcionalidade de palavra-passe de aplicação para ativar uma aplicação ignorar o multi-factor Authentication e continuar a trabalhar. |
| [Lembre-se de multi-factor Authentication para dispositivos fidedignos e browsers](#remember-multi-factor-authentication-for-trusted-devices) |Utilize esta funcionalidade para memorizar dispositivos fidedignos e browsers para um número definido de dias após um utilizador tiver com êxito com sessão iniciada utilizando o multi-factor Authentication. |
| [Métodos de verificação selecionável](#selectable-verification-methods) |Utilize esta funcionalidade para selecionar a lista de métodos de autenticação que os utilizadores têm capacidade para utilizar. |

## <a name="block-and-unblock-users"></a>Bloquear e desbloquear utilizadores

Utilize o _bloquear e desbloquear utilizadores_ funcionalidade para impedir que os utilizadores receber pedidos de autenticação. Quaisquer tentativas de autenticação para utilizadores bloqueados são rejeitadas automaticamente. Os utilizadores permanecerem bloqueados 90 dias desde o momento em que este é bloqueado.

### <a name="block-a-user"></a>Bloquear um utilizador

1. Inicie sessão no [portal do Azure](https://portal.azure.com) como administrador.
2. Navegue até à **do Azure Active Directory** > **servidor MFA** > **bloquear/desbloquear utilizadores**.
3. Selecione **adicionar** para bloquear um utilizador.
4. Selecione o **grupo de replicação**. Introduza o nome de utilizador para o utilizador bloqueado **username<span></span>@domain.com**. Introduza um comentário no **razão** campo.
5. Selecione **adicionar** para concluir a bloquear o utilizador.

### <a name="unblock-a-user"></a>Desbloquear um utilizador

1. Inicie sessão no [portal do Azure](https://portal.azure.com) como administrador.
2. Navegue até à **do Azure Active Directory** > **servidor MFA** > **bloquear/desbloquear utilizadores**.
3. Selecione **desbloqueio** no **ação** coluna junto ao utilizador a desbloquear.
4. Introduza um comentário no **pelo motivo para desbloquear** campo.
5. Selecione **desbloqueio** para concluir a desbloquear o utilizador.

## <a name="fraud-alert"></a>Alerta de fraudes

Configurar o _alerta de fraude_ funcionalidade para que os utilizadores podem comunicar fraudulenta tenta aceder aos respetivos recursos. Os utilizadores podem comunicar tentativas de fraude utilizando a aplicação móvel ou através do seu telefone.

### <a name="turn-on-fraud-alerts"></a>Ativar alertas de fraude

1. Inicie sessão no [portal do Azure](https://portal.azure.com) como administrador.
2. Navegue até à **do Azure Active Directory** > **servidor MFA** > **alerta de fraude**.

   ![Ativar alertas de fraude](./media/multi-factor-authentication-whats-next/fraudalert.png)

3. Definir o **permitir que os utilizadores enviem alertas de fraude** definição **no**.
4. Selecione **Guardar**.

### <a name="configuration-options"></a>Opções de configuração

- **Bloquear utilizador quando é reportada fraude**: se um utilizador reportar fraude, a conta está bloqueada para 90 dias, ou até que um administrador unblocks a respetiva conta. Um administrador pode rever os inícios de sessão utilizando o relatório de início de sessão e tome as medidas necessárias para evitar fraude futura. Um administrador pode, em seguida, [desbloquear](#unblock-a-user) a conta de utilizador.
- **Código para reportar fraude durante a saudação inicial**: quando os utilizadores recebem uma chamada telefónica para efetuar a verificação de dois passos, normalmente prima  **#**  para confirmar o seu início de sessão. Para reportar fraude, o utilizador introduzir um código antes prima  **#** . Este código é **0** por predefinição, mas pode personalizá-lo.

  >[!NOTE]
  >Saudações de voz predefinido da Microsoft instruir os utilizadores que prima **0#** para submeter um alerta de fraude. Se pretender utilizar um código diferente de **0**, registar e carregar as suas próprias saudações de voz personalizadas com instruções adequadas para os seus utilizadores.
  >

### <a name="view-fraud-reports"></a>Ver relatórios de fraude

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **do Azure Active Directory** > **inícios de sessão**. O relatório de fraude faz agora parte do relatório padrão do Azure AD inícios de sessão.

## <a name="one-time-bypass"></a>Omissão de uso individual

O _omissão de uso individual_ funcionalidade permite ao utilizador autenticar-se de uma única vez sem efetuar verificação de dois passos. A omissão é temporária e expira após um número de segundos especificado. Em situações onde a aplicação móvel ou por telefone não está a receber uma notificação ou uma chamada telefónica, pode permitir que uma omissão de uso individual para que o utilizador possa aceder o recursos desejados.

### <a name="create-a-one-time-bypass"></a>Criar uma omissão de uso individual

1. Inicie sessão no [portal do Azure](https://portal.azure.com) como administrador.
2. Navegue até à **do Azure Active Directory** > **servidor MFA** > **omissão de uso individual**.

   ![Criar a omissão de uso individual](./media/multi-factor-authentication-whats-next/onetimebypass.png)

3. Selecione **Adicionar**.
4. Se necessário, selecione o grupo de replicação para a omissão.
5. Introduza o nome de utilizador como **username<span></span>@domain.com**. Introduza o número de segundos que a omissão deve última. Introduza a razão para a omissão. 
6. Selecione **Adicionar**. O limite de tempo entra em vigor imediatamente. O utilizador tem de iniciar sessão antes de expira a omissão de uso individual. 

### <a name="view-the-one-time-bypass-report"></a>Ver o relatório de omissão de uso individual

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Navegue até à **do Active Directory** > **servidor MFA** > **omissão de uso individual**.

## <a name="custom-voice-messages"></a>Mensagens de voz personalizadas
Pode utilizar os seus próprios gravações ou saudações para verificação de dois passos com o _mensagens de voz personalizadas_ funcionalidade. Estas mensagens podem ser utilizadas na adição ou substituir as gravações de Microsoft.

Antes de começar, tenha em atenção as seguintes restrições:

* Os formatos de ficheiro suportados são wav e. mp3.
* O limite de tamanho de ficheiro é 5 MB.
* Mensagens de autenticação devem ser inferiores a 20 segundos. Mensagens que são mais do que 20 segundos pode fazer com que a verificação falhar. O utilizador não poderá responder antes de termina a mensagem e a verificação de tempo excedido.

### <a name="set-up-a-custom-message"></a>Configurar uma mensagem personalizada

1. Inicie sessão no [portal do Azure](https://portal.azure.com) como administrador.
2. Navegue até à **do Azure Active Directory** > **servidor MFA** > **definições de chamada telefónica**.

   ![Mensagens de telefone personalizado do registo](./media/multi-factor-authentication-whats-next/phonecallsettings.png)

3. Selecione **adicionar saudação**.
4. Escolha o tipo de saudação. Escolha o idioma.
5. Selecione um ficheiro de som. mp3 ou WAV para carregar.
6. Selecione **Adicionar**.

## <a name="caching-in-azure-multi-factor-authentication"></a>A colocação em cache no multi-factor Authentication do Azure

Pode definir um período de tempo para permitir que as tentativas de autenticação, depois de um utilizador é autenticado utilizando o _colocação em cache_ funcionalidade. Tentativas de autenticação subsequentes para o utilizador especificado no período de tempo autenticar-se automaticamente. A colocação em cache é principalmente utilizada quando sistemas no local, tais como VPN, enviam vários pedidos de verificação, enquanto o primeiro pedido ainda está em curso. Esta funcionalidade permite que os pedidos subsequentes seja bem sucedida automaticamente, após o utilizador é concluída com êxito a primeira verificação em curso. 

>[!NOTE]
>A funcionalidade de colocação em cache não se destina a ser utilizado para inícios de sessão ao Azure Active Directory (Azure AD).

### <a name="set-up-caching"></a>Configurar a colocação em cache 

1. Inicie sessão no [portal do Azure](https://portal.azure.com) como administrador.
2. Navegue até à **do Azure Active Directory** > **servidor MFA** > **regras a colocação em cache**.

   ![Configurar regras de colocação em cache](./media/multi-factor-authentication-whats-next/cachingrules.png)

3. Selecione **Adicionar**.
4. Selecione o **tipo de cache** na lista pendente. Introduza o número máximo de **segundos de cache**. 
5. Se necessário, selecione um tipo de autenticação e especifique uma aplicação. 
6. Selecione **Adicionar**.


## <a name="trusted-ips"></a>IPs Fidedignos

O _IPs fidedignos_ funcionalidade do Azure multi-factor Authentication é utilizada por administradores de um inquilino federado ou não geridos. A funcionalidade ignora a verificação de dois passos para os utilizadores que iniciar sessão a partir da intranet da empresa. A funcionalidade está disponível com a versão completa do multi-factor Authentication do Azure e não a versão gratuita para administradores. Para obter mais informações sobre como obter a versão completa do multi-factor Authentication do Azure, consulte [Azure multi-factor Authentication](multi-factor-authentication.md).

| Tipo de inquilino do Azure AD | Opções de funcionalidades de IPs fidedignas |
|:--- |:--- |
| Gerido |**Específico intervalo de endereços IP**: os administradores especificar um intervalo de endereços IP que pode ignorar a verificação de dois passos para os utilizadores que iniciar sessão a partir da intranet da empresa.|
| Federado |**Todos os utilizadores federados**: todos os utilizadores federados que iniciar sessão a partir de dentro da organização podem ignorar a verificação de dois passos. Os utilizadores ignorar a verificação através da utilização de uma afirmação que é emitida por serviços de Federação do Active Directory (AD FS).<br/>**Específico intervalo de endereços IP**: os administradores especificar um intervalo de endereços IP que pode ignorar a verificação de dois passos para os utilizadores que iniciar sessão a partir da intranet da empresa. |

Os IPs fidedignos ignorar funciona apenas a partir de dentro da intranet da empresa. Se selecionar a **todos os utilizadores federados** opção e um utilizador inicia sessão de fora da intranet da empresa, o utilizador tem de autenticar através da verificação de dois passos. O processo é o mesmo, mesmo que o utilizador apresente uma afirmação de AD FS. 

**Experiência do utilizador final no interior da rede empresarial**

Quando a funcionalidade de IPs fidedignos está desativada, a verificação é necessária para fluxos de browser. As palavras-passe de aplicação são necessárias para aplicações de cliente avançado mais antigas. 

Quando a funcionalidade de IPs fidedignos está ativada, a verificação de dois passos é *não* necessários para fluxos de browser. Palavras-passe de aplicação são *não* necessários para aplicações de cliente avançado mais antigas, desde que o utilizador ainda não criou uma palavra-passe de aplicação. Depois de uma palavra-passe de aplicação está a ser utilizado, a palavra-passe permanece necessária. 

**Experiência de utilizador final fora da rede empresarial**

Independentemente se estiver ativada a funcionalidade de IPs fidedignos, verificação de dois passos é necessária para fluxos de browser. As palavras-passe de aplicação são necessárias para aplicações de cliente avançado mais antigas. 

### <a name="enable-named-locations-by-using-conditional-access"></a>Ativar localizações com nome utilizando o acesso condicional

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No lado esquerdo, selecione **do Azure Active Directory** > **acesso condicional** > **denominado localizações**.
3. Selecione **nova localização**.
4. Introduza um nome para a localização.
5. Selecione **marcar como localização fidedigna**.
6. Introduza o intervalo de IP na notação CIDR como **192.168.1.1/24**.
7. Selecione **Criar**.

### <a name="enable-the-trusted-ips-feature-by-using-conditional-access"></a>Ativar a funcionalidade de IPs fidedignos ao utilizar o acesso condicional

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No lado esquerdo, selecione **do Azure Active Directory** > **acesso condicional** > **denominado localizações**.
3. Selecione **MFA configurar IPs fidedignos**.
4. No **definições do serviço** página **IPs fidedignos**, escolha entre as duas opções seguintes:
   
   * **Para pedidos de utilizadores federados com origem na minha intranet**: escolher esta opção, selecione a caixa de verificação. Todos os utilizadores federados que o início de sessão da rede empresarial ignorar a verificação em dois passos, utilizando uma afirmação que é emitida pelo AD FS. Certifique-se de que o AD FS tem uma regra para adicionar a afirmação de intranet para o tráfego adequado. Se a regra não existir, crie a seguinte regra no AD FS:<br/>

     ```
     c:[Type== "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"] => issue(claim = c);
     ```
     
   * **Para pedidos de um intervalo específico de IPs públicos**: escolher esta opção, introduza os endereços IP na caixa de texto, utilizando a notação CIDR.
   
     * Para os endereços IP que se encontrem no xxx.xxx.xxx.1 intervalo através de xxx.xxx.xxx.254, utilizam a notação como **xxx.xxx.xxx.0/24**.
     * Para um único endereço IP, utilize a notação como **xxx<span></span>.xxx.xxx.xxx/32**.
     
     Introduza até 50 intervalos de endereços IP. Os utilizadores que iniciar sessão a partir destes endereços IP ignorar a verificação de dois passos.

5. Selecione **Guardar**.

### <a name="enable-the-trusted-ips-feature-by-using-service-settings"></a>Ativar a funcionalidade de IPs fidedignos, utilizando as definições de serviço

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No lado esquerdo, selecione **do Azure Active Directory** > **utilizadores e grupos** > **todos os utilizadores**.
3. Selecione **multi-factor Authentication**.
4. Em multi-factor Authentication, selecione **definições do serviço**.
5. No **definições do serviço** página **IPs fidedignos**, escolha entre as duas opções seguintes:
   
   * **Para pedidos de utilizadores federados na minha intranet**: escolher esta opção, selecione a caixa de verificação. Todos os utilizadores federados que o início de sessão da rede empresarial ignorar a verificação em dois passos, utilizando uma afirmação que é emitida pelo AD FS. Certifique-se de que o AD FS tem uma regra para adicionar a afirmação de intranet para o tráfego adequado. Se a regra não existir, crie a seguinte regra no AD FS:<br/>

     ```
     c:[Type== "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"] => issue(claim = c);
     ```

   * **Para pedidos de um sub-redes de endereços do intervalo de IP especificada**: escolher esta opção, introduza os endereços IP na caixa de texto, utilizando a notação CIDR. 
     
     * Para os endereços IP que se encontrem no xxx.xxx.xxx.1 intervalo através de xxx.xxx.xxx.254, utilizam a notação como **xxx.xxx.xxx.0/24**.
     * Para um único endereço IP, utilize a notação como **xxx<span></span>.xxx.xxx.xxx/32**.
     
     Introduza até 50 intervalos de endereços IP. Os utilizadores que iniciar sessão a partir destes endereços IP ignorar a verificação de dois passos.

6. Selecione **Guardar**.

![Ativar IPs fidedignos com definições de serviço](./media/multi-factor-authentication-whats-next/trustedips3.png)

## <a name="app-passwords"></a>Palavras-passe de aplicações

Algumas aplicações, como o Office 2010 ou anterior e Apple Mail, não suportam a verificação de dois passos. As aplicações não estão configuradas para aceitar uma verificação de segundo. Para utilizar estas aplicações, tirar partido do _palavras-passe de aplicação_ funcionalidade. Pode utilizar uma palavra-passe de aplicação em vez da palavra-passe tradicional para permitir que uma aplicação ignorar a verificação de dois passos e continuar a trabalhar.

>[!NOTE]
>Autenticação moderna para os clientes do Microsoft Office 2013 e posterior
> 
>Os clientes do Office 2013 e posterior (incluindo o Outlook), suporta os protocolos de autenticação moderna e podem ser ativados para funcionarem com verificação de dois passos. Depois do cliente estiver ativado, as palavras-passe de aplicação não são necessárias para o cliente. Para obter mais informações, consulte o [anúncio de pré-visualização pública de autenticação moderna do Office 2013](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).
>

### <a name="considerations-about-app-passwords"></a>Considerações sobre palavras-passe de aplicação

Quando utilizar palavras-passe de aplicação, considere os seguintes pontos importantes:

* As palavras-passe de aplicação só são introduzidas uma vez por aplicação. Os utilizadores não têm de manter um registo das palavras-passe ou introduzi-las sempre.
* A palavra-passe real é gerada automaticamente e não é fornecida pelo utilizador. A palavra-passe gerada automaticamente é mais difícil um atacante adivinhar e é mais segura.
* Não há um limite de 40 palavras-passe por utilizador. 
* As aplicações que as palavras-passe em cache e utilizá-los em cenários no local podem começar a falhar porque a palavra-passe de aplicação não é conhecida fora a conta escolar ou profissional. Um exemplo deste cenário é mensagens de correio eletrónico Exchange que estão no local, mas o correio é arquivado na nuvem. Neste cenário, a mesma palavra-passe não funciona.
* Depois de multi-factor Authentication está ativada na conta de utilizador, palavras-passe de aplicação podem ser utilizadas com clientes mais não baseadas no browser como o Outlook e o Microsoft Skype para empresas. Não não possível efetuar ações administrativas através da utilização de palavras-passe de aplicação através de aplicações não baseadas no browser, tais como o Windows PowerShell. Não não possível efetuar as ações, mesmo quando o utilizador tem uma conta de administrador. Para executar scripts do PowerShell, crie uma conta de serviço com uma palavra-passe segura e não ative a conta para a verificação de dois passos.

>[!WARNING]
>As palavras-passe de aplicação não funcionam em ambientes híbridos onde os clientes comunicam com no local e na nuvem pontos finais de deteção automática. As palavras-passe de domínio são necessários para autenticar no local. As palavras-passe de aplicação são necessários para autenticar com a nuvem.
>

### <a name="guidance-for-app-password-names"></a>Orientações para nomes de palavra-passe de aplicação

Os nomes de palavra-passe de aplicações reflitam o dispositivo no qual está a utilizar. Se tiver um portátil com aplicações não baseadas no browser como o Outlook, Word e Excel, crie uma palavra-passe com o nome **portátil** para estas aplicações. Criar outra aplicação palavra-passe com o nome **ambiente de trabalho** para as mesmas aplicações que são executados no seu computador de secretária. 

>[!NOTE]
>Recomendamos que crie uma palavra-passe por dispositivo, em vez de uma palavra-passe por aplicação.

### <a name="federated-or-single-sign-on-app-passwords"></a>Palavras-passe de federada ou único início de sessão na aplicação

AD do Azure suporta a Federação ou início de sessão único (SSO), com local no Windows Server Active Directory Domain Services (AD DS). Se a sua organização estiver federada com o Azure AD e estiver a utilizar o Azure multi-factor Authentication, considere os seguintes pontos sobre palavras-passe de aplicação.

>[!NOTE]
>Os pontos seguintes aplicam-se apenas aos clientes do Federado (SSO).

* As palavras-passe de aplicação são verificadas pelo Azure AD e, por conseguinte, ignorar a Federação. Federação é utilizada ativamente apenas quando configurar palavras-passe de aplicação.
* Não é possível contactar o fornecedor de identidade (IdP) para utilizadores federados do (SSO), ao contrário do fluxo passivo. As palavras-passe de aplicação são armazenadas na conta escolar ou profissional. Se um utilizador sai da empresa, as informações do utilizador passa para a conta escolar ou profissional utilizando **DirSync** em tempo real. A desativação/eliminação da conta pode demorar até três horas a sincronização, que pode atrasar a desativação/eliminação a palavra-passe de aplicação no Azure AD.
* Definições de controlo de acesso de cliente no local não são cumpridas pela funcionalidade de palavras-passe de aplicação.
* Sem autenticação no local a capacidade de registo/auditoria está disponível para utilização com a funcionalidade de palavras-passe de aplicação.
* Alguns arquiteturas avançadas requerem uma combinação de credenciais para a verificação de dois passos com clientes. Estas credenciais podem incluir um trabalho ou escola conta username e palavras-passe e palavras-passe de aplicação. Os requisitos dependem da forma como a autenticação é efetuada. Para clientes que se autenticarem numa infraestrutura no local, uma empresa ou escola conta nome de utilizador e palavra-passe necessária. Para clientes de autenticação no Azure AD, é necessária uma palavra-passe de aplicação.

  Por exemplo, suponha que tem a arquitetura do seguinte:

  * A instância no local do Active Directory está federada com o Azure AD.
  * Está a utilizar o Exchange online.
  * Está a utilizar o Skype para empresas no local.
  * Está a utilizar o Azure multi-factor Authentication.

  ![Utilizar palavras-passe de aplicação numa organização federada](./media/multi-factor-authentication-whats-next/federated.png)

  Neste cenário, utilize as seguintes credenciais:

  * Para iniciar sessão no Skype para empresas, utilize o seu trabalho ou escola username de conta e palavra-passe.
  * Para aceder o livro de endereços a partir de um cliente do Outlook que liga ao Exchange online, utilize uma palavra-passe de aplicação.

### <a name="allow-users-to-create-app-passwords"></a>Permitir que os utilizadores criem palavras-passe de aplicação

Por predefinição, os utilizadores não é possível criar palavras-passe de aplicação. A funcionalidade de palavras-passe de aplicação tem de estar ativada. Para conceder aos utilizadores a capacidade de criar palavras-passe de aplicação, utilize o seguinte procedimento:

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No lado esquerdo, selecione **do Azure Active Directory** > **utilizadores e grupos** > **todos os utilizadores**.
3. Selecione **multi-factor Authentication**.
4. Em multi-factor Authentication, selecione **definições do serviço**.
5. No **definições do serviço** página, selecione o **permitir aos utilizadores criar palavras-passe de aplicação para iniciar sessão em aplicações não baseadas no browser** opção.

   ![Permitir que os utilizadores criem palavras-passe de aplicação](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="create-app-passwords"></a>Criar palavras-passe de aplicação

Os utilizadores podem criar palavras-passe de aplicação durante o respetivo registo inicial. O utilizador tem a opção de criar palavras-passe de aplicação no final do processo de registo.

Os utilizadores também podem criar palavras-passe de aplicação após o registo. As palavras-passe de aplicação podem ser alteradas via as definições no portal do Azure ou no portal do Office 365. Para obter mais informações e os passos detalhados para os seus utilizadores, consulte [quais são as palavras-passe de aplicação no Azure multi-factor Authentication?](./end-user/multi-factor-authentication-end-user-app-passwords.md)

<a name="remember-multi-factor-authentication-for-devices-that-users-trust"></a>
## <a name="remember-multi-factor-authentication-for-trusted-devices"></a>Lembre-se de multi-factor Authentication para dispositivos fidedignos
O _não se esqueça de multi-factor Authentication_ funcionalidade para dispositivos e browsers que são consideradas fidedignas pelo utilizador é uma funcionalidade livre para todos os utilizadores de multi-factor Authentication. Os utilizadores podem ignorar verificações subsequentes para um número especificado de dias, após serem tem com êxito com sessão iniciada num dispositivo utilizando o multi-factor Authentication. A funcionalidade melhora a capacidade de utilização ao minimizar o número de vezes que um utilizador tem de executar a verificação de dois passos no mesmo dispositivo.

>[!IMPORTANT]
>Se um dispositivo ou a conta for comprometido, tendo em conta multi-factor Authentication para dispositivos fidedignos pode afetar a segurança. Se uma conta de empresa fica comprometida ou se um dispositivo fidedigno é perdido ou roubado, deve [restaurar o multi-factor Authentication em todos os dispositivos](multi-factor-authentication-manage-users-and-devices.md#restore-mfa-on-all-remembered-devices-for-a-user).
>
>A acção de restauro revoga o estado de todos os dispositivos fidedigno e o utilizador é necessário para executar novamente a verificação de dois passos. Também pode instruir os utilizadores para restaurar o multi-factor Authentication nos respetivos dispositivos com as instruções em [gerir as definições de verificação em dois passos](./end-user/multi-factor-authentication-end-user-manage-settings.md#require-two-step-verification-again-on-a-device-youve-marked-as-trusted).
>

### <a name="how-the-feature-works"></a>Como funciona a funcionalidade

A funcionalidade de multi-factor Authentication Lembre-se define um cookie persistente no browser quando um utilizador seleciona o **não perguntar novamente durante X dias** opção no início de sessão. Não é pedido ao utilizador novamente para o multi-factor Authentication a partir desse mesmo browser até que o cookie expira. Se o utilizador abre um browser diferente no mesmo dispositivo ou limpa os cookies, se for pedidas novamente para verificar. 

O **não perguntar novamente durante X dias** opção não é apresentada nas aplicações não baseadas no browser, independentemente se a aplicação suporta a autenticação moderna. Utilizam estas aplicações _tokens de atualização_ que forneça novos tokens de acesso a cada hora. Quando um token de atualização é validado, do Azure AD verifica que a última verificação de dois passos ocorreu num número especificado de dias. 

A funcionalidade reduz o número de autenticações nas aplicações web, que normalmente sempre de linha de comandos. A funcionalidade aumenta o número de autenticações para clientes de autenticação moderna que normalmente pedem todos os 90 dias.

>[!IMPORTANT]
>O **não se esqueça de multi-factor Authentication** funcionalidade não é compatível com o **manter a minha sessão iniciada** funcionalidade do AD FS, quando os utilizadores executar verificação de dois passos para o AD FS através de multi-factor do Azure Servidor de autenticação ou uma solução de terceiros multi-factor authentication.
>
>Se os seus utilizadores selecionarem **manter a minha sessão iniciada** no AD FS e marca os seus dispositivos como fidedigna para o multi-factor Authentication, o utilizador não é automaticamente verificado após o **Lembre-se a autenticação multifator**número de dias expirar. Azure AD solicita uma verificação de dois passos raiz, mas do AD FS devolve um token com a afirmação de multi-factor Authentication original e data, em vez de executar novamente a verificação. Define esta reação desativar um ciclo de verificação entre o Azure AD e AD FS.
>

### <a name="enable-remember-multi-factor-authentication"></a>Lembre-se de ativar multi-factor Authentication

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No lado esquerdo, selecione **do Azure Active Directory** > **utilizadores e grupos** > **todos os utilizadores**.
3. Selecione **multi-factor Authentication**.
4. Em multi-factor Authentication, selecione **definições do serviço**.
5. No **definições do serviço** página **gerir Lembre-se a autenticação multifator**, selecione o **permitir que os utilizadores memorizem a autenticação multifator em dispositivos eles confiam**opção.

   ![Lembre-se de multi-factor Authentication para dispositivos fidedignos](./media/multi-factor-authentication-whats-next/remember.png)

6. Defina o número de dias para permitir que os dispositivos fidedignos ignorar a verificação de dois passos. A predefinição é 14 dias.
7. Selecione **Guardar**.

### <a name="mark-a-device-as-trusted"></a>Marcar um dispositivo como fidedigna

Depois de ativar a funcionalidade de multi-factor Authentication de lembrar, os utilizadores podem marcar um dispositivo como fidedigna quando iniciam sessão no selecionando **não voltar a perguntar**.

![Selecione "Não voltar a perguntar" para dispositivos fidedignos](./media/multi-factor-authentication-whats-next/trusted.png)

## <a name="selectable-verification-methods"></a>Métodos de verificação selecionável

Pode escolher os métodos de verificação que estão disponíveis para os seus utilizadores utilizando o _métodos de verificação selecionável_ funcionalidade. A tabela seguinte fornece uma breve descrição geral dos métodos.

Quando os utilizadores inscrevem das respetivas contas de Azure multi-factor Authentication, o método de verificação preferida que escolherem entre as opções que tiver ativado. É fornecida orientação para o processo de inscrição de utilizador no [configurar a minha conta para a verificação de dois passos](multi-factor-authentication-end-user-first-time.md).

| Método | Descrição |
|:--- |:--- |
| Ligar para telefone |Coloca uma chamada de voz automatizada. O utilizador atende a chamada e prime # no teclado do telefone para se autenticar. O número de telefone não está sincronizado para o Active Directory no local. |
| Mensagem de texto para telefone |Envia uma mensagem de texto que contém um código de verificação. É pedido ao utilizador para introduzir o código de verificação para a interface de início de sessão. Este processo é denominado SMS unidirecional. SMS bidirecional significa que o utilizador tem texto novamente um código específico. SMS bidirecional é preterida e não é suportado depois 14 de Novembro de 2018. Os utilizadores que estão configurados para o SMS bidirecionais são automaticamente mudados para _chamada para o telefone_ verificação nessa altura.|
| Notificação através de aplicação móvel |Envia uma notificação push para o seu telefone ou dispositivo registado. O utilizador vê a notificação e seleciona **verifique** para concluir a verificação. A aplicação Authenticator da Microsoft está disponível para [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), e [iOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
| Código de verificação da aplicação móvel |A aplicação Microsoft Authenticator gera um novo código de verificação de OATH cada 30 segundos. O utilizador introduz o código de verificação para a interface de início de sessão. A aplicação Authenticator da Microsoft está disponível para [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), e [iOS](http://go.microsoft.com/fwlink/?Linkid=825073). |

### <a name="enable-and-disable-verification-methods"></a>Ativar e desativar os métodos de verificação

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No lado esquerdo, selecione **do Azure Active Directory** > **utilizadores e grupos** > **todos os utilizadores**.
3. Selecione **multi-factor Authentication**.
4. Em multi-factor Authentication, selecione **definições do serviço**.
5. No **definições do serviço** página **opções de verificação**, selecionar/anular a seleção de métodos para fornecer aos seus utilizadores.

   ![Selecionar os métodos de verificação](./media/multi-factor-authentication-whats-next/authmethods.png)

6. Clique em **Guardar**.
