---
title: Requisitos por tipo de listagem | Azure
description: Este artigo descreve os critérios de elegibilidade e publicação parceiros de requisitos a tentar compreender como publicar aplicações no Azure Marketplace.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: jm-aditi-ms
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 06/05/2018
ms.author: ellacroi
ms.openlocfilehash: dbb14854f00fb133c3604a1dd529d42120371fd2
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34825255"
---
# <a name="requirements-by-listing-type"></a>Requisitos por tipo de listagem  
Os requisitos técnicos e marketing conteúdos variam consoante o storefront, o tipo de oferta e o tipo de listagem. Reveja as seguintes especificações para verificar a sua conformidade.  
1. Storefront requisitos:  
    *   [AppSource](#storefront-requirements:-appSource)  
    *   [Azure Marketplace](#storefront-requirements:-azure-marketplace)  
2. Tipo de listagem e requisitos do tipo de oferta:  
    *   Para obter mais informações sobre tipos de listagem e tipos de oferta, visite a determinar o tipo de lista para a página da sua solução localizado em [docs.microsoft.com/azure/marketplace/determine-your-listing-type](./determine-your-listing-type.md).  

## <a name="storefront-requirements-appsource"></a>Requisitos de storefront: AppSource  
A tabela seguinte descreve os requisitos de pré-requisitos para a publicação no AppSource.  
| Requisito | Detalhes | Obrigatório ou recomendado |  
|:--- |:--- |:--- |  
| ***Azure Active Directory (Azure AD)*** | A aplicação tem de permitir Azure Active Directory federado-início de sessão único (SSO federado o Azure AD) com consentimento ativado.<ul> <li>Para obter mais informações sobre como ativar o Azure AD federada SSO, visite a configurar-início de sessão único para aplicações que não estão no Azure Active Directory página de Galeria de aplicações, localizada em [docs.microsoft.com/azure/active-directory/ Active-directory--personalizada-aplicações saas](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps).</li> </ul> | Necessário |   
| ***Integração com os serviços de nuvem da Microsoft*** | A aplicação deve integrar com outros serviços do Microsoft Cloud, como os serviços do Microsoft Power BI, o Cortana Intelligence ou o Microsoft Azure.<ul> <li>Um exemplo de um serviço Microsoft Cloud é Internet das coisas.</li> </ul> | Recomendado |  
| ***público-alvo*** | A aplicação tem de ser para utilizadores de linha de negócios e os proprietários de empresas. | Necessário | 
| ***Software como uma aplicação de serviço (SaaS) para empresas*** | A aplicação tem de cumprir os seguintes requisitos.<ul> <li>Uma aplicação SaaS de linha de negócio</li> <li>Concentra-se de processo empresarial</li> <li>Direcionadas para os clientes de negócio</li> <li>Permitir que os utilizadores utilizar as respetivas credenciais de trabalho para iniciar sessão como o nome de utilizador e palavra-passe</li> </ul> | Necessário |  
| ***Período de avaliação gratuita e experiência de avaliação gratuita*** | A aplicação têm de incluir um as seguintes opções para que um cliente utilizar a aplicação gratuitamente durante um período de tempo limitado.<ul> <li>Forneça um `try` método, pelo que os clientes podem iniciar uma avaliação da sua aplicação no AppSource</li> <li>Forneça um `request trial` opção na AppSource, pelo que os clientes podem pedir uma versão de avaliação da sua aplicação</li> </ul>A versão de avaliação gratuita que fornecer tem oferecem cliente um predefinidos período de tempo para experimentar a aplicação sem custos adicionais. | Necessário |  
| ***Solução facilmente configurável e pronto a utilizar*** | A aplicação tem de ser mais fácil e rápida configurar e configurar com nenhuma personalização necessária. | Necessário |  
| ***Levar gestão*** | Tem de ativar o CRM aceitar dados de oportunidades potenciais antes de poder receber leva do storefront.<ul> <li>Exemplos de CRMs são Marketo, Microsoft Dynamics ou Salesforce</li> </ul> | Necessário |  
| ***Política de privacidade e termos de utilização*** | A aplicação tem de fornecer uma ligação à sua página de política de privacidade utilizando um URL público. Os seus termos de utilização tem de ser fornecidos durante a publicação como texto. | Necessário |  
| ***Suporte*** | A aplicação tem de fornecer uma ligação à sua página de suporte ao cliente utilizar um URL público. Se a aplicação for uma versão de avaliação, em seguida, tem de suportar sem custos adicionais durante o período de avaliação. | Necessário |  

## <a name="storefront-requirements-azure-marketplace"></a>Requisitos storefront: Azure Marketplace  
Seguem-se os requisitos de pré-requisitos para a lista de tipos no Azure Marketplace.  
| Requisito | Detalhes | Tipo de listagem |  
|:--- |:--- |:--- |  
| ***Políticas de participação*** | A aplicação tem de seguir as políticas de participação do Azure Marketplace.<ul> <li>Para obter mais informações sobre as políticas de participação, visite as políticas de participação do Azure Marketplace página localizada em [azure.microsoft.com/support/legal/marketplace/participation-policies](https://azure.microsoft.com/support/legal/marketplace/participation-policies).</li></ul> | lista<br />Transact<br />Versão de avaliação |  
| ***Integração com a Microsoft*** | A oferta deve utilizar ou expandir os tipos de serviço do Microsoft Azure, tais como a computação, o funcionamento em rede ou o armazenamento. A oferta deve são alinhados com uma categoria existente do Azure Marketplace como bases de dados, de segurança ou de rede.<ul> <li>Para mais informações sobre as ofertas de Marketplace, visite as aplicações de Marketplace página localizada em [azuremarketplace.microsoft.com/marketplace/apps](https://azuremarketplace.microsoft.com/marketplace/apps).</li> </ul> | lista<br />Transact<br />Versão de avaliação |  
| ***público-alvo*** | A oferta é necessário para profissionais IT, os programadores de nuvem ou outras funções do cliente técnica. | lista<br />Transact<br />Versão de avaliação |  
| ***Levar gestão*** | Tem de ativar o CRM (Marketo, Microsoft Dynamics ou Salesforce) aceitar dados de oportunidades potenciais antes de poder receber leva do storefront. | lista<br />Transact<br />Versão de avaliação |  
| ***Política de privacidade e termos de utilização*** | A aplicação tem de fornecer uma ligação à sua página de política de privacidade utilizando um URL público. Os seus termos de utilização tem de ser fornecidos durante a publicação como texto. | lista<br />Transact<br />Versão de avaliação |  
| ***Suporte*** | A oferta tem de fornecer uma ligação à sua página de suporte ao cliente utilizar um URL público. Se a oferta é uma versão de avaliação, em seguida, tem de suportar sem custos adicionais durante o período de avaliação. | Transact<br />Versão de avaliação |    

## <a name="non-transact-listings"></a>Não transact listagens  
Esta secção descreve todos os tipos de oferta não utilizem Transact listagem tipo. 

### <a name="list"></a>Lista  
A lista de tipo de lista inclui os seguintes tipos de oferta nos storefronts no marketplace.  

| Tipo de oferta | Loja | Detalhes |  
|:---        |:---        |:---     |  
| Serviços de consultoria | AppSource | [Requisitos: AppSource: lista: consultar os serviços](#requirements:-appsource:-list:-consulting-services) |  
| Serviços de consultoria | Azure Marketplace | [Requisitos: Azure Marketplace: lista: consultar os serviços](#requirements:-azure-marketplace:-list:-consulting-services) |  
| Contactar-me | AppSource | [](#) |  
| Contactar-me | Azure Marketplace | [Requisitos: AppSource: lista: contactar-Me](#requirements:-azure-marketplace:-list:-contact-me) |  

#### <a name="requirements-appsource-list-consulting-service"></a>Requisitos: AppSource: lista: consultadoria de serviço  

| Requisitos | Detalhes |  
|:--- |:--- |  
| Características de oferta de serviço | O serviço de consultoria têm de cumprir os seguintes critérios.<ul> <li>Entrega o engagement um âmbito fixo, duração fixo, preço fixo (ou livre).</li> <li>Incluída principalmente para pré-vendas.</li> <li>O limite para um único cliente.</li> <li>Realize no site.</li> </ul> |  
| Requisitos de parceiro para serviços de consultoria | Cumprem os critérios na área de relevante para o seu serviço.<table><tr><th>Área de solução</th><th>Critérios</th></tr><tr><td>Dynamics 365 para o Engagement de cliente</td><td>Têm competência Silver ou Gold gestão de relação de cliente de nuvem.</td></tr><tr><td>Dynamics 365 para financeiros e de operações, Enterprise edition</td><td>Ter competência Silver ou Gold planeamento de recursos de empresa e as receitas das suas operações de nuvem de 12 meses à direita de $25,000 ou mais.</td></tr><tr><td>Dynamics 365 para financeiros e de operações, a edição de negócio</td><td>Servir como fornecedor de serviços em nuvem (CSP) ou Digital parceiro de registo (DPOR) para um ou mais clientes.</td></tr><tr><td>Power BI</td><td>Cumpre os critérios de parceiros de solução.</td></tr><tr><td>PowerApps</td><td>Ter uma solução de demonstração de parceiro.</td></tr></table><ul> <li>Para obter mais informações sobre a gestão de relação de cliente, visite a gestão de relação de cliente de nuvem página localizada em [partner.microsoft.com/membership/cloud-customer-relationship-management-competency](https://partner.microsoft.com/membership/cloud-customer-relationship-management-competency).</li> <li>Para obter mais informações sobre o planeamento de recursos, visite o planeamento de recursos de empresa página localizada em [partner.microsoft.com/membership/enterprise-resource-planning-competency](https://partner.microsoft.com/membership/enterprise-resource-planning-competency).</li> <li>Para mais informações sobre o CSP, visite o fornecedor de serviços em nuvem página localizada em [partner.microsoft.com/cloud-solution-provider](https://partner.microsoft.com/cloud-solution-provider).</li> <li>Para obter mais informações sobre DPOR, visite o parceiro de registo Digital e página de associação de parceiro, localizado em [partner.microsoft.com/membership/digital-partner-of-record](https://partner.microsoft.com/membership/digital-partner-of-record).</li> <li>Para obter mais informações sobre critérios de parceiros de solução, visite a descrição geral de parceiros de solução e documento Incentives localizado em [www.microsoftpartnerserverandcloud.com/_layouts/download.aspx? SourceUrl=Hosted%20Documents/Power%20BI%20Program%20Overview%20%26%20Incentives.pdf](https://www.microsoftpartnerserverandcloud.com/_layouts/download.aspx?SourceUrl=Hosted%20Documents/Power%20BI%20Program%20Overview%20%26%20Incentives.pdf).</li> <li>Para obter mais informações sobre a demonstração de parceiro, visite a demonstração de parceiro página localizada em [powerapps.microsoft.com/partner-showcase](https://powerapps.microsoft.com/partner-showcase).</li> </ul> |  

#### <a name="requirements-azure-marketplace-list-consulting-service"></a>Requisitos: Azure Marketplace: lista: consultadoria de serviço  

| Requisitos | Detalhes |  
|:--- |:--- |  
| Características de oferta de serviço | O serviço de consultoria têm de cumprir os seguintes critérios.<ul> <li>Entrega o engagement um âmbito fixo, duração fixo, preço fixo (ou livre).</li> <li>Incluída principalmente para pré-vendas.</li> <li>O limite para um único cliente.</li> <li>Realize no site.</li> </ul> |  
| Requisitos de parceiro para serviços de consultoria | Tem de ter silver ou gold das competências seguintes na área de relevante para o seu serviço. <table><tr><th>Área de solução</th><th>Competência</th></tr><td>Infraestrutura e plataforma em nuvem</td><td>Plataforma em nuvem<br />Data Center</td><tr><td>Desenvolvimento de aplicações e ISV</td><td>Desenvolvimento de Aplicações<br />Integração de Aplicações<br />DevOps</td></tr><tr><td>Gestão de dados e análise</td><td>Análise de Dados<br />Data Platform</td></tr></table><ul> <li>Para mais informações sobre competências, visite competências através do Microsoft Partner Network página localizada em [partner.microsoft.com/membership/competencies](https://partner.microsoft.com/membership/competencies).</li> <li>Para obter mais informações sobre a listagem, visite o Azure Marketplace serviços de consultoria da página, localizada em [docs.microsoft.com/azure/marketplace/consulting-services](https://docs.microsoft.com/azure/marketplace/consulting-services).</li></ul> |  

<!-- #### Requirements: Azure Marketplace: List: Contact Me -->

---   

### <a name="trial"></a>Avaliação  

| Tipo de oferta | Loja | Detalhes |  
|:---        |:---        |:---     |  
| Livre / avaliação de SaaS | AppSource | [Listar os requisitos do tipo: versão de avaliação](#listing-type-requirements:-trial) |  
| Livre / avaliação de SaaS | Azure Marketplace | [Requisitos: Azure Marketplace: versão de avaliação: versão de avaliação gratuita / avaliação de SaaS](#requirements:-azure-marketplace:-trial:-free-trial-/-saas-trial) |  
| Demo interativa | AppSource | [Listar os requisitos do tipo: versão de avaliação](#listing-type-requirements:-trial) |  
| Demo interativa | Azure Marketplace | [Requisitos: Azure Marketplace: versão de avaliação: demonstração interativa](#requirements:-azure-marketplace:-trial:-interactive-demo) |  
| Versão de teste | AppSource | [Listar os requisitos do tipo: versão de avaliação](#listing-type-requirements:-trial) |  
| Versão de teste | Azure Marketplace | [Requisitos: Azure Marketplace: versão de avaliação: versão de teste](#requirements:-azure-marketplace:-trial:-test-drive) |  

#### <a name="requirements-azure-marketplace-trial"></a>Requisitos: Azure Marketplace: versão de avaliação  

| Requisito | Detalhes |  
|:--- |:--- |  
| Período de avaliação gratuita e experiência de avaliação gratuita | O cliente pode utilizar a aplicação gratuitamente durante um período de tempo limitado.<br /><br />O cliente não é necessário pagar quaisquer taxas de licenciamento ou de subscrição para a aplicação ou a oferta. O cliente não é necessário pagar para o produto de terceiros do primeiro Microsoft subjacente ou um serviço. Todas as opções de avaliação são implementadas para a sua subscrição do Azure. Tem de avaliação de controlo único da otimização de custos e gestão.<br /><br />Pode escolher uma versão de avaliação gratuita, demonstração interativa ou versão de teste. Independentemente daquilo que escolher, a avaliação gratuita tem oferecem cliente um predefinidos período de tempo para experimentar a aplicação sem custos adicionais.<ul> <li>Para iniciar o processo de criação de uma unidade de teste, envie um e-mail para [ amp-testdrive@microsoft.com ](mailto:amp-testdrive@microsoft.com).</li> </ul>Nota: Experiências de avaliação do Azure Marketplace SaaS têm de permitir aos clientes utilizar credenciais de trabalho para iniciar sessão.<ul> <li>Para obter mais informações, visite AppSource experiências avaliação secção localizada em [docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified#appsource-trial-experiences](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified#appsource-trial-experiences).</li> </ul> |  
| Solução facilmente configurável e pronto a utilizar | A aplicação tem de ser mais fácil e rápida configurar e a configurar. |  
| Disponibilidade / tempo de atividade | A plataforma ou aplicações de SaaS tem de ter um tempo de atividade de, pelo menos, 99,9%. |  
| Azure Active Directory | A oferta têm de permitir que Azure Active Directory (Azure AD) federadas início de sessão único (SSO) (Azure AD federado SSO) com o consentimento ativado. |  

#### <a name="requirements-azure-marketplace-trial-free-trial--saas-trial"></a>Requisitos: Azure Marketplace: versão de avaliação: versão de avaliação gratuita / avaliação de SaaS  

| Vantagem | Requisito |  
|:--- |:--- |  
| Permite que um cliente experimentar o produto antes de comprar com um método automatizado para converter para paga utilização. Também permite proofs de conceito para o cliente e o engagement conjunta com equipas de vendas da Microsoft. | A sua solução é uma máquina virtual ou modelo de solução.<br /><br />A solução é uma oferta de SaaS e oferecem um produto de SaaS multi-inquilino.<br /><br />Tem uma experiência de primeira execução para obter um cliente de cópias de segurança e executar rapidamente.<br /><br />Ter um único inquilino, mas estiver a adicionar clientes como os utilizadores convidados. |  

#### <a name="requirements-azure-marketplace-trial-interactive-demo"></a>Requisitos: Azure Marketplace: versão de avaliação: demonstração interativa  

| Vantagem | Requisito |  
|:--- |:--- |  
| Permite que os seus clientes ver a sua solução numa ação sem a complexidade de configuração. | A solução requer definições complexas que seriam difíceis de alcançar dentro do período de avaliação. |  

#### <a name="requirements-azure-marketplace-trial-test-drive"></a>Requisitos: Azure Marketplace: versão de avaliação: versão de teste  

| Vantagem | Requisito |  
|:--- |:--- |  
| Permite que um cliente experimentar o produto antes de poderem comprar.<br /><br />Fornece uma experiência assistida da sua solução com as definições de pré-configuradas.<br /><br />Seguem-se vantagens adicionais ao utilizar uma unidade de teste.<ul> <li>% de 27 de pesquisas de utilizador no mercado são foi refinado por utilizadores para mostrar apenas as ofertas com unidades de teste.</li> <li>Ofertas com unidades de teste geram n clientes potenciais do 38% mais clientes potenciais que oferece sem.</li> <li>% de 36 do novo aquisições de cliente no marketplace provenientes de clientes que demorou uma unidade de teste.</li> <li>Unidades de teste ativar sellers de campo da Microsoft compreender melhor o seu produto para os esforços vende conjunta.</li> </ul> | A solução é uma máquina virtual, um modelo de solução ou uma aplicação SaaS com um inquilino único ou é complexa para aprovisionar. <br /><br />Não tem um método para converter a versão de avaliação para uma oferta paga. |  

---

## <a name="transact-specific-listings"></a>Listagens de Transact-específico

### <a name="transact"></a>Transact  

| Tipo de oferta | Loja | Detalhes |   
|:---        |:---        | :--- |  
| As aplicações do Azure: aplicação gerida | Azure Marketplace | [Requisitos: Azure Marketplace: Transact: as aplicações do Azure: aplicação gerida](#requirements:-azure-marketplace:-transact:-azure-apps:-managed-app) |  
| As aplicações do Azure: modelo de solução | Azure Marketplace | [Requisitos: Azure Marketplace: Transact: as aplicações do Azure: modelo de solução](#requirements:-azure-marketplace:-transact:-azure-apps:-solution-template) |  
| Contentores | Azure Marketplace | [Requisitos: Azure Marketplace: Transact: contentor](#requirements:-azure-marketplace:-transact:-container) |  
| Aplicação SaaS  | Azure Marketplace | [Requisitos: Azure Marketplace: Transact: aplicações SaaS](#requirements:-azure-marketplace:-transact:-saas-app) |  
| Máquina virtual | Azure Marketplace | [Requisitos: Azure Marketplace: Transact: Máquina Virtual](#requirements:-azure-marketplace:-transact:-virtual-machine) |  

<!-- #### Requirements: Azure Marketplace: Transact: Azure apps: Managed app  

#### Requirements: Azure Marketplace: Transact: Azure apps: Solution template   -->

#### <a name="requirements-azure-marketplace-transact-container"></a>Requisitos: Azure Marketplace: Transact: contentor  

| Requisito | Detalhes |  
|:--- |:--- |  
| Faturação e medição | Suporta qualquer um do livre ou modelo de faturação BYOL. |  
| Imagem do docker | A imagem do contentor deve basear-se no formato de imagem de Docker e tem de ser retirada dos registos do contentor do Azure. |  

#### <a name="requirements-azure-marketplace-transact-saas-app"></a>Requisitos: Azure Marketplace: Transact: aplicações SaaS  

| Requisito | Detalhes |  
|:--- |:--- |  
| Faturação e medição | A oferta tem um preço uma taxa mensal do plana. Baseada na utilização preço e baseada na utilização *true segurança* opções não são suportadas neste momento. |  
| Cancelamento | A oferta é cancelable pelo cliente em qualquer altura. |  
| Página de destino de transação | Aloje uma página de destino de associação de marcas de transação do Azure. A página de destino permite que os seus clientes criar e gerir a sua conta de serviço de SaaS. |  
| Subscrição de SaaS API | Fornece um serviço que interage com a subscrição de SaaS para criar, atualizar e eliminar um plano de serviço e a conta de utilizador. Todas as alterações de API críticas têm de ser suportadas dentro de 24 horas. Quaisquer alterações de API não críticas são atualizadas periodicamente. |  

#### <a name="requirements-azure-marketplace-transact-virtual-machine"></a>Requisitos: Azure Marketplace: Transact: Máquina Virtual  

| Requisito | Detalhes |  
|:--- |:--- | 
| Faturação e medição | A VM tem de suportar BYOL ou pay as you go faturação mensal. |  
| Compatível com o Azure de disco rígido virtual (VHD) | As VMs têm de ser criadas no Windows ou Linux.<ul> <li>Para obter mais informações sobre como criar um VHD do Linux, visite a criar um VHD compatível com o Azure secção (baseado em Linux), localizado em [docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#2- Create-an-Azure-Compatible-VHD-Linux-based](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#2-create-an-azure-compatible-vhd-linux-based).</li> <li>Para obter mais informações sobre como criar um VHD do Windows, visite a criar um VHD compatível com o Azure secção (baseado no Windows), localizado em [docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#3- Create-an-Azure-Compatible-VHD-Windows-Based](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#3-create-an-azure-compatible-vhd-windows-based).</li> </ul> |  

## <a name="next-steps"></a>Passos Seguintes
*   Visite o [Azure Marketplace e AppSource publicador guia](./marketplace-publishers-guide.md) página.  
 
---  
