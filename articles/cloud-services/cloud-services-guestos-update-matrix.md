---
title: Saiba mais sobre as versões de SO de convidado mais recente do Azure | Microsoft Docs
description: As últimas novidades da versão e compatibilidade SDK para o SO de convidado de serviços de nuvem do Azure.
services: cloud-services
documentationcenter: na
author: raiye
manager: timlt
editor: ''
ms.assetid: 6306cafe-1153-44c7-8554-623b03d59a34
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 6/1/2018
ms.author: raiye
ms.openlocfilehash: dd6a9b3e8777c6efa4d6db07d4d65c86463f6cfe
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063617"
---
# <a name="azure-guest-os-releases-and-sdk-compatibility-matrix"></a>Versões de SO convidado do Azure e matriz de compatibilidade SDK
Fornece-lhe informações atualizadas sobre a Azure mais recente do sistema operativo convidado versões para serviços em nuvem. Estas informações ajudam a planear o caminho de atualização antes de um SO convidado está desativado. Se configurar as funções de utilizar *automática* atualizações de SO convidado, conforme descrito em [definições de atualização de SO de convidado do Azure][Azure Guest OS Update Settings], não é vital que leia esta página.

> [!IMPORTANT]
> Esta página é aplicável a serviços em nuvem funções web e de trabalho, que são executados sobre um SO convidado. Faz **não aplicar** para máquinas de virtuais do IaaS.
>
>


> [!TIP]
>  Subscrever o [Feed RSS de atualização de SO convidado] para receber a notificação mais atempada em todas as alterações de SO convidado.
>
>

> [!IMPORTANT]
> A partir da implementação de Novembro, apenas versões mais recentes 2 do SO convidado serão suportados e estão disponíveis no portal do Azure.
>
>

Não souber sobre como atualizar o SO convidado? Verifique [isto] [ cloud updates] enviados.

## <a name="news-updates"></a>Atualizações de notícias de última hora
###### <a name="june-1-2018"></a>**1 de Junho de 2018**
O SO convidado podem foi libertado.

###### <a name="may-4-2018"></a>**4 de Maio de 2018**
Sistema operativo convidado Abril lançou.

###### <a name="april-6-2018"></a>**6 de Abril de 2018**
Lançou a Março do sistema operativo convidado.

###### <a name="march-19-2018"></a>**19 de Março de 2018**
Sistema operativo convidado Fevereiro lançou.

###### <a name="january-29-2018"></a>**29 de Janeiro de 2018**
Foi lançado o SO convidado de Janeiro de 2 de famílias de SO (WA-convidado-SO-2.70_201801-01) & 3 (WA-convidado-SO-3.57_201801-01)

###### <a name="january-4-2018"></a>**4 de Janeiro de 2018**
Foi lançado o SO convidado de Janeiro de 4 de famílias de SO (WA-convidado-SO-4.50_201801-01) & 5 (WA-convidado-so-5.15_201801-01) e contém patches de segurança importantes.  

###### <a name="january-4-2018"></a>**4 de Janeiro de 2018**
SO de convidado de Dezembro foi libertado.

###### <a name="december-14-2017"></a>**14 de Dezembro de 2017**
SO de convidado de Novembro foi libertado.

###### <a name="november-8-2017"></a>**8 de Novembro de 2017**
SO de convidado de Outubro foi libertado.

###### <a name="october-6-2017"></a>**6 de Outubro de 2017**
SO de convidado de Setembro foi libertado. Para a versão de Setembro de 2016 do Windows Server, netfx3 está ativada por predefinição. Devem adicionar clientes ' dism /online /Enable-Feature /disable-feature /featurename:netfx3' no respetivo OnStart se o seu fluxo de trabalho requer-lhes para executarem uma aplicação de 2. x .NET com um tempo de execução de 4. x ou se a execução de uma aplicação de 2. x do .NET, processado um erro e, em seguida, foi executada uma aplicação de 4. x do .NET.



