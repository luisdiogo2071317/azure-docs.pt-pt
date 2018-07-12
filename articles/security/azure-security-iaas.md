---
title: Melhores práticas de segurança de IaaS cargas de trabalho no Azure | Documentos da Microsoft
description: " A migração de cargas de trabalho de IaaS do Azure oferece oportunidades para reavaliar a nossa designs "
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 02c5b7d2-a77f-4e7f-9a1e-40247c57e7e2
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/14/2018
ms.author: barclayn
ms.openlocfilehash: 37620e70377e3f1fbeeeb73aaa294c5f54cf5b3d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38724100"
---
# <a name="security-best-practices-for-iaas-workloads-in-azure"></a>Melhores práticas de segurança para cargas de trabalho de IaaS no Azure

Na maioria dos infraestrutura como um cenários de serviço (IaaS), [máquinas virtuais do Azure (VMs)](https://docs.microsoft.com/azure/virtual-machines/) é a carga de trabalho principal para organizações que utilizam a cloud de informática. Esse fato é especialmente evidente nos [cenários híbridos](https://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) em que as organizações querem lentamente migrar cargas de trabalho para a cloud. Em tais cenários, siga os [considerações de segurança geral para IaaS](https://social.technet.microsoft.com/wiki/contents/articles/3808.security-considerations-for-infrastructure-as-a-service-iaas.aspx)e aplique as melhores práticas de segurança para todas as suas VMs.

A responsabilidade de segurança baseia-se no tipo de serviço em nuvem. A tabela a seguir resume o saldo de responsabilidade para a Microsoft e:

![Áreas de responsabilidade](./media/azure-security-iaas/sec-cloudstack-new.png)

Requisitos de segurança variam consoante vários fatores, incluindo diferentes tipos de cargas de trabalho. Não uma dessas práticas recomendadas pode por si só, proteger seus sistemas. Como qualquer outra coisa em segurança, terá de escolher as opções apropriadas e veja como as soluções podem se complementam ao preencher as lacunas.

Este artigo aborda várias práticas recomendadas de segurança VM, cada um derivado de cliente e experiências de diretas da Microsoft com as VMs.

As melhores práticas baseiam-se um consenso de opinião, e trabalhar com recursos atuais da plataforma do Azure e conjuntos de recursos. Uma vez que opiniões e as tecnologias podem mudar ao longo do tempo, este artigo será atualizado refletir essas alterações.

## <a name="use-privileged-access-workstations"></a>Utilizar estações de trabalho de acesso privilegiado

As organizações, muitas vezes, enquadram-se vítimas ataques cibernéticos porque os administradores executem ações durante o uso de contas com direitos elevados. Embora essa não seja o resultado de atividades maliciosas, isso acontece porque a configuração existente e os processos permitem-lo. A maioria destes utilizadores compreende o risco dessas ações de um ponto de vista conceitual, mas ainda optar por fazê-los.

Fazendo coisas como a verificação do e-mail e navegação na Internet parecem inocente o suficiente. Mas eles podem expor contas elevadas para comprometer, atores maliciosos. Navegação de atividades, elaboradas especialmente e-mails ou outras técnicas pode ser utilizado para obter acesso à sua empresa. Recomendamos vivamente a utilização de estações de trabalho de gestão segura (SAWs) para realizar todas as tarefas de administração do Azure. SAWs são uma maneira de reduzir a exposição a comprometimento acidental.

Estações de trabalho acesso privilegiado (PAWs) fornecem um sistema operativo dedicado para tarefas confidenciais – o que está protegida contra ataques de Internet e vetores de ameaças. Separar tarefas confidenciais e contas a partir de estações de trabalho de utilização diária e dispositivos fornece proteção forte. Essa separação limita o impacto de ataques de phishing, aplicação e OS vulnerabilidades vários ataques de representação e ataques de roubo de credenciais. (batimentos de tecla registo, Pass-the-Hash e Pass-the-Ticket)

A abordagem da PAW é uma extensão da prática bem estabelecida e recomendada para utilizar uma conta administrativa atribuída individualmente. A conta administrativa é separada a partir de uma conta de usuário padrão. Uma PAW fornece uma estação de trabalho fidedigna para essas contas confidenciais.

Para obter mais orientações de implementação e informações, consulte [estações de trabalho de acesso privilegiado](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/privileged-access-workstations).

## <a name="use-multi-factor-authentication"></a>Utilizar o multi-factor Authentication

No passado, o perímetro da rede foi utilizado para controlar o acesso a dados empresariais. Num mundo de cloud-first, mobile-first, a identidade é o plano de controlo: usá-lo para controlar o acesso aos serviços de IaaS a partir de qualquer dispositivo. Também usá-lo para obter visibilidade e informações sobre onde e como os seus dados está a ser utilizados. Proteger a identidade digital dos seus utilizadores do Azure é o fundamento de proteger as suas subscrições do roubo de identidade e de outros aos cybercrimes.

Uma das etapas mais vantajosa que pode executar para proteger uma conta é ativar a autenticação de dois fatores. Autenticação de dois fatores é uma forma de autenticar ao utilizar algo além de uma palavra-passe. Ele ajuda a mitigar o risco de acesso por alguém que consiga obter palavra-passe de alguém de outra.

[O Azure multi-factor Authentication](../active-directory/authentication/multi-factor-authentication.md) ajuda a salvaguardar o acesso aos dados e aplicações, satisfazendo um processo de início de sessão simples. Proporciona uma autenticação segura com uma variedade de opções de verificação simples, telefonema, mensagem de texto ou notificação de aplicação móvel. Os utilizadores escolher o método que preferir.

A maneira mais fácil de utilizar o multi-factor Authentication é a aplicação móvel Microsoft Authenticator, que pode ser utilizada em dispositivos móveis com o Windows, iOS e Android. Com a versão mais recente do Windows 10 e a integração do Active Directory no local com o Azure Active Directory (Azure AD), [Windows Hello para empresas](../active-directory/active-directory-azureadjoin-passport-deployment.md) pode ser utilizado para o início de sessão único totalmente integrado-aos recursos do Azure. Neste caso, o dispositivo Windows 10 é utilizado como o segundo fator de autenticação.

Para contas de gerir a sua subscrição do Azure e para as contas que podem iniciar sessão em máquinas virtuais, utilizar o multi-factor Authentication dá-lhe um muito maior nível de segurança que usar apenas uma palavra-passe. Outras formas de autenticação de dois fatores podem funcionar tão bem, mas pode ser complicado implementá-las se não ainda estiverem na produção.

Captura de ecrã seguinte mostra algumas das opções disponíveis para o Azure multi-factor Authentication:

![Opções de autenticação Multifatores](./media/azure-security-iaas/mfa-options.png)

## <a name="limit-and-constrain-administrative-access"></a>Limitar e restringir o acesso administrativo

Proteger as contas que podem gerir a sua subscrição do Azure é importante. O comprometimento de qualquer uma dessas contas nega o valor de todas as outras etapas que poderá efetuar para garantir a confidencialidade e integridade dos seus dados. Recentemente, ilustrada pela [Edward Snowden](https://en.wikipedia.org/wiki/Edward_Snowden) ataques internos impõem uma ameaça enorme para a segurança geral de qualquer organização.

Avalie o indivíduos de direitos administrativos ao seguintes critérios de como estas:

- São eles executar tarefas que exigem privilégios administrativos?
- A frequência com que as tarefas são executadas?
- Existe um motivo específico, por que as tarefas não podem ser efetuadas por outro administrador em nome deles?

Documente todas as outras abordagens alternativas conhecidas para conceder o privilégio e por que motivo por que não são aceitável.

Administração just-in-time impede que a existência de desnecessária de contas com direitos elevados durante períodos quando esses direitos não são necessários. Contas de tem direitos elevados por um período limitado, para que os administradores podem fazer seus trabalhos. Em seguida, esses direitos são removidos no final de uma mudança ou quando uma tarefa está concluída.

Pode usar [Privileged Identity Management](../active-directory/privileged-identity-management/pim-configure.md) para gerir, monitorize e controle de acesso na sua organização. Ele ajuda a ter em mente as ações que indivíduos efetuar na sua organização. Também traz administração just-in-time para o Azure AD, introduzindo o conceito de administradores elegíveis. Estes são indivíduos que têm contas com o potencial de ser concedidos direitos de administrador. Estes tipos de utilizadores podem aceder através de um processo de ativação e ser concedidos direitos de administrador para um período limitado.

## <a name="use-devtest-labs"></a>Utilizar o DevTest Labs

Azure para ambientes de desenvolvimento e laboratórios elimina os atrasos que introduz a aquisição de hardware. Isto permite às organizações ganha agilidade no desenvolvimento e teste. Por outro lado, uma falta de familiaridade com o Azure ou um desejo para o ajudar a agilizar sua adoção pode levar o administrador para ser excessivamente permissivo com atribuição de direitos. Este risco inadvertidamente pode expor a organização a ataques internos. Alguns utilizadores poderão ter acesso muito mais do que eles devem ter.

O [Azure DevTest Labs](../devtest-lab/devtest-lab-overview.md) service utiliza [controlo de acesso](../role-based-access-control/overview.md) (RBAC). Ao utilizar o RBAC, pode separar responsabilidades dentro da sua equipa em funções que concedem apenas o nível de acesso necessário aos utilizadores para realizarem seus trabalhos. RBAC vem com funções predefinidas (proprietário, o utilizador de laboratório e contribuinte). Pode até usar estas funções para atribuir direitos aos parceiros externos e simplificar consideravelmente a colaboração.

Uma vez que os laboratórios DevTest utiliza o RBAC, é possível criar adicionais, [funções personalizadas](../lab-services/devtest-lab-grant-user-permissions-to-specific-lab-policies.md). DevTest Labs não apenas simplifica o gerenciamento de permissões, simplifica o processo de obtenção de ambientes aprovisionados. Ele também ajuda a lidar com outros desafios típicos de equipes que trabalham em ambientes de desenvolvimento e teste. Requer uma preparação, mas a longo prazo, ele irá facilitar as coisas para a sua equipa.

Funcionalidades de laboratórios DevTest do Azure incluem:

- Controle administrativo sobre as opções disponíveis para os utilizadores. O administrador pode gerenciar centralmente a coisas como permitidos de tamanhos de VM, o número máximo de VMs, e quando as VMs são iniciadas e de encerramento.
- Automatização de criação do ambiente de laboratório.
- Controlo de custos.
- Distribuição simplificada de VMs para o trabalho colaborativo temporária.
- Self-Service que permite aos utilizadores aprovisionar os seus laboratórios através de modelos.
- Gerir e a limitação de consumo.

![DevTest Labs](./media/azure-security-iaas/devtestlabs.png)

Sem custos adicionais está associado com a utilização do DevTest Labs. A criação de laboratórios, políticas, modelos e artefactos é gratuita. Paga apenas os recursos do Azure utilizados em seus laboratórios, como máquinas virtuais, contas de armazenamento e redes virtuais.

## <a name="control-and-limit-endpoint-access"></a>Controle e o limite de acesso de ponto final

Alojamento de laboratórios ou sistemas de produção no Azure significa que seus sistemas precisam ser acessível a partir da Internet. Por predefinição, uma nova máquina virtual de Windows tem a porta RDP acessível a partir da Internet e uma máquina virtual Linux tem de abrir a porta de SSH. Passos para pontos de extremidade exposto de limite são necessário para minimizar o risco de acesso não autorizado.

Tecnologias no Azure podem ajudar a limitar o acesso a esses pontos de extremidade administrativos. No Azure, pode utilizar [grupos de segurança de rede](../virtual-network/security-overview.md) (NSGs). Quando utiliza o Azure Resource Manager para a implementação, os NSGs limitam o acesso de todas as redes para apenas os pontos finais de gestão (RDP ou SSH). Quando pensa NSGs, pense em ACLs de roteador. Pode usá-los para controlar rigidamente a comunicação de rede entre vários segmentos das redes do Azure. Isso é semelhante à criação de redes em redes de perímetro ou a outras redes isoladas. Eles não Inspecione o tráfego, mas eles ajudam com a segmentação de rede.

Uma forma mais dinâmica de limitar o acesso às máquinas virtuais é utilizar o Centro de segurança do Azure [Just-in administração time](../security-center/security-center-just-in-time.md). Centro de segurança pode bloquear as VMs do Azure e fornece acesso quando necessário. O processo funciona ao permitir acesso a um utilizador que solicitá-lo Depois de verificar com base na respetiva [controlo de acesso baseado em funções](../role-based-access-control/role-assignments-portal.md) (RBAC), que tem as permissões necessárias. Vontade de centro de segurança do Azure, em seguida, verifique os grupos de segurança de rede (NSGs) para permitir tráfego de entrada necessário.

### <a name="site-to-site-vpnvpn-gatewayvpn-gateway-howto-site-to-site-resource-manager-portalmd"></a>[VPN site a site](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

Uma VPN de site a site expande a sua rede no local para a cloud. Isso lhe dá outra oportunidade para utilizar NSGs, porque também pode modificar os NSGs para não permitir o acesso a partir de qualquer lugar em vez de rede local. Em seguida, pode exigir que a administração é feita ao ligar primeiro à rede do Azure através de VPN.

A opção de VPN de site a site poderá ser mais atraente em casos em que está a alojar os sistemas de produção que estão estreitamente integrados com os seus recursos no local no Azure.

### <a name="point-to-sitevpn-gatewayvpn-gateway-howto-point-to-site-rm-psmd"></a>[Ponto a site](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

Em situações em que deseja gerenciar sistemas que não precisam de acesso a recursos no local. Esses sistemas podem ser isolados em sua própria rede virtual do Azure. Os administradores podem VPN para o Azure alojado ambiente da sua estação de trabalho administrativa.

>[!NOTE]
>Pode utilizar qualquer uma das opções de VPN para reconfigurar as ACLs nos NSGs para não permitir o acesso a pontos finais de gestão da Internet.

### <a name="remote-desktop-gatewayactive-directoryauthenticationhowto-mfaserver-nps-rdgmd"></a>[Gateway de Ambiente de Trabalho Remoto](../active-directory/authentication/howto-mfaserver-nps-rdg.md)

Pode utilizar o Gateway de ambiente de trabalho remoto para ligar em segurança para servidores de ambiente de trabalho remoto através de HTTPS, ao aplicar controlos mais detalhados para essas ligações.

Recursos de que teria acesso incluem:

- Opções do administrador para limitar as ligações a pedidos de sistemas específicos.
- Autenticação de smart card ou Azure multi-factor Authentication.
- Controle sobre quais sistemas alguém pode ligar através do gateway.
- Controlo sobre o redirecionamento de dispositivo e o disco.

### <a name="vm-availability"></a>Disponibilidade da VM

Se uma VM executa aplicações críticas que tem de ter elevada disponibilidade, é altamente recomendável que várias VMs são utilizadas. Para uma melhor disponibilidade, criar, pelo menos, duas VMs na [conjunto de disponibilidade](../virtual-machines/windows/tutorial-availability-sets.md).

[O Azure Load Balancer](../load-balancer/load-balancer-overview.md) também requer que as VMs com balanceamento de carga pertencem ao mesmo conjunto de disponibilidade. Se estas VMs têm de ser acedidas a partir da Internet, tem de configurar uma [Balanceador de carga com acesso à Internet](../load-balancer/load-balancer-internet-overview.md).

## <a name="use-a-key-management-solution"></a>Utilize uma solução de gestão de chaves

A gestão de chaves segura é essencial para proteger os dados na cloud. Com o [do Azure Key Vault](../key-vault/key-vault-whatis.md), pode armazenar em segurança as chaves de encriptação e pequenos segredos, como palavras-passe em módulos de segurança de hardware (HSMs). Para maior segurança, pode importar ou gerar chaves nos HSMs.

Microsoft processa as suas chaves em FIPS 140-2 de nível 2 validadas HSMs (hardware e firmware). Monitorize e audite a utilização de chaves com o registo do Azure: registos de pipes no sistema do Azure ou sistemas Security Information and Event Management (SIEM) para a deteção de ameaças e análise adicional.

Qualquer pessoa com uma subscrição do Azure pode criar e utilizar cofres de chaves. Embora a chave de cofre beneficiar os programadores e administradores de segurança, pode ser implementado e gerido por um administrador que é responsável por gerir serviços do Azure numa organização.

## <a name="encrypt-virtual-disks-and-disk-storage"></a>Encriptar discos virtuais e armazenamento em disco

[O Azure Disk Encryption](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0) aborda ameaças de roubo de dados ou exposição contra acesso não autorizado que é obtida ao mover um disco. O disco pode ser ligado a outro sistema como uma forma de ignorar a outros controlos de segurança. Utilizações de encriptação do disco [BitLocker](https://technet.microsoft.com/library/hh831713) no Windows e o DM-Crypt no Linux para encriptar o sistema operativo e unidades de dados. O Azure Disk Encryption integra-se com o Key Vault para controlar e gerir as chaves de encriptação. Está disponível para VMs padrão e as VMs com o armazenamento premium.

Para obter mais informações, consulte [do Azure Disk Encryption no Windows e VMs de IaaS Linux](azure-security-disk-encryption.md).

[Encriptação do serviço de armazenamento do Azure](../storage/common/storage-service-encryption.md) ajuda a proteger os dados em repouso. Este é ativado ao nível da conta de armazenamento. Ele encripta os dados à medida que ele é escrito nos nossos datacenters e estes são desencriptados automaticamente conforme acessá-lo. Ele suporta os seguintes cenários:

- Encriptação de blobs de blocos, blobs de acréscimo e blobs de páginas
- Encriptação de arquivados VHDs e modelos trazidos para o Azure no local
- Encriptação de discos de dados e SO subjacentes para VMs IaaS que criou utilizando os VHDs

Antes de continuar com a encriptação de armazenamento do Azure, tenha em atenção duas limitações:

- Não está disponível em contas de armazenamento clássicas.
- Esta encripta apenas os dados escritos depois de encriptação está ativada.

## <a name="use-a-centralized-security-management-system"></a>Utilizar um sistema de gerenciamento de segurança centralizada

Os servidores devem ser monitorados para aplicação de patches, configuração, eventos e atividades que podem ser considerados como questões de segurança. Para resolver essas questões, pode usar [Centro de segurança](https://azure.microsoft.com/services/security-center/) e [Operations Management Suite segurança e conformidade](https://azure.microsoft.com/services/security-center/). As duas opções ir além da configuração no sistema operativo. Eles também fornecem a monitorização da configuração da infraestrutura subjacente, como a configuração de rede e a utilização de aplicação virtual.

## <a name="manage-operating-systems"></a>Gerir sistemas operativos

Numa implementação do IaaS, é ainda responsáveis pela gestão dos sistemas que implementar, assim como qualquer outro servidor ou estação de trabalho no seu ambiente. Aplicação de patches, proteção, atribuições de direitos e qualquer outra atividade relacionados com a manutenção do seu sistema ainda são da responsabilidade do cliente. Para sistemas que estão estreitamente integrados com os seus recursos no local, pode querer utilizar as mesmas ferramentas e procedimentos que está a utilizar no local para coisas como antivírus, antimalware, aplicação de patches e cópia de segurança.

### <a name="harden-systems"></a>Proteger sistemas

Todas as máquinas virtuais IaaS do Azure devem ser protegidas, para que elas expõem apenas pontos finais de serviço que são necessários para as aplicações que são instaladas. Para máquinas de virtuais do Windows, siga as recomendações que a Microsoft publica como linhas de base para o [Security Compliance Manager](https://technet.microsoft.com/solutionaccelerators/cc835245.aspx) solução.

Gestor de conformidade de segurança é uma ferramenta gratuita. Pode usá-lo rapidamente configurar e gerenciar seus desktops, o Data Center tradicional e a nuvem privada e pública utilizando a política de grupo e o System Center Configuration Manager.

Security Compliance Manager fornece pronto para implementar políticas e pacotes de configuração do DCM que são testados. Essas linhas de base são baseiam [orientações de segurança da Microsoft](https://technet.microsoft.com/library/cc184906.aspx) setor e recomendações de melhores práticas. Eles ajudam a gerir os descompassos de configuração, os requisitos de conformidade de endereço e reduza as ameaças de segurança.

Pode usar o Security Compliance Manager para importar a configuração atual de seus computadores ao utilizar dois métodos diferentes. Em primeiro lugar, pode importar políticas de grupo baseada no Active Directory. Em segundo lugar, é possível importar a configuração de um "principal dourada" máquina de referência utilizando o [LocalGPO ferramenta](https://blogs.technet.microsoft.com/secguide/2016/01/21/lgpo-exe-local-group-policy-object-utility-v1-0/) para criar cópias de segurança a política de grupo local. Em seguida, pode importar a política de grupo local no Security Compliance Manager.

Comparar os seus padrões de práticas recomendadas do setor, personalizá-los e criar novas políticas e pacotes de configuração de gestão de configuração pretendida. Linhas de base foram publicadas para todos os sistemas operativos suportados, incluindo a atualização de aniversário do Windows 10 e Windows Server 2016.


### <a name="install-and-manage-antimalware"></a>Instalar e gerenciar antimalware

Para ambientes alojados em separado do seu ambiente de produção, pode usar uma extensão de antimalware para ajudar a proteger as suas máquinas virtuais e serviços em nuvem. Integra-se [Centro de segurança do Azure](../security-center/security-center-intro.md).

[O Microsoft Antimalware](azure-security-antimalware.md) inclui recursos como a proteção em tempo real, análise agendada, remediação de software maligno, atualizações de assinatura, atualizações de mecanismos, exemplos de relatórios, recolha de eventos de exclusão, e [desuportedoPowerShell](https://msdn.microsoft.com/library/dn771715.aspx).

![Antimalware do Azure](./media/azure-security-iaas/azantimalware.png)

### <a name="install-the-latest-security-updates"></a>Instalar as atualizações de segurança mais recentes 

Algumas das primeiras cargas de trabalho que os clientes mover para o Azure são laboratórios e sistemas de face externa. Se as máquinas de virtuais alojados no Azure alojar aplicações ou serviços que precisam de estar acessíveis à Internet, ser vigilante sobre a aplicação de patches. Aplicar o patch ponha o sistema operativo. Vulnerabilidades não corrigidas em aplicativos de terceiros também podem levar a problemas que podem ser evitados se gerenciamento de patches boa estiver em vigor.

### <a name="deploy-and-test-a-backup-solution"></a>Implementar e testar uma solução de cópia de segurança

Tal como atualizações de segurança, uma cópia de segurança tem de ser tratados da mesma forma que lidar com qualquer outra operação. Isso é verdadeiro de sistemas que fazem parte do seu ambiente de produção, alargando-se para a cloud. Sistemas de teste e desenvolvimento tem de seguir as estratégias de cópia de segurança que fornecem capacidades de restauro que são semelhantes às que os utilizadores que cresceram acostumados a, com base nessa experiência com ambientes no local.

Cargas de trabalho de produção movidas para o Azure, devem integrar com soluções de cópia de segurança existentes sempre que possível. Em alternativa, pode utilizar [cópia de segurança do Azure](../backup/backup-azure-arm-vms.md) para ajudar a solucionar seus requisitos de cópia de segurança.

## <a name="monitor"></a>Monitorizar

### <a name="security-centersecurity-centersecurity-center-intromd"></a>[Centro de Segurança](../security-center/security-center-intro.md)

Centro de segurança fornece avaliação contínua do Estado de segurança dos seus recursos do Azure para identificar potenciais vulnerabilidades de segurança. Uma lista de recomendações orienta-o no processo de configuração de controlos necessários.

Os exemplos incluem:

- Aprovisionamento de antimalware para ajudar a identificar e remover software malicioso.
- Configurar grupos de segurança de rede e regras para controlar o tráfego para as máquinas virtuais.
- Aprovisionamento de firewalls de aplicações web para ajudar na defesa contra ataques que visam seus aplicativos web.
- Implementar atualizações do sistema em falta.
- Contemplando a configurações de SO que não correspondam a linhas de base recomendadas.

A imagem seguinte mostra algumas das opções que pode ativar no Centro de segurança.

![Políticas do Centro de segurança do Azure](./media/azure-security-iaas/security-center-policies.png)

### <a name="operations-management-suiteoperations-management-suiteoperations-management-suite-overviewmd"></a>[Operations Management Suite](../operations-management-suite/operations-management-suite-overview.md) 

Operation Management Suite é uma solução baseada na cloud IT management que o ajuda a gerenciar e proteger no local e infraestrutura na cloud da Microsoft. Uma vez o Operations Management Suite é implementado como um serviço baseado na nuvem, podem ser implementado rapidamente e com o mínimo investimento em recursos de infraestrutura.

Novas funcionalidades fornecidas automaticamente, poupando-lhe de manutenção contínua e os custos de atualização. Operations Management Suite também se integra com o System Center Operations Manager. Tem diferentes componentes para o ajudar a gerenciar melhor as suas cargas de trabalho do Azure, incluindo uma [segurança e conformidade](../operations-management-suite/oms-security-getting-started.md) módulo.

Pode utilizar as funcionalidades de segurança e conformidade no Operations Management Suite para ver informações sobre seus recursos. A informação está organizada em quatro categorias principais:

- **Domínios de segurança**: explorar ainda mais os registos de segurança ao longo do tempo. Aceder a avaliação de software maligno, atualizar a avaliação, informações de segurança de rede, informações de identidade e acesso e computadores com eventos de segurança. Tire partido de acesso rápido ao dashboard do Centro de segurança do Azure.
- **Problemas relevantes**: identificar rapidamente o número de problemas ativos e a gravidade desses problemas.
- **Deteções (pré-visualização)**: identificar ataque padrões ao visualizar os alertas de segurança à medida que acontecem em relação aos recursos.
- **Informações sobre ameaças**: identificar ataque padrões ao visualizar o número total de servidores com tráfego IP malicioso de saída, o tipo malicioso da ameaça e um mapa que mostra onde são provenientes destes IPs.
- **Consultas de segurança comuns**: ver uma lista das consultas de segurança mais comuns que pode utilizar para monitorizar o seu ambiente. Quando clica dessas consultas, o **pesquisa** painel abre-se e mostra os resultados para essa consulta.

Captura de ecrã seguinte mostra um exemplo das informações que pode apresentar o Operations Management Suite.

![Linhas de base de segurança do Operations Management Suite](./media/azure-security-iaas/oms-security-baseline.png)

### <a name="monitor-vm-performance"></a>Monitorizar o desempenho de VM

Abuso de recursos pode ser um problema quando os processos VM consumam mais recursos do que deveriam. Problemas de desempenho com uma VM podem levar a interrupção do serviço, que viola o princípio de segurança de disponibilidade. Por esse motivo, é imperativo para monitorizar o acesso VM não apenas reativamente, enquanto está a ocorrer um problema, mas também de forma proativa, com desempenho de linha de base, conforme medido durante o funcionamento normal.

Ao analisar [ficheiros de registo de diagnóstico do Azure](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/), pode monitorizar os seus recursos VM e identificar potenciais problemas que possam comprometer o desempenho e disponibilidade. A extensão de diagnóstico do Azure fornece capacidades de monitorização e diagnóstico em VMs baseadas no Windows. Pode ativar estas capacidades, incluindo a extensão como parte da [modelo Azure Resource Manager](../virtual-machines/windows/extensions-diagnostics-template.md).

Também pode utilizar [do Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-metrics.md) ganhar visibilidade sobre o estado de funcionamento dos seus recursos.

As organizações que não monitorizar o desempenho da VM são não é possível determinar se são determinadas alterações nos padrões de desempenho normal ou anormal. Se a VM está a consumir mais recursos do que o normal, uma anomalia tal pode indicar um potencial ataque de um recurso externo ou um processo comprometido em execução na VM.

## <a name="next-steps"></a>Passos Seguintes

* [Blogue da Equipa de Segurança do Azure](https://blogs.msdn.microsoft.com/azuresecurity/)
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)
* [Padrões e práticas recomendadas de segurança do Azure](security-best-practices-and-patterns.md)
