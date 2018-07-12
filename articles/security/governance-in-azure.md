---
title: Governação no Azure | Documentos da Microsoft
description: Saiba mais sobre serviços computação baseados na nuvem que podem aumentar e reduzir verticalmente para satisfazer as necessidades do seu aplicativo ou a empresa.
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
ms.openlocfilehash: 579e900ee6616af8fd197e501364acd8e18d3e37
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38970528"
---
# <a name="governance-in-azure"></a>Governação no Azure

O Azure fornece muitas opções de segurança e a capacidade para controlá-las, de modo a que pode satisfazer os requisitos únicos das implementações da sua organização.

Governação de cloud do Azure refere-se para os processos de tomada de decisões, critérios e políticas envolvidos no planejamento, arquitetura, aquisição, implantação, operação e gerenciamento de informática na cloud. Governação de cloud do Azure fornece uma auditoria integrada e a abordagem de consultoria para rever e aconselhá organizações na respetiva utilização da plataforma do Azure. 

Para criar um plano de Governança de cloud do Azure, terá de realizar um exame minucioso sobre as pessoas, processos e tecnologias agora no local. Em seguida, pode criar estruturas que tornam mais fácil para que a TI consistentemente suportar as necessidades de negócio ao mesmo tempo, os utilizadores com a flexibilidade para utilizar as funcionalidades do Azure.

## <a name="implementation-of-policies-processes-and-standards"></a>Implementação de políticas, processos e normas 

Gestão estabeleceu funções e responsabilidades para supevisionar a implementação de políticas de segurança de informações e a continuidade operacional em todo o Azure. Gestão do Azure é responsável por supervisionar segurança e continuidade às práticas dentro de suas respectivas equipes (incluindo a terceiros). Ele também facilita a conformidade com as políticas de segurança, processos e normas.

### <a name="account-provisioning"></a>Aprovisionamento de contas

A definição de hierarquia de conta é um grande passo para utilizar e estruturar os serviços do Azure dentro de uma empresa. É a estrutura de governação de núcleo. Os clientes que tenham um contrato Enterprise (EA) podem subdividir o ambiente em Departamentos, contas e subscrições.

![Aprovisionamento de contas](./media/governance-in-azure/security-governance-in-azure-fig1.png)

