---
title: Notas de versão para o agente do Azure File Sync (pré-visualização) | Microsoft Docs
description: Notas de versão para o agente de sincronização de ficheiros do Azure (pré-visualização).
services: storage
author: wmgries
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 03/12/2018
ms.author: wgries
ms.openlocfilehash: b42287580078b4391ddbc5b8ff2835131c64236d
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="release-notes-for-the-azure-file-sync-agent-preview"></a>Notas de versão para o agente do Azure File Sync (pré-visualização)
O Azure File Sync permite-lhe centralizar as partilhas de ficheiros da sua organização nos Ficheiros do Azure sem abdicar da flexibilidade, do desempenho e da compatibilidade de um servidor de ficheiros no local. As suas instalações do Windows Server são transformadas numa cache rápida da sua partilha de ficheiros do Azure. Pode utilizar qualquer protocolo disponível no Windows Server para aceder aos seus dados localmente (incluindo SMB, NFS e FTPS). Pode ter o número de caches que precisar em todo o mundo.

Este artigo disponibiliza as notas de versão das versões suportadas do agente do Azure File Sync.

## <a name="supported-versions"></a>Versões suportadas
São suportadas as seguintes versões para o agente do Azure File Sync:

| Referência | Número de versão do agente | Data da versão | Estado |
|----|----------------------|--------------|------------------|
| Rollup de atualização de Março | 2.2.0.0 | 12 de Março de 2018 | Suportado (versão recomendada) |
| Rollup de atualização de Fevereiro | 2.1.0.0 | 28 de fevereiro de 2018 | Suportadas |
| Atualizar 1 | 2.0.11.0 | 8 de fevereiro de 2018 | Suportadas |
| Rollup de atualização de Janeiro | 1.4.0.0 | 8 de Janeiro de 2018 | Suportado até 8 de Maio de 2018<sup>1</sup> |
| Update rollup de Novembro | 1.3.0.0 | 30 de Novembro de 2017 | Suportado até 8 de Maio de 2018<sup>1</sup> |
| Rollup de atualização de Outubro | 1.2.0.0 | 31 de outubro de 2017 | Suportado até 8 de Maio de 2018<sup>1</sup> |
| Versão de pré-visualização inicial | 1.1.0.0 | 26 de setembro de 2017 | Suportado até 8 de Maio de 2018<sup>1</sup> |

\[1\]: versões do agente de sincronização de ficheiros do Azure durante a pré-visualização intencionalmente não está em conformidade com a política de atualização. A política de atualização será imposta a partir da primeira versão do agente após a sincronização de ficheiros do Azure está declarada geralmente disponível.

### <a name="azure-file-sync-agent-update-policy"></a>Política de atualização do agente do Azure File Sync
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-2200"></a>Versão do agente 2.2.0.0
As seguintes notas de versão destinam-se a versão 2.2.0.0 do agente de sincronização de ficheiros do Azure lançada 12 de Março de 2018.  Estas notas são adicionais relativamente às notas de versão listadas para a versão 2.1.0.0 e 2.0.11.0

Instalação de v2.1.0.0 para alguns clientes irão falhar devido a FileSyncSvc não parar. Esta atualização correções que emitem.

## <a name="agent-version-2100"></a>Versão 2.1.0.0 do agente
As notas de versão seguintes destinam-se à versão 2.1.0.0 do agente do Azure File Sync disponibilizada em 28 de fevereiro de 2018. Estas notas complementam as notas de versão enumeradas para a versão 2.0.11.0.

Esta versão inclui as seguintes alterações:
- Melhoramentos no processamento da ativação pós-falha do cluster.
- Melhoramentos no processamento fiável de ficheiros em camadas.
- Suporte para a instalação do agente em máquinas de controlador de domínio adicionadas a um ambiente de domínio do Windows Server 2008 R2.
- Corrigido nesta versão: geração de diagnósticos em excesso em servidores com muitos ficheiros.
- Melhoramentos no processamento de erros de falhas de sessão.
- Melhoramentos no processamento de erros de falhas de transferência de ficheiros.
- Alterado nesta versão: o intervalo predefinido para executar a disposição em camadas na cloud quando esta está ativada num ponto final do servidor é agora de 1 hora. 
- Problema de bloqueio temporário: mudança de recursos do Azure File Sync (Serviço de Sincronização de Armazenamento) para uma nova subscrição do Azure.

## <a name="agent-version-20110"></a>Versão 2.0.11.0 do agente
As notas de versão seguintes destinam-se à versão 2.0.11.0 do agente do Azure File Sync (disponibilizada em 9 de fevereiro de 2018). 

### <a name="agent-installation-and-server-configuration"></a>Instalação do agente e configuração do servidor
Para obter mais informações sobre como instalar e configurar o agente do Azure File Sync com o Windows Server, veja [Planear uma implementação do Azure File Sync (pré-visualização)](storage-sync-files-planning.md) e [Como implementar o Azure File Sync (pré-visualização)](storage-sync-files-deployment-guide.md).

