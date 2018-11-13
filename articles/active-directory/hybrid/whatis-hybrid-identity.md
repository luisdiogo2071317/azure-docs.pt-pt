---
title: Ligar o Active Directory ao Azure Active Directory. | Microsoft Docs
description: O Azure AD Connect irá integrar os diretórios no local ao Azure Active Directory. Isto permite-lhe fornecer uma identidade comum para as aplicações do Office 365, do Azure e do SaaS integradas com o Azure AD.
keywords: introdução ao Azure AD Connect, descrição geral do Azure AD Connect, o que é o Azure AD Connect, instalar o Active Directory
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 59bd209e-30d7-4a89-ae7a-e415969825ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/02/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 2aca42c23cc213d5d7e451105052d5d5d697b77d
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/03/2018
ms.locfileid: "50979476"
---
# <a name="hybrid-identity-and-microsoft-identity-solutions"></a>Soluções de identidade híbrida e de identidade da Microsoft
As soluções de identidade híbrida do [Microsoft Azure Active Directory (Azure AD)](../../active-directory/fundamentals/active-directory-whatis.md) permitem-lhe sincronizar objetos de diretórios no local com o Azure AD e continuar a gerir os seus utilizadores no local. A primeira decisão a tomar no planeamento da sincronização do Active Directory do Windows Server no local com o Azure AD é determinar se vai utilizar as identidades geridas ou a identidade federada. 

- **Identidades geridas** - Contas de utilizador e grupos que são sincronizados a partir de um Active Directory no local e a autenticação de utilizador é gerida pelo Azure.   
- As **identidades federadas** proporcionam mais controlo sobre os utilizadores ao separar a autenticação do utilizador do Azure e ao delegar a autenticação a um fornecedor de identidades no local e fidedigno. 

Há várias opções disponíveis para configurar a identidade híbrida. Enquanto analisa o modelo de identidade que melhor se adequa às necessidades da sua organização, também tem de pensar no tempo, na infraestrutura existente, na complexidade e nos custos. Estes fatores são diferentes para cada organização e podem sofrer mudanças ao longo do tempo. Contudo, caso os seus requisitos se venham mesmo a alterar, também tem a flexibilidade para mudar para outro modelo de identidade.

## <a name="managed-identity"></a>Identidade gerida 

A identidade gerida é a forma mais simples de sincronizar objetos de diretórios no local (utilizadores e grupos) com o Azure AD. 

![Identidade híbrida sincronizada](./media/whatis-hybrid-identity/managed.png)

Embora a identidade gerida seja o método mais fácil e rápido, os utilizadores ainda precisam de manter uma palavra-passe separada para os recursos baseados na cloud. Para o evitar, também pode (opcionalmente) [sincronizar um hash de palavras-passe de utilizadores](how-to-connect-password-hash-synchronization.md) com o seu diretório do Azure AD. A sincronização dos hashes de palavras-passe permite que os utilizadores iniciem sessão nos recursos baseados na cloud da organização com o mesmo nome de utilizador e a mesma palavra-passe que utilizam no local. Periodicamente, o Azure AD Connect verifica se há alterações no diretório no local e mantém o diretório do Azure AD sincronizado. Quando um atributo ou uma palavra-passe de um utilizador é alterado no Active Directory no local, essa alteração é atualizada automaticamente no Azure AD. 

À maioria das organizações que só precisam que os utilizadores iniciem sessão no Office 365, em aplicações SaaS e noutros recursos baseados no Azure AD, recomenda-se a opção de sincronização de hash de palavras-passe predefinida. Se essa opção não funcionar para si, terá de decidir entre a autenticação pass-through e o AD FS.

> [!TIP]
> As palavras-passe dos utilizadores são armazenadas no Active Directory do Windows Server no local na forma de valores de hash que representam as palavras-passe reais deles. Os valores de hash são o resultado de uma função matemática unidirecional (o algoritmo hash). Não existe nenhum método para reverter o resultado de uma função unidirecional para a versão de texto simples de uma palavra-passe. Não pode utilizar um hash de palavra-passe para iniciar sessão na sua rede no local. Se optar por sincronizar as palavras-passe, o Azure AD Connect extrai os hashes das palavras-passe do Active Directory no local e aplica um processamento de segurança extra aos hashes antes de serem sincronizados com o Azure AD. Também pode utilizar a sincronização de hash de palavras-passe juntamente com a repetição de escrita de palavras-passe para ativar a reposição personalizada de palavras-passe no Azure AD. Além disso, pode ativar o início de sessão único (SSO) para os utilizadores em computadores associados a um domínio que estejam ligados à rede empresarial. Com o início de sessão único, os utilizadores ativados só precisam de introduzir um nome de utilizador para acederem aos recursos na cloud em segurança. 
>

