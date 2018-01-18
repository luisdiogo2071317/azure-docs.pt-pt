---
title: "Resolução de problemas - Azure Active Directory de reposição de palavra-passe self-service"
description: "Reposição de resolução de problemas passe self-service do Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro;seohack1
ms.openlocfilehash: c038a9ec682a5971a5f79b9fe36e667493702cbd
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="troubleshoot-self-service-password-reset"></a>Resolver problemas de reposição de palavra-passe self-service

Está a ter um problema com a reposição de palavra-passe self-service do Azure Active Directory (Azure AD) (SSPR)? As informações que se segue podem ajudar a dar a funcionar novamente.

## <a name="troubleshoot-self-service-password-reset-errors-that-a-user-might-see"></a>Resolver erros de reposição de palavra-passe self-service que poderá ver um utilizador

| Erro | Detalhes | Detalhes técnicos |
| --- | --- | --- |
| TenantSSPRFlagDisabled = 9 | Pedimos desculpa, que não é possível repor a palavra-passe neste momento porque o administrador desativou a palavra-passe de reposição da sua organização. Não há nenhuma ação adicional que pode tomar para resolver esta situação. Contacte o administrador e peça-lhes para ativar esta funcionalidade. Para obter mais informações, consulte [ajuda, esqueci minha palavra-passe do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password#common-problems-and-their-solutions). | SSPR_0009: Detetámos que reposição de palavra-passe não foi ativada pelo seu administrador. Contacte o administrador e peça-lhes para ativar a reposição da sua organização palavra-passe. |
| WritebackNotEnabled = 10 |Pedimos desculpa, que não é possível repor a palavra-passe neste momento porque o administrador não foi ativado um serviço necessário para a sua organização. Não há nenhuma ação adicional que pode tomar para resolver esta situação. Contacte o administrador e peça-lhes para verificar a configuração da sua organização. Para obter mais informações sobre este serviço necessário, consulte o artigo [configurar a repetição de escrita de palavras-passe](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-writeback#configure-password-writeback). | SSPR_0010: Detetámos que repetição de escrita de palavras-passe não foi ativada. Contacte o administrador e peça-lhes para ativar a repetição de escrita de palavras-passe. |
| SsprNotEnabledInUserPolicy = 11 | Pedimos desculpa, que não é possível repor a palavra-passe neste momento porque o administrador não configurou para a sua organização de reposição de palavra-passe. Não há nenhuma ação adicional que pode tomar para resolver esta situação. Contacte o administrador e peça-lhes para configurar a reposição de palavra-passe. Para saber mais sobre a palavra-passe repor a configuração, consulte [início rápido: repor a palavra-passe self-service do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started). | SSPR_0011: Sua organização não tiver definido uma política de reposição de palavra-passe. Contacte o administrador e peça-lhes para definir uma política de reposição de palavra-passe. |
| UserNotLicensed = 12 | Pedimos desculpa, que não é possível repor a palavra-passe neste momento porque necessários licenças estão em falta na sua organização. Não há nenhuma ação adicional que pode tomar para resolver esta situação. Contacte o administrador e peça-lhes para verificar a sua atribuição de licenças. Para saber mais sobre o licenciamento, consulte [reposição de palavra-passe self-service do Azure AD, os requisitos de licenciamento](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-licensing). | SSPR_0012: A sua organização não tem as licenças necessárias necessárias para efetuar a reposição de palavra-passe. Contacte o administrador e peça-lhes para rever as atribuições de licenças. |
| UserNotMemberOfScopedAccessGroup = 13 | Pedimos desculpa, que não é possível repor a palavra-passe neste momento porque o administrador não configurou a sua conta para utilizar a reposição de palavra-passe. Não há nenhuma ação adicional que pode tomar para resolver esta situação. Contacte o administrador e peça-lhes para configurar a conta para a reposição de palavra-passe. Para saber mais sobre a configuração da conta para a reposição de palavra-passe, consulte o artigo [implementar para os utilizadores de reposição de palavra-passe](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-best-practices). | SSPR_0012: Não é um membro de um grupo ativado para a reposição de palavra-passe. Contacte o administrador e a pedido para ser adicionado ao grupo. |
| UserNotProperlyConfigured = 14 | Pedimos desculpa, que não é possível repor a palavra-passe neste momento porque faltam informações necessárias da sua conta. Não há nenhuma ação adicional que pode tomar para resolver esta situação. . Contacte o administrador e peça-lhes para repor a palavra-passe por si. Depois de ter acesso à sua conta novamente, tem de registar as informações necessárias. Para registar informações, siga os passos a [registar para a reposição de palavra-passe self-service](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-reset-register) artigo. | SSPR_0014: Informações de segurança adicional são necessárias para repor a palavra-passe. Para continuar, contacte o administrador e peça-lhes para repor a palavra-passe. Depois de ter acesso à sua conta, pode registar informações de segurança adicionais em https://aka.ms/ssprsetup. O administrador pode adicionar informações de segurança adicionais à sua conta, seguindo os passos no [conjunto e os dados de autenticação de leitura para a reposição de palavra-passe](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-data#set-and-read-authentication-data-using-powershell). |
| OnPremisesAdminActionRequired = 29 | Pedimos desculpa, que não é possível repor a palavra-passe neste momento devido a um problema com a configuração de reposição de palavra-passe da sua organização. Não há nenhuma ação adicional que pode tomar para resolver esta situação. Contacte o administrador e peça-lhes para investigar. Para saber mais sobre o potencial problema, consulte o artigo [resolver problemas de repetição de escrita de palavras-passe](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback). | SSPR_0029: Estamos não é possível repor a palavra-passe devido a um erro na configuração no local. Contacte o administrador e peça-lhes para investigar. |
| OnPremisesConnectivityError = 30 | Pedimos desculpa, que não é possível repor a palavra-passe neste momento devido a problemas de conectividade com a sua organização. Não há nenhuma ação a tomar nesse momento, mas o problema poderá ser resolvido se tentar novamente mais tarde. Se o problema persistir, contacte o administrador e peça-lhes para investigar. Para obter mais informações sobre problemas de conectividade, consulte o artigo [resolver problemas de conectividade de repetição de escrita de palavras-passe](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback-connectivity). | SSPR_0030: Não é possível repor a palavra-passe devido a uma ligação fraca com o seu ambiente no local. Contacte o administrador e peça-lhes para investigar.|


## <a name="troubleshoot-the-password-reset-configuration-in-the-azure-portal"></a>Resolver problemas relacionados com a configuração de reposição de palavra-passe no portal do Azure

| Erro | Solução |
| --- | --- |
| Não vejo o **reposição de palavra-passe** secção em AD do Azure no portal do Azure. | Isto pode acontecer se não tiver um Azure AD Premium ou Basic licença atribuída para o administrador efetuar a operação. <br> <br> Atribua uma licença para a conta de administrador em questão. Pode seguir os passos a [atribuir, certifique-se e resolver problemas com licenças](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) artigo.|
| Não consigo ver uma opção de configuração específica. | Muitos elementos da IU estão ocultos nas até que forem necessárias. Tente ativar todas as opções que pretende ver. |
| Não vejo o **integração no local** separador. | Esta opção só fica visível se tiver transferido o Azure AD Connect e tiver configurado a repetição de escrita de palavras-passe. Para obter mais informações, consulte [introdução ao Azure AD Connect utilizando as definições rápidas](./connect/active-directory-aadconnect-get-started-express.md). |

## <a name="troubleshoot-password-reset-reporting"></a>Resolver problemas relacionados com relatórios de reposição de palavra-passe

| Erro | Solução |
| --- | --- |
| Não consigo ver quaisquer tipos de atividades de gestão de palavra-passe no **gestão de palavras-passe Self-Service** categoria de evento de auditoria. | Isto pode acontecer se não tiver um Azure AD Premium ou Basic licença atribuída para o administrador efetuar a operação. <br> <br> Pode resolver este problema ao atribuir uma licença para a conta de administrador em questão. Siga os passos a [atribuir, certifique-se e resolver problemas com licenças](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) artigo. |
| Registos de utilizador mostram várias vezes. | Atualmente, quando um utilizador se regista, vamos registar cada peça individual de dados que estão registados como um evento separado. <br> <br> Se pretender que estes dados de agregação e que tenham uma maior flexibilidade na forma como pode ver, pode transferir o relatório e abrir os dados como uma tabela dinâmica no Excel.

## <a name="troubleshoot-the-password-reset-registration-portal"></a>Resolver problemas relacionados com o portal de registo de reposição de palavra-passe

| Erro | Solução |
| --- | --- |
| O diretório não está ativado para a reposição de palavra-passe. **O administrador não activou utilizar esta funcionalidade.** | Comutador de **ativada de reposição de palavra-passe self-service** sinalizador para **selecionados** ou **todos os** e, em seguida, selecione **guardar**. |
| O utilizador não tem um Azure AD Premium ou Basic licença atribuída. **O administrador não activou utilizar esta funcionalidade.** | Isto pode acontecer se não tiver um Azure AD Premium ou Basic licença atribuída para o administrador efetuar a operação. <br> <br> Pode resolver este problema ao atribuir uma licença para a conta de administrador em questão. Siga os passos a [atribuir, certifique-se e resolver problemas com licenças](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) artigo.|
| Não há um erro ao processar o pedido. | Isto pode ser causado por vários problemas, mas, geralmente, este erro é causado por uma interrupção do serviço ou um problema de configuração. Se vir este erro e que isso afeta a sua empresa, contacte o suporte da Microsoft para obter assistência adicional. |

## <a name="troubleshoot-the-password-reset-portal"></a>Resolver problemas relacionados com o portal de reposição de palavra-passe

| Erro | Solução |
| --- | --- |
| O diretório não está ativado para a reposição de palavra-passe. | Comutador de **ativada de reposição de palavra-passe self-service** sinalizador para **selecionados** ou **todos os** e, em seguida, selecione **guardar**. |
| O utilizador não tem um Azure AD Premium ou Basic licença atribuída. | Isto pode acontecer se não tiver um Azure AD Premium ou Basic licença atribuída para o administrador efetuar a operação. <br> <br> Pode resolver este problema, se atribuir uma licença para a conta de administrador em questão. Siga os passos a [atribuir, certifique-se e resolver problemas com licenças](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) artigo. |
| O diretório está ativado para a reposição de palavra-passe, mas o utilizador tem informações de autenticação em falta ou com formato incorreto. | Antes de continuar, certifique-se de que o utilizador foi correctamente formado dados de contacto no ficheiro no diretório. Para obter mais informações, consulte [de dados utilizadas por palavra-passe self-service do Azure AD repor](active-directory-passwords-data.md). |
| O diretório está ativado para a reposição de palavra-passe, mas o utilizador tem apenas um conjunto de dados de contactos no ficheiro quando a política estiver definida para exigir dois métodos de verificação. | Antes de continuar, certifique-se de que o utilizador tem, pelo menos, dois métodos de contactos está corretamente configurados. Um exemplo é ter um número de telemóvel *e* um número de telefone do escritório. |
| O diretório está ativado para a reposição de palavra-passe e o utilizador está configurado corretamente, mas o utilizador não é possível ser contactados. | Isto pode ser resultado de um erro de temporária do serviço ou se houver dados de contactos incorreto, não é possível corretamente detetar. <br> <br> Se o utilizador tem de aguardar 10 segundos, "tente de novo" e aparecem ligações "Contacte o administrador". Se o utilizador seleciona "tente de novo", repete a chamada. Se o utilizador seleciona "Contacte o administrador", envia um e-mail de formulário para os respetivos administradores pedir uma reposição para ser executada para essa conta de utilizador de palavra-passe. |
| O utilizador recebe nunca a reposição de palavra-passe SMS ou chamada telefónica. | Isto pode ser resultado de um número de telefone incorreto no diretório. Certifique-se de que o número de telefone está no formato "+ xxxyyyzzzzXeeee CC". <br> <br> Reposição de palavra-passe não suporta extensões, mesmo se especificar um no diretório. As extensões estão repartidas antes da chamada é emitida. Utilizar um número sem uma extensão ou integrar a extensão do número de telefone na sua exchange ramo privada (PBX). |
| O utilizador nunca recebe o e-mail de reposição de palavra-passe. | A causa mais comum para este problema é que a mensagem foi rejeitada por um filtro de spam. Verifique a spam, a pasta lixo ou eliminada itens para o correio eletrónico. <br> <br> Certifique-se também de que está a verificar a conta de e-mail correto para a mensagem. |
| Posso definiu uma política de reposição de palavra-passe, mas quando uma conta de administrador utiliza uma reposição de palavra-passe, essa política não está aplicada. | Gere a Microsoft e controlos de política para garantir o nível mais elevado de segurança de reposição de palavra-passe de administrador. |
| O utilizador é impedido de tentar uma palavra-passe repor demasiadas vezes num dia. | Iremos implementar um mecanismo de limitação automático para impedir que os utilizadores da tentativa de repor as palavras-passe demasiadas vezes num curto período de tempo. Limitação ocorre quando: <br><ul><li>O utilizador tenta validar um número de telefone 5 vezes dentro de uma hora.</li><li>O utilizador tenta utilizar a porta de perguntas de segurança 5 vezes dentro de uma hora.</li><li>O utilizador tenta repor uma palavra-passe para a mesma conta de utilizador 5 vezes dentro de uma hora.</li></ul>Para corrigir este problema, instruir o utilizador Aguarde 24 horas após a última tentativa. O utilizador, em seguida, pode repor a palavra-passe. |
| O utilizador verá um erro ao validar o respetivo número de telefone. | Este erro ocorre quando o número de telefone introduzido não corresponde ao número de telefone no ficheiro. Certifique-se de que o utilizador está a introduzir o número de telemóvel completo, incluindo o código de área e país, quando tentam utilizar um método baseado em telefone para a reposição de palavra-passe. |
| Não há um erro ao processar o pedido. | Isto pode ser causado por vários problemas, mas, geralmente, este erro é causado por uma interrupção do serviço ou um problema de configuração. Se vir este erro e que isso afeta a sua empresa, contacte o suporte da Microsoft para obter assistência adicional. |

## <a name="troubleshoot-password-writeback"></a>Resolver problemas de repetição de escrita de palavras-passe

| Erro | Solução |
| --- | --- |
| O serviço de reposição de palavra-passe não é iniciado no local. Erro 6800 é apresentado no registo de eventos de aplicações do computador do Azure AD Connect. <br> <br> Depois de integração, os utilizadores federados ou sincronizadas de hash de palavra-passe não é possível repor as palavras-passe. | Quando a repetição de escrita de palavras-passe está ativada, o motor de sincronização chama a biblioteca de repetição de escrita para executar a configuração (integração) ao comunicar com o serviço de integração em nuvem. Quaisquer erros encontrados durante a ativação ou ao iniciar o ponto final do Windows Communication Foundation (WCF) para resultados de repetição de escrita de palavras-passe erros no registo de eventos no computador do Azure AD Connect. <br> <br> Durante o reinício do serviço Azure AD Sync (ADSync), se tiver sido configurada a repetição de escrita, o ponto final WCF é iniciado. No entanto, se o arranque do ponto final falhar, iremos registar evento 6800 e permitir que o arranque do serviço de sincronização. A presença deste evento significa que o ponto final de repetição de escrita de palavras-passe não foram iniciados cópias de segurança. Detalhes de registo de eventos para este evento 6800, juntamente com o registo de eventos entradas geram pelo componente PasswordResetService, indicam a razão pela qual não é possível iniciar o ponto final. Reveja estes erros de registo de eventos e tente reiniciar o Azure AD Connect, se a repetição de escrita de palavras-passe ainda não está a funcionar. Se o problema persistir, tente desativar e, em seguida, volte a ativar a repetição de escrita de palavras-passe.
| Quando um utilizador tenta repor uma palavra-passe ou desbloquear uma conta com repetição de escrita de palavras-passe ativada, a operação falhar. <br> <br> Além disso, pode ver um evento no registo de eventos do Azure AD Connect que contenha: "motor de sincronização devolveu um erro hr = 800700CE, mensagem = o nome de ficheiro ou a extensão é demasiado longa" após a ocorrência da operação de desbloqueio. | Localize a conta do Active Directory do Azure AD Connect e repor a palavra-passe para que o contém não mais de 127 carateres. Em seguida, abra o **serviço de sincronização** do **iniciar** menu. Navegue até à **conectores** e localize o **conector do Active Directory**. Selecione-o e, em seguida, selecione **propriedades**. Navegue para o **credenciais** página e introduza a palavra-passe nova. Selecione **OK** para fechar a página. |
| No último passo do processo de instalação do Azure AD Connect, verá um erro que indica que não foi possível configurar a repetição de escrita essa palavra-passe. <br> <br> O registo de eventos de aplicações do Azure AD Connect contém erro 32009 com o texto "Erro ao obter auth token." | Este erro ocorre nos dois seguintes casos: <br><ul><li>Especificou uma palavra-passe incorreta para a conta de administrador global especificada no início do processo de instalação do Azure AD Connect.</li><li>Tentou utilizar um utilizador federado para a conta de administrador global especificada no início do processo de instalação do Azure AD Connect.</li></ul> Para corrigir este problema, certifique-se de que não está a utilizar uma conta federada para o administrador global que especificou no início do processo de instalação. Certifique-se também de que a palavra-passe especificada está correta. |
| O registo de eventos do Azure AD Connect máquina contém erro 32002 que é acionado executando PasswordResetService. <br> <br> Lê o erro: "erro ligar ao barramento de serviço. O fornecedor de tokens não foi possível fornecer um token de segurança." | O ambiente no local não é possível estabelecer ligação ao ponto final do Service Bus do Azure na nuvem. Normalmente, este erro é causado por uma regra de firewall a bloquear uma ligação de saída para um endereço de web ou a porta específica. Consulte [pré-requisitos de conectividade](./connect/active-directory-aadconnect-prerequisites.md) para obter mais informações. Depois de atualizar estas regras, reinicie a máquina do Azure AD Connect e repetição de escrita de palavras-passe deve começar a trabalhar novamente. |
| Depois de funcionar durante algum tempo, os utilizadores federados ou sincronizadas de hash de palavra-passe não é possível repor as palavras-passe. | Em alguns casos raros, o serviço de repetição de escrita de palavras-passe pode não conseguir reiniciar quando o Azure AD Connect foi reiniciado. Nestes casos, em primeiro lugar, verifique se a repetição de escrita de palavras-passe parece estar ativado no local. Pode verificar, utilizando o Assistente do Azure AD Connect ou o PowerShell (consulte a secção de HowTos anterior). Se a funcionalidade parece estar ativado, tente ativar ou desativar a funcionalidade novamente através da IU ou do PowerShell. Se isto não funcionar, experimente uma desinstalação completa e reinstale do Azure AD Connect. |
| Utilizadores federados ou sincronizadas de hash de palavra-passe que tentam de repor as palavras-passe veem um erro ao tentar enviar a palavra-passe. O erro indica que ocorreu um problema de serviço. <br ><br> Para além deste problema, durante as operações de reposição de palavra-passe, poderá ver um erro que o agente de gestão foi negado acesso nos seus registos de eventos no local. | Se vir estes erros no registo de eventos, certifique-se de que a conta de agente de gestão do Active Directory (ADMA) que foi especificada no assistente no momento da configuração tem as permissões necessárias para a repetição de escrita de palavras-passe. <br> <br> Tenha em atenção que, depois desta permissão é atribuído, pode demorar até uma hora para as permissões para trickle baixo através de `sdprop` tarefa em segundo plano no controlador de domínio (DC). <br> <br> Palavra-passe repor funcione, a permissão tem de ser marcados no descritor de segurança do objeto de utilizador cujo palavra-passe está a ser reposta. Até que esta permissão aparece no objeto user, reposição de palavra-passe continua a falhar com um acesso negado a mensagem. |
| Federados ou os utilizadores sincronizados de hash de palavra-passe, que tentam repor as palavras-passe, consulte um erro depois de submeter a palavra-passe. O erro indica que ocorreu um problema de serviço. <br> <br> Para além deste problema, durante as operações de reposição de palavra-passe, poderá ver um erro nos seus registos de eventos do serviço do Azure AD Connect que indica um erro "Objeto não foi possível encontrar". | Este erro normalmente indica que o motor de sincronização não é possível localizar o objeto de utilizador no espaço de conector do Azure AD ou o metaverso ligado (MV) ou o objeto de espaço de conector do Azure AD. <br> <br> Para resolver este problema, certifique-se de que o utilizador, de facto, está sincronizado no local ao Azure AD através da instância atual do Azure AD Connect e inspecionar o estado dos objetos de MV e espaços conectores. Certifique-se de que o objeto serviços de certificados do Active Directory (AD CS) está ligado ao objeto de MV através da regra "Microsoft.InfromADUserAccountEnabled.xxx".|
| Federados ou os utilizadores sincronizados de hash de palavra-passe, que tentam repor as palavras-passe apresentado um erro depois de submeter a palavra-passe. O erro indica que ocorreu um problema de serviço. <br> <br> Para além deste problema, durante as operações de reposição de palavra-passe, poderá ver um erro nos seus registos de eventos do serviço do Azure AD Connect que indica que existe um erro de "Correspondências vários encontradas". | Isto indica que o motor de sincronização detetou que o objeto de MV está ligado a mais do que um objeto do AD CS através de "Microsoft.InfromADUserAccountEnabled.xxx". Isto significa que o utilizador tem uma conta ativada em mais do que uma floresta. Tenha em atenção que este cenário não é suportado para a repetição de escrita de palavras-passe. |
| Operações de palavra-passe falhar com um erro de configuração. O registo de eventos da aplicação contém o Azure AD Connect erro 6329 com o texto "0x8023061f (falha na operação porque a sincronização de palavra-passe não está ativada neste agente de gestão)". | Este erro ocorre que se a configuração do Azure AD Connect é alterada para adicionar uma nova floresta do Active Directory (ou para remover e voltar a adicionar uma floresta existente), após já tenha sido ativada a funcionalidade de repetição de escrita de palavras-passe. Operações de palavra-passe de utilizadores adicionadas recentemente florestas falhar. Para corrigir o problema, desative e volte a ativar a funcionalidade de repetição de escrita de palavras-passe depois das alterações de configuração de floresta foram concluídas. |

## <a name="password-writeback-event-log-error-codes"></a>Códigos de erro de registo de eventos de repetição de escrita de palavras-passe

É uma prática quando resolver problemas com a repetição de escrita de palavras-passe inspecionar o registo de eventos de aplicações no seu computador do Azure AD Connect. Este registo de eventos contém eventos de duas origens de interesse para repetição de escrita de palavras-passe. A origem de PasswordResetService descreve as operações e problemas relacionados com a operação de repetição de escrita de palavras-passe. A origem de ADSync descreve as operações e problemas relacionados com a definição de palavras-passe no seu ambiente do Active Directory.

### <a name="if-the-source-of-the-event-is-adsync"></a>Se a origem do evento é ADSync

| Código | Nome ou da mensagem | Descrição |
| --- | --- | --- |
| 6329 | BAIL: MMS(4924) 0x80230619: "uma restrição impede a palavra-passe a ser alterada para o atual especificado." | Este evento ocorre quando o serviço de repetição de escrita de palavras-passe tenta definir uma palavra-passe no diretório no local que não cumprem a idade da palavra-passe, histórico, complexidade ou requisitos de filtragem do domínio. <br> <br> Se tiver uma antiguidade mínima da palavra-passe e tiver alterado recentemente a palavra-passe dentro desse período de tempo, não está tiverem de alterar a palavra-passe novamente até atingir a duração especificada no seu domínio. Para fins de teste, a idade mínima deve ser definida como 0. <br> <br> Se tiver requisitos de histórico de palavras-passe ativados, então tem de selecionar uma palavra-passe que não tenha sido utilizada nos últimos *N* vezes, onde *N* é a definição do histórico de palavra-passe. Se selecionar uma palavra-passe que foi utilizada nos últimos *N* exceder o tempo, em seguida, neste caso, verá uma falha. Para fins de teste, o histórico de palavra-passe deve ser definido como 0. <br> <br> Se tiver requisitos de complexidade de palavra-passe, todos eles são aplicados quando o utilizador tenta alterar ou repor uma palavra-passe. <br> <br> Se tiver ativados de filtros de palavra-passe e um utilizador seleccionar uma palavra-passe que não cumpre os critérios de filtragem, em seguida, a reposição ou falha da operação de alteração. |
| 6329 | MMS(3040): admaexport.cpp(2837): O servidor não contém o controlo de política de palavra-passe LDAP. | Este problema ocorre se o controlo LDAP_SERVER_POLICY_HINTS_OID (1.2.840.113556.1.4.2066) não está ativado nos controladores de domínio. Para utilizar a funcionalidade de repetição de escrita de palavras-passe, tem de ativar o controlo. Para tal, os controladores de domínio tem de ser no Windows Server 2008 (com SP mais recente) ou posterior. Se os DCs estão no 2008 (pré-R2), em seguida, também tem de aplicar correção [KB2386717](http://support.microsoft.com/kb/2386717). |
| HR 8023042 | Motor de sincronização devolveu um erro hr = 80230402, mensagem = uma tentativa de obtenção de um objeto falhou porque não existem entradas duplicadas com a mesma âncora. | Este erro ocorre quando o mesmo ID de utilizador está ativado em vários domínios. Um exemplo é se estiver a sincronizar as florestas de conta e recursos e ter o mesmo ID de utilizador presente e ativado em cada floresta. <br> <br> Este erro também pode ocorrer se utilizar um atributo âncora não exclusivo, como um alias ou UPN e dois utilizadores partilham esse mesmo atributo de âncora. <br> <br> Para resolver este problema, certifique-se de que não tem quaisquer utilizadores duplicados dentro os domínios e de que utiliza um atributo âncora exclusivo para cada utilizador. |

### <a name="if-the-source-of-the-event-is-passwordresetservice"></a>Se a origem do evento é PasswordResetService

| Código | Nome ou da mensagem | Descrição |
| --- | --- | --- |
| 31001 | PasswordResetStart | Este evento indica que o serviço no local detetado que pedido para um utilizador federado ou sincronizadas de hash de palavra-passe que origina da nuvem de reposição de uma palavra-passe. Este evento é o primeiro evento na cada operação de repetição de escrita de reposição de palavra-passe. |
| 31002 | PasswordResetSuccess | Este evento indica que um utilizador selecionado uma nova palavra-passe durante uma operação de reposição de palavra-passe. Determinámos que esta palavra-passe cumpra os requisitos de palavra-passe empresarial. A palavra-passe tem foi escrita com êxito na novamente para o ambiente do Active Directory local. |
| 31003 | PasswordResetFail | Este evento indica que um utilizador selecionado uma palavra-passe e a palavra-passe chegou com êxito para o ambiente no local. Mas, ao tentar definir a palavra-passe no ambiente do Active Directory local, Ocorreu uma falha. Esta falha pode ocorrer por vários motivos: <br><ul><li>Palavra-passe do utilizador não cumprem a idade, o histórico, a complexidade ou filtrar os requisitos para o domínio. Para resolver este problema, crie uma nova palavra-passe.</li><li>A conta de serviço do ADMA não tem as permissões adequadas para definir a nova palavra-passe da conta de utilizador em causa.</li><li>A conta de utilizador está num grupo protegido, tais como o grupo de administradores ou empresarial do domínio, que não permite operações de conjunto de palavra-passe.</li></ul>|
| 31004 | OnboardingEventStart | Este evento ocorre se ativar a repetição de escrita de palavras-passe com o Azure AD Connect e ter iniciamos integração sua organização para o serviço de web de repetição de escrita de palavras-passe. |
| 31005 | OnboardingEventSuccess | Este evento indica que o processo de integração foi concluída com êxito e de que a capacidade de repetição de escrita de palavras-passe está pronta a utilizar. |
| 31006 | ChangePasswordStart | Este evento indica que o serviço no local detetou um pedido de alteração de palavra-passe para um utilizador federado ou sincronizadas de hash de palavra-passe que origina da nuvem. Este evento é o primeiro evento na cada operação de repetição de escrita de alteração de palavra-passe. |
| 31007 | ChangePasswordSuccess | Este evento indica que um utilizador selecionada uma nova palavra-passe durante uma operação de alteração de palavra-passe, Determinámos que a palavra-passe cumpra os requisitos de palavra-passe empresarial, e que a palavra-passe tiver sido com êxito escrita de volta para o ambiente do Active Directory local. |
| 31008 | ChangePasswordFail | Este evento indica que um utilizador selecionado uma palavra-passe e que a palavra-passe chegou com êxito para o ambiente no local, mas ao tentar definir a palavra-passe no ambiente do Active Directory local, Ocorreu uma falha. Esta falha pode ocorrer por vários motivos: <br><ul><li>Palavra-passe do utilizador não cumprem a idade, o histórico, a complexidade ou filtrar os requisitos para o domínio. Para resolver este problema, crie uma nova palavra-passe.</li><li>A conta de serviço do ADMA não tem as permissões adequadas para definir a nova palavra-passe da conta de utilizador em causa.</li><li>A conta de utilizador está num grupo protegido, tais como admins do domínio ou a empresa, que não permite operações de conjunto de palavra-passe.</li></ul> |
| 31009 | ResetUserPasswordByAdminStart | O serviço no local detetada que pedido para um utilizador federado ou sincronizadas de hash de palavra-passe provenientes de administrador em nome de um utilizador de reposição de uma palavra-passe. Este evento é o primeiro evento na cada operação de repetição de escrita de reposição de palavra-passe é iniciada por um administrador. |
| 31010 | ResetUserPasswordByAdminSuccess | O administrador selecionado uma nova palavra-passe durante uma operação de reposição de palavra-passe iniciadas pelo administrador. Determinámos que esta palavra-passe cumpra os requisitos de palavra-passe empresarial. A palavra-passe tem foi escrita com êxito na novamente para o ambiente do Active Directory local. |
| 31011 | ResetUserPasswordByAdminFail | O administrador selecionado uma palavra-passe em nome de um utilizador. A palavra-passe chegou com êxito para o ambiente no local. Mas, ao tentar definir a palavra-passe no ambiente do Active Directory local, Ocorreu uma falha. Esta falha pode ocorrer por vários motivos: <br><ul><li>Palavra-passe do utilizador não cumprem a idade, o histórico, a complexidade ou filtrar os requisitos para o domínio. Tente uma palavra-passe nova para resolver este problema.</li><li>A conta de serviço do ADMA não tem as permissões adequadas para definir a nova palavra-passe da conta de utilizador em causa.</li><li>A conta de utilizador está num grupo protegido, tais como admins do domínio ou a empresa, que não permite operações de conjunto de palavra-passe.</li></ul>  |
| 31012 | OffboardingEventStart | Este evento ocorre se desativar a repetição de escrita de palavras-passe com o Azure AD Connect e indica que iniciamos descarregar o serviço de web de repetição de escrita de palavras-passe à sua organização. |
| 31013| OffboardingEventSuccess| Este evento indica que o processo de exclusão teve êxito e que capacidade de repetição de escrita de palavras-passe foi desativada com êxito. |
| 31014| OffboardingEventFail| Este evento indica que o processo de exclusão não foi concluída com êxito. Isto pode dever-se um erro de permissões da conta de administrador de nuvem ou no local especificado durante a configuração. O erro também pode ocorrer se estiver a tentar utilizar um administrador global da nuvem federado ao desativar a repetição de escrita de palavras-passe. Para corrigir este problema, verifique as permissões administrativas e certifique-se de que não está a utilizar uma conta federada ao configurar a capacidade de repetição de escrita de palavras-passe.|
| 31015| WriteBackServiceStarted| Este evento indica que o serviço de repetição de escrita de palavras-passe foi iniciado com êxito. Está pronto para aceitar pedidos de gestão de palavra-passe da nuvem.|
| 31016| WriteBackServiceStopped| Este evento indica que o serviço de repetição de escrita de palavras-passe foi parado. Quaisquer pedidos de gestão de palavra-passe da nuvem não será bem sucedidos.|
| 31017| AuthTokenSuccess| Este evento indica que iremos obteve com êxito a um token de autorização para o administrador global especificado durante a configuração do Azure AD Connect para iniciar o processo de exclusão ou inclusão.|
| 31018| KeyPairCreationSuccess| Este evento indica que foi criada com êxito a chave de encriptação da palavra-passe. Esta chave é utilizada para encriptar as palavras-passe da nuvem para serem enviados para o seu ambiente no local.|
| 32000| UnknownError| Este evento indica que ocorreu um erro desconhecido durante uma operação de gestão de palavra-passe. Observe o texto da exceção no evento para obter mais detalhes. Se tiver problemas, tente desativar e, em seguida, reativar a repetição de escrita de palavras-passe. Se isto não ajuda, inclua uma cópia do seu registo de eventos, juntamente com o controlo insider especificado do ID para o seu engenheiro de suporte.|
| 32001| ServiceError| Este evento indica que ocorreu um erro ao ligar para a palavra-passe da nuvem repor o serviço. Este erro ocorre normalmente quando o serviço no local não foi possível ligar ao serviço web de reposição de palavra-passe.|
| 32002| ServiceBusError| Este evento indica que ocorreu um erro ao ligar à instância de Service Bus do seu inquilino. Isto pode acontecer se está a bloquear ligações de saída no seu ambiente no local. Certifique-se a firewall para se certificar de que permitir ligações através de TCP 443 e para https://ssprsbprodncu-sb.accesscontrol.windows.net/ e, em seguida, tente novamente. Se ainda está a ter problemas, tente desativar e, em seguida, reativar a repetição de escrita de palavras-passe.|
| 32003| InPutValidationError| Este evento indica que a entrada transmitida a nossa API do serviço web era inválida. Repita a operação.|
| 32004| DecryptionError| Este evento indica que ocorreu um erro ao desencriptar a palavra-passe que chegaram da nuvem. Isto pode dever-se uma incompatibilidade de chave de desencriptação entre o serviço em nuvem e de ambiente no local. Para resolver este problema, desative e volte a ativar a repetição de escrita de palavras-passe no seu ambiente no local.|
| 32005| ConfigurationError| Durante a integração, iremos guardar informações específicas de inquilino num ficheiro de configuração no seu ambiente no local. Este evento indica que ocorreu um erro ao guardar este ficheiro ou que quando o serviço foi iniciado, Ocorreu um erro ao ler o ficheiro. Para corrigir este problema, tente desativar e, em seguida, reativar a repetição de escrita de palavras-passe para forçar um reescrever do ficheiro de configuração.|
| 32007| OnBoardingConfigUpdateError| Durante a integração, iremos enviar dados da nuvem para o local serviço de reposição de palavra-passe. Os dados, em seguida, são escritos num ficheiro dentro da memória antes de ser enviada para o serviço de sincronização para ser armazenadas em segurança no disco. Este evento indica que existe um problema com a escrever ou atualizar os dados na memória. Para corrigir este problema, tente desativar e, em seguida, reativar a repetição de escrita de palavras-passe para forçar um reescrever deste ficheiro de configuração.|
| 32008| ValidationError| Este evento indica que recebemos uma resposta inválida do serviço web de reposição de palavra-passe. Para corrigir este problema, tente desativar e, em seguida, reativar a repetição de escrita de palavras-passe.|
| 32009| AuthTokenError| Este evento indica que não foi possível obter uma autorização token para a conta de administrador global especificada durante a configuração do Azure AD Connect. Este erro pode ser causado por um nome de utilizador incorreto ou a palavra-passe especificada para a conta de administrador global. Este erro também pode ocorrer se a conta de administrador global especificada está federada. Para corrigir este problema, execute novamente a configuração com o nome de utilizador correto e a palavra-passe e certifique-se de que o administrador é uma conta de geridos (a apenas na nuvem ou sincronizados a palavra-passe).|
| 32010| CryptoError| Este evento indica que ocorreu um erro ao gerar a chave de encriptação da palavra-passe ou a desencriptação de uma palavra-passe que são recebidos a partir do serviço de nuvem. Este erro provavelmente indica um problema com o seu ambiente. Ver os detalhes do seu registo de eventos para obter mais informações sobre como resolver este problema. Também pode tentar desativar e, em seguida, voltando a reativar o serviço de repetição de escrita de palavras-passe.|
| 32011| OnBoardingServiceError| Este evento indica que o serviço no local não foi corretamente comunicar com o serviço web de reposição de palavra-passe para iniciar o processo de integração. Isto pode acontecer devido a uma regra de firewall ou se houver um problema ao obter uma autenticação de token para o seu inquilino. Para corrigir este problema, certifique-se de que não está a bloquear ligações de saída através de TCP 443 e TCP 9350-9354 ou para https://ssprsbprodncu-sb.accesscontrol.windows.net/. Certifique-se também que a conta de administrador do Azure AD, que está a utilizar com a sua integração não está federado.|
| 32013| OffBoardingError| Este evento indica que o serviço no local não foi corretamente comunicar com o serviço web de reposição de palavra-passe para iniciar o processo de exclusão. Isto pode acontecer devido a uma regra de firewall ou se houver um problema ao obter uma autorização token para o seu inquilino. Para corrigir este problema, certifique-se de que não está a bloquear ligações de saída através de 443 ou para https://ssprsbprodncu-sb.accesscontrol.windows.net/ e que a conta de administrador do Azure Active Directory que está a utilizar para offboard não está federado.|
| 32014| ServiceBusWarning| Este evento indica que tivemos de tentar para ligar à instância de Service Bus do seu inquilino. Em condições normais, este deve não ser uma preocupação, mas se vir este evento muitas vezes, considere verificar a sua ligação de rede ao Service Bus, especialmente se é uma ligação de latência elevada ou largura de banda baixa.|
| 32015| ReportServiceHealthError| Para monitorizar o estado de funcionamento do serviço de repetição de escrita da palavra-passe, enviar dados de heartbeat para o nosso serviço web de reposição de palavra-passe a cada cinco minutos. Este evento indica que ocorreu um erro ao enviar estas informações de estado de funcionamento para o serviço web de nuvem. Estas informações de estado de funcionamento não incluem um informações de identificação de objeto (OII) ou dados de informação identificativa (PII), não sendo puramente um heartbeat e estatísticas de serviço básico, para que o podermos fornecer informações de estado do serviço na nuvem.|
| 33001| ADUnKnownError| Este evento indica que ocorreu um erro desconhecido devolvido pelo Active Directory. Verifique o registo de eventos do servidor do Azure AD Connect para eventos da origem ADSync para obter mais informações.|
| 33002| ADUserNotFoundError| Este evento indica que o utilizador que está a tentar repor ou alterar uma palavra-passe não foi encontrado no diretório no local. Este erro pode ocorrer quando o utilizador tiver sido eliminado no local, mas não na nuvem. Este erro também pode ocorrer se houver um problema com a sincronização. Verifique os registos de sincronização e os detalhes de alguns a última sincronização executar para obter mais informações.|
| 33003| ADMutliMatchError| Ao repor uma palavra-passe ou pedido de alteração origina da nuvem, utilizamos a âncora de nuvem especificada durante o processo de configuração do Azure AD Connect para determinar como ligar esse pedido de volta para um utilizador no seu ambiente no local. Este evento indica que foram encontrados dois utilizadores no seu diretório no local com o mesmo atributo de âncora de nuvem. Verifique os registos de sincronização e os detalhes de alguns a última sincronização executar para obter mais informações.|
| 33004| ADPermissionsError| Este evento indica que a conta de serviço do agente de gestão do Active Directory (ADMA) não tem as permissões adequadas na conta em questão para definir uma nova palavra-passe. Certifique-se de que a conta ADMA na floresta do utilizador tem de reposição e alterar as permissões de palavra-passe em todos os objetos na floresta. Para obter mais informações sobre como definir as permissões, consulte o passo 4: configurar as permissões adequadas do Active Directory.|
| 33005| ADUserAccountDisabled| Este evento indica que iremos tentou repor ou alterar uma palavra-passe para uma conta que foi desativado no local. Ativar a conta e repita a operação.|
| 33006| ADUserAccountLockedOut| Este evento indica que iremos tentou repor ou alterar uma palavra-passe para uma conta que foi bloqueada no local. Bloqueios podem ocorrer quando um utilizador tem tentou uma alteração ou operação de palavra-passe demasiadas vezes num curto período de reposição. Desbloquear a conta e repita a operação.|
| 33007| ADUserIncorrectPassword| Este evento indica que o utilizador especificado uma palavra-passe atual incorreta, quando efetuar uma palavra-passe alterar a operação. Especifique a palavra-passe atual correta e tente novamente.|
| 33008| ADPasswordPolicyError| Este evento ocorre quando o serviço de repetição de escrita de palavras-passe tenta definir uma palavra-passe no diretório no local que não cumprem a idade da palavra-passe, histórico, complexidade ou requisitos de filtragem do domínio. <br> <br> Se tiver uma antiguidade mínima da palavra-passe e tiver alterado recentemente a palavra-passe dentro desse período de tempo, não está tiverem de alterar a palavra-passe novamente até atingir a duração especificada no seu domínio. Para fins de teste, a idade mínima deve ser definida como 0. <br> <br> Se tiver requisitos de histórico de palavras-passe ativados, então tem de selecionar uma palavra-passe que não tenha sido utilizada nos últimos *N* vezes, onde *N* é a definição do histórico de palavra-passe. Se selecionar uma palavra-passe que foi utilizada nos últimos *N* exceder o tempo, em seguida, neste caso, verá uma falha. Para fins de teste, o histórico de palavra-passe deve ser definido como 0. <br> <br> Se tiver requisitos de complexidade de palavra-passe, todos eles são aplicados quando o utilizador tenta alterar ou repor uma palavra-passe. <br> <br> Se tiver ativados de filtros de palavra-passe e um utilizador seleccionar uma palavra-passe que não cumpre os critérios de filtragem, em seguida, a reposição ou falha da operação de alteração.|
| 33009| ADConfigurationError| Este evento indica que ocorreu uma escrita de problema uma palavra-passe de volta para o seu diretório no local devido a um problema de configuração com o Active Directory. Verifique o registo de eventos de aplicações do computador do Azure AD Connect para mensagens do serviço ADSync para obter mais informações em que ocorreu o erro.|

## <a name="troubleshoot-password-writeback-connectivity"></a>Resolver problemas de conectividade de repetição de escrita de palavras-passe

Se estiver a ocorrer interrupções de serviço com o componente de repetição de escrita de palavras-passe do Azure AD Connect, seguem-se alguns passos rápidos que pode tomar para resolver este problema:

* [Confirmar a conectividade de rede](#confirm-network-connectivity)
* [Reinicie o serviço do Azure AD Connect sincronização](#restart-the-azure-ad-connect-sync-service)
* [Desative e volte a ativar a funcionalidade de repetição de escrita de palavras-passe](#disable-and-re-enable-the-password-writeback-feature)
* [Instalar a versão mais recente do Azure AD Connect](#install-the-latest-azure-ad-connect-release)
* [Resolver problemas de repetição de escrita de palavras-passe](#troubleshoot-password-writeback)

Em geral, para recuperar o seu serviço da forma mais rápida, recomendamos que executar estes passos pela ordem discutida anteriormente.

### <a name="confirm-network-connectivity"></a>Confirmar a conectividade de rede

O ponto mais comuns de falha é que a firewall e de ou portas de proxy e tempos limite de inatividade foram configurados incorretamente. 

Para o Azure AD Connect versão 1.1.443.0 e superior, tem do HTTPS saídas aceder ao seguinte:

   - passwordreset.microsoftonline.com
   - servicebus.windows.net

Para obter mais granularidade, referenciar a lista atualizada de [intervalos de IP do Microsoft Azure Datacenter](https://www.microsoft.com/download/details.aspx?id=41653) atualizado a cada quarta-feira e colocar em vigor o seguinte segunda-feira.

Para obter mais informações, reveja os pré-requisitos de conectividade no [pré-requisitos do Azure AD Connect](./connect/active-directory-aadconnect-prerequisites.md) artigo.



### <a name="restart-the-azure-ad-connect-sync-service"></a>Reinicie o serviço do Azure AD Connect sincronização

Para resolver problemas de conetividade ou outros problemas transitórios com o serviço, reinicie o serviço do Azure AD Connect Sync:

   1. Como administrador, selecione **iniciar** no servidor com o Azure AD Connect.
   2. Introduza **services.msc** no campo de procura e selecione **Enter**.
   3. Procure o **Microsoft Azure AD Sync** entrada.
   4. Faça duplo clique na entrada de serviço, selecione **reiniciar**e, em seguida, aguarde a conclusão da operação.

   ![Reinicie o serviço de sincronização do Azure AD][Service restart]

Estes passos restabelecer a ligação com o serviço de nuvem e resolva quaisquer interrupções que pode apresentar. Se reiniciar o serviço ADSync não resolver o problema, recomendamos que experimente desativar e, em seguida, volte a ativar a funcionalidade de repetição de escrita de palavras-passe.

### <a name="disable-and-re-enable-the-password-writeback-feature"></a>Desative e volte a ativar a funcionalidade de repetição de escrita de palavras-passe

Para resolver problemas de conectividade, desativar e, em seguida, volte a ativar a funcionalidade de repetição de escrita de palavras-passe:

   1. Como administrador, abra o Assistente de configuração de ligar do Azure AD.
   2. No **ligar para o Azure AD**, introduza as suas credenciais de administrador global do Azure AD.
   3. No **ligar ao AD DS**, introduza as credenciais de administrador de serviços de domínio do AD.
   4. No **identificar os utilizadores de forma exclusiva**, selecione o **seguinte** botão.
   5. No **funcionalidades opcionais**, desmarque a **repetição de escrita de palavras-passe** caixa de verificação.
   6. Selecione **seguinte** através as restantes páginas de diálogo sem alterar quaisquer até chegar à **pronto para configurar** página.
   7. Certifique-se de que o **pronto para Configurar página** mostra o **repetição de escrita de palavras-passe** opção como **desativada** e, em seguida, selecione a verde **configurar** botão para consolidar as alterações.
   8. No **concluído**, desmarque a **sincronizar agora** opção e, em seguida, selecione **concluir** para fechar o assistente.
   9. Volte a abrir o Assistente de configuração de ligar do Azure AD.
   10. Repita os passos 2-8, mas certifique-se de que seleciona o **repetição de escrita de palavras-passe** opção o **funcionalidades opcionais** página para reativar o serviço.

Estes passos restabelecer a ligação com o nosso serviço em nuvem e resolva quaisquer interrupções que pode apresentar.

Se desativar e, em seguida, volte a ativar a funcionalidade de repetição de escrita de palavras-passe não resolverem o problema, recomendamos que a reinstalação do Azure AD Connect.

### <a name="install-the-latest-azure-ad-connect-release"></a>Instalar a versão mais recente do Azure AD Connect

Reinstalar o Azure AD Connect pode resolver problemas de conectividade entre os nossos serviços de nuvem e de ambiente do Active Directory local e de configuração.

Recomendamos que efetue este passo apenas depois de tentarem os primeiros dois passos descritos anteriormente.

> [!WARNING]
> Se tiver personalizado as regras de sincronização de out of box, *de segurança-los antes de continuar com a atualização e, em seguida, voltá-los manualmente depois tiver terminado.*

   1. Transferir a versão mais recente do Azure AD Connect do [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=615771).
   2. Porque já tiver instalado o Azure AD Connect, terá de efetuar uma atualização no local para atualizar a instalação do Azure AD Connect para a versão mais recente.
   3. Executar o pacote transferido e siga o ecrã instruções para atualizar a máquina do Azure AD Connect.

Os passos anteriores devem voltar a estabelecer a ligação com o nosso serviço em nuvem e resolva quaisquer interrupções que pode apresentar.

Se instalar a versão mais recente do servidor do Azure AD Connect não resolver o problema, recomendamos que experimente desativar e, em seguida, reativar a repetição de escrita de palavras-passe como passo final depois de instalar a versão mais recente.

## <a name="verify-that-azure-ad-connect-has-the-required-permission-for-password-writeback"></a>Certifique-se de que o Azure AD Connect tem a permissão necessária para a repetição de escrita de palavras-passe

O Azure AD Connect necessita do Active Directory **Repor palavra-passe** permissão para efetuar a repetição de escrita de palavras-passe. Para saber se o Azure AD Connect tem a permissão necessária para uma conta de utilizador do Active Directory indicado no local, pode utilizar a funcionalidade de permissão Efetivo do Windows:

   1. Inicie sessão servidor do Azure AD Connect e iniciar o **Synchronization Service Manager** selecionando **iniciar** > **serviço de sincronização**.
   2. Sob o **conectores** separador, selecione o local **serviços de domínio do Active Directory** conector e, em seguida, selecione **propriedades**.  

   ![Permissão Efetivo - passo 2](./media/active-directory-passwords-troubleshoot/checkpermission01.png)  
  
   3. Na janela de pop-up, selecione **ligar à floresta do Active Directory** e anote o **nome de utilizador** propriedade. Esta propriedade é a conta de AD DS utilizada pelo Azure AD Connect para efetuar a sincronização de diretórios. Para o Azure AD Connect efetuar a repetição de escrita de palavras-passe, a conta do AD DS tem de ter de repor o permissão de palavra-passe.  
   
   ![Permissão Efetivo - passo 3](./media/active-directory-passwords-troubleshoot/checkpermission02.png) 
  
   4. Inicie sessão para um controlador de domínio no local e inicie a **computadores e utilizadores do Active Directory** aplicação.
   5. Selecione **vista** e certifique-se a **funcionalidades avançadas** opção está ativada.  
   
   ![Permissão Efetivo - passo 5](./media/active-directory-passwords-troubleshoot/checkpermission03.png) 
  
   6. Procure a conta de utilizador do Active Directory que pretende verificar. O nome da conta com o botão direito e selecione **propriedades**.  
   
   ![Permissão Efetivo - passo 6](./media/active-directory-passwords-troubleshoot/checkpermission04.png) 

   7. Na janela de pop-up, vá para o **segurança** separador e selecione **avançadas**.  
   
   ![Permissão Efetivo - passo 7](./media/active-directory-passwords-troubleshoot/checkpermission05.png) 
   
   8. No **definições avançadas de segurança de administrador** janela de pop-up, vá para o **acesso Efetivo** separador.
   9. Selecione **selecionar um utilizador**, selecione a conta do AD DS utilizada pelo Azure AD Connect (ver passo 3) e, em seguida, selecione **ver acesso Efetivo**.  
   
   ![Permissão Efetivo - passo 9](./media/active-directory-passwords-troubleshoot/checkpermission06.png) 
  
   10. Desloque para baixo e procure **Repor palavra-passe**. Se a entrada tem uma marca de verificação, a conta de AD DS tem permissão para repor a palavra-passe da conta de utilizador do Active Directory selecionada.  
   
   ![Permissão Efetivo - passo 10](./media/active-directory-passwords-troubleshoot/checkpermission07.png)  

## <a name="azure-ad-forums"></a>Fóruns do Azure AD

Se tiver uma pergunta geral acerca do Azure AD e de reposição de palavra-passe self-service, pode pedir da Comunidade para obter assistência sobre a [fóruns do Azure AD](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD). Membros da Comunidade do incluem engineers, gestores de produto, Most e para profissionais de TI.

## <a name="contact-microsoft-support"></a>Contacte o suporte da Microsoft

Se não conseguir encontrar a resposta a um problema, o nosso equipas de suporte estão sempre disponíveis para ajudá-lo a obter.

Para ajudar corretamente, pedimos-lhe fornecer a maior quantidade detalhe possível ao abrir um incidente. Estes detalhes incluem:

* **Descrição geral do erro**: o que é o erro? Qual era o comportamento que foi reparado? Como é que podemos recriar o erro? Fornece o mesmo detalhes quanto possível.
* **Página**: que página em estava quando é reparado o erro? Incluem o URL que se poderá e uma captura de ecrã da página.
* **Suporta código**: qual era o código de suporte que foi gerado quando o utilizador vimos o erro?
    * Para localizar este código, recriar o erro, em seguida, selecione o **suporta código** e enviar o engenheiro de suporte o GUID que resulta da ligação na parte inferior do ecrã.

    ![Encontrar o código de suporte na parte inferior do ecrã][Support code]

    * Se estiver numa página sem um código de suporte na parte inferior, selecione F12 e procure o SID e CID e enviar esses dois resultados ao engenheiro de suporte técnico.
* **Data, hora e fuso horário**: incluem a precisa data e hora *com o fuso horário* que ocorreu o erro.
* **ID de utilizador**: foi que o utilizador que vimos o erro? Um exemplo é  *user@contoso.com* .
    * Este é um utilizador federado?
    * Este é um utilizador de sincronização de hash de palavra-passe?
    * Este é um utilizador apenas na nuvem?
* **Licenciamento**: o utilizador tem uma licença do Azure AD Premium ou Basic do Azure AD atribuída?
* **Registo de eventos da aplicação**: Se estiver a utilizar a repetição de escrita de palavras-passe e o erro é na sua infraestrutura no local, inclua uma cópia zipped do seu registo de eventos de aplicações do servidor do Azure AD Connect.



[Service restart]: ./media/active-directory-passwords-troubleshoot/servicerestart.png "Reinicie o serviço de sincronização do Azure AD"
[Support code]: ./media/active-directory-passwords-troubleshoot/supportcode.png "O código de suporte está localizado no direito na parte inferior da janela"

## <a name="next-steps"></a>Passos Seguintes

Os artigos seguintes fornecem informações adicionais sobre a reposição através do Azure AD palavra-passe:

* [Como posso concluir uma implementação com êxito da SSPR?](active-directory-passwords-best-practices.md)
* [Repor ou alterar a palavra-passe](active-directory-passwords-update-your-own-password.md)
* [Registar-se na reposição personalizada de palavra-passe](active-directory-passwords-reset-register.md)
* [Tem alguma pergunta sobre licenciamento?](active-directory-passwords-licensing.md)
* [Que dados são utilizados pela SSPR e que dados devem ser preenchidos por si para os seus utilizadores?](active-directory-passwords-data.md)
* [Que métodos de autenticação estão disponíveis para os utilizadores?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Quais são as opções de política da SSPR?](active-directory-passwords-policy.md)
* [O que é a repetição de escrita de palavras-passe e por que me deve interessar?](active-directory-passwords-writeback.md)
* [Como posso comunicar a atividade da SSPR?](active-directory-passwords-reporting.md)
* [Quais são todas as opções na SSPR e o que significam?](active-directory-passwords-how-it-works.md)
* [Tenho uma pergunta que ainda não foi abordada](active-directory-passwords-faq.md)
