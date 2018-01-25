---
title: "Pontos finais de serviço de rede virtuais e regras para a SQL Database do Azure | Microsoft Docs"
description: "Marcar uma sub-rede como um ponto final de serviço de rede Virtual. Em seguida, o ponto final como uma regra de rede virtual para a ACL de SQL Database do Azure. A base de dados SQL, em seguida, aceita comunicações de todas as máquinas virtuais e outros nós na sub-rede."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: VNet Service endpoints
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 01/23/2018
ms.author: genemi
ms.openlocfilehash: 6294216568e1d4c50ef6e6b6d2348a2a221406b0
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="use-virtual-network-service-endpoints-and-rules-for-azure-sql-database"></a>Utilizar pontos finais do serviço de rede Virtual e as regras para a SQL Database do Azure

*Regras de rede virtual* são uma funcionalidade de segurança de firewall que controla se o servidor da SQL Database do Azure aceita comunicações que são enviadas do sub-redes específicas em redes virtuais. Este artigo explica por que razão a funcionalidade de regra de rede virtual, por vezes, é a melhor opção em segurança permitir comunicações para a base de dados do SQL do Azure.

Para criar uma regra de rede virtual, tem primeiro de existir um [ponto final do serviço de rede virtual] [ vm-virtual-network-service-endpoints-overview-649d] para a regra para efeitos de referência.


> [!NOTE]
> Para a base de dados SQL do Azure, esta funcionalidade está disponível na pré-visualização em todas as regiões de nuvem pública do Azure.

#### <a name="how-to-create-a-virtual-network-rule"></a>Como criar uma regra de rede virtual

