---
title: O que é dedicado HSM? -Azure dedicado por HSM | Documentos da Microsoft
description: Descrição geral de HSM dedicados do Azure fornece recursos de armazenamento de chaves no Azure que cumpra FIPS 140-2 nível 3 certificação
services: dedicated-hsm
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.custom: mvc, seodec18
ms.date: 12/07/2018
ms.author: barclayn
ms.openlocfilehash: 1eeafa33c8c1cdbcd7d0e55e3860dda1b8d451fe
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53080083"
---
# <a name="what-is-azure-dedicated-hsm"></a>O que é o Azure Dedicated HSM?

HSM dedicada do Azure é um serviço do Azure que fornece armazenamento de chaves criptográfico no Azure. HSM dedicados cumpre os requisitos de segurança mais exigentes. É a solução ideal para clientes que precisam de FIPS 140-2 nível 3 validado dispositivos e controle completo e exclusivo do dispositivo de HSM. Os dispositivos HSM são implementados globalmente em várias regiões do Azure e podem ser facilmente aprovisionados como um par de dispositivos e configurados para elevada disponibilidade. HSMs podem também ser aprovisionados em várias regiões, para garantir em relação a ativação pós-falha ao nível regional. Tem a Microsoft lançou o serviço HSM dedicado com o [SafeNet Luna rede HSM 7 (modelo A790)](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/) appliance da Gemalto. Este dispositivo oferece os mais altos níveis de desempenho e opções de integração de criptografia. Quando aprovisionado, os HSMs são ligados diretamente à rede virtual de um cliente e também poderiam ser acessados por ferramentas de gerenciamento e de aplicativos no local ao configurar a conectividade VPN ponto a site ou site a site. Os clientes vão adquirir software e documentação para configurar e gerir dispositivos HSM a partir do portal de suporte da Gemalto.

## <a name="why-use-azure-dedicated-hsm"></a>Porquê utilizar o HSM dedicados do Azure?

### <a name="fips-140-2-level-3-compliance"></a>O FIPS 140-2 de nível 3 de conformidade

