---
title: 'Do Azure Active Directory Connect: FAQ – | Microsoft Docs'
description: Esta página tem perguntas mais frequentes sobre o Azure AD Connect.
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
ms.date: 05/30/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: dbe6f5f6f3aa128b3180c1b7aecb17853aa6a0aa
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34801403"
---
# <a name="frequently-asked-questions-for-azure-active-directory-connect"></a>Perguntas mais frequentes sobre Azure Active Directory Connect

## <a name="general-installation"></a>Instalação geral
**P: instalação funcionará se o Administrador Global do AD do Azure tiver 2FA ativado?**  
Com as compilações de Fevereiro de 2016, este cenário é suportado.

**P: existe uma forma de instalar o Azure AD Connect automático?**  
Só é suportada para instalar o Azure AD Connect, utilizando o Assistente de instalação. Não é suportada uma instalação automática e silenciosa.

**P: Tenho uma floresta em que um domínio não pode ser contactado. Como instalar o Azure AD Connect?**  
Com as compilações de Fevereiro de 2016, este cenário é suportado.

**P: o agente de estado de funcionamento de AD DS funciona no server core?**  
Sim. Depois de instalar o agente, pode concluir o processo de registo utilizando o cmdlet PowerShell seguinte: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

**P: AADConnect suporta a sincronizar a partir de dois domínios no Azure AD?**</br>
Sim, este cenário é suportado. Consulte [vários domínios](active-directory-aadconnect-multiple-domains.md)
 
**P: pode ter vários conectores para o mesmo domínio do Active Directory no Azure AD connect?**</br> Não, vários conectores para o mesmo domínio do AD não é suportada. 

## <a name="network"></a>Rede
**P: Posso tem uma firewall, o dispositivo de rede ou outra coisa que limita as ligações de tempo máximo pode permanecer aberta na minha rede. Quanto tempo os meus limiar do tempo limite do lado do cliente estará ao utilizar o Azure AD Connect?**  
Todo o software de rede, dispositivos físicos ou há mais alguma coisa que limita o tempo máximo de ligações podem permanecer abertas deve utilizar um limiar de, pelo menos, 5 minutos (300 segundos) para a conectividade entre o servidor onde está instalado o cliente do Azure AD Connect e Azure Active Directory. Esta recomendação também se aplica a todas as ferramentas de sincronização do Microsoft Identity lançadas anteriormente.

**P: são SLDs (domínios de etiqueta única) suportada?**  
Não, do Azure AD Connect não suporta local florestas/domínios SLDs a utilizar.

**P: são florestas com domínios de AD não contíguo suportados?**  
Não, do Azure AD Connect não suporta florestas no local que contém os espaços de nomes não contíguo.

**P: são "separada por pontos" com o nome de NetBios suportado?**  
Não, do Azure AD Connect não suporta florestas/domínios de no local onde o nome NetBios contenha um ponto final "." no nome.

**P: puro IPv6 ambiente é suportado?**  
Não, do Azure AD Connect não suporta puro ambiente IPv6.

## <a name="federation"></a>de Federação
**P: o que posso fazer se receber uma mensagem de e-mail que perguntar-me para renovar certificado meu do Office 365**  
Utilize as orientações que está destacada no [renovar certificados](active-directory-aadconnect-o365-certs.md) documento sobre como renovar o certificado.