Se apenas a criar uma regra de rede virtual, pode avançar diretamente para os passos e uma explicação [posteriormente neste artigo](#anchor-how-to-by-using-firewall-portal-59j).






<a name="anch-terminology-and-description-82f" />

## <a name="terminology-and-description"></a>Terminologia e descrição

**Rede virtual:** pode fazer com que as redes virtuais associadas à subscrição do Azure.

**Sub-rede:** uma rede virtual contém **sub-redes**. Quaisquer máquinas virtuais (VMs) do Azure que tiver são atribuídas a sub-redes. Uma sub-rede pode conter várias VMs ou outros nós de computação. Computação nós que estão fora da rede virtual não é possível aceder à sua rede virtual, exceto se configurar a segurança para permitir o acesso.

**Ponto final de serviço de rede virtual:** A [ponto final do serviço de rede Virtual] [ vm-virtual-network-service-endpoints-overview-649d] uma sub-rede cujos valores de propriedade incluem um ou mais nomes de tipo de serviço do Azure formal. Neste artigo está interessados no nome do tipo de **Microsoft.Sql**, que se refere ao serviço do Azure com o nome da base de dados do SQL Server.

**Regra de rede virtual:** uma regra de rede virtual para o servidor de base de dados do SQL Server é uma sub-rede que está listada na lista de controlo de acesso (ACL) do seu servidor de base de dados SQL. Para ser na ACL para a base de dados do SQL Server, a sub-rede tem de conter o **Microsoft.Sql** nome de tipo.

Uma regra de rede virtual indica que o servidor de base de dados SQL para aceitar comunicações de cada nó que está na sub-rede.







<a name="anch-benefits-of-a-vnet-rule-68b" />

## <a name="benefits-of-a-virtual-network-rule"></a>Vantagens de uma regra de rede virtual

Até que a ação, VMs as sub-redes não consegue comunicar com a base de dados do SQL Server. Uma ação que estabelece a comunicação é a criação de uma regra de rede virtual. A lógica por detrás para escolher a abordagem de regra de VNet requer um debate comparar e contraste que envolve as opções de segurança concorrentes disponibilizadas pela firewall.

#### <a name="a-allow-access-to-azure-services"></a>A. Permitir acesso aos serviços do Azure

O painel de firewall tem um **ON/OFF** botão assinalada como **permitir o acesso aos serviços do Azure**. O **ON** definição permite as comunicações de todos os endereços IP do Azure e todas as sub-redes do Azure. Estes Azure IPs sub-redes poderão não ser detidas por si. Isto **ON** definição é provavelmente mais aberta que pretende que a base de dados do SQL Server para ser. A funcionalidade de regra de rede virtual oferece muito controlo finer granular.

#### <a name="b-ip-rules"></a>B. Regras IP

A firewall de base de dados SQL permite-lhe especificar os intervalos de endereços IP do que as comunicações são aceites na base de dados do SQL Server. Esta abordagem é adequada para estáveis endereços IP que estão fora da rede privada do Azure. Mas muitos nós dentro da rede privada do Azure estão configurados com *dinâmica* endereços IP. Endereços IP dinâmicos podem ser alteradas, por exemplo, quando a VM é reiniciada. Seria folly para especificar um endereço IP dinâmico numa regra de firewall, num ambiente de produção.

Pode salvage a opção de IP por obter um *estático* endereço IP para a VM. Para obter mais informações, consulte [configurar endereços IP privados para uma máquina virtual utilizando o portal do Azure][vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w].

No entanto, a abordagem IP estática pode tornar-se difícil gerir, e é dispendioso quando terminar à escala. Regras de rede virtual são mais fáceis de estabelecer e gerir.

#### <a name="c-cannot-yet-have-sql-database-on-a-subnet"></a>C. Ainda não pode ter a base de dados SQL numa sub-rede

Se o servidor de base de dados do Azure SQL era um nó numa sub-rede na sua rede virtual, todos os nós dentro da rede virtual conseguiu comunicar com a base de dados do SQL Server. Neste caso, as suas VMs conseguiu comunicar com a base de dados SQL sem necessidade de quaisquer regras de rede virtual ou das regras IP.

No entanto a partir de Setembro de 2017, o serviço SQL Database do Azure não está ainda entre os serviços que podem ser atribuídos a uma sub-rede.






<a name="anch-details-about-vnet-rules-38q" />

## <a name="details-about-virtual-network-rules"></a>Detalhes sobre regras de rede virtual

Esta secção descreve vários detalhes sobre regras de rede virtual.

#### <a name="only-one-geographic-region"></a>Apenas uma região geográfica

Cada ponto final de serviço de rede Virtual aplica-se apenas uma região do Azure. O ponto final não ativar a outras regiões aceitar comunicações de sub-rede.

É limitada para a região que o seu ponto final subjacente aplica-se a qualquer regra de rede virtual.

#### <a name="server-level-not-database-level"></a>Ao nível do servidor, não ao nível da base de dados

Cada regra de rede virtual aplica-se ao seu servidor de SQL Database do Azure completa, não apenas para uma determinada base de dados no servidor. Por outras palavras, se aplica a regra de rede virtual ao nível do servidor, não ao nível da base de dados.

- Em contrapartida, as regras IP podem aplicar o nível.

#### <a name="security-administration-roles"></a>Funções de administração de segurança

Há uma separação de funções de segurança na administração de pontos finais do serviço de rede Virtual. Tem de fazer nada cada uma das seguintes funções:

- **Administrador de rede:** &nbsp; ativar o ponto final.
- **Administrador de base de dados:** &nbsp; atualizar a lista de controlo de acesso (ACL) para adicionar a sub-rede especificada para o servidor de base de dados SQL.

*Alternativa RBAC:*

As funções de administrador de rede e administrador de base de dados têm mais capacidades que são necessárias para gerir as regras de rede virtual. É necessário apenas um subconjunto das respetivas capacidades.

Tem a opção de utilizar [controlo de acesso baseado em funções (RBAC)] [ rbac-what-is-813s] no Azure para criar uma função personalizada único que tenha apenas o necessário subconjunto das funcionalidades. A função personalizada pode ser utilizada em vez de envolver o administrador de rede ou o administrador da base de dados. A área de superfície da sua exposição de segurança é inferior se adicionar um utilizador para uma função personalizada, por oposição a adicionar o utilizador para as outras duas funções de administrador principais.

> [!NOTE]
> Em alguns casos SQL Database do Azure e a sub-rede da VNet estão em subscrições diferentes. Nestes casos, tem de garantir as seguintes configurações:
> - Ambas as subscrições tem de estar no mesmo inquilino do Azure Active Directory.
> - O utilizador tem as permissões necessárias para iniciar as operações, tais como pontos finais do serviço de ativação e adicionar uma sub-rede da VNet para o servidor especificado.

## <a name="limitations"></a>Limitações

Para a base de dados SQL do Azure, a funcionalidade de regras de rede virtual tem as seguintes limitações:

- Atualmente, uma aplicação Web do Azure numa sub-rede que tem **pontos finais de serviço** ativada does não ainda funcione como esperado. Estamos a trabalhar para ativar esta funcionalidade.
    - Até que esta funcionalidade é totalmente implementada, recomendamos que mova a aplicação Web para outra sub-rede que não tem pontos finais de serviço ativados no SQL Server.

- Na firewall para a base de dados do SQL Server, cada regra de rede virtual faz referência a uma sub-rede. Estas sub-redes referenciadas tem de estar alojadas na mesma região geográfica que aloja a base de dados do SQL Server.

- Cada servidor da SQL Database do Azure pode ter até 128 entradas ACL para qualquer determinada rede virtual.

- Regras de rede virtual são aplicadas apenas a redes virtuais do Azure Resource Manager; e não a [modelo de implementação clássica] [ arm-deployment-model-568f] redes.

- Pontos finais do serviço de rede virtual no desativar para a SQL Database do Azure também permite que os pontos finais para os serviços MySQL e PostGres Azure. No entanto, com pontos finais no, as tentativas para ligar a partir de pontos finais para as instâncias de MySQL ou Postgres irão falhar.
    - O motivo subjacente é essa MySQL e PostGres não suportam atualmente a ACL da.

- Na firewall, intervalos de endereços IP aplicam-se aos seguintes itens de rede, mas não as regras de rede virtual:
    - [Rede privada virtual (VPN) site a Site (S2S)][vpn-gateway-indexmd-608y]
    - No local através de [ExpressRoute][expressroute-indexmd-744v]

#### <a name="expressroute"></a>ExpressRoute

Se a sua rede é ligado à rede do Azure através da utilização de [ExpressRoute][expressroute-indexmd-744v], cada circuito está configurado com dois endereços IP públicos no Microsoft Edge. Dois endereços IP são utilizados para ligar a Services da Microsoft, tal como para o Storage do Azure, utilizando o Peering público do Azure.

Para permitir a comunicação entre o seu circuito e SQL Database do Azure, tem de criar regras de rede IP para os endereços IP públicos do seu circuitos. Para localizar os endereços IP públicos do seu circuito do ExpressRoute, abra um pedido de suporte com o ExpressRoute, utilizando o portal do Azure.


<!--
FYI: Re ARM, 'Azure Service Management (ASM)' was the old name of 'classic deployment model'.
When searching for blogs about ASM, you probably need to use this old and now-forbidden name.
-->

## <a name="impact-of-removing-allow-all-azure-services"></a>Impacto da remoção "Permitir a todos os serviços do Azure"

Número de utilizadores que pretende remover **permitir todos os serviços do Azure** dos respetivos servidores SQL do Azure e substitua-o com uma regra de Firewall de VNet.
No entanto, a remover Isto afeta as seguintes funcionalidades do Azure SQLDB:

#### <a name="import-export-service"></a>Serviço de exportação de importação
Serviço de exportação de importação de SQLDB do Azure é executada em VMs no Azure. Estas VMs não estão na sua VNet e, por conseguinte, obtém um IP Azure ao ligar à base de dados. No remover **permitir todos os serviços do Azure** estas VMs não será capazes de aceder as bases de dados.
Pode contornar o problema. Executar a importação BACPAC ou exportar diretamente no seu código utilizando a API de DACFx. Certifique-se de que é implementado numa VM que está na sub-rede da VNet para o qual tem de definir a regra de firewall.

#### <a name="sql-database-query-editor"></a>Editor de consultas de base de dados do SQL Server
O Editor de consultas de base de dados do Azure SQL Server é implementado em VMs no Azure. Estas VMs não estão na sua VNet. Por conseguinte, as VMs obtém um IP Azure ao ligar à base de dados. No remover **permitir todos os serviços do Azure**, estas VMs não será capazes de aceder as bases de dados.

#### <a name="table-auditing"></a>Tabela de auditoria
Atualmente, existem duas formas de ativar a auditoria na sua base de dados do SQL Server. Auditoria de tabela falha depois de ter ativado pontos finais de serviço no seu servidor de SQL do Azure. Mitigação aqui é mover para auditoria de Blob.


## <a name="impact-of-using-vnet-service-endpoints-with-azure-storage"></a>Impacto da utilização de pontos finais do serviço de VNet com o storage do Azure

Armazenamento do Azure tiver implementado a mesma funcionalidade que permite limitar a conectividade à sua conta de armazenamento.
Se optar por utilizar esta funcionalidade com uma conta de armazenamento que está a ser utilizada por um servidor de SQL do Azure, pode depare com problemas. Em seguida, é uma lista e debate das funcionalidades do Azure SQLDB que são afetados por este.

#### <a name="azure-sqldw-polybase"></a>Azure SQLDW PolyBase
O PolyBase costuma é utilizado para carregar dados para o Azure SQLDW das contas do Storage. Se a conta de armazenamento que está a carregar dados a partir de limita o acesso apenas a um conjunto de sub-redes da VNet, irá interromper a conectividade do PolyBase à conta.

#### <a name="azure-sqldb-blob-auditing"></a>Blob do Azure SQLDB auditoria
Auditoria de blob pushes registos de auditoria para a sua própria conta de armazenamento. Se esta conta de armazenamento utiliza a funcionalidade de pontos finais do serviço de prevenir irá interromper a conectividade do SQLDB do Azure para a conta de armazenamento.


## <a name="adding-a-vnet-firewall-rule-to-your-server-without-turning-on-vnet-service-endpoints"></a>Adicionar uma regra de Firewall de VNET para o servidor sem desativar na VNET pontos finais de serviço

Há quanto tempo antes desta funcionalidade foi melhorada, eram necessários para ativar a VNet serviço pontos finais em antes de pode implementar uma regra de VNet em direto na Firewall. Os pontos finais relacionados com uma determinada sub-rede de VNet para uma base de dados do SQL do Azure. Mas agora em Janeiro de 2018, pode contornar este requisito, definindo o **IgnoreMissingServiceEndpoint** sinalizador.

Definição simplesmente uma regra de Firewall não ajudar a proteger o servidor. Tem também de ativar pontos finais do serviço de VNet para garantir a segurança tenha efeito. Quando ativar pontos finais de serviço, a sub-rede da VNet ocorrer no período de indisponibilidade até concluir a transição de desativado para no. Isto é particularmente verdadeiro no contexto de grande VNets. Pode utilizar o **IgnoreMissingServiceEndpoint** sinalizador a reduzir ou eliminar o período de indisponibilidade durante a transição.

Pode definir o **IgnoreMissingServiceEndpoint** sinalizador utilizando o PowerShell. Para obter mais informações, consulte [PowerShell para criar um ponto final do serviço de rede Virtual e uma regra para a SQL Database do Azure][sql-db-vnet-service-endpoint-rule-powershell-md-52d].


## <a name="errors-40914-and-40615"></a>Erros 40914 e 40615

Erro de ligação 40914 está relacionada com *regras de rede virtual*, conforme especificado no painel de Firewall no portal do Azure. Erro 40615 é semelhante, exceto se relaciona com *regras de endereços IP* na Firewall.

#### <a name="error-40914"></a>Erro 40914

*Mensagem de texto:* não é possível abrir o servidor '*[nome do servidor]*' pedido pelo início de sessão. Cliente não tem permissão para aceder ao servidor.

*Descrição do erro:* o cliente estiver numa sub-rede que tem pontos finais do servidor de rede virtual. Mas o servidor da SQL Database do Azure não tem nenhuma regra de rede virtual que concede à sub-rede à direita para comunicar com a base de dados do SQL Server.

*Resolução do erro:* painel na Firewall do portal do Azure, utilize as regras de rede virtual controlam ao [adicionar uma regra de rede virtual](#anchor-how-to-by-using-firewall-portal-59j) para a sub-rede.

#### <a name="error-40615"></a>Erro 40615

*Mensagem de texto:* não é possível abrir o servidor '{0}' pedido pelo início de sessão. Cliente com o endereço IP '{1}' não tem permissão para aceder ao servidor.

*Descrição do erro:* o cliente está a tentar ligar a partir de um endereço IP que não está autorizado a ligar ao servidor de SQL Database do Azure. A firewall do servidor não tem nenhuma regra de endereço IP que permite que um cliente comunicar do endereço IP fornecido para a base de dados do SQL Server.

*Resolução do erro:* introduza o endereço IP do cliente como uma regra IP. Fazê-lo utilizando o painel de Firewall no portal do Azure.


Uma lista de várias mensagens de erro de base de dados SQL é documentada [aqui][sql-database-develop-error-messages-419g].





<a name="anchor-how-to-by-using-firewall-portal-59j" />

## <a name="portal-can-create-a-virtual-network-rule"></a>Portal pode criar uma regra de rede virtual

Esta secção ilustra a forma como pode utilizar o [portal do Azure] [ http-azure-portal-link-ref-477t] para criar um *regra de rede virtual* na base de dados SQL do Azure. A regra indica a base de dados do SQL Server para aceitar comunicações de uma sub-rede específica que tenha sido etiquetada como sendo uma *ponto final do serviço de rede Virtual*.

#### <a name="powershell-alternative"></a>PowerShell alternativa

Um script do PowerShell, também pode criar regras de rede virtual. O cmdlet fundamental **New-AzureRmSqlServerVirtualNetworkRule**. Se estiver interessado, consulte [PowerShell para criar um ponto final do serviço de rede Virtual e uma regra para a SQL Database do Azure][sql-db-vnet-service-endpoint-rule-powershell-md-52d].

#### <a name="prerequisites"></a>Pré-requisitos

Já tem de ter uma sub-rede que está marcada com o ponto final de serviço de rede Virtual específico *nome do tipo* relevantes para a SQL Database do Azure.

- É o nome do tipo de ponto final relevante **Microsoft.Sql**.
- Se a sub-rede não pode ser marcada com o nome do tipo, consulte o artigo [Certifique-se de que a sub-rede é um ponto final][sql-db-vnet-service-endpoint-rule-powershell-md-a-verify-subnet-is-endpoint-ps-100].

<a name="a-portal-steps-for-vnet-rule-200" />

### <a name="azure-portal-steps"></a>Passos de portais do Azure

1. Inicie sessão no [portal do Azure][http-azure-portal-link-ref-477t].

2. Em seguida, navegue no portal para **servidores SQL** &gt; **Firewall / redes virtuais**.

3. Definir o **permitir o acesso aos serviços do Azure** controlo como OFF.

    > [!IMPORTANT]
    > Se deixar o controlo definido como ON, o servidor da SQL Database do Azure aceita comunicações de nenhuma sub-rede. Abandonar o fileparser o controlo definido como ON, poderá ser excessivo acesso a partir de um ponto de vista da segurança. Em conjunto, a funcionalidade de ponto final de serviço de rede Virtual do Microsoft Azure, em coordenação com a funcionalidade de regra de rede virtual da base de dados do SQL Server, pode reduzir a área de superfície de segurança.

4. Clique em de **+ adicionar existente** controlar, além de **redes virtuais** secção.

    ![Clique em Adicionar existente (sub-rede ponto final, como uma regra de SQL Server).][image-portal-firewall-vnet-add-existing-10-png]

5. Na nova **Create/Update** painel, preencha os controlos com os nomes dos seus recursos Azure.

    > [!TIP]
    > Tem de incluir o correto **prefixo de endereço** para a sub-rede. Pode encontrar o valor no portal.
    > Navegue **todos os recursos** &gt; **todos os tipos de** &gt; **redes virtuais**. O filtro mostra as redes virtuais. Clique na sua rede virtual e clique **sub-redes**. O **intervalo de endereços** coluna tem o prefixo de endereço precisa.

    ![Preencha os campos para a nova regra.][image-portal-firewall-create-update-vnet-rule-20-png]

6. Clique em de **OK** botão perto da parte inferior do painel.

7.  Consulte a regra de rede virtual resultante no painel de firewall.

    ![Consulte a nova regra, no painel de firewall.][image-portal-firewall-vnet-result-rule-30-png]


> [!NOTE]
> Os seguintes Estados ou Estados aplicam às regras:
> - **Pronto:** indica que a operação que iniciou a é concluída com êxito.
> - **Com falhas:** indica que a operação que iniciou a falhou.
> - **Eliminar:** apenas se aplica a operação de eliminação e indica que a regra foi eliminada e já não se aplica.
> - **Em curso:** indica que a operação está em curso. Se aplica a regra antiga enquanto a operação é neste estado.


<a name="anchor-how-to-links-60h" />

## <a name="related-articles"></a>Artigos relacionados

- [Pontos finais do serviço de rede virtual do Azure][vm-virtual-network-service-endpoints-overview-649d]
- [Regras de firewall ao nível do servidor e o nível de base de dados de base de dados SQL do Azure][sql-db-firewall-rules-config-715d]

A funcionalidade de regra de rede virtual para a SQL Database do Azure está disponível no enlace tardio de Setembro de 2017.

## <a name="next-steps"></a>Passos Seguintes

- [Utilize o PowerShell para criar um ponto final de serviço de rede virtual e, em seguida, uma regra de rede virtual para a SQL Database do Azure.][sql-db-vnet-service-endpoint-rule-powershell-md-52d]


<!-- Link references, to images. -->

[image-portal-firewall-vnet-add-existing-10-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-vnet-add-existing-10.png

[image-portal-firewall-create-update-vnet-rule-20-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-create-update-vnet-rule-20.png

[image-portal-firewall-vnet-result-rule-30-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-vnet-result-rule-30.png



<!-- Link references, to text, Within this same Github repo. -->

[arm-deployment-model-568f]: ../azure-resource-manager/resource-manager-deployment-model.md

[expressroute-indexmd-744v]: ../expressroute/index.md

[rbac-what-is-813s]: ../active-directory/role-based-access-control-what-is.md

[sql-db-firewall-rules-config-715d]: sql-database-firewall-configure.md

[sql-database-develop-error-messages-419g]: sql-database-develop-error-messages.md

[sql-db-vnet-service-endpoint-rule-powershell-md-52d]: sql-database-vnet-service-endpoint-rule-powershell.md

[sql-db-vnet-service-endpoint-rule-powershell-md-a-verify-subnet-is-endpoint-ps-100]: sql-database-vnet-service-endpoint-rule-powershell.md#a-verify-subnet-is-endpoint-ps-100

[vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w]: ../virtual-network/virtual-networks-static-private-ip-arm-pportal.md

[vm-virtual-network-service-endpoints-overview-649d]: https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview

[vpn-gateway-indexmd-608y]: ../vpn-gateway/index.md



<!-- Link references, to text, Outside this Github repo (HTTP). -->

[http-azure-portal-link-ref-477t]: https://portal.azure.com/




<!-- ??2
#### Syntax related articles
- REST API Reference, including JSON

- Azure CLI

- ARM templates
-->
