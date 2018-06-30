---
title: Governação no Azure | Microsoft Docs
description: Saiba mais sobre serviços informáticos baseado na nuvem que podem aumentar e reduzir verticalmente para satisfazer as necessidades da sua aplicação ou a empresa.
services: security
documentationcenter: na
author: UnifyCloud
manager: mbaldwin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2017
ms.author: TomSh
ms.openlocfilehash: 9c6509f25be7fe520a427e17ca1206e10f296fea
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110767"
---
# <a name="governance-in-azure"></a>Governação no Azure

Azure dá-lhe muitas opções de segurança e a capacidade de controlá-las de modo a que pode satisfazer os requisitos exclusivos de implementações da sua organização.

Governação de nuvem do Azure refere-se a processos de tomada de decisão, os critérios e envolvidos no planeamento de políticas, arquitetura, aquisição, implementação, operação e gestão de informática em nuvem. Governação de nuvem do Azure fornece uma auditoria integrada e uma abordagem de consultoria para rever e indicar organizações na respetiva utilização de plataforma do Azure. 

Para criar um plano de governação de nuvem do Azure, terá de tomar uma visão detalhada das pessoas, processos e as tecnologias agora no local. Em seguida, pode criar estruturas que tornam mais fácil para que as TI consistentemente suportar necessidades ao fornecer aos utilizadores a flexibilidade para utilizar as funcionalidades do Azure.

## <a name="implementation-of-policies-processes-and-standards"></a>Implementação de políticas, processos e normas 

Gestão estabelecida funções e responsabilidades para supervisionam implementação de políticas de segurança de informações e continuidade operacional em todo o Azure. Gestão do Azure é responsável por supervisionar segurança e de práticas de continuidade dentro do respetivas respetivas equipas (incluindo terceiros). -Também facilita a conformidade com as normas, processos e políticas de segurança.

### <a name="account-provisioning"></a>Aprovisionamento de contas

Definir a hierarquia de conta é um passo principais para utilizar e serviços do Azure dentro de uma empresa de estrutura. É a estrutura de governação core. Os clientes que têm um Enterprise Agreement (EA) podem subdividir o ambiente para departamentos, contas e subscrições.

![Aprovisionamento de contas](./media/governance-in-azure/security-governance-in-azure-fig1.png)