## <a name="releases"></a>versões
## <a name="family-5-releases"></a>Versões de família 5
**Windows Server 2016**

.NET framework instalado: 4.0, 4.5, 4.5.1, 4.5.2, 4.6, 4.6.1, 4.6.2

> [!NOTE]
> As datas com uma * estão sujeitas a alterações.
>
> A palavra-passe RDP 5 da família de SO tem de ser no mínimo 10 carateres.
>

| Cadeia de configuração | Data da versão | Desativar data | Data expirada |
| --- | --- | --- | --- |
| WA-CONVIDADO-SO-5.19_201805-01 |1 de Junho de 2018 |Post 5.21 |TBD |
| WA-CONVIDADO-SO-5.18_201804-01 |4 de Maio de 2018 |Post 5.20 |TBD |
|~~WA-CONVIDADO-SO-5.17_201803-01~~ |6 de Abril de 2018 |1 de Junho de 2018|TBD |
|~~WA-CONVIDADO-SO-5.16_201802-01~~ |12 de Março de 2018 |4 de Maio de 2018 |TBD |
|~~WA-GUEST-OS-5.15_201801-01~~ |4 de Janeiro de 2018 |6 de Abril de 2018 |TBD |
|~~WA-GUEST-OS-5.14_201712-01~~ |4 de Janeiro de 2018 |12 de Março de 2018 |TBD |
|~~WA-GUEST-OS-5.13_201711-01~~ |14 de Dezembro de 2017 |4 de Janeiro de 2018|TBD |
|~~WA-GUEST-OS-5.12_201710-02~~ |8 de Novembro de 2017 |4 de Janeiro de 2018 |TBD |
|~~WA-GUEST-OS-5.11_201709-01~~ |6 de Outubro de 2017 |14 de Dezembro de 2017 |TBD |
|~~WA-GUEST-OS-5.10_201708-01~~ |24 de Agosto de 2017 |14 de Dezembro de 2017 |TBD |


## <a name="family-4-releases"></a>Versões de família 4
**Windows Server 2012 R2**

.NET framework instalado: 4.0, 4.5, 4.5.1, o 4.5.2

> [!NOTE]
> As datas com uma * estão sujeitas a alterações
>
>

| Cadeia de configuração | Data da versão | Desativar data | Data expirada |
| --- | --- | --- | --- |
| WA-CONVIDADO-SO-4.54_201805-01 |1 de Junho de 2018 |Post 4.56 |TBD |
| WA-CONVIDADO-SO-4.53_201804-01 |4 de Maio de 2018 |Post 4.55 |TBD |
|~~WA-CONVIDADO-SO-4.52_201803-01~~ |6 de Abril de 2018 |1 de Junho de 2018 |TBD |
|~~WA-CONVIDADO-SO-4.51_201802-01~~ |12 de Março de 2018 |4 de Maio de 2018 |TBD |
|~~WA-GUEST-OS-4.50_201801-01~~ |4 de Janeiro de 2018 |6 de Abril de 2018 |TBD |
|~~WA-GUEST-OS-4.49_201712-01~~ |4 de Janeiro de 2018 |12 de Março de 2018 |TBD |
|~~WA-GUEST-OS-4.48_201711-01~~ |14 de Dezembro de 2017 |4 de Janeiro de 2018 |TBD |
|~~WA-GUEST-OS-4.47_201710-02~~ |8 de Novembro de 2017 |4 de Janeiro de 2018 |TBD |
|~~WA-GUEST-OS-4.46_201709-01~~ |6 de Outubro de 2017 |14 de Dezembro de 2017 |TBD |
|~~WA-GUEST-OS-4.45_201708-01~~ |24 de Agosto de 2017 |14 de Dezembro de 2017 |TBD |



## <a name="family-3-releases"></a>Versões da família 3
**Windows Server 2012**

