---
title: Azure Active Directory Connect FAQ – | Microsoft Docs
description: Este artigo responde a perguntas mais frequentes sobre o Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 4e47a087-ebcd-4b63-9574-0c31907a39a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 7edabc99da5e1466e848336c647a33213c9edd8b
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37132994"
---
# <a name="azure-active-directory-connect-faq"></a>FAQ do Azure Active Directory Connect

## <a name="general-installation"></a>Instalação geral
**P: instalação funcionará se o Administrador Global do Azure Active Directory (Azure AD) tem de ativar a autenticação de dois fatores (2FA)?**  
A partir de compilações de Fevereiro de 2016, este cenário é suportado.

**P: existe uma forma de instalar o Azure AD Connect automático?**  
Instalação do Azure AD Connect só é suportada quando utiliza o Assistente de instalação. Uma instalação autónoma, automática não é suportada.

**P: Tenho uma floresta em que um domínio não pode ser contactado. Como instalar o Azure AD Connect?**  
A partir de compilações de Fevereiro de 2016, este cenário é suportado.

**P: é o Azure Active Directory Domain Services agente de estado de funcionamento (Azure AD DS) funcionam no server core?**  
Sim. Depois de instalar o agente, pode concluir o processo de registo utilizando o cmdlet PowerShell seguinte: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

**P: é o Azure AD Connect suporte a sincronização de dois domínios no Azure AD?**  
Sim, este cenário é suportado. Consulte [vários domínios](active-directory-aadconnect-multiple-domains.md).
 
**P: pode ter vários conectores para o mesmo domínio do Active Directory no Azure AD Connect?**  
Não, vários conectores para o mesmo domínio do AD não são suportados. 

**P: posso mover a base de dados do Azure AD Connect da base de dados local para uma instância remota do SQL Server?**   
Sim, os passos seguintes fornecem orientações gerais sobre como fazê-lo. Estamos atualmente a trabalhar num documento mais detalhado.
1. Cópia de segurança da base de dados LocalDB ADSync.
A forma mais simples para o fazer consiste em utilizar o SQL Server Management Studio instalado no mesmo computador como o Azure AD Connect. Ligar ao *(localdb)\.\ADSync*e, em seguida, efetuar cópias de segurança da base de dados ADSync.

2. Restaure a base de dados ADSync à sua instância do SQL Server remota.

3. Instalar o Azure AD Connect contra existente [base de dados remota do SQL Server](active-directory-aadconnect-existing-database.md).
   O artigo demonstra como migrar para a base de dados local do SQL Server. Se estiver a migrar para a utilização de uma base de dados do SQL Server remoto, no passo 5 do processo também tem de introduzir uma conta de serviço existente que o serviço de sincronização do Windows irão executar como. Esta conta de serviço de motor de sincronização é descrita aqui:
   
      **Utilizar uma conta de serviço existente**: por predefinição, o Azure AD Connect utiliza uma conta de serviço virtuais para os serviços de sincronização a utilizar. Se utilizar uma instância remota do SQL Server ou utilizar um proxy que requeira autenticação, utilize uma conta de serviço gerida ou uma conta de serviço no domínio e conhecer a palavra-passe. Nesses casos, introduza a conta a utilizar. Certifique-se de que os utilizadores que estejam a executar a instalação são administradores de sistema no SQL Server para que as credenciais de início de sessão para a conta de serviço podem ser criadas. Para obter mais informações, consulte [contas e permissões do Azure AD Connect](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account). 
   
      Com a compilação mais recente, o aprovisionamento da base de dados pode agora ser realizado fora de banda pelo administrador SQL e, em seguida, instalado pelo administrador do Azure AD Connect com direitos de proprietário da base de dados. Para obter mais informações, consulte [instalar o Azure AD Connect através da utilização de permissões de administrador do SQL Server delegado](active-directory-aadconnect-sql-delegation.md).

Para manter as coisas simples, é recomendável que os utilizadores que instalar o Azure AD Connect ser administradores de sistema no SQL Server. No entanto, com compilações recentes pode agora utilizar delegado administradores do SQL Server, conforme descrito em [instalar o Azure AD Connect utilizando as permissões de administrador do SQL delegado](active-directory-aadconnect-sql-delegation.md).

## <a name="network"></a>Rede
**P: Posso ter uma firewall, o dispositivo de rede ou outra coisa que limita a hora em que as ligações podem permanecer abertas na minha rede. O meu limiar do tempo limite do lado do cliente estará quando utilizar o Azure AD Connect?**  
Todo o software de rede, dispositivos físicos ou há mais alguma coisa que limita o tempo máximo que as ligações podem permanecer abertas deve utilizar um limiar de, pelo menos, cinco minutos (300 segundos) para a conectividade entre o servidor onde está instalado o cliente do Azure AD Connect e o Azure Active Directory. Esta recomendação também se aplica a todas as ferramentas de sincronização do Microsoft Identity lançadas anteriormente.

