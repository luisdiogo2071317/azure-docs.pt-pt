---
title: Como funciona - Azure Active Directory de reposição de palavra-passe self-service
description: Aprofunde-se de redefinição de senha de autoatendimento de AD do Azure
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: f2d0b009c4451a4108222ac7aa7954ba6dd86699
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37869012"
---
# <a name="self-service-password-reset-in-azure-ad-deep-dive"></a>Senhas de auto-atendimento Repor no aprofundamento do Azure AD

Como a palavra-passe self-service reposição trabalho (SSPR)? O que significa essa opção na interface do? Continue a ler para obter mais informações sobre a SSPR do Azure Active Directory (Azure AD).

## <a name="how-does-the-password-reset-portal-work"></a>Como a palavra-passe portal de reposição?

Quando um usuário Ir para o portal de reposição de palavra-passe, um fluxo de trabalho é inicializado para determinar:

   * Como deve ser localizada a página?
   * É a conta de utilizador válido?
   * O que organização o utilizador pertence ao?
   * Em que é gerida a senha do usuário?
   * O utilizador está licenciado para utilizar a funcionalidade?

Página de reposição de leitura, os seguintes passos para saber mais sobre a lógica por trás da palavra-passe:

1. O utilizador seleciona o **não é possível aceder à sua conta** ligar ou vai diretamente para [ https://aka.ms/sspr ](https://passwordreset.microsoftonline.com).
   * Com base na localidade do navegador, a experiência é renderizada no idioma apropriado. A experiência de reposição de palavra-passe é localizada para os mesmos idiomas que suporta o Office 365.
2. O usuário digita um ID de utilizador e passa um captcha.
3. O Azure AD verifica que o utilizador é capaz de utilizar esta funcionalidade, efetuando as seguintes verificações:
   * Verifica se o utilizador tem esta funcionalidade ativada e tem um Azure AD licença atribuída.
     * Se o utilizador não tenha esta funcionalidade ativada ou ter uma licença atribuída, é pedido ao utilizador que contacte o seu administrador para repor a palavra-passe.
   * Verifica se o utilizador tem o direito desafiar definidos na respetiva conta de acordo com a política do administrador de dados.
     * Se a política requer apenas um desafio, em seguida, ele garante que o utilizador tem os dados apropriados definidos para, pelo menos, um dos desafios ativados pela política de administrador.
       * Se o desafio de utilizador não estiver configurado, o utilizador recomenda que contacte o seu administrador para repor a palavra-passe.
     * Se a política requer dois desafios, em seguida, ele garante que o utilizador tem os dados apropriados definidos para, pelo menos, dois dos desafios ativados pela política de administrador.
       * Se o desafio de utilizador não estiver configurado, o utilizador recomenda que contacte o seu administrador para repor a palavra-passe.
   * Verifica se a senha do usuário é gerida no local (autenticação federada de pass-through ou sincronizados de hash de palavra-passe).
     * Se a repetição de escrita é implementada e a senha do usuário é geridos no local, em seguida, o utilizador tem permissão para continuar para autenticar e repor a palavra-passe.
     * Se a repetição de escrita não estiver implementada e a senha do usuário é geridos no local, é pedido ao utilizador que contacte o seu administrador para repor a palavra-passe.
4. Se for determinado que o utilizador é capaz de reposta com êxito a palavra-passe, o utilizador é orientado através do processo de reposição.

## <a name="authentication-methods"></a>Métodos de autenticação

Se estiver ativada a SSPR, tem de selecionar pelo menos uma das seguintes opções para os métodos de autenticação. Às vezes ouvir essas opções referidas como "gates." É altamente recomendável que escolha, pelo menos, dois métodos de autenticação para que os utilizadores têm mais flexibilidade.

* Email
* Número de telemóvel
* Telefone do escritório
* Perguntas de segurança

![Autenticação][Authentication]

### <a name="what-fields-are-used-in-the-directory-for-the-authentication-data"></a>Os campos são utilizados no diretório para os dados de autenticação?

* **Telefone do escritório**: corresponde ao telefone do escritório.
    * Os utilizadores não conseguem definir este campo propriamente ditas. Tem de ser definido por um administrador.
* **Número de telemóvel**: corresponde do telefone de autenticação (não visível publicamente) ou o número de telemóvel (publicamente visível).
    * O serviço procura primeiro o telefone de autenticação e, em seguida, apresenta não cai para o telemóvel se for o telefone de autenticação.
* **Alternativas de endereço de e-mail**: corresponde ao e-mail de autenticação (não visível publicamente) ou o correio eletrónico alternativo.
    * O serviço procura primeiro o e-mail de autenticação e, em seguida, falha novamente para o correio eletrónico alternativo.

Por predefinição, apenas o telefone do escritório de atributos de nuvem e telefone celular são sincronizadas com o diretório em nuvem do diretório no local para dados de autenticação.

Os utilizadores só podem repor a palavra-passe, se tiverem dados presentes nos métodos de autenticação que o administrador tiver ativado e requer.

Se os usuários não querem seu número de telefone celular seja visível no diretório, mas continuam a querer usá-lo para a reposição de palavra-passe, os administradores não devem preenchê--lo no diretório. Os utilizadores, em seguida, devem preencher seus **telefone de autenticação** através do atributo a [portal de registo de reposição de palavra-passe](https://aka.ms/ssprsetup). Os administradores podem ver estas informações no perfil do usuário, mas não está publicada em outro lugar.

### <a name="the-number-of-authentication-methods-required"></a>O número de métodos de autenticação necessária

Esta opção determina o número mínimo dos métodos de autenticação disponíveis ou limites de que um utilizador deve percorrer a repor ou desbloquear a palavra-passe. Ele pode ser definido para um ou dois.

Os utilizadores podem optar por fornecer mais métodos de autenticação, se o administrador ativa a esse método de autenticação.

Se um utilizador não tem os métodos necessários mínimos registados, verão uma página de erro que os direcione para solicitar que um administrador de reposição da palavra-passe.

#### <a name="change-authentication-methods"></a>Alterar o método de autenticação

Se iniciar com uma política que tenha apenas um método de autenticação obrigatório para repor ou desbloquear registado e alterar a que, para dois métodos, o que acontece?

| Número de métodos registados | Número de métodos necessários | Resultado |
| :---: | :---: | :---: |
| 1 ou mais | 1 | **Capaz de** a repor ou desbloquear |
| 1 | 2 | **Não é possível** a repor ou desbloquear |
| 2 ou mais | 2 | **Capaz de** a repor ou desbloquear |

Se alterar os tipos de métodos de autenticação que um utilizador pode utilizar, poderá inadvertidamente de impedir que os utilizadores a capacidade de utilizar o SSPR se eles não têm a quantidade mínima de dados disponíveis.

Exemplo: 
1. A política original está configurada com dois métodos de autenticação necessários. Utiliza apenas o número de telefone do escritório e as perguntas de segurança. 
2. O administrador altera a política para deixar de utilizar as perguntas de segurança, mas permite a utilização de um número de telefone e uma mensagem de e-mail alternativa.
3. Os utilizadores sem o número de telefone e campos de correio eletrónico alternativo preenchidos não é possível repor as palavras-passe.

### <a name="how-secure-are-my-security-questions"></a>Quão seguras são minhas perguntas de segurança?

Se utilizar perguntas de segurança, recomendamos que utilize-os em conjunto com outro método. Perguntas de segurança podem ser menos seguras do que outros métodos, porque algumas pessoas podem saber as respostas a perguntas de outro utilizador.

> [!NOTE] 
> Perguntas de segurança são armazenadas em privado e segura num objeto de utilizador no diretório e apenas podem ser respondidas por utilizadores durante o registo. Não é possível para um administrador ler ou modificar um utilizador perguntas ou respostas.
>

### <a name="security-question-localization"></a>Localização de pergunta de segurança

Todas as perguntas predefinidas que se seguem são localizadas para o conjunto completo de idiomas do Office 365 e baseiam-se na localidade do navegador do usuário:

* Em que localidade conheceu o seu primeiro cônjuge/companheiro(a)?
* Em que localidade se conheceram os seus pais?
* Em que localidade mora o seu irmão/irmã mais próximo?
* Em que localidade nasceu o seu pai?
* Em que localidade teve o seu primeiro emprego?
* Em que localidade nasceu a sua mãe?
* Onde estava (localidade) na Passagem de Ano de 2000?
* Qual é o apelido do seu professor/professora preferido na secundária?
* Qual é o nome de uma universidade a que se candidatou, mas que não frequentou?
* Qual é o nome do local onde organizou a sua primeira festa de casamento?
* Qual é o segundo nome do seu pai?
* Qual é o seu prato preferido?
* Qual é o primeiro e último nome da sua avó materna?
* Qual é o segundo nome da sua mãe?
* O que é o mês de nascimento e ano de seu irmão mais antigo? (por exemplo, de Novembro de 1985)
* Qual é o segundo nome do seu irmão/irmã mais velho?
* Qual é o primeiro e último nome do seu avô paterno?
* Qual é o segundo nome do seu irmão/irmã mais novo?
* Qual foi a escola que frequentou durante o sexto ano?
* Qual era o primeiro e último nome do seu melhor amigo de infância?
* Qual era o primeiro e último nome do seu primeiro amor?
* Qual era o apelido do seu professor/professora preferido da primária?
* Qual era a marca e modelo do seu primeiro carro ou motociclo?
* Como se chamava a primeira escola que frequentou?
* Qual era o nome do hospital em que nasceu?
* Qual era o nome da rua da sua primeira casa de infância?
* Qual era o nome do seu herói de infância?
* Qual era o nome do seu peluche preferido?
* Qual era o nome do seu primeiro animal de estimação?
* Qual era a sua alcunha de infância?
* Qual era o seu desporto preferido na secundária?
* Qual foi o seu primeiro emprego?
* Quais eram os quatro últimos algarismos do seu número de telefone de infância?
* Quando era criança, o que queria ser quando crescesse?
* Como se chama a maior celebridade que já conheceu?

### <a name="custom-security-questions"></a>Perguntas de segurança personalizadas

Perguntas de segurança personalizadas não são localizadas para diferentes regiões. Todas as perguntas personalizadas são apresentadas no mesmo idioma à medida que são introduzidos na interface do utilizador administrativo, mesmo que a Localidade do navegador do usuário é diferente. Se tiver perguntas localizadas, deve usar as perguntas predefinidas.

O comprimento máximo de uma pergunta de segurança personalizada é 200 carateres.

Para ver o portal de reposição de palavra-passe e perguntas diferentes localizadas no idioma anexar "? mkt =<Locale>" no final da palavra-passe de reposição de URL com o exemplo a seguir a localização de mensagens em fila para o espanhol [ https://passwordreset.microsoftonline.com/?mkt=es-us ](https://passwordreset.microsoftonline.com/?mkt=es-us).

### <a name="security-question-requirements"></a>Requisitos de pergunta de segurança

* O limite de caracteres de resposta mínimo é de três carateres.
* O limite de caracteres de resposta máximo é de 40 carateres.
* Os utilizadores não podem responder a mesma pergunta mais do que uma vez.
* Os usuários não é possível fornecer a mesma resposta à pergunta mais do que um.
* Qualquer conjunto de caracteres pode ser usado para definir as perguntas e respostas, incluindo os caracteres Unicode.
* O número de perguntas definidas tem de ser maior que ou igual ao número de perguntas que foram necessárias para se registar.

## <a name="registration"></a>Registo

### <a name="require-users-to-register-when-they-sign-in"></a>Exigir que os utilizadores se registem quando iniciam sessão

Para ativar esta opção, um utilizador que está ativado para a reposição de palavra-passe tem de concluir o registo de reposição de palavra-passe, iniciar sessão aplicações ao utilizar o Azure AD. Isto inclui o seguinte:

* Office 365
* Portal do Azure
* Painel de Acesso
* Aplicações federadas
* Aplicativos personalizados através da utilização do Azure AD

Quando a necessidade de registo está desativada, os utilizadores podem registar manualmente as informações de contacto. Podem visite [ https://aka.ms/ssprsetup ](https://aka.ms/ssprsetup) ou selecione o **Registre-se para a reposição de palavra-passe** ligação sob o **perfil** separador no painel de acesso.

> [!NOTE]
> Os utilizadores podem dispensar o portal de registo de reposição de palavra-passe, selecionando **Cancelar** ou se fechar a janela. Mas, são-lhe pedido para se registar cada vez que iniciar sessão até que sejam concluídas seu registro.
>
> Tal não interromper a ligação do usuário se eles já tiver iniciados sessão.

### <a name="set-the-number-of-days-before-users-are-asked-to-reconfirm-their-authentication-information"></a>Definir o número de dias antes de ser pedido aos utilizadores que voltem as informações de autenticação a

Esta opção determina o período de tempo entre a definição e reconfirmando as informações de autenticação quando e está disponível apenas se ativar a **exigir que os utilizadores se registem ao iniciar** opção.

Valores válidos são 0 para 730 dias. o, com "0", que significa que nunca ser pedido aos utilizadores que voltem as informações de autenticação a.

## <a name="notifications"></a>Notificações

### <a name="notify-users-on-password-resets"></a>Notificar os utilizadores sobre reposições de palavras-passe

Se esta opção estiver definida como **Sim**, em seguida, o utilizador que está a repor a palavra-passe recebe um e-mail a notificar que a palavra-passe foi alterada. O e-mail é enviado através do portal SSPR para os respetivos endereços de e-mail principal e alternativo que estão no ficheiro no Azure AD. Ninguém mais é notificado sobre esta reposição de evento.

### <a name="notify-all-admins-when-other-admins-reset-their-passwords"></a>Notificar todos os administradores quando outros administradores repõem as palavras-passe

Se esta opção estiver definida como **Sim**, em seguida, *todos os administradores* receber um e-mail para o respetivo endereço de e-mail principal registado no Azure AD. A mensagem de e-mail notifica-los que outro administrador foi alterado a palavra-passe ao utilizar o SSPR.

Exemplo: Existem quatro administradores num ambiente. O administrador A repõe a palavra-passe ao utilizar o SSPR. Os administradores B, C e D recebem um e-mail que o alerta da reposição de palavra-passe.

## <a name="on-premises-integration"></a>Integração no local

Se instalar, configura e ativar o Azure AD Connect, tem as seguintes opções adicionais para integrações no local. Se estas opções são desativadas, em seguida, repetição de escrita não foi corretamente configurada. Para obter mais informações, consulte [configurar a repetição de escrita de palavra-passe](howto-sspr-writeback.md#configure-password-writeback).

![Repetição de escrita][Writeback]

Esta página fornece um rápido status do cliente de repetição de escrita do locais uma das duas mensagens seguintes são apresentadas com base na configuração atual:

* O cliente de repetição de escrita no local está em execução.
* Azure AD está online e está ligado ao seu cliente de repetição de escrita no local. No entanto, parece que a versão instalada do Azure AD Connect está desatualizada. Considere [atualizar o Azure AD Connect](./../connect/active-directory-aadconnect-upgrade-previous-version.md) para se certificar de que tem as mais recentes funcionalidades de conectividade e correções de erros importantes.
* Infelizmente, não foi possível verificar o estado de repetição de escrita no local do cliente porque a versão instalada do Azure AD Connect está desatualizada. [Atualizar o Azure AD Connect](./../connect/active-directory-aadconnect-upgrade-previous-version.md) para poder verificar o estado da ligação.
* Infelizmente, parece não é possível ligar ao seu cliente de repetição de escrita no local neste momento. [Resolver problemas do Azure AD Connect](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback-connectivity) para restabelecer a ligação.
* Infelizmente, a foi possível ligar ao seu cliente de repetição de escrita no local porque a repetição de escrita de palavra-passe não foi corretamente configurada. [Configurar a repetição de escrita de palavra-passe](howto-sspr-writeback.md#configure-password-writeback) para restabelecer a ligação.
* Infelizmente, parece não é possível ligar ao seu cliente de repetição de escrita no local neste momento. Isto pode dever-se a problemas temporários do nosso lado. Se o problema persistir, [resolver problemas relacionados com o Azure AD Connect](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback-connectivity) para restabelecer a ligação.

### <a name="write-back-passwords-to-your-on-premises-directory"></a>Gravar palavras-passe no seu diretório no local

Esse controle determina se a repetição de escrita de palavra-passe está ativada para este diretório. Se estiver repetição de escrita ativado, ele indica o estado do serviço de repetição de escrita no local. Isto é útil se pretender desativar temporariamente a repetição de escrita de palavra-passe sem ter de reconfigurar o Azure AD Connect.

* Se o parâmetro estiver definido como **Sim**, em seguida, a repetição de escrita é ativada e a autenticação federada, pass-through ou os utilizadores sincronizado de hash de palavra-passe são capazes de repor as palavras-passe.
* Se o parâmetro estiver definido como **não**, em seguida, a repetição de escrita está desativada e a autenticação federada, pass-through ou os utilizadores sincronizado de hash de palavra-passe não são possível repor as palavras-passe.

### <a name="allow-users-to-unlock-accounts-without-resetting-their-password"></a>Permitir que os utilizadores desbloqueiem as contas sem repor a palavra-passe

Esse controle designa se os usuários que visitam o portal de reposição de palavra-passe devem ser dada a opção para desbloquear suas contas do Active Directory no local sem ter de repor a palavra-passe. Por predefinição, o Azure AD desbloqueia contas quando ele efetuar uma reposição de palavra-passe. Utilize esta definição para separar as duas operações. 

* Se definido como **Sim**, em seguida, os utilizadores recebem a opção para repor a palavra-passe e desbloquear a conta de ou para desbloquear a conta sem ter de repor a palavra-passe.
* Se definido como **não**, em seguida, os utilizadores são só poderá efetuar uma reposição de palavra-passe combinado e operação de desbloqueio de conta.

## <a name="how-does-password-reset-work-for-b2b-users"></a>Como a palavra-passe reposta trabalho para utilizadores B2B?
Alteração e reposição de palavra-passe são totalmente suportadas em todas as configurações do empresa-empresa (B2B). Reposição de palavra-passe de utilizador B2B é suportada nos seguintes três casos:

   * **Os utilizadores de uma organização de parceiro com um inquilino do Azure AD existente**: se a organização que nossa parceria com a tiver um inquilino do Azure AD existente, podemos *respeitar quaisquer políticas de reposição de palavra-passe estão ativadas nesse inquilino*. Palavra-passe reposta para trabalhar, a organização de parceiro precisa apenas Certifique-se de que o Azure AD SSPR está ativado. Não incorre em encargos adicionais para os clientes do Office 365 e pode ser ativada ao seguir os passos no nosso [introdução à gestão de palavra-passe](https://azure.microsoft.com/documentation/articles/active-directory-passwords-getting-started/#enable-users-to-reset-or-change-their-aad-passwords) guia.
   * **Os utilizadores que se inscrevam através de** inscrição Self-Service: se a organização nossa parceria com a utilizado o [inscrição Self-Service](../users-groups-roles/directory-self-service-signup.md) de recursos para obter um inquilino, nós deixamos que eles repor a palavra-passe com a mensagem de e-mail que registado.
   * **Utilizadores B2B**: quaisquer novos utilizadores B2B criados com a nova [capacidades do Azure AD B2B](../active-directory-b2b-what-is-azure-ad-b2b.md) também poderão repor as palavras-passe com a mensagem de e-mail que registado durante o processo de convite.

Para testar este cenário, aceda a http://passwordreset.microsoftonline.com com um destes utilizadores de parceiro. Se tiverem um correio eletrónico alternativo ou e-mail de autenticação definidos, a palavra-passe reposição funciona conforme esperado.

> [!NOTE]
> Contas da Microsoft que tem sido concedidas acesso de convidado com o seu inquilino do Azure AD, como as do Hotmail.com, Outlook.com ou outros endereços de e-mail pessoal, não são possível utilizar o Azure AD SSPR. Tem de repor a palavra-passe, utilizando as informações encontradas na [quando não pode iniciar sessão na sua conta Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) artigo.

## <a name="next-steps"></a>Passos Seguintes

Os artigos seguintes disponibilizam informações adicionais relativamente à reposição de palavras-passe através do Azure AD:

* [Como posso concluir uma implementação com êxito da SSPR?](howto-sspr-deployment.md)
* [Repor ou alterar a palavra-passe](../active-directory-passwords-update-your-own-password.md)
* [Registar-se na reposição personalizada de palavra-passe](../active-directory-passwords-reset-register.md)
* [Tem alguma pergunta sobre licenciamento?](concept-sspr-licensing.md)
* [Que dados são utilizados pela SSPR e que dados devem ser preenchidos por si para os seus utilizadores?](howto-sspr-authenticationdata.md)
* [Que métodos de autenticação estão disponíveis para os utilizadores?](concept-sspr-howitworks.md#authentication-methods)
* [Quais são as opções de política da SSPR?](concept-sspr-policy.md)
* [O que é a repetição de escrita de palavras-passe e por que me deve interessar?](howto-sspr-writeback.md)
* [Como posso comunicar a atividade da SSPR?](howto-sspr-reporting.md)
* [Quais são todas as opções na SSPR e o que significam?](concept-sspr-howitworks.md)
* [Creio que algo está a funcionar incorretamente. Como posso resolver problemas da SSPR?](active-directory-passwords-troubleshoot.md)
* [Tenho uma pergunta que ainda não foi abordada](active-directory-passwords-faq.md)

[Authentication]: ./media/concept-sspr-howitworks/sspr-authentication-methods.png "Métodos de autenticação do Azure AD disponíveis e a quantidade necessária"
[Writeback]: ./media/concept-sspr-howitworks/troubleshoot-writeback-running.png "Configuração de integração de repetição de escrita de palavra-passe e informações de resolução de problemas no local"
