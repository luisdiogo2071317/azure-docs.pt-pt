---
title: "Notas de versão do agente do Azure File Sync | Microsoft Docs"
description: "Notas de versão do Azure File Sync"
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: tamram
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: 9b6dfec6465482efcbf55d0441e44a0278f44a22
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="azure-file-sync-agent-release-notes"></a>Notas de versão do agente do Azure File Sync
O Azure File Sync (pré-visualização) permite-lhe centralizar as partilhas de ficheiros da sua organização nos Ficheiros do Azure sem abdicar da flexibilidade, do desempenho e da compatibilidade de um servidor de ficheiros no local. Isto é feito ao transformar os Servidores do Windows numa cache rápida da partilha de ficheiros do Azure. Pode utilizar qualquer protocolo disponível no Windows Server para aceder aos seus dados localmente (incluindo SMB, NFS e FTPS) e pode ter o número de caches que precisar em todo o mundo.

Este artigo aborda as notas de versão para versões suportadas do agente do Azure File Sync.

## <a name="supported-versions"></a>Versões suportadas
O Azure File Sync suporta as seguintes versões:

| Número de versão do agente | Data da versão | Suportado até |
|----------------------|--------------|------------------|
| 2.0.11.0 | 2018-02-08 | Versão atual |
| 1.1.0.0 | 2017-09-26 | 2018-07-30 |

### <a name="azure-file-sync-agent-update-policy"></a>Política de atualização do agente do Azure File Sync
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-20110"></a>Versão 2.0.11.0 do agente
As notas de versão seguintes destinam-se à versão 2.0.11.0 do agente lançada a 9 de fevereiro de 2018. 

### <a name="agent-installation-and-server-configuration"></a>Instalação do agente e configuração do servidor
Para obter mais informações sobre como instalar e configurar o agente do Azure File Sync com o Windows Server, veja [Planear uma implementação do Azure File Sync (pré-visualização)](storage-sync-files-planning.md) e [Como implementar o Azure File Sync (pré-visualização)](storage-sync-files-deployment-guide.md).

- O pacote de instalação do agente (MSI) tem de ser instalado com permissões elevadas (administrador).
- Não é suportado nas opções de implementação do Windows Server Core ou do Nano Server.
- É suportado apenas no Windows Server 2016 e 2012 R2.
- O agente requer, pelo menos, 2 GB de memória física.

### <a name="interoperability"></a>Interoperabilidade
- O antivírus, a cópia de segurança e outras aplicações que acedam a ficheiros em camadas podem causar uma remoção indesejável, a menos que respeitem o atributo offline e ignorem a leitura do conteúdo desses ficheiros. Para obter mais informações, veja [Resolver problemas do Azure File Sync (pré-visualização)](storage-sync-files-troubleshoot.md)
- Com esta atualização, o DFS-R passa a ser suportado.  Veja o [guia de planeamento](storage-sync-files-planning.md#distributed-file-system-dfs) para obter mais informações.
- Não utilize a filtragem de ficheiros do Gestor de Recursos do Servidor de Ficheiros (ou outro): as filtragens de ficheiros podem causar falhas de sincronização infinitas se os ficheiros forem bloqueados por esse motivo.
- A duplicação de um Servidor Registado (incluindo a clonagem de VMs) pode originar resultados inesperados (em particular, a sincronização pode nunca convergir).
- A eliminação de duplicados de dados e a disposição em camadas na cloud não são suportadas no mesmo volume.
 
### <a name="sync-limitations"></a>Limitações de sincronização
Os seguintes itens não são sincronizados, mas o resto do sistema irá continuar a funcionar normalmente:
- Caminhos com mais de 2048 carateres
- Parte DACL de um descritor de segurança superior a 2 mil (isto só é um problema se tiver mais de 40 Entradas de Controlo de Acesso num único item)
- Parte SACL do descritor de segurança (utilizada para auditoria)
- Atributos expandidos
- Fluxos de dados alternativos
- Pontos de reanálise
- Ligações fixas
- A compressão (se estiver definida num ficheiro de servidor) não será mantida quando as alterações forem sincronizadas nesse ficheiro a partir de outros pontos finais
- Qualquer ficheiro encriptado com EFS (ou outra encriptação do modo de utilizador) que impeça o nosso serviço de ler os dados 
    
    > [!Note]  
    > O Azure File Sync encripta sempre os dados em trânsito e os dados podem ser encriptados quando estão inativos no Azure.
 
### <a name="server-endpoints"></a>Pontos Finais do Servidor
- Um ponto final de servidor só pode ser criado num volume NTFS. ReFS, FAT, FAT32 e outros sistemas de ficheiros não são suportados pelo Azure File Sync neste momento.
- Um ponto final de servidor pode não estar no volume do sistema (por exemplo, C:\MyFolder não é um caminho aceitável, a menos que C:\MyFolder seja um ponto de montagem).
- O Clustering de Ativação Pós-falha só é suportado com Discos em Cluster e não com Volumes Partilhados de Cluster (CSVs).
- Um ponto final de servidor não pode ser aninhado, mas pode coexistir no mesmo volume em paralelo entre si.
- A eliminação de um grande número de diretórios de um servidor em simultâneo (mais de 10 000) pode causar falhas de sincronização; elimine os diretórios em lotes inferiores a 10 000 e certifique-se de que a sincronização de operações de eliminação é efetuada com êxito antes de eliminar o lote seguinte.
- Com esta versão, foi adicionado suporte para a raiz de sincronização que está na raiz de um volume.
- Não armazene um SO ou o ficheiro de paginação de uma aplicação num ponto final de servidor.
- Nesta versão, foram adicionados novos eventos para controlar o runtime total das camadas da cloud (EventID 9016), o progresso de carregamento da sincronização (EventID 9302) e os ficheiros que não foram sincronizados (EventID 9900).
- Nesta versão, o desempenho da sincronização do espaço de nomes de DR aumentou significativamente.
 
### <a name="cloud-tiering"></a>Disposição em camadas na cloud
- Nesta nova versão, os novos ficheiros serão dispostos em camadas no espaço de 1 hora, em conformidade com a definição de políticas de camadas (anteriormente, 32 horas). Proporcionamos um cmdlet do PowerShell para a disposição em camadas a pedido, para que possa avaliar as camadas de forma mais eficiente sem aguardar pelo processo em segundo plano.
- Se um ficheiro em camadas for copiado através do Robocopy para outra localização, o ficheiro resultante não será colocado em camadas, mas o atributo offline pode ser definido, uma vez que o Robocopy inclui incorretamente esse atributo nas operações de cópia.
- Ao visualizar as propriedades do ficheiro de um cliente do SMB, o atributo offline pode parecer estar definido incorretamente devido à colocação em cache de metadados de ficheiros do SMB.
- Nesta nova versão, os ficheiros são agora transferidos como ficheiros em camadas noutros servidores, desde que o ficheiro seja novo ou já fosse um ficheiro em camadas.

## <a name="agent-version-1100"></a>Versão do agente 1.1.0.0
As notas de versão seguintes destinam-se à versão do agente 1.1.0.0 lançada a 9 de setembro de 2017. Este lançamento marca a versão de Pré-visualização inicial do Azure File Sync!

### <a name="agent-installation-and-server-configuration"></a>Instalação do agente e configuração do servidor
Para obter mais informações sobre como instalar e configurar o agente do Azure File Sync com o Windows Server, veja [Planear uma implementação do Azure File Sync (pré-visualização)](storage-sync-files-planning.md) e [Como implementar o Azure File Sync (pré-visualização)](storage-sync-files-deployment-guide.md).

- O pacote de instalação do agente (MSI) tem de ser instalado com permissões elevadas (administrador).
- Não é suportado nas opções de implementação do Windows Server Core ou do Nano Server.
- É suportado apenas no Windows Server 2016 e 2012 R2.
- O agente requer, pelo menos, 2 GB de memória física.

### <a name="interoperability"></a>Interoperabilidade
- O antivírus, a cópia de segurança e outras aplicações que acedam a ficheiros em camadas podem causar uma remoção indesejável, a menos que respeitem o atributo offline e ignorem a leitura do conteúdo desses ficheiros. Para obter mais informações, veja [Resolver problemas do Azure File Sync (pré-visualização)](storage-sync-files-troubleshoot.md)
- Não utilize a filtragem de ficheiros do Gestor de Recursos do Servidor de Ficheiros (ou outro): as filtragens de ficheiros podem causar falhas de sincronização infinitas se os ficheiros forem bloqueados por esse motivo.
- A duplicação de um Servidor Registado (incluindo a clonagem de VMs) pode originar resultados inesperados (em particular, a sincronização pode nunca convergir).
- A eliminação de duplicados de dados e a disposição em camadas na cloud não são suportadas no mesmo volume.
 
### <a name="sync-limitations"></a>Limitações de sincronização
Os seguintes itens não são sincronizados, mas o resto do sistema irá continuar a funcionar normalmente:
- Caminhos com mais de 2048 carateres
- Parte DACL de um descritor de segurança superior a 2 mil (isto só é um problema se tiver mais de 40 Entradas de Controlo de Acesso num único item)
- Parte SACL do descritor de segurança (utilizada para auditoria)
- Atributos expandidos
- Fluxos de dados alternativos
- Pontos de reanálise
- Ligações fixas
- A compressão (se estiver definida num ficheiro de servidor) não será mantida quando as alterações forem sincronizadas nesse ficheiro a partir de outros pontos finais
- Qualquer ficheiro encriptado com EFS (ou outra encriptação do modo de utilizador) que impeça o nosso serviço de ler os dados 
    
    > [!Note]  
    > O Azure File Sync encripta sempre os dados em trânsito e os dados podem ser encriptados quando estão inativos no Azure.
 
### <a name="server-endpoints"></a>Pontos Finais do Servidor
- Um ponto final de servidor só pode ser criado num volume NTFS. ReFS, FAT, FAT32 e outros sistemas de ficheiros não são suportados pelo Azure File Sync neste momento.
- Um ponto final de servidor pode não estar no volume do sistema (por exemplo, C:\MyFolder não é um caminho aceitável, a menos que C:\MyFolder seja um ponto de montagem).
- O Clustering de Ativação Pós-falha só é suportado com Discos em Cluster e não com Volumes Partilhados de Cluster (CSVs).
- Um ponto final de servidor não pode ser aninhado, mas pode coexistir no mesmo volume em paralelo entre si.
- A eliminação de um grande número de diretórios de um servidor em simultâneo (mais de 10 000) pode causar falhas de sincronização; elimine os diretórios em lotes inferiores a 10 000 e certifique-se de que a sincronização de operações de eliminação é efetuada com êxito antes de eliminar o lote seguinte.
- Não suportado na raiz de um volume.
- Não armazene um SO ou o ficheiro de paginação de uma aplicação num ponto final de servidor.
 
### <a name="cloud-tiering"></a>Disposição em camadas na cloud
- Para garantir que os ficheiros podem ser removidos corretamente, o sistema pode não dispor em camadas automaticamente os ficheiros novos ou alterados durante até 32 horas, incluindo a primeira disposição em camadas após a configuração de um novo Ponto Final de Servidor. Fornecemos um cmdlet do PowerShell para camadas a pedido para que possa avaliar a disposição em camadas de forma mais eficiente sem aguardar pelo processo em segundo plano.
- Se um ficheiro em camadas for copiado através do Robocopy para outra localização, o ficheiro resultante não será colocado em camadas, mas o atributo offline pode ser definido, uma vez que o Robocopy inclui incorretamente esse atributo nas operações de cópia.
- Ao visualizar as propriedades do ficheiro de um cliente do SMB, o atributo offline pode parecer estar definido incorretamente devido à colocação em cache de metadados de ficheiros do SMB.
