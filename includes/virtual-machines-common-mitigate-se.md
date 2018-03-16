---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: bf4adf075842effeb26aa5a600c09f7bd1866264
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
**Documente última atualização**: 6 de Março, 10:00 AM PST.

A divulgação recente de um [nova classe de vulnerabilidades de CPU](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002) conhecido como ataques de canal de lado de execução speculative resultou no questões a partir de clientes de pesquisa mais efeitos de clareza.  

A infraestrutura que executa o Azure e isola as cargas de trabalho do cliente umas das outras está protegida.  Isto significa que outros clientes em execução no Azure não é possível ataques a sua aplicação utilizar estas vulnerabilidades.

> [!NOTE] 
> No enlace tardio de Fevereiro de 2018 Intel Corporation publicados atualizado [orientações de revisão Microcode](https://newsroom.intel.com/wp-content/uploads/sites/11/2018/03/microcode-update-guidance.pdf) no estado dos respetivos versões microcode, que melhorar estabilidade e mitigar contra as vulnerabilidades recentes das divulgados por [Projeto Google Zero](https://googleprojectzero.blogspot.com/2018/01/reading-privileged-memory-with-side.html). As mitigações colocar no local pelo Azure em [3 de Janeiro de 2018](https://azure.microsoft.com/en-us/blog/securing-azure-customers-from-cpu-vulnerability/) não são afetados pela atualização de microcode da Intel. Microsoft já colocar mitigações seguras no local para proteger os clientes do Azure a partir de outros inquilinos do Azure.  
>
> Microcode da Intel endereços variante 2 Spectre ([Exposures-2017-5715](https://www.cve.mitre.org/cgi-bin/cvename.cgi?name=2017-5715)) para proteger contra ataques que apenas seriam aplicável onde executar cargas de trabalho partilhadas ou não fidedignas dentro as suas VMs no Azure. A nossa engenheiros estiver a testar a estabilidade para minimizar os impactos no desempenho de microcode, antes de efetuar disponível para clientes do Azure.  Como os clientes muito poucos executam cargas de trabalho não fidedignas nas respetivas VMs, maioria dos clientes não terá de ativar esta capacidade, uma vez lançada. 
>
> Esta página será atualizada como estão disponíveis mais informações.  






## <a name="keeping-your-operating-systems-up-to-date"></a>Manter os sistemas de operativos atualizado

Enquanto uma atualização de SO não é necessário para isolar as suas aplicações em execução no Azure a partir de outros clientes em execução no Azure, é sempre melhor prática para manter as versões de SO atualizados. 

Nas ofertas de seguintes, eis nosso ações recomendadas para atualizar o sistema operativo: 

<table>
<tr>
<th>Oferta</th> <th>Ação Recomendada </th>
</tr>
<tr>
<td>Cloud Services do Azure </td>  <td>Permitir atualização automática ou certifique-se de que está a executar o SO convidado mais recentes.</td>
</tr>
<tr>
<td>Máquinas virtuais do Linux do Azure</td> <td>Instale atualizações através do seu fornecedor de sistema operativo, quando disponível. </td>
</tr>
<tr>
<td>Máquinas virtuais do Azure Windows </td> <td>Certifique-se de que está a executar uma aplicação antivírus suportada antes de instalar atualizações do SO. Contacte o fabricante de software antivírus de informações de compatibilidade.<p> Instalar o [agregação de segurança de Janeiro](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002). </p></td>
</tr>
<tr>
<td>Outros serviços do Azure PaaS</td> <td>Não há nenhuma ação necessária para clientes que utilizam estes serviços. Azure automaticamente mantém as versões de SO atualizado. </td>
</tr>
</table>

## <a name="additional-guidance-if-you-are-running-untrusted-code"></a>Orientações adicionais se estiver a executar o código não fidedigno 

É necessária nenhuma ação de cliente adicionais, a menos que estejam a executar o código não fidedigno. Se autorizar o código que não seja fidedigna (por exemplo, permitir que um dos seus clientes para carregar um binário ou fragmento de código que, em seguida, executar na nuvem na aplicação), em seguida, os seguintes passos adicionais que deverão ser executados.  


### <a name="windows"></a>Windows 
Se estiver a utilizar o Windows e a alojar o código não fidedigno, deve ativar também chamada Shadowing de endereço Virtual Kernel (KVA) que fornece proteção adicional contra vulnerabilidades de canal de lado de execução speculative uma funcionalidade do Windows. Esta funcionalidade está desativada por predefinição e pode afetar o desempenho se estiver ativada. Siga [Windows Server KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) instruções para ativar a proteção no servidor. Se estiver a executar o serviços de nuvem do Azure, certifique-se de que está a executar WA-convidado-so-5.15_201801-01 ou WA-convidado-SO-4.50_201801-01 (disponível a partir em 10 de Janeiro de 2018) e ativar o registo da chave através de uma tarefa de arranque.


### <a name="linux"></a>Linux
Se estiver a utilizar o Linux e alojar código não fidedigno, também deve atualizar o Linux para uma versão mais recente que implementa o isolamento de tabela de página de kernel (KPTI) que separa as tabelas de página utilizadas pelo kernel das pertencendo ao espaço de utilizador. Estes mitigações requerem uma atualização de SO Linux e podem ser obtidas através do seu fornecedor de distribuição quando disponível. O fornecedor de SO pode indicar-lhe se proteções ativadas ou desativadas por predefinição.



## <a name="next-steps"></a>Passos seguintes

Para obter mais informações, consulte [clientes proteger Azure da vulnerabilidade de CPU](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).
