---
title: Integração de datacenter do Azure Stack - DNS
description: Saiba como integrar o DNS do Azure Stack com o seu datacenter DNS
services: azure-stack
author: jeffgilb
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: wfayed
ms.lastreviewed: 10/15/2018
keywords: ''
ms.openlocfilehash: 13525fffb7e6720fe81759876ffd0fe71559279c
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56182855"
---
# <a name="azure-stack-datacenter-integration---dns"></a>Integração de datacenter do Azure Stack - DNS
Para poder aceder aos pontos finais do Azure Stack (**portal**, **adminportal**, **gestão**, **adminmanagement**, etc.)  a partir de fora do Azure Stack, tem de integrar os serviços de DNS do Azure Stack com os servidores DNS que alojam as zonas DNS que pretende utilizar no Azure Stack.

## <a name="azure-stack-dns-namespace"></a>Espaço de nomes de DNS de pilha do Azure
É necessário fornecer algumas informações importantes relacionados com DNS quando implementar o Azure Stack.


|Campo  |Descrição  |Exemplo|
|---------|---------|---------|
|Região|A localização geográfica da sua implementação do Azure Stack.|`east`|
|Nome de domínio externo|O nome da zona que pretende utilizar para a sua implementação do Azure Stack.|`cloud.fabrikam.com`|
|Nome de domínio interno|O nome da zona interno que é utilizada para serviços de infraestrutura no Azure Stack.  É integrado ao serviço de diretório e privadas (não acessível a partir de fora a implementação do Azure Stack).|`azurestack.local`|
|Reencaminhador DNS|Servidores DNS que são usados para encaminhar consultas DNS, zonas e registos DNS que estão alojados fora do Azure Stack, tanto na intranet empresarial ou na internet pública.|`10.57.175.34`<br>`8.8.8.8`|
|Prefixo de nomenclatura (opcional)|O prefixo de nomenclatura desejar que seus nomes de máquina de instância de função de infraestrutura do Azure Stack ter.  Se não for indicado, a predefinição é `azs`.|`azs`|

O nome de domínio completamente qualificado (FQDN) da sua implementação do Azure Stack e pontos de extremidade é a combinação do parâmetro de região e o parâmetro de nome de domínio externo. Utilizar os valores dos exemplos na tabela anterior, o FQDN para esta implementação do Azure Stack seria o seguinte nome:

`east.cloud.fabrikam.com`

Como tal, os exemplos de alguns dos pontos finais para esta implementação pareceria-se com os seguintes URLs:

`https://portal.east.cloud.fabrikam.com`

`https://adminportal.east.cloud.fabrikam.com`

Para utilizar este espaço de nomes DNS de exemplo para uma implementação do Azure Stack, as seguintes condições são necessárias:

- A zona `fabrikam.com` está registado um registo de domínios, um servidor DNS empresarial interno, ou ambos, dependendo dos requisitos de resolução de nome.
- Domínio subordinado `cloud.fabrikam.com` existe sob a zona `fabrikam.com`.
- Os servidores DNS que alojam as zonas `fabrikam.com` e `cloud.fabrikam.com` pode ser contatado da implementação do Azure Stack.

Para conseguir resolver nomes DNS para pontos finais de pilha do Azure e instâncias de fora do Azure Stack, terá de integrar os servidores DNS que alojam a zona DNS externa para o Azure Stack com os servidores DNS que alojam a zona principal que pretende utilizar.


## <a name="resolution-and-delegation"></a>Resolução e delegação

Existem dois tipos de servidores DNS:

- Um servidor DNS autoritativo aloja zonas DNS. Responde a consultas DNS para os registos apenas dessas zonas.
- Um servidor DNS recursivo não aloja zonas DNS. Responde a todas as consultas DNS ao chamar servidores DNS autoritativos para recolher os dados de que necessita.

O Azure Stack inclui oficiais e os servidores DNS recursivos. Os servidores de recursiva são utilizados para resolver os nomes de tudo, exceto para a zona privada interna e a zona DNS pública externa dessa implementação do Azure Stack. 

![Arquitetura de pilha DNS do Azure](media/azure-stack-integrate-dns/Integrate-DNS-01.png)

## <a name="resolving-external-dns-names-from-azure-stack"></a>Resolução de nomes DNS externos do Azure Stack

Para resolver os nomes DNS para pontos finais de fora do Azure Stack (por exemplo: www.bing.com), tem de fornecer servidores DNS que pode utilizar o Azure Stack para reencaminhar pedidos DNS para o qual o Azure Stack não é obrigatório. Para a implementação, os servidores DNS do Azure Stack reencaminha os pedidos para são necessários na folha de cálculo de implementação (no campo de reencaminhador de DNS). Forneça, pelo menos, dois servidores neste campo para tolerância a falhas. Sem estes valores, falha na implementação do Azure Stack.

### <a name="configure-conditional-dns-forwarding"></a>Configurar o reencaminhamento condicional de DNS

