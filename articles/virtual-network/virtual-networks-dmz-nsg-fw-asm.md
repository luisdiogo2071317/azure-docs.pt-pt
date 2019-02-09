---
title: Exemplo de rede de Perímetro – criar uma rede de Perímetro a proteger aplicações com um Firewall e NSGs | Documentos da Microsoft
description: Criar um DMZ com um Firewall e de grupos de segurança de rede (NSG)
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: ''
ms.assetid: c78491c7-54ac-4469-851c-b35bfed0f528
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: jonor;sivae
ms.openlocfilehash: 31d945f64cccd0c811d4dc45163583224102fb8a
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/08/2019
ms.locfileid: "55965244"
---
# <a name="example-2--build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs"></a>Exemplo 2 – criar uma rede de Perímetro a proteger aplicações com um Firewall e NSGs
[Regressar à página de práticas recomendada de segurança limites][HOME]

Neste exemplo irá criar uma rede de Perímetro com uma firewall, servidores quatro windows e grupos de segurança de rede. Ele irá também explicar cada um dos comandos relevantes para fornecer uma compreensão mais aprofundada de cada passo. Também há uma seção de cenário de tráfego para fornecer um detalhado passo a passo de como o tráfego passa pelas camadas de defesa na rede de Perímetro. Finalmente, as referências de seção é o código completo e a instrução para criar este ambiente de teste e experimente com vários cenários. 

![DMZ com NVA e NSG de entrada][1]

## <a name="environment-description"></a>Descrição de ambiente
Neste exemplo, existe uma subscrição que contém o seguinte:

* Dois serviços de cloud: "FrontEnd001" e "BackEnd001"
* Uma rede Virtual "CorpNetwork", com duas sub-redes: "Front-end" e "Back-end"
* Um único grupo de segurança de rede que é aplicado a ambas as sub-redes
* Uma aplicação virtual de rede, neste exemplo, um Barracuda NextGen Firewall, ligada à sub-rede de front-end
* Windows Server que representa um servidor de web de aplicações ("IIS01")
* Servidores de duas janelas que representem o aplicativo novamente terminam servidores ("AppVM01", "AppVM02")
* Um servidor do Windows que representa um servidor DNS ("DNS01")

> [!NOTE]
> Embora este exemplo utiliza um Barracuda NextGen Firewall, muitas das diferentes aplicações virtuais de rede podem ser utilizadas para este exemplo.
> 
> 

A seção de referências abaixo é um script do PowerShell que criará a maior parte do ambiente descrito acima. A criar as VMs e redes virtuais, embora são efetuados pelo script de exemplo, não estão descritas em detalhe neste documento.

Para criar o ambiente:

1. Guarde o ficheiro de xml de configuração de rede incluído na seção referências (atualizado com os nomes, a localização e o IP endereços de acordo com o cenário determinado)
2. Atualize as variáveis de utilizador no script para corresponder ao ambiente de que script está a ser executado em (subscrições, os nomes de serviço, etc.)
3. Execute o script do PowerShell

**Nota**: A região demonstrada no script do PowerShell têm de corresponder a região demonstrada no ficheiro xml de configuração de rede.

Assim que o script é executado com êxito os passos seguintes do script posterior podem ser aumentados:

1. Configurar as regras de firewall, isto é explicado na seção abaixo: Regras de firewall.
2. Opcionalmente, na seção referências são dois scripts para configurar o servidor web e o servidor de aplicações com uma aplicação web simples para permitir que o teste com esta configuração de rede de Perímetro.

A secção seguinte explica a maioria das declarações a scripts relativas a grupos de segurança de rede.

## <a name="network-security-groups-nsg"></a>Grupos de Segurança de Rede (NSG)
Neste exemplo, um grupo NSG é criado e, em seguida, é carregado com regras de seis. 

> [!TIP]
> Em termos gerais, deve criar as regras específicas de "Permitir de" primeiro e, em seguida, as regras mais genéricas de "Negar" último. Define a prioridade atribuída que as regras são avaliadas primeiro. Assim que o tráfego for encontrado para aplicar a uma regra específica, não existem regras adicionais são avaliadas. As regras do NSG podem aplicar em qualquer um na direção de entrada ou saída (a partir do ponto de vista da sub-rede).
> 
> 

