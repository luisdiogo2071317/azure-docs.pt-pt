---
title: 'Azure AD Connect: Migrar de Federação para autenticação pass-through do Azure AD | Documentos da Microsoft'
description: Informações sobre mover o seu ambiente de identidade híbrida da Federação para autenticação pass-through.
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
ms.openlocfilehash: c7d236769d5e9adca0402affc2d0eccdf78a6837
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54107757"
---
# <a name="migrate-from-federation-to-pass-through-authentication-for-azure-ad"></a>Migrar de Federação para autenticação pass-through do Azure AD
O documento seguinte fornece orientações sobre a migração do AD FS para autenticação pass-through.

>[!NOTE]
>Um que pode ser baixado uma cópia deste documento do está disponível [aqui](https://aka.ms/ADFSTOPTADPDownload).

## <a name="prerequisites-for-pass-through-authentication"></a>Pré-requisitos para a autenticação pass-through
Os seguintes pré-requisitos são necessários para poder migrar.
### <a name="update-azure-ad-connect"></a>Atualização do Azure AD Connect

Mínimo para realizar com êxito os passos para migrar para a autenticação pass-through, deve ter [do Azure AD connect](https://www.microsoft.com/download/details.aspx?id=47594) 1.1.819.0. Esta versão contém alterações significativas na forma como o início de sessão de conversão é executada e reduz o tempo geral para migrar de Federação para autenticação em nuvem de potencialmente horas e minutos.

> [!IMPORTANT]
> Desatualizado documentação, ferramentas e blogs indicam que a conversão de utilizador é uma etapa necessária ao converter domínios federados gerida. **Conversão dos usuários** é não são necessárias mais e a Microsoft está trabalhando na atualização de documentação e ferramentas para refletir isso.

Para atualizar o Azure AD Connect para a versão mais recente deste [instruções de atualização](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version).

### <a name="plan-authentication-agent-number-and-placement"></a>Planear o número de agente de autenticação e a colocação

Autenticação pass-through é conseguida através da implementação de agentes de leves no Azure AD Connect servidor e nos seus servidores do Windows no local. Instale os agentes mais próximo possível para seus controladores de domínio do Active Directory para reduzir a latência.

Para a maioria dos autenticação de clientes dois ou três agentes são suficientes para elevada disponibilidade e capacidade e um inquilino pode ter mais do que 12 agentes registrados. O primeiro agente é sempre instalado no servidor do AAD Connect em si. Consulte a [informações sobre estimativas de tráfego de rede e a orientação de desempenho](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-current-limitations) para compreender as limitações de agente e as opções de implementação do agente.

### <a name="plan-migration-method"></a>Método de migração do plano

Existem dois métodos para migrar de autenticação federada para PTA e SSO totalmente integrado. O método que utilizar dependerá de como o AD FS foi originalmente configurado.

- **Utilizar o Azure AD Connect**. Se o AD FS foi originalmente configurado com o Azure AD Connect, em seguida, a alteração para a autenticação pass-through *tem* ser realizado através do Assistente do Azure AD Connect.

Ao utilizar o Azure AD Connect, ele é executado o cmdlet Set-MsolDomainAuthentication para automaticamente quando alterar o método de início de sessão do utilizador e, portanto, não tem nenhum controle sobre ele unfederating todos os domínios verificados federados no seu inquilino do Azure AD.  
‎  
> [!NOTE]
> Neste momento, não é possível evitar totalmente não federating domínios no seu inquilino ao alterar o utilizador iniciar sessão para a autenticação pass-through quando o AAD Connect tinha sido originalmente utilizado para configurar o AD FS para.  
‎
- **Utilizar o Azure AD Connect com o PowerShell**. Este método pode ser utilizado apenas quando o AD FS não foi originalmente configurado com o Azure AD Connect. Ainda precisa de alterar o método de início de sessão do utilizador através do Assistente do Azure AD Connect, mas a principal diferença é que não será automaticamente executado o cmdlet Set-MsolDomainAuthentication para como ela tem não conhecimento do farm do AD FS e, portanto, tem controle total sobre quais os domínios que são convertidos e em que ordem.

Para compreender qual método deve utilizar, execute os passos na secção seguinte.

#### <a name="verify-current-user-sign-in-settings"></a>Certifique-se de início de sessão configurações do usuário atual

Verifique se o seu atual início de sessão definições do utilizador ao iniciar sessão no portal do Azure AD [ https://aad.portal.azure.com ](https://aad.portal.azure.com/) com uma conta de Administrador Global. 

Na **sessão do utilizador** secção, certifique-se de que **Federação** é **ativado** e que **o início de sessão único totalmente integrado** e  **Autenticação pass-through** estão **desativado**. 

![Figura 5](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image1.png)

#### <a name="verify-how-federation-was-configured"></a>Certifique-se de que forma o Federação foi configurada

   1. Aceda ao seu servidor do Azure AD Connect e o lançamento do Azure AD Connect, em seguida, selecione **configurar**.
   2. Sobre o **tarefas adicionais** ecrã, selecione **ver configuração atual** e, em seguida, selecione **seguinte**.</br>
   ![Figura 31](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image2.png)</br>
   3. Na **rever a sua solução** ecrã, desloque para baixo até **serviços de Federação do Active Directory (AD FS)**.</br>
   Se vir que a configuração do AD FS está nesta secção, em seguida, pode presumir com segurança do AD FS foi originalmente configurado através do Azure AD Connect e, por conseguinte, a conversão de seus domínios de federadas para gerido pode ser acionada por meio do Azure AD Connect  **Alterar a sessão do utilizador** opção, este processo é detalhado na secção **opção 1: Configuração da autenticação pass-through, utilizando o Azure AD Connect**.  
‎
   4. Se não pode ver os serviços de Federação do Active Directory, listado nas configurações atuais, em seguida, precisará converter manualmente os domínios de federado para geridos através do PowerShell que está detalhado na secção **opção 2 - comutador da Federação para PTA com o Azure AD Connect e o PowerShell**.

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


Validar as definições que poderão ter sido personalizadas para a documentação de design e implementação de Federação, especificamente **PreferredAuthenticationProtocol**, **SupportsMfa**, e  **PromptLoginBehavior**.

Mais informações sobre o que estas definições podem ser encontradas abaixo.

[Serviços de Federação do Active Directory solicitar = suporte de parâmetros de início de sessão](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-prompt-login)  
‎[Conjunto MsolDomainAuthentication](https://docs.microsoft.com/powershell/module/msonline/set-msoldomainauthentication?view=azureadps-1.0)

> [!NOTE]
> Se o valor de SupportsMfa está definido como "True", em seguida, isso significa que está a utilizar uma solução de MFA no local para injetar um desafio de fator 2nd o fluxo de autenticação de utilizador. Isso deixará de funcionar para cenários de autenticação do Azure AD e, em vez disso, terá de tirar partido do serviço do MFA do Azure (com base na cloud) para executar a mesma função. Cuidadosamente avaliar os seus requisitos de MFA antes de prosseguir e certifique-se de que compreende como tirar partido do MFA do Azure, as implicações de licenciamento e o processo de registo do utilizador final antes de converter seus domínios.

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
| Estão a bloquear os clientes de autenticação através do AD FS.| Considere substituir os controles deve bloquear clientes antigos de autenticação atualmente presentes no AD FS com uma combinação de [controla o acesso condicional para a autenticação de legado](https://docs.microsoft.com/azure/active-directory/conditional-access/conditions) e [acesso de cliente do Exchange Online Regras](https://aka.ms/EXOCAR). |
| Exigir que os usuários realizem MFA em relação a uma solução de servidor MFA no local durante a autenticação do AD FS.| Não será possível inserir uma submissão da MFA através da solução MFA no local para o fluxo de autenticação para um domínio gerido, no entanto, pode utilizar o serviço de MFA do Azure para fazê-lo daqui para frente uma vez o domínio é convertido. Se os utilizadores não estiver a utilizar hoje de MFA do Azure, em seguida, isso envolverá um passo de registo de utilizador final onetime que tem para preparar e comunicar aos seus utilizadores finais. |
| Utilize políticas de controlo de acesso (regras de AuthZ) hoje no AD FS para controlar o acesso ao Office 365.| Considere substitui-los com o equivalente do Azure AD [políticas de acesso condicional](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal) e [regras de acesso de cliente Online do Exchange](https://aka.ms/EXOCAR).|

### <a name="considerations-for-common-ad-fs-customizations"></a>Considerações comuns personalizações do AD FS

#### <a name="inside-corporate-network-claim"></a>Afirmação da rede empresarial interior

A afirmação InsideCorporateNetwork é emitida pelo AD FS, se a autenticação de utilizador é dentro da rede empresarial. Esta afirmação, em seguida, pode ser passada para o Azure AD e utilizada para ignorar o multi-factor authentication com base na localização de rede dos utilizadores. Ver [IPs fidedignos para utilizadores federados](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud) para obter informações sobre como determinar se tem isso atualmente ativada no AD FS.

A afirmação InsideCorporateNetwork não estarão disponível mais uma vez que os seus domínios são convertidos em autenticação pass-through. [Localizações com nome no Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-named-locations) pode ser utilizado para substituir esta funcionalidade.

Depois de configurar localizações com o nome, todas as políticas de acesso condicional configuradas para incluir ou excluir as localizações de rede "Todas as localizações fidedignas" ou "IPs fidedignos de MFA" tem de ser atualizado para refletir os locais com o nome criado recentemente.

Ver [Active Directory localizações acesso condicional do](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-locations) para obter mais informações sobre a condição de localização no acesso condicional.

#### <a name="hybrid-azure-ad-joined-devices"></a>Dispositivos do híbrida do Azure AD associado

Associar um dispositivo para o Azure AD permite-lhe criar regras de acesso condicional que impõem os dispositivos que cumprem as normas de acesso de segurança e conformidade e permite que os utilizadores iniciem sessão num dispositivo com uma conta escolar ou profissional de organizacional em vez de pessoal conta. Dispositivos de associados a um híbrido do Azure AD permite-lhe associar os dispositivos associados a um domínio do AD para o Azure AD. Seu ambiente federado poderá ter sido configurado com esta funcionalidade.

Para garantir a que associar híbrida continua a trabalhar para novos dispositivos associados ao domínio, uma vez que os seus domínios foram convertidos para a autenticação pass-through, o Azure AD Connect tem de ser configurado para sincronizar contas de computador do Active Directory para que os clientes Windows 10 O Azure AD. Para contas de computador do Windows 7 e Windows 8, Junte-se a híbrida irá utilizar o SSO totalmente integrado para registar o computador no Azure AD e não é necessário sincronizá-los tal como para dispositivos Windows 10. No entanto terá que implantar um arquivo de workplacejoin.exe atualizada (por meio de um ficheiro. msi) para estes clientes de nível inferior para que eles podem se registrar com o SSO totalmente integrado. [Transferir o. msi](https://www.microsoft.com/download/details.aspx?id=53554).

Para obter mais informações sobre este requisito, consulte [como configurar híbrida do Azure Active Directory associados a um dispositivos](https://docs.microsoft.com/azure/active-directory/device-management-hybrid-azuread-joined-devices-setup)

#### <a name="branding"></a>Personalização

Sua organização pode ter [personalizou sua páginas de início de sessão do AD FS](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-user-sign-in-customization) para apresentar informações mais pertinentes para a organização. Se assim for, considere fazer semelhante [personalizações para a página de início de sessão do Azure AD](https://docs.microsoft.com/azure/active-directory/customize-branding).

Embora personalizações semelhantes estejam disponíveis, devem esperar algumas alterações visual. Pode querer incluir alterações esperadas nas comunicações aos utilizadores finais.

> [!NOTE]
> Imagem corporativa da empresa está disponível apenas se comprou o Premium ou básica licença para o Azure AD ou ter uma licença do Office 365.

## <a name="planning-for-smart-lockout"></a>Planeamento de bloqueio inteligente

Bloqueio inteligente do Azure AD protege contra ataques de força bruta a senhas e impede que a conta do Active Directory no local que está a ser bloqueada quando está a ser utilizada a autenticação pass-through e uma política de grupo de bloqueio de conta é definida no Active Directory. 

Para obter mais informações sobre [funcionalidade de bloqueio inteligente e editar a respetiva configuração](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-smart-lockout).

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

Após a autenticação pass-through e Seamless SSO são implementadas, a experiência de início de sessão do utilizador final será alterado ao aceder ao Office 365 e outros recursos autenticados através do Azure AD de associados. Os utilizadores externos à rede agora verá a página do Azure AD início de sessão apenas, em oposição ao que está a ser redirecionado para a página baseada em formulários, apresentada por servidores do Proxy de aplicações Web com acesso externos.

Existem vários elementos para planejar sua estratégia de comunicação. Estas incluem:

* Notificar os utilizadores da funcionalidade de futuro e lançada por meio de
  * E-mail e outros canais de comunicação interna
  * Elementos visuais, tais como cartazes
  * Ao vivo ou outras comunicações
* Determinar quem irá personalizar e que enviará as comunicações e quando.

## <a name="implementing-your-solution"></a>Implementar a solução

Agora que tenha se planejado sua solução, está pronto para implementá-lo. Implementação inclui os seguintes componentes:

   1. Preparar para o início de sessão único totalmente integrado no
   2. Alterar início de sessão no método de autenticação pass-through e ativar o SSO totalmente integrado

## <a name="step-1--prepare-for-seamless-sso"></a>Passo 1 – preparar para SSO totalmente integrado

Para os seus dispositivos para utilizar o SSO totalmente integrado, terá de adicionar um URL de AD do Azure para as definições de zona de Intranet dos utilizadores ao utilizar a política de grupo no Active Directory.

Por predefinição, o navegador calcula automaticamente a zona correta, Internet ou Intranet, a partir de uma URL específica. Por exemplo, "http://contoso/"é mapeado para a zona da Intranet, ao passo que"http://intranet.contoso.com/" mapeia para a zona de Internet (porque o URL contém um período). Browsers não irão enviar os tíquetes Kerberos para um ponto final da cloud, como o URL de AD do Azure, a menos que explicitamente, adicione o URL para a zona de Intranet do navegador.

Siga os [passos para distribuir o](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-quick-start) as alterações necessárias para os seus dispositivos.

> [!IMPORTANT]
> Efetuar esta alteração não irá modificar a forma como os utilizadores iniciam sessão Azure AD. No entanto, é importante que esta configuração é aplicada a todos os seus dispositivos antes de continuar com a passo 3 também tenha em atenção que os utilizadores a iniciar sessão em dispositivos que não tenham recebido esta configuração simplesmente tem de introduzir o nome de utilizador e palavra-passe para iniciar sessão Azure AD.

## <a name="step-2--change-sign-in-method-to-pta-and-enable-seamless-sso"></a>Passo 2 – alterar método de início de sessão para PTA e ativar o SSO totalmente integrado

### <a name="option-a-configuring-pta-by-using-azure-ad-connect"></a>Opção a: Configurar o PTA através do Azure AD Connect

Utilize este método quando o AD FS foi inicialmente configurado com o Azure AD Connect. Não é possível utilizar este método se o AD FS não foi originalmente configurado com o Azure AD Connect.

> [!IMPORTANT]
> Lembre-se de que ao seguir os passos abaixo todos os seus domínios será convertido de federados para gerido. Reveja a secção Planear o método de migração para obter mais informações.[](#_Plan_Migration_Method)

Primeiro tem de alterar o método de início de sessão:

   1. Sobre o Azure AD ligar servidor, abra o assistente.
   2. Selecione **início de sessão de utilizador de alteração** e, em seguida, selecione **próxima**. 
   3. Na **ligar para o Azure AD** ecrã fornecer o nome de utilizador e palavra-passe de um Administrador Global.
   4. O **sessão do utilizador** ecrã, alterar o botão de opção de **federação com o AD FS** para **autenticação pass-through**, selecione **ativar início de sessão único**  , em seguida, selecione **próxima**.
   5. No ecrã de ativar o início de sessão único, introduza as credenciais da conta de administrador de domínio, em seguida, selecione avançar.  

   > [!NOTE]
   > As credenciais de administrador de domínio são necessárias para ativar o início de sessão único totalmente integrado, como o processo executa as seguintes ações que exigem essas permissões elevadas. As credenciais de administrador de domínio não são armazenadas no Azure AD Connect ou no Azure AD. Serem utilizados apenas para ativar a funcionalidade e, em seguida, rejeitados após a conclusão com êxito
   >
   > * É criada uma conta de computador com o nome AZUREADSSOACC (que representa o Azure AD) no seu local do Active Directory (AD).
   > * Chave de desencriptação do Kerberos da conta de computador é compartilhado de forma segura com o Azure AD.
   > * Além disso, os dois Kerberos principal nomes de serviço (SPNs) são criados para representar dois URLs que são utilizadas durante o início de sessão no Azure AD.
   > * As credenciais de administrador de domínio não são armazenadas no Azure AD Connect ou no Azure AD. Serem utilizados apenas para ativar a funcionalidade e, em seguida, rejeitados após a conclusão com êxito

   6. Na **pronto para configurar** ecrã, certifique-se **o processo de sincronização de iniciar quando tiver concluído a configuração** caixa de verificação está selecionada. Em seguida, selecione **configurar**.</br>
   ![imagem](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image8.png)</br>
   7. Abra o **portal do Azure AD**, selecione **Azure Active Directory**e, em seguida, selecione **do Azure AD Connect**.
   8. Certifique-se de que **Federação está desativada** enquanto **início de sessão único totalmente integrado** e **autenticaçãoPass-completa** são **ativado**.</br>
   ![imagem](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image9.png)</br>

Em seguida tem de implementar métodos de autenticação adicionais. Abra o **portal do Azure**, navegue até **Azure Active Directory, Azure AD Connect** e clique em **autenticação pass-through**.

Partir do **autenticação pass-through** página, clique no botão Download. Do **baixe** ecrã de agente, clique em **aceitar os termos e transferir**.

A transferência de agentes de autenticação adicional será iniciada. Instale o agente de autenticação secundária num servidor associado a um domínio. 

> [!NOTE]
> O primeiro agente é sempre instalado no servidor do Azure AD Connect em si como parte das alterações de configuração feitas na seção de início de sessão do utilizador da ferramenta Azure AD Connect. Quaisquer agentes de autenticação adicional deve ser instalados num servidor separado. Recomenda-se de ter entre 2 e 3 autenticação agentes adicionais disponíveis. 

Execute a instalação do agente de autenticação. Durante a instalação, terá de fornecer credenciais de uma **Administrador Global** conta.

![Imagem 1243067202](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image11.png)

![Imagem 1243067210](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image12.png)

Depois de instalar o agente de autenticação, pode voltar à página de estado de funcionamento do agente de autenticação pass-through para verificar o estado dos agentes adicionais.


Vá para testes e próximos passos.

> [!IMPORTANT]
> Ignorar a seção opção b: Mudar da Federação PTA através da utilização do Azure AD Connect e o PowerShell como os passos em que secção não se aplicam.  

### <a name="option-b---switch-from-federation-to-pta-using-azure-ad-connect-and-powershell"></a>Opção B - comutador da Federação para PTA com o Azure AD Connect e o PowerShell

Utilize esta opção quando a Federação não foi configurada inicialmente com o Azure AD Connect. Primeiro tem de ativar autenticação pass-through:

   1. Sobre o Azure AD ligar servidor, abra o assistente.
   2. Selecione **início de sessão de utilizador de alteração** e, em seguida, selecione **próxima**.
   3. Na **ligar para o Azure AD** ecrã fornecer o nome de utilizador e palavra-passe de um Administrador Global.
   4. Sobre o **sessão do utilizador** ecrã, alterar o botão de opção de **não configure** para **autenticação pass-through**, selecione **ativar início de sessão único** , em seguida, selecione **próxima**.
   5. Na **ativar o início de sessão único** ecrã, introduza as credenciais da conta de administrador de domínio, em seguida, selecione **próxima**.

   > [!NOTE]
   > As credenciais de administrador de domínio são necessárias para ativar o início de sessão único totalmente integrado, como o processo executa as seguintes ações que exigem essas permissões elevadas. As credenciais de administrador de domínio não são armazenadas no Azure AD Connect ou no Azure AD. Estão utilizadas apenas para ativar a funcionalidade e, em seguida, rejeitados após a conclusão com êxito.
   >
   > * É criada uma conta de computador com o nome AZUREADSSOACC (que representa o Azure AD) no seu local do Active Directory (AD).
   > * Chave de desencriptação do Kerberos da conta de computador é compartilhado de forma segura com o Azure AD.
   > * Além disso, os dois Kerberos principal nomes de serviço (SPNs) são criados para representar dois URLs que são utilizadas durante o início de sessão no Azure AD.

   6. Na **pronto para configurar** ecrã, certifique-se **o processo de sincronização de iniciar quando tiver concluído a configuração** caixa de verificação está selecionada. Em seguida, selecione **configurar**.</br>
   ‎![imagem](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image18.png)</br>
   Ao selecionar a configurar, ocorrem os seguintes passos:
   * O primeiro agente de autenticação pass-through está instalado
   * A funcionalidade de pass-through está ativada
   * Totalmente integrado início de sessão único está ativado.  
   7. Certifique-se de que **Federação** ainda está **ativado** e **totalmente integrada início de sessão único** e **autenticaçãoPass-completa** agora estão ativadas.
   ![Figura 2](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image19.png)
   8. Selecione **autenticação pass-through** e certifique-se de que o estado é **Active Directory**.</br>
   
   Se o agente de autenticação não estiver ativo, siga [estes passos de resolução de problemas](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-pass-through-authentication) antes de continuar com o processo de conversação de domínio no próximo passo. Corre o risco de causar uma falha de autenticação se converter seus domínios antes de a validar que os agentes de PTA instalaram com êxito e que o respetivo estado é apresentado como "Ativo" no portal do Azure.  
   9. Em seguida, implante agentes de autenticação adicional. Abra o **portal do Azure**, navegue até **Azure Active Directory**, **do Azure AD Connect** e clique em **autenticação pass-through**.
   10. Partir do **autenticação pass-through** página, clique no **transferir** botão. Do **transferir o agente** ecrã, clique em **aceitar os termos e transferir**.
   
   A transferência de agentes de autenticação adicional será iniciada. Instale o agente de autenticação secundária num servidor associado a um domínio.

  > [!NOTE]
  > O primeiro agente é sempre instalado no servidor do Azure AD Connect em si como parte das alterações de configuração feitas na seção de início de sessão do utilizador da ferramenta Azure AD Connect. Quaisquer agentes de autenticação adicional deve ser instalados num servidor separado. Recomenda-se de ter entre 2 e 3 autenticação agentes adicionais disponíveis.
  
   11. Execute a instalação do agente de autenticação. Durante a instalação, terá de fornecer credenciais de uma **Administrador Global** conta.</br>
   ![Imagem 1243067215](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image23.png)</br>
   ![Imagem 1243067216](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image24.png)</br>
   12. Depois de instalar o agente de autenticação, pode voltar à página de estado de funcionamento do agente de autenticação pass-through para verificar o estado dos agentes adicionais.

Neste momento, Federação ainda está ativado e operacional dos seus domínios. Para continuar com a implementação, cada domínio tem de ser convertidos de federados para gerida para que a autenticação pass-through começa a servir pedidos de autenticação para o domínio.

Nem todos os domínios precisam a ser convertido ao mesmo tempo, pode escolher para começar um domínio de teste no seu inquilino de produção ou de domínio com o menor número de utilizadores.

A conversão é efetuada utilizando o módulo do PowerShell do Azure AD.

   1. Open **PowerShell** e início de sessão do Azure AD com um **Administrador Global** conta.
   2. Para converter o primeiro domínio, execute o seguinte comando:
 
 ``` PowerShell
 Set-MsolDomainAuthentication -Authentication Managed -DomainName <domainname>
 ```
 
   3. Abra o **portal do Azure AD**, selecione **Azure Active Directory**e, em seguida, selecione **do Azure AD Connect**.  
   4. Depois de ter convertido a todos os domínios federados, certifique-se de que **Federação está desativada** enquanto **totalmente integrada início de sessão único** e **autenticação pass-through** são  **Ativado**.</br>
   ![imagem](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image26.png)</br>

## <a name="testing-and-next-steps"></a>Passos de testes e seguintes

### <a name="test-pass-through-authentication"></a>Autenticação pass-through de teste 

Quando o seu inquilino estava a utilizar o Federação, os utilizadores foram redirecionados da página de início de sessão do Azure AD para o seu ambiente do AD FS. Agora que o inquilino está configurado para utilizar autenticação pass-through em vez de Federação, os utilizadores não irão obter redirecionados para o AD FS e em vez disso, irão iniciar sessão diretamente através da página de início de sessão do Azure AD.

Abra o Internet Explorer no modo InPrivate para evitar o SSO totalmente integrado a iniciar sessão automaticamente e vá para a página de início de sessão do Office 365 ([https://portal.office.com](https://portal.office.com/)). Tipo de **UPN** do seu utilizador e clique em **próxima**. Certifique-se para o tipo de UPN do utilizador híbrido que foi sincronizado do Active Directory no local e que foi anteriormente federado. O utilizador verá a tela de digitar seu nome de utilizador e palavra-passe.

![Figura 18](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image27.png)

![Figura 19](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image28.png)

Depois de digitar a palavra-passe, deve obter redirecionado para o portal do Office 365.

![Figura 23](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image29.png)

### <a name="test-seamless-single-sign-on"></a>Testar totalmente integrada início de sessão único

   1. Início de sessão a um domínio associado a um computador que está ligado à rede empresarial. 
   2. Open **Internet Explorer** ou **Chrome** e aceda a um dos seguintes URLs:   
      ‎  
      ‎[https://myapps.microsoft.com/contoso.com](https://myapps.microsoft.com/contoso.com) [https://myapps.microsoft.com/contoso.onmicrosoft.com](https://myapps.microsoft.com/contoso.onmicrosoft.com) (substitua Contoso com o seu domínio).

      O utilizador será redirecionado rapidamente para a página de início de sessão do Azure AD e ver a mensagem "A tentar iniciar sessão" e não devem ser solicitados para um nome de utilizador ou uma palavra-passe.</br>
   ![Imagem 24](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image30.png)</br>
   3. Em seguida, o utilizador irá obter redirecionado e iniciou sessão no painel de acesso com êxito:

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

## <a name="roll-over-the-seamless-sso-kerberos-decryption-key"></a>Sobre a chave de desencriptação do Kerberos do SSO totalmente integrada

É importante frequentemente rollover de chave de desencriptação Kerberos da conta de computador AZUREADSSOACC (que representa o Azure AD) criado no seu local floresta do AD. Recomendamos vivamente que efetue o rollover da chave de desencriptação do Kerberos, pelo menos, a cada 30 dias para se alinhar com como membros do domínio do Active Directory submeter as alterações de palavra-passe. Porque não existe nenhum dispositivo associado anexado ao objeto de conta de computador AZUREADSSOACC o rollover tem de ser executados manualmente.

Siga estes passos no servidor no local onde está a executar o Azure AD Connect, para iniciar o rollover da chave de desencriptação do Kerberos.

[Como posso reverter sobre a chave de desencriptação do Kerberos da conta de computador AZUREADSSOACC](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-faq)?

## <a name="monitoring-and-logging"></a>Monitorização e registos

Os servidores que executam os agentes de autenticação devem ser monitorizados para manter a disponibilidade da solução. Além dos contadores de desempenho geral do servidor, os agentes de autenticação expor objetos de desempenho que podem ser utilizados para compreender as estatísticas de autenticação e erros.

Agentes de autenticação de registo operações em registos de eventos do Windows em "Aplicação e serviço Logs\Microsoft\AzureAdConnect\AuthenticationAgent\Admin".

Registos de resolução de problemas pode ser ativada se for necessário.

Ver mais informações [monitorização e registo](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-Pass-through-authentication).

## <a name="next-steps"></a>Passos Seguintes

- [Conceitos de design do Azure AD Connect](plan-connect-design-concepts.md)
- [Escolha a autenticação correta](https://docs.microsoft.com/azure/security/azure-ad-choose-authn)
- [Topologias suportadas](plan-connect-design-concepts.md)
