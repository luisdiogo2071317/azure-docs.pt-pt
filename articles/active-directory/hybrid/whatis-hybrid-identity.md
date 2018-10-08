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
ms.date: 09/18/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 40ac3ca92c65607df056b883608dde60d816143e
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47181787"
---
# <a name="hybrid-identity-and-microsofts-identity-solutions"></a>Identidade híbrida e soluções de identidade da Microsoft
Hoje em dia, as empresas e corporações são cada vez mais uma combinação de aplicações locais e na cloud.  Ter aplicações e utilizadores que requerem acesso a essas aplicações, no local e na cloud, tornou-se um cenário desafiante.

As soluções de identidade da Microsoft abrangem capacidades baseadas na cloud e no local, ao criar uma única identidade de utilizador para autenticação e autorização a todos os recursos, independentemente da localização. Chamamos-lhe identidade híbrida.

## <a name="what-is-azure-ad-connect"></a>O que é o Azure AD Connect?

O Azure AD Connect é a ferramenta da Microsoft concebida para satisfazer e atingir os seus objetivos de identidade híbrida.  Isto permite-lhe fornecer uma identidade comum para o utilizadores das aplicações do Office 365, do Azure e do SaaS integradas com o Azure AD.  Proporciona as seguintes funcionalidades:
    
- [Sincronização](how-to-connect-sync-whatis.md) - este componente é responsável pela criação de utilizadores, grupos e outros objetos. Também é responsável por verificar se as informações de identidade dos seus utilizadores e grupos no local têm correspondência na nuvem.  É responsável por sincronizar os hashes de palavra-passe com o Azure AD.
-   [AD FS e integração de federação](how-to-connect-fed-whatis.md) - a federação é uma parte opcional do Azure AD Connect e pode ser utilizada para configurar um ambiente híbrido utilizando uma infraestrutura do AD FS no local. Fornece também funções de gestão do AD FS, como a renovação de certificados e implementações adicionais de servidor AD FS.
-   [Autenticação pass-through](how-to-connect-pta.md) - outro componente opcional que permite aos utilizadores utilizar a mesma palavra-passe no local e na cloud, mas que não requer a infraestrutura adicional de um ambiente federado
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

## <a name="next-steps"></a>Passos Seguintes


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









