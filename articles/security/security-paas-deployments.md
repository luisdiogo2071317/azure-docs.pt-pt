---
title: Proteger implementações PaaS | Documentos da Microsoft
description: " Compreenda as vantagens de segurança de PaaS versus outros modelos de serviço de nuvem e Aprenda as práticas recomendadas para proteger a sua implementação de PaaS do Azure. "
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: techlake
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: da5d59aaaea8e6186609eb5f3419fba5e67d4279
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/15/2018
ms.locfileid: "42054167"
---
# <a name="securing-paas-deployments"></a>Proteger implementações de PaaS

Este artigo fornece informações que lhe permite:

- Compreender as vantagens de segurança de alojamento de aplicações na cloud
- Avaliar as vantagens de segurança da plataforma como serviço (PaaS) em relação a outros modelos de serviço em nuvem
- Alterar o foco de segurança de uma rede de centrada para uma abordagem de segurança de perímetro voltada para a identidade
- Implementar gerais PaaS melhores práticas recomendações de segurança

## <a name="cloud-security-advantages"></a>Vantagens de segurança da cloud
Há vantagens de segurança em ser na cloud. Num ambiente no local, as organizações prováveis ter responsabilidades por cumprir e recursos limitados disponíveis para investir em segurança, que cria um ambiente em que os invasores estejam habilitados a explorar vulnerabilidades em todas as camadas.

![Vantagens de segurança da era da cloud][1]

As organizações conseguem melhorar sua deteção de ameaças e tempos de resposta com capacidades de segurança com base na cloud de um fornecedor e inteligência da cloud.  Mudando responsabilidades para o fornecedor de cloud, as empresas podem obter mais cobertura de segurança, que permite que sejam realocar recursos de segurança e o orçamento para outras prioridades de negócios.

## <a name="division-of-responsibility"></a>Divisão de responsabilidade
É importante entender a divisão de responsabilidade entre e a Microsoft. No local, que possui a pilha inteira, mas como mover para a cloud algumas responsabilidades transferir para o Microsoft. A matriz de responsabilidade seguinte mostra as áreas da pilha numa implantação de SaaS, PaaS e IaaS, que é responsável por e a Microsoft é responsável.

![Zonas de responsabilidade][2]

Para todos os tipos de implementação de nuvem, que possui seus dados e identidades. É responsável por proteger a segurança dos seus dados e identidades, recursos no local e os componentes da nuvem que controle (que varia consoante o tipo de serviço).

Responsabilidades que são sempre retidas por si, independentemente do tipo de implementação, são:

- Dados
- Pontos Finais
- Conta
- Gestão de acesso

## <a name="security-advantages-of-a-paas-cloud-service-model"></a>Vantagens de segurança de um PaaS na cloud modelo de serviço
Utilizar a mesma matriz de responsabilidade, vamos analisar as vantagens de segurança de uma implementação de PaaS do Azure versus no local.

![Vantagens de segurança de PaaS][3]

A iniciar na parte inferior da pilha, a infraestrutura física, o Microsoft reduz riscos comuns e as responsabilidades. Uma vez que a nuvem da Microsoft é monitorizada continuamente pela Microsoft, é difícil a ataques. Não faz sentido para um atacante buscar a nuvem da Microsoft como um destino. A menos que o atacante tem muito dinheiro e recursos, o invasor provavelmente-se de que passar para outro destino.  

No meio da pilha, não há diferença entre uma implementação de PaaS e no local. Na camada da aplicação e a camada de gestão de acesso e de conta, terá dos riscos semelhantes. Na próxima seção passos deste artigo, iremos guiá-lo para as melhores práticas para eliminar ou minimizar esses riscos.

Na parte superior da pilha, a governação de dados e gestão de direitos, que tomar um risco que pode ser atenuado por gestão de chaves. (A gestão de chaves é descrita em práticas recomendadas.) Embora a gestão de chaves é uma responsabilidade adicional, tem áreas numa implantação de PaaS que já não tem de gerir, pelo que pode deslocar recursos para gestão de chaves.