De forma declarativa, as seguintes regras estão sendo criadas para tráfego de entrada:

1. É permitido o tráfego DNS interno (porta 53)
2. É permitido o tráfego RDP (porta 3389) da Internet a qualquer VM
3. É permitido o tráfego HTTP (porta 80) da Internet para a NVA (firewall)
4. Qualquer tráfego (todas as portas) de IIS01 para AppVM1 é permitido
5. Qualquer tráfego (todas as portas) da Internet para toda a VNet (ambas as sub-redes) é negado
6. Qualquer tráfego (todas as portas) partir da sub-rede Frontend para a sub-rede de back-end é negado

Com estas regras vinculado a cada sub-rede, se um pedido HTTP foi entrado da Internet para o servidor web, ambas as regras 3 (permitir) e 5 (negar) aplicaria, mas uma vez que a regra 3 tem uma prioridade mais alta apenas seriam aplicáveis e regra 5 não seria entram em cena. Assim, o pedido HTTP teria permissão para a firewall. Se esse mesmo tráfego estava a tentar aceder ao servidor DNS01, regra 5 (negar) seria o primeiro a aplicar e o tráfego que não permitia a passar para o servidor. Regra 6 (negar) bloqueia a sub-rede de front-end ao falar com a sub-rede de back-end (exceto para o tráfego permitido nas regras 1 e 4), isso protege a rede de back-end, no caso de comprometimentos um atacante a aplicação web no front-end, o invasor teria de ter acesso limitado à a rede de back-end "protegido" (apenas para recursos expostos no servidor AppVM01).

Existe uma regra de saída predefinida que permita o tráfego de saída à internet. Neste exemplo, estamos a permitir o tráfego de saída e sem modificar quaisquer regras de saída. Para bloquear o tráfego em ambas as direções, encaminhamento de definido pelo utilizador é necessária, isso é explorado num exemplo diferente que pode ser encontrado no [documento de limites de segurança principal][HOME].

As regras do NSG referidas acima são muito semelhantes às regras NSG no [exemplo 1 - criar um DMZ simples com NSGs][Example1]. Consulte a descrição de NSG esse documento para uma visão detalhada de cada regra NSG e de atributos.

## <a name="firewall-rules"></a>Regras da Firewall
Um cliente de gestão tem de ser instalada num PC para gerir a firewall e criar as configurações necessárias. Veja a documentação do seu firewall (ou outra NVA) do fornecedor sobre como gerir o dispositivo. O resto desta secção descreve a configuração do firewall em si, através do cliente de gestão de fornecedores (ou seja, não o portal do Azure ou o PowerShell).

Aqui podem encontrar instruções para a transferência do cliente e ligar a Barracuda utilizado neste exemplo: [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin)

Na firewall, as regras de reencaminhamento tem de ser criado. Uma vez que este exemplo apenas encaminha o tráfego de internet no ligado para o firewall e, em seguida, para o servidor web, é necessário o reencaminhamento de apenas uma regra NAT. No Barracuda NextGen Firewall utilizado neste exemplo, a regra seria uma regra NAT de destino ("horário de Verão NAT") para passar este tráfego.

Para criar a seguinte regra (ou verifique se as regras existentes do padrão), a partir do dashboard de cliente do Barracuda NG Admin, navegue até ao separador de configuração, na configuração operacional secção, clique em conjunto de regras. Uma grade chamada, "Regras de principal" mostrará as regras existentes do Active Directory e desativadas na firewall. No canto superior direito dessa grade é um pequeno, verde "+" botão, clique nele para criar uma nova regra (Nota: a firewall pode ser "bloqueada" para que as alterações, se vir um botão marcados como "Bloquear" e não é possível criar ou editar regras, clicar neste botão para o conjunto de regras de "desbloquear" e permitir a edição). Se pretender editar uma regra existente, selecione essa regra, com o botão direito e selecione Editar regra.

Criar uma nova regra e forneça um nome, como "WebTraffic". 

O ícone de regra NAT de destino tem esta aparência: ![Ícone NAT de destino][2]

A regra em si teria o aspeto semelhante ao seguinte:

![Regra de firewall][3]