## <a name="pass-through-authentication"></a>Autenticação pass-through

A [Autenticação pass-through do Azure AD](how-to-connect-pta.md) proporciona uma solução simples de validação de palavras-passe para os serviços baseados no Azure AD mediante a utilização do Active Directory no local. Se as políticas de segurança e conformidade da sua organização não permitirem o envio das palavras-passe dos utilizadores, mesmo em forma de hahs, e só precisar de suportar o SSO de ambiente de trabalho para dispositivos associados a um domínio, recomenda-se que considere a autenticação pass-through. A autenticação pass-through não requer implementação no DMZ, o que simplifica a infraestrutura de implementação em comparação com o AD FS. Quando os utilizadores iniciam sessão com o Azure AD, este método de autenticação valida as paalvras-passe deles diretamente no seu Active Directory no local.

![Autenticação pass-through](./media/whatis-hybrid-identity/pass-through-authentication.png)

Com a autenticação pass-through, não é precisa uma infraestrutura de rede complexa nem é necessário armazenar as palavras-passe no local na cloud. Em conjunto com o início de sessão único, a autenticação pass-through proporciona uma experiência verdadeiramente integrada para iniciar sessão no Azure AD ou noutros serviços cloud.

A autenticação pass-through é configurada com o Azure AD Connect, que utiliza um agente no local simples que escuta os pedidos de validação de palavras-passe. O agente pode ser implementado facilmente em vários computadores, de modo a fornecer elevada disponibilidade e balanceamento de carga. Uma vez que todas as comunicações são só de saída, o conector não tem de ser instalado num DMZ. Os requisitos do computador de servidor do conector são os seguintes:

- Windows Server 2012 R2 ou posterior
- Associado a um domínio na floresta através da qual os utilizadores são validados

## <a name="federated-identity-ad-fs"></a>Identidade federada (AD FS)

Para ter mais controlo sobre a forma como os utilizadores acedem ao Office 365 e a outros serviços cloud, pode utilizar o [Active Directory Federation Services (AD FS)](how-to-connect-fed-whatis.md) e configurar a sincronização de diretórios com o início de sessão único (SSO). Federar os inícios de sessão dos utilizadores com o AD FS delega a autenticação num servidor no local que valida as credenciais daqueles. Neste modelo, as credenciais do Active Directory no local nunca são transmitidas ao Azure AD.

![Identidade federada](./media/whatis-hybrid-identity/federated-identity.png)

Também denominado “federação de identidade”, este método de início de sessão garante que todas as autenticações dos utilizadores são controladas no local e permite que os administradores implementem níveis de controlo de acesso mais rigorosos. A federação de identidade com o AD FS é a opção mais complicada e exige a implementação de servidores adicionais no ambiente no local. Também exige que se comprometa a disponibilizar suporte permanente para a infraestrutura do Active Directory e do AD FS. Este alto nível de suporte é necessário porque, em caso de indisponibilidade do acesso à Internet no local, do controlador de domínio ou dos servidores do AD FS, os utilizadores não conseguem iniciar sessão nos serviços cloud.

> [!TIP]
> Se optar por utilizar a Federação com o Azure Active Directory Federation Services (AD FS), pode, facultativamente, configurar a sincronização de hash de palavras-passe como método alternativo, para o caso de a infraestrutura do AD FS falhar.
>

## <a name="common-scenarios-and-recommendations"></a>Cenários comuns e recomendações

Seguem-se alguns cenários comuns de identidades híbridas e gestão de acessos com recomendações relativamente à opção (ou opções) de identidade híbrida que pode ser mais adequada a cada cenário.

