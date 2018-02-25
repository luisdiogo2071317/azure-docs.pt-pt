---
title: "Replicar uma multicamadas de aplicação web baseada em IIS utilizando o Azure Site Recovery | Microsoft Docs"
description: "Saiba como replicar máquinas virtuais IIS web farm utilizando o Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2018
ms.author: nisoneji
ms.openlocfilehash: a4a8ea14fecac73b187c9c7d3f9ca318bb2671c5
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/24/2018
---
# <a name="replicate-a-multi-tier-iis-based-web-application-by-using-site-recovery"></a>Replicar uma aplicação web de várias camadas, com base no IIS, utilizando a recuperação de sites

Software de aplicação é o motor de produtividade da empresa numa organização. Várias aplicações web podem servir diferentes fins numa organização. Algumas aplicações, como as aplicações utilizadas para processamento de folha de pagamentos, aplicações financeiras e direcionadas para o cliente de Web sites, podem ser fundamental para uma organização. Para evitar a perda de produtividade, é importante para a organização para que estas aplicações continuamente e funcional. Mais importante ainda, estas aplicações consistentemente disponíveis podem ajudar a evitar danos para a imagem da organização ou marca.

As aplicações web críticos normalmente estão definidas como aplicações de várias camadas: web, base de dados e aplicações estão em diferentes camadas. Para além das que está a ser distribuídos por várias camadas, as aplicações podem também utilizar vários servidores em cada camada para equilibrar o tráfego. Além disso, os mapeamentos entre várias camadas e no servidor web podem ser baseados em endereços IP estáticos. Na ativação pós-falha, alguns destes mapeamentos tem de ser atualizado, especialmente se vários sites são configurados no servidor web. Se as aplicações web utilizam SSL, tem de atualizar enlaces de certificado.

Métodos tradicionais de recuperação que não são baseados na replicação envolvem a cópia de segurança de vários ficheiros de configuração, definições de registo, enlaces, componentes personalizados (COM ou .NET), conteúdo e certificados. Os ficheiros são recuperados através de um conjunto de passos manuais. Os métodos tradicionais de recuperação de cópia de segurança e recuperar manualmente os ficheiros são complexa, propensas ao erro e não dimensionável. Por exemplo, pode facilmente se esqueça de cópia de segurança de certificados. Após a ativação pós-falha, está a esquerda com a opção de não mas comprar novos certificados para o servidor.

Uma solução de recuperação após desastre boa suporta a recuperação de modelação planos de arquiteturas de aplicações complexas. Deve também conseguir adicionar passos personalizados para o plano de recuperação para processar os mapeamentos de aplicação entre camadas. Se ocorrer um desastre, mapeamentos de aplicação fornecem uma solução de clique único, captura se de que o ajuda a levar a uma RTO inferior.

Este artigo descreve como proteger uma aplicação web com base nos serviços de informação Internet (IIS), utilizando [do Azure Site Recovery](site-recovery-overview.md). O artigo aborda as melhores práticas para replicar uma aplicação web de três camadas, com base no IIS para o Azure, como fazê-lo um exercício de recuperação após desastre e como efetuar a ativação pós-falha a aplicação no Azure.

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, certifique-se de que sabe como efetuar as seguintes tarefas:

* [Replicar uma máquina virtual para o Azure](site-recovery-vmware-to-azure.md)
* [Estruturar uma rede de recuperação](site-recovery-network-design.md)
* [Efetue uma ativação pós-falha de teste para o Azure](site-recovery-test-failover-to-azure.md)
* [Efetue uma ativação pós-falha para o Azure](site-recovery-failover.md)
* [Replicar um controlador de domínio](site-recovery-active-directory.md)
* [Replicação do SQL Server](site-recovery-sql.md)

## <a name="deployment-patterns"></a>Padrões de implementação
Uma aplicação web baseada em IIS normalmente segue-se um dos padrões de implementação seguintes:

**Padrão de implementação 1**

Um farm de web IIS com base com Application Request Routing (ARR), um servidor IIS e SQL Server.

![Diagrama de um farm de web IIS com base que tenha três camadas](./media/site-recovery-iis/deployment-pattern1.png)

**Padrão de implementação 2**

Um farm web baseado no IIS com o ARR, um servidor IIS, um servidor de aplicações e do SQL Server.

![Diagrama de um farm de web IIS com base tem quatro camadas](./media/site-recovery-iis/deployment-pattern2.png)

## <a name="site-recovery-support"></a>Suporte de recuperação de site

Para os exemplos neste artigo, vamos utilizar máquinas virtuais VMware com o IIS 7.5 no Windows Server 2012 R2 Enterprise. Porque a replicação do Site Recovery não está específicas da aplicação, as recomendações neste artigo são esperadas para aplicar nos cenários listados na seguinte tabela e para versões diferentes do IIS.

