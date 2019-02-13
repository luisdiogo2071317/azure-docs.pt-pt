---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 9cd5a63b5b1d7000605f90752be755356d40bb4a
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56213167"
---
>[!NOTE]
>Para recursos que não está corrigidos, pode abrir um pedido de suporte para pedir o aumento das quotas. **Não** crie contas dos Serviços de Multimédia do Azure adicionais numa tentativa de obter limites superiores.

| Recurso | Limite Predefinido | 
| --- | --- | 
| Contas dos Serviços de Multimédia do Azure (AMS) numa subscrição individual | 25 (fixo) |
| Unidades Reservadas de Multimédia (RUs) por conta do AMS |25 (S1)<br/>10 (S2, S3) <sup>(1)</sup> | 
| Trabalhos por conta do AMS | 50,000<sup>(2)</sup> |
| Tarefas em cadeia por trabalho | 30 (fixo) |
| Elementos por conta do AMS | 1 000 000|
| Elementos por tarefa | 50 |
| Elementos por trabalho | 100 |
| Localizadores exclusivos associados a um elemento ao mesmo tempo | 5<sup>(4)</sup> |
| Canais em direto por conta do AMS |5|
| Programas no estado de paragem por canal |50|
| Programas no estado parado por canal |3|
| Pontos finais de transmissão em fluxo (parada ou em execução) por conta dos Media Services|2|
| Unidades de transmissão em fluxo por ponto final de transmissão em fluxo |10 |
| Contas de armazenamento | 1000<sup>(5)</sup> (fixo) |
| Políticas | 1,000,000<sup>(6)</sup> |
| Tamanho dos ficheiros| Em alguns cenários, existe um limite no tamanho de ficheiro máximo suportado para processamento nos serviços de multimédia. <sup>7</sup> |
  
<sup>1</sup> se alterar o tipo (por exemplo, a partir de S2 para S1), os limites máx. de RU são repostos.

<sup>2</sup> Este número inclui trabalhos em fila, concluídos, ativos e cancelados. Não inclui trabalhos eliminados. Pode utilizar **IJob.Delete** ou o pedido HTTP **DELETE** para eliminar os trabalhos antigos.

A partir de 1 de Abril de 2017, qualquer registo de tarefa na sua conta mais de 90 dias será automaticamente eliminado, juntamente com os seus registos de tarefas associados, mesmo se o número total de registos for inferior à quota máxima. Se precisar de arquivar as informações de tarefas, pode utilizar o código descrito [aqui](../articles/media-services/previous/media-services-dotnet-manage-entities.md).

<sup>3</sup> ao fazer um pedido de lista tarefa entidades, um máximo de 1.000 tarefas é devolvido por pedido. Se tiver de estar a par de todos os Trabalhos submetidos, pode utilizar “top/skip”, conforme descrito em [OData system query options (Opções de consultas de sistema OData)](https://msdn.microsoft.com/library/gg309461.aspx).

<sup>4</sup> Os localizadores não foram concebidos para gerir o controlo de acesso por utilizador. Para conceder direitos de acesso diferentes a utilizadores individuais, utilize soluções de Gestão de Direitos Digitais (Digital Rights Management, DRM). Para obter mais informações, veja [esta](../articles/media-services/previous/media-services-content-protection-overview.md) secção.

<sup>5</sup> As contas de armazenamento têm de estar na mesma subscrição do Azure.

<sup>6</sup> Existe um limite de um milhão de políticas para diferentes políticas do AMS (por exemplo, para a política Locator ou ContentKeyAuthorizationPolicy). 

>[!NOTE]
> Se estiver sempre a utilizar os mesmos dias, permissões de acesso, etc., deve utilizar o mesmo ID de política. Para obter informações e um exemplo, consulte [esta](../articles/media-services/previous/media-services-dotnet-manage-entities.md#limit-access-policies) secção.

<sup>7</sup>se estiver a carregar conteúdo para um elemento nos serviços de multimédia do Azure para processá-lo com um dos processadores de multimédia no serviço (ou seja, codificadores como o Media Encoder Standard e Media Encoder Premium Workflow ou motores de análise como detetor de caras), em seguida, deve estar ciente das restrições sobre os tamanhos de ficheiro máximo suportados. 

O tamanho máximo suportado para um único blob está atualmente até 5 TB de armazenamento de Blobs do Azure. No entanto, os limites adicionais aplicam-se nos serviços de multimédia do Azure com a base sobre os tamanhos VM que são utilizados pelo serviço. A tabela seguinte mostra os limites em cada um da unidades reservadas de multimédia (S1, S2, S3.) Se o ficheiro de origem for maior do que os limites definidos na tabela, a tarefa de codificação irá falhar. Se está a codificar fontes de resolução de 4K de longa duração, tem de utilizar unidades reservadas de multimédia S3 para alcançar o desempenho necessário. Se tiver conteúdo de 4K, que é maior do que o limite de 260 GB a unidades reservadas de multimédia S3, contacte-nos em amshelp@microsoft.com para possíveis atenuações suportar o seu cenário.

| Tipo de Unidade Reservada de Multimédia | Tamanho máximo de entrada (GB)| 
| --- | --- | 
|S1 | 325|
|S2 | 640|
|S3 | 260|