|Preciso de:|PHS e SSO<sup>1</sup>| PTA e SSO<sup>2</sup> | AD FS<sup>3</sup>|
|-----|-----|-----|-----|
|Sincronizar contas de utilizador, de contactos e de grupos novas criadas no Active Directory no local com a cloud automaticamente.|![Recomendado](./media/whatis-hybrid-identity/ic195031.png)| ![Recomendado](./media/whatis-hybrid-identity/ic195031.png) |![Recomendado](./media/whatis-hybrid-identity/ic195031.png)|
|Configurar o meu inquilino para cenários híbridos do Office 365|![Recomendado](./media/whatis-hybrid-identity/ic195031.png)| ![Recomendado](./media/whatis-hybrid-identity/ic195031.png) |![Recomendado](./media/whatis-hybrid-identity/ic195031.png)|
|Permitir que os meus utilizadores iniciem sessão e acedam aos serviços cloud com as palavras-passe no local|![Recomendado](./media/whatis-hybrid-identity/ic195031.png)| ![Recomendado](./media/whatis-hybrid-identity/ic195031.png) |![Recomendado](./media/whatis-hybrid-identity/ic195031.png)|
|Implementar o início de sessão único com as credenciais da empresa|![Recomendado](./media/whatis-hybrid-identity/ic195031.png)| ![Recomendado](./media/whatis-hybrid-identity/ic195031.png) |![Recomendado](./media/whatis-hybrid-identity/ic195031.png)|
|Ter a certeza de que não há hashes de palavras-passe armazenados na cloud| |![Recomendado](./media/whatis-hybrid-identity/ic195031.png)|![Recomendado](./media/whatis-hybrid-identity/ic195031.png)|
|Ativar soluções de autenticação multifator na cloud| |![Recomendado](./media/whatis-hybrid-identity/ic195031.png)|![Recomendado](./media/whatis-hybrid-identity/ic195031.png)|
|Ativar soluções de autenticação multifator no local| | |![Recomendado](./media/whatis-hybrid-identity/ic195031.png)|
|Suportar a autenticação de smartcards para os meus utilizadores<sup>4</sup>| | |![Recomendado](./media/whatis-hybrid-identity/ic195031.png)|
|Apresentar notificações de expiração de palavras-passe no Portal do Office e no ambiente de trabalho do Windows 10| | |![Recomendado](./media/whatis-hybrid-identity/ic195031.png)|

> <sup>1</sup> Sincronização de hash de palavras-passe com início de sessão único.
>
> <sup>2</sup> Autenticação pass-through e início de sessão único. 
>
> <sup>3</sup> Início de sessão único federado com o AD FS.
>
> <sup>4</sup> O AD FS pode ser integrado no PKI da sua empresa, para permitir o início de sessão com certificados. Esses certificados podem ser certificados de software implementados através de canais de aprovisionamento fidedignos, como o MDM ou o GPO, ou certificados de smartcard (incluindo cartões PIV/CAC) ou Hello for Business (cert-trust). Para obter mais informações sobre o suporte da autenticação de smartcards, veja [este blogue](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/).
>

## <a name="what-is-azure-ad-connect"></a>O que é o Azure AD Connect?

O Azure AD Connect é a ferramenta da Microsoft concebida para satisfazer e atingir os seus objetivos de identidade híbrida.  Isto permite-lhe fornecer uma identidade comum para o utilizadores das aplicações do Office 365, do Azure e do SaaS integradas com o Azure AD.  Proporciona as seguintes funcionalidades:
    