Aqui qualquer endereço que chegue a Firewall de entrada a tentar contactar o HTTP (porta 80 ou 443 para HTTPS) será enviada a interface de "IP Local do DHCP1" a Firewall e redirecionada para o servidor Web com o endereço IP do 10.0.1.5N. Uma vez que o tráfego é entram na porta 80 e vai para o servidor web na porta 80 não foi necessária nenhuma alteração de porta. No entanto, a lista de destino pode ter sido 10.0.1.5:8080 se escutámos de nosso servidor Web na porta 8080, portanto, converter a entrada na porta 80 na firewall para a porta de entrada 8080 no servidor web.

Um método de ligação deve também ser demonstrado, para a regra de destino a partir da Internet, "SNAT dinâmica" é mais adequada. 

Embora apenas uma regra foi criada é importante que sua prioridade está definida corretamente. Se na grade de todas as regras na firewall esta nova regra é, na parte inferior (abaixo a regra "BLOCKALL") será nunca entram em cena. Certifique-se que da regra criada recentemente para o tráfego da web for superior a regra BLOCKALL.

Assim que a regra é criada, tem de ser enviada por push para o firewall e, em seguida, ativado, se isso não é feito a alteração de regra não irão surtir efeito. O processo de ativação e de push é descrito na secção seguinte.

## <a name="rule-activation"></a>Ativação de regra
Com o conjunto de regras modificado para adicionar esta regra, o conjunto de regras tem de ser carregado para o firewall e ativado.

![Ativação de regra de firewall][4]

No canto superior direita do cliente de gestão são um cluster de botões. Clique no botão "Enviar alterações" para enviar as regras de modificação para o firewall e, em seguida, clique no botão "Ativar".

Com a ativação de regras de firewall desta versão de ambiente de exemplo está concluída. Opcionalmente, os scripts de compilação de postagem na seção referências podem ser executados para adicionar uma aplicação a este ambiente para testar o abaixo os cenários de tráfego.

> [!IMPORTANT]
> É importante perceber que não atingirá o servidor web diretamente. Quando um navegador solicita uma página HTTP do FrontEnd001.CloudApp.Net, o ponto final HTTP (porta 80) passa este tráfego para a firewall não o servidor web. O firewall, em seguida, de acordo com a regra criada acima, NATs que pedem para o servidor Web.
> 
> 

## <a name="traffic-scenarios"></a>Cenários de tráfego
#### <a name="allowed-web-to-web-server-through-firewall"></a>(Permitido) Servidor de Web para Web através da Firewall
1. Página da HTTP de pedidos de utilizador de Internet da FrontEnd001.CloudApp.Net (Internet do serviço da nuvem com acesso à)
2. Tráfego de passes do serviço de nuvem por meio de pontos finais abertos na porta 80 à interface local do firewall no 10.0.1.4:80
3. A sub-rede de front-end começa o processamento da regra de entrada:
   1. 1 de regra de NSG (DNS) não se aplicam, mudar para a próxima regra
   2. 2 de regra de NSG (RDP) não se aplicam, mudar para a próxima regra
   3. Aplicar a regra de NSG 3 (Internet à Firewall), o tráfego é o processamento da regra permitidos, pare
4. O tráfego chega o endereço IP interno do firewall (10.0.1.4)
5. Regra de encaminhamento de firewall ver isso é o tráfego da porta 80, redireciona-o para o servidor de web IIS01
6. IIS01 está à escuta de tráfego da web, recebe essa solicitação e começa a processar o pedido
7. IIS01 pede-lhe o SQL Server no AppVM01 informações
8. Não existem regras de saída na sub-rede de front-end, o tráfego é permitido
9. A sub-rede de back-end começa o processamento da regra de entrada:
   1. 1 de regra de NSG (DNS) não se aplicam, mudar para a próxima regra
   2. 2 de regra de NSG (RDP) não se aplicam, mudar para a próxima regra
   3. 3 de regra de NSG (Internet à Firewall) não se aplicam, mudar para a próxima regra
   4. 4 de regra de NSG (IIS01 para AppVM01) é aplicável, o tráfego é permitido, parar o processamento da regra