Se não tiver um Enterprise Agreement, considere utilizar [etiquetas do Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) ao nível da subscrição para definir a hierarquia. Uma subscrição do Azure é a unidade básica que contém todos os recursos. Também define os limites de vários no Azure, como o número de núcleos e de recursos. Subscrições podem conter [grupos de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), que pode conter recursos. [O controlo de acesso baseado em funções (RBAC)](https://docs.microsoft.com/en-us/azure/role-based-access-control/overview) princípios de aplicam a essas três níveis.

Cada empresa é diferente. Para empresas não empresariais, a hierarquia da utilização de etiquetas do Azure permite uma flexibilidade na forma como o Azure está organizado. Antes de implementar recursos no Azure, deve modelar uma hierarquia e compreender o impacto sobre faturação, acesso a recursos e complexidade.

### <a name="subscription-controls"></a>Controlos de subscrição

Subscrições de determinam como a utilização de recursos é comunicada e cobrada. Pode configurar subscrições para separado de faturação e pagamento. Uma conta do Azure pode ter várias subscrições. Subscrições podem ser utilizadas para determinar a utilização de recursos do Azure de vários departamentos numa empresa.

Por exemplo, uma empresa tem departamento de TI HR, e departamentos de Marketing e estes departamentos são executados projetos diferentes. A empresa pode baseá a faturação na utilização de cada departamento de recursos do Azure, como máquinas virtuais. A empresa, em seguida, pode controlar as finanças de cada departamento.

As subscrições do Azure estabelecer três parâmetros:

- ID exclusivo de subscritor

- Localização de faturação

- Conjunto de recursos disponíveis

Para um indivíduo, esses parâmetros incluem um ID da conta Microsoft, um número de cartão de crédito e o conjunto completo de serviços do Azure. Microsoft impõe limites de consumo, dependendo do tipo de subscrição.

Hierarquias de inscrição do Azure definem como os serviços estão estruturados dentro de um contrato Enterprise. Portal do Enterprise Agreement permite aos clientes dividir o acesso aos recursos do Azure associada com um Enterprise Agreement, com base nas hierarquias flexíveis que são personalizáveis para as necessidades da organização. O padrão de hierarquia deve corresponder à gestão da organização e a estrutura geográfica para a conta para o acesso de faturação e de recursos associado.

Os três padrões de alto nível da hierarquia são unidade funcional, empresariais e geográfica. Departamentos são uma construção administrativa para agrupamentos de conta. Dentro de cada departamento, as contas podem ser atribuídas subscrições, que crie silos de faturação e limites de chaves várias no Azure (por exemplo, o número de VMs e as contas de armazenamento).

![Controlos de subscrição](./media/governance-in-azure/security-governance-in-azure-fig2.png)


Para organizações com um Enterprise Agreement, as subscrições do Azure, siga uma hierarquia de nível de quatro:

1. administrador de inscrição Enterprise

2. administrador do departamento

3. proprietário da conta

4. Administrador de serviço

Esta hierarquia é regida pelas seguintes:

- Relação de faturação.

- Administração de conta.

- Acesso a recursos através do RBAC.

- Limites:

  - Utilização e faturação (com base em números de oferta cartão de taxas)

  - Limites

  - Rede virtual

- Anexo para o Azure Active Directory (Azure AD). Uma instância do Azure AD pode ser associada com várias subscrições.

- Associação com uma conta de inscrição empresarial.

### <a name="role-based-access-control"></a>Controlo de acesso baseado em funções

[RBAC](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) permite a gestão de acesso detalhada de recursos no Azure. Ao utilizar o RBAC, pode conceder apenas a quantidade de acesso que os utilizadores precisam desempenhar as suas funções. As empresas devem focar-se em fornecer aos funcionários as permissões exatas que precisam. Demasiados permissões expõem uma conta para os atacantes. Permissões insuficientes significam que os funcionários não é possível obter o trabalho feito de forma eficiente. 

Em vez de dar everybody sem restrições permissões na sua subscrição do Azure ou recursos, pode permitir que apenas determinadas ações. Por exemplo, pode utilizar o RBAC para permitir que um empregado gerir máquinas virtuais numa subscrição, enquanto outros empregados gere as bases de dados do SQL Server na mesma subscrição.

Para conceder acesso, atribuir funções de utilizadores, grupos ou aplicações num determinado âmbito. O âmbito de uma atribuição de função pode ser uma subscrição, um grupo de recursos ou um único recurso. Uma função atribuída a um âmbito principal também concede acesso a subordinados contidos. Por exemplo, um utilizador com acesso a um grupo de recursos pode gerir todos os recursos que contém, como Web sites, sub-redes e máquinas virtuais. Dentro de cada subscrição, pode criar até 2000 atribuições de funções.

Uma função é uma coleção de permissões e RBAC tem várias funções incorporadas. As seguintes funções incorporadas que se aplicam a todos os tipos de recursos:

- **Proprietário** tem acesso total a todos os recursos, incluindo o direito para delegar o acesso a outras pessoas.

- **Contribuidor** pode criar e gerir todos os tipos de recursos do Azure, mas não é possível conceder acesso a outras pessoas.

- **Leitor** pode ver todos os recursos do Azure.

O resto das funções incorporadas do Azure permite a gestão de recursos do Azure específicos. Por exemplo, a função de contribuinte de Máquina Virtual permite ao utilizador criar e gerir máquinas virtuais. Para obter uma lista de todas as funções incorporadas e as respetivas operações, consulte [funções incorporadas do RBAC](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

RBAC suporta operações de gestão de recursos do Azure no portal do Azure e APIs do Azure Resource Manager. Na maioria dos casos, o RBAC não é possível autorizar as operações de nível de dados para recursos do Azure. Para obter informações sobre como o RBAC está a ser expandida para as operações de dados, consulte [compreender as definições de função](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-definitions).

Se as funções incorporadas não satisfazer as suas necessidades de acesso específicas, pode [criar uma função personalizada](https://docs.microsoft.com/azure/role-based-access-control/custom-roles). Tal como funções incorporadas, funções personalizadas podem ser atribuídas a utilizadores, grupos e aplicações, a subscrição, o grupo de recursos e o âmbito de recursos. Pode criar funções personalizadas utilizando [Azure PowerShell](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell), [CLI do Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli)e o [REST API](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest).

### <a name="resource-management"></a>Gestão de recursos

O Azure oferece dois modelos de implementação: [clássico](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) e do Azure Resource Manager.

O modelo clássico, cada recurso existe independentemente. Não é possível agrupar recursos relacionados. Tem de controlar manualmente os recursos que compõem a sua solução ou a aplicação e não se esqueça de geri-los numa abordagem coordenada. A unidade básica de gestão é a subscrição. É difícil de dividir recursos dentro de uma subscrição, o que conduz a criação de grandes quantidades de subscrições.

O modelo de implementação Resource Manager inclui o conceito de uma [grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). Os grupos de recursos são um contentor para os recursos que partilham um ciclo de vida comum. Pode incluir todos os recursos para a solução, ou apenas os recursos que pretende gerir como um grupo. Decida como pretende atribuir recursos a grupos de recursos com base no que é mais adequado para a sua organização.

O modelo de implementação Resource Manager oferece várias vantagens:

- Pode implementar, gerir e monitorizar todos os serviços da sua solução como um grupo, em vez de os processar individualmente.

- Pode implementar a solução durante todo o ciclo de vida repetidamente e ter a confiança de que os recursos são implementados num estado consistente.

- Pode aplicar o controlo de acesso a todos os recursos no seu grupo de recursos. Essas políticas são aplicadas automaticamente quando são adicionados novos recursos ao grupo de recursos.

- Pode aplicar etiquetas a recursos para organizar logicamente todos os recursos na sua subscrição.

- Pode utilizar o JavaScript Object Notation (JSON) para definir a infraestrutura da sua solução. O ficheiro JSON é conhecido como modelo do Resource Manager.

- Pode definir as dependências entre os recursos, de modo que está a ser implementados na ordem correta.

![Resource Manager](./media/governance-in-azure/security-governance-in-azure-fig4.png)

Para obter recomendações sobre modelos, veja o artigo [Melhores práticas para criar modelos do Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices).

O Azure Resource Manager analisa as dependências para o ajudar a garantir que os recursos são criados na ordem correta. Se um recurso depende de um valor de outro recurso (por exemplo, uma máquina virtual necessitar de uma conta de armazenamento para discos), [definir uma dependência](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-define-dependencies) no modelo.

Também pode utilizar o modelo para atualizar a infraestrutura. Por exemplo, pode adicionar um recurso à solução e adicionar regras de configuração para os recursos que já estão implementados. Se o modelo especificar a criação de um recurso, mas esse recurso já existir, o Resource Manager efetua uma atualização em vez de criar um novo recurso. O Resource Manager atualiza o recurso existente para o mesmo Estado que seria como novo.

O Resource Manager fornece extensões para cenários quando precisar de mais operações, como a instalação de software que não está incluído na configuração.

### <a name="resource-tracking"></a>Controlo de recursos

Como os utilizadores na sua organização adicionam recursos à subscrição, torna-se mais importante para associar a recursos com o departamento apropriado, o cliente e o ambiente. Pode anexar os metadados a recursos através do [etiquetas](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags). Utilize etiquetas para fornecer informações sobre o recurso ou o proprietário. As etiquetas permitem não só agregar e grupo de recursos de várias formas, mas também utilizar esses dados para fins de estorno.

Utilizar etiquetas quando tem uma coleção complexa de grupos de recursos e recursos e precisam de visualizar esses elementos da forma que mais adequada para si. Por exemplo, pode Etiquetar recursos que desempenham uma função semelhante na sua organização ou que pertençam ao mesmo departamento.

Sem etiquetas, os utilizadores na sua organização podem criar vários recursos que poderão ser difíceis de identificar e gerir a mais tarde. Por exemplo, pode querer eliminar todos os recursos para um projeto. Se esses recursos não são etiquetados para o projeto, tem de encontrá-los manualmente. A etiquetagem pode ser um meio importante para reduzir os custos desnecessários associados à sua subscrição.

Recursos não precisam de residir no mesmo grupo de recursos para partilhar uma etiqueta. Pode criar a sua própria taxonomia de etiquetas para se certificar de que todos os utilizadores na sua organização utilizam etiquetas comuns em vez de aplicar inadvertidamente etiquetas ligeiramente diferentes (por exemplo, "Depart" em vez de "departamento").

As políticas de recursos permitem-lhe criar regras padrão para a sua organização. Pode criar políticas para garantir que os recursos são etiquetados com os valores adequados.

Também pode ver recursos com etiquetas através do Portal do Azure. O [relatório de utilização](https://docs.microsoft.com/azure/billing/billing-understand-your-bill) para a sua subscrição inclui valores e nomes de etiqueta, por isso, pode dividir os custos por etiquetas.

Para obter mais informações sobre etiquetas, consulte [iniciativa de política de etiquetas de faturação](../azure-policy/scripts/billing-tags-policy-init.md).

As seguintes limitações aplicam-se às etiquetas:

- Cada recurso ou grupo de recursos pode ter um máximo de 15 pares de chave/valor de etiqueta. Esta limitação aplica-se apenas a etiquetas diretamente aplicadas ao recurso ou grupo de recursos. Um grupo de recursos pode conter muitos recursos com 15 pares de chave/valor de etiqueta.

- O nome da etiqueta está limitado a 512 carateres.

- O valor da etiqueta está limitado a 556 carateres.

- As etiquetas aplicadas ao grupo de recursos não são herdadas pelos recursos nesse grupo de recursos.

Se tiver mais de 15 valores que têm de ser associados a um recurso, utilize uma cadeia JSON para o valor da etiqueta. A cadeia JSON pode conter muitos valores que são aplicados a uma chave de etiqueta única.

#### <a name="tags-for-billing"></a>Etiquetas para faturação

As etiquetas permitem-lhe agrupar os dados de faturação. Por exemplo, se estiver a executar várias VMs para diferentes organizações, utilize etiquetas para utilização do grupo pelo centro de custos. Também pode utilizar etiquetas para categorizar os custos pelo ambiente de tempo de execução, tais como a utilização de faturação para VMs em execução no ambiente de produção.

Pode obter informações sobre etiquetas através de [utilização de recursos do Azure e RateCard APIs](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview) ou o ficheiro de valores separados por vírgulas (CSV) de utilização. Transferir o ficheiro de utilização do [portal de contas do Azure](https://account.windowsazure.com/) ou [EA portal](https://ea.azure.com/).

Para obter mais informações sobre acesso programático para as informações de faturação, consulte [obter informações acerca do consumo de recursos do Microsoft Azure](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview). Para operações de REST API, consulte [referência de API de REST de faturação do Azure](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).

Quando transferir a utilização de CSV para serviços que suportam etiquetas com faturação, as etiquetas são apresentadas na coluna de etiquetas.

### <a name="critical-resource-controls"></a>Controlos de recurso crítico

Como a sua organização adiciona serviços principais para a subscrição, torna-se mais importante certificar-se de que esses serviços estão disponíveis para evitar a interrupção de negócios. [Bloqueios de recurso](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) permitem-lhe restringir operações nos recursos de elevado valor onde modificar ou eliminá-los teria um impacto significativo na sua aplicações ou a infraestrutura de nuvem. Pode aplicar bloqueios para uma subscrição, o grupo de recursos ou o recurso. Normalmente, aplicar bloqueios dos recursos, como redes virtuais, gateways e as contas de armazenamento.

Bloqueios de recursos atualmente suportam dois valores: **CanNotDelete** e **ReadOnly**. **CanNotDelete** significa que os utilizadores (com os direitos adequados) podem ainda ler ou modificar um recurso, mas não é possível eliminá-lo. **Só de leitura** significa que os utilizadores autorizados não é possível eliminar ou modificar um recurso.

Bloqueios de recursos são aplicadas apenas a operações acontecer na plane a gestão, que consiste em operações de envio para <https://management.azure.com>. Os bloqueios não restringir como recursos executar as suas próprias funções. Alterações de recurso estão limitadas, mas as operações de recurso não estão restringidas. Por exemplo, um **ReadOnly** bloqueio na base de dados SQL impede-o de eliminar ou modificar a base de dados, mas não o impede de criar, atualizar ou eliminar dados na base de dados.

Aplicar **ReadOnly** pode originar resultados inesperados porque algumas operações que parecem como leitura operações necessitam de ações adicionais. Por exemplo, colocar uma **ReadOnly** bloqueio numa conta de armazenamento impede que todos os utilizadores listar as chaves. A operação de listagem das chaves é processada através de um pedido POST porque as chaves devolvidas estão disponíveis para operações de escrita.

![Controlos de recurso crítico](./media/governance-in-azure/security-governance-in-azure-fig5.png)

Para obter outro exemplo, colocar uma **ReadOnly** bloqueio de um recurso do App Service do Azure impede o Explorador de servidores do Visual Studio a apresentação de ficheiros para o recurso porque esse interação requer acesso de escrita.

Ao contrário do controlo de acesso baseado em funções, utilize as bloqueios de gestão para aplicar uma restrição em todos os utilizadores e funções. Para saber mais sobre como definir permissões, consulte [gerir o acesso através do portal do Azure e o RBAC](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).

Ao aplicar um bloqueio num âmbito principal, todos os recursos desse âmbito herdam o bloqueio do mesmo. Mesmo recursos que adicionar mais tarde herdam o bloqueio do principal. O bloqueio mais restritivo na herança tem precedência.

Para criar ou eliminar as bloqueios de gestão, tem de ter acesso a ações Microsoft.Authorization/ ou Microsoft.Authorization/locks/. Das funções incorporadas, apenas o proprietário e o administrador de acesso de utilizador são concedidas essas ações.

### <a name="api-access-to-billing-information"></a>Acesso a API para as informações de faturação

Utilize as APIs de faturação do Azure para retirar dados de utilização e de recursos para as ferramentas de análise de dados preferencial. A utilização de recursos do Azure e RateCard APIs podem ajudá-lo com precisão prever e gerir os custos. As APIs são implementadas como um fornecedor de recursos e a parte da família de APIs expostas através do Azure Resource Manager.

#### <a name="resource-usage-api-preview"></a>Utilização de recursos API (pré-visualização)

Utilizar o Azure [API de utilização de recursos](https://msdn.microsoft.com/library/azure/mt219003) para obter os seus dados de consumo estimado do Azure. A API inclui:

- **RBAC**: Configurar políticas de acesso no [portal do Azure](https://portal.azure.com/) ou através de [cmdlets Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) para especificar quais os utilizadores ou aplicações podem obter acesso a dados de utilização da subscrição. Os chamadores tem de utilizar tokens padrão do Azure Active Directory para autenticação. Adicione o autor da chamada à função ou o leitor de faturação, leitor, proprietário ou contribuinte obter acesso aos dados de utilização de uma subscrição do Azure específica.

- **As agregações de hora ou diárias**: os chamadores podem especificar se pretendem os seus dados de utilização do Azure em incrementos de horário ou diários. A predefinição é diária.

- **Os metadados de instância (inclui os sinalizadores de recurso)**: obter os detalhes de nível de instância, como o URI do recurso completamente qualificado (/subscriptions/{targetsubscriptionid}/resourcegroups/{targetresourcegroupname} {id de subscrição} /..), informações do grupo de recursos e as etiquetas de recursos. Estes metadados ajuda-o a deterministicamente e através de programação alocar utilização por etiquetas, em casos de utilização como charging de cruzada.

- **Metadados do recurso**: detalhes de recursos, tais como o nome de medição, categoria de medição, subcategoria de medição, unidade e região dar o autor da chamada uma melhor compreensão sobre o que estava a ser consumido. Estamos a trabalhar também para assegurar a terminologia de metadados de recursos através do portal do Azure, a utilização do Azure CSV, EA CSV e de outras experiências de destinado ao público para o ajudar a correlacionar dados através de experiências de faturação.

- **Utilização para todos os tipos de oferta**: dados de utilização estão disponíveis para oferecem todos os tipos, incluindo pay as you go, o MSDN, o compromisso monetário, o crédito monetário e o EA.

#### <a name="resource-ratecard-api"></a>Recurso RateCard API

Utilize a API de RateCard de recursos do Azure para obter a lista de recursos do Azure disponíveis e as informações de preços estimadas para cada. A API inclui:

- **RBAC**: configurar as políticas de acesso no portal do Azure ou através de cmdlets do Azure PowerShell para especificar quais os utilizadores ou aplicações podem obter acesso aos dados RateCard. Os chamadores tem de utilizar tokens padrão do Azure Active Directory para autenticação. Adicione o autor da chamada à função de leitor, proprietário ou contribuinte obter acesso aos dados de utilização para uma determinada subscrição do Azure.

- **Suporte para pay as you go, o MSDN, o compromisso monetário e o crédito monetário ofertas (mas não EA)**: Esta API fornece informações de taxa de nível de oferta do Azure. O autor da chamada desta API tem de passar as informações de oferta para obter os detalhes do recurso e taxas. EA atualmente não é suportada porque o EA proporciona tiver personalizado taxas por inscrição. 

#### <a name="scenarios"></a>Cenários

A combinação de padrões de utilização e RateCard APIs possibilita estes cenários:

- **Compreender Azure gastam durante o mês**: Utilize a combinação de padrões de utilização e RateCard APIs para obter informações sobre a melhor nuvem gastam durante o mês. Pode analisar a hora a hora e diariamente estimativas de utilização e encargos.

- **Configure alertas**: utilizar a utilização e APIs de RateCard para obter consumo de nuvem estimado e os encargos e configure baseada em recursos ou monetário com base em alertas.

- **Prever fatura**: Get seu consumo estimado nuvem gastam e aplicar algoritmos de machine learning para prever a que a fatura seria no fim do ciclo de faturação.

- **Executar um análise de custos de pré-consumo**: utilizar a API de RateCard para prever a quantidade a fatura seria para a sua utilização esperada ao mover as cargas de trabalho para o Azure. Se tiver cargas de trabalho existentes em nuvens privadas ou outros nuvens, também é possível mapear a utilização com o Azure gastam a taxas para obter uma estimativa melhor do Azure. Esta estimativa dá-lhe a capacidade de dinâmico na oferta e comparar entre os tipos de oferta diferentes para além de pay as you go, como o compromisso monetário e crédito monetário.

- **Executar uma análise investiguem**: pode determinar se é mais económico executar cargas de trabalho noutra região ou outra configuração do recurso do Azure. Os custos de recursos do Azure podem divergir com base na região do Azure que está a utilizar. Também pode determinar se o outro tipo de oferta do Azure fornece uma melhor taxa sobre um recurso do Azure.

### <a name="networking-controls"></a>Controlos de rede

Pode ser o acesso aos recursos externos (através da internet) ou interno (na rede da empresa). É fácil para os utilizadores na sua organização inadvertidamente colocar recursos a errado lugar para cima e, potencialmente, abra-los para acesso malicioso. Tal como acontece com dispositivos no local, as empresas tem de adicionar controlos adequados para garantir que os utilizadores do Azure tomar decisões corretas.

![Controlos de rede](./media/governance-in-azure/security-governance-in-azure-fig6.png)

Para governação de subscrição, os seguintes recursos de núcleos fornecem básico controlo de acesso.

#### <a name="network-connectivity"></a>Conectividade de rede

[Redes virtuais](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) são objetos de contentor para sub-redes. Apesar de não estritamente necessários, uma rede virtual é frequentemente utilizada para ligar aplicações a recursos empresariais internos. O serviço de rede Virtual do Azure permite-lhe ligar de forma segura a recursos do Azure uns aos outros com redes virtuais.

Uma rede virtual é uma representação da sua própria rede na nuvem. Uma rede virtual é um isolamento lógico da nuvem do Azure dedicado à sua subscrição. Também pode ligar redes virtuais à sua rede no local.

Seguem-se capacidades para redes virtuais do Azure:

- **Isolamento**: redes virtuais são isoladas entre si. Pode criar redes virtuais separadas para o desenvolvimento, teste e produção que utilizam os mesmos blocos de endereços CIDR. Por outro lado, pode criar várias redes virtuais que utilizam diferentes blocos de endereços CIDR e ligar redes em conjunto. Pode segmentar uma rede virtual em várias sub-redes. O Azure oferece resolução dos nomes internos para VMs e serviços de nuvem do Azure instâncias de função que estão ligadas a uma rede virtual. Opcionalmente, pode configurar uma rede virtual para utilizar os seus próprios servidores DNS, em vez de utilizar a resolução do nome interno do Azure.

- **Conectividade Internet**: máquinas virtuais do Azure todas as e instâncias de função de serviços em nuvem que estão ligadas a uma rede virtual tem acesso à internet, por predefinição. Também pode ativar o acesso de entrada a recursos específicos, conforme necessário.

- **Conectividade de recursos do Azure**: pode ligar recursos do Azure, tais como serviços em nuvem e VMs, a mesma rede virtual. Os recursos podem ligar entre si através de endereços IP privados, mesmo se não estiverem em sub-redes diferentes. O Azure oferece encaminhamento predefinido entre sub-redes, redes virtuais e redes no local, pelo que não tem de configurar e gerir as rotas.

- **Conectividade de rede virtual**: pode ligar redes virtuais entre si. Recursos que estão ligados à rede virtual, em seguida, podem comunicar com qualquer recurso no qualquer outra rede virtual.

- **No local conectividade**: pode ligar redes virtuais para redes no local através de ligações de rede privada entre a rede e o Azure ou através de uma ligação de rede privada virtual (VPN) site a site através da internet.

- **Filtragem de tráfego**: pode filtrar o tráfego de rede (entrado e saído) para máquinas virtuais e serviços em nuvem pelo endereço IP de origem e porta, endereço IP de destino e porta e protocolo.

- **Encaminhamento**: Opcionalmente, pode substituir predefinição do encaminhamento do Azure, ao configurar as seus próprios rotas ou utilizando as rotas BGP através de um gateway de rede.

#### <a name="network-access-controls"></a>Controlos de acesso de rede

[Grupos de segurança de rede](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (NSGs) são como uma firewall e fornecer as regras para como um recurso pode "conversar"com através da rede. Fornecem controlo sobre a forma como uma sub-rede (ou a máquina virtual) pode ligar à internet ou outras sub-redes na mesma rede virtual.

Um grupo de segurança de rede contém uma lista de regras de segurança que permitem ou negam o tráfego de rede para recursos ligados a redes virtuais do Azure. Os NSGs podem ser associados a sub-redes, VMs individuais (clássica) ou interfaces de rede individuais (NICs) ligadas a VMs (Resource Manager).

Quando um NSG é associado uma sub-rede, as regras são aplicadas a todos os recursos ligados à sub-rede. Pode restringir mais o tráfego ao associar um NSG a uma VM ou NIC.

## <a name="security-and-compliance-with-organizational-standards"></a>Segurança e conformidade com as normas organizacionais

Cada empresa tem necessidades diferentes e será reap distintas beneficia de soluções de nuvem. Ainda assim, os clientes de todos os tipos têm as mesmas básicas preocupações mover para a nuvem. O que os clientes pretendem de fornecedores de nuvem é:

- **Proteger os nossos dados**: líderes das IT confirma que a nuvem pode fornecer dados de uma maior segurança e controlo administrativo. Mas estes estão ainda preocupados com a que a migrar para a nuvem irá deixá-los mais vulnerável a hackers que as soluções de internas atuais.

- **Manter os nossos dados privada**: serviços em nuvem elevar os desafios de privacidade exclusivo. Como ver empresas para a nuvem para poupar nos custos da infraestrutura e melhorar a respetiva flexibilidade, estes também preocupar com a perda do controlo de onde os dados são armazenados, que está a aceder ao mesmo e como é utilizado.

- **Dê-nos controlo**: mesmo como empresas tirar partido da nuvem para implementar soluções inovadoras mais, está a preocupados com a perda do controlo dos respetivos dados. As divulgações recentes das agências governamentais aceder aos dados de cliente, através de meios legais e extralegal, certifique-algumas CIOs wary de armazenar os seus dados na nuvem.

- **Promover a transparência**: decisores de negócios compreender a importância da segurança, privacidade e o controlo. Mas, pretendem também a capacidade para verificar independentemente como os respetivos dados sejam armazenados, acedidos e protegidos.

- **Manter a conformidade**: como empresas expandir a sua utilização de tecnologias de nuvem, a complexidade e o âmbito de padrões e regulamentos continuarem a evoluir. As empresas precisam de saber o que irão ser satisfeitas as normas de conformidade.

## <a name="security-configuration-for-monitoring-logging-and-auditing"></a>Configuração de segurança para monitorização, registo e auditoria

Os subscritores do Azure podem gerir os seus ambientes de nuvem a partir de vários dispositivos. Estes dispositivos podem incluir estações de trabalho de gestão, PCs de programadores e os dispositivos de utilizador final, mesmo com privilégios que tenham permissões específicas de tarefas. 

Em alguns casos, as funções administrativas são efetuadas através das consolas baseadas na web, tais como o portal do Azure. Noutros casos, poderão existir ligações diretas para o Azure a partir de sistemas no local através de VPNs, os serviços de Terminal, protocolos de aplicação de cliente ou (através de programação) a API de gestão de serviço do Azure (SMAPI). Além disso, os pontos finais de cliente podem ser um domínio associado ou isolado e não geridos, como tablets ou smartphones.

Este variabilidade pode acarretar um risco significativo para uma implementação de nuvem. Pode ser difícil de gerir, controlar e auditar as ações administrativas. Este variabilidade também pode acarretar ameaças de segurança através do acesso não regulado para os pontos finais de cliente que são utilizados para gerir os cloud services. A utilização das estações de trabalho gerais ou pessoais para desenvolver e gerir a infraestrutura abre vetores de ameaças imprevisíveis, como navegação na Web (por exemplo, ataques de tipo “watering hole”) ou e-mail (engenharia social e phishing).

Monitorização, registo e a auditoria fornecem uma base para controlar e compreender as atividades administrativas. Todas as ações em detalhe de auditoria poderá nem sempre ser viável devido à quantidade de dados gerados. Mas a auditoria da eficácia das políticas de gestão é a melhor prática.

Governação de segurança do Azure dos GPOs do Azure Active Directory Domain Services (AD DS) pode ajudar a controlar as interfaces de Windows dos administradores, tal como a partilha de ficheiros. Inclua estações de trabalho de gestão em monitorização, registo e auditoria de processos. Controle todos os acessos e utilizações de administrador e programador.

### <a name="azure-security-center"></a>Centro de Segurança do Azure

[Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-intro) fornece uma vista central do Estado de segurança dos recursos nas subscrições. Fornece recomendações que ajudam a impedir recursos comprometidos. Pode ativar o mais detalhadas políticas – por exemplo, aplicar políticas em grupos de recurso específico que permitem à empresa personalizar a sua postura ao risco que está a endereçamento.

![Centro de Segurança do Azure](./media/governance-in-azure/security-governance-in-azure-fig7.png)

Centro de segurança fornece gestão de políticas e monitorização de segurança integrada nas suas subscrições do Azure, ajuda a detetar ameaças que caso contrário podem passar despercebidas e funciona com um ecossistema abrangente de soluções de segurança. Depois de ativar [políticas de segurança](https://docs.microsoft.com/azure/security-center/security-center-policies) para recursos de uma subscrição, o Centro de segurança analisa a segurança dos seus recursos para identificar potenciais vulnerabilidades. As informações sobre a configuração da rede estão disponíveis de forma instantânea.

Centro de segurança do Azure representa uma combinação de melhor prática análise e segurança gestão de políticas para todos os recursos dentro de uma subscrição do Azure. Permite que as equipas de segurança e officers de risco para evitar, detetar e responder a ameaças de segurança como automaticamente recolhe e analisa os dados de segurança dos seus recursos do Azure, rede e soluções de parceiros, como os programas antimalware e firewalls.

Além disso, o Centro de segurança do Azure aplica análises avançadas, incluindo machine learning e análise comportamental. Utiliza a ameaças globais de produtos e serviços, a Microsoft Crimes digitais unidade (DCU), o Microsoft Security Response Center (MSRC), Microsoft e de feeds externos. Pode aplicar [governação de segurança](https://www.credera.com/blog/credera-site/azure-governance-part-4-other-tools-in-the-toolbox/) amplamente ao nível da subscrição. Em alternativa, pode restringir-a para baixo para requisitos específicos e aplicá-las para recursos individuais através da definição de política.

Por fim, o Centro de segurança do Azure analisa o funcionamento de segurança de recursos com base nessas políticas e utiliza estas informações para fornecer insightful dashboards e os alertas de eventos, tais como a deteção de software maligno ou a ligação de IP maliciosa tentativas. Para obter mais informações sobre como aplicar recomendações, consulte [implementar recomendações de segurança no Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-recommendations).

Centro de segurança do Azure monitoriza os seguintes recursos do Azure:

- Máquinas virtuais (VMs) (incluindo os serviços em nuvem)

- Redes virtuais

- Bases de dados SQL

- Integrado com a sua subscrição do Azure, tal como uma firewall de aplicação web em VMs e de soluções de parceiros de [ambiente de serviço de aplicações](https://docs.microsoft.com/azure/app-service/app-service-app-service-environments-readme)

Quando aceder primeiro ao centro de segurança, a recolha de dados está ativada em todas as máquinas virtuais na sua subscrição. Recomendamos que mantenha a recolha de dados ativada, mas pode [desativá-lo](https://docs.microsoft.com/azure/security-center/security-center-faq) na política de centro de segurança.

### <a name="log-analytics"></a>Log Analytics

Segurança de informações do Log Analytics do Azure software desenvolvimento e o serviço da equipa e [programa governação](https://github.com/Microsoft/azure-docs/blob/master/articles/log-analytics/log-analytics-security.md) suporta os requisitos de negócio. Respeite as leis e regulamentos conforme descrito em [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) e [compatibilidade de centro de fidedignidade da Microsoft](https://www.microsoft.com/TrustCenter/Compliance/default.aspx). Como a análise de registos estabelece requisitos de segurança, identifica os controlos de segurança e gere e riscos de monitores também está descrita não existe. Anual, a equipa de revê políticas, normas, procedimentos e diretrizes.

Cada membro de equipa de desenvolvimento de análise de registos recebe formação de segurança da aplicação formal. Um sistema de controlo de versão ajuda a proteger cada projeto de desenvolvimento de software.

A Microsoft tem uma equipa de segurança e conformidade supervisiona e avalia todos os serviços Microsoft. Officers de segurança de informações constituem a equipa e não estiver associados a engenharia departamentos que desenvolver a análise de registos. Os officers segurança tem sua própria cadeia de gestão. Estes realize avaliações independentes dos produtos e serviços para ajudar a garantir a segurança e conformidade.

A funcionalidade principal do Log Analytics é fornecida por um conjunto de serviços que são executados no Azure. Cada serviço proporciona uma função de gestão específica e pode combinar serviços para alcançar cenários de gestão diferentes.

![Serviços Azure para gestão](./media/governance-in-azure/security-governance-in-azure-fig9.JPG)

Estes serviços de gestão foram concebidos na nuvem. Completamente estiver alojados no Azure, pelo que não envolvem implementar e gerir recursos no local. A configuração é mínima e pode ser configurado e em execução num fim de minutos.

Colocar um agente em qualquer computador Windows ou Linux no seu centro de dados e irá enviar dados para análise de registos. Aqui, pode ser analisado juntamente com todos os outros dados recolhidos a partir de serviços em nuvem ou no local. Utilize a cópia de segurança do Azure e o Azure Site Recovery para tirar partido da nuvem para cópia de segurança e a elevada disponibilidade para recursos no local.

![Serviços de gestão no dashboard do Azure](./media/governance-in-azure/security-governance-in-azure-fig8.png)

Os Runbooks na nuvem, normalmente, não é possível aceder os recursos no local, mas pode instalar um agente num ou mais computadores que irão alojar os runbooks no seu centro de dados. Quando inicia um runbook, especifique se pretende que seja executado na nuvem ou uma função de trabalho local.

Diferentes soluções estão disponíveis da Microsoft e de parceiros que pode adicionar à sua subscrição do Azure para aumentar o valor do seu investimento na análise de registos. Por exemplo, o Azure oferece [soluções de gestão](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solutions)– prepackaged conjuntos de lógica que implementam um cenário de gestão através de um ou mais serviços de gestão.

![Galeria de soluções de gestão no Azure](./media/governance-in-azure/security-governance-in-azure-fig10.png)

Como parceiro, pode criar as suas próprias soluções para suportar as suas aplicações e serviços e fornecem aos utilizadores através de modelos do Azure Marketplace ou início rápido.

## <a name="performance-alerting-and-monitoring"></a>Monitorização e alertas de desempenho

### <a name="alerting"></a>Alertas

Os alertas são um método de monitorização de recursos do Azure de métricas, eventos ou registos. Estes notificá-lo quando for cumprida uma condição que tiver especificado.

Alertas estão disponíveis nos vários serviços, incluindo:

- **Azure Application Insights**: permite web alertas de teste e a métrica. Para obter mais informações, consulte [definir alertas no Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-alerts) e [monitorizar a disponibilidade e capacidade de resposta de qualquer Web site](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability).

- **Análise de registo**: permite que o encaminhamento de atividade e os registos de diagnóstico para análise de registos. Permite métrica, registo e outros tipos de alerta. Para obter mais informações, consulte [alertas na análise de registos](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts).

- **Monitor do Azure**: permite que os alertas com base nos valores métricos e eventos de registo de atividade. Pode utilizar o [API REST da Azure Monitor](https://msdn.microsoft.com/library/dn931943.aspx) para gerir alertas. Para obter mais informações, consulte [utilizando o portal do Azure, PowerShell ou a interface de linha de comandos para criar alertas](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal).

### <a name="monitoring"></a>Monitorização

Problemas de desempenho na sua aplicação em nuvem podem afetar a sua empresa. Com os vários componentes interligados e versões frequentes, degradations pode acontecer em qualquer altura. E se estiver a desenvolver uma aplicação, os utilizadores, geralmente, detetar problemas que não foi encontrado no teste. Deve conhecer sobre estes problemas imediatamente e tem as ferramentas para diagnosticar e corrigir.

Há uma série de ferramentas para a monitorização de serviços e aplicações do Azure. Algumas das funcionalidades correspondentes sobrepõem-se. Trata-se apenas parcialmente devido a diária esbater pessoal entre desenvolvimento e a operação de uma aplicação.

Seguem-se as ferramentas principais:

- **Monitor do Azure** é uma ferramenta básica para monitorizar os serviços em execução no Azure. Proporciona ao nível da infraestrutura dados sobre o débito de um serviço e o ambiente surrounding. Se estiver a gerir todas as suas aplicações no Azure e decidir se pretende aumentar ou reduzir verticalmente de recursos, o Monitor do Azure pode ajudar a começar.

- **Application Insights** podem ser utilizadas para desenvolvimento e como uma solução de monitorização de produção. Funciona ao instalar um pacote na sua aplicação, pelo que dá-lhe uma vista mais interna de que está a suceder. Os dados incluem tempos de resposta das dependências, rastreios de exceção, a depuração de instantâneos e perfis de execução. Fornece ferramentas para analisar este telemetria para ajudar a depurar uma aplicação e para ajudar a compreender de que os utilizadores estão a fazer com o mesmo. Pode saber se um pico de pedidos de tempos de resposta é devido a algo numa aplicação ou algum problema resourcing externo. Se utilizar o Visual Studio e a aplicação está com falha, pode aceder à direita para a linha de problema de código, para que o pode corrigir.

- **Análise de registo** destina-se os utilizadores que precisam para otimizar o desempenho e planear a manutenção em aplicações em execução na produção. Este recolhe e agrega dados de várias origens, com um atraso de 10 a 15 minutos. Fornece uma solução de gestão de IT holística do Azure, no local e a infraestrutura de terceiros baseados na nuvem (tais como os Amazon Web Services). Fornece ferramentas para analisar os dados entre origens, permite consultas complexas em todos os registos e proativamente podem alertá em condições especificadas. Pode mesmo recolher dados personalizados no seu repositório central e, em seguida, consultar e visualizar os dados.

- **O System Center Operations Manager** é para gerir e monitorizar as instalações de nuvem de elevado. Poderá estar já familiarizado com o mesmo como uma ferramenta de gestão para o local no Windows Server e Hyper-V com base em nuvens, mas também pode integrar e gerir aplicações do Azure. Entre outras coisas, poderá instalar Application Insights num aplicações em direto existentes. Se uma aplicação ficar inativo, o Operations Manager indica em segundos.


## <a name="next-steps"></a>Passos Seguintes

- [Melhores práticas para criar modelos do Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices)

- [Exemplos para implementar a governação de subscrição do Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-subscription-examples)

- [Microsoft Azure Government](https://docs.microsoft.com/azure/azure-government/)
