---
title: Configuração de conectividade de máquinas virtuais para o SAP HANA no Azure (instâncias grandes) | Documentos da Microsoft
description: Configuração de conectividade de máquinas virtuais e utilizar o SAP HANA no Azure (instâncias grandes).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f0e09e6dfce5d0ede525f461193866219b7f9429
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44164756"
---
# <a name="connecting-azure-vms-to-hana-large-instances"></a>Ligar VMs do Azure para instâncias grandes do HANA

Como já foi mencionado no [descrição geral do SAP HANA (instâncias grandes) e a arquitetura no Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) como a implementação mínima de instâncias grandes do HANA com a camada de aplicação SAP do Azure é:

![VNet do Azure ligado ao SAP HANA no Azure (instâncias grandes) e no local](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

Mais cuidadosa do lado de VNet do Azure, percebemos que a necessidade de:

- A definição de uma VNet do Azure que vai ser utilizado para implementar as VMs da camada de aplicação SAP em.
- Que automaticamente significa que uma sub-rede de predefinição na Vnet do Azure é definido que realmente é utilizado para implementar as VMs em.
- A VNet do Azure que é criada tem de ter pelo menos uma sub-rede VM e uma sub-rede de Gateway do ExpressRoute. Estas sub-redes devem ser atribuídas os intervalos de endereços IP como discutido e especificados nas seções a seguir.

Então, vamos examinar mais detalhadamente a criação da VNet do Azure para instâncias grandes do HANA

## <a name="creating-the-azure-vnet-for-hana-large-instances"></a>Criar a VNet do Azure para instâncias grandes do HANA

>[!Note]
>A VNet do Azure para instâncias grandes do HANA tem de ser criada com o modelo de implementação Azure Resource Manager. O modelo de implementação do Azure mais antigo, comumente conhecido como modelo de implementação clássica, não é suportado com a solução de instância grande do HANA.

A VNet pode ser criada com o portal do Azure, o PowerShell, o modelo do Azure ou a CLI do Azure (veja [criar uma rede virtual com o portal do Azure](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network)). O exemplo a seguir, vamos ver numa VNet criada através do portal do Azure.

Se dermos uma olhada em definições de uma VNet do Azure através do portal do Azure, vamos dar uma olhada em algumas das definições e como aqueles se relaciona com o que estamos a lista de IP diferente intervalos de endereços. Como estamos a falar sobre o **espaço de endereços**, queremos dizer que o espaço de endereços VNet do Azure está autorizada a utilizar. Este espaço de endereço também é o intervalo de endereços VNet utiliza para a propagação de rotas do BGP. Isso **espaço de endereços** pode ser visto aqui:

![Endereço espaço de VNet do Azure apresentado no portal do Azure](./media/hana-overview-connectivity/image1-azure-vnet-address-space.png)

No caso anterior, com 10.16.0.0/16, a VNet do Azure foi indicada um intervalo de endereços IP em vez disso, grande e amplo para utilizar. Significa que todos os intervalos de endereços IP das sub-redes subsequentes nesta VNet podem ter seus intervalos dentro do que "espaço de endereços". Normalmente, que não recomendamos um intervalo de endereço amplo para VNet individual no Azure. Mas a obtenção de um passo adiante, vamos ver as sub-redes definidas na VNet do Azure:

![Intervalos de endereços de sub-redes de VNet do Azure e o IP](./media/hana-overview-connectivity/image2b-vnet-subnets.png)

Como pode ver, vamos ver uma VNet com uma sub-rede VM primeiro (denominada "predefinido") e uma sub-rede denominada "GatewaySubnet".
Na secção seguinte, para o intervalo de endereços IP da sub-rede, que foi chamado de "predefinição" em gráficos como nos Referimos **intervalo de endereços IP de sub-rede de VM do Azure**. Nas seções a seguir, fazemos referência para o intervalo de endereços IP da sub-rede do Gateway como **intervalo de endereços IP de sub-rede de Gateway de VNet**. 

No caso demonstrado pelos dois gráficos acima, verá que o **espaço de endereços VNet** abrange ambos os casos, o **intervalo de endereços IP de sub-rede de VM do Azure** e o **intervalo de endereços IP de sub-rede de Gateway de VNet**. 

Em outros casos em que tem de conservar ou ser específico com os intervalos de endereços IP, pode restringir os **espaço de endereços VNet** de uma VNet para os intervalos específicos a ser utilizado por cada sub-rede. Neste caso, pode definir o **espaço de endereços VNet** de uma VNet específicas como vários intervalos como mostrado aqui:

![Espaço de endereços do VNet do Azure com dois espaços](./media/hana-overview-connectivity/image3-azure-vnet-address-space_alternate.png)

Neste caso, o **espaço de endereços VNet** tem dois espaços definidos. Estes dois espaços, são idênticos para os intervalos de endereços IP definidos para **intervalo de endereços IP de sub-rede de VM do Azure** e o **intervalo de endereços IP de sub-rede de Gateway de VNet.**

Pode usar qualquer padrão de nomenclatura que quiser para estas sub-redes de inquilino (sub-redes VM). No entanto, **deve sempre haver um e apenas uma sub-rede do gateway para cada VNet** que liga-se para o SAP HANA no circuito do ExpressRoute do Azure (instâncias grandes). E **esta sub-rede de gateway deve sempre ter o nome "GatewaySubnet"** para garantir que a localização correta do gateway do ExpressRoute.

> [!WARNING] 
> É essencial que a sub-rede do gateway sempre tem o nome "GatewaySubnet".

Várias sub-redes VM podem ser utilizadas, até mesmo utilizando intervalos de endereços não contínuos. Mas como mencionado anteriormente, estes intervalos de endereço devem ser cobertos pela **espaço de endereços VNet** da VNet, de forma agregada ou numa lista dos intervalos exatos de sub-redes VM e a sub-rede do gateway.

Resumir o fato importante sobre uma VNet do Azure que se liga a instâncias grandes do HANA:

- Tem de submeter para a Microsoft a **espaço de endereços VNet** quando efetuar uma implementação inicial de instâncias grandes do HANA. 
- O **espaço de endereços VNet** pode ser um intervalo maior que abrange o leque de intervalos de endereços IP de sub-rede de VM do Azure e o intervalo de endereços IP de sub-rede de Gateway de VNet.
- Ou pode enviar como **espaço de endereços VNet** intervalos de intervalos de endereços IP de sub-rede VM e o intervalo de endereços IP de sub-rede de Gateway de VNet de endereços de vários intervalos que abrangem o IP diferente.
- O definido **espaço de endereços VNet** é utilizada propagação de encaminhamento de BGP.
- O nome da sub-rede do Gateway tem de ser: **"GatewaySubnet".**
- O **espaço de endereços VNet** é utilizado como um filtro no lado do instância grande do HANA para permitir ou não o tráfego para as unidades de instância grande do HANA a partir do Azure. Se as informações de encaminhamento de BGP de VNet do Azure e os intervalos de endereços IP configurados para filtragem do lado de instância grande do HANA não corresponderem, podem surgir problemas de conectividade.
- Existem alguns detalhes sobre a sub-rede do Gateway que são abordadas adiante, no secção 'Ligar uma VNet para ExpressRoute de instância grande do HANA'



## <a name="different-ip-address-ranges-to-be-defined"></a>Intervalos de endereços IP diferentes a definir 

Introduzimos já alguns dos intervalos de endereços IP necessários para implementar as instâncias grandes do HANA nas secções anteriores. Mas há algumas mais intervalos de endereços IP, o que são importantes. Vamos percorrer alguns detalhes adicionais. Os seguintes endereços IP que nem todos precisam para serem submetidas para a Microsoft tem de ser definida antes de enviar um pedido para a implementação inicial:

- **Espaço de endereços da VNet:** conforme já apresentado anteriormente, é ou são os intervalos que atribuiu de endereço IP (ou plano para atribuir) para o parâmetro de espaço de endereço na ligar redes virtuais do Azure (VNet) no ambiente de instância grande do SAP HANA. Recomenda-se que este parâmetro de espaço de endereços é um valor de com várias linha composto de intervalos de sub-rede VM do Azure e o intervalo de sub-rede de Gateway do Azure, como mostrado anteriormente nos elementos gráficos. Este intervalo não pode sobrepor seu no local ou intervalos de endereços do conjunto de IP do servidor ou ER P2P. Como obter esta ou estas intervalos de endereços IP? O fornecedor de equipa ou o serviço de rede empresarial deve fornecer um ou vários intervalos de endereços de IP, que é ou não são utilizados no interior da rede. Exemplo: Se a sub-rede de VM do Azure (consulte anteriormente) é 10.0.1.0/24 e a sub-rede de Gateway do Azure (veja a seguir) for 10.0.2.0/28, em seguida, seu espaço de endereços VNet do Azure é recomendado para ser duas linhas; 10.0.1.0/24 e 10.0.2.0/28. Embora os valores de espaço de endereços podem ser agregados, é recomendado correspondentes-los para os intervalos de sub-rede para evitar a reutilização acidental de intervalos de endereços IP não utilizados dentro de espaços de endereços maiores no futuro noutro local na sua rede. **O espaço de endereços da VNET é um intervalo de endereços IP, que tem de ser submetidas à Microsoft quando pedir para uma implementação inicial**

- **Intervalo do endereço IP de sub-rede do Azure VM:** intervalo de endereços IP este, como discutido anteriormente já, é aquele que atribuiu (ou se pretende atribuir) para o parâmetro de sub-rede da VNet do Azure na sua VNET do Azure, ligar para o ambiente de instância grande do SAP HANA. Este intervalo de endereços IP é utilizado para atribuir endereços IP às VMs do Azure. Os endereços IP fora deste intervalo têm permissão para ligar ao seu servidor de instância grande do SAP HANA (es). Se for necessário, podem ser utilizadas várias sub-redes VM do Azure. R/24 bloco CIDR é recomendado pela Microsoft para cada sub-rede de VM do Azure. Este intervalo de endereços tem de ser uma parte dos valores utilizados no espaço de endereços VNet do Azure. Como obter esse intervalo de endereços IP? O fornecedor de equipa ou o serviço de rede empresarial deve fornecer um intervalo de endereços IP que não é atualmente utilizado no interior da rede.

- **Intervalo de endereços IP de sub-rede de Gateway de VNet:** dependendo dos recursos que pretende utilizar, o tamanho recomendado seria:
   - Gateway ExpressRoute de ultra desempenho: / 26 bloco de endereços - necessário para a classe de tipo II dos SKUs
   - Coexistência com o ExpressRoute através de um Gateway do ExpressRoute de elevado desempenho e de VPN (ou menor): / 27 bloco de endereços
   - Todas as outras situações: / 28 bloco de endereços. Este intervalo de endereços tem de ser uma parte dos valores utilizados os valores de "Espaço de endereços da VNet". Este intervalo de endereços tem de ser uma parte dos valores utilizados os valores de espaço de endereços do VNet do Azure que tem de submeter para a Microsoft. Como obter esse intervalo de endereços IP? O fornecedor de equipa ou o serviço de rede empresarial deve fornecer um intervalo de endereços IP que não é atualmente utilizado no interior da rede. 

- **Intervalo para a conectividade de ER P2P de endereços:** esse intervalo é o intervalo IP para a sua ligação de SAP HANA grandes instância ExpressRoute (ER) P2P. Este intervalo de endereços IP tem de ser/29 intervalo de endereços IP de CIDR. Este intervalo não pode sobrepor com no local ou outro IP Azure intervalos de endereços. Este intervalo de endereços IP é utilizado para configurar a conectividade de ER do Gateway do ExpressRoute de VNet do Azure para os servidores de instância grande do SAP HANA. Como obter esse intervalo de endereços IP? O fornecedor de equipa ou o serviço de rede empresarial deve fornecer um intervalo de endereços IP que não é atualmente utilizado no interior da rede. **Esse intervalo é um intervalo de endereços IP, que tem de ser submetidas à Microsoft quando pedir para uma implementação inicial**
  
- **Intervalo de endereços do conjunto IP do servidor:** este intervalo de endereços IP é usado para atribuir o endereço IP individual para servidores de instâncias grandes do HANA. O tamanho da sub-rede recomendada é um /24 CIDR bloquear - mas, se for necessário pode ser mais pequena para um mínimo de fornecer 64 endereços IP. Deste intervalo, os 30 primeiros endereços IP estão reservados para utilização pela Microsoft. Certifique-se de que esse fato é contabilizado ao escolher o tamanho do intervalo. Este intervalo não pode sobrepor com no local ou outro IP Azure endereços. Como obter esse intervalo de endereços IP? O fornecedor de equipa ou o serviço de rede empresarial deve fornecer um intervalo de endereços IP que não é atualmente usado no interior da rede. Um /24 CIDR exclusivo (recomendado) bloquear a ser utilizado para atribuir endereços IP específicos, necessários para o SAP HANA no Azure (instâncias grandes). **Esse intervalo é um intervalo de endereços IP, que tem de ser submetidas à Microsoft quando pedir para uma implementação inicial**
 
Ainda que precise definir e planear os intervalos de endereços IP acima, nem todos os-los tem de ser transmitidas à Microsoft. Para resumir o acima, os intervalos de endereços IP que é necessário que dê um nome para a Microsoft são:

- Espaços de endereço de VNet do Azure
- Intervalo de endereços para conectividade de ER P2P
- Intervalo de endereços de conjunto IP do servidor

Adição de VNets adicionais que precisam de ligar a instâncias grandes do HANA, requer a submeter o espaço de endereços do VNet do Azure novo que está a adicionar à Microsoft. 

Segue-se um exemplo de alguns intervalos de exemplo e os intervalos diferentes conforme necessário configurar e, eventualmente, fornecer à Microsoft. Como pode ver, o valor para o espaço de endereços VNet do Azure não é agregado no primeiro exemplo, mas é definido de intervalos de intervalo de endereços IP de sub-rede a primeira VM do Azure e o intervalo de endereços IP de sub-rede de Gateway de VNet. Utilizar várias sub-redes VM na VNet do Azure funciona da mesma forma, configurar e submeter o IP adicional intervalos de endereços de sub-redes VM adicionais como parte do espaço de endereços da VNet do Azure.

![Intervalos de endereços IP necessários no SAP HANA na implementação básica do Azure (instâncias grandes)](./media/hana-overview-connectivity/image4b-ip-addres-ranges-necessary.png)

Também tem a possibilidade de agregação de dados que enviar à Microsoft. Nesse caso, o espaço de endereços da VNet do Azure só inclui um espaço. Usando os intervalos de endereços IP utilizados no exemplo anterior. Este espaço de endereço VNet agregado pode ter o seguinte aspeto:

![Segundo possibilidade de intervalos de endereços IP necessários no SAP HANA na implementação básica do Azure (instâncias grandes)](./media/hana-overview-connectivity/image5b-ip-addres-ranges-necessary-one-value.png)

Como pode ver acima, em vez de dois intervalos menores que definiu o espaço de endereços da VNet do Azure, temos um intervalo maior que abrange 4096 endereços IP. Uma definição grande do espaço de endereços deixa alguns bastante grandes intervalos não utilizados. Uma vez que o valor ou valores de espaço de endereços VNet é utilizado para a propagação de rotas do BGP, a utilização de intervalos não utilizados no local ou em outro lugar na sua rede pode causar problemas de encaminhamento. Portanto, é recomendado para manter o espaço de endereços estreitamente alinhado com o espaço de endereços de sub-rede real usado. Se for necessário, sem incorrer em períodos de indisponibilidade na VNet, pode sempre adicionar novos valores de espaço de endereço posteriormente.
 
> [!IMPORTANT] 
> Cada endereço IP tem de espaço de endereços do intervalo de ER-P2P, conjunto de IP do servidor, Azure VNet **não** se sobrepõem entre si ou qualquer outro intervalo utilizado qualquer outra parte de sua rede; cada um tem de ser discreto e como os dois gráficos show anterior, talvez não seja um sub-rede de qualquer outro intervalo. Se ocorrerem sobreposições entre intervalos de, a VNet do Azure não pode ligar ao circuito do ExpressRoute.

## <a name="next-steps-after-address-ranges-have-been-defined"></a>Passos seguintes depois de intervalos de endereços foram definidos
Depois dos intervalos de endereços IP foram definidos, têm de ocorrer as seguintes atividades:

1. Submeta os intervalos de endereços IP para o espaço de endereços do VNet do Azure, a conectividade de ER P2P e o intervalo de endereços do conjunto de IP do servidor, juntamente com outros dados que tenham sido listados no início do documento. No momento, também pode começar a criar a VNet e sub-redes VM. 
2. Um circuito Express Route é criado pela Microsoft, entre a sua subscrição do Azure e o carimbo de data / instância grande do HANA.
3. É criada uma rede de inquilino no carimbo a instância grande pela Microsoft.
4. Microsoft configura o funcionamento em rede no SAP HANA na infraestrutura do Azure (instâncias grandes) para aceitar endereços IP do seu Azure VNet espaço de endereços que se comunique com instâncias grandes do HANA.
5. Consoante o específico SAP HANA no Azure (instâncias grandes) SKU, adquirido, Microsoft atribui uma unidade de computação numa rede de inquilino, alocar e Monte o armazenamento e instalar o sistema operativo (SUSE ou Red Hat Linux). Endereços IP para estas unidades são retirados o endereço de conjunto de IP do servidor intervalo submetidas à Microsoft.

No final do processo de implantação, a Microsoft fornece os seguintes dados para si:
- Informações necessárias para ligar a vnet ou Vnets do Azure para o circuito do ExpressRoute que se liga a VNets do Azure para instâncias grandes do HANA:
     - Chaves de autorização
     - PeerID do ExpressRoute
- Dados para aceder a instâncias grandes do HANA após o estabelecimento de circuito do ExpressRoute e de VNet do Azure.

Também pode encontrar a seqüência de conexão instâncias grandes do HANA no documento [programa de configuração de ponto a ponto para as instâncias grandes do SAP HANA](https://azure.microsoft.com/resources/sap-hana-on-azure-large-instances-setup/). Muitos dos passos seguintes são exibidos num exemplo de implementação contidas nesse documento. 

**Passos seguintes?**

- Consultar [ligação de uma VNet para ExpressRoute de instância grande do HANA](hana-connect-vnet-express-route.md).