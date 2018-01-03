---
title: "Serviço de cópia de segurança de infraestrutura melhores práticas para a pilha do Azure | Microsoft Docs"
description: "Pode seguir o conjunto de melhores práticas quando implementar e gerir a pilha do Azure no seu centro de dados para ajudar a mitigar a perda de dados, se existir uma falha catastrófica."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 221FDE40-3EF8-4F54-A075-0C4D66EECE1A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/27/2017
ms.author: mabrigg
ms.openlocfilehash: b9438f3bab92f40a5c79ce7b7a572195c182be45
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2018
---
# <a name="infrastructure-backup-service-best-practices"></a>Serviço de cópia de segurança de infraestrutura melhores práticas

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pode seguir as melhores práticas quando implementar e gerir a pilha do Azure no seu centro de dados para ajudar a mitigar a perda de dados na eventualidade de ocorrer uma falha catastrófica.

Deve rever as melhores práticas em intervalos regulares para verificar que a instalação ainda está em conformidade quando são efetuadas alterações para o fluxo de operação. Deve encontrar problemas ao implementar estas melhores práticas, contacte Support da Microsoft para obter ajuda.

## <a name="configuration-best-practices"></a>Melhores práticas de configuração

### <a name="deployment"></a>Implementação

Ative a infraestrutura de cópia de segurança após a implementação de cada nuvem de pilha do Azure. Pode agendar cópias de segurança de qualquer cliente/servidor com acesso ao ponto de final de API de gestão do operador de ferramentas de AzureStack a utilizar.

### <a name="networking"></a>Redes

A cadeia de convenção de Nomenclatura Universal (UNC) para o caminho tem de utilizar um nome de domínio completamente qualificado (FQDN). Endereço IP é possível se a resolução de nomes não é possível. Uma cadeia em UNC Especifica a localização dos recursos, tais como ficheiros partilhados ou dispositivos.

### <a name="encryption"></a>Encriptação

A chave de encriptação é utilizada para encriptar os dados de cópia de segurança que obtém exportados para armazenamento externo. A chave pode ser gerada com ferramentas AzureStack. 

![Ferramentas de AzureStack](media\azure-stack-backup\azure-stack-backup-encryption1.png)

A chave deve ser armazenada numa localização segura (por exemplo, segredo do Cofre de chaves do Azure público). Esta chave deve ser utilizada durante a reimplementação da pilha do Azure. 

![Armazenar a chave de uma localização segura.](media\azure-stack-backup\azure-stack-backup-encryption2.png)

## <a name="operational-best-practices"></a>Melhores práticas operacionais

### <a name="backups"></a>Cópias de segurança

 - Controlador de cópia de segurança da infraestrutura tem de ser acionado a pedido. Recomenda-se a cópia de segurança, pelo menos, duas vezes por dia.
 - Executar tarefas de cópia de segurança enquanto o sistema está em execução pelo que não existe nenhum período de indisponibilidade para as aplicações de utilizador ou experiências de gestão. Esperar que as tarefas de cópia de segurança 40 de 20 minutos para uma solução que se encontra sob carga razoável.
 - Utilizar OEM fornecido a instrução, comutadores de rede manualmente cópia de segurança e o anfitrião de ciclo de vida de hardware (HLH) devem ser armazenados na partilha de cópia de segurança mesma onde o controlo de arquivos de controlador de cópia de segurança de infraestrutura plane dados de cópia de segurança. Considere armazenar o comutador e configurações de HLH na pasta de região. Se tiver várias instâncias de pilha do Azure na mesma região, considere utilizar um identificador para cada configuração que pertence a uma unidade de escala.

### <a name="folder-names"></a>Nomes de pastas

 - Infraestrutura cria a pasta MASBACKUP automaticamente. Esta é uma partilha gerida pela Microsoft. Pode criar partilhas no mesmo nível como MASBACKUP. Não é recomendado criar pastas ou dados de armazenamento dentro MASBACKUP não criar a pilha do Azure. 
 -  O utilizador FQDN e região no nome da pasta para diferenciar os dados de cópia de segurança de nuvens diferentes. O nome de domínio completamente qualificado (FQDN) da sua implementação de pilha do Azure e os pontos finais é a combinação do parâmetro de região e o parâmetro de nome de domínio externo. Para obter mais informações, consulte [integração de centro de dados de pilha do Azure - DNS](azure-stack-integrate-dns.md).

Por exemplo, a partilha de cópia de segurança é AzSBackups alojados no fileserver01.contoso.com. Em que a partilha de ficheiros poderá ser uma pasta de implementação de pilha do Azure utilizando o nome de domínio externo e uma subpasta que utiliza o nome de região. 

FQDN: contoso.com  
Região: nyc


    \\fileserver01.contoso.com\AzSBackups
    \\fileserver01.contoso.com\AzSBackups\contoso.com
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\MASBackup

Pasta de MASBackup é onde pilha do Azure armazena os respetivos dados de cópia de segurança. Não deve utilizar esta pasta para armazenar os seus próprios dados. OEM não deve utilizar esta pasta para armazenar dados de cópia de segurança está. 

OEMs são encouraged para armazenar dados de cópia de segurança para os respetivos componentes sob a pasta de região. Cada comutadores de rede, o anfitrião de ciclo de vida de hardware (HLH) e assim sucessivamente podem ser armazenados na sua própria subpasta. Por exemplo:

    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\HLH
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\Switches
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\DeploymentData
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\Registration

### <a name="monitoring"></a>Monitorização

Os seguintes alertas são suportados pelo sistema:

| Alerta                                                   | Descrição                                                                                     | Remediação                                                                                                                                |
|---------------------------------------------------------|-------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Cópia de segurança falhou porque a partilha de ficheiros está fora do capacidade | Partilha de ficheiros é sem capacidade e o controlador de cópia de segurança não é possível exportar ficheiros de cópia de segurança para a localização. | Adicione mais capacidade de armazenamento e tente novamente a cópia de segurança. Elimine as cópias de segurança existentes (a partir de mais antigos pela primeira vez) para libertar espaço.                    |
| Cópia de segurança falhou devido a problemas de conectividade.             | Rede entre a pilha do Azure e o ficheiro de partilha está com problemas.                          | Resolver o problema de rede e tente novamente a cópia de segurança.                                                                                            |
| Cópia de segurança falhou devido a uma falha no caminho                | O caminho de partilha de ficheiros não pode ser resolvido                                                          | Mapear a partilha a partir de outro computador para garantir que a partilha está acessível. Terá de atualizar o caminho se já não é válido.       |
| Cópia de segurança falhou devido ao problema de autenticação               | Poderá existir um problema com as credenciais ou um problema de rede que afeta a autenticação.    | Mapear a partilha a partir de outro computador para garantir que a partilha está acessível. Terá de atualizar as credenciais, se já não são válidos. |
| Cópia de segurança falhou devido a uma falha geral                    | Os pedidos falhados pode ser devido a um problema intermitente. Volte a tentar fazer cópia de segurança.                    | Contacte o suporte                                                                                                                               |

## <a name="next-steps"></a>Passos Seguintes

 - Reveja o material de referência para o [serviço de cópia de segurança da infraestrutura](azure-stack-backup-reference.md).  
 - Ativar o [serviço cópia de segurança da infraestrutura](azure-stack-backup-enable-backup-console.md).