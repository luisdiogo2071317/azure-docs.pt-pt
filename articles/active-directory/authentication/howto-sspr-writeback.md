---
title: O Azure AD SSPR com repetição de escrita de palavra-passe | Documentos da Microsoft
description: Utilize o Azure AD e o Azure AD Connect para palavras-passe de repetição de escrita para um diretório no local
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: fce92845591f20f7f2e9cff1a856e0904629255b
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054799"
---
# <a name="password-writeback-overview"></a>Descrição geral de repetição de escrita de palavra-passe

Com a repetição de escrita de palavra-passe, pode configurar o Azure Active Directory (Azure AD) para gravar palavras-passe no Active Directory no local. Repetição de escrita de palavra-passe remove a necessidade de configurar e gerir uma solução de (SSPR) de reposição de palavra-passe self-service complicado no local e fornece uma forma conveniente com base na cloud para que os utilizadores reponham as respetivas palavras-passe no local onde quer que estejam. Repetição de escrita de palavra-passe é um componente do [Azure Active Directory Connect](./../connect/active-directory-aadconnect.md) que pode ser habilitado e utilizado por subscritores atuais do Premium [edições do Azure Active Directory](../fundamentals/active-directory-whatis.md).

Repetição de escrita de palavra-passe fornece as seguintes funcionalidades:

* **Fornece comentários de atraso de zero**: repetição de escrita de palavra-passe é uma operação síncrona. Os utilizadores são notificados imediatamente se a palavra-passe não cumpria a política ou não foi possível ser repor ou alterada por qualquer motivo.
* **Redefinições de senha de oferece suporte para os utilizadores que utilizam os serviços de Federação do Active Directory (AD FS) ou outras tecnologias de Federação**: com repetição de escrita de palavra-passe, desde que as contas de utilizador federado são sincronizadas para o inquilino do Azure AD, eles são capazes de gerir as palavras-passe no local do Active Directory na nuvem.
* **Redefinições de senha de oferece suporte para os utilizadores que utilizam** [sincronização de hash de palavra-passe](./../connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md): quando o serviço de reposição de palavra-passe Deteta que uma conta de utilizador sincronizado está ativada para sincronização de hash de palavra-passe, podemos repor para este local da conta e a palavra-passe de cloud em simultâneo. Repetição de escrita de palavra-passe não depende de sincronização de hash de palavra-passe.
* **Redefinições de senha de oferece suporte para os utilizadores que utilizam a autenticação pass-through**: com repetição de escrita de palavra-passe, desde que as contas de autenticação pass-through são sincronizadas para o inquilino do Azure AD, eles são capazes de gerir os seus ativos no local Palavras-passe da cloud.
* **Palavra-passe de suporta muda de painel de acesso e do Office 365**: quando federadas ou utilizadores de palavra-passe sincronizada são fornecidos para alterar suas senhas expiradas ou não expirada, podemos escrever essas palavras-passe de volta ao seu ambiente do Active Directory local.
* **Oferece suporte a repetição de escrita de palavras-passe quando um administrador repõe-los a partir do portal do Azure**: sempre que um administrador repõe a palavra-passe de um usuário na [portal do Azure](https://portal.azure.com), se estiver federado que o utilizador ou palavra-passe sincronizada, vamos definir a palavra-passe o administrador seleciona também do Active Directory local. Esta funcionalidade não é atualmente suportada no portal de administração do Office.
* **Impõe as políticas de palavra-passe do Active Directory no local**: quando um utilizador repõe a palavra-passe, podemos Certifique-se de que cumpre a política do Active Directory no local antes vamos confirmar esse diretório. Esta revisão inclui a verificação do histórico, complexidade, idade, filtros de palavra-passe e outras restrições de palavra-passe que definiu no local Active Directory.
* **Não requer quaisquer regras de firewall de entrada**: repetição de escrita de palavra-passe utiliza um reencaminhamento do Service bus do Azure como um canal de comunicação subjacente. Não tem de abrir qualquer portas no seu firewall para esta funcionalidade funcione.
* **Não é suportada para contas de utilizador que existem em grupos protegidos no Active Directory no local**: para obter mais informações sobre grupos protegidos, consulte [protegidos a contas e grupos no Active Directory](https://technet.microsoft.com/library/dn535499.aspx).

## <a name="how-password-writeback-works"></a>Como funciona a repetição de escrita de palavra-passe

Quando um hash de palavra-passe ou federado utilizador sincronizado vem repor ou alterar a palavra-passe na cloud, ocorre o seguinte:

1. Podemos verificar que tipo de palavra-passe tem do utilizador. Se podemos ver que a palavra-passe é geridos no local:
   * Verificamos se o serviço de repetição de escrita está em execução. Se estiver em execução, vamos permitir que o utilizador a que se continuar.
   * Se o serviço de repetição de escrita não está disponível, podemos informar ao usuário que a palavra-passe não pode ser reposta neste momento.
2. Em seguida, o utilizador passa as portas de autenticação adequado e alcance o **Repor palavra-passe** página.
3. O usuário seleciona uma nova palavra-passe e confirma isso.
4. Quando o utilizador seleciona **submeter**, podemos criptografar a palavra-passe de texto sem formatação com uma chave simétrica que foi criada durante o processo de configuração da repetição de escrita.
5. Depois de criptografar a senha, podemos incluí-lo numa carga que é enviada por um canal HTTPS para o reencaminhamento de barramento de serviço específico de inquilino (que também configuramos para durante o processo de configuração da repetição de escrita). Este reencaminhamento está protegido por uma palavra-passe gerada aleatoriamente que sabe apenas sua instalação no local.
6. Depois que a mensagem de atinge o service bus, o ponto final de reposição de palavra-passe reativado automaticamente e vê que ele tem um pedido de reposição pendente.
7. O serviço de procura, em seguida, o utilizador, utilizando o atributo de âncora de cloud. Para esta pesquisa com êxito:

    * O objeto de utilizador tem de existir no espaço conector do Active Directory.
    * O objeto de utilizador têm de estar associado ao objeto de metaverso (MV) correspondente.
    * O objeto de utilizador têm de estar associado ao objeto de conector correspondente do Azure Active Directory.
    * A ligação de objeto de conector do Active Directory para a MV tem de ter a regra de sincronização `Microsoft.InfromADUserAccountEnabled.xxx` na ligação. <br> <br>
    Quando chega a chamada da cloud, o motor de sincronização utiliza a **cloudAnchor** atributo para procurar o objeto de espaço conector do Azure Active Directory. Em seguida, segue-se a ligação para o objeto de MV e, em seguida, segue-se a ligação para o objeto do Active Directory. Como pode haver vários objetos do Active Directory (multifloresta) para o mesmo utilizador, o motor de sincronização depende o `Microsoft.InfromADUserAccountEnabled.xxx` ligação para a escolha correta.

    > [!Note]
    > Como resultado, essa lógica de palavra-passe de repetição de escrita para trabalhar do Azure AD Connect tem de ser capaz de comunicar com o emulador do controlador (PDC) de domínio primário. Se precisar de ativar esta opção manualmente, pode ligar o Azure AD Connect para o emulador PDC. Com o botão direito a **propriedades** do conector de sincronização do Active Directory e, em seguida, selecione **configurar partições de diretório**. A partir daí, procure o **definições de ligação do controlador de domínio** secção e selecione a caixa intitulada **utilizar apenas controladores de domínio preferencial**. Mesmo que o controlador de domínio preferencial não for um emulador PDC, o Azure AD Connect tenta se conectar com o PDC para repetição de escrita de palavra-passe.

8. Depois que o usuário é encontrada a conta, vamos tentar repor a palavra-passe diretamente na floresta do Active Directory adequada.
9. Se a operação de definição de palavra-passe for bem-sucedida, podemos dizer o utilizador que a palavra-passe foi alterada.
    > [!NOTE]
    > Se a senha do usuário é sincronizada com o Azure AD utilizando a sincronização de palavra-passe, é provável que a política de palavra-passe no local é mais fraca do que a política de palavra-passe na cloud. Neste caso, estamos ainda impor que quer que a política no local é em vez disso, sincronização de hash de palavra-passe para sincronizar o hash dessa palavra-passe. Esta política assegura que seus locais é imposta na cloud, quer se utilizar a sincronização de palavra-passe ou Federação para fornecer início de sessão único.

10. Se a palavra-passe definida a operação falhar, vamos retornar um erro ao usuário e informá-los, tente novamente. A operação pode falhar porque:
    * O serviço estava.
    * A palavra-passe que selecionou não cumpria as políticas da organização.
    * Não pode possível localizar o utilizador no local Active Directory.

    Temos uma mensagem específica para muitos desses casos e informar ao usuário, o que podem fazer para resolver o problema.

## <a name="configure-password-writeback"></a>Configurar a repetição de escrita de palavra-passe

Recomendamos que utilize a funcionalidade de atualização automática do [do Azure AD Connect](./../connect/active-directory-aadconnect-get-started-express.md) se pretender utilizar a repetição de escrita de palavra-passe.

O DirSync e o Azure AD Sync, já não são suportadas como uma forma de ativar a repetição de escrita de palavra-passe. Para obter mais informações ajudar na sua transição, consulte [atualizar do DirSync e Azure AD Sync](../connect/active-directory-aadconnect-dirsync-deprecated.md).

Os passos seguintes partem do princípio de já ter configurado o Azure AD Connect no seu ambiente utilizando o [Express](./../connect/active-directory-aadconnect-get-started-express.md) ou [personalizado](./../connect/active-directory-aadconnect-get-started-custom.md) definições.

1. Para configurar e ativar a repetição de escrita de palavra-passe, inicie sessão no seu servidor do Azure AD Connect e iniciar o **do Azure AD Connect** Assistente de configuração.
2. Sobre o **bem-vindo** página, selecione **configurar**.
3. Sobre o **tarefas adicionais** página, selecione **personalizar as opções de sincronização**e, em seguida, selecione **seguinte**.
4. Sobre o **ligar para o Azure AD** página, introduza uma credencial de administrador global e, em seguida, selecione **próxima**.
5. Sobre o **ligar diretórios** e **domínio/UO** filtragem de páginas, selecione **seguinte**.
6. Sobre o **funcionalidades opcionais** página, selecione a caixa junto a **repetição de escrita de palavra-passe** e selecione **seguinte**.
   ![Ativar a repetição de escrita de palavra-passe no Azure AD Connect][Writeback]
7. Sobre o **pronto para configurar** página, selecione **configurar** e aguarde o conclusão do processo.
8. Quando vir a configuração concluir, selecione **saída**.

Para tarefas de resolução de problemas comuns relacionados com a repetição de escrita de palavra-passe, consulte a secção [resolver problemas de repetição de escrita de palavra-passe](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) no nosso artigo de resolução de problemas.

## <a name="active-directory-permissions"></a>Permissões do Active Directory

A conta especificada no utilitário do Azure AD Connect tem os seguintes itens definiu se quiser ser no âmbito do SSPR:

* **Repor palavra-passe** 
* **Alterar palavra-passe** 
* **Permissões de escrita** no `lockoutTime`  
* **Permissões de escrita** no `pwdLastSet`
* **Expandido direitos** em qualquer uma:
   * O objeto raiz de *cada domínio* nessa floresta
   * As unidades organizacionais (UOs) de utilizador que pretende no escopo para SSPR

Se não tiver a certeza sobre a conta a conta descrita refere-se, abra a IU de configuração do Azure Active Directory Connect e selecione o **ver configuração atual** opção. A conta que tem de adicionar permissão está listado em **diretórios sincronizados**.

Se definir estas permissões, a conta de serviço MA para cada floresta pode gerir as palavras-passe em nome das contas de utilizador nessa floresta. 

> [!IMPORTANT]
> Se não atribuir estas permissões, em seguida, mesmo que a repetição de escrita parece estar configurada corretamente, os utilizadores receberão erros quando tentarem gerir as respetivas palavras-passe no local na nuvem.
>

> [!NOTE]
> Pode demorar até uma hora ou mais até que estas permissões sejam replicadas para todos os objetos no seu diretório.
>

Para configurar as permissões adequadas para a repetição de escrita de palavra-passe ocorrer, conclua os seguintes passos:

1. Abra o Active Directory utilizadores e computadores com uma conta que possua as permissões de administração do domínio adequado.
2. Do **View** menu, certifique-se **funcionalidades avançadas** está ativada.
3. No painel esquerdo, clique no objeto que representa a raiz do domínio e selecione **propriedades** > **segurança** > **avançadas**.
4. Partir do **permissões** separador, selecione **Add**.
5. Escolha a conta que as permissões estão a ser aplicadas a (a partir do programa de configuração do Azure AD Connect).
6. Na **aplica-se ao** na lista pendente, selecione **utilizadores descendentes** objetos.
7. Sob **permissões**, selecione as caixas para o seguinte:
    * **Repor palavra-passe**
    * **Alterar palavra-passe**
    * **Escrever lockoutTime**
    * **Escrever pwdLastSet**
8. Selecione **aplicar/OK** para aplicar as alterações e sair quaisquer caixas de diálogo de abertura.

## <a name="licensing-requirements-for-password-writeback"></a>Requisitos de licenciamento para a repetição de escrita de palavra-passe

Para obter informações sobre o licenciamento, consulte [licenças necessárias para a repetição de escrita de palavra-passe](concept-sspr-licensing.md) ou os seguintes sites:

* [O Azure Active Directory, preços do site](https://azure.microsoft.com/pricing/details/active-directory/)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Enterprise](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

### <a name="on-premises-authentication-modes-that-are-supported-for-password-writeback"></a>Modos de autenticação no local, que são suportados para repetição de escrita de palavra-passe

Suporte de repetição de escrita de palavra-passe para os seguintes tipos de palavra-passe de utilizador:

* **Os utilizadores apenas na cloud**: repetição de escrita de palavra-passe não se aplica esta situação, porque não existe nenhuma palavra-passe no local.
* **Os utilizadores de palavra-passe sincronizada**: repetição de escrita de palavra-passe é suportada.
* **Os utilizadores federados**: repetição de escrita de palavra-passe é suportada.
* **Os utilizadores de autenticação pass-through**: repetição de escrita de palavra-passe é suportada.

### <a name="user-and-admin-operations-that-are-supported-for-password-writeback"></a>Operações de utilizador e administrador que são suportadas para repetição de escrita de palavra-passe

As palavras-passe são repetidas nas seguintes situações:

* **Operações do utilizador final suportados**
  * Operação de palavra-passe de alterar qualquer voluntária pelo utilizador final de self-service
  * Operação de palavra-passe, por exemplo, expiração de palavra-passe de alterar qualquer força do utilizador final self-service
  * Qualquer utilizador final Self-reposição palavra-passe que são originados pelo [portal de reposição de palavra-passe](https://passwordreset.microsoftonline.com)
* **Operações de administrador suportados**
  * Operação de palavra-passe de alterar qualquer voluntária administrador de self-service
  * Operação de palavra-passe, por exemplo, expiração de palavra-passe de alterar qualquer força de self-service de administrador
  * Reposição de qualquer senha de autoatendimento de administrador que são originados pelo [portal de reposição de palavra-passe](https://passwordreset.microsoftonline.com)
  * Qualquer palavra-passe de utilizador final iniciadas pelo administrador de reposição do [portal do Azure](https://portal.azure.com)

### <a name="user-and-admin-operations-that-are-not-supported-for-password-writeback"></a>Operações de utilizador e administrador que não são suportadas para repetição de escrita de palavra-passe

Palavras-passe são *não* repetição de escrita em qualquer uma das seguintes situações:

* **Operações do utilizador final não suportado**
  * Qualquer utilizador final a repor a sua própria palavra-passe com o PowerShell versão 1, versão 2 ou o Azure AD Graph API
* **Operações de administrador não suportado**
  * Qualquer palavra-passe de utilizador final iniciadas pelo administrador de reposição do [portal de gestão do Office](https://portal.office.com)
  * Qualquer palavra-passe de utilizador final iniciadas pelo administrador de reposição da versão 1, versão 2 ou o Azure AD Graph API do PowerShell

Estamos a trabalhar para remover essas limitações, mas não temos uma linha de tempo específica que partilhamos ainda.

## <a name="password-writeback-security-model"></a>Modelo de segurança de repetição de escrita de palavra-passe

Repetição de escrita de palavra-passe é um serviço altamente seguro. Para garantir que suas informações estão protegidas, ativamos um modelo de segurança em camadas quatro descrita da seguinte forma:

* **Reencaminhamento do barramento de serviço específico de inquilino**
  * Quando configurar o serviço, criamos um reencaminhamento de barramento de serviço específico de inquilino que está protegido por uma senha forte gerada aleatoriamente, que a Microsoft nunca tem acesso a.
* **Chave de encriptação da palavra-passe bloqueado e criptograficamente fortes,**
  * Depois de criar reencaminhamento do service bus, podemos criar uma chave simétrica forte que usamos para criptografar a palavra-passe, pois isso vem durante a transmissão. Esta chave só reside no arquivo de segredos da sua empresa na cloud, que é bastante bloqueado e auditada, assim como qualquer outra palavra-passe no diretório.
* **Setor padrão Transport Layer Security (TLS)**
  1. Quando uma palavra-passe, repor ou alterar a operação ocorre na cloud, pegamos a palavra-passe de texto sem formatação e criptografá-la com a chave pública.
  2. Vamos colocar isso numa mensagem de HTTPS, que é enviada por um canal criptografado, através de certificados da Microsoft SSL para o reencaminhamento do service bus.
  3. Depois da mensagem chega no barramento de serviço, o agente no local é reativado e autentica para o service bus, utilizando a palavra-passe forte que foi gerada anteriormente.
  4. O agente no local seleciona a mensagem encriptada e desencripta a mesma, utilizando a chave privada gerada.
  5. O agente no local tenta definir a palavra-passe através da API de SetPassword do AD DS. Este passo é o que permite-nos para impor a política de palavra-passe do Active Directory no local (por exemplo, a complexidade, idade, histórico e filtros) na cloud.
* **Políticas de expiração de mensagem** 
  * Se a mensagem encontra-se no barramento de serviço porque o serviço no local está indisponível, exceder o tempo limite e é removido após alguns minutos. O limite de tempo e a remoção da mensagem aumenta ainda mais a segurança.

### <a name="password-writeback-encryption-details"></a>Detalhes de encriptação de repetição de escrita de palavra-passe

Depois de um utilizador submete uma reposição de palavra-passe, o pedido de reposição passa por vários passos de encriptação antes de chegarem no seu ambiente no local. Estes passos de encriptação garantem a fiabilidade do serviço máximo e a segurança. Estes modelos são descritos da seguinte forma:

* **Passo 1: Encriptação de palavra-passe com a chave RSA de 2048 bits**: depois de um utilizador submete uma palavra-passe para a repetição de escrita no local, a palavra-passe submetida em si é encriptado com uma chave RSA de 2048 bits.
* **Passo 2: Encriptação de nível de pacote com o AES-GCM**: todo o pacote, a palavra-passe e os metadados necessários, ainda é encriptado utilizando AES-GCM. Esta encriptação impede que qualquer pessoa com acesso direto para o canal de ServiceBus subjacente visualizar ou violação com o conteúdo.
* **Passo 3: Toda a comunicação ocorre através de TLS/SSL**: toda a comunicação com o ServiceBus ocorre num canal SSL/TLS. Essa criptografia protege o conteúdo de terceiros não autorizados.
* **Rollover de chave automático a cada seis meses**: a cada seis meses ou cada repetição de escrita de palavra-passe de tempo está desativada e, em seguida, novamente, ativada no Azure AD Connect. Podemos fazer o roll over todas as chaves para garantir a segurança do serviço máximo e a segurança.

### <a name="password-writeback-bandwidth-usage"></a>Utilização de largura de banda de repetição de escrita de palavra-passe

Repetição de escrita de palavra-passe é um serviço de baixa largura de banda que apenas envia pedidos para o agente no local nas seguintes circunstâncias:

* Duas mensagens são enviadas quando o recurso está ativado ou desativado através do Azure AD Connect.
* Uma mensagem é enviada uma vez a cada cinco minutos como um heartbeat do serviço para, desde que o serviço está em execução.
* Duas mensagens são enviadas a cada hora é submetida uma nova palavra-passe:
    * A primeira mensagem é um pedido para executar a operação.
    * A segunda mensagem contém o resultado da operação e é enviada nas seguintes circunstâncias:
        * Sempre que uma nova palavra-passe é submetida durante uma reposição de palavra-passe self-service do utilizador.
        * Sempre que uma nova palavra-passe é submetida durante uma operação de alteração de palavra-passe do utilizador.
        * Sempre que uma nova palavra-passe é submetida durante uma palavra-passe de utilizador iniciadas pelo administrador de reposição (apenas a partir de portais de administração do Azure).

#### <a name="message-size-and-bandwidth-considerations"></a>Considerações de largura de banda e de tamanho de mensagem

O tamanho de cada da mensagem descrita anteriormente, normalmente, é menos de 1 KB. Mesmo que sob cargas extremas, o próprio serviço de repetição de escrita de palavra-passe está a consumir alguns kilobits por segundo de largura de banda. Uma vez que cada mensagem é enviada em tempo real, apenas quando necessário por uma operação de atualização de palavra-passe e porque o tamanho da mensagem é tão pequeno, a utilização de largura de banda da capacidade de repetição de escrita é demasiado pequena para ter um impacto considerável.

## <a name="next-steps"></a>Passos Seguintes

* [Como posso concluir uma implementação com êxito da SSPR?](howto-sspr-deployment.md)
* [Reponha ou altere a palavra-passe](../user-help/active-directory-passwords-update-your-own-password.md).
* [Registe-se na reposição personalizada de palavras-passe](../user-help/active-directory-passwords-reset-register.md).
* [Tem alguma pergunta sobre licenciamento?](concept-sspr-licensing.md)
* [Que dados são utilizados pela SSPR e que dados devem ser preenchidos por si para os seus utilizadores?](howto-sspr-authenticationdata.md)
* [Que métodos de autenticação estão disponíveis para os utilizadores?](concept-sspr-howitworks.md#authentication-methods)
* [Quais são as opções de política da SSPR?](concept-sspr-policy.md)
* [Como posso comunicar a atividade da SSPR?](howto-sspr-reporting.md)
* [Quais são todas as opções na SSPR e o que significam?](concept-sspr-howitworks.md)
* [Creio que algo está a funcionar incorretamente. Como posso resolver problemas da SSPR?](active-directory-passwords-troubleshoot.md)
* [Tenho uma pergunta que ainda não foi abordada](active-directory-passwords-faq.md)

[Writeback]: ./media/howto-sspr-writeback/enablepasswordwriteback.png "Ativar a repetição de escrita de palavra-passe no Azure AD Connect"