A plataforma do Azure também fornece a proteção contra DDoS forte, utilizando várias tecnologias baseadas na rede. No entanto, todos os tipos de métodos de proteção de DDoS baseados em rede tem seus limites numa base por ligação e por centro de dados. Para ajudar a evitar o impacto dos ataques de DDoS grandes, pode tirar partido core cloud capacidade do Azure de permitir a rápida e automaticamente aumentar horizontalmente para se Defender contra ataques de DDoS. Vamos em mais detalhes sobre como pode fazer isso nos artigos de práticas recomendadas.

## <a name="modernizing-the-defenders-mindset"></a>Modernizar a mentalidade do defender
Com a PaaS implementações vêm uma mudança na sua abordagem geral para segurança. Mudar a necessidade de controlar tudo o que mesmo a responsabilidade de partilha com a Microsoft.

Outra diferença significativa entre PaaS e as implementações tradicionais no local, é uma nova vista do que define o perímetro segurança principal. Historicamente, o perímetro de segurança no local primário foi a sua rede e a maioria das estruturas de segurança no local utilizam a rede como seu pivot de segurança principal. Para implementações de PaaS, é mais bem atendidas Considerando a identidade a ser perímetro segurança principal.

## <a name="identity-as-the-primary-security-perimeter"></a>Identidade como perímetro segurança principal
Um das cinco características essenciais de computação em nuvem é amplo acesso à rede, que torna a centrada em rede pensando menos relevante. O objetivo de muita computação em nuvem é permitir que os utilizadores acedam aos recursos, independentemente da localização. Para a maioria dos usuários, a respetiva localização vai ser em algum lugar na Internet.

A figura seguinte mostra como o perímetro de segurança deixou de ser um perímetro de rede para um perímetro de identidade. A segurança se torna menos sobre defesa de rede e mais informações sobre a Defender seus dados, bem como gerir a segurança das suas aplicações e os utilizadores. A principal diferença é que deseja enviar mais perto de segurança para o que é importante para a sua empresa.

![Identidade como novo perímetro de segurança][4]

Inicialmente, os serviços de PaaS do Azure (por exemplo, funções da web e SQL do Azure) fornecido pouca ou nenhuma rede tradicional defesas no perímetro. Ele foi compreendido que finalidade do elemento era para ser exposto à Internet (função web) e que a autenticação fornece o novo perímetro (por exemplo, o BLOB ou o SQL do Azure).

Práticas de segurança moderno partem do princípio de que o adversário invadiu o perímetro da rede. Por conseguinte, as práticas de defesa modernos foram movidas para identidade. As organizações tem de estabelecer um perímetro de segurança baseada em identidades com forte higienização de autenticação e autorização (melhores práticas).

## <a name="recommendations-for-managing-the-identity-perimeter"></a>Recomendações para a gestão do perímetro de identidade

Princípios e padrões para o perímetro da rede estão disponíveis há décadas. Por outro lado, o mercado dispõe relativamente menos experiência com o uso de identidade como perímetro segurança principal. Dito isso, temos acumulado suficiente experiência para fornecer algumas recomendações gerais que comprovado no campo e aplicam-se a quase todos os serviços de PaaS.

O seguinte resume uma abordagem geral de práticas recomendada para gerenciar seu perímetro de identidade.