- [Sincronização](how-to-connect-sync-whatis.md) - este componente é responsável pela criação de utilizadores, grupos e outros objetos. Também é responsável por verificar se as informações de identidade dos seus utilizadores e grupos no local têm correspondência na nuvem.  É responsável por sincronizar os hashes de palavra-passe com o Azure AD.
- [Sincronização de hashes de palavras-passe](how-to-connect-password-hash-synchronization.md) - um componente opcional que permite aos utilizadores utilizarem a mesma palavra-passe no local e na cloud, mediante a sincronização de um hash das palavras-passe deles com o Azure AD.
-   [AD FS e integração de federação](how-to-connect-fed-whatis.md) - a federação é uma parte opcional do Azure AD Connect e pode ser utilizada para configurar um ambiente híbrido utilizando uma infraestrutura do AD FS no local. Fornece também funções de gestão do AD FS, como a renovação de certificados e implementações adicionais de servidor AD FS.
-   [Autenticação pass-through](how-to-connect-pta.md) - outro componente opcional que permite aos utilizadores utilizar a mesma palavra-passe no local e na cloud, mas que não requer a infraestrutura adicional de um ambiente federado
-   [PingFederate e integração de federação](how-to-connect-install-custom.md#configuring-federation-with-pingfederate) - outra opção de federação que lhe permite utilizar PingFederate como o fornecedor de identidade.
-   [Monitorização do Estado de Funcionamento ](whatis-hybrid-identity-health.md) - o Azure AD Connect Health pode proporcionar uma monitorização robusta e uma localização central no portal do Azure para visualizar esta atividade. 


![O que é o Azure AD Connect](./media/whatis-hybrid-identity/arch.png)



## <a name="what-is-azure-ad-connect-health"></a>O que é o Azure AD Connect Health?

O Azure Active Directory (Azure AD) Connect Health ajuda a monitorizar e a obter informações sobre a sua infraestrutura de identidade no local, bem como sobre os serviços de sincronização. Permite-lhe manter uma ligação fiável ao Office 365 e aos Microsoft Online Services ao fornecer capacidades de monitorização para os componentes de identidade chave, como servidores de Serviços de Federação do Active Directory (AD FS), servidores do Azure AD Connect (também conhecido como Motor de Sincronização), controladores de domínio do Active Directory, etc. Torna também facilmente acessíveis os pontos de dados chave sobre estes componentes, para que possa obter informações sobre a utilização e outras informações importantes para tomar decisões informadas.

As informações são apresentadas no [portal do Azure AD Connect Health](https://aka.ms/aadconnecthealth). No portal do Azure AD Connect Health, pode visualizar alertas, a monitorização do desempenho, a análise de utilização e outras informações. O Azure AD Connect Health ativa a lente única do estado de funcionamento dos componentes de identidade chave num único local.

![O que é o Azure AD Connect Health](./media/whatis-hybrid-identity-health/aadconnecthealth2.png)


À medida que as funcionalidades do Azure AD Connect Health aumentam, o portal fornece um dashboard único através a lente de identidade. Obtém um ambiente ainda mais robusto, em bom estado de funcionamento e integrado de que os seus utilizadores podem tirar partido para aumentar a sua capacidade para concluir tarefas.


## <a name="why-use-azure-ad-connect"></a>Porquê utilizar o Azure AD Connect?
A integração dos diretórios no local com o Azure AD torna os utilizadores mais produtivos, ao fornecer-lhes uma identidade comum para acederem a recursos na nuvem e no local. Os utilizadores e a organizações podem tirar partido do seguinte:

* Os utilizadores podem utilizar uma identidade única para acederem às aplicações no local e aos serviços na nuvem, como o Office 365.
* Uma ferramenta única para proporcionar uma experiência fácil de implementação para sincronização e início de sessão.
* Fornece as capacidades mais recentes para os seus cenários. O Azure AD Connect substitui as versões anteriores das ferramentas de integração de identidade, como o DirSync e o Azure AD Sync. Para obter mais informações, consulte [Comparação das ferramentas de integração de diretórios de identidade híbrida](plan-hybrid-identity-design-considerations-tools-comparison.md).

## <a name="why-use-azure-ad-connect-health"></a>Porquê utilizar o Azure AD Connect Health?
A integração dos diretórios no local com o Azure AD torna os utilizadores mais produtivos, porque existe uma identidade comum para acederem a recursos na cloud e no local. No entanto, esta integração acarreta o desafio de garantir que este ambiente está em bom estado de funcionamento, de modo a que os utilizadores possam aceder com fiabilidade aos recursos no local e na cloud a partir de qualquer dispositivo. O Azure AD Connect Health ajuda a monitorizar e a obter informações acerca da sua infraestrutura de identidade no local que é utilizada para aceder ao Office 365 ou a outras aplicações do Azure AD. É tão simples como instalar um agente em cada um dos servidores de identidade no local.

### <a name="azure-ad-connect-health-for-ad-fshow-to-connect-health-adfsmd"></a>[Azure AD Connect Health para AD FS](how-to-connect-health-adfs.md)
O Azure AD Connect Health para AD FS suporta o AD FS 2.0 no Windows Server 2008 R2, no Windows Server 2012, no Windows Server 2012 R2 e no Windows Server 2016. Suporta também a monitorização de servidores proxy do AD FS ou de aplicações Web que fornecem suporte de autenticação para acesso à extranet. Com uma instalação fácil e rápida do Agente de Estado de Funcionamento, o Azure AD Connect Health para AD FS fornece um conjunto de capacidades principais.

#### <a name="key-benefits-and-best-practices"></a>Principais benefícios e melhores práticas

- *Segurança melhorada*
  - [Tendências de bloqueio de extranet](how-to-connect-health-adfs.md#usage-analytics-for-ad-fs)
  - [Relatório de inícios de sessão falhados](how-to-connect-health-adfs.md#risky-ip-report-public-preview) 
  - [Conformidade com a privacidade](reference-connect-health-user-privacy.md)    
- *Receber alertas de todos os [problemas críticos do sistema ADFS](how-to-connect-health-alert-catalog.md#alerts-for-active-directory-federation-services)*
    - Configuração e disponibilidade do servidor 
    - [Desempenho e conectividade](how-to-connect-health-adfs.md#performance-monitoring-for-ad-fs) 
  - Manutenção regular    
- *Fácil de implementar e gerir*
  - Rápida [instalação do agente](how-to-connect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs) 
  - Atualização automática do agente para a versão mais recente 
  - Dados disponíveis no portal em alguns minutos    
- *Métricas de utilização [avançadas](how-to-connect-health-adfs.md#usage-analytics-for-ad-fs)* 
  - Utilização principal das aplicações
  - Localizações de rede e ligação TCP
  - Pedidos de token por servidor    
- *Experiência de utilizador excecional* 
  - Modo de dashboard do portal do Azure
  - [Alertas por e-mail](how-to-connect-health-adfs.md#alerts-for-ad-fs)    

#### <a name="feature-highlight"></a>Realce de funcionalidade

*   Monitorização com alertas para saber quando os servidores AD FS e proxy do AD FS não estão em bom estado
*   Notificações por e-mail para alertas críticos
*   Tendências em dados de desempenho, que são úteis para o planeamento da capacidade do AD FS
*   Análise de utilização do inícios de sessão no AD FS com pivôs (aplicações, utilizadores, localização de rede, etc.)
*   Relatórios do AD FS, tais como os 50 utilizadores com mais tentativas de nome de utilizador/palavra-passe incorretas e o último endereço IP dos mesmos
*   Relatório de IP de risco para inícios de sessão do AD FS com falhas

Saiba mais aqui sobre [Utilizar o Azure AD Connect Health com o AD FS](how-to-connect-health-adfs.md)

### <a name="azure-ad-connect-health-for-synchow-to-connect-health-syncmd"></a>[Azure AD Connect Health para sincronização](how-to-connect-health-sync.md)
O Azure AD Connect Health para sincronização monitoriza e fornece informações sobre as sincronizações que ocorrem entre o Active Directory no local e o Azure AD. O Azure AD Connect Health para sincronização fornece o seguinte conjunto de capacidades principais:

* Monitorização com alertas para saber quando um servidor do Azure AD Connect, também conhecido como Motor de Sincronização, não está em bom estado de funcionamento
* Notificações por e-mail para alertas críticos
* Sincronizar informações operacionais, que incluem gráficos de latência para operações de sincronização e as tendências em operações diferentes, tal como adições, atualizações e eliminações
* Informações de leitura rápida acerca das propriedades de sincronização e da última exportação com êxito para o Azure AD
* Relatórios sobre erros de sincronização ao nível do objeto \(não requerem o Azure AD Premium\)

Saiba mais aqui sobre [Utilizar o Azure AD Connect Health para sincronização](how-to-connect-health-sync.md)

### <a name="azure-ad-connect-health-for-ad-dshow-to-connect-health-addsmd"></a>[Azure AD Connect Health para AD DS](how-to-connect-health-adds.md)
O Azure AD Connect Health para Active Directory Domain Services (AD DS) fornece monitorização para controladores de domínio instalados no Windows Server 2008 R2, no Windows Server 2012, no Windows Server 2012 R2 e no Windows Server 2016. A instalação do Agente de Estado de Funcionamento permite-lhe monitorizar o ambiente do AD DS no local a partir da cloud. O Azure AD Connect Health para AD DS fornece o seguinte conjunto de capacidades principais:

* Alertas de monitorização para detetar quando os controladores de domínio estão em mau estado de funcionamento e notificações por e-mail para alertas críticos
* O dashboard de Controladores de Domínio, que fornece uma vista rápida do estado de funcionamento e operacional dos controladores de domínio
* O dashboard de Estado de Replicação que inclui as informações de replicação mais recentes e hiperligações para guias de resolução de problemas quando forem detetados erros
* Acesso rápido em qualquer local a gráficos de dados de desempenho de contadores de desempenho populares, que são necessários para efeitos de resolução de problemas e de monitorização

Saiba mais aqui sobre [Utilizar o Azure AD Connect Health com o AD DS](how-to-connect-health-adds.md)

## <a name="next-steps"></a>Passos seguintes


- [Hardware e pré-requisitos](how-to-connect-install-prerequisites.md) 
- [Definições rápidas](how-to-connect-install-express.md)
- [Definições personalizadas](how-to-connect-install-custom.md)
- [Sincronização de hash de palavra-passe](how-to-connect-password-hash-synchronization.md)|
- [Autenticação pass-through](how-to-connect-pta.md)
- [Azure AD Connect e a federação](how-to-connect-fed-whatis.md)
- [Instalar os agentes do Azure AD Connect Health](how-to-connect-health-agent-install.md) 
- [Sincronização do Azure AD Connect](how-to-connect-sync-whatis.md)
- [Histórico de versões](reference-connect-version-history.md)
- [Comparação das ferramentas de integração de diretórios](plan-hybrid-identity-design-considerations-tools-comparison.md)
- [FAQ do Azure AD Connect](reference-connect-faq.md)









