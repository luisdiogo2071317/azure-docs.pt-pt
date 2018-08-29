---
title: Requisitos de sistema de disco do Microsoft Azure Data Box | Documentos da Microsoft
description: Saiba mais sobre o software e os requisitos de rede para o seu disco do Azure Data Box
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/28/2018
ms.author: alkohli
ms.openlocfilehash: 2fdd574adf3587f11984bee2a2549d9bcd0c4c0d
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/28/2018
ms.locfileid: "43126010"
---
# <a name="azure-data-box-disk-system-requirements-preview"></a>Requisitos de sistema de disco de caixa de dados do Azure (pré-visualização)

Este artigo descreve os requisitos de sistema importante para a sua solução de disco do Microsoft Azure Data Box e para os clientes a ligar o disco do Data Box. Recomendamos que reveja as informações cuidadosamente antes de implementar o disco do Data Box e, em seguida, regressar mais tarde a ela conforme necessário durante a implantação e operação subseqüente.

> [!IMPORTANT]
> Disco data Box está em pré-visualização. Reveja os [termos de utilização para a pré-visualização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) antes de implementar esta solução. 

Os requisitos de sistema incluem as plataformas suportadas para clientes que se conectam a discos, as contas de armazenamento suportados e tipos de armazenamento.


## <a name="supported-operating-systems-for-clients"></a>Sistemas operativos suportados por clientes

Eis uma lista de sistemas operativos suportados para desbloquear o disco e a operação de cópia de dados por meio dos clientes ligados para o disco do Data Box.

| **Sistema operativo/plataforma** | **Versões** |
| --- | --- |
| Windows Server |2008 R2 SP1 <br> 2012 <br> 2012 R2 <br> 2016 |
| Windows |7, 8, 10 |
| Windows PowerShell |4.0 |
| .NET Framework |4.5.1 |
| Estrutura de gerenciamento do Windows |4.0|

> [!NOTE] 
> Necessidades do BitLocker seja ativado nos clientes que executam o disco desbloquear ferramenta e são usadas para copiar os dados.


## <a name="supported-storage-accounts"></a>Contas de armazenamento suportadas

Aqui está uma lista dos tipos de armazenamento suportadas para o disco do Data Box.

| **Conta de armazenamento** | **Notas** |
| --- | --- |
| Clássica | Standard |
| Fins Gerais  |Standard; ambos o V1 e V2 são suportadas. São suportadas camadas frequentes e esporádica. |


## <a name="supported-storage-types"></a>Tipos de armazenamento suportadas

Aqui está uma lista dos tipos de armazenamento suportadas para o disco do Data Box.

| **Formato de ficheiro** | **Notas** |
| --- | --- |
| Blob de blocos do Azure | |
| BLOBs de páginas do Azure  | |


## <a name="next-step"></a>Passo seguinte

* [Implementar o seu disco do Azure Data Box](data-box-disk-deploy-ordered.md)