10. AppVM01 recebe a consulta SQL e responde
11. Uma vez que existem regras de saída da sub-rede de back-end a resposta é permitida
12. A sub-rede de front-end começa o processamento da regra de entrada:
    1. Não existe nenhuma regra NSG aplica-se a entrada tráfego da sub-rede de back-end para a sub-rede de front-end, para que nenhuma do NSG de regras aplicam-se
    2. A regra predefinida do sistema a permitir o tráfego entre sub-redes permitiria que este tráfego para que o tráfego é permitido.
13. O servidor IIS recebe a resposta SQL e conclui a resposta HTTP e envia para o requerente
14. Uma vez que esta é uma sessão NAT do firewall, o destino de resposta (inicialmente) é para o Firewall
15. O firewall recebe a resposta do servidor Web e reencaminha para o usuário de Internet
16. Uma vez que existem não existem regras de saída da sub-rede de front-end a resposta é permitida e o utilizador de Internet recebe a página da web solicitado.

#### <a name="allowed-rdp-to-backend"></a>(Permitido) RDP para back-end
1. Administrador do servidor na internet solicita a sessão do RDP para AppVM01 no BackEnd001.CloudApp.Net:xxxxx onde xxxxx é o número de porta aleatoriamente atribuída para RDP para AppVM01 (a porta atribuída pode ser encontrada no Portal do Azure ou através do PowerShell)
2. Uma vez que a Firewall apenas está a escutar no endereço FrontEnd001.CloudApp.Net, não está envolvida com este fluxo de tráfego
3. A sub-rede de back-end começa o processamento da regra de entrada:
   1. 1 de regra de NSG (DNS) não se aplicam, mudar para a próxima regra
   2. Aplicar a regra de NSG 2 (RDP), o tráfego é o processamento da regra permitidos, pare
4. Com não existem regras de saída, aplicam-se de regras predefinidas e tráfego de retorno é permitido
5. Sessão RDP está ativada
6. AppVM01 solicita a palavra-passe de nome de utilizador

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Permitido) Pesquisa de DNS de servidor da Web no servidor DNS
1. Web Server, IIS01, necessidades de dados de um feed em www.data.gov, mas tem de resolver o endereço.
2. A configuração de rede para as listas de VNet DNS01 (10.0.2.4 na sub-rede de back-end) que o servidor DNS primário, IIS01 envia o pedido DNS para DNS01
3. Não existem regras de saída na sub-rede de front-end, o tráfego é permitido
4. A sub-rede de back-end começa o processamento da regra de entrada:
   1. Aplicar a regra de NSG 1 (DNS), o tráfego é o processamento da regra permitidos, pare
5. Servidor DNS recebe o pedido
6. Servidor DNS não tem o endereço em cache e pede-lhe um servidor DNS de raiz na internet
7. Não existem regras de saída na sub-rede de back-end, o tráfego é permitido
8. Servidor DNS de Internet responde, uma vez que esta sessão foi iniciada internamente, a resposta é permitida
9. Servidor DNS coloca a resposta em cache e responde à solicitação inicial para IIS01
10. Não existem regras de saída na sub-rede de back-end, o tráfego é permitido
11. A sub-rede de front-end começa o processamento da regra de entrada:
    1. Não existe nenhuma regra NSG aplica-se a entrada tráfego da sub-rede de back-end para a sub-rede de front-end, para que nenhuma do NSG de regras aplicam-se
    2. A regra predefinida do sistema a permitir o tráfego entre sub-redes permitiria que este tráfego para que o tráfego é permitido
12. IIS01 recebe a resposta do DNS01

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(Permitido) Ficheiro de acesso do servidor Web no AppVM01
1. IIS01 solicita um ficheiro num AppVM01
2. Não existem regras de saída na sub-rede de front-end, o tráfego é permitido
3. A sub-rede de back-end começa o processamento da regra de entrada:
   1. 1 de regra de NSG (DNS) não se aplicam, mudar para a próxima regra
   2. 2 de regra de NSG (RDP) não se aplicam, mudar para a próxima regra
   3. 3 de regra de NSG (Internet à Firewall) não se aplicam, mudar para a próxima regra
   4. 4 de regra de NSG (IIS01 para AppVM01) é aplicável, o tráfego é permitido, parar o processamento da regra
