---
title: Monitorização de SAP HANA no Azure (instâncias grandes) | Documentos da Microsoft
description: Monitor de SAP HANA no Azure (instâncias grandes).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e5a5d462be5555090d1dfced5fa07c9b748eb312
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44345663"
---
# <a name="how-to-monitor-sap-hana-large-instances-on-azure"></a>Como monitorizar o SAP HANA (instâncias grandes) no Azure

SAP HANA no Azure (instâncias grandes) não é diferente de qualquer outra implementação do IaaS — tem de monitorizar o que o sistema operacional e a aplicação está a fazer e como os aplicativos consomem os seguintes recursos:

- CPU
- Memória
- Largura de banda de rede
- Espaço em disco

Com máquinas de virtuais do Azure, terá de saber se as classes de recursos com o nome acima são suficientes, ou eles obterem esgotados. Eis mais detalhes sobre cada uma das classes diferentes:

**Consumo de recursos de CPU:** a proporção que SAP definido para determinada carga de trabalho no HANA é imposta para se certificar de que deve ser suficiente recursos de CPU disponíveis para trabalhar com os dados que são armazenados na memória. No entanto, poderá haver casos em que o HANA consome muitos CPUs, execução de consultas devido à falta de índices ou problemas semelhantes. Isso significa que deve monitorar o consumo de recursos de CPU da unidade de instância grande do HANA, bem como recursos de CPU consumidos pelos serviços específicos do HANA.

**Consumo de memória:** é importante monitorizar de dentro do HANA, bem como fora do HANA na unidade. No HANA, monitorize a forma como os dados está a consumir HANA memória alocada para se manterem dentro as diretrizes de dimensionamento necessárias do SAP. Também queira monitorizar o consumo de memória no nível da instância grande para se certificar de que software adicional instalado não-HANA não consumir demasiada memória e, portanto competir com o HANA para a memória.

**Largura de banda de rede:** gateway de VNet do Azure a é limitado em largura de banda de dados movendo para a VNet do Azure, pelo que é útil monitorizar os dados recebidos por todas as VMs do Azure dentro de uma VNet para saber como fechar é os limites do SKU de gateway do Azure sel ected. Na unidade instância grande do HANA, faz sentido para monitorizar o tráfego recebido e enviado rede também e para controlar os volumes que são processados ao longo do tempo.

**Espaço em disco:** normalmente aumenta o consumo de espaço em disco ao longo do tempo. Causas mais comuns são: aumenta o volume de dados, execução de backups de log de transações, armazenamento de ficheiros de rastreio e a execução de instantâneos de armazenamento. Portanto, é importante monitorizar a utilização do espaço em disco e gerir o espaço em disco associado com a unidade de instância grande do HANA.

Para o **SKUs do tipo II** das instâncias grandes do HANA, o servidor é fornecido com as ferramentas de diagnóstico do sistema pré-carregado. Pode utilizar estas ferramentas de diagnóstico para efetuar a verificação de estado de funcionamento do sistema. Execute o seguinte comando para gerar o ficheiro de registo de verificação de estado de funcionamento em /var/log/health_check.
```
/opt/sgi/health_check/microsoft_tdi.sh
```
Quando trabalha com a equipe de Support da Microsoft para solucionar um problema, também pode ser solicitado a fornecer os ficheiros de registo ao utilizar estas ferramentas de diagnóstico. Pode zip do arquivo usando o seguinte comando.
```
tar  -czvf health_check_logs.tar.gz /var/log/health_check
```

**Passos seguintes?**

- Consultar [como monitorizar o SAP HANA (instâncias grandes) no Azure](troubleshooting-monitoring.md).