.NET framework instalado: 4.0, 4.5, 4.5.1, o 4.5.2

> [!NOTE]
> As datas com uma * estão sujeitas a alterações
>
>

| Cadeia de configuração | Data da versão | Desativar data | Data expirada |
| --- | --- | --- | --- |
| WA-CONVIDADO-SO-3.61_201805-01 |1 de Junho de 2018 |Post 3.63 |TBD |
| WA-CONVIDADO-SO-3.60_201804-01 |4 de Maio de 2018 |Post 3.62 |TBD |
|~~WA-CONVIDADO-SO-3.59_201803-01~~ |6 de Abril de 2018 |1 de Junho de 2018 |TBD |
|~~WA-CONVIDADO-SO-3.58_201802-01~~ |19 de Março de 2018 |4 de Maio de 2018 |TBD |
|~~WA-GUEST-OS-3.57_201801-01~~ |29 de Janeiro de 2018 |6 de Abril de 2018 |TBD |
|~~WA-GUEST-OS-3.56_201712-01~~ |4 de Janeiro de 2018 |19 de Março de 2018 |TBD |
|~~WA-GUEST-OS-3.55_201711-01~~ |14 de Dezembro de 2017 |29 de Janeiro de 2018 |TBD |
|~~WA-GUEST-OS-3.54_201710-02~~ |8 de Novembro de 2017 |4 de Janeiro de 2018 |TBD |
|~~WA-GUEST-OS-3.53_201709-01~~ |6 de Outubro de 2017 |14 de Dezembro de 2017 |TBD |
|~~WA-GUEST-OS-3.52_201708-01~~ |24 de Agosto de 2017 |14 de Dezembro de 2017 |TBD |


## <a name="family-2-releases"></a>Versões de família 2
**Windows Server 2008 R2 SP1**

.NET framework instalado: 3.5, 4.0, 4.5, 4.5.1, 4.5.2

> [!NOTE]
> As datas com uma * estão sujeitas a alterações
>
>

| Cadeia de configuração | Data da versão | Desativar data | Data expirada |
| --- | --- | --- | --- |
| WA-CONVIDADO-SO-2.74_201805-01 |1 de Junho de 2018 |Post 2.76 |TBD |
| WA-CONVIDADO-SO-2.73_201804-01 |4 de Maio de 2018 |Post 2.75 |TBD |
|~~WA-CONVIDADO-SO-2.72_201803-01~~ |6 de Abril de 2018 |1 de Junho de 2018 |TBD |
|~~WA-CONVIDADO-SO-2.71_201802-01~~ |12 de Março de 2018 |4 de Maio de 2018 |TBD |
|~~WA-GUEST-OS-2.70_201801-01~~ |29 de Janeiro de 2018 |6 de Abril de 2018 |TBD |
|~~WA-GUEST-OS-2.69_201712-01~~ |4 de Janeiro de 2018 |12 de Março de 2018 |TBD |
|~~WA-GUEST-OS-2.68_201711-01~~ |14 de Dezembro de 2017 |29 de Janeiro de 2018 |TBD |
|~~WA-GUEST-OS-2.67_201710-02~~ |8 de Novembro de 2017 |4 de Janeiro de 2018 |TBD |
|~~WA-GUEST-OS-2.66_201709-01~~ |6 de Outubro de 2017 |14 de Dezembro de 2017 |TBD |
|~~WA-GUEST-OS-2.65_201708-01~~ |24 de Agosto de 2017 |14 de Dezembro de 2017 |TBD |


## <a name="msrc-patch-updates"></a>Atualizações de correção MSRC
A lista de patches que estão incluídas com cada versão de SO convidado mensal está disponível [aqui][patches].

## <a name="sdk-support"></a>Suporte SDK
Apesar do [política de extinção para o Azure SDK] [ retire policy sdk] indica que apenas versões acima 2.2 são suportadas e específica famílias de SO convidado permitem-lhe utilizar versões anteriores. Deve sempre utilizar o SDK mais recente suportado.