> [!IMPORTANT]
> Isto aplica-se apenas a uma implementação do AD FS.

Para habilitar a resolução de nome com a infraestrutura DNS existente, configure o reencaminhamento condicional.

Para adicionar um reencaminhador condicional, tem de utilizar o ponto final com privilégios.

Para este procedimento, utilize um computador na sua rede de centro de dados que pode se comunicar com o ponto final com privilégios no Azure Stack.

1. Abra uma sessão elevada do Windows PowerShell (executar como administrador) e ligar ao endereço IP do ponto final com privilégios. Utilize as credenciais para autenticação de CloudAdmin.

   ```
   $cred=Get-Credential 
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $cred
   ```

2. Depois de ligar para o ponto final com privilégios, execute o seguinte comando do PowerShell. Substitua os valores de exemplo fornecidos com o seu nome de domínio e endereços IP dos servidores DNS que pretende utilizar.

   ```
   Register-CustomDnsServer -CustomDomainName "contoso.com" -CustomDnsIPAddresses "192.168.1.1","192.168.1.2"
   ```

## <a name="resolving-azure-stack-dns-names-from-outside-azure-stack"></a>Resolução de nomes de DNS do Azure Stack de fora do Azure Stack
Os servidores autoritativos são aqueles que contêm as informações de zona DNS externas e qualquer zonas criados pelo utilizador. Integre com estes servidores para permitir a delegação de zona ou reencaminhamento condicional para resolver nomes DNS do Azure Stack de fora do Azure Stack.

## <a name="get-dns-server-external-endpoint-information"></a>Obtenha informações de ponto final externo do servidor DNS

Para integrar a sua implementação do Azure Stack com a sua infraestrutura DNS, terá das seguintes informações:

- Servidor DNS FQDNs
- Endereços IP do servidor DNS

Os FQDNs dos servidores de DNS do Azure Stack de tem o seguinte formato:

`<NAMINGPREFIX>-ns01.<REGION>.<EXTERNALDOMAINNAME>`

`<NAMINGPREFIX>-ns02.<REGION>.<EXTERNALDOMAINNAME>`

Os servidores a utilizar os valores de exemplo, o FQDN para o DNS são:

`azs-ns01.east.cloud.fabrikam.com`

`azs-ns02.east.cloud.fabrikam.com`


Estas informações também são criadas no final de todas as implementações do Azure Stack num ficheiro denominado `AzureStackStampInformation.json`. Este ficheiro está localizado no `C:\CloudDeployment\logs` pasta da máquina de virtual de implementação. Se não tiver a certeza de que os valores que foram utilizados para a sua implementação do Azure Stack, pode obter os valores a partir daqui.

Se a máquina virtual de implementação já não está disponível ou está inacessível, pode obter os valores ao ligar ao ponto final com privilégios e executar o `Get-AzureStackStampInformation` cmdlet do PowerShell. Para obter mais informações, consulte [ponto final com privilégios](azure-stack-privileged-endpoint.md).

## <a name="setting-up-conditional-forwarding-to-azure-stack"></a>Configurar o reencaminhamento condicional para o Azure Stack

A forma mais simples e mais segura para integrar o Azure Stack com a sua infraestrutura DNS é fazer o reencaminhamento condicional da zona de servidor que aloja a zona principal. Essa abordagem é recomendada se o ter controlo direto sobre os servidores DNS que alojam a zona de principal para o Azure Stack externo espaço de nomes DNS.

Se não estiver familiarizado com a forma de fazer condicional de reencaminhamento com o DNS, consulte o seguinte artigo do TechNet: [Atribuir um reencaminhador condicional para um nome de domínio](https://technet.microsoft.com/library/cc794735), ou a documentação específica à sua solução DNS.

Em cenários em que especificou sua zona de DNS do Azure Stack externo para se parecer com um domínio filho de seu nome de domínio empresarial, não é possível utilizar reencaminhamento condicional. Tem de configurar a delegação de DNS.

Exemplo:

- Nome de domínio DNS da empresa: `contoso.com`
- Nome de domínio DNS externo do Azure Stack: `azurestack.contoso.com`

## <a name="delegating-the-external-dns-zone-to-azure-stack"></a>Delegar a zona DNS externa para o Azure Stack

Para nomes de DNS a ser resolvido a partir fora de uma implementação do Azure Stack, terá de configurar a delegação de DNS.

Cada entidade de registo tem as suas próprias ferramentas de gestão de DNS para alterar os registos do servidor de nome de um domínio. Na página de gestão de DNS da entidade de registo, edite os registos NS e substitua os registos NS para a zona por aqueles que o Azure Stack.

A maioria das entidades de registo DNS exigem que forneça um mínimo de dois servidores DNS para concluir a delegação.

## <a name="next-steps"></a>Passos Seguintes

[Integração da firewall](azure-stack-firewall.md)