- **Não perca suas chaves ou as credenciais** proteger chaves e as credenciais é essencial para proteger implementações PaaS. Perda de chaves e as credenciais é um problema comum. É uma ótima solução utilizar uma solução centralizada, onde podem ser armazenados chaves e segredos em módulos de segurança de hardware (HSM). O Azure disponibiliza um HSM na cloud com o [do Azure Key Vault](../key-vault/key-vault-whatis.md).
- **Não coloque as credenciais e outros segredos em código-fonte ou o GitHub** a única coisa que pior do que perder as chaves e as credenciais é fazer com que uma entidade não autorizada obter acesso aos mesmos. Os invasores são capazes de tirar partido de bot tecnologias para localizar as chaves e segredos armazenados nos repositórios de código, como o GitHub. Não coloque chaves e segredos nestes repositórios de código fonte públicos.
- **Proteger as suas interfaces de gerenciamento de VM nos serviços de PaaS e IaaS híbrida** serviços de IaaS e PaaS são executados em máquinas virtuais (VMs). Dependendo do tipo de serviço, estão disponíveis várias interfaces de gestão que ativar remoto gere estas VMs diretamente. Protocolos de gestão remota, como [protocolo SSH (Secure Shell)](https://en.wikipedia.org/wiki/Secure_Shell), [protocolo RDP (Remote Desktop)](https://support.microsoft.com/kb/186607), e [PowerShell remoto](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/enable-psremoting) pode ser utilizado. Em geral, recomendamos que não ativar acesso remoto direto para VMs da Internet. Se estiver disponível, deve usar abordagens alternativas, como o uso de uma rede privada virtual numa rede virtual do Azure. Se abordagens alternativas não estão disponíveis, em seguida, certifique-se de que use as frases de acesso complexas e se estiver disponível, autenticação de dois fatores (como [multi-factor Authentication](../active-directory/authentication/multi-factor-authentication.md)).
- **Utilizar plataformas fortes de autenticação e autorização**

  - Utilize identidades federadas no Azure AD em vez de arquivos de utilizador personalizada. Quando utilizar identidades federadas, pode tirar partido de uma abordagem baseada na plataforma e delegar a gestão de identidades autorizadas para os seus parceiros. Uma abordagem de identidade federada é especialmente importante em cenários de quando os funcionários estão terminados e que informações necessita de ser refletido através de vários sistemas de identidade e autorização.
  - Utilize mecanismos de autenticação e autorização de plataforma fornecido em vez de código personalizado. O motivo é que a desenvolver o código de autenticação personalizado pode estar sujeita a erros. A maioria dos seus desenvolvedores não é especialistas em segurança e é pouco provável que tenha em atenção as sutilezas e os desenvolvimentos mais recentes na autenticação e autorização. Código comercial (por exemplo, da Microsoft) é, muitas vezes, extensivamente segurança revista.
  - Utilize a autenticação multifator. Autenticação multifator é o padrão atual para autenticação e autorização porque evita as vulnerabilidades de segurança inerentes a tipos de nome de utilizador e palavra-passe de autenticação. Acesso para ambas as interfaces de gestão do Azure (portal/remota do PowerShell) e orientadas para serviços o cliente deve ser criado e configurado para utilizar [Azure multi-factor Authentication (MFA)](../active-directory/authentication/multi-factor-authentication.md).
  - Utilize protocolos de autenticação padrão, como o OAuth2 e Kerberos. Esses protocolos foram extensivamente ponto a ponto revista e provavelmente são implementados como parte de suas bibliotecas de plataforma para autenticação e autorização.

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, nos concentramos nos vantagens de segurança de uma implementação de PaaS do Azure. Em seguida, aprenda as práticas recomendadas para proteger a sua PaaS soluções web e móveis. Vamos começar com o serviço de aplicações do Azure, base de dados SQL do Azure e Azure SQL Data Warehouse. Como artigos sobre práticas recomendadas para outros serviços do Azure tornam-se disponível, links serão fornecidos na lista seguinte:

- [Serviço de Aplicações do Azure](security-paas-applications-using-app-services.md)
- [Base de dados SQL do Azure e o armazém de dados SQL do Azure](security-paas-applications-using-sql.md)
- [Armazenamento do Azure](security-paas-applications-using-storage.md)
- Cache REDIS do Azure
- Service Bus do Azure
- Firewalls de aplicações Web

<!--Image references-->
[1]: ./media/security-paas-deployments/advantages-of-cloud.png
[2]: ./media/security-paas-deployments/responsibility-zones.png
[3]: ./media/security-paas-deployments/advantages-of-paas.png
[4]: ./media/security-paas-deployments/identity-perimeter.png