- O pacote de instalação do agente (MSI) tem de ser instalado com permissões elevadas (administrador).
- O agente não é suportado nas opções de implementação do Windows Server Core ou do Nano Server.
- O agente só é suportado no Windows Server 2016 e no Windows Server 2012 R2.
- O agente requer, pelo menos, 2 GB de memória física.

### <a name="interoperability"></a>Interoperabilidade
- O antivírus, a cópia de segurança e outras aplicações que acedam a ficheiros em camadas podem causar uma revogação indesejável, a menos que respeitem o atributo offline e ignorem a leitura do conteúdo desses ficheiros. Para obter mais informações, veja [Resolver problemas do Azure File Sync (pré-visualização)](storage-sync-files-troubleshoot.md).
- Esta versão adiciona suporte para DFS-R. Para obter mais informações, veja o [Guia de planeamento](storage-sync-files-planning.md#distributed-file-system-dfs).
- Não utilize o Gestor de Recursos do Servidor de Ficheiros (FSRM) ou outras filtragens de ficheiros. As filtragens de ficheiros podem provocar inúmeras falhas de sincronização quando os ficheiros são bloqueados devido à filtragem de ficheiros.
- A duplicação de Servidores Registados (incluindo a clonagem de VMs) pode dar origem a resultados inesperados. Em particular, a sincronização poderá nunca convergir.
- A eliminação de duplicados de dados e a disposição em camadas na cloud não são suportadas no mesmo volume.
 
### <a name="sync-limitations"></a>Limitações de sincronização
Os itens seguintes não são sincronizados, mas o restante sistema continua a funcionar normalmente:
- Caminhos com mais de 2048 carateres.
- A parte da lista de controlo de acesso discricionário (DACL) de um descritor de segurança se for superior a 2 KB. (Este problema aplica-se apenas quando tem mais do que cerca de 40 registos de controlo de acesso (ACEs) num único item.)
- A parte da lista de controlo de acesso do sistema (SACL) de um descritor de segurança utilizado para auditoria.
- Atributos expandidos.
- Fluxos de dados alternados.
- Pontos de reanálise.
- Ligações fixas.
- A compressão (se estiver definida num ficheiro de servidor) não é mantida quando as alterações são sincronizadas nesse ficheiro a partir de outros pontos finais.
- Qualquer ficheiro encriptado com EFS (ou outra encriptação do modo de utilizador) que impeça a leitura dos dados por parte do serviço. 
    
    > [!Note]  
    > O Azure File Sync encripta sempre os dados em trânsito. Os dados são sempre encriptados quando estão inativos no Azure.
 
### <a name="server-endpoints"></a>Pontos finais do servidor
- Só é possível criar um ponto final do servidor num volume NTFS. O ReFS, FAT, FAT32 e outros sistemas de ficheiros não são atualmente suportados pelo Azure File Sync.
- Um ponto final do servidor não pode encontrar-se no volume de sistema. Por exemplo, C:\AMinhaPasta não é um caminho aceitável, a menos que C:\AMinhaPasta seja um ponto de montagem.
- O Clustering de Ativação Pós-falha só é suportado com discos em cluster, mas não com Volumes Partilhados de Cluster (CSVs).
- Não é possível aninhar um ponto final do servidor. Pode coexistir no mesmo volume em paralelo com outro ponto final.
- A eliminação de um grande número de diretórios (mais de 10.000) a partir de um servidor, de uma só vez, pode provocar falhas de sincronização. Elimine diretórios em lotes com menos de 10.000. Certifique-se de que as operações de eliminação são sincronizadas com êxito antes de eliminar o lote seguinte.
- Esta versão adiciona suporte para a raiz de sincronização na raiz de um volume.
- Não armazene o ficheiro de paginação de uma aplicação ou SO que se encontre num ponto final do servidor.
- Alterado nesta versão: foram adicionados novos eventos para controlar o runtime total da disposição em camadas na cloud (EventID 9016), o progresso do carregamento da sincronização (EventID 9302) e os ficheiros que não foram sincronizados (EventID 9900).
- Alterado nesta versão: o desempenho da sincronização do espaço de nomes DR aumentou significativamente.
 
### <a name="cloud-tiering"></a>Disposição em camadas na cloud
- Alterado em relação à versão anterior: os ficheiros novos são dispostos em camadas no espaço de 1 hora (antes: 32 horas), sujeito à definição de política de disposição em camadas. Fornecemos um cmdlet do PowerShell para efetuar a disposição em camadas a pedido. Pode utilizar o cmdlet para avaliar a disposição em camadas de forma mais eficiente sem ter de aguardar pelo processo em segundo plano.
- Se um ficheiro disposto em camadas for copiado para outra localização com o Robocopy, o ficheiro resultante não é disposto em camadas. O atributo offline pode estar definido porque o Robocopy inclui incorretamente esse atributo nas operações de cópia.
- Ao visualizar as propriedades do ficheiro a partir de um cliente SMB, o atributo offline pode parecer estar definido incorretamente devido à colocação em cache dos metadados de ficheiros por parte do SMB.
- Alterado em relação à versão anterior: os ficheiros são agora transferidos como ficheiros em camadas noutros servidores, desde que o ficheiro seja novo ou já seja um ficheiro em camadas.

## <a name="agent-version-1100"></a>Versão do agente 1.1.0.0
As notas de versão seguintes destinam-se à versão 1.1.0.0 do agente do Azure File Sync (disponibilizada em 9 de setembro de 2017, pré-visualização inicial). 

### <a name="agent-installation-and-server-configuration"></a>Instalação do agente e configuração do servidor
Para obter mais informações sobre como instalar e configurar o agente do Azure File Sync com o Windows Server, veja [Planear uma implementação do Azure File Sync (pré-visualização)](storage-sync-files-planning.md) e [Como implementar o Azure File Sync (pré-visualização)](storage-sync-files-deployment-guide.md).

- O pacote de instalação do agente (MSI) tem de ser instalado com permissões elevadas (administrador).
- O agente não é suportado nas opções de implementação do Windows Server Core ou do Nano Server.
- O agente é suportado apenas no Windows Server 2016 e 2012 R2.
- O agente requer, pelo menos, 2 GB de memória física.

### <a name="interoperability"></a>Interoperabilidade
- O antivírus, a cópia de segurança e outras aplicações que acedam a ficheiros em camadas podem causar uma revogação indesejável, a menos que respeitem o atributo offline e ignorem a leitura do conteúdo desses ficheiros. Para obter mais informações, veja [Resolver problemas do Azure File Sync (pré-visualização)](storage-sync-files-troubleshoot.md).
- Não utilize o FSRM ou outras filtragens de ficheiros. As filtragens de ficheiros podem provocar inúmeras falhas de sincronização quando os ficheiros são bloqueados devido à filtragem de ficheiros.
- A duplicação de Servidores Registados (incluindo a clonagem de VMs) pode dar origem a resultados inesperados. Em particular, a sincronização poderá nunca convergir.
- A eliminação de duplicados de dados e a disposição em camadas na cloud não são suportadas no mesmo volume.
 
### <a name="sync-limitations"></a>Limitações de sincronização
Os itens seguintes não são sincronizados, mas o restante sistema continua a funcionar normalmente:
- Caminhos com mais de 2048 carateres.
- A parte da DACL de um descritor de segurança se for superior a 2 KB. (Este problema aplica-se apenas quando tem mais do que cerca de 40 ACEs num único item.)
- A parte da SACL de um descritor de segurança utilizada para auditoria.
- Atributos expandidos.
- Fluxos de dados alternados.
- Pontos de reanálise.
- Ligações fixas.
- A compressão (se estiver definida num ficheiro de servidor) não é mantida quando as alterações são sincronizadas nesse ficheiro a partir de outros pontos finais.
- Qualquer ficheiro encriptado com EFS (ou outra encriptação do modo de utilizador) que impeça a leitura dos dados por parte do serviço. 
    
    > [!Note]  
    > O Azure File Sync encripta sempre os dados em trânsito. Os dados são sempre encriptados quando estão inativos no Azure.
 
### <a name="server-endpoints"></a>Pontos finais do servidor
- Só é possível criar um ponto final do servidor num volume NTFS. O ReFS, FAT, FAT32 e outros sistemas de ficheiros não são atualmente suportados pelo Azure File Sync.
- Um ponto final do servidor não pode encontrar-se no volume de sistema. Por exemplo, C:\AMinhaPasta não é um caminho aceitável, a menos que C:\AMinhaPasta seja um ponto de montagem.
- O Clustering de Ativação Pós-falha só é suportado com discos em cluster e não com CSVs.
- Não é possível aninhar um ponto final do servidor. Pode coexistir no mesmo volume em paralelo com outro ponto final.
- A eliminação de um grande número de diretórios (mais de 10.000) a partir de um servidor, de uma só vez, pode provocar falhas de sincronização. Elimine diretórios em lotes com menos de 10.000. Certifique-se de que as operações de eliminação são sincronizadas com êxito antes de eliminar o lote seguinte.
- Um ponto final do servidor na raiz de um volume não é atualmente suportado.
- Não armazene o ficheiro de paginação de uma aplicação ou SO que se encontre num ponto final do servidor.
 
### <a name="cloud-tiering"></a>Disposição em camadas na cloud
- A fim de garantir que os ficheiros são revogados corretamente, o sistema poderá não dispor automaticamente em camadas os ficheiros novos ou alterados durante um período de 32 horas. Este processo inclui a disposição em camadas inicial após a configuração de um novo ponto final do servidor. Fornecemos um cmdlet do PowerShell para efetuar a disposição em camadas a pedido. Pode utilizar o cmdlet para avaliar a disposição em camadas de forma mais eficiente sem ter de aguardar pelo processo em segundo plano.
- Se um ficheiro disposto em camadas for copiado para outra localização com o Robocopy, o ficheiro resultante não é disposto em camadas. O atributo offline pode estar definido porque o Robocopy inclui incorretamente esse atributo nas operações de cópia.
- Ao visualizar as propriedades do ficheiro a partir de um cliente SMB, o atributo offline pode parecer estar definido incorretamente devido à colocação em cache dos metadados de ficheiros por parte do SMB.