| Família de SO convidado | Versões do SDK compatível |
| --- | --- |
| 5 |Versão 2.9.5.1+ |
| 4 |Versão 2.1 + |
| 3 |Versão 1.8 + |
| 2 |Versão 1.3 + |
| 1 |Versão 1.0 + |

## <a name="guest-os-release-information"></a>Informações de versão de SO convidado
Existem três datas que são importantes para versões de SO convidado: **versão** data, **desativada** data, e **expiração** data. Um SO convidado é considerado disponível quando está no Portal e pode ser selecionado como o destino de SO convidado. Quando um SO convidado atingir o **desativada** data, este é removido do Azure. No entanto, qualquer serviço de nuvem nesse SO convidado ainda funcionará como normal.

A janela entre o **desativada** data e a **expiração** data fornece uma memória intermédia para facilmente transição de um SO convidado para uma mais recente. Se estiver a utilizar *automática* como o SO convidado, poderá sempre a versão mais recente e não precisa de preocupar com-prestes a expirar.

Quando o **expiração** data passa, qualquer serviço de nuvem ainda utilizar nesse SO convidado será parado, eliminar ou forçado a atualizar. Pode ler mais sobre a política de extinção [aqui][retirepolicy].

## <a name="guest-os-family-version-explanation"></a>Explicação da família de SO convidado
As famílias de SO convidado baseiam-se em versões de lançamento do Microsoft Windows Server. O SO convidado é o sistema operativo subjacente que Cloud Services do Azure é executado. Cada SO convidado possui um família, a versão e a versão número.

* **Família de SO convidado**  
  Um sistema operativo Windows Server de versão baseiam um SO convidado. Por exemplo, *família 3* é baseado no Windows Server 2012.
* **Versão de SO convidado**  
  Específicos a uma imagem de família de SO convidado plus relevantes [Microsoft Security Response Center (MSRC)] [ msrc] patches que estão disponíveis na data é produzida a nova versão de SO convidado. Nem todos os patches podem ser incluídos.

    Os números começam em 0 e incrementados por 1 sempre que é adicionado um novo conjunto de atualizações. Só são mostradas zeros à direita se importantes. Ou seja, versão 2.10 é uma versão diferente, muito posterior à versão 2.1.
* **Versão de SO convidado**  
  Um rerelease de uma versão de SO convidado. Um rerelease ocorre se o Microsoft localiza problemas durante o teste; necessidade de alterações. A versão mais recente sempre substitui quaisquer anterior versões, público ou não. O portal do Azure apenas permitirá aos utilizadores escolher a versão mais recente para uma determinada versão. Implementações em execução uma versão anterior são normalmente não force atualizada dependendo da gravidade da erros.

No exemplo abaixo, 2 é a família, 12 é a versão e "rel2" é a versão.

**Versão de SO convidado** - 2.12 rel2

**Cadeia de configuração para esta versão** -WA-convidado-SO-2.12_201208-02

A cadeia de configuração para um SO convidado tem esta mesma informação incorporada, juntamente com uma data que mostra os patches MSRC foram considerados para essa versão. Neste exemplo, patches MSRC produzidos para o Windows Server 2008 R2 até e incluindo Agosto de 2012 foram contemplados para inclusão. Apenas os patches aplicar especificamente para essa versão do Windows Server estão incluídos. Por exemplo, se uma correção MSRC aplica-se ao Microsoft Office, não será incluído porque esse produto não faz parte da imagem base do Windows Server.

## <a name="guest-os-system-update-process"></a>Processo de atualização do sistema de SO convidado
Esta página inclui informações sobre os lançamentos de SO convidado. Os clientes indicou que querem saber quando uma versão ocorre porque as respetivas funções de serviço em nuvem serão reiniciado se forem definidos para a atualização "Automática". Versões de SO convidado normalmente ocorrerem, pelo menos, cinco (5) dias após o MSRC atualizar a versão que ocorre na segunda Terça-feira de cada mês. Novos lançamentos incluem os patches MSRC relevantes para cada família de SO convidado.