Muitas organizações têm regulamentos da indústria rigorosas que ditam o armazenamento de chaves criptográficas cumpre [FIPS 140-2 nível 3](https://csrc.nist.gov/publications/detail/fips/140/2/final) requisitos. Um serviço multi-inquilino do Azure Key Vault da Microsoft apenas fornece atualmente FIPS 140-2 nível 2 certificação. Azure HSM dedicados atende uma necessidade real de setor de serviços financeiros, agências governamentais e outras pessoas que têm de cumprir FIPS 140-2 nível 3 requisitos.

### <a name="single-tenant-devices"></a>Dispositivos de inquilino único

Muitos dos nossos clientes têm um requisito para inquilinos de único dispositivo de armazenamento de criptografia. O serviço HSM de dedicada do Azure permite que o aprovisionamento de um dispositivo físico a partir de um dos centros de dados distribuídos globalmente da Microsoft. Assim que aprovisionado para um cliente, apenas que o cliente será capaz de aceder ao dispositivo.

### <a name="full-administrative-control"></a>Controle administrativo completo

Bem como dispositivos de inquilino único, muitos clientes requerem controle administrativo completo e único acesso para efeitos administrativos. Depois de aprovisionada, apenas que o cliente tem qualquer acesso de nível administrativo ou aplicação no dispositivo. Microsoft não terá nenhum controle administrativo após acesso primeiro do cliente, o que requer a alteração de alterar a palavra-passe. A partir desse ponto, o cliente tem um inquilino único verdadeiro com o controle administrativo completo e a capacidade de gestão de aplicações. A Microsoft manter um acesso de nível de monitor (não uma função de administrador) para a telemetria através de ligação de porta serial que abrange os monitores de hardware, tais como temperatura, o estado de funcionamento de fonte de alimentação de energia e o estado de funcionamento do ventoinha. O cliente está livre para Desative esta opção se for necessário, mas seria, em seguida, não receber alertas de estado de funcionamento proativa da Microsoft.

### <a name="high-performance"></a>Elevado Desempenho

O dispositivo da Gemalto foi selecionado para este serviço devido à sua vasta gama de suporte de algoritmos criptográficos, a variedade de sistemas operativos suportados e um amplo suporte de API. O modelo específico implementado oferece um desempenho excelente com 10.000 operações por segundo para RSA 2048. Ele oferece suporte a 10 partições que podem ser utilizadas para instâncias de aplicação único. Esta é uma latência baixa, alta capacidade e dispositivos de alto débito.

### <a name="unique-cloud-based-offering"></a>Oferta com base na Cloud exclusiva

Microsoft reconheceu uma necessidade específica entre um único conjunto de clientes e é o único fornecedor de cloud que oferece novos clientes um serviço HSM dedicado, que tem certificação FIPS 140-2 nível 3 validadas e oferece desse tipo de certa forma de baseado na nuvem e aplicação no local integração.

## <a name="is-azure-dedicated-hsm-right-for-you"></a>É o HSM do Azure dedicado adequado para si?

HSM dedicada do Azure é um serviço especializado atender a requisitos exclusivos entre um tipo específico de organização de grande escala. Como resultado, é esperado que a maior parte dos clientes do Azure não se ajustará o perfil de utilização para este serviço. Muitos irão encontrar o serviço Azure Key Vault para ser mais adequado e mais económico. Para ajudar a decidir o que se ajusta às suas necessidades identificámos os seguintes critérios.

### <a name="best-fit"></a>Que melhor se ajusta

Mais adequado a cenários de "lift-and-shift", que requerem acesso direto e único aos dispositivos HSM. Os exemplos incluem:

- Migração de aplicações no local às máquinas de virtuais do Azure
- Migrando aplicativos do Amazon AWS EC2 para máquinas virtuais do Azure que utilizam o serviço clássico de HSM da Cloud de AWS (Amazon não oferece este serviço para novos clientes)
- Software compactado em execução em máquinas virtuais do Azure, como Apache/Ngnix descarga de SSL, o Oracle TDE e o ADCS

### <a name="not-a-fit"></a>Não é uma opção

Serviços cloud da Microsoft que suportam encriptação com chaves geridas pelo cliente (por exemplo, o Azure Information Protection, Azure Disk Encryption, Azure Data Lake Store, armazenamento do Azure, SQL do Azure, chave de cliente do Office 365) não estão integrados com o HSM do Azure dedicado.

### <a name="it-depends"></a>Depende

Uma combinação de complexa potencial de requisitos e comprometimentos o que podem ou não não possível efetuar dependem muitos cenários. Um exemplo é o requisito de nível 3 FIPS 140-2, que, muitas vezes, é exigido e, pelo que, atualmente o HSM dedicados é as únicas opções.  Se estes requisitos estabelecidos não são relevantes, em seguida, muitas vezes, seria decisão entre o Azure Key Vault e o HSM dedicados com base na avaliação de uma combinação de requisitos. Os exemplos incluem:

- Novo código sendo executado na máquina de Virtual do Azure do cliente
- Servidor SQL TDE numa máquina Virtual do Azure
- Encriptação do lado do cliente de armazenamento do Azure
- SQL Server e BD SQL do Azure Always encriptada

## <a name="next-steps"></a>Próximos Passos

Tendo em consideração a natureza altamente especializada deste serviço, é recomendado que alguns dos conceitos chave encontrados neste conjunto de documentação são totalmente compreendidas, preços, suporte e contratos de nível de serviço são totalmente compreendidos e, em seguida, é um tutorial disponíveis para facilitar o aprovisionamento de HSMs num ambiente de rede virtual existente. [Guias de integração da Gemalto](https://safenet.gemalto.com/partners/microsoft/) e guias de procedimentos para decidir a arquitetura de implantação também são um ótimo recurso.

* [Elevada disponibilidade](high-availability.md)
* [Segurança física](physical-security.md)
* [Redes](networking.md)
* [Capacidade de suporte](supportability.md)
* [Monitorização](monitoring.md)
