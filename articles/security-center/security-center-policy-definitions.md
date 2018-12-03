---
title: Definições de política do Azure monitorizadas no Centro de segurança do Azure | Documentos da Microsoft
description: Definições de política do Azure monitorizadas no Centro de segurança do Azure.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: c89cb1aa-74e8-4ed1-980a-02a7a25c1a2f
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/22/2018
ms.author: rkarlin
ms.openlocfilehash: a4f9fc31f411d36e63775a3665b6dfe27eec7710
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/26/2018
ms.locfileid: "52319380"
---
# <a name="azure-security-policies-monitored-by-azure-security-center"></a>Políticas de segurança do Azure monitorizadas pelo centro de segurança do Azure
Este artigo fornece uma lista de definições de política do Azure que pode ser monitorizada no Centro de segurança.

## <a name="available-security-policies"></a>Políticas de segurança disponíveis

Para compreender as diretivas internas que são monitorizadas pelo centro de segurança, consulte a tabela seguinte:

| Política | O que faz a política |
| --- | --- |
|Auditar a ativação dos registos de diagnósticos no Service Fabric e nos Conjuntos de Dimensionamento de Máquinas Virtuais|Recomendamos que ative os Registos para poder recriar esse registo de atividade quando é necessário proceder a inquéritos em caso de um incidente ou comprometimento.|
|Auditoria de regras de autorização em espaços de nomes do Hub de Eventos|Os clientes do Hub de eventos não devem utilizar uma política de acesso de nível de espaço de nomes que fornece acesso a todas as filas e tópicos num espaço de nomes. Para alinhar com o menor privilégio modelo de segurança, deve criar políticas de acesso no nível de entidade para filas e tópicos para fornecer acesso a apenas a entidade específica.|
|Auditoria de existência de regras de autorização em entidades do Hub de Eventos|Auditar a existência de regras de autorização de entidades do Hub de eventos para conceder acesso menos privilegiado.|
|Auditar o acesso sem restrições à rede para as contas de armazenamento|Auditar o acesso de rede sem restrições nas definições de firewall da conta de armazenamento. Em vez disso, configure regras de rede para que apenas aplicativos de redes permitidas podem acessar a conta de armazenamento. Para permitir ligações a partir da Internet específico ou os clientes locais, o acesso pode ser concedido para tráfego a partir de redes virtuais do Azure específicos ou para intervalos de endereços IP de Internet públicos.|
|Auditar utilização das regras RBAC personalizadas|Funções incorporadas, como "Leitor de proprietário, contribuinte," em vez de funções do RBAC personalizadas, que são suscetíveis a erros de auditoria. Usar funções personalizadas é tratado como uma exceção e requer uma revisão rigorosa e a Modelagem de ameaças.|
|Auditoria de ativação de registos de diagnóstico no Azure Stream Analytics|Ativar registos de auditoria e retenha-los até um ano. Isto permite-lhe recriar os registos de atividade para efeitos de investigação quando ocorrer um incidente de segurança ou a rede estiver comprometida.|
|Auditoria de transferência segura para contas de armazenamento|Requisito de auditoria de seguro transferência na sua conta de armazenamento. Transferência segura é uma opção que força a sua conta de armazenamento para aceitar pedidos apenas a partir de ligações seguras (HTTPS). Utilização de HTTPS garante a autenticação entre o servidor e o serviço e protege os dados em trânsito de ataques de camada de rede, tais como o man-in-the-middle, interceptação e seqüestro de sessão.|
|Auditoria de aprovisionamento de um administrador do Azure Active Directory para o SQL server|Auditoria de aprovisionamento de um administrador do Azure Active Directory para o servidor SQL para ativar a autenticação do Azure AD. Autenticação do Azure AD permite a gestão de permissões simplificado e o gerenciamento centralizado de identidades de utilizadores de base de dados e outros serviços Microsoft.|
|Auditoria de regras de autorização em espaços de nomes do Service Bus|Os clientes do Service Bus não devem utilizar uma política de acesso de nível de espaço de nomes que fornece acesso a todas as filas e tópicos num espaço de nomes. Para alinhar com o menor privilégio modelo de segurança, deve criar políticas de acesso no nível de entidade para filas e tópicos para fornecer acesso a apenas a entidade específica.|
|Auditoria de ativação de registos de diagnóstico no Service Bus|Ativar registos de auditoria e retenha-los até um ano. Isto permite-lhe recriar os registos de atividade para efeitos de investigação quando ocorrer um incidente de segurança ou a rede estiver comprometida.|
|Auditar a definição da propriedade ClusterProtectionLevel para EncryptAndSign no Service Fabric|O Service Fabric fornece três níveis de proteção (nenhum, início de sessão e EncryptAndSign) para a comunicação de nó para nó a utilizar um certificado de cluster principal. Defina o nível de proteção para garantir que todas as mensagens de nó para nó são criptografadas e assinadas digitalmente.| 
|Auditoria de utilização do Azure Active Directory para autenticação de clientes no Service Fabric|Auditoria de utilização de autenticação de cliente apenas através do Azure Active Directory no Service Fabric| 
|Auditoria de ativação de registos de diagnóstico para o serviço de Pesquisa|Ativar registos de auditoria e retenha-los até um ano. Isto permite-lhe recriar os registos de atividade para efeitos de investigação quando ocorrer um incidente de segurança ou a rede estiver comprometida.| 
|Auditoria de ativação apenas de ligações seguras à Cache de Redis|Auditoria que ative apenas as ligações através de SSL para a Cache de Redis. Utilização de ligações seguras garante a autenticação entre o servidor e o serviço e protege os dados em trânsito de ataques de camada de rede, tais como o man-in-the-middle, interceptação e seqüestro de sessão| 
|Auditoria de ativação de registos de diagnóstico no Logic Apps|Ativar registos de auditoria e retenha-los até um ano. Isto permite-lhe recriar os registos de atividade para efeitos de investigação quando ocorrer um incidente de segurança ou a rede estiver comprometida.| 
|Auditoria de ativação de registos de diagnóstico no Cofre de Chaves|Ativar registos de auditoria e retenha-los até um ano. Isto permite-lhe recriar os registos de atividade para efeitos de investigação quando ocorrer um incidente de segurança ou a rede estiver comprometida.|
|Auditar a ativação dos registos de diagnóstico no Hub de Eventos|Ativar registos de auditoria e retenha-los até um ano. Isto permite que a recriação de registos de atividade para efeitos de investigação, quando ocorrer um incidente de segurança ou a rede estiver comprometida.| 
|Auditar a ativação dos registos de diagnóstico no Azure Data Lake Store|Ativar registos de auditoria e retenha-los até um ano. Isto permite-lhe recriar os registos de atividade para efeitos de investigação quando ocorrer um incidente de segurança ou a rede estiver comprometida.| 
|Auditoria de ativação de registos de diagnóstico no Data Lake Analytics|Ativar registos de auditoria e retenha-los até um ano. Isto permite-lhe recriar os registos de atividade para efeitos de investigação quando ocorrer um incidente de segurança ou a rede estiver comprometida.| 
|Auditar utilização das contas de armazenamento clássicas|Utilizar o Azure Resource Manager para as suas contas de armazenamento para fornecer aprimoramentos de segurança, tais como: controlo de acesso mais forte (RBAC), melhor auditoria, implementação Azure Resource Manager com base e governação de acesso de identidades geridas, acesso ao Cofre de chaves para segredos , Autenticação baseada no AD do azure e o suporte para etiquetas e grupos de recursos para facilitar a gestão segurança.| 
|Auditar a utilização das máquinas virtuais clássicas|Utilizar o Azure Resource Manager para as suas máquinas virtuais para fornecer aprimoramentos de segurança, tais como: controlo de acesso mais forte (RBAC), melhor auditoria, implementação Azure Resource Manager com base e governação de acesso de identidades geridas, acesso ao Cofre de chaves para segredos , Autenticação baseada no AD do azure e o suporte para etiquetas e grupos de recursos para facilitar a gestão segurança.| 
|Auditoria de configuração de regras de alerta de métrica em contas de Batch|Configuração de auditoria de regras de alerta de métrica na conta do Batch para ativar a métrica necessária.| 
|Auditoria de configuração de regras de alerta de métrica em contas de Batch|Configuração de auditoria de regras de alerta de métrica na conta do Batch para ativar a métrica necessária.| 
|Auditar a ativação dos registos de diagnóstico nas contas do Batch|Ativar registos de auditoria e retenha-los até um ano. Isto permite-lhe recriar os registos de atividade para efeitos de investigação quando ocorrer um incidente de segurança ou a rede estiver comprometida.| 
|Auditoria de ativação da encriptação de variáveis de conta de Automatização|É importante ativar a encriptação de recursos de variáveis de conta de Automatização ao armazenar dados confidenciais.| 
|Auditar a ativação dos registos de diagnóstico nos Serviços de Aplicação|Registos de auditoria que ative o diagnóstico na aplicação. Isto permite-lhe recriar os registos de atividade para efeitos de investigação se ocorrer um incidente de segurança ou a rede estiver comprometida.| 
|Auditar o estado de encriptação de dados transparente|Estado de encriptação de dados transparente para bases de dados SQL de auditoria.| 
|Auditar as Definições de auditoria ao nível do servidor SQL|Audita a existência de auditoria de SQL ao nível do servidor.| 
|[Pré-visualização]: monitorizar base de dados SQL não encriptada no Centro de Segurança do Azure|Bases de dados ou servidores SQL não encriptados serão monitorizados pelo centro de segurança do Azure como recomendações.| 
|[Pré-visualização]: monitorizar base de dados SQL não auditada no Centro de Segurança do Azure|Servidores do SQL Server e bases de dados que não têm a auditoria de SQL ativada serão monitorizados pelo centro de segurança do Azure como recomendações.| 
|[Pré-visualização]: monitorizar atualizações do sistema em falta no Centro de Segurança do Azure|Atualizações de sistema de segurança nos seus servidores em falta serão monitorizadas pelo centro de segurança do Azure como recomendações.| 
|[Pré-visualização]: auditar encriptação por blob em falta para contas de armazenamento|Esta política audita contas de armazenamento sem encriptação de Blobs. Aplica-se apenas aos tipos de recurso Microsoft. Storage, não a outros fornecedores de armazenamento. Rede possível acesso Just In Time será monitorizado pelo centro de segurança do Azure como recomendações.| 
|[Pré-visualização]: Monitorizar o possível acesso de rede Just In Time (JIT) no Centro de Segurança do Azure|Rede possível acesso apenas no Time (JIT) será monitorizado pelo centro de segurança do Azure como recomendações.| 
|[Pré-visualização]: monitorizar possível colocação em Lista de Permissões de aplicações no Centro de Segurança do Azure|Configuração de lista de permissões de aplicação possíveis será monitorizada pelo centro de segurança do Azure.| 
|[Pré-visualização]: monitorizar o acesso de rede permissivo no Centro de Segurança do Azure|Grupos de segurança de rede com regras demasiado permissivas serão monitorizados pelo centro de segurança do Azure como recomendações.| 
|[Pré-visualização]: monitorizar as vulnerabilidades de SO no Centro de Segurança do Azure|Servidores que não satisfaçam a linha de base configurada serão monitorizados pelo centro de segurança do Azure como recomendações.| 
|[Pré-visualização]: monitorizar o Endpoint Protection em falta no Centro de Segurança do Azure|Servidores sem um agente do Endpoint Protection instalado serão monitorizados pelo centro de segurança do Azure como recomendações.| 
|[Pré-visualização]: monitorizar Discos de VM não encriptados no Centro de Segurança do Azure|As VMs sem uma encriptação de disco ativada serão monitorizadas pelo centro de segurança do Azure como recomendações.| 
|[Pré-visualização]: monitorizar Vulnerabilidades de VM no Centro de Segurança do Azure|Monitoriza vulnerabilidades detetadas pela solução Avaliação de Vulnerabilidades e VMs sem uma solução de Avaliação de Vulnerabilidades no Centro de Segurança do Azure como recomendações.| 
|[Pré-visualização]: monitorizar aplicação Web não protegida no Centro de Segurança do Azure|Aplicativos Web sem uma proteção de Firewall de aplicações Web serão monitorizados pelo centro de segurança do Azure como recomendações.| 
|[Pré-visualização]: monitorizar pontos finais de redes desprotegidos no Centro de Segurança do Azure|Pontos finais de rede sem proteção por Firewall de próxima geração serão monitorizados pelo centro de segurança do Azure como recomendações.| 
|[Pré-visualização]: Monitorizar os resultados de avaliação de vulnerabilidade SQL no Centro de Segurança do Azure|Monitorizar os resultados da análise de Avaliação de Vulnerabilidades e recomendações para remediar vulnerabilidades de base de dados.| 
|[Pré-visualização]: Audite o número máximo de proprietários numa subscrição|Recomenda-se a designação no máximo de 3 proprietários da subscrição para poder reduzir o potencial de falha de um proprietário comprometido.| 
|[Pré-visualização]: Audite o número mínimo de proprietários para a subscrição|Recomenda-se a designação de mais do que um proprietário da subscrição para ter a redundância de acesso de administrador.| 
|[Pré-visualização]: Audite contas com permissões de proprietário que não sejam ativadas pelo MFA numa subscrição|O Multi-Factor Authentication (MFA) deve estar ativado para todas as contas de subscrição com permissões de proprietário para evitar falhas de contas ou de recursos.| 
|[Pré-visualização]: Audite as contas com permissões de escrita que não sejam ativadas pelo MFA numa subscrição|O Multi-Factor Authentication (MFA) deve estar ativado para todas as contas de subscrição com privilégios de escrita para evitar falhas de contas ou de recursos.| 
|[Pré-visualização]: Audite as contas com permissões de leitura que não sejam ativadas pelo MFA numa subscrição|O Multi-Factor Authentication (MFA) deve estar ativado para todas as contas de subscrição com privilégios de leitura para evitar uma falha de contas ou de recursos.| 
|[Pré-visualização]: Audite contas preteridas com permissões de proprietário numa subscrição|As contas preteridas com permissões de proprietário devem ser removidas da sua subscrição. As contas preteridas são contas que foram bloqueadas de iniciar sessão.| 
|[Pré-visualização]: Audite contas preteridas numa subscrição|Deve remover as contas preteridas das suas subscrições. As contas preteridas são contas que foram bloqueadas de iniciar sessão.| 
|[Pré-visualização]: Audite contas externas com permissões de proprietário numa subscrição|As contas externas com permissões de proprietário devem ser removidas da sua subscrição para impedir acessos não monitorizados.| 
|[Pré-visualização]: Audite contas externas com permissões de escrita numa subscrição|As contas externas com privilégios de escrita devem ser removidas da sua subscrição para impedir o acesso não monitorizado.| 
|[Pré-visualização]: Audite contas externas com permissões de leitura numa subscrição|Deve remover as contas externas com privilégios de leitura da sua subscrição para impedir o acesso não monitorizado.| 




## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu a configurar as políticas de segurança no Centro de Segurança. Para saber mais sobre o Centro de Segurança, veja os artigos seguintes:

* [Guia de operações e planeamento do Centro de Segurança do Azure](security-center-planning-and-operations-guide.md): saiba como planear e compreender as considerações de conceção do Centro de Segurança do Azure.
* [Monitorização de estado de funcionamento de segurança no Centro de Segurança do Azure](security-center-monitoring.md): saiba como monitorizar o estado de funcionamento dos seus recursos do Azure.
* [Gerir e responder a alertas de segurança no Centro de Segurança do Azure](security-center-managing-and-responding-alerts.md): saiba como gerir e responder a alertas de segurança.
* [Monitor partner solutions with Azure Security Center](security-center-partner-solutions.md) (Monitorizar soluções de parceiros com o Centro de Segurança do Azure): saiba como monitorizar o estado de funcionamento das suas soluções de parceiros.
* [Azure Security Center FAQ](security-center-faq.md) (FAQ do Centro de Segurança do Azure): obtenha respostas às perguntas mais frequentes sobre como utilizar o serviço.
* [Blogue de Segurança do Azure](http://blogs.msdn.com/b/azuresecurity/): encontre publicações no blogue acerca da segurança e conformidade do Azure.

Para saber mais sobre o Azure Policy, veja [O que é o Azure Policy?](../azure-policy/azure-policy-introduction.md)