Microsoft Azure está constantemente lançar atualizações. O SO convidado é apenas um desse atualização no pipeline. Uma versão pode ser afetada por vários fatores demasiado numerosos para listar aqui. Além disso, o Azure é executado na literalmente centenas de milhares de máquinas. Isto significa que não é possível atribuir uma data e hora exatas quando as funções selecionadas serão reiniciado. Estamos a trabalhar um plano para limitar ou tempo reinícios.

Quando uma nova versão do SO convidado for publicada, pode demorar tempo para propagar totalmente através do Azure. Como os serviços são atualizados para o novo sistema operativo convidado, são reiniciados para respeitar domínios de atualização. Serviços configurados para utilizar atualizações de "Automático" irão obter uma versão primeiro. Após a atualização, verá a nova versão de SO convidado listada para o serviço no portal do Azure. Rereleases poderão ocorrer durante este período. Algumas versões podem ser implementadas por períodos mais longos de tempo e reinícios de atualização automáticos poderão não ocorrer para muitos semanas após a data de lançamento oficial. Depois de um SO convidado está disponível, pode, em seguida, explicitamente escolher essa versão do portal ou no ficheiro de configuração.

Para uma grande quantidade de informações importantes sobre reinicia e ponteiros para obter mais detalhes técnicos de informações de atualizações do convidado e sistema operativo anfitrião, consulte o blogue do MSDN post intitulado [função instância reinicia devido a atualizações do SO] [ restarts].

Se atualizar manualmente o SO convidado, consulte o [política de extinção de SO convidado] [ retirepolicy] para obter informações adicionais.

## <a name="guest-os-supportability-and-retirement-policy"></a>Suporte de SO convidado e a política de extinção
A política de Suportabilidade e extinção de SO convidado é explicada [aqui][retirepolicy].

[cloud updates]: https://docs.microsoft.com/en-us/azure/cloud-services/cloud-services-update-azure-service
[Feed RSS de atualização de SO convidado]: https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/master/GuestOS/GuestOSFeed.xml
[Install .NET on a Cloud Service Role]: https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-install-dotnet/?WT.mc_id=azurebg_email_Trans_963_RevisedNET_Update
[Azure Guest OS Update Settings]: cloud-services-how-to-configure-portal.md
[ssl3 announcement]: http://azure.microsoft.com/blog/2014/12/09/azure-security-ssl-3-0-update/
[Microsoft Security Advisory 3009008]: https://technet.microsoft.com/library/security/3009008.aspx
[ssl3-fixit]: http://go.microsoft.com/?linkid=9863266
[MS14-066]: https://technet.microsoft.com/library/security/ms14-066.aspx
[MS14-046]: https://technet.microsoft.com/library/security/ms14-046.aspx
[retire policy sdk]: https://msdn.microsoft.com/library/dn479282.aspx
[server and gos]: https://msdn.microsoft.com/library/dn775043.aspx
[azuresupport]: http://azure.microsoft.com/support/options/
[net install pkg]: http://www.microsoft.com/download/details.aspx?id=42643
[msrc]: https://technet.microsoft.com/en-us/security/dn440717.aspx
[update guest os portal]: https://msdn.microsoft.com/library/gg433101.aspx
[update guest os svc]: https://msdn.microsoft.com/library/gg456324.aspx
[restarts]: http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx
[patches]: cloud-services-guestos-msrc-releases.md
[retirepolicy]: cloud-services-guestos-retirement-policy.md
[fam1retire]: cloud-services-guestos-family1-retirement.md
[fix]: https://technet.microsoft.com/library/security/ms17-010.aspx
