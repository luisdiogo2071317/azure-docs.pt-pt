---
title: incluir ficheiro
description: incluir ficheiro
services: storage
author: rogara
ms.service: storage
ms.topic: include
ms.date: 04/09/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: f64645db782b055e1c544f257149411f29fc99d7
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "34806320"
---
## <a name="about-vhds"></a>Sobre VHDs
Os VHDs utilizados no Azure são ficheiros .vhd armazenados como blobs de páginas numa conta de armazenamento standard ou premium no Azure. Para obter detalhes sobre blobs de páginas, consulte [Understanding block blobs and page blobs (Noções sobre blobs de blocos e blobs de páginas)](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs/). Para obter detalhes sobre o armazenamento premium, consulte [High-performance premium storage and Azure VMs (VMs do Azure e armazenamento premium de elevado desempenho)](../articles/virtual-machines/windows/premium-storage.md).

O Azure suporta o formato VHD de disco fixo. O formato fixo dispõe do disco lógico de forma linear no ficheiro, para que o deslocamento do disco X seja armazenado no deslocamento do blob X. Um rodapé pequeno no final do blob descreve as propriedades do VHD. Muitas vezes, os wastes-format espaço porque a maioria dos discos tem grandes intervalos não utilizados nos mesmos. No entanto, o Azure armazena ficheiros .vhd num formato disperso, para receber os benefícios dos discos fixos e dinâmicos ao mesmo tempo. Para obter mais informações, consulte [introdução com discos rígidos virtuais](https://technet.microsoft.com/library/dd979539.aspx).

Todos os ficheiros VHD no Azure que pretende utilizar como uma origem para criar discos ou nas imagens são só de leitura, exceto os ficheiros. vhd carregado ou copiadas para o armazenamento do Azure pelo utilizador (o que pode ser de leitura / escrita ou só de leitura). Quando cria um disco ou a imagem, o Azure faz cópias da origem de ficheiros. vhd. Estas cópias podem ser só de leitura ou de leitura e escrita, dependendo de como utiliza o VHD.

Quando cria uma máquina virtual a partir de uma imagem, o Azure cria um disco para a máquina virtual que é uma cópia do ficheiro .vhd de origem. Para proteger contra a eliminação acidental, o Azure coloca uma concessão em qualquer ficheiro .vhd de origem que é utilizado para criar uma imagem, um disco do sistema operativo ou um disco de dados.

Antes de poder eliminar um ficheiro .vhd de origem, terá de remover a concessão, ao eliminar o disco ou a imagem. Para eliminar um ficheiro .vhd que esteja a ser utilizado por uma máquina virtual como um disco do sistema operativo, pode eliminar a máquina virtual, o disco do sistema operativo e o ficheiro .vhd de origem em simultâneo, ao eliminar a máquina virtual e todos os discos associados. No entanto, a eliminação de um ficheiro .vhd que é uma origem para um disco exige vários passos numa determinada ordem. Primeiro desanexe o disco da máquina virtual e, em seguida, elimine o disco e depois o ficheiro .vhd.

> [!WARNING]
> Se eliminar um ficheiro .vhd de origem do armazenamento ou eliminar a sua conta de armazenamento, a Microsoft não pode recuperar os dados por si.

## <a name="types-of-disks"></a>Tipos de discos 
Os Discos do Azure foram concebidos para garantir uma disponibilidade de 99,999%. Discos do Azure tem consistentemente entregar durabilidade de nível empresarial, com um líder da indústria ZERO % Annualized taxa de falhas.

Existem três camadas de desempenho para o armazenamento que pode escolher quando criar os seus discos – discos de SSD Premium, SSD Standard (pré-visualização) e armazenamento HDD padrão. Além disso, existem dois tipos de discos - não gerido e geridos.

### <a name="standard-hdd-disks"></a>Discos HDD padrão
Discos HDD padrão são apoiados por HDDs e fornecem armazenamento económico. Armazenamento HDD padrão pode ser replicado localmente no Centro de dados de um ou ser georredundante com centros de dados primária e secundária. Para obter mais informações sobre a replicação de armazenamento, consulte [replicação de armazenamento do Azure](../articles/storage/common/storage-redundancy.md). 

Para obter mais informações sobre a utilização de discos padrão HDD, consulte [armazenamento padrão e os discos](../articles/virtual-machines/windows/standard-storage.md).

### <a name="standard-ssd-disks-preview"></a>Discos SSD padrão (pré-visualização)
Discos SSD padrão foram concebidos para resolver o mesmo tipo de cargas de trabalho como discos de HDD padrão, mas oferecem mais consistente desempenho e fiabilidade de HDD. Discos SSD padrão combinam os elementos de discos de Premium SSD e discos de HDD padrão para formar uma solução económica melhor adequados para aplicações, como servidores web que não é necessário o IOPS elevado em discos. Sempre que disponível, os discos de SSD padrão estão a opção de implementação recomendado para a maioria das cargas de trabalho. Discos SSD padrão só estão disponíveis como discos geridos e enquanto na pré-visualização só estão disponíveis nos [selecione regiões](../articles/virtual-machines/windows/faq-for-disks.md) e com o tipo de resiliência de armazenamento localmente redundante (LRS).

### <a name="premium-ssd-disks"></a>Discos SSD Premium 
Discos SSD Premium são apoiados por SSDs e fornece suporte de disco de elevado desempenho, baixa latência para VMs em execução I/O intensivo cargas de trabalho. Normalmente, pode utilizar discos Premium SSD com tamanhos que incluem um "s" no nome da série. Por exemplo, não há uma série de Dv3 e a série Dsv3, a série Dsv3 podem ser utilizadas com discos de Premium SSD.  Para mais informações, consulte [Premium Storage (Armazenamento Premium)](../articles/virtual-machines/windows/premium-storage.md).

### <a name="unmanaged-disks"></a>Discos não geridos
Os discos não geridos são o tipo de discos tradicionais que foram utilizados por VMs. Com estes discos, criar a sua própria conta de armazenamento e especifique que conta de armazenamento ao criar o disco. Certifique-se de que não incluir demasiados discos na mesma conta de armazenamento, porque foi excedido o [metas de escalabilidade](../articles/storage/common/storage-scalability-targets.md) da conta de armazenamento (20.000 IOPS, por exemplo), resultando em VMs que está a ser limitadas. Com discos não geridos, deve maximizar a utilização de uma ou mais contas de armazenamento para obter o melhor desempenho das suas VMs.

### <a name="managed-disks"></a>Managed disks 
Os Managed Disks suportam a criação/gestão da conta de armazenamento em segundo plano para si, e asseguram que não tem de se preocupar com os limites de escalabilidade da conta de armazenamento. Basta especificar o tamanho do disco e o escalão de desempenho (Standard/Premium), e o Azure cria e gere o disco por si. Conforme adiciona discos ou aumente e reduza verticalmente a VM, não terá de se preocupar se o armazenamento está a ser utilizado. 

Também pode gerir as imagens personalizadas numa conta de armazenamento por região do Azure e utilizá-las para criar centenas de VMs na mesma subscrição. Para mais informações sobre discos geridos, veja [Managed Disks Overview (Descrição geral dos Managed Disks)](../articles/virtual-machines/windows/managed-disks-overview.md).

Recomendamos que utilize os Managed Disks do Azure para novas VMs e que converta os discos não geridos anteriores para discos geridos, para tirar partido das diversas funcionalidades disponíveis nos Managed Disks.

### <a name="disk-comparison"></a>Comparação de discos
A tabela seguinte fornece uma comparação de HDD padrão, Standard SSD e Premium SSD para discos não geridos e geridos para o ajudar a decidir o que utilizar.

|    | Disco Premium do Azure |Disco de SSD Standard do Azure (pré-visualização)| Disco HDD padrão do Azure 
|--- | ------------------ | ------------------------------- | ----------------------- 
| Tipo de Disco | Unidades de Estado Sólido (SSD) | Unidades de Estado Sólido (SSD) | Unidades de Disco Rígido (HDD)  
| Descrição geral  | Suporte de disco de baixa latência, alto desempenho baseado em SSD para VMs com cargas de trabalho intensivas em E/S ou ambiente de produção crítico de missão de alojamento |Desempenho e fiabilidade de HDD mais consistente. Otimizado para cargas de trabalho de IOPS baixa| Com base em HDD disco económico para acesso influxo
| Cenário  | Cargas de trabalho confidenciais de produção e de desempenho |Servidores Web, as aplicações empresariais ligeiramente utilizado e Dev/teste| Cópia de segurança, não crítico, acesso influxo 
| Tamanho do Disco | P4: 32 GiB (apenas discos geridos)<br>P6: 64 GiB (apenas discos geridos)<br>P10: GiB 128<br>P15: 256 GiB (apenas discos geridos)<br>P20: GiB 512<br>P30: 1024 GiB<br>P40: 2048 GiB<br>P50: 4095 GiB |Discos geridos pelo apenas:<br>E10: GiB 128<br>E15: GiB 256<br>E20: GiB 512<br>E30: 1024 GiB<br>E40: 2048 GiB<br>E50: 4095 GiB | Os discos não geridos: 1 GiB – 4 TiB (4095 GiB) <br><br>Managed Disks:<br> S4: GiB 32 <br>S6: GiB 64 <br>S10: GiB 128 <br>S15: GiB 256 <br>S20: GiB 512 <br>S30: 1024 GiB <br>S40: 2048 GiB<br>S50: 4095 GiB
| Débito Máx por Disco | 250 MiB/s | Até 60 MiB/s | Até 60 MiB/s 
| IOPs Máx por disco | 7500 IOPS | Até 500 IOPS | Até 500 IOPS 