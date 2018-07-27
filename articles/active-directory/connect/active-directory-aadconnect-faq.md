---
title: Azure Active Directory Connect FAQ - | Documentos da Microsoft
description: Este artigo responde às perguntas mais frequentes sobre o Azure AD Connect.
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
ms.openlocfilehash: 1897f2c5d532c030b2711a35eabeefd889f560c8
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/26/2018
ms.locfileid: "39265000"
---
# <a name="azure-active-directory-connect-faq"></a>FAQ do Azure Active Directory Connect

## <a name="general-installation"></a>Instalação geral
**P: instalação funcionará se o Administrador Global do Azure Active Directory (Azure AD) tem a autenticação de dois fatores (2FA) ativada?**  
No momento da elaboração as compilações de Fevereiro de 2016, este cenário é suportado.

**P: existe uma forma de instalar o Azure AD Connect automático?**  
Instalação do Azure AD Connect só é suportada quando utiliza o Assistente de instalação. Não é suportada uma instalação automática e silenciosa.

**P: Tenho uma floresta em que um domínio não pode ser contactado. Como posso instalar o Azure AD Connect?**  
No momento da elaboração as compilações de Fevereiro de 2016, este cenário é suportado.

**P: o que o Azure Active Directory Domain Services funcionam de agente de estado de funcionamento (Azure AD DS) no server core?**  
Sim. Depois de instalar o agente, pode concluir o processo de registo utilizando o cmdlet PowerShell seguinte: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

**P: o Azure AD Connect suporte sincronizar a partir de dois domínios no Azure AD?**  
Sim, este cenário é suportado. Consulte a [vários domínios](active-directory-aadconnect-multiple-domains.md).
 
**P: pode ter vários conectores para o mesmo domínio do Active Directory no Azure AD Connect?**  
Não, vários conectores para o mesmo domínio do AD não são suportadas. 

**P: posso mover a base de dados do Azure AD Connect da base de dados local para uma instância remota do SQL Server?**   
Sim, os passos seguintes fornecem orientações gerais sobre como fazer isso. Estamos atualmente a trabalhar num documento mais detalhado.
1. Criar cópias de segurança da base de dados LocalDB ADSync.
A forma mais simples de fazer isso é usar o SQL Server Management Studio instalado no mesmo computador que o Azure AD Connect. Ligar à *(localdb)\.\ADSync*e, em seguida, criar cópias de segurança da base de dados do ADSync.

2. Restaure a base de dados ADSync à sua instância do SQL Server remota.