### <a name="source-and-target"></a>A origem e destino

Cenário | Para um site secundário | Para o Azure
--- | --- | ---
Hyper-V | Sim | Sim
VMware | Sim | Sim
Servidor físico | Não | Sim
Azure|ND|Sim

## <a name="replicate-virtual-machines"></a>Replicar máquinas virtuais

Para iniciar a replicação de todas as IIS web farm máquinas virtuais no Azure, siga as orientações no [ativação pós-falha de teste para o Azure no Site Recovery](site-recovery-test-failover-to-azure.md).

Se estiver a utilizar um endereço IP estático, pode especificar o endereço IP que pretende que a máquina virtual a tomar. Para definir o endereço IP, aceda a **computação e rede definições** > [**IP de destino**](./site-recovery-replicate-vmware-to-azure.md#view-and-manage-vm-properties).

![Captura de ecrã que mostra como definir o IP de destino no painel de computação de recuperação de Site e da rede](./media/site-recovery-active-directory/dns-target-ip.png)

## <a name="create-a-recovery-plan"></a>Criar um plano de recuperação
Um plano de recuperação suporta a sequenciação de várias camadas numa aplicação multicamada durante uma ativação pós-falha. Sequenciação ajuda a manter a consistência da aplicação. Quando cria um plano de recuperação para uma aplicação web de várias camadas, concluir os passos descritos na [criar um plano de recuperação utilizando a recuperação de Site](site-recovery-create-recovery-plans.md).

### <a name="add-virtual-machines-to-failover-groups"></a>Adicionar máquinas virtuais para grupos de ativação pós-falha
Uma aplicação web do IIS de várias camada típica é composta pelos seguintes componentes:
* Uma camada de base de dados que tenha as máquinas virtuais do SQL Server.
* A camada web, que é composta por um servidor IIS e uma camada de aplicação. 

Adicione máquinas virtuais para diferentes grupos com base na camada de:

1. Crie um plano de recuperação. Adicione as máquinas virtuais de camada de base de dados em grupo 1. Isto garante que as máquinas de virtuais de camada de base de dados são encerrar último e produzidas primeiro.
1. Adicione as máquinas de virtuais de camada de aplicação de grupo 2. Isto garante que as máquinas de virtuais de camada de aplicação são produzidas depois da camada de base de dados foram produzida.
1. Adicione as máquinas de virtuais de camada web no grupo 3. Isto garante que as máquinas de virtuais de camada web são produzidas depois da camada de aplicação foram produzida.
1. Adicione máquinas de virtuais de balanceamento de carga no grupo 4. Isto garante que as máquinas de virtuais de balanceamento de carga são produzidas depois da camada web foram produzida.

Para obter mais informações, consulte [personalizar o plano de recuperação](site-recovery-runbook-automation.md#customize-the-recovery-plan).


### <a name="add-a-script-to-the-recovery-plan"></a>Adicionar um script para o plano de recuperação
Para o web farm do IIS funcione corretamente, poderá ter de fazer algumas operações de pós-falha de máquinas virtuais do Azure ou durante uma ativação pós-falha de teste. Pode automatizar algumas operações de pós-falha. Por exemplo, pode atualizar a entrada DNS, alterar um enlace de site ou alterar uma cadeia de ligação, adicionando scripts correspondentes para o plano de recuperação. [Adicionar um script do VMM para um plano de recuperação](site-recovery-how-to-add-vmmscript.md) descreve como configurar tarefas automatizadas utilizando um script.

#### <a name="dns-update"></a>Atualização de DNS
Se o DNS está configurado para a atualização dinâmicas DNS, máquinas virtuais atualizar, normalmente, o DNS com o novo endereço IP, quando são iniciados. Se pretender adicionar um passo explícito para atualizar o DNS com os novos endereços IP das máquinas virtuais, adicione um [script para atualizar o IP no DNS](https://aka.ms/asr-dns-update) como uma ação de pós-falha nos grupos do plano de recuperação.  

#### <a name="connection-string-in-an-applications-webconfig"></a>Cadeia de ligação em Web. config de uma aplicação
A cadeia de ligação Especifica a base de dados que o Web site comunica com. Se a cadeia de ligação acarreta o nome da máquina virtual da base de dados, não existem passos adicionais são pós-falha necessária. A aplicação automaticamente pode comunicar com a base de dados. Além disso, se o endereço IP para a máquina virtual de base de dados é mantido, não ser necessário atualizar a cadeia de ligação. 

Se a cadeia de ligação refere-se a máquina virtual de base de dados utilizando um endereço IP, tem de ser atualizada pós-falha. Por exemplo, os seguintes pontos de cadeia de ligação à base de dados com o IP endereços 127.0.1.2:

        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
        <connectionStrings>
        <add name="ConnStringDb1" connectionString="Data Source= 127.0.1.2\SqlExpress; Initial Catalog=TestDB1;Integrated Security=False;" />
        </connectionStrings>
        </configuration>

Para atualizar a cadeia de ligação na camada web, adicione um [script de atualização de ligação de IIS](https://aka.ms/asr-update-webtier-script-classic) após grupo 3 no plano de recuperação.

#### <a name="site-bindings-for-the-application"></a>Enlaces de site para a aplicação
Cada site é composta por informações de enlace. As informações de enlace incluem o tipo de enlace, o endereço IP no qual o servidor IIS escuta os pedidos para o site, o número de porta e os nomes de anfitrião para o site. Durante a ativação pós-falha, poderá ter de atualizar estes enlaces se houver uma alteração no endereço IP que tiver associado aos mesmos.

> [!NOTE]
>
> Se definir o enlace de site para **todos os não atribuídos**, não precisa de atualizar este enlace post-ativação pós-falha. Além disso, se o endereço IP associado um site não estiver pós-falha alterada, não precisa de atualizar o enlace de site. (A retenção do endereço IP depende da arquitetura de rede e sub-redes atribuídas aos sites primário e de recuperação. Atualizá-las poderá não estar viável para a sua organização.)

![Captura de ecrã que mostra a definição o enlace SSL](./media/site-recovery-iis/sslbinding.png)

Se o endereço IP que tenha associado um site, atualize todos os enlaces de site com o novo endereço IP. Para alterar os enlaces de site, adicione um [script de atualização de escalão de web IIS](https://aka.ms/asr-web-tier-update-runbook-classic) após grupo 3 no plano de recuperação.

#### <a name="update-the-load-balancer-ip-address"></a>Atualizar o endereço IP do Balanceador de carga
Se tiver uma máquina virtual do ARR, para atualizar o endereço IP, adicionar um [script de ativação pós-falha do IIS ARR](https://aka.ms/asr-iis-arrtier-failover-script-classic) após as 4 de grupo.

#### <a name="ssl-certificate-binding-for-an-https-connection"></a>Enlace do certificado SSL para uma ligação HTTPS
Um Web site poderá ter um certificado SSL associado que ajuda a garantir uma comunicação segura entre o servidor web e o browser do utilizador. Se o Web site tiver uma ligação HTTPS e também tem um enlace de site HTTPS associado, para o endereço IP do servidor de IIS com um enlace de certificado SSL, tem de adicionar um novo enlace de site para o certificado com o endereço IP do post de máquina virtual a IIS-ativação pós-falha.

O certificado SSL pode ser emitido contra estes componentes:

* O nome de domínio completamente qualificado do Web site.
* O nome do servidor.
* Um certificado de caráter universal para o nome de domínio.  
* Um endereço IP. Se o certificado SSL é emitido contra o endereço IP do servidor de IIS, outro certificado SSL tem de ser emitido contra o endereço IP do servidor do IIS no site do Azure. Um enlace SSL adicional para este certificado tem de ser criado. Por este motivo, recomendamos que não utilize um certificado SSL emitido contra o endereço IP. Esta opção é menos amplamente utilizada e logo que vai ser preterida em conformidade com as novas alterações de fórum de autoridade/browser de certificado.

#### <a name="update-the-dependency-between-the-web-tier-and-the-application-tier"></a>Atualizar a dependência entre a camada web e a camada de aplicação
Se tiver uma dependência de aplicação específicos com base no endereço IP de máquinas virtuais, tem de atualizar este pós-falha dependência.

## <a name="run-a-test-failover"></a>Executar uma ativação pós-falha de teste

1. No portal do Azure, selecione o Cofre dos serviços de recuperação.
2. Selecione o plano de recuperação que criou para o web farm do IIS.
3. Selecione **ativação pós-falha de teste**.
4. Para iniciar o processo de ativação pós-falha de teste, selecione o ponto de recuperação e a rede virtual do Azure.
5. Quando o ambiente secundário, pode realizar validações.
6. Quando validações estiverem concluídas, para limpar o ambiente de ativação pós-falha de teste, selecione **validações concluir**.

Para obter mais informações, consulte [ativação pós-falha de teste para o Azure no Site Recovery](site-recovery-test-failover-to-azure.md).

## <a name="run-a-failover"></a>Executar uma ativação pós-falha

1. No portal do Azure, selecione o Cofre dos serviços de recuperação.
1. Selecione o plano de recuperação que criou para o web farm do IIS.
1. Selecione **ativação pós-falha**.
1. Para iniciar o processo de ativação pós-falha, selecione o ponto de recuperação.

Para obter mais informações, consulte [ativação pós-falha na recuperação de Site](site-recovery-failover.md).

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre [replicar outras aplicações](site-recovery-workload.md) utilizando a recuperação de Site.