Se não tiver um Enterprise Agreement, considere utilizar [etiquetas do Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) ao nível da subscrição para definir a hierarquia. Uma subscrição do Azure é a unidade básica que contém todos os recursos. Também define vários limites no Azure, como o número de núcleos e recursos. As assinaturas podem conter [grupos de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), que pode conter recursos. [Controlo de acesso baseado em funções (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) princípios aplicam-se nesses três níveis.

Toda empresa é diferente. Para empresas não empresariais, a hierarquia da utilização de etiquetas do Azure permite flexibilidade em como o Azure está organizado. Antes de implementar recursos no Azure, deve modelam uma hierarquia e compreender o impacto sobre faturação, acesso a recursos e complexidade.

### <a name="subscription-controls"></a>Controles de subscrição

Subscrições determinam como a utilização de recursos é comunicada e cobrada. Pode configurar subscrições de faturação separada e pagamento. Uma conta do Azure pode ter várias subscrições. Subscrições podem ser utilizadas para determinar a utilização de recursos do Azure de vários departamentos numa empresa.

Por exemplo, uma empresa tem IT, RH, e departamentos de Marketing e esses departamentos estiverem a executar projetos diferentes. A empresa pode basear sua faturação na utilização de cada departamento de recursos do Azure, como as máquinas virtuais. A empresa, em seguida, pode controlar as finanças de cada departamento.

As subscrições do Azure estabeleça três parâmetros:

- ID do assinante exclusivo

- Localização de faturação

- Conjunto de recursos disponíveis

Para um indivíduo, esses parâmetros incluem um ID da conta Microsoft, um número de cartão de crédito e o conjunto completo de serviços do Azure. A Microsoft impõe limites de consumo, dependendo do tipo de subscrição.

Hierarquias de inscrição do Azure definem como os serviços são estruturados dentro de um contrato Enterprise. O portal do contrato Enterprise permite aos clientes dividir o acesso aos recursos do Azure associado a um Enterprise Agreement com base em hierarquias flexíveis que são personalizáveis para as necessidades da organização. O padrão de hierarquia deve corresponder ao gerenciamento e a estrutura geográfica para levar em conta o acesso de faturação e recursos associado da organização.

Os três padrões de alto nível de hierarquia são a unidade funcional, business e geográfica. Os departamentos são uma construção administrativa para agrupamentos de conta. Dentro de cada departamento, as contas podem ser atribuídas subscrições, que criar silos de faturação e vários limites de chaves no Azure (por exemplo, número de VMs e contas de armazenamento).

![Controles de subscrição](./media/governance-in-azure/security-governance-in-azure-fig2.png)


Para organizações com um Enterprise Agreement, as subscrições do Azure, siga uma hierarquia de nível de quatro:

1. Administrador de inscrição de Enterprise

2. Administrador do departamento

3. Proprietário da conta

4. Administrador de serviço

Esta hierarquia rege o seguinte:

- Relação de faturação.

- Administração da conta.

- Acesso a recursos através do RBAC.

- Limites:

  - Utilização e faturação (cartão de taxas com base nos números de oferta)

  - Limites

  - Rede virtual

- Anexo para o Azure Active Directory (Azure AD). Uma instância do Azure AD pode ser associada com o número de subscrições.

- Associação com uma conta de inscrição empresarial.

### <a name="role-based-access-control"></a>Controlo de acesso baseado em funções

[RBAC](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) permite a gestão de acesso detalhadas de recursos no Azure. Ao utilizar o RBAC, pode conceder apenas a quantidade de acesso que os utilizadores precisam realizar seus trabalhos. As empresas devem se concentrar em dar aos funcionários as permissões exatas que precisam. Demasiadas permissões expõem uma conta para os atacantes. Permissões insuficientes significam que os funcionários não é possível obter a trabalhar com eficiência. 

Em vez de dar todas as pessoas sem restrições permissões na sua subscrição do Azure ou recursos, pode permitir que apenas determinadas ações. Por exemplo, pode utilizar o RBAC para permitir que um funcionário gerir máquinas virtuais numa subscrição, enquanto outro funcionário gerencia bases de dados do SQL na mesma subscrição.

Para conceder acesso, atribuir funções a utilizadores, grupos ou aplicações num determinado âmbito. O âmbito de uma atribuição de função pode ser uma subscrição, um grupo de recursos ou um único recurso. As funções atribuídas a um âmbito principal também concede acesso para os filhos que ele contém. Por exemplo, um utilizador com acesso a um grupo de recursos pode gerir todos os recursos nele contidos, como Web sites, máquinas virtuais e sub-redes. Em cada subscrição, pode criar até 2000 atribuições de funções.

Uma função é uma coleção de permissões e RBAC tem várias funções incorporadas. As funções incorporadas seguintes aplicam-se a todos os tipos de recursos:

- **Proprietário** tem acesso total a todos os recursos, incluindo o direito de delegar o acesso a outras pessoas.

- **Contribuinte** pode criar e gerir todos os tipos de recursos do Azure, mas não é possível conceder acesso a outras pessoas.

- **Leitor** pode ver todos os recursos do Azure.

O restante das funções incorporadas no Azure permitir a gestão de recursos específicos do Azure. Por exemplo, a função de contribuinte de Máquina Virtual permite ao utilizador criar e gerir máquinas virtuais. Para obter uma lista de todas as funções incorporadas e respetivas operações, consulte [funções incorporadas do RBAC](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

RBAC suporta operações de gestão de recursos do Azure no portal do Azure e APIs do Azure Resource Manager. Na maioria dos casos, o RBAC não é possível autorizar as operações de nível de dados para recursos do Azure. Para obter informações sobre como o RBAC está a ser expandido para operações de dados, consulte [compreender as definições de função](https://docs.microsoft.com/azure/role-based-access-control/role-definitions).

Se as funções incorporadas não atenderem às suas necessidades de acesso específicos, pode [criar uma função personalizada](https://docs.microsoft.com/azure/role-based-access-control/custom-roles). Tal como funções incorporadas, as funções personalizadas podem ser atribuídas a utilizadores, grupos e aplicações com a subscrição, o grupo de recursos e o âmbito de recursos. Pode criar funções personalizadas usando [do Azure PowerShell](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell), [CLI do Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli)e o [REST API](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest).

### <a name="resource-management"></a>Gestão de recursos

O Azure disponibiliza dois modelos de implementação: [clássico](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) e Azure Resource Manager.

O modelo clássico, cada recurso existe forma independente. Não é possível agrupar recursos relacionados. Precisa controlar manualmente quais os recursos que compõem a sua aplicação ou solução e não se esqueça de geri-los numa abordagem de coordenada. A unidade básica de gestão é a subscrição. É difícil dividir recursos dentro de uma subscrição, o que leva à criação de um grande número de subscrições.

O modelo de implementação do Gestor de recursos inclui o conceito de um [grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). Os grupos de recursos são um contentor para os recursos que partilham um ciclo de vida comum. Ele pode incluir todos os recursos para a solução, ou apenas os recursos que pretende gerir como um grupo. Decida como pretende atribuir recursos a grupos de recursos com base no que é mais adequado para a sua organização.

O modelo de implementação Resource Manager oferece várias vantagens:

- Pode implementar, gerir e monitorizar todos os serviços da sua solução como um grupo, em vez de os processar individualmente.

- Pode implementar a solução durante seu ciclo de vida e ter a confiança de que os recursos são implementados num estado consistente repetidamente.

- Pode aplicar o controlo de acesso a todos os recursos no grupo de recursos. Essas políticas são aplicadas automaticamente quando são adicionados novos recursos para o grupo de recursos.

- Pode aplicar etiquetas a recursos para organizar logicamente todos os recursos na sua subscrição.

- Pode utilizar o JavaScript Object Notation (JSON) para definir a infraestrutura da sua solução. O ficheiro JSON é conhecido como modelo do Resource Manager.

- Pode definir as dependências entre os recursos, para que serem implementadas na ordem correta.

![Resource Manager](./media/governance-in-azure/security-governance-in-azure-fig4.png)

Para obter recomendações sobre modelos, veja o artigo [Melhores práticas para criar modelos do Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices).

O Azure Resource Manager analisa as dependências para ajudar a garantir que os recursos são criados na ordem correta. Se um recurso depende de um valor de outro recurso (por exemplo, uma máquina virtual necessita de uma conta de armazenamento para discos), [definir uma dependência](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-define-dependencies) no modelo.

Também pode utilizar o modelo para atualizar a infraestrutura. Por exemplo, pode adicionar um recurso à solução e adicionar regras de configuração para os recursos que já estão implementados. Se o modelo especificar a criação de um recurso, mas esse recurso já existir, o Resource Manager efetua uma atualização em vez de criar um novo recurso. O Resource Manager atualiza o recurso existente para o mesmo Estado que teria como novo.

Resource Manager fornece extensões para cenários quando precisar de mais de operações, como a instalação de software que não está incluído na configuração.

### <a name="resource-tracking"></a>Controlo de recursos

À medida que os utilizadores na sua organização adicionar recursos para a subscrição, torna-se mais importante associar recursos com o departamento apropriado, o cliente e o ambiente. Pode anexar metadados aos recursos através de [etiquetas](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags). Utilizar etiquetas para fornecer informações sobre o recurso ou o proprietário. As etiquetas permitem que não só a agregar e agrupar recursos de várias maneiras, mas também usar esses dados para fins de estorno.

Utilize etiquetas quando tem uma coleção complexa de grupos de recursos e recursos e precisam de visualizar esses elementos da forma que faz mais sentido para. Por exemplo, pode marcar os recursos que desempenham uma função semelhante na sua organização ou que pertençam ao mesmo departamento.

Sem etiquetas, os utilizadores na sua organização podem criar vários recursos que talvez seja difícil identificar e gerir mais tarde. Por exemplo, pode querer eliminar todos os recursos para um projeto. Se esses recursos não estão marcados para o projeto, terá de encontrá-los manualmente. A etiquetagem pode ser um meio importante para reduzir os custos desnecessários associados à sua subscrição.

Recursos não precisam de residir no mesmo grupo de recursos para partilhar uma etiqueta. Pode criar sua própria taxonomia de etiquetas para se certificar de que todos os utilizadores na sua organização utilizam etiquetas comuns em vez de aplicar inadvertidamente etiquetas ligeiramente diferentes (por exemplo, "Depart" em vez de "departamento").

Políticas de recursos permitem-lhe criar regras padrão para a sua organização. Pode criar políticas para garantir que os recursos são marcados com os valores adequados.

Também pode ver recursos com etiquetas através do Portal do Azure. O [relatório de utilização](https://docs.microsoft.com/azure/billing/billing-understand-your-bill) para a sua subscrição inclui nomes de etiquetas e valores, por isso, pode dividir os custos por etiquetas.

Para obter mais informações sobre etiquetas, consulte [iniciativa de política de etiquetas de faturação](../azure-policy/scripts/billing-tags-policy-init.md).

As seguintes limitações aplicam-se às etiquetas:

- Cada recurso ou grupo de recursos pode ter um máximo de 15 pares de chave/valor de etiqueta. Esta limitação aplica-se apenas a etiquetas diretamente aplicadas ao recurso ou grupo de recursos. Um grupo de recursos pode conter muitos recursos que cada um tem 15 pares de chave/valor de etiqueta.

- O nome da etiqueta está limitado a 512 carateres.

- O valor da etiqueta está limitado a 556 carateres.

- As etiquetas aplicadas ao grupo de recursos não são herdadas pelos recursos nesse grupo de recursos.

Se tiver mais de 15 valores que têm de ser associados a um recurso, utilize uma cadeia JSON para o valor da etiqueta. A cadeia de caracteres do JSON pode conter muitos valores que são aplicados a uma chave de etiqueta única.

#### <a name="tags-for-billing"></a>Etiquetas de faturação

As etiquetas permitem-lhe agrupar os dados de faturas. Por exemplo, se estiver a executar várias VMs para organizações diferentes, utilize etiquetas para a utilização do grupo pelo centro de custos. Também pode utilizar etiquetas para categorizar os custos ao ambiente de tempo de execução, como a utilização de faturação para VMs em execução no ambiente de produção.

Pode obter informações sobre etiquetas através do [utilização de recursos do Azure e RateCard APIs](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview) ou o ficheiro de valores separados por vírgulas (CSV) de utilização. Transferir o ficheiro de utilização a partir do [portal de contas do Azure](https://account.windowsazure.com/) ou o [portal EA](https://ea.azure.com/).

Para obter mais informações sobre o acesso programático a informações de faturação, consulte [obter informações sobre o consumo de recursos do Microsoft Azure](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview). Para operações de REST API, consulte [referência de API de REST de faturação do Azure](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).

Ao baixar a utilização de CSV para serviços que suportam etiquetas com a faturação, as etiquetas são apresentadas na coluna de etiquetas.

### <a name="critical-resource-controls"></a>Controles de recurso crítico

À medida que sua organização adiciona serviços principais para a subscrição, torna-se mais importante garantir que esses serviços estão disponíveis para evitar a interrupção do negócio. [Bloqueios de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) permitem-lhe restringir as operações nos recursos de alto valor onde modificar ou eliminá-los seria ter um impacto significativo em seus aplicativos ou a infraestrutura de nuvem. Pode aplicar bloqueios para uma subscrição, grupo de recursos ou recurso. Normalmente, aplicar bloqueios para recursos fundamentais, como redes virtuais, gateways e as contas de armazenamento.

Bloqueios de recursos atualmente suportam dois valores: **CanNotDelete** e **só de leitura**. **CanNotDelete** significa que os utilizadores (com os direitos adequados), ainda podem ler ou modificar um recurso, mas não é possível eliminá-lo. **Só de leitura** significa que os utilizadores autorizados não é possível eliminar ou modificar um recurso.

Bloqueios de recursos aplicam-se apenas às operações que ocorrem no plano de gestão, que consiste em operações enviadas para <https://management.azure.com>. Os bloqueios não restringem como recursos executam suas próprias funções. As alterações de recursos são restritas, mas as operações de recursos não estão restritas. Por exemplo, um **só de leitura** bloqueio na base de dados SQL impede-o de eliminem ou modifiquem o banco de dados, mas ele não o impede de criar, atualizar ou eliminar dados na base de dados.

Aplicando **só de leitura** pode levar a resultados inesperados, uma vez que algumas operações que parecem ler operações exigem ações adicionais. Por exemplo, colocar uma **só de leitura** bloqueio numa conta de armazenamento impede que todos os utilizadores de listar as chaves. A operação de listagem de chaves é manipulada por meio de um pedido POST, porque as chaves retornadas estão disponíveis para operações de escrita.

![Controles de recurso crítico](./media/governance-in-azure/security-governance-in-azure-fig5.png)

Por outro exemplo, colocar uma **só de leitura** bloqueio num recurso do App Service do Azure impede que o Visual Studio Server Explorer de apresentar ficheiros para o recurso porque essa interação requer acesso de escrita.

Ao contrário do controlo de acesso baseado em funções, usar os bloqueios de gestão para aplicar uma restrição em todos os utilizadores e funções. Para saber mais sobre como definir permissões, veja [gerir o acesso com RBAC e o portal do Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).

Quando aplica um bloqueio num âmbito principal, todos os recursos dentro daquele escopo herdam o mesmo bloqueio. Até mesmo recursos que adicione mais tarde herdam o bloqueio do pai. O bloqueio mais restritivo na herança tem precedência.

Para criar ou eliminar bloqueios de gestão, tem de ter acesso a ações Microsoft.Authorization/ ou Microsoft.Authorization/locks/. Das funções incorporadas, apenas o proprietário e o administrador de acesso de utilizador são concedidos dessas ações.

### <a name="api-access-to-billing-information"></a>Acesso à API para informações de faturação

Utilize APIs de faturação do Azure para extrair dados de utilização e de recursos em suas ferramentas de análise de dados preferencial. A utilização de recursos do Azure e RateCard APIs podem ajudá-lo a prever e gerir os seus custos com precisão. As APIs são implementadas como um fornecedor de recursos e a parte da família de APIs expostas pelo Azure Resource Manager.

#### <a name="resource-usage-api-preview"></a>Utilização de recursos da API (pré-visualização)

Utilizar o Azure [API de utilização de recursos](https://msdn.microsoft.com/library/azure/mt219003) para colocar os seus dados de consumo do Azure estimado. A API inclui:

- **RBAC**: Configurar políticas de acesso no [portal do Azure](https://portal.azure.com/) ou através de [cmdlets do Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) para especificar quais os utilizadores ou aplicações podem obter acesso a dados de utilização da subscrição. Os autores de chamadas tem de utilizar tokens padrão do Azure Active Directory para autenticação. Adicione o autor da chamada à função seja o leitor de faturação, leitor, proprietário ou Contribuidor para obter acesso a dados de utilização de uma subscrição do Azure específica.

- **Agregações de horas ou diárias**: chamadores podem especificar se pretendem os dados de utilização do Azure em incrementos de hora a hora ou diariamente. A predefinição é diário.

- **Metadados de instância (inclui as etiquetas de recursos)**: obter detalhes de nível de instância, como o URI de recurso completamente qualificado (/subscriptions/ {subscrição-id} /...), informações do grupo de recursos e as etiquetas de recursos. Estes metadados ajudam-o a forma determinista e por meio de programação alocar utilização por etiquetas, para casos de utilização como a cobrança de consumo entre.

- **Metadados do recurso**: detalhes do recurso, como o nome do medidor, categoria do medidor, subcategoria do medidor, unidade e a região dar o autor da chamada uma melhor compreensão sobre o que foi consumido. Estamos também a trabalhar para alinhar a terminologia de metadados do recurso em todo o portal do Azure, utilização do Azure no. CSV, EA CSV e outras experiências de destinado ao público, para o ajudar a correlacionar dados em experiências de faturação.

- **Utilização para todos os tipos de oferta**: dados de utilização estão disponíveis para todos os tipos, incluindo o pay as you go, o MSDN, o compromisso monetário, o crédito monetário e o EA da oferta.

#### <a name="resource-ratecard-api"></a>API do recurso RateCard

Utilize a API de RateCard de recursos do Azure para obter a lista de recursos do Azure disponíveis e informações de preços estimadas para cada um. A API inclui:

- **RBAC**: configurar as políticas de acesso no portal do Azure ou através de cmdlets do Azure PowerShell para especificar quais os utilizadores ou aplicações, podem obter acesso aos dados RateCard. Os autores de chamadas tem de utilizar tokens padrão do Azure Active Directory para autenticação. Adicione o autor da chamada à função de leitor, proprietário ou Contribuidor para obter acesso a dados de utilização para uma subscrição específica do Azure.

- **Suporte para pay as you go, MSDN, alocação monetária e crédito monetário ofertas (mas não EA)**: Esta API fornece as informações de taxas de nível de oferta do Azure. O autor da chamada desta API têm de introduzir a informações sobre as ofertas para obter detalhes do recurso e taxas. EA atualmente não é suportado porque as ofertas EA personalizaram taxas por inscrição. 

#### <a name="scenarios"></a>Cenários

A combinação da utilização e RateCard APIs torna esses cenários possíveis:

- **Compreender o Azure gastar durante o mês**: Utilize a combinação da utilização de e APIs RateCard para obter melhores informações sobre a sua cloud gastar durante o mês. Pode analisar a hora a hora e diariamente estimativas de utilização e custos.

- **Configure alertas**: utilizar as APIs de RateCard e de utilização para obter o consumo na cloud estimado e os encargos e posso configurar alertas com base em recursos ou monetário com base em.

- **Prever a fatura**: Get seu consumo estimado e a cloud de gastos em aplicam algoritmos de machine learning para prever o que a cobrar seria no final do ciclo de faturação.

- **Executar um pré-consumo de análise de custo**: utilizar a API de RateCard para prever quanto será para a sua utilização esperada ao mover as cargas de trabalho para o Azure. Se tiver cargas de trabalho existentes em outras clouds ou nuvens privadas, também pode mapear a utilização com o Azure gastar de taxas para obter uma melhor estimativa do Azure. Esta estimativa dá-lhe a capacidade de dinamizar na oferta e comparar entre os tipos de oferta diferente, além de pay as you go, como um compromisso e o crédito monetário.

- **Executar uma análise de hipóteses**: pode determinar se é mais rentável para executar cargas de trabalho em outra região ou em outra configuração do recurso do Azure. Os custos de recursos do Azure podem divergir com base na região do Azure que está a utilizar. Também pode determinar se o outro tipo de oferta do Azure fornece uma melhor taxa sobre um recurso do Azure.

### <a name="networking-controls"></a>Controles de sistema de rede

Acesso a recursos pode ser interno (dentro da rede a corporation) ou externos (por meio da internet). É fácil para os utilizadores na sua organização inadvertidamente colocar recursos no ponto errado e potencialmente abri-los para o acesso malicioso. Tal como acontece com dispositivos no local, as empresas tem de adicionar controlos adequados para garantir que os utilizadores do Azure tomem as decisões certas.

![Controles de sistema de rede](./media/governance-in-azure/security-governance-in-azure-fig6.png)

Para governação de subscrições, os seguintes recursos de núcleo fornecem controle básico de acesso.

#### <a name="network-connectivity"></a>Conectividade de rede

[Redes virtuais](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) são objetos de contentor para sub-redes. Embora não seja estritamente necessário, uma rede virtual é frequentemente utilizada para ligar aplicações aos recursos empresariais internos. O serviço de rede Virtual do Azure permite-lhe ligar em segurança a recursos do Azure entre si com as redes virtuais.

Uma rede virtual é uma representação da sua própria rede na cloud. Uma rede virtual é um isolamento lógico da cloud do Azure dedicado à sua subscrição. Também pode ligar redes virtuais à sua rede no local.

Seguem-se os recursos de redes virtuais do Azure:

- **Isolamento**: as redes virtuais são isoladas uns dos outros. Pode criar redes virtuais separadas para desenvolvimento, teste e produção que utilizam os mesmos blocos de endereços CIDR. Por outro lado, pode criar várias redes virtuais que utilizam diferentes blocos de endereços CIDR e ligar a redes em conjunto. Pode segmentar uma rede virtual em várias sub-redes. O Azure proporciona a resolução de nomes interna para instâncias de função VMs e serviços Cloud do Azure que estão ligadas a uma rede virtual. Opcionalmente, pode configurar uma rede virtual para utilizar seus próprios servidores DNS, em vez de utilizar a resolução do nome interno do Azure.

- **Conectividade com a Internet**: todas as máquinas virtuais e Azure instâncias de função dos serviços Cloud que estão ligadas a uma rede virtual tem acesso à internet, por predefinição. Também pode ativar o acesso de entrada para recursos específicos, conforme necessário.

- **Conectividade de recursos do Azure**: pode ligar recursos do Azure, como serviços Cloud e VMs, a mesma rede virtual. Os recursos podem ligar-se entre si através de endereços IP privados, mesmo que estejam em sub-redes diferentes. O Azure fornece encaminhamento predefinido entre sub-redes, redes virtuais e redes no local, para que não tenha de configurar e gerir rotas.

- **Conectividade de rede virtual**: pode ligar redes virtuais entre si. Recursos que estão ligados a nenhuma rede virtual, em seguida, podem comunicar com qualquer recurso em qualquer outra rede virtual.

- **Conectividade no local**: ligar redes virtuais em redes no local por meio de conexões de rede privada entre a rede e do Azure ou através de uma ligação de rede privada virtual (VPN) site a site através da internet.

- **Filtragem de tráfego**: pode filtrar o tráfego de rede (entrado e saído) para máquinas virtuais e serviços em nuvem por endereço IP de origem e porta, endereço IP de destino e porta e protocolo.

- **Encaminhamento**: Opcionalmente, pode substituir o encaminhamento do Azure ao configurar as sua próprias rotas ou através de rotas BGP através de um gateway de rede de predefinição.

#### <a name="network-access-controls"></a>Controlos de acesso de rede

[Grupos de segurança de rede](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (NSGs) são, como uma firewall e fornecer regras para como um recurso pode "falar" através da rede. Eles fornecem controle sobre como uma sub-rede (ou máquinas virtuais) podem ligar à internet ou outras sub-redes na mesma rede virtual.

Um grupo de segurança de rede contém uma lista de regras de segurança que permitem ou negam o tráfego de rede para recursos ligados a redes virtuais do Azure. Os NSGs podem ser associados a sub-redes, VMs individuais (clássicas) ou interfaces de rede individuais (NIC) ligadas a VMs (Resource Manager).

Quando um NSG é associado uma sub-rede, as regras são aplicadas a todos os recursos ligados à sub-rede. Pode restringir mais o tráfego ao associar um NSG a uma VM ou NIC.

## <a name="security-and-compliance-with-organizational-standards"></a>Segurança e conformidade com as normas organizacionais

Cada empresa tem necessidades diferentes e será aproveitam as vantagens de soluções na cloud. Ainda assim, os clientes de todos os tipos têm os mesmo conceitos básicos sobre como mover para a cloud. O que os clientes querem de fornecedores de cloud é:

- **Proteger os nossos dados**: os líderes de TI reconhece que a cloud pode proporcionar dados de maior segurança e controlo administrativo. Mas eles são ainda preocupados que migrar para a cloud irá deixá-los mais vulnerável a hackers do que suas atuais soluções internas.

- **Manter os nossos dados privada**: serviços Cloud criam desafios de privacidade únicos. Como as empresas a olhar para a cloud para poupar nos custos de infraestrutura e melhorar a respetiva flexibilidade, eles também preocupar com a perda de controle de onde os dados são armazenados, quem está a aceder e como ele é usado.

- **Envie-no controle**:, mesmo que as empresas tirar partido da cloud para implementar mais soluções inovadoras, que estão preocupados pela perda de controlo dos seus dados. As recentes divulgações de organismos públicos aceder aos dados dos clientes, por meios extralegal e legais, fazer alguns CIOs cautela armazenar seus dados na cloud.

- **Promover a transparência**: tomadores de decisão de negócios compreendam a importância da segurança, privacidade e controlo. Mas também querem a capacidade de forma independente verificar como seus dados estão a ser armazenados, acedidos e protegidos.

- **Manter a conformidade**: conforme as empresas expandir o uso das tecnologias de cloud, a complexidade e o escopo de normas e regulamentos continuam a evoluir. As empresas precisam de saber que atinge os seus padrões de conformidade.

## <a name="security-configuration-for-monitoring-logging-and-auditing"></a>Configuração de segurança para monitorização, registo e auditoria

Os subscritores do Azure podem gerir os respetivos ambientes de cloud de vários dispositivos. Estes dispositivos podem incluir estações de trabalho de gestão, PCs de programadores e os dispositivos de utilizador final, mesmo com privilégios que tenham permissões específicas de tarefas. 

Em alguns casos, as funções administrativas são realizadas através das consolas baseadas na web, como o portal do Azure. Em outros casos, podem existir ligações diretas para o Azure desde sistemas no local através de VPNs, os serviços de Terminal, protocolos de aplicação de cliente ou (através de programação) a API de gestão de serviço do Azure (SMAPI). Além disso, os pontos finais de cliente podem ser um domínio associado ou isolado e não geridas, como tablets ou smartphones.

Esta variabilidade pode acarretar um risco significativo para uma implementação na cloud. Pode ser difícil gerir, controlar e auditar as ações administrativas. Este variabilidade também pode introduzir a ameaças de segurança através do acesso não regulado para pontos de extremidade do cliente que são utilizados para gerir serviços cloud. A utilização das estações de trabalho gerais ou pessoais para desenvolver e gerir a infraestrutura abre vetores de ameaças imprevisíveis, como navegação na Web (por exemplo, ataques de tipo “watering hole”) ou e-mail (engenharia social e phishing).

Monitorização, registo e auditoria fornecem uma base para controlar e compreender as atividades administrativas. Auditoria de todas as ações em detalhe nem sempre é viável devido à quantidade de dados gerados. Mas a auditoria da eficácia das políticas de gestão é uma prática recomendada.

Governação de segurança do Azure de GPOs do Azure Active Directory Domain Services (AD DS) pode ajudar a controlar interfaces do Windows dos administradores, como a partilha de ficheiros. Inclua estações de trabalho de gestão em monitorização, registo e auditoria de processos. Controle todos os acessos e utilizações de administrador e programador.

### <a name="azure-security-center"></a>Centro de Segurança do Azure

[Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-intro) fornece uma visão central do Estado de segurança de recursos nas subscrições. Fornece recomendações que ajudam a evitar recursos comprometidos. Ele pode ativar mais detalhadas políticas – por exemplo, aplicar políticas a grupos de recursos específicos que permitem à empresa personalizar a sua postura ao risco que estão a resolver.

![Centro de Segurança do Azure](./media/governance-in-azure/security-governance-in-azure-fig7.png)

Centro de segurança fornece gestão de políticas e monitorização de segurança integrada nas suas subscrições do Azure, ajuda a detetar ameaças que caso contrário podem passar despercebidas e funciona com um ecossistema abrangente de soluções de segurança. Depois de ativar [políticas de segurança](https://docs.microsoft.com/azure/security-center/security-center-policies) para os recursos de uma subscrição, o Centro de segurança analisa a segurança dos seus recursos para identificar potenciais vulnerabilidades. As informações sobre a configuração da rede estão disponíveis de forma instantânea.

Centro de segurança do Azure representa uma combinação de melhor prática segurança e de análise de gestão de políticas para todos os recursos dentro de uma subscrição do Azure. Permite que as equipes de segurança e Diretores de risco para prevenir, detetar e responder a ameaças de segurança, pois coleta e analisa dados de segurança dos seus recursos do Azure, rede e soluções de parceiros, como programas antimalware e firewalls automaticamente.

Além disso, o Centro de segurança do Azure aplica análises avançadas, incluindo o machine learning e análise comportamental. Utiliza informações sobre ameaças globais da Microsoft produtos e serviços, a Microsoft Digital Crimes unidade (DCU), o Microsoft Security Response Center (MSRC) e feeds externos. Pode aplicar [governação de segurança](https://www.credera.com/blog/credera-site/azure-governance-part-4-other-tools-in-the-toolbox/) amplamente ao nível da subscrição. Em alternativa, pode restringi-la para baixo para requisitos específicos e aplicá-las para recursos individuais através da definição de política.

Por fim, o Centro de segurança do Azure analisa o estado de funcionamento do segurança de recursos com base nessas políticas e usa essas informações para fornecer dashboards elucidativos e os alertas para eventos, como deteção de software maligno ou a ligação de IP maliciosa tentativas. Para obter mais informações sobre como aplicar recomendações, veja [implementar recomendações de segurança no Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-recommendations).

Centro de segurança do Azure monitoriza os seguintes recursos do Azure:

- Máquinas virtuais (VMs) (incluindo os serviços cloud)

- Redes virtuais

- Bases de dados SQL

- Parceiros de soluções integradas com a sua subscrição do Azure, como uma firewall de aplicações web em VMs e, no [ambiente de serviço de aplicações](https://docs.microsoft.com/azure/app-service/app-service-app-service-environments-readme)

Quando aceder primeiro ao centro de segurança, a recolha de dados é ativada em todas as máquinas virtuais na sua subscrição. Recomendamos que mantenha a recolha de dados ativada, mas pode [desativá-la](https://docs.microsoft.com/azure/security-center/security-center-faq) na política do Centro de segurança.

### <a name="log-analytics"></a>Log Analytics

Segurança de informações do Log Analytics do Azure software development e o serviço da Equipe e [programa de governação](https://github.com/Microsoft/azure-docs/blob/master/articles/log-analytics/log-analytics-security.md) oferece suporte a seus requisitos de negócios. Ele respeita as leis e regulamentos conforme descrito em [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) e [Microsoft Trust Center conformidade](https://www.microsoft.com/TrustCenter/Compliance/default.aspx). Como o Log Analytics estabelece os requisitos de segurança, identifica os controlos de segurança e gere e os riscos de monitores também está descrita aqui. Anualmente, a equipe analisa as políticas, normas, procedimentos e diretrizes.

Cada membro de equipe de desenvolvimento do Log Analytics recebe treinamento de segurança de aplicativo formal. Um sistema de controle de versão ajuda a proteger a cada projeto de software em desenvolvimento.

A Microsoft tem uma equipa de segurança e conformidade que supervisiona e avalia todos os serviços Microsoft. Responsáveis pela segurança de informações compõem a equipe, e eles não são associados com os departamentos de engenharia que desenvolver do Log Analytics. Os responsáveis pela segurança tem sua própria cadeia de gestão. Eles conduzir independentes avaliações de produtos e serviços para ajudar a garantir a segurança e conformidade.

A funcionalidade principal do Log Analytics é fornecida por um conjunto de serviços que são executados no Azure. Cada serviço proporciona uma função de gestão específica e pode combinar serviços para alcançar cenários de gestão diferentes.

![Serviços do Azure para gestão](./media/governance-in-azure/security-governance-in-azure-fig9.JPG)

Estes serviços de gestão foram criados na cloud. Totalmente estão alojados no Azure, para que elas não envolverem implementar e gerir recursos no local. Configuração é mínima e pode estar em execução numa questão de minutos.

Coloque um agente em qualquer computador Windows ou Linux no seu datacenter e este enviará dados ao Log Analytics. Lá, podem ser analisados, juntamente com todos os outros dados recolhidos a partir de serviços cloud ou no local. Utilize a cópia de segurança do Azure e o Azure Site Recovery para tirar partido da cloud para cópias de segurança e a elevada disponibilidade para recursos no local.

![Serviços de gestão no dashboard do Azure](./media/governance-in-azure/security-governance-in-azure-fig8.png)

Runbooks na cloud, normalmente, não é possível aceder aos recursos no local, mas pode instalar um agente num ou mais computadores que alojarão os runbooks no seu datacenter. Quando inicia um runbook, especifica se pretende que seja executado na cloud ou numa função de trabalho local.

Soluções diferentes estão disponíveis da Microsoft e de parceiros, que pode adicionar à sua subscrição do Azure para aumentar o valor do seu investimento no Log Analytics. Por exemplo, o Azure oferece [soluções de gestão](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solutions)– pré-empacotados conjuntos de lógica que implementam um cenário de gestão, utilizando um ou mais serviços de gestão.

![Galeria de soluções de gestão do Azure](./media/governance-in-azure/security-governance-in-azure-fig10.png)

Como parceiro, pode criar suas próprias soluções para seus aplicativos e serviços de suporte e disponibilizá-las aos utilizadores através de modelos do Azure Marketplace ou do início rápido.

## <a name="performance-alerting-and-monitoring"></a>Alertas de desempenho e monitoramento

### <a name="alerting"></a>Alertas

Os alertas são um método de métricas de monitorização de recursos do Azure, eventos ou registos. Eles notificá-lo quando for cumprida uma condição que tiver especificado.

Alertas estão disponíveis em serviços, incluindo:

- **O Azure Application Insights**: permite que os alertas de métrica e teste da web. Para obter mais informações, consulte [definir alertas no Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-alerts) e [monitorizar a disponibilidade e capacidade de resposta de qualquer Web site](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability).

- **Log Analytics**: permite que o encaminhamento de atividade e os registos de diagnóstico para o Log Analytics. Ele permite que as métricas, registos e outros tipos de alerta. Para obter mais informações, consulte [alertas no Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts).

- **O Azure Monitor**: permite que os alertas com base nos valores de métrica e eventos de registo de atividade. Pode utilizar o [API de REST do Azure Monitor](https://msdn.microsoft.com/library/dn931943.aspx) gerir alertas. Para obter mais informações, consulte [utilizar o portal do Azure, PowerShell ou a interface de linha de comandos para criar alertas](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal).

### <a name="monitoring"></a>Monitorização

Problemas de desempenho na sua aplicação na cloud podem afetar o seu negócio. Com vários componentes interconectados que têm e freqüentes lançamentos, degradações podem acontecer em qualquer altura. E se estiver a desenvolver uma aplicação, os usuários normalmente Descubra problemas que não foram encontrados no teste. Deve conhecer esses problemas imediatamente e ter as ferramentas para diagnosticar e corrigi-los.

Há uma grande variedade de ferramentas para a monitorização de aplicações do Azure e serviços. Alguns dos seus recursos se podem sobrepor. Trata-se em parte devido a desfocar entre o desenvolvimento e o funcionamento de um aplicativo.

Aqui estão as ferramentas de principal:

- **O Azure Monitor** é uma ferramenta básica para monitorizar os serviços em execução no Azure. Ele fornece dados ao nível da infraestrutura relativamente ao débito de um serviço e o ambiente. Se estiver a gerir todas as suas aplicações no Azure e decidir se deve aumentar ou reduzir recursos verticalmente, do Azure Monitor pode ajudá-lo a começar.

- **O Application Insights** podem ser utilizados para desenvolvimento e como uma solução de monitorização de produção. Funciona ao instalar um pacote na sua aplicação, para que ele oferece uma visão mais interna do que está acontecendo. Os dados incluem tempos de resposta das dependências, rastreios de exceção, a depuração de instantâneos e perfis de execução. Ele fornece ferramentas para analisar todos os esta telemetria para o ajudar a depurar uma aplicação e para ajudar a compreender o que os utilizadores estão a fazer com ele. Pode dizer se um pico em tempos de resposta é devido à colocação num aplicativo ou algum problema relativo a recursos externo. Se utilizar o Visual Studio e a aplicação está com defeito, vai diretamente para a linha de problema de código para que pode corrigi-lo.

- **Log Analytics** é para aqueles que precisam de otimizar o desempenho e planear a manutenção em aplicativos em execução na produção. Coleta e agrega dados de várias origens, com um atraso de 10 a 15 minutos. Ele fornece uma solução de gestão de TI holística para o Azure, no local e a infraestrutura baseada na cloud de terceiros (por exemplo, o Amazon Web Services). Ele fornece ferramentas para analisar dados de origens, permite consultas complexas em todos os registos e pode alertar pró-ativamente nas condições especificadas. Pode até mesmo recolher dados personalizados no seu repositório central e, em seguida, consultar e visualizar dados.

- **O System Center Operations Manager** é para gerir e monitorizar as instalações de cloud de grandes dimensões. Poderá já estar familiarizado com ele, como uma ferramenta de gestão para o Windows Server no local e Hyper-V com base em nuvens, mas também pode integrar e gerir aplicações do Azure. Entre outras coisas, ele pode instalar o Application Insights nas aplicações em direto existentes. Se uma aplicação ficar inativo, o Operations Manager indica em segundos.


## <a name="next-steps"></a>Passos Seguintes

- [Melhores práticas para criar modelos do Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices)

- [Exemplos para implementar a governação de subscrições do Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-subscription-examples)

- [Microsoft Azure Government](https://docs.microsoft.com/azure/azure-government/)