3. Instalar o Azure AD Connect contra existente [banco de dados do SQL remoto](active-directory-aadconnect-existing-database.md).
   O artigo demonstra como migrar para utilizar uma base de dados local do SQL. Se estiver a migrar para a utilização de uma base de dados do SQL remoto, no passo 5 do processo de também tem de introduzir uma conta de serviço que o serviço de sincronização do Windows será executado como. Esta conta de serviço de motor de sincronização é descrita aqui:
   
      **Utilizar uma conta de serviço existente**: por predefinição, o Azure AD Connect utiliza uma conta de serviço virtual para os serviços de sincronização para utilizar. Se utilizar uma instância remota do SQL Server ou utilizar um proxy que exija a autenticação, utilizar uma conta de serviço gerida ou uma conta de serviço no domínio e conhecer a palavra-passe. Nesses casos, introduza a conta a utilizar. Certifique-se de que os utilizadores que estejam a executar a instalação são os administradores de sistema no SQL, para que as credenciais de início de sessão para a conta de serviço podem ser criadas. Para obter mais informações, consulte [contas e permissões do Azure AD Connect](active-directory-aadconnect-accounts-permissions.md#adsync-service-account). 
   
      Com a compilação mais recente, o aprovisionamento da base de dados pode agora ser realizado fora de banda pelo administrador SQL e, em seguida, instalado pelo administrador do Azure AD Connect com direitos de proprietário da base de dados. Para obter mais informações, consulte [instalar o Azure AD Connect utilizando as permissões de administrador do SQL delegado](active-directory-aadconnect-sql-delegation.md).

Para simplificar as coisas, é recomendável que os utilizadores que instalar o Azure AD Connect ser os administradores de sistema no SQL. No entanto, com compilações recentes pode agora utilizar delegada administradores do SQL, conforme descrito em [instalar o Azure AD Connect utilizando as permissões de administrador do SQL delegado](active-directory-aadconnect-sql-delegation.md).

## <a name="network"></a>Rede
**P: Tenho um firewall, o dispositivo de rede ou outra coisa que limita a hora em que as ligações podem permanecer abertas na minha rede. O que deve meu limite de tempo limite do lado do cliente ser ao utilizar o Azure AD Connect?**  
Todos os softwares de sistema de rede, dispositivos físicos ou qualquer outra coisa que limita o tempo máximo que ligações podem permanecer abertas deve utilizar um limiar de, pelo menos, cinco minutos (300 segundos) para conectividade entre o servidor onde está instalado o cliente do Azure AD Connect e o Azure Active Directory. Esta recomendação aplica-se também de todas as ferramentas de sincronização do Microsoft Identity lançadas anteriormente.

**P: são suportados domínios de etiqueta única (SLDs)?**  
Enquanto é altamente recomendável em relação a esta configuração de rede ([consulte o artigo](https://support.microsoft.com/en-us/help/2269810/microsoft-support-for-single-label-domains)), através do Azure AD Connect com um domínio de etiqueta única for suportada, enquanto a configuração de rede para o domínio de nível único está a funcionar corretamente.

**P: são florestas com domínios de AD não contíguos suportados?**  
Não, do Azure AD Connect não suporta florestas no local que contêm espaços de nomes não contíguos.

**P: são "pontuada" nomes de NetBIOS suportados?**  
Não, do Azure AD Connect não suporta florestas no local ou em domínios em que o nome NetBIOS contém um ponto (.).

**P: é o ambiente de IPv6 puro suportada?**  
Não, do Azure AD Connect não suporta um ambiente IPv6 puro.

## <a name="federation"></a>de Federação
**P: o que devo fazer se eu recebo uma mensagem de e-mail que lhe pede-me para renovar o meu certificado do Office 365?**  
Para obter orientações sobre como renovar o certificado, veja [renovar certificados](active-directory-aadconnect-o365-certs.md).

**P: Tenho "Atualizar automaticamente a terceira parte confiável" definido para a entidade confiadora do Office 365. É necessário efetuar qualquer ação quando o meu token automaticamente o certificado de assinatura é agregado ao longo do?**  
Utilize as orientações que é descrita no artigo [renovar certificados](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Ambiente
**P: é possível mudar o nome do servidor após a instalação do Azure AD Connect?**  
Não. Alterar o nome do servidor renderiza o motor de sincronização não é possível ligar à instância de base de dados do SQL, e não é possível iniciar o serviço.

## <a name="identity-data"></a>Dados de identidade
**P: por que o atributo userPrincipalName (UPN) no Azure AD não corresponde ao UPN no local?**  
Para obter informações, veja estes artigos:

* [Nomes de utilizador no Office 365, o Azure ou o Intune não correspondem o UPN no local ou o ID de início de sessão alternativo](https://support.microsoft.com/en-us/kb/2523192)
* [As alterações não são sincronizadas pela ferramenta de sincronização do Azure Active Directory depois de alterar o UPN de uma conta de utilizador para utilizar um domínio federado diferente](https://support.microsoft.com/en-us/kb/2669550)

Também pode configurar o Azure AD para permitir que o mecanismo de sincronização atualizar o UPN, conforme descrito em [funcionalidades de serviço de sincronização do Azure AD Connect](active-directory-aadconnectsyncservice-features.md).

**P: é suportado para a configuração soft-correspondência de um grupo do Azure AD no local ou o objeto de contato com um grupo do Azure AD existente ou contacte o objeto?**  
Sim, essa correspondência de forma recuperável se baseia o proxyAddress. Correspondência de forma recuperável não é suportada para os grupos que não estão ativadas para correio.

**P: é possível definir o atributo de ImmutableId num grupo do Azure AD existente ou contactar o objeto para o disco rígido-match-lo a um grupo do Azure AD no local ou contacte o objeto manualmente?**  
Não, manualmente, definindo o atributo de ImmutableId num grupo existente do Azure AD ou o objeto de contacto para o disco rígido correspondência-lo atualmente não é suportada.

## <a name="custom-configuration"></a>Configuração personalizada
**P: onde estão documentados os cmdlets do PowerShell do Azure AD Connect?**  
Com exceção dos cmdlets que estão documentados neste site, os outros cmdlets do PowerShell encontrados no Azure AD Connect não são suportados para uso do cliente.

**P: Posso utilizar a opção "Importação do servidor de exportação/servidor" que se encontra no Synchronization Service Manager para mover a configuração entre servidores?**  
Não. Esta opção não recupera todas as definições de configuração e não deve ser utilizado. Em vez disso, utilize o Assistente para criar a configuração de base num segundo servidor e utilize o editor de regra de sincronização para gerar scripts do PowerShell para mover qualquer regra personalizada entre servidores. Para obter mais informações, consulte [migração rotativa](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).

**P: as palavras-passe ser colocados em cache para a página de início de sessão do Azure, e esta colocação em cache não consiga porque contém um elemento de entrada de palavra-passe com o *preenchimento automático = "false"* atributo?**  
Atualmente, modificar os atributos HTML do **palavra-passe** campo, incluindo a marca de preenchimento automático, não é suportado. Estamos atualmente a trabalhar num recurso que permite para JavaScript personalizada, que lhe permite adicionar qualquer atributo para o **palavra-passe** campo.

**P: a página de início de sessão do Azure apresenta os nomes de utilizador de utilizadores que se anteriormente tem iniciado sessão com êxito. Esse comportamento pode ser seja desativado?**  
Atualmente, modificar os atributos HTML do **palavra-passe** campo de entrada, incluindo a marca de preenchimento automático, não é suportado. Estamos atualmente a trabalhar num recurso que permite para JavaScript personalizada, que lhe permite adicionar qualquer atributo para o **palavra-passe** campo.

**P: existe uma forma de impedir sessões simultâneas?**  
Não.

## <a name="auto-upgrade"></a>Atualização automática

**P: quais são as vantagens e as conseqüências de usar automaticamente a atualização?**  
Podemos são aconselhá todos os clientes a ativar a atualização automática para a sua instalação do Azure AD Connect. A vantagem é que sempre recebem os patches mais recentes, incluindo atualizações de segurança a vulnerabilidades que foram encontrados no Azure AD Connect. O processo de atualização é tranqüila e ocorre automaticamente assim que uma nova versão estiver disponível. Vários milhares de clientes do Azure AD Connect utilizam a atualização automática com cada nova versão.

O processo de atualização automática sempre primeiro estabelece se uma instalação é elegível para atualização automática. Se for elegível, a atualização é executada e testada. O processo inclui também à procura de alterações personalizadas a regras e fatores ambientais específicos. Se os testes mostrarem que uma atualização é concluída com êxito, a versão anterior é automaticamente restaurada.

Dependendo do tamanho do ambiente, o processo pode demorar algumas horas. Enquanto a atualização está em curso, acontece sem sincronização entre o Windows Server Active Directory e o Azure AD.

**P: foi recebido um e-mail informando-me que meu atualização automática já não funciona e preciso de instalar uma nova versão. Por que motivo é necessário fazer isso?**  
No ano passado, lançámos uma versão do Azure AD Connect, que, em determinadas circunstâncias, poderá ter desativado a funcionalidade de atualização automática no seu servidor. Podemos corrigir o problema no Azure AD Connect versão 1.1.750.0. Se tiver sido afetados pelo problema, pode mitigá-lo ao executar um script do PowerShell para o reparar ou atualizando manualmente para a versão mais recente do Azure AD Connect. 

Para executar o script do PowerShell [transferir o script](https://aka.ms/repairaadconnect) e executá-lo no seu servidor do Azure AD Connect numa janela do PowerShell administrativa. Para saber como executar o script [visualizar este vídeo breve](https://aka.ms/repairaadcau).

Para atualizar manualmente, tem de transferir e executar a versão mais recente do ficheiro AADConnect.msi.
 
-  Se a sua versão atual é anterior 1.1.750.0, [transferir e atualizar para a versão mais recente](https://www.microsoft.com/en-us/download/details.aspx?id=47594).
- Se a sua versão do Azure AD Connect é 1.1.750.0 ou posterior, não é necessária nenhuma ação adicional. Já está a utilizar a versão que contém a correção de atualização automática. 

**P: foi recebido um e-mail informando-me para atualizar para a versão mais recente para voltar a ativar atualização automática. Estou usando a versão 1.1.654.0. É necessário atualizar?**  
Sim, terá de atualizar para versão 1.1.750.0 ou posterior para ativar novamente a atualização automática. [Transferir e atualizar para a versão mais recente](https://www.microsoft.com/en-us/download/details.aspx?id=47594).

**P: foi recebido um e-mail informando-me para atualizar para a versão mais recente para voltar a ativar atualização automática. Se o ter utilizado o PowerShell para ativar a atualização automática, é ainda necessário instalar a versão mais recente?**  
Sim, ainda terá de atualizar para versão 1.1.750.0 ou posterior. Ativar o serviço de atualização automática com o PowerShell não mitigar o problema de atualização automática encontrado nas versões antes 1.1.750.0.

**P: eu quero atualizar para uma versão mais recente, mas não tenho a certeza de quem instalado do Azure AD Connect, e não temos o nome de utilizador e palavra-passe. Precisamos isso?**
Não precisa de saber o nome de utilizador e palavra-passe que foi primeiramente usada para atualizar o Azure AD Connect. Utilize qualquer conta do Azure AD que tenha a função de Administrador Global.

**P: como posso saber qual é a versão do Azure AD Connect, estou usando?**  
Para verificar a versão do Azure AD Connect está instalada no seu servidor, vá para Painel de controle e procurar a versão instalada do Microsoft Azure AD Connect, selecionando **programas** > **programas e funcionalidades** , como mostrado aqui:

![Versão do Azure AD Connect no painel de controlo](media/active-directory-aadconnect-faq/faq1.png)

**P: como posso atualizar para a versão mais recente do Azure AD Connect?**  
Para saber como atualizar para a versão mais recente, veja [do Azure AD Connect: atualizar de uma versão anterior para a versão mais recente](active-directory-aadconnect-upgrade-previous-version.md). 

**P: nós já atualizado para a versão mais recente do Azure AD Connect no ano passado. Precisamos atualizar novamente?**  
A equipa do Azure AD Connect torna as atualizações frequentes para o serviço. Para se beneficiar de correções de erros e atualizações de segurança, bem como novas funcionalidades, é importante manter o seu servidor atualizados com a versão mais recente. Se ativar a atualização automática, a sua versão de software é atualizado automaticamente. Para obter o histórico de versões do Azure AD Connect, consulte [do Azure AD Connect: histórico de versões](active-directory-aadconnect-version-history.md).

**P: quanto tempo demora para fazer a atualização e o que é o impacto sobre os meus utilizadores?**  
O tempo necessário para atualizar depende do tamanho do seu inquilino. Para organizações maiores, será melhor efetuar a atualização na noite ou ao fim de semana. Durante a atualização, nenhuma atividade de sincronização ocorre.

**P: acredito que eu atualizado para o Azure AD Connect, mas o portal do Office ainda menciona DirSync. Por que é isto?**  
A equipe do Office está trabalhando para obter as atualizações de portais do Office para refletir o nome do produto atual. Não reflete qual ferramenta de sincronização estiver a utilizar.

**P: o meu estado de atualização automática disser, "Suspenso". Por que é esta suspensa? Deve habilitá-lo?**  
Um bug foi introduzido numa versão anterior que, em determinadas circunstâncias, deixa o estado de atualização automática definido como "Suspenso". Manualmente permitindo que ele é tecnicamente possível, mas requer vários passos complexos. A melhor coisa que pode fazer é instalar a versão mais recente do Azure AD Connect.

**P: a minha empresa tem requisitos de gerenciamento de alterações strict, e quero controlar quando ele foi enviado. Posso controlar quando é iniciada a atualização automática?**  
Não, não existe nenhum desses recursos hoje mesmo. O recurso está sendo avaliado para uma versão futura.

**P: Posso obterá uma mensagem de e-mail se a atualização automática falhou? Como posso saber se foi bem-sucedida?**  
Não será notificado do resultado da atualização. O recurso está sendo avaliado para uma versão futura.

**P: publica uma linha cronológica para quando planeia lançar das atualizações automáticas?**  
Atualização automática é a primeira etapa no processo de liberação de uma versão mais recente. Sempre que houver uma nova versão, as atualizações são enviadas automaticamente. As versões mais recentes do Azure AD Connect são previamente anunciadas no [plano do Azure AD](../fundamentals/whats-new.md).

**P: o atualização automática também atualizar o Azure AD Connect Health?**  
Sim, a atualização automática também atualiza o Azure AD Connect Health.

**P: também atualização automática servidores Azure AD Connect no modo de teste?**  
Sim, pode atualização automática um servidor do Azure AD Connect que está no modo de teste.

**P: se a falha de atualização automática e meu servidor do Azure AD Connect não for iniciado, o que devo fazer?**  
Em casos raros, o serviço do Azure AD Connect não iniciar depois de efetuar a atualização. Nestes casos, o reinício do servidor normalmente corrige o problema. Se o serviço do Azure AD Connect ainda não iniciar, abra um pedido de suporte. Para obter mais informações, consulte [criar um pedido de serviço para contactar o suporte do Office 365](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/). 

**P: não sei quais são os riscos quando atualizar para uma versão mais recente do Azure AD Connect. Pode ligar-me para me ajudar com a atualização?**  
Se precisar de ajuda a atualizar para uma versão mais recente do Azure AD Connect, abra um pedido de suporte em [criar um pedido de serviço para contactar o suporte do Office 365](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/).

## <a name="troubleshooting"></a>Resolução de problemas
**P: como posso obter ajuda com o Azure AD Connect?**

[Pesquise na Base de dados de Conhecimento Microsoft (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* Procure o KB para soluções técnicas para questões de break-fix comuns sobre o suporte do Azure AD Connect.

[Fóruns do Azure Active Directory](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* Pesquise para perguntas e respostas técnicas ou faça suas próprias perguntas, acedendo a [a Comunidade do Azure AD](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Obter suporte para o Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto)