**P: Posso ter "Atualizar automaticamente da entidade confiadora" definido para da entidade confiadora do Office 365. É necessário efetuar qualquer ação, quando faz o meu automaticamente o certificado de assinatura de token através de?**  
Utilize as orientações que é descrita no artigo [renovar certificados](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Ambiente
**P: é suportado para mudar o nome do servidor após a instalação do Azure AD Connect?**  
Não. Alterar o nome do servidor fará com que o motor de sincronização para não conseguir ligar à base de dados SQL e não será possível iniciar o serviço.

## <a name="identity-data"></a>Dados de identidade
**P: o atributo UPN (userPrincipalName) no Azure AD não corresponde ao UPN no local - por que motivo?**  
Consulte estes artigos:

* [Os nomes de utilizador no Office 365, Azure ou Intune não correspondem a UPN no local ou o ID de início de sessão alternativo](https://support.microsoft.com/en-us/kb/2523192)
* [As alterações não são sincronizadas pela ferramenta de sincronização de diretórios do Azure Active Directory depois de alterar o UPN de uma conta de utilizador para utilizar um diferente domínio federado](https://support.microsoft.com/en-us/kb/2669550)

Também pode configurar o Azure AD para permitir que o motor de sincronização atualizar o userPrincipalName conforme descrito em [funcionalidades do serviço de sincronização do Azure AD Connect](active-directory-aadconnectsyncservice-features.md).

**P: é, suportado para correspondência de forma recuperável AD grupo/contacte objetos com objetos existentes do grupo/contacte do Azure AD no local?**  
Sim, esta correspondência de forma recuperável será baseada no /proxyaddress.  Correspondência de forma recuperável não é suportada para os grupos que não estão ativadas para correio.

**P: é, suportado para definir manualmente ImmutableId atributo em objetos existentes do Azure AD grupo/contacte rígido corresponder ao que no local objetos de AD grupo/contacto?**  
Não, manualmente a definição do atributo ImmutableId num objeto existente do Azure AD grupo/contacte para correspondência de disco rígida atualmente não é suportado.

## <a name="custom-configuration"></a>Configuração personalizada
**P: onde estão documentados os cmdlets do PowerShell do Azure AD Connect?**  
Com exceção dos cmdlets documentados neste site, os outros cmdlets do PowerShell encontrados no Azure AD Connect não são suportados para utilização do cliente.

**P: Posso utilizar "Servidor de exportação/importação do servidor" encontrada no *Synchronization Service Manager* para mover a configuração entre servidores?**  
Não. Esta opção não irá obter todas as definições de configuração e não deve ser utilizada. Em vez disso, deve utilizar o Assistente para criar a configuração base no segundo servidor e utilize o editor de regra de sincronização para gerar scripts do PowerShell para mover qualquer regra personalizada entre servidores. Consulte [Swing migração](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).

**P: as palavras-passe ser colocadas em cache para a página de início de sessão do Azure e pode isto impedida porque contém um elemento de entrada de palavra-passe com a conclusão automática = "false" atributo?**</br>
Atualmente, modificar os atributos HTML do campo entrada de palavra-passe, incluindo a tag de conclusão automática, não é suportada. Uma funcionalidade que vai permitir Javascript personalizado, permitindo-lhe adicionar qualquer atributo para o campo de palavra-passe, está atualmente a ser trabalhada.

**P: na página início de sessão do Azure, são apresentados os nomes de utilizador para os utilizadores que tenham anteriormente sessão iniciada com êxito.  Este comportamento pode ser ativado?**</br>
Atualmente, modificar os atributos HTML do campo entrada de palavra-passe, incluindo a tag de conclusão automática, não é suportada. Uma funcionalidade que vai permitir Javascript personalizado, permitindo-lhe adicionar qualquer atributo para o campo de palavra-passe, está atualmente a ser trabalhada.

**P: existe uma forma de impedir sessões simultâneas?**</br>
Não.

## <a name="auto-upgrade"></a>Atualização automática

**P: quais são as vantagens e consequências da utilização de auto atualização?**</br>
Todos os clientes que aconselhado para ativar a atualização automática para a instalação do Azure AD Connect. Os benefícios são que receberão sempre as correções mais recentes, incluindo atualizações de segurança de vulnerabilidades que foram encontradas no Azure AD Connect. O processo de atualização é sem esforços e acontecerá automaticamente assim que estiver disponível uma nova versão. Muitos milhares de clientes do Azure AD Connect utilizam a atualização automática com cada nova versão.

O processo de atualização automática irá estabelecer sempre primeiro se uma instalação é elegível para atualização automática (este processo inclui a procura personalizadas alterações às regras, fatores ambientais específicos etc.) e, se, por isso, a atualização é executada e testada. Se os testes mostram se uma atualização não foi bem sucedida, a versão anterior irá obter restaurada automaticamente.

Dependendo do tamanho do ambiente, o processo pode demorar algumas horas e, enquanto a atualização acontece, acontecerá sem sincronização entre o Windows Server AD e AD do Azure.

**P: Posso recebeu uma mensagem de e-mail a informar-me que já não funciona a minha atualização automática e preciso de instalar a nova versão. Por que motivo é necessário fazê-lo?**</br>
Último ano, uma versão do Azure AD Connect que, em determinadas circunstâncias, poderá ter desativada a funcionalidade de atualização automática no seu servidor, foi lançada. No Azure AD Connect versão 1.1.750.0 corrigir este problema. Os clientes que foram afetados por este problema tem de atualizar manualmente para a versão mais recente do Azure AD Connect para mitigar o problema. Para atualizar manualmente, tem de transferir e executar a versão mais recente do ficheiro AADConnect.msi.
 
-  Se a versão atual é anterior ao 1.1.750.0, tem de atualizar para a versão mais recente, [que pode ser transferida aqui](https://www.microsoft.com/en-us/download/details.aspx?id=47594).
- Se a sua versão do Azure AD Connect é 1.1.750.0 ou mais recente, não terá de efetuar qualquer ação para mitigar o problema de atualização automática, tal como já está na versão que tenha uma correção para este. 

**P: Posso recebeu uma mensagem de e-mail a informar-me para atualizar para a versão mais recente para reativar a atualização automática. Sou no 1.1.654.0, é necessário atualizar?** </br>    
Sim, terá de atualizar para 1.1.750 ou mais recente para reativar a atualização automática. Segue-se a ligação que explica como atualizar para uma versão mais recente

**P: Posso recebeu uma mensagem de e-mail a informar-me para atualizar para a versão mais recente para reativar a atualização automática. Tiver utilizado o PowerShell para ativar a atualização automática, é ainda necessário instalar a versão mais recente?**</br>    
Sim, é preciso atualizar para versão 1.1.750.0 ou mais recente. Ativar o serviço de atualização automática com o PowerShell não mitigar o problema de atualização automática encontrado em versões antes 1.1.750

**P: Posso pretende atualizar para uma versão mais recente, mas não estou a certeza de que instalar o Azure AD Connect e não temos o nome de utilizador e palavra-passe.  Precisamos disto?**</br>
Não precisa de saber o nome de utilizador e palavra-passe que foi inicialmente utilizada para atualizar o Azure AD Connect – qualquer conta do Azure AD que tenha a função de Administrador Global pode ser utilizado.

**P: como posso saber qual é a versão do Azure AD Connect sou no?**</br>   
Para verificar qual é a versão do Azure AD Connect está instalada no seu servidor, aceda ao painel de controlo e procurar a versão instalada do Microsoft Azure AD Connect em "Programas > programas e funcionalidades":

![versão](media/active-directory-aadconnect-faq/faq1.png)

**P: como posso efetuar uma atualização para a versão mais recente do AADConnect?**</br>    
Isto [artigo](active-directory-aadconnect-upgrade-previous-version.md) explica como atualizar para uma versão mais recente. 

**P: estamos já atualizado para a versão mais recente do AADConnect último ano, necessitamos atualizar novamente?**</br> As equipas do Azure AD Connect torna atualizações frequentes para o serviço e é importante que o servidor está atualizado com a versão mais recente beneficiar das correções e atualizações de segurança, bem como novas funcionalidades. Se ativar a atualização automática, que a versão de software será atualizada automaticamente. Para localizar o histórico de lançamento da versão do Azure AD Connect, siga este [ligação](active-directory-aadconnect-version-history.md).

**P: quanto tempo necessário para efetuar a atualização e o que é o impacto nos meus utilizadores?**</br>    
O tempo necessário para atualizar depende do tamanho do seu inquilino e para as organizações de maior poderá ser melhor efetuar este procedimento no evening ou fim de semana. Durante a atualização nenhuma atividade de sincronização ocorre.

**P: Posso considerar posso atualizado para AADConnect mas no portal do Office menciona ainda DirSync.  Por que motivo é isto?**</br>    
A equipa do Office está a trabalhar para obter atualizações ao portal do Office para refletir o nome de produto atual – não refletir que estiver a utilizar a ferramenta de sincronização.

**P: Posso marcada meu estado de atualização automática e diz "suspenso". Por que motivo está-suspenso? Deve posso ativá-la?**</br>     
Foi introduzido um erro numa versão anterior que, em determinadas circunstâncias, impedir que o estado de atualização automática definido para "suspenso". Ativar manualmente é tecnicamente possível mas seria necessários vários passos complexos, pelo que a melhor coisa que pode fazer é instalar a versão mais recente do Azure AD Connect

**P: minha empresa tem requisitos de gestão de alteração strict e quiser controlar quando este é enviado. Posso controlar quando é iniciada a atualização automática?**</br> Não, não há nenhum essa funcionalidade hoje em dia, esta funcionalidade é algo que está a ser avaliação para uma versão futura.

**P: Posso obterá uma mensagem de e-mail caso de falha da atualização automática? Como posso saber que foi efetuada com êxito?**</br>     
Não será notificado quanto o resultado da atualização, esta funcionalidade é algo que está a ser avaliação para uma versão futura.

**Q:do que publicar uma linha cronológica relativamente ao planear push enviados automaticamente atualizações?**</br>    
Atualização automática é o primeiro passo no processo de lançamento de uma versão mais recente, por isso, sempre que exista uma nova versão, atualizações automática é feito o Push. As versões mais recentes do Azure AD Connect são previamente anunciadas no [do Azure AD plano](../../active-directory/whats-new.md).

**P: atualização automática atualização AAD Connect Health?**</br>   Sim, a atualização automática também atualiza AAD Connect Health

**P: são também servidores de atualização automática AAD Connect no modo de teste?**</br>   
Não, a não atualização automática um servidor do Azure AD Connect que está no modo de teste.

**P: se Auto-atualização falhará e o meu servidor AAD Connect não iniciar, o que devo fazer?**</br>   
Em casos raros, o serviço do Azure AD Connect não é iniciado depois de efetuar a atualização. Nestes casos, reinicie o servidor, o que normalmente corrige o problema. Se o serviço do Azure AD Connect ainda não iniciar, abra um pedido de suporte. Eis um [ligação](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/) que explica como fazê-lo. 

**P: não sou se quais são os riscos quando atualizar para uma versão mais recente do Azure AD Connect. Pode telefonar-me para o ajudar-me com a atualização?**</br>
Se precisar de ajuda a atualização para uma versão mais recente do Azure AD Connect, abra um pedido de suporte, eis um [ligação](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/) que mostra como fazê-lo.

## <a name="troubleshooting"></a>Resolução de problemas
**P: como posso obter ajuda com o Azure AD Connect?**

[Procurar na Base de dados de Conhecimento Microsoft (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* Procure a Base de dados de conhecimento (BDC da Microsoft) para técnicas soluções para problemas de break-fix comuns sobre o suporte do Azure AD Connect.

[Fóruns do Microsoft Azure Active Directory](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* Pode pesquisar e navegue para a technical questões e respostas da Comunidade ou peça ao seu próprio pergunta clicando [aqui](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Como obter suporte para o Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto)



