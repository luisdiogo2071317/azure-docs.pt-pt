---
title: 'Azure AD Connect: Migrar de Federação para a sincronização de hash de palavra-passe para o Azure AD | Documentos da Microsoft'
description: Informações sobre mover o seu ambiente de identidade híbrida da Federação para sincronização de hash de palavra-passe.
services: active-directory
author: billmath
manager: mtillman
ms.reviewer: martincoetzer
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 12/13/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: c226eb19dbd2049c486acfb1ffb9423fdb1dad43
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2018
ms.locfileid: "53410266"
---
# <a name="migrate-from-federation-to-password-hash-synchronization-for-azure-ad"></a>Migrar de Federação para a sincronização de hash de palavra-passe para o Azure AD
O documento seguinte fornece orientações sobre a migração do AD FS para sincronização de hash de palavra-passe.

>[!NOTE]
>Um que pode ser baixado uma cópia deste documento do está disponível [aqui](https://aka.ms/ADFSTOPHSDPDownload).


## <a name="prerequisites-for-the-migration"></a>Pré-requisitos para a migração 
Os seguintes pré-requisitos são necessários para poder migrar.
### <a name="update-azure-ad-connect"></a>Atualização do Azure AD Connect

Mínimo para realizar com êxito os passos para migrar para a autenticação pass-through, deve ter [do Azure AD connect](https://www.microsoft.com/download/details.aspx?id=47594) 1.1.819.0. Esta versão contém alterações significativas na forma como o início de sessão de conversão é executada e reduz o tempo geral para migrar de Federação para autenticação em nuvem de potencialmente horas e minutos.

> [!IMPORTANT]
> Desatualizado documentação, ferramentas e blogs indicam que a conversão de utilizador é uma etapa necessária ao converter domínios federados gerida. Tenha em atenção que a conversão dos usuários não é necessária mais e a Microsoft está trabalhando na atualização de documentação e ferramentas para refletir isso.

Para atualizar o Azure AD Connect para a versão mais recente deste [instruções de atualização](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version).

### <a name="password-hash-synchronization-required-permissions"></a>Permissões de necessária a sincronização de hash de palavra-passe

O Azure AD Connect pode ser configurado com definições rápidas ou instalação personalizada. Se tiver utilizado a opção de instalação personalizada, o [permissões obrigatórias](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-accounts-permissions) para sincronização de Hash de palavra-passe não pode estar em vigor.

O Azure AD Connect AD DS do serviço de necessidades de conta as seguintes permissões para ser capaz de sincronizar os hashes de palavra-passe.

* Replicar as alterações de diretório

* Diretório de replicar muda tudo

Agora é um bom momento para validar estas permissões estão em vigor para todos os domínios na floresta.

### <a name="plan-migration-method"></a>Método de migração do plano

Existem dois métodos para migrar de autenticação federada para sincronização de Hash de palavra-passe e o SSO totalmente integrado. O método que utilizar dependerá de como o AD FS foi originalmente configurado. 



- **Opção a: Utilizar o Azure AD Connect**. Se o AD FS foi originalmente configurado com o Azure AD Connect, a alteração para a sincronização de Hash de palavra-passe como o método de início de sessão do utilizador deve ser executada por meio do Assistente do Azure AD Connect.   
Ao utilizar o Azure AD Connect, ele é executado o cmdlet Set-MsolDomainAuthentication para automaticamente quando alterar o método de início de sessão do utilizador e, portanto, não tem nenhum controle sobre ele unfederating todos os domínios verificados federados no seu inquilino do Azure AD.

> [!NOTE]
> Neste momento, não é possível evitar unfederating todos os domínios no seu inquilino, quando altera o utilizador inicie sessão para a sincronização de Hash de palavra-passe quando o AAD Connect tinha sido originalmente utilizado para configurar o AD FS para.  



- **Opção b: Utilizar o Azure AD Connect com o PowerShell**. Este método pode ser utilizado apenas quando o AD FS não foi originalmente configurado com o Azure AD Connect. Ainda precisa de alterar o método de início de sessão do utilizador através do Assistente do Azure AD Connect, mas a principal diferença é que não será automaticamente executado o cmdlet Set-MsolDomainAuthentication para como ela tem não conhecimento do farm do AD FS e, portanto, tem controle total sobre quais os domínios que são convertidos e em que ordem.

Para compreender qual método deve utilizar, execute os passos na secção seguinte.

#### <a name="verify-current-user-sign-in-settings"></a>Certifique-se de início de sessão configurações do usuário atual

Verifique se o seu atual início de sessão definições do utilizador ao iniciar sessão no portal do Azure AD [ https://aad.portal.azure.com ](https://aad.portal.azure.com/) com uma conta de Administrador Global.

Na secção de sessão do utilizador, certifique-se de que o Federação está ativada e que o início de sessão único totalmente integrado e autenticação pass-through estão desativados. Verifique também se esse Estado da sincronização de palavra-passe que deve ser apresentado como desativado, a menos que isso anteriormente foi ativado.

![Figura 5](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image1.png)

#### <a name="verify-azure-ad-connect-configuration"></a>Verificar a configuração do Azure AD Connect

   1. Aceda ao seu servidor do Azure AD Connect e iniciar o Azure AD Connect e, em seguida, selecione configurar. 
   2. No ecrã de tarefas adicionais, selecione ver configuração atual e, em seguida, selecione avançar.</br>
   ![Figura 31](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image2.png)</br>
   
   3. No ecrã de rever a sua solução, tome nota do Estado da sincronização de palavra-passe.</br> 

   Se a sincronização de Hash de palavra-passe está definida como desativado, terá de seguir os passos neste guia para ativá-la. Se a sincronização de Hash de palavra-passe está definida como ativado, pode ignorar com segurança a secção [passo 1 – ativar a sincronização de Hash de palavra-passe](#step-1--enable-password-hash-synchronization) neste guia.
   4. No ecrã de rever a sua solução, desloque para baixo para o serviços de Federação do Active Directory (AD FS).</br>
 
   Se vir que a configuração do AD FS está nesta secção, em seguida, pode presumir com segurança do AD FS foi originalmente configurado através do Azure AD Connect e, por conseguinte, a conversão de seus domínios de federadas para gerido pode ser acionada por meio do Azure AD Connect "alteração de sessão de utilizador -em "opção, este processo é detalhado na secção **opção A - comutador da Federação da sincronização de Hash de palavra-passe através da utilização do Azure AD Connect**.
   5. Se não pode ver os serviços de Federação do Active Directory, listado nas configurações atuais, em seguida, precisará converter manualmente os domínios de federado para geridos através do PowerShell, que se encontra detalhado na secção **opção B - comutador da Federação para Sincronização de Hash de palavra-passe com o Azure AD Connect e o PowerShell**.

### <a name="document-current-federation-settings"></a>Definições de Federação atuais do documento

Pode encontrar a definição de Federação atual ao executar o cmdlet Get-MsolDomainFederationSettings.

O comando é:

``` PowerShell
Get-MsolDomainFederationSettings -DomainName YourDomain.extention | fl *
```

Por exemplo:

``` PowerShell
Get-MsolDomainFederationSettings -DomainName Contoso.com | fl *
```
Valide as definições que poderão ter sido personalizadas para a documentação de design e implementação de Federação, especificamente PreferredAuthenticationProtocol SupportsMfa e PromptLoginBehavior.

Mais informações sobre o que estas definições podem ser encontradas abaixo.

[Serviços de Federação do Active Directory solicitar = suporte de parâmetros de início de sessão](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-prompt-login)  
‎[Conjunto MsolDomainAuthentication](https://docs.microsoft.com/powershell/module/msonline/set-msoldomainauthentication?view=azureadps-1.0)

> [!NOTE]
> Se o valor de SupportsMfa está definido como "True", em seguida, isso significa que está a utilizar uma solução de MFA no local para injetar um desafio de fator 2nd o fluxo de autenticação de utilizador. Isso deixará de funcionar para cenários de autenticação do Azure AD e, em vez disso, terá de tirar partido do serviço do MFA do Azure (com base na cloud) para executar a mesma função. Cuidadosamente avaliar os seus requisitos de MFA antes de prosseguir e certifique-se de que compreende como tirar partido do MFA do Azure, as implicações de licenciamento e o processo de registo do utilizador final antes de converter seus domínios. Nosso guia de implementação para a MFA do Azure, que apresenta mais detalhes pode ser encontrado em [ https://aka.ms/deploymentplans ](https://aka.ms/deploymentplans).

#### <a name="backup-federation-settings"></a>Definições de Federação de cópia de segurança

Embora não serão efetuadas alterações a outras partes da entidade Confiadora no farm do AD FS durante este processo, recomenda-se para se certificar de que tem uma cópia de segurança válida atual do farm do AD FS que pode ser restaurada. Pode fazê-lo usando o Microsoft gratuito [AD FS rápida restaurar ferramenta](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-rapid-restore-tool). Essa ferramenta pode ser usada para criar cópias de segurança e restaurar para um farm existente ou um novo farm do AD FS.

Se optar por não utilizar a ferramenta de restauro rápido do AD FS, em seguida, no mínimo, deve exportar a "Microsoft Office 365 plataforma de identidade" da entidade confiadora fidedignidade de terceiros e quaisquer regras de afirmação personalizada associado que possa ter adicionado. Pode fazê-lo através do seguinte exemplo do PowerShell

``` PowerShell
(Get-AdfsRelyingPartyTrust -Name "Microsoft Office 365 Identity Platform") | Export-CliXML "C:\temp\O365-RelyingPartyTrust.xml"
```

## <a name="deployment-considerations-and-ad-fs-usage"></a>Considerações sobre a implementação e utilização do AD FS

### <a name="validate-your-current-ad-fs-usage"></a>Validar a sua utilização atual do AD FS

Antes de federados a converter para gerido, deve examinar mais atentamente como está a utilizar do AD FS hoje mesmo para o Azure AD/Office 365 e outros aplicativos (confianças de entidade confiadora). Especificamente, deve considerar a tabela seguinte:

| IF| em seguida |
|-|-|
| Pretender manter o AD FS para os outros aplicativos.| Vai utilizar o AD FS e o Azure AD e será necessário considerar a experiência de utilizador final como resultado. Os usuários podem precisar para se autenticar duas vezes em alguns cenários, uma vez para o Azure AD (onde irá obter SSO em diante para outros aplicativos como o Office 365) e novamente para todos os aplicativos ainda vinculados ao AD FS como uma fidedignidade de entidade confiadora. |
| O AD FS é bastante dependente de definições de personalização específicos no ficheiro onload.js que não pode ser duplicado no Azure AD e personalizadas (por exemplo, que tenha alterado a experiência de início de sessão para que os utilizadores apenas de introduzir um formato de SamAccountName para o respetivo nome de utilizador em vez de para um UPN, ou tem um intensamente com marca a experiência de início de sessão)| Terá de verificar se os seus requisitos de personalização atual podem ser atendidos pelo Azure AD antes de continuar. Consulte as secções de marca do AD FS e personalização do AD FS para obter mais informações e orientações.|
| Estão a bloquear os clientes de autenticação através do AD FS.| Considere substituir os controles deve bloquear clientes antigos de autenticação atualmente presentes no AD FS com uma combinação de [controla o acesso condicional para a autenticação de legado](https://docs.microsoft.com/azure/active-directory/conditional-access/conditions) e [acesso de cliente do Exchange Online Regras](http://aka.ms/EXOCAR).|
| Exigir que os usuários realizem MFA em relação a uma solução de servidor MFA no local durante a autenticação do AD FS.| Não será possível inserir uma submissão da MFA através da solução MFA no local para o fluxo de autenticação para um domínio gerido, no entanto, pode utilizar o serviço de MFA do Azure para fazê-lo daqui para frente uma vez o domínio é convertido. Se os utilizadores não estiver a utilizar hoje de MFA do Azure, em seguida, isso envolverá um passo de registo de utilizador final de uma única vez que tem para preparar e comunicar aos seus utilizadores finais.|
| Utilize políticas de controlo de acesso (regras de AuthZ) hoje no AD FS para controlar o acesso ao Office 365.| Considere substitui-los com o equivalente do Azure AD [políticas de acesso condicional](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal) e [regras de acesso de cliente Online do Exchange](http://aka.ms/EXOCAR).|

### <a name="considerations-for-common-ad-fs-customizations"></a>Considerações comuns personalizações do AD FS

#### <a name="inside-corporate-network-claim"></a>Afirmação da rede empresarial interior

A afirmação InsideCorporateNetwork é emitida pelo AD FS, se a autenticação de utilizador é dentro da rede empresarial. Esta afirmação, em seguida, pode ser passada para o Azure AD e utilizada para ignorar o multi-factor authentication com base na localização de rede dos utilizadores. Ver [IPs fidedignos para utilizadores federados](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud) para obter informações sobre como determinar se tem isso atualmente ativada no AD FS.

A afirmação InsideCorporateNetwork não estarão disponível mais uma vez que os seus domínios são convertidos em sincronização de Hash de palavra-passe. [Localizações com nome no Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-named-locations) pode ser utilizado para substituir esta funcionalidade.

Depois de configurar localizações com o nome, todas as políticas de acesso condicional configuradas para incluir ou excluir as localizações de rede "Todas as localizações fidedignas" ou "IPs fidedignos de MFA" tem de ser atualizado para refletir os locais com o nome criado recentemente.

Ver [Active Directory localizações acesso condicional do](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-locations) para obter mais informações sobre a condição de localização no acesso condicional.

#### <a name="hybrid-azure-ad-joined-devices"></a>Dispositivos do híbrida do Azure AD associado

Ingressar num dispositivo para o Azure AD permite-lhe criar regras de acesso condicional que impõem os dispositivos que cumprem as normas de acesso de segurança e conformidade e permite aos utilizadores para início de sessão para um dispositivo com uma conta escolar ou profissional de organizacional em vez de pessoal conta. Dispositivos de associados a um híbrido do Azure AD permite-lhe associar os dispositivos associados a um domínio do AD para o Azure AD. Seu ambiente federado poderá ter sido configurado com esta funcionalidade.

Para garantir a que associar híbrida continua a trabalhar para novos dispositivos associados ao domínio, uma vez que os seus domínios foram convertidos para sincronização de Hash de palavra-passe, o Azure AD Connect tem de ser configurado para sincronizar contas de computador do Active Directory para que os clientes Windows 10 O Azure AD. Para contas de computador do Windows 7 e Windows 8, Junte-se a híbrida irá utilizar o SSO totalmente integrado para registar o computador no Azure AD e não é necessário sincronizá-los tal como para dispositivos Windows 10. No entanto terá que implantar um arquivo de workplacejoin.exe atualizada (por meio de um ficheiro. msi) para estes clientes de nível inferior para que eles podem se registrar com o SSO totalmente integrado. [Transferir o. msi](https://www.microsoft.com/download/details.aspx?id=53554). 

Para obter mais informações, consulte [como configurar híbrida do Azure Active Directory dispositivos associados ao](https://docs.microsoft.com/azure/active-directory/device-management-hybrid-azuread-joined-devices-setup).

#### <a name="branding"></a>Personalização

Sua organização pode ter [personalizou sua páginas de início de sessão do AD FS](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-user-sign-in-customization) para apresentar informações mais pertinentes para a organização. Se assim for, considere fazer semelhante [personalizações para a página de início de sessão do Azure AD](https://docs.microsoft.com/azure/active-directory/customize-branding).

Embora personalizações semelhantes estejam disponíveis, devem esperar algumas alterações visual. Pode querer incluir alterações esperadas nas comunicações aos utilizadores finais.

> [!NOTE]
> Imagem corporativa da empresa está disponível apenas se comprou o Premium ou básica licença para o Azure AD ou ter uma licença do Office 365.

## <a name="planning-deployment-and-support"></a>Planear a implementação e suporte

### <a name="plan-the-maintenance-window"></a>Planear a janela de manutenção

Enquanto o processo de conversão de domínio em si é relativamente rápido, do Azure AD pode ainda enviar alguns pedidos de autenticação para servidores do AD FS durante um período de até quatro horas após concluir a conversão de domínio. Durante esse período de quatro horas e, dependendo de vários caches de lado do serviço, estas autenticações não podem ser aceite pelo Azure AD e os utilizadores irão receber um erro, como poderão autenticar com êxito do AD FS ainda, mas já não é do Azure AD aceitará um do utilizador emitiu o token, como essa confiança de Federação foi agora removida.

> [!NOTE]
> Isto irá apenas os utilizadores de impacto que acessarem os serviços através de um browser durante esta janela de conversão de post até que a cache do lado do serviço está desmarcada. Não devem ser afetados clientes herdados (Exchange ActiveSync, Outlook 2010/2013), como o Exchange Online mantém um cache das credenciais por um período de tempo que é utilizado para autenticar novamente o usuário silenciosamente sem a necessidade de voltar atrás para o AD FS. As credenciais armazenadas no dispositivo para estes clientes são utilizadas para autenticar novamente próprios automaticamente assim que isso em cache está desmarcado e, por conseguinte, os utilizadores não devem receber quaisquer pedidos de palavra-passe como resultado do processo de conversão de domínio. Por outro lado, para clientes de autenticação moderna (Office 2013 de Maio de 2016, aplicações IOS e Android) estes utilizarem uma válido atualizar Token para obter novos tokens de acesso de acesso contínuo aos recursos em vez de aceder para o AD FS e, por conseguinte, imunes a quaisquer pedidos de palavra-passe como um resultado da conversão domínio processar e continuará a funcionar sem qualquer configuração adicional necessária.

> [!IMPORTANT]
> Não encerre o seu ambiente do AD FS ou remova a confiança de entidade confiadora até ter verificado a todos os utilizadores são autenticar com êxito utilizando a autenticação na nuvem do Office 365.

### <a name="plan-for-rollback"></a>Plano de reversão

Se um grande problema é encontrado e não pode ser resolvido rapidamente, que pode optar por revertê-lo a solução de volta para a Federação. É importante planejar o que fazer se a implementação não for conforme planejado. Se a conversão do domínio ou os utilizadores falhar durante a implementação, ou precisar de reverter para a Federação, tem de compreender como a reduzir qualquer interrupção e a reduzir o impacto para os seus utilizadores.

#### <a name="rolling-back"></a>A reverter

Consulte a documentação de design e implementação de Federação para os detalhes da sua implementação específica. O processo deve envolver:

* Converter os domínios de geridos para federados utilizando Convert-MSOLDomainToFederated 

* Se necessário, adicionais a configurar regras de afirmações.

### <a name="plan-change-communications"></a>Planear as comunicações de alteração

Uma parte importante do planeamento de implementação e suporte é garantir que os utilizadores finais são proativamente informados sobre as alterações e o que poderão ter ou o que devem fazer. 

Após a sincronização de Hash de palavra-passe e Seamless SSO são implementadas, a experiência de início de sessão do utilizador final será alterado ao aceder ao Office 365 e outros recursos autenticados através do Azure AD de associados. Os utilizadores externos à rede agora verá o início de sessão do Azure AD apenas, a página em vez de a ser redirecionado para a página baseada em formulários, apresentada por servidores do Proxy de aplicações Web com acesso externos.

Existem vários elementos para planejar sua estratégia de comunicação. Estas incluem:

* Notificar os utilizadores da funcionalidade de futuro e lançada por meio de
  * E-mail e outros canais de comunicação interna
  * Elementos visuais, tais como cartazes
  * Ao vivo ou outras comunicações
* Determinar quem irá personalizar e que enviará as comunicações e quando.

## <a name="implementing-your-solution"></a>Implementar a solução

Agora que tenha se planejado sua solução, está pronto para implementá-lo. Implementação inclui os seguintes componentes:

1. Ativar a sincronização de hash de palavras-passe

2. Preparar para o início de sessão único totalmente integrado no

3. Alterar método de início de sessão de sincronização de Hash de palavra-passe e ativar o SSO totalmente integrado

### <a name="step-1--enable-password-hash-synchronization"></a>Passo 1 – ativar a sincronização de hash de palavra-passe

O primeiro passo para implementar esta solução é ativar a sincronização de Hash de palavra-passe no Assistente do Azure AD Connect. Sincronização de Hash de palavra-passe é uma funcionalidade opcional que pode ser ativada em ambientes com o Federação sem qualquer impacto para o fluxo de autenticação. Neste caso, o Azure AD Connect irá iniciar a sincronização de hashes de palavra-passe sem afetar os utilizadores a iniciar sessão com a Federação.

Por esse motivo, recomendamos a execução deste passo como uma tarefa de preparação bem antes de alterar o seu método de início de sessão domínios. Isso lhe dará tempo bastante para validar a sincronização de Hash de palavra-passe está a funcionar corretamente.

Para ativar a sincronização de Hash de palavra-passe:

   1. Sobre o Azure AD ligar servidor, abra o assistente e selecione configurar.
   2. Selecione as opções de sincronização de personalizar e, em seguida, selecione avançar.
   3. No ecrã do Azure AD ligar ao fornece o nome de utilizador e palavra-passe de um Administrador Global.
   4. Na ligar o ecrã de diretórios, clique em seguinte.
   5. No ecrã de filtragem de domínio e UO, clique em seguinte.
   6. No ecrã de funcionalidades opcionais, selecione a sincronização de palavra-passe e selecione seguinte.
   ![Figura 21](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image6.png)</br>
   7. Selecione em seguida em todos os ecrãs e configurar restante na última tela.
   8. O Azure AD Connect irá iniciar a sincronização de hashes de palavra-passe na próxima sincronização.

Assim que tiver sido ativada a sincronização de Hash de palavra-passe, a palavra-passe codifica para todos os utilizadores no Azure AD Connect, escopo de sincronização será com novo hash e escrito para o Azure AD. Dependendo do número de utilizadores, esta operação pode demorar minutos ou várias horas.

Para fins de planejamento, deve determinar que aproximadamente 20 000 utilizadores podem ser processados em 1 hora.

Para validar a sincronização de Hash de palavra-passe está corretamente a funcionar, utilize a tarefa de resolução de problemas no Assistente do Azure AD Connect.

   1. Abra uma nova sessão do Windows PowerShell no seu servidor do Azure AD Connect com a execução como opção de administrador.
   2. Execute Set-ExecutionPolicy RemoteSigned ou Set-ExecutionPolicy irrestrito.
   3. Inicie o Assistente do Azure AD Connect.
   4. Navegue para a página de tarefas adicionais, selecione a resolução de problemas e clique em seguinte.
   5. Na página de resolução de problemas, clique em Iniciar para iniciar o menu de resolução de problemas no PowerShell.
   6. No menu principal, selecione a sincronização de hash de palavra-passe de resolução de problemas.
   7. No menu do sub, selecione a sincronização de hash não funciona em todos os de palavra-passe.

Se encontrar problemas, utilize as informações na [este artigo](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-troubleshoot-password-hash-synchronization) para resolver problemas.

### <a name="step-2--prepare-for-seamless-sso"></a>Passo 2 – preparar para SSO totalmente integrado

Para os seus dispositivos para utilizar o SSO totalmente integrado, terá de adicionar um URL de AD do Azure para as definições de zona de Intranet dos utilizadores ao utilizar a política de grupo no Active Directory.

Por predefinição, o navegador calcula automaticamente a zona correta, Internet ou Intranet, a partir de uma URL específica. Por exemplo, "http://contoso/"é mapeado para a zona da Intranet, ao passo que"http://intranet.contoso.com/" mapeia para a zona de Internet (porque o URL contém um período). Browsers não irão enviar os tíquetes Kerberos para um ponto final da cloud, como o URL de AD do Azure, a menos que explicitamente, adicione o URL para a zona de Intranet do navegador.

Siga os [passos para distribuir o](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-quick-start) as alterações necessárias para os seus dispositivos.

> [!IMPORTANT]
> Efetuar esta alteração não irá modificar a forma como os utilizadores iniciam sessão Azure AD. No entanto, é importante que esta configuração é aplicada a todos os seus dispositivos antes de continuar com o passo 3. Tenha também em atenção que os utilizadores a iniciar sessão em dispositivos que não tenham recebido esta configuração simplesmente tem de introduzir o nome de utilizador e palavra-passe para iniciar sessão Azure AD.

### <a name="step-3--change-sign-in-method-to-phs-and-enable-seamless-sso"></a>Passo 3 – alterar método de início de sessão para PHS e ativar o SSO totalmente integrado

#### <a name="option-a---switch-from-federation-to-phs-by-using-azure-ad-connect"></a>Opção A - mudar da Federação para PHS através da utilização do Azure AD Connect

Utilize este método quando o AD FS foi inicialmente configurado com o Azure AD Connect. Não é possível utilizar este método se o AD FS não foi originalmente configurado com o Azure AD Connect. Primeira **alterar método de utilizador de início de sessão**

   1. Sobre o Azure AD ligar servidor, abra o assistente.
   2. Selecione o utilizador de alterar início de sessão e, em seguida, selecione avançar.
   ![Figura 27](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image7.png)</br>
   3. Na **ligar para o Azure AD** ecrã fornecer o nome de utilizador e palavra-passe de um **Administrador Global**.
   4. Na **sessão do utilizador** ecrã, alterar o botão de opção de federação com o AD FS para transmitir a sincronização de Hash e certificar-se de que a caixa de verificação não converter contas de utilizador que isso esteja um passo preterido e será removido numa versão futura do AAD Connect. Também selecionar ativar início de sessão único, em seguida, selecione **seguinte**.
   ![29 de imagem](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image8.png)</br>
   
   > [!NOTE]
   > A partir do Azure AD Connect versão 1.1.880.0, a totalmente integrada única início de sessão na caixa de verificação está ativada por predefinição.
   
   > [!IMPORTANT]
   > Pode ignorar os avisos que indica a conversão de utilizador e sincronização de hash de palavras-passe completa são os passos necessários para a conversão da Federação para autenticação na nuvem. Tenha em atenção que estes passos não são necessários mais, as futuras versões do Azure AD Connect não terá uma opção para converter os utilizadores. Se continuar a ver estes avisos, verificação de que estão a executar a versão mais recente do Azure AD Connect e que está a utilizar a versão mais recente deste guia. Para obter mais informações, consulte a [secção atualização Azure AD Connect](#_Update_Azure_AD).
   
   5. No ecrã de ativar o início de sessão único, introduza as credenciais da conta de administrador de domínio, em seguida, selecione avançar.
   ![Figura 35](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image9.png)</br>
   
   > [!NOTE]
   > As credenciais de administrador de domínio são necessárias para ativar o início de sessão único totalmente integrado, como o processo executa as seguintes ações que exigem essas permissões elevadas. As credenciais de administrador de domínio não são armazenadas no Azure AD Connect ou no Azure AD. Serem utilizados apenas para ativar a funcionalidade e, em seguida, rejeitados após a conclusão com êxito
   >  * É criada uma conta de computador com o nome AZUREADSSOACC (que representa o Azure AD) no seu local do Active Directory (AD).
   >  * Chave de desencriptação do Kerberos da conta de computador é compartilhado de forma segura com o Azure AD.
   >  * Além disso, os dois Kerberos principal nomes de serviço (SPNs) são criados para representar dois URLs que são utilizadas durante o início de sessão no Azure AD.
   >  * As credenciais de administrador de domínio não são armazenadas no Azure AD Connect ou no Azure AD. Serem utilizados apenas para ativar a funcionalidade e, em seguida, rejeitados após a conclusão com êxito
   
   6. No preparado para o ecrã configurar, certifique-se de "Iniciar o processo de sincronização quando tiver concluído a configuração de" caixa de verificação está selecionada. Em seguida, selecione configurar.
   ![Figura 36](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image10.png)</br>
   
   > [!IMPORTANT]
   > Neste momento, todos os domínios federados serão alterados para a autenticação gerida que irá agora tirar partido de sincronização de Hash de palavra-passe como o método de autenticação.
       
   7. Abra o portal do Azure AD, selecione Azure Active Directory e, em seguida, selecione Azure AD Connect.
   8. Certifique-se de que o Federação está desativada durante o início de sessão único totalmente integrado- e sincronização de palavra-passe estão ativadas.  
  ![Figura 37](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image11.png)</br>
   9. Aceda a [etapas de testes e seguintes](#testing-and-next-steps).
   
   > [!IMPORTANT]
   > Ignorar a seção opção B - mudar da Federação para a sincronização de Hash de palavra-passe com o Azure AD Connect e o PowerShell como os passos nesta secção não se aplicam.  

#### <a name="option-b---switch-from-federation-to-phs-using-azure-ad-connect-and-powershell"></a>Opção B - comutador da Federação para PHS com o Azure AD Connect e o PowerShell

Utilize esta opção quando a Federação não foi configurada inicialmente com o Azure AD Connect.

Como parte deste processo, irá ativar o SSO totalmente integrado e alternar sua domínios a partir de federados para gerido.

   1. Sobre o Azure AD ligar servidor, abra o assistente.
   2. Selecione o utilizador de alterar início de sessão e, em seguida, selecione avançar. 
   3. No ecrã do Azure AD ligar ao fornece o nome de utilizador e palavra-passe de um Administrador Global.
   4. No ecrã sessão do utilizador, altere o botão de opção de não configurar a sincronização de Hash de palavra-passe, selecione ativar início de sessão único, em seguida, selecione em seguida.
   
   Antes da alteração: ![Imagem 20](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image12.png)</br>

   Após a alteração:  
   ![Figura 22](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image13.png)</br>
   
   > [!NOTE]
   > A partir do Azure AD Connect versão 1.1.880.0, a totalmente integrada única início de sessão na caixa de verificação está ativada por predefinição.
   
   5. No ecrã de ativar o início de sessão único, introduza as credenciais da conta de administrador de domínio, em seguida, selecione avançar.
   
   > [!NOTE]
   > As credenciais de administrador de domínio são necessárias para ativar o início de sessão único totalmente integrado, como o processo executa as seguintes ações que exigem essas permissões elevadas. As credenciais de administrador de domínio não são armazenadas no Azure AD Connect ou no Azure AD. Estão utilizadas apenas para ativar a funcionalidade e, em seguida, rejeitados após a conclusão com êxito.
   > * É criada uma conta de computador com o nome AZUREADSSOACC (que representa o Azure AD) no seu local do Active Directory (AD).
   > * Chave de desencriptação do Kerberos da conta de computador é compartilhado de forma segura com o Azure AD.
   > * Além disso, os dois Kerberos principal nomes de serviço (SPNs) são criados para representar dois URLs que são utilizadas durante o início de sessão no Azure AD.
   
   6. No preparado para o ecrã configurar, certifique-se de "Iniciar o processo de sincronização quando tiver concluído a configuração de" caixa de verificação está selecionada. Em seguida, selecione configurar.
   ![Imagem 41](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image15.png)</br>
   Quando configura a selecionar, será configurado de acordo com o passo de pré-visualizações SSO totalmente integrado. Não possível alterar a configuração de sincronização de Hash de palavra-passe como ele foi ativado anteriormente.
   
   > [!IMPORTANT]
   > Não serão efetuadas alterações à forma como os utilizadores iniciam sessão neste momento.  
   
   7. No Portal do Azure AD, certifique-se de que o Federação continua a ser ativado e agora totalmente integrada início de sessão único está ativado.
   ![Imagem 42](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image16.png)

#### <a name="convert-domains-from-federated-to-managed"></a>Converter domínios a partir de federado para gerido

Neste momento, Federação ainda está ativado e operacional dos seus domínios. Para continuar com a implementação, cada domínio tem de ser convertidos de federados para gerido para forçar autenticação do utilizador através da sincronização de Hash de palavra-passe.

> [!IMPORTANT]
> Nem todos os domínios precisam a ser convertido ao mesmo tempo, pode escolher para começar um domínio de teste no seu inquilino de produção ou de domínio com o menor número de utilizadores.

A conversão é efetuada utilizando o módulo do PowerShell do Azure AD.

   1. Abra o PowerShell e o início de sessão para o Azure AD através de uma conta de Administrador Global.  
   2. Para converter o primeiro domínio, execute o seguinte comando:  
   
   ``` PowerShell
   Set-MsolDomainAuthentication -Authentication Managed -DomainName <domainname>
   ```
   
   3. Abra o portal do Azure AD, selecione Azure Active Directory e, em seguida, selecione Azure AD Connect.
   4. Certifique-se de que o domínio foi convertido em gerida, executando o seguinte comando:
   
   ``` PowerShell
   Get-MsolDomain -DomainName <domainname>
   ```

## <a name="testing-and-next-steps"></a>Passos de testes e seguintes

### <a name="test-authentication-with-phs"></a>Autenticação de teste com o PHS

Quando o seu inquilino estava a utilizar o Federação, os utilizadores foram redirecionados da página de início de sessão do Azure AD para o seu ambiente do AD FS. Agora que o inquilino está configurado para utilizar a sincronização de Hash de palavra-passe em vez de Federação, os utilizadores não irão obter redirecionados para o AD FS e em vez disso, irão iniciar sessão diretamente através da página de início de sessão do Azure AD.

Abra o Internet Explorer no modo InPrivate para evitar o SSO totalmente integrado a iniciar sessão automaticamente e vá para a página de início de sessão do Office 365 ([http://portal.office.com](http://portal.office.com/)). Escreva o UPN do seu utilizador e clique em seguinte. Certifique-se para o tipo de UPN do utilizador híbrido que foi sincronizado do Active Directory no local e que foi anteriormente federado. O utilizador verá a tela de digitar seu nome de utilizador e palavra-passe.

![Figura 9](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image18.png)

![Figura 12](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image19.png)

Depois de digitar a palavra-passe, deve obter redirecionado para o portal do Office 365.

![Figura 17](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image20.png)

### <a name="test-seamless-single-sign-on"></a>Testar totalmente integrada início de sessão único

Inicie sessão para um computador associado ao domínio que está ligado à rede empresarial. Abra o Internet Explorer e aceda a um dos seguintes URLs:  
  
[https://myapps.microsoft.com/contoso.com](https://myapps.microsoft.com/contoso.com) [https://myapps.microsoft.com/contoso.onmicrosoft.com](https://myapps.microsoft.com/contoso.onmicrosoft.com) (substitua Contoso com o seu domínio).

O utilizador será redirecionado rapidamente para a página de início de sessão do Azure AD e ver a mensagem "A tentar iniciar sessão" e não devem ser solicitados para um nome de utilizador ou uma palavra-passe.

![Imagem 24](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image21.png)

Em seguida, o utilizador irá obter redirecionado e iniciou sessão no painel de acesso com êxito:

> [!NOTE]
> Totalmente integrado Single Sign-On funciona em serviços do Office 365, que oferece suporte a sugestão de domínio (por exemplo, myapps.microsoft.com/contoso.com). Portal do Office 365 (portal.office.com) atualmente não suporta a sugestão de domínio e, portanto, é esperado que os utilizadores terão de digitar o respetivo UPN. Assim que um UPN é o início de sessão único totalmente integrado, introduzido no pode obter a permissão de Kerberos em nome do utilizador e registrá-los em sem escrever uma palavra-passe. 

> [!TIP]
> Considere implementar [associação do Azure AD híbrido no Windows 10](https://docs.microsoft.com/azure/active-directory/device-management-introduction) para uma única início de sessão experiência melhorada.

### <a name="removal-of-the-relying-party-trust"></a>Remoção da confiança de entidade confiadora

Depois de validar que todos os utilizadores e os clientes são autenticar com êxito através do Azure AD, ele pode ser considerado seguro remover o Office 365 confiança de entidade confiadora.

Se o AD FS não está a ser utilizado para outros fins (outra entidade Confiadora confianças foram configurados), é seguro desativar o AD FS agora.

### <a name="rollback"></a>Reversão

Se um grande problema é encontrado e não pode ser resolvido rapidamente, que pode optar por revertê-lo a solução de volta para a Federação.

Consulte a documentação de design e implementação de Federação para os detalhes da sua implementação específica. O processo deve envolver:

* Converter os domínios de geridos para federados utilizando Convert-MSOLDomainToFederated

* Se necessário, adicionais a configurar regras de afirmações.

### <a name="enable-synchronization-of-userprincipalname-updates"></a>Ativar a sincronização de atualizações de userPrincipalName

Historicamente, as atualizações para o atributo UserPrincipalName com o serviço de sincronização no local foi bloqueado, a menos que ambas estas condições forem verdadeiras:

* O utilizador for gerido (não federadas).

* Não foi atribuído uma licença do utilizador.

Para obter instruções sobre como verificar ou ativar esta funcionalidade, consulte o seguinte artigo:

[Sincronizar atualizações de userPrincipalName](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsyncservice-features).

### <a name="troubleshooting"></a>Resolução de problemas

A equipa de suporte deve compreender como resolver quaisquer problemas de autenticação que podem surgir durante ou após a alteração da Federação para gerido. Utilize a seguinte documentação de resolução de problemas para ajudar a sua equipa de suporte se familiarizar com os passos de resolução de problemas comuns e as ações adequadas que podem ajudar a isolar e resolver o problema.

[Resolver problemas de sincronização de Hash de palavra-passe do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-troubleshoot-password-hash-synchronization)

[Resolver problemas relacionados com o Azure Active Directory totalmente integrada início de sessão único](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-sso)  

## <a name="roll-over-the-seamless-sso-kerberos-decryption"></a>O rollover a desencriptação do Kerberos de SSO totalmente integrado

É importante frequentemente rollover de chave de desencriptação Kerberos da conta de computador AZUREADSSOACC (que representa o Azure AD) criado no seu local floresta do AD. Recomendamos vivamente que efetue o rollover da chave de desencriptação do Kerberos, pelo menos, a cada 30 dias para se alinhar com como membros do domínio do Active Directory submeter as alterações de palavra-passe. Como há não associado dispositivo ligado para o objeto de conta de computador AZUREADSSOACC o rollover tem de ser realizado manualmente.

Siga estes passos no servidor no local onde está a executar o Azure AD Connect, para iniciar o rollover da chave de desencriptação do Kerberos.

[Como posso reverter sobre a chave de desencriptação do Kerberos da conta de computador AZUREADSSOACC](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-faq)?

## <a name="next-steps"></a>Passos Seguintes

- [Conceitos de design do Azure AD Connect](plan-connect-design-concepts.md)
- [Escolha a autenticação correta](https://docs.microsoft.com/azure/security/azure-ad-choose-authn)
- [Topologias suportadas](plan-connect-design-concepts.md)
