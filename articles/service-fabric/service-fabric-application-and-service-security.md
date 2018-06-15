---
title: Saiba mais sobre a segurança de aplicações do Azure Service Fabric | Microsoft Docs
description: Uma descrição geral de como em segurança executar aplicações de micro-serviços no Service Fabric. Saiba como executar script de arranque e serviços em contas de segurança diferentes, autenticar e autorizar utilizadores, gerir segredos de aplicações, proteger as comunicações de serviço, utilize um gateway da API e aplicação segura dados inativos.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/16/2018
ms.author: ryanwi
ms.openlocfilehash: fa6d46186ad833b68e60c24f742d210b7845759a
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34207915"
---
# <a name="service-fabric-application-and-service-security"></a>Segurança de serviço e aplicação de Service Fabric
Pode colocar uma arquitetura de micro-serviços [muitas vantagens](service-fabric-overview-microservices.md). Gerir a segurança dos micro-serviços, no entanto, é um desafio e diferente de gerir a segurança de aplicações monolithic tradicional. 

Com um monolith, a aplicação, normalmente, é executada num ou mais servidores dentro de uma rede e é mais fácil identificar as portas expostas e o endereço IP e de APIs. Não há frequentemente um perímetro ou limites e uma base de dados para proteger. Se for comprometido desse sistema devido a uma violação de segurança ou um ataque, é provável que tudo no sistema estará disponível para o atacante. Com micro-serviços, o sistema é mais complexo.  Os serviços são descentralizados e distribuídos em vários anfitriões e migrar a partir do anfitrião.  Com a segurança adequada, limitar os privilégios que um atacante pode obter e a quantidade de dados disponíveis num ataque único por ser, um serviço.  Comunicação não é interna, mas não acontece através de uma rede, e existem muitas expostas portas e as interações entre os serviços. Saber quais são estes interações de serviço e quando ocorrem é fundamental para a segurança da aplicação.

Este artigo não é um guia de segurança de micro-serviços, existem muitas esses recursos disponíveis online, mas descreve como diferentes aspetos de segurança pode ser efetuado no Service Fabric.

## <a name="authentication-and-authorization"></a>Autenticação e autorização
Muitas vezes, é necessário para recursos e APIs expostas através de um serviço para serem limitados à determinados utilizadores fidedignos ou clientes. A autenticação é o processo de forma fiável ascertaining identidade do utilizador.  Autorização é o processo que faz com que as APIs ou serviços disponível para algumas autenticar os utilizadores, mas não a outras pessoas.

### <a name="authentication"></a>Autenticação
O primeiro passo para tomar decisões de fidedignidade ao nível da API é autenticação. A autenticação é o processo de forma fiável ascertaining identidade do utilizador.  Em cenários de microsserviço, a autenticação é normalmente processada centralmente. Se estiver a utilizar um Gateway de API, pode [descarga autenticação](/azure/architecture/patterns/gateway-offloading) ao gateway. Se utilizar esta abordagem, certifique-se de que não não possível aceder os serviços individuais diretamente (sem o Gateway da API), a menos que adicionais de segurança no local para autenticar as mensagens se entram do gateway ou não.

Se os serviços podem ser acedidos diretamente, como um serviço de autenticação do Azure Active Directory ou um microsserviço autenticação dedicado a agir como um serviço de tokens (STS) pode ser utilizado para autenticar os utilizadores de segurança. Decisões de confiança são partilhadas entre os serviços com tokens de segurança ou de cookies. 

Para o ASP.NET Core, o mecanismo de principal para [autenticar utilizadores](/dotnet/standard/microservices-architecture/secure-net-microservices-web-applications/) é o sistema de associação de identidade de núcleo de ASP.NET. Identidade do ASP.NET Core armazena as informações de utilizador (incluindo informações de início de sessão, funções e afirmações) num arquivo de dados configurado pelo programador. Identidade do ASP.NET Core suporta a autenticação de dois fatores.  Também são suportados fornecedores de autenticação externos, pelo que os utilizadores podem iniciar sessão através de processos de autenticação existentes de fornecedores, como a Microsoft, Google, Facebook e Twitter. 

### <a name="authorization"></a>Autorização
Após a autenticação, tem de serviços autorizar o acesso de utilizador ou a determinar que um utilizador é capaz de fazer. Este processo permite que um serviço tornar as APIs disponíveis para alguns utilizadores autenticados, mas não a todos os. Autorização é orthogonal e independente de autenticação, o que é o processo de ascertaining que seja um utilizador. Autenticação, pode criar um ou mais identidades para o utilizador atual.

[Autorização do ASP.NET Core](/dotnet/standard/microservices-architecture/secure-net-microservices-web-applications/authorization-net-microservices-web-applications) pode ser efetuado com base em funções dos utilizadores ou baseado na política personalizada, o que poderá incluir a inspecionar afirmações ou outras heurística.

