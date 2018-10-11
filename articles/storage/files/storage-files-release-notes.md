---
title: Notas de versão do agente do Azure File Sync | Documentos da Microsoft
description: Notas de versão do agente do Azure File Sync.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 10/10/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: 6f39088416f4b2c3d9b04a3d83c41c8738b2c595
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079502"
---
# <a name="release-notes-for-the-azure-file-sync-agent"></a>Notas de versão do agente do Azure File Sync
O Azure File Sync permite-lhe centralizar as partilhas de ficheiros da sua organização nos Ficheiros do Azure sem abdicar da flexibilidade, do desempenho e da compatibilidade de um servidor de ficheiros no local. As suas instalações do Windows Server são transformadas numa cache rápida da sua partilha de ficheiros do Azure. Pode utilizar qualquer protocolo disponível no Windows Server para aceder aos seus dados localmente (incluindo SMB, NFS e FTPS). Pode ter o número de caches que precisar em todo o mundo.

Este artigo disponibiliza as notas de versão das versões suportadas do agente do Azure File Sync.

## <a name="supported-versions"></a>Versões suportadas
São suportadas as seguintes versões para o agente do Azure File Sync:

| Etapa | Número de versão do agente | Data da versão | Estado |
|----|----------------------|--------------|------------------|
| Pacote cumulativo de atualizações de Setembro | 3.3.0.0 | 24 de setembro de 2018 | Suportado (versão recomendada) |
| Pacote cumulativo de atualizações de Agosto | 3.2.0.0 | 15 de Agosto de 2018 | Suportadas |
| Disponibilidade geral | 3.1.0.0 | 19 de Julho de 2018 | Suportadas |
| Rollup de atualização de Junho | 3.0.13.0 | 29 de Junho de 2018 | Não suportado - a expirou de versão do agente de 1 de Outubro de 2018 |
| Atualização 2 | 3.0.12.0 | 22 de maio de 2018 | Não suportado - a expirou de versão do agente de 1 de Outubro de 2018 |
| Rollup de atualização de Abril | 2.3.0.0 | 8 de Maio de 2018 | Não suportado - a expirou de versão do agente de 1 de Outubro de 2018 |
| Pacote cumulativo de atualizações de Março | 2.2.0.0 | 12 de Março de 2018 | Não suportado - a expirou de versão do agente de 1 de Outubro de 2018 |
| Pacote cumulativo de atualizações de Fevereiro | 2.1.0.0 | 28 de fevereiro de 2018 | Não suportado - a expirou de versão do agente de 1 de Outubro de 2018 |
| Atualização 1 | 2.0.11.0 | 8 de fevereiro de 2018 | Não suportado - a expirou de versão do agente de 1 de Outubro de 2018 |
| Pacote cumulativo de atualizações de Janeiro | 1.4.0.0 | 8 de Janeiro de 2018 | Não suportado - a expirou de versão do agente de 1 de Outubro de 2018 |
| Update rollup de Novembro | 1.3.0.0 | 30 de Novembro de 2017 | Não suportado - a expirou de versão do agente de 1 de Outubro de 2018 |
| Pacote cumulativo de atualizações de Outubro | 1.2.0.0 | 31 de outubro de 2017 | Não suportado - a expirou de versão do agente de 1 de Outubro de 2018 |
| Versão de pré-visualização inicial | 1.1.0.0 | 26 de setembro de 2017 | Não suportado - a expirou de versão do agente de 1 de Outubro de 2018 |

### <a name="azure-file-sync-agent-update-policy"></a>Política de atualização do agente do Azure File Sync
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-3300"></a>Versão do agente 3.3.0.0
As notas de versão seguintes destinam-se a versão 3.3.0.0 do agente do Azure File Sync disponibilizada 24 de Setembro de 2018. Estas notas são adicionais as notas de versão enumeradas para a versão 3.1.0.0.

Esta versão inclui a correção seguinte:
- Estado do servidor registado é "Aparece offline" após o agente é atualizado para a versão 3.1 ou 3.2 do Azure File Sync.
- Serviço de agente de sincronização (FileSyncSvc) de armazenamento falha devido a ficheiros que tenham caminhos longos.
- Falha de registo do servidor com o erro: não foi possível carregar o ficheiro ou a assemblagem Kailani.Afs.StorageSyncProtocol.V3.

## <a name="agent-version-3200"></a>Versão do agente 3.2.0.0
As notas de versão seguintes destinam-se a versão 3.2.0.0 do agente do Azure File Sync disponibilizada 15 de Agosto de 2018. Estas notas são adicionais as notas de versão enumeradas para a versão 3.1.0.0.

Esta versão inclui a correção seguinte:
- Falha de sincronização com fora de erro de memória (0x8007000e) devido a fuga de memória

## <a name="agent-version-3100"></a>Versão do agente 3.1.0.0
As notas de versão seguintes destinam-se a versão 3.1.0.0 do agente do Azure File Sync (disponibilizada a 19 de Julho de 2018).

### <a name="evaluation-tool"></a>Ferramenta de avaliação
Antes de implementar o Azure File Sync, deve avaliar se é compatível com o seu sistema usando a ferramenta de avaliação do Azure File Sync. Essa ferramenta é um cmdlet do AzureRM PowerShell que verifica a existência de potenciais problemas com o seu sistema de ficheiros e o conjunto de dados, tais como carateres não suportados ou uma versão de SO não suportada. Para instalação e instruções de utilização, consulte [ferramenta de avaliação](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-tool) secção no guia de planejamento. 

### <a name="agent-installation-and-server-configuration"></a>Instalação do agente e configuração do servidor
Para obter mais informações sobre como instalar e configurar o agente de sincronização de ficheiros do Azure com o Windows Server, consulte [planear uma implementação do Azure File Sync](storage-sync-files-planning.md) e [como implementar o Azure File Sync](storage-sync-files-deployment-guide.md).

- O pacote de instalação do agente tem de ser instalado com permissões elevadas (administrador).
- O agente não é suportado nas opções de implementação do Windows Server Core ou Nano Server.
- O agente só é suportado no Windows Server 2016 e no Windows Server 2012 R2.
- O agente requer, pelo menos, 2 GB de memória física.
- O serviço de agente de sincronização de armazenamento (FileSyncSvc) não suporta pontos finais do servidor localizados num volume que tenha o diretório de informações (SVI) do volume de sistema comprimido. Esta configuração irá levar a resultados inesperados.

### <a name="interoperability"></a>Interoperabilidade
- O antivírus, a cópia de segurança e outras aplicações que acedam a ficheiros em camadas podem causar uma revogação indesejável, a menos que respeitem o atributo offline e ignorem a leitura do conteúdo desses ficheiros. Para obter mais informações, consulte [resolver problemas relacionados com o Azure File Sync](storage-sync-files-troubleshoot.md).
- Não utilize o Gestor de Recursos do Servidor de Ficheiros (FSRM) ou outras filtragens de ficheiros. As filtragens de ficheiros podem provocar inúmeras falhas de sincronização quando os ficheiros são bloqueados devido à filtragem de ficheiros.
- Executar o sysprep num servidor que tem instalado o agente de sincronização de ficheiros do Azure não é suportado e pode levar a resultados inesperados. Registo de servidor e a instalação do agente deve ocorrer depois de implementar a imagem do servidor e concluir a mini-configuração de sysprep.
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
 
### <a name="server-endpoint"></a>Ponto final do servidor
- Só é possível criar um ponto final do servidor num volume NTFS. O ReFS, FAT, FAT32 e outros sistemas de ficheiros não são atualmente suportados pelo Azure File Sync.
- Ficheiros em camadas ficará inutilizáveis se os ficheiros não são removidos antes de eliminar o ponto final do servidor.
- Na cloud em camadas não é suportada no volume do sistema. Para criar um ponto final do servidor no volume do sistema, desative a camada ao criar o ponto final do servidor de cloud.
- O Clustering de Ativação Pós-falha só é suportado com discos em cluster, mas não com Volumes Partilhados de Cluster (CSVs).
- Não é possível aninhar um ponto final do servidor. Pode coexistir no mesmo volume em paralelo com outro ponto final.
- Não armazene o ficheiro de paginação de uma aplicação ou SO que se encontre num ponto final do servidor.
- O nome do servidor no portal não é atualizado se o servidor for renomeado. Para atualizar o nome do servidor no portal, anule o registo e voltar a registar o servidor.

### <a name="cloud-endpoint"></a>Ponto final da cloud
- O Azure File Sync suporta diretamente a efetuar alterações à partilha de ficheiros do Azure. No entanto, todas as alterações feitas na partilha de ficheiros do Azure primeiro tem de ser detetado por uma tarefa de deteção de alteração de sincronização de ficheiros do Azure. Uma tarefa de deteção de alteração de início, de um ponto final da cloud, uma vez a cada 24 horas. Além disso, as alterações feitas para uma partilha de ficheiros do Azure através do protocolo REST não atualizará a hora da última modificação de SMB e não irão ser vistas como uma alteração por sincronização.
- O serviço de sincronização de armazenamento e/ou a conta de armazenamento pode ser movida para um grupo de recursos diferente ou de uma subscrição no inquilino do Azure AD existente. Se a conta de armazenamento for movida, precisa dar o acesso de serviço de sincronização de ficheiros de híbrida para a conta de armazenamento (veja [Certifique-se o Azure File Sync tem acesso à conta de armazenamento](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > O Azure File Sync não suporta a mover a subscrição para um Azure diferente inquilino do AD.

### <a name="cloud-tiering"></a>Disposição em camadas na cloud
- Se um ficheiro disposto em camadas for copiado para outra localização com o Robocopy, o ficheiro resultante não é disposto em camadas. O atributo offline pode estar definido porque o Robocopy inclui incorretamente esse atributo nas operações de cópia.
- Ao visualizar as propriedades do ficheiro a partir de um cliente SMB, o atributo offline pode parecer estar definido incorretamente devido à colocação em cache dos metadados de ficheiros por parte do SMB.