4. AppVM01 recebe a solicitação e responde com o ficheiro (partindo do princípio de acesso está autorizado)
5. Uma vez que existem regras de saída da sub-rede de back-end a resposta é permitida
6. A sub-rede de front-end começa o processamento da regra de entrada:
   1. Não existe nenhuma regra NSG aplica-se a entrada tráfego da sub-rede de back-end para a sub-rede de front-end, para que nenhuma do NSG de regras aplicam-se
   2. A regra predefinida do sistema a permitir o tráfego entre sub-redes permitiria que este tráfego para que o tráfego é permitido.
7. O servidor IIS recebe o ficheiro.

#### <a name="denied-web-direct-to-web-server"></a>(Negado) Web direto para o servidor Web
Uma vez que o servidor Web e o Firewall IIS01 estão no mesmo serviço em nuvem partilham o mesmo endereço IP destinado ao público. Portanto, qualquer tráfego HTTP seria direcionado para o firewall. Enquanto o pedido seria servido com êxito, ele não pode ir diretamente para o servidor Web, ela passada, conforme projetado, através da Firewall do primeiro. Ver o primeiro cenário nesta secção para o fluxo de tráfego.

#### <a name="denied-web-to-backend-server"></a>(Negado) Web para o servidor back-end
1. O utilizador de Internet tenta acessar um arquivo no AppVM01 através do serviço de BackEnd001.CloudApp.Net
2. Uma vez que não existem não existem pontos finais aberta para a partilha de ficheiros, isto não transmitia o serviço em nuvem e não alcançar o servidor
3. Se os pontos de extremidade foram abertos por algum motivo, a regra NSG 5 (Internet a VNet) bloquearia deste tráfego

#### <a name="denied-web-dns-lookup-on-dns-server"></a>(Negado) Pesquisa de DNS da Web no servidor DNS
1. Utilizador de Internet tenta procurar um registo DNS interno no DNS01 através do serviço de BackEnd001.CloudApp.Net
2. Uma vez que não existem não existem pontos finais aberta para DNS, isso não transmitia o serviço em nuvem e não alcançar o servidor
3. Se os pontos de extremidade foram abertos por algum motivo, a regra NSG 5 (Internet a VNet) bloquearia este tráfego (Nota: essa regra 1 (DNS) não seriam aplicadas por dois motivos, primeiro o endereço de origem é a internet, esta regra aplica-se apenas a VNet local como origem também trata de uma regra de permissão, para que ele nunca seria a negar o tráfego)

#### <a name="denied-web-to-sql-access-through-firewall"></a>(Negado) Web para o acesso através da Firewall do SQL
1. Utilizador de Internet solicita dados SQL FrontEnd001.CloudApp.Net (Internet do serviço da nuvem com acesso à)
2. Uma vez que não existem não existem pontos finais aberta para SQL, isso não transmitia o serviço em nuvem e não alcançar o firewall
3. Se os pontos de extremidade foram abertos por algum motivo, a sub-rede de front-end começa o processamento da regra de entrada:
   1. 1 de regra de NSG (DNS) não se aplicam, mudar para a próxima regra
   2. 2 de regra de NSG (RDP) não se aplicam, mudar para a próxima regra
   3. Aplicar a regra de NSG 2 (Internet à Firewall), o tráfego é o processamento da regra permitidos, pare
4. O tráfego chega o endereço IP interno do firewall (10.0.1.4)
5. Firewall tem não existem regras de reencaminhamento para SQL e ignora o tráfego

## <a name="conclusion"></a>Conclusão
Essa é uma maneira relativamente direta de proteger a sua aplicação com um firewall e isolamento da sub-rede de back-end do tráfego de entrada.

Mais exemplos e uma descrição geral dos limites de segurança de rede podem ser encontradas [aqui][HOME].

## <a name="references"></a>Referências
### <a name="main-script-and-network-config"></a>Script principal e a configuração de rede
Guarde o Script completo num arquivo de script do PowerShell. Guarde a configuração de rede para um ficheiro denominado "NetworkConf2.xml".
Modifique as variáveis definidas pelo utilizador, conforme necessário. Execute o script e, em seguida, siga as instruções de configuração de regra de Firewall acima.

#### <a name="full-script"></a>Script completo
Este script irá, com base nas variáveis definidas pelo utilizador:

1. Ligar a uma subscrição do Azure
2. Criar uma nova conta de armazenamento
3. Criar uma nova VNet e duas sub-redes, conforme definido no ficheiro de configuração de rede
4. Criar VMs do servidor do windows de 4
5. Configure o NSG incluindo:
   * Criar um NSG
   * Preenchendo-o com as regras
   * O NSG de ligação para as sub-redes apropriadas

Este script do PowerShell deve ser executado localmente num que Internet ligado PC ou servidor.

> [!IMPORTANT]
> Quando esse script for executado, pode haver avisos ou outras mensagens de informações que exibida no PowerShell. Apenas mensagens de erro em vermelho são o motivo de preocupação.
> 
> 

```powershell
    <# 
     .SYNOPSIS
      Example of DMZ and Network Security Groups in an isolated network (Azure only, no hybrid connections)

     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet (plus the NVA on the FrontEnd subnet)
       - Three Servers on the BackEnd Subnet
       - Network Security Groups to allow/deny traffic patterns as declared

      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).

     .Notes
      Security requirements are different for each use case and can be addressed in a
      myriad of ways. Please be sure that any sensitive data or applications are behind
      the appropriate layer(s) of protection. This script serves as an example of some
      of the techniques that can be used, but should not be used for all scenarios. You
      are responsible to assess your security needs and the appropriate protections
      needed, and then effectively implement those protections.

      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       myFirewall - 10.0.1.4
       IIS01      - 10.0.1.5

      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6

    #>

    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()

    # User Defined Global Variables
      # These should be changes to reflect your subscription and services
      # Invalid options will fail in the validation section

      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"

      # Service Details
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"

      # Network Details
        $VNetName = "CorpNetwork"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf2.xml"

      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

      # NSG Details
        $NSGName = "MyVNetSG"

    # User Defined VM Specific Config
        # Note: To ensure proper NSG Rule creation later in this script:
        #       - The Web Server must be VM 1
        #       - The AppVM1 Server must be VM 2
        #       - The DNS server must be VM 4
        #
        #       Otherwise the NSG rules in the last section of this
        #       script will need to be changed to match the modified
        #       VM array numbers ($i) so the NSG Rule IP addresses
        #       are aligned to the associated VM IP addresses.

        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $FrontEndService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"

        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"

        # VM 2 - The First Application Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"

        # VM 3 - The Second Application Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"

        # VM 4 - The DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"

    # ----------------------------- #
    # No User Defined Variables or   #
    # Configuration past this point #
    # ----------------------------- #

      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop

      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}

      # Update Subscription Pointer to New Storage Account
        Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop

    # Validation
    $FatalError = $false

    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}

    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}

    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}

    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation variable is correct and the network config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}

    If ($FatalError) {
        Write-Host "A fatal error has occurred, please see the above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}

    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop

    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop

    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all the EndPoints we'll need once we're up and running
                # Note: Web traffic goes through the firewall, so we'll need to set up a HTTP endpoint.
                #       Also, the firewall will be redirecting web traffic to a new IP and Port in a
                #       forwarding rule, so the HTTP endpoint here will have the same public and local
                #       port and the firewall will do the NATing and redirection as declared in the
                #       firewall rule.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                    # Note: A Remote Desktop endpoint is automatically created when each VM is created.
                }
            $i++
        }

    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan

      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

      # Add NSG Rules
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[4] -DestinationPortRange '53' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[1]) to $($VMName[2])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[2] -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
            -Protocol *

        # Assign the NSG to the Subnets
            Write-Host "Binding the NSG to both subnets" -ForegroundColor Cyan
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host
```

#### <a name="network-config-file"></a>Ficheiro de configuração de rede
Guarde este ficheiro xml com localização atualizada e adicionar a ligação a este ficheiro para a variável de $NetworkConfigFile no script acima.

```xml
    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>
```

#### <a name="sample-application-scripts"></a>Scripts de aplicativo de exemplo
Se pretender instalar um aplicativo de exemplo para este e outros exemplos de rede de Perímetro, foi fornecido na seguinte hiperligação: [Script de aplicação de exemplo][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Rede de Perímetro com NSG de entrada"
[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Ícone NAT de destino"
[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Regra de firewall"
[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Ativação de regra de firewall"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
[Example1]: ./virtual-networks-dmz-nsg-asm.md