## <a name="restrict-and-secure-access-using-an-api-gateway"></a>Restringir e proteger o acesso a utilizar um gateway de API
Geralmente, as aplicações da cloud precisam de um gateway de front-end que forneça um único ponto de entrada para utilizadores, dispositivos ou outras aplicações. Um [gateway da API](/azure/architecture/microservices/gateway) encontra-se entre clientes e serviços e é o ponto de entrada para todos os serviços que a aplicação está a fornecer. Atua como um proxy inverso, o encaminhamento de pedidos de clientes aos serviços. -Também pode efetuar várias tarefas de corte em vários locais como a autenticação e autorização, terminação de SSL e limitação de taxa. Se não implementar um gateway, os clientes devem enviar pedidos diretamente aos serviços front-end.

No Service Fabric, um gateway pode ser qualquer serviço sem monitorização de estado, tal como um [aplicação ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md), ou outro serviço concebida para a entrada de tráfego, tais como [Træfik](https://docs.traefik.io/), [Event Hubs](https://docs.microsoft.com/azure/event-hubs/), [IoT Hub](https://docs.microsoft.com/azure/iot-hub/), ou [API Management do Azure](https://docs.microsoft.com/azure/api-management).

Gestão de API integra-se diretamente com o Service Fabric, permitindo-lhe publicar APIs com um vasto conjunto de regras de encaminhamento para os serviços de recursos de infraestrutura do serviço de back-end.  Pode proteger o acesso aos serviços de back-end, impedir ataques DOS através da utilização de limitação ou certifique-se de chaves de API, tokens JWT, certificados e outras credenciais. Para obter mais informações, leia [Service Fabric descrição geral da API Management do Azure](service-fabric-api-management-overview.md).

## <a name="manage-application-secrets"></a>Gerir segredos da aplicação
Os segredos podem ser qualquer informações confidenciais, tais como cadeias de ligação de armazenamento, as palavras-passe ou outros valores que não devem ser processados em texto simples. Este artigo utiliza o Cofre de chaves do Azure para gerir as chaves e segredos. No entanto, *utilizando* segredos numa aplicação está na nuvem plataforma agnóstica para permitir que as aplicações implementadas para um cluster alojado em qualquer lugar.

A forma recomendada para gerir as definições de configuração de serviço é efetuada através de [pacotes de configuração do serviço][config-package]. Pacotes de configuração são com versão e atualizável através de atualizações graduais geridas com a reversão da validação do Estado de funcionamento e o automático. Este é preferido para a configuração global como esta reduz as hipóteses de uma interrupção do serviço global. Os segredos encriptados são sem exceção. Service Fabric tem funções incorporadas para encriptar e desencriptar valores num ficheiro de Settings.xml pacote configuração utilizando a encriptação do certificado.

O diagrama seguinte ilustra o fluxo básico para gestão secreta numa aplicação de Service Fabric:

![Descrição geral da gestão secreta][overview]

Existem quatro passos principais deste fluxo:

1. Obter um certificado de encriptação de dados.
2. Instale o certificado no seu cluster.
3. Encriptar o segredo valores quando implementar uma aplicação com o certificado e inseri-los no ficheiro de configuração de Settings.xml de um serviço.
4. Leitura de valores encriptados Settings.xml ao desencriptar com o mesmo certificado de encriptação. 

[O Cofre de chaves do Azure] [ key-vault-get-started] é aqui utilizada como uma localização de armazenamento seguro para certificados e como uma forma de obter certificados instalados em clusters de Service Fabric no Azure. Se não estiver a implementar no Azure, não terá de utilizar o Cofre de chaves para gerir os segredos em aplicações de Service Fabric.

Por exemplo, consulte [gerir segredos de aplicação](service-fabric-application-secret-management.md).

## <a name="secure-the-hosting-environment"></a>Proteger o ambiente de alojamento
Ao utilizar o Service Fabric do Azure, pode proteger aplicações em execução no cluster em contas de utilizador diferente. Serviço de recursos de infraestrutura também ajuda a proteger os recursos utilizados por aplicações no momento da implementação sob as contas de utilizador - por exemplo, ficheiros, diretórios e certificados. Isto faz com que aplicações em execução, mesmo num ambiente alojado partilhado, mais segura entre si.

O manifesto da aplicação declara os princípios de segurança (utilizadores e grupos) necessários executam os serviços e recursos segurados.  Estes princípios são referenciados nas políticas, por exemplo o executar como, ponto final de enlace, de segurança do pacote de partilha ou políticas de acesso de segurança.  As políticas, em seguida, são aplicadas aos recursos de serviço no **ServiceManifestImport** secção de manifesto da aplicação.

Quando declarar principais, também pode definir e criar grupos de utilizadores que podem ser adicionados um ou mais utilizadores para cada grupo de ser geridos em conjunto. Isto é útil quando existem vários utilizadores para pontos de entrada de serviço diferente e que precisam para tem determinados privilégios comuns que estão disponíveis ao nível do grupo.

Por predefinição, as aplicações de Service Fabric executam sob a conta que executa o processo de Fabric.exe em. Serviço de recursos de infraestrutura também fornece a capacidade para executar aplicações com uma conta de utilizador local ou a conta de sistema local, o que é especificada no manifesto da aplicação. Para obter mais informações, consulte [executar um serviço como uma conta de utilizador local ou como conta de sistema local](service-fabric-application-runas-security.md).  Também pode [executar um script de arranque do serviço como uma conta de utilizador ou sistema local](service-fabric-run-script-at-service-startup.md).

Quando estiver a executar o Service Fabric num cluster de autónomo do Windows, pode executar um serviço em [contas de domínio do Active Directory](service-fabric-run-service-as-ad-user-or-group.md) ou [contas de serviço geridas de grupo](service-fabric-run-service-as-gmsa.md).

## <a name="secure-containers"></a>Proteger contentores
O Service Fabric fornece um mecanismo para serviços no interior de um contentor para aceder a um certificado que está instalado em nós num cluster do Windows ou Linux (versão 5.7 ou superior). Este certificado PFX pode ser utilizado para autenticar a aplicação ou serviço ou uma comunicação segura com outros serviços. Para obter mais informações, consulte [importar um certificado para um contentor](service-fabric-securing-containers.md).

Além disso, a Service Fabric também suporta a gMSA (grupo contas de serviço geridas) para contentores do Windows. Para obter mais informações, consulte [configurar gMSA para contentores de Windows](service-fabric-setup-gmsa-for-windows-containers.md).

## <a name="secure-service-communication"></a>Proteger a comunicação de serviço
No Service Fabric, um serviço é executado algures num cluster de Service Fabric, normalmente distribuído por várias VMs. O Service Fabric fornece várias opções para proteger as suas comunicações de serviço.

Pode ativar pontos finais HTTPS no seu [ASP.NET Core ou Java](service-fabric-service-manifest-resources.md#example-specifying-an-https-endpoint-for-your-service) serviços web.

Pode estabelecer uma ligação segura entre o proxy inverso e serviços, permitindo um canal seguro de ponto a ponto. A ligar aos serviços segurados só é suportada quando proxy inverso estiver configurado para escutar HTTPS. Para informações sobre como configurar o proxy inverso, leia o artigo [proxy no Azure Service Fabric inverso](service-fabric-reverseproxy.md).  [Ligar a um serviço seguro](service-fabric-reverseproxy-configure-secure-communication.md) descreve como estabelecer uma ligação segura entre os serviços e proxy inverso.

A estrutura da aplicação Reliable Services fornece alguns pilhas de comunicação prebuilt e ferramentas que pode utilizar para melhorar a segurança. Saiba como melhorar a segurança quando estiver a utilizar a comunicação remota do serviço (no [c#](service-fabric-reliable-services-secure-communication.md) ou [Java](service-fabric-reliable-services-secure-communication-java.md)) ou utilizar [WCF](service-fabric-reliable-services-secure-communication-wcf.md).

## <a name="encrypt-application-data-at-rest"></a>Encriptar dados da aplicação Inativos
Cada [tipo de nó](service-fabric-cluster-nodetypes.md) num cluster de Service Fabric em execução no Azure é copiado por um [conjunto de dimensionamento da máquina virtual](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). Ao utilizar um modelo do Azure Resource Manager, pode anexar discos de dados aos conjuntos de dimensionamento que compõem o cluster do Service Fabric.  Se os serviços de guardam os dados para um disco de dados ligados, pode [encriptar destes discos de dados](../virtual-machine-scale-sets/virtual-machine-scale-sets-encrypt-disks-ps.md) para proteger os dados de aplicação.

<!--TO DO: Enable BitLocker on Windows standalone clusters?
TO DO: Encrypt disks on Linux clusters?-->


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Passos Seguintes
* [Executar um script de configuração no arranque do serviço](service-fabric-run-script-at-service-startup.md)
* [Especificar recursos num manifesto de serviço](service-fabric-service-manifest-resources.md)
* [Implementar uma aplicação](service-fabric-deploy-remove-applications.md)
* [Saiba mais sobre a segurança do cluster](service-fabric-cluster-security.md)

<!-- Links -->
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[config-package]: service-fabric-application-and-service-manifests.md
[service-fabric-cluster-creation-via-arm]: service-fabric-cluster-creation-via-arm.md

<!-- Images -->
[overview]:./media/service-fabric-application-and-service-security/overview.png