**P: são suportados domínios de etiqueta única (SLDs)?**  
Não, do Azure AD Connect não suporta florestas no local ou em domínios que utilizam SLDs.

**P: são florestas com domínios de AD não contíguo suportados?**  
Não, do Azure AD Connect não suporta florestas no local que contêm espaços de nomes não contíguo.

**P: são "separada por pontos" nomes NetBIOS suportados?**  
Não, do Azure AD Connect não suporta florestas no local ou domínios em que o nome NetBIOS contém um ponto (.).

**P: puro IPv6 ambiente é suportado?**  
Não, do Azure AD Connect não suporta um ambiente IPv6 puro.

## <a name="federation"></a>de Federação
**P: o que posso fazer se receber uma mensagem de e-mail a perguntar-me para renovar certificado meu do Office 365?**  
Para obter orientações sobre como renovar o certificado, consulte [renovar certificados](active-directory-aadconnect-o365-certs.md).

**P: Posso ter "Atualizar automaticamente da entidade confiadora" definido para a entidade confiadora do Office 365. É necessário efetuar qualquer ação, quando faz o meu automaticamente o certificado de assinatura de token através de?**  
Utilize as orientações que é descrita no artigo [renovar certificados](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Ambiente
**P: é suportado para mudar o nome do servidor após a instalação do Azure AD Connect?**  
Não. Alterar o nome do servidor compõe o motor de sincronização não é possível ligar à instância de base de dados do SQL e não é possível iniciar o serviço.

## <a name="identity-data"></a>Dados de identidade
**P: por que motivo o atributo userPrincipalName (UPN) no Azure AD não coincide com o UPN no local?**  
Para informações, consulte estes artigos:

* [Nomes de utilizador no Office 365, Azure ou Intune não correspondem a UPN no local ou o ID de início de sessão alternativo](https://support.microsoft.com/en-us/kb/2523192)
* [As alterações não são sincronizadas pela ferramenta de sincronização do Azure Active Directory depois de alterar o UPN de uma conta de utilizador para utilizar um diferente domínio federado](https://support.microsoft.com/en-us/kb/2669550)

Também pode configurar o Azure AD para permitir que o motor de sincronização atualizar o UPN, conforme descrito em [funcionalidades do serviço de sincronização do Azure AD Connect](active-directory-aadconnectsyncservice-features.md).

**P: é suportado para configuração soft-correspondência de um grupo do Azure AD no local ou do objeto de contacto com um grupo do Azure AD existente ou contactar o objeto?**  
Sim, esta correspondência de forma recuperável baseiam o /proxyaddress. Correspondência de forma recuperável não é suportada para os grupos que não estão ativadas para correio.

**P: é suportado para manualmente defina o atributo ImmutableId num grupo do Azure AD existente ou contactar o objeto para o disco rígido-match-la a um grupo do Azure AD no local ou contactar o objeto?**  
Não, definição manualmente o atributo ImmutableId um grupo do Azure AD existente ou o objeto de contacto para o disco rígido-match-não é atualmente suportada.

## <a name="custom-configuration"></a>Configuração personalizada
**P: onde estão documentados os cmdlets do PowerShell do Azure AD Connect?**  
Com exceção dos cmdlets que estão documentados neste site, os outros cmdlets do PowerShell encontrados no Azure AD Connect não são suportados para utilização do cliente.

**P: Posso utilizar a opção de "Importação do servidor de exportação/servidor" for encontrada no serviço Gestor de sincronização para mover a configuração entre servidores?**  
Não. Esta opção não obter todas as definições de configuração e não deve ser utilizada. Em vez disso, utilize o Assistente para criar a configuração base no segundo servidor e utilize o editor de regra de sincronização para gerar scripts do PowerShell para mover qualquer regra personalizada entre servidores. Para obter mais informações, consulte [Swing migração](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).

**P: as palavras-passe ser colocadas em cache para a página de início de sessão do Azure e pode esta colocação em cache impedida porque contém um elemento de entrada de palavra-passe com o *autocomplete = "false"* atributo?**  
Atualmente, modificar os atributos HTML do **palavra-passe** campo, incluindo a tag de conclusão automática, não é suportado. Estamos atualmente uma funcionalidade que permite JavaScript personalizado, o que lhe permite adicionar qualquer atributo para o **palavra-passe** campo.

**P: a página de início de sessão do Azure apresenta os nomes de utilizador de utilizadores que tenham anteriormente sessão iniciada com êxito. Este comportamento pode ser ativado?**  
Atualmente, modificar os atributos HTML do **palavra-passe** campo de entrada, incluindo a tag de conclusão automática, não é suportado. Estamos atualmente uma funcionalidade que permite JavaScript personalizado, o que lhe permite adicionar qualquer atributo para o **palavra-passe** campo.

**P: existe uma forma de impedir sessões simultâneas?**  
Não.

## <a name="auto-upgrade"></a>Atualização automática

**P: quais são as vantagens e consequências da utilização de auto atualização?**  
Iremos são indicar todos os clientes para ativar a atualização automática para a instalação do Azure AD Connect. A vantagem é que recebem sempre as correções mais recentes, incluindo atualizações de segurança de vulnerabilidades que foram encontradas no Azure AD Connect. O processo de atualização é sem esforços e ocorre automaticamente assim que estiver disponível uma nova versão. Muitos milhares de clientes do Azure AD Connect utilizam a atualização automática com cada nova versão.

O processo de atualização automática estabelece sempre primeiro se uma instalação é elegível para atualização automática. Se é elegível, a atualização é executada e testada. O processo também inclui a procura alterações personalizadas para as regras e os fatores ambientais específicos. Se os testes mostram que uma atualização sem êxito, a versão anterior é automaticamente restaurada.

Dependendo do tamanho do ambiente, o processo pode demorar algumas horas. Enquanto a atualização está em curso, acontece sem sincronização entre o Windows Server Active Directory e o Azure AD.

**P: Posso recebeu uma mensagem de e-mail a informar-me que já não funciona a minha atualização automática e preciso de instalar uma nova versão. Por que motivo é necessário fazê-lo?**  
Último ano, lançada uma versão do Azure AD Connect que, em determinadas circunstâncias, poderá ter desativada a funcionalidade de atualização automática no seu servidor. Iremos corrigir o problema no Azure AD Connect versão 1.1.750.0. Se foram afetados pelo problema, pode mitigá-lo executando um script do PowerShell para o reparar ou atualizar manualmente para a versão mais recente do Azure AD Connect. 

Para executar o script do PowerShell [transferir o script](https://aka.ms/repairaadconnect) e executá-lo no seu servidor do Azure AD Connect numa janela do PowerShell administrativa. Para saber como executar o script, [ver neste breve vídeo](https://aka.ms/repairaadcau).

Para atualizar manualmente, tem de transferir e executar a versão mais recente do ficheiro AADConnect.msi.
 
-  Se a versão atual é anterior ao 1.1.750.0, [transferir e atualizar para a versão mais recente](https://www.microsoft.com/en-us/download/details.aspx?id=47594).
- Se a sua versão do Azure AD Connect é 1.1.750.0 ou posterior, não é necessária nenhuma ação adicional. Já está a utilizar a versão que contém a correção de atualização automática. 

**P: Posso recebeu uma mensagem de e-mail a informar-me para atualizar para a versão mais recente para reativar a atualização automática. Estou a utilizar a versão 1.1.654.0. É necessário atualizar?**  
Sim, terá de atualizar para a versão 1.1.750.0 ou posterior para reativar a atualização automática. [Transfira e atualize para a versão mais recente](https://www.microsoft.com/en-us/download/details.aspx?id=47594).

**P: Posso recebeu uma mensagem de e-mail a informar-me para atualizar para a versão mais recente para reativar a atualização automática. Se tiver utilizado o PowerShell para ativar a atualização automática, é ainda necessário instalar a versão mais recente?**  
Sim, é preciso atualizar para versão 1.1.750.0 ou posterior. Ativar o serviço de atualização automática com o PowerShell não mitigar o problema de atualização automática encontrado em versões antes 1.1.750.0.

**P: Posso pretende atualizar para uma versão mais recente, mas não estou a certeza de que instalar o Azure AD Connect e não temos o nome de utilizador e palavra-passe. Precisamos disto?**
Não precisa de saber o nome de utilizador e palavra-passe que foi inicialmente utilizada para atualizar o Azure AD Connect. Utilize qualquer conta do Azure AD que tenha a função de Administrador Global.

**P: como posso saber qual é a versão do Azure AD Connect estiver a utilizar?**  
Para verificar qual é a versão do Azure AD Connect está instalada no seu servidor, aceda ao painel de controlo e procurar a versão instalada do Microsoft Azure AD Connect, selecionando **programas** > **programas e funcionalidades** , conforme mostrado aqui:

![Versão do Azure AD Connect no painel de controlo](media/active-directory-aadconnect-faq/faq1.png)

**P: como atualizar a versão mais recente do Azure AD Connect?**  
Para saber como atualizar para a versão mais recente, consulte [do Azure AD Connect: atualização a partir de uma versão anterior para a versão mais recente](active-directory-aadconnect-upgrade-previous-version.md). 

**P: estamos já atualizado para a versão mais recente do Azure AD Connect último ano. É necessário atualizar novamente?**  
A equipa do Azure AD Connect faz atualizações frequentes para o serviço. Para tirar partido de correções e atualizações de segurança, bem como novas funcionalidades, é importante manter o seu servidor atualizado com a versão mais recente. Se ativar a atualização automática, a sua versão do software é atualizada automaticamente. Para obter o histórico de lançamento da versão do Azure AD Connect, consulte [do Azure AD Connect: histórico de lançamento da versão](active-directory-aadconnect-version-history.md).

**P: quanto tempo demora a efetuar a atualização, e o que é o impacto nos meus utilizadores?**  
O tempo necessário para atualizar depende do tamanho do seu inquilino. Para organizações de maior, poderá ser melhor efetuar a atualização no evening ou fim de semana. Durante a atualização, nenhuma atividade de sincronização ocorre.

**P: posso considerar que posso atualizado para o Azure AD Connect, mas o portal do Office ainda menciona suportadas DirSync. Por que motivo é isto?**  
A equipa do Office está a trabalhar para obter atualizações ao portal do Office para refletir o nome de produto atual. Não reflete que estiver a utilizar a ferramenta de sincronização.

**P: meu estado de atualização automática diz: "Suspenso". Por que motivo está-suspenso? Deve posso ativá-la?**  
Foi introduzido um erro numa versão anterior que, em determinadas circunstâncias, deixa o estado de atualização automática definido como "Suspenso". Ativar manualmente é tecnicamente possível mas seria necessários vários passos complexos. A melhor coisa que pode fazer é instalar a versão mais recente do Azure AD Connect.

**P: minha empresa tem requisitos de gestão de alterações strict e quiser controlar quando este foi enviado. Posso controlar quando é iniciada a atualização automática?**  
Não, não há nenhum essa funcionalidade hoje. A funcionalidade está a ser avaliada para uma versão futura.

**P: Posso obterá uma mensagem de e-mail se a atualização automática falhou? Como posso saber que foi efetuada com êxito?**  
Não será notificado do resultado da atualização. A funcionalidade está a ser avaliada para uma versão futura.

**P: publicar numa linha cronológica para quando pretender emitir saída atualizações automática?**  
Atualização automática é o primeiro passo no processo de lançamento de uma versão mais recente. Sempre que exista uma nova versão, as atualizações são instaladas automaticamente. As versões mais recentes do Azure AD Connect são previamente anunciadas no [do Azure AD plano](../fundamentals/whats-new.md).

**P: atualização automática também atualizar o Azure AD Connect Health?**  
Sim, a atualização automática também atualiza o Azure AD Connect Health.

**P: é também atualização automática do Azure AD Connect servidores no modo de teste?**  
Sim, é possível atualização automática um servidor do Azure AD Connect que está no modo de teste.

**P: se falha de atualização automática e meu servidor do Azure AD Connect não iniciar, o que devo fazer?**  
Em casos raros, não iniciar o serviço do Azure AD Connect após efetuar a atualização. Nestes casos, o reinício do servidor normalmente corrige o problema. Se o serviço do Azure AD Connect ainda não iniciar, abra um pedido de suporte. Para obter mais informações, consulte [criar um pedido de serviço para contactar o suporte do Office 365](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/). 

**P: não sou se quais são os riscos durante a atualização para uma versão mais recente do Azure AD Connect. Pode telefonar-me para o ajudar-me com a atualização?**  
Se precisar de ajuda a atualização para uma versão mais recente do Azure AD Connect, abra um pedido de suporte em [criar um pedido de serviço para contactar o suporte do Office 365](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/).

## <a name="troubleshooting"></a>Resolução de problemas
**P: como posso obter ajuda com o Azure AD Connect?**

[Procurar na Base de dados de Conhecimento Microsoft (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* Procure o KB para a technical soluções para problemas de break-fix comuns sobre o suporte do Azure AD Connect.

[Fóruns do Azure Active Directory](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* Procurar técnicas perguntas e respostas ou fazer as suas próprias perguntas, acedendo a [Comunidade do Azure AD](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Obter suporte para o Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto)
