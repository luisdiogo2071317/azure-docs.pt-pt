---
title: Notas de versão de pré-visualização de Gateway de caixa de dados do Azure | Documentos da Microsoft
description: Descreve problemas em aberto críticos e resoluções para o Gateway de caixa de dados do Azure a executar a versão de pré-visualização.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 01/29/2019
ms.author: alkohli
ms.openlocfilehash: 34e7a578fb04b617221376f5d1065997b12d2613
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55302126"
---
# <a name="azure-data-box-gateway-preview-release-notes"></a>Notas de versão de pré-visualização de Gateway de caixa de dados do Azure

## <a name="overview"></a>Descrição geral

As seguintes notas de versão identificam os problemas em aberto críticos e os problemas resolvidos para versão de pré-visualização do Microsoft Azure dados caixa Gateway.

As notas de versão são atualizadas continuamente e, à medida que são descobertos problemas críticos que requerem uma solução, eles são adicionados. Antes de implementar o Gateway de caixa de dados, reveja com atenção as informações contidas nas notas de versão.

Versão de pré-visualização corresponde à versão software **2.0 de versão de pré-visualização do Data caixa Gateway**.

## <a name="issues-fixed-in-preview-release"></a>Problemas corrigidos na versão de pré-visualização

A tabela seguinte fornece um resumo dos problemas corrigidos nesta versão.

| Não. | Problema |
| --- | --- |
| **1.** | Nesta versão, quando um ficheiro que foi carregado por outra ferramenta (AzCopy) é atualizado e, em seguida, atualizado de forma que aumenta/expande o tamanho do ficheiro, em seguida, o seguinte erro é observado: *Erro 400: InvalidBlobOrBlock (o conteúdo de blob ou de bloqueios especificado é inválido.)*|
| **2.** |Devido a um erro nesta versão, poderá ver instâncias do código de erro 110 na *error.xml* com nomes irreconhecível item. | 
| **3.** |Devido a um erro nesta versão, poderá ver instâncias do código de erro 2003 durante o carregamento de ficheiros específicos. | 
| **4.** |Nesta versão, pode atualizar partilha de apenas um por vez. | 


## <a name="known-issues-in-preview-release"></a>Problemas conhecidos da versão de pré-visualização

A tabela seguinte fornece um resumo dos problemas conhecidos para o Gateway de caixa de dados a executar a versão de pré-visualização.

| Não. | Funcionalidade | Problema | Solução ou enviar comentários |
| --- | --- | --- | --- |
| **1.** |Atualizações |Os dispositivos de Gateway de caixa de dados criados na pré-visualização do anterior versões não podem ser atualizadas para esta versão. |Transferir as imagens de disco virtual a partir da nova versão e configure e implemente novos dispositivos. Para obter mais informações, aceda a [preparar a implementação de Gateway de caixa de dados do Azure](data-box-gateway-deploy-prep.md). |
| **2.** |Disco de dados aprovisionados |Uma vez aprovisionou um disco de dados de um determinado tamanho especificado e criado o Gateway de caixa de dados correspondente, tem não diminuir o disco de dados. A tentar reduzir os resultados de disco numa perda de todos os dados locais no dispositivo. | |
| **3.** |Mudar o Nome |Não é suportada a mudança de nome de objetos. |Se esta funcionalidade é crucial para seu fluxo de trabalho, contacte o Support da Microsoft. |
| **4.** |Copiar| Se um ficheiro só de leitura é copiado para o dispositivo, a propriedade só de leitura não é preservada. | |
| **5.** |Tipos de ficheiro | Os seguintes tipos de ficheiro do Linux não são suportados: arquivos, os ficheiros de bloco, soquetes, pipes, links simbólicos de caracteres.  |Copiar esses arquivos partilham resultados nos ficheiros de comprimento 0, que são criados no NFS. Estes ficheiros permanecem no estado de erro e também são apresentados na *error.xml*. |
| **6.** |Eliminação | Devido a um erro nesta versão, se eliminar uma partilha NFS, em seguida, a partilha não pode ser eliminada. Apresenta o estado de partilha *a eliminação*.  |Isto ocorre apenas quando a partilha está a utilizar um nome de ficheiro não suportado. |
| **7.** |Atualizar | Permissões e listas de controle de acesso (ACLs) não são mantidas numa operação de atualização.  | |
| **8.** |Copiar | Cópia de dados pode falhar com o erro:  Não foi possível concluir a operação pedida devido a uma limitação do sistema de ficheiros.  |Este erro ocorre quando o alternativo dados Stream (ADS) associada ao ficheiro exceder 128 KB (limite máximo para ReFS).  |
| **9.** |Links simbólicos |Links simbólicos não são suportados.  |Links simbólicos aos diretórios resultam em diretórios nunca introdução marcada como offline. Como resultado, não poderá ver em cinzento vários nos diretórios que indica que os diretórios estão offline e todo o conteúdo associado foi completamente carregado para o Azure. |
| **10.** |Ajuda online |As ligações de ajuda no portal do Azure não podem ligar à documentação.|As ligações de ajuda irão funcionar na versão de disponibilidade geral. |



## <a name="next-steps"></a>Passos Seguintes

- [Preparar a implementação de Gateway de caixa de dados do Azure](data-box-gateway-deploy-prep.md).


