---
title: Práticas recomendadas do serviço de cópia de segurança de infra-estrutura para o Azure Stack | Documentos da Microsoft
description: Pode seguir o conjunto de melhores práticas quando implementar e gerir o Azure Stack no seu datacenter para o ajudar a minimizar a perda de dados, se houver uma falha catastrófica.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 221FDE40-3EF8-4F54-A075-0C4D66EECE1A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2019
ms.author: jeffgilb
ms.reviewer: hectorl
ms.lastreviewed: 02/08/2019
ms.openlocfilehash: d2568a4dfc4fefe9628fc63dcc0526b0876fde00
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/11/2019
ms.locfileid: "55993882"
---
# <a name="infrastructure-backup-service-best-practices"></a>Práticas recomendadas de serviço de cópia de segurança de infra-estrutura

*Aplica-se a: Integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Pode seguir as melhores práticas quando implementar e gerir o Azure Stack no seu datacenter para o ajudar a minimizar a perda de dados em caso de uma falha catastrófica.

Deve rever as melhores práticas em intervalos regulares para verificar que a instalação ainda está em conformidade quando forem feitas alterações para o fluxo de operação. Deve encontrar algum problema ao implementar estas melhores práticas, contacte o Support da Microsoft para obter ajuda.

## <a name="configuration-best-practices"></a>Melhores práticas de configuração

### <a name="deployment"></a>Implementação

Ative a infraestrutura de cópia de segurança após a implementação de cada nuvem do Azure Stack. Com o Azure Stack PowerShell pode agendar cópias de segurança de qualquer cliente/servidor com acesso ao ponto de final de API de gestão do operador.

### <a name="networking"></a>Redes

A cadeia de caracteres de convenção de Nomenclatura Universal (UNC) para o caminho tem de utilizar um nome de domínio completamente qualificado (FQDN). Endereço IP é possível se a resolução de nomes não é possível. Uma cadeia de caracteres UNC Especifica a localização de recursos, tais como ficheiros partilhados ou dispositivos.

### <a name="encryption"></a>Encriptação

#### <a name="version-1901-and-newer"></a>Versão mais recente e 1901

O certificado de encriptação é utilizado para encriptar os dados de cópia de segurança que são exportados para armazenamento externo. O certificado pode ser um certificado autoassinado, uma vez que o certificado é utilizado apenas para transportar as chaves. Consulte New-SelfSignedCertificate para obter mais informações sobre como criar um certificado.  
A chave deve ser armazenada numa localização segura (por exemplo, certificado global do Azure Key Vault). O formato CER do certificado é utilizado para encriptar os dados. O formato PFX deve ser usado durante a implementação de recuperação na cloud do Azure Stack para desencriptar os dados de cópia de segurança.

![Armazenado o certificado numa localização segura.](media/azure-stack-backup/azure-stack-backup-encryption-store-cert.png)

#### <a name="1811-and-older"></a>1811 e mais antigos

A chave de encriptação é utilizada para encriptar os dados de cópia de segurança que são exportados para armazenamento externo. A chave é gerada como parte da [ativar a cópia de segurança para o Azure Stack com o PowerShell](azure-stack-backup-enable-backup-powershell.md).

A chave deve ser armazenada numa localização segura (por exemplo, segredo global do Azure Key Vault). Esta chave tem de ser utilizada durante a nova implementação do Azure Stack. 

![Armazenada a chave de uma localização segura.](media/azure-stack-backup/azure-stack-backup-encryption2.png)

## <a name="operational-best-practices"></a>Melhores práticas operacionais

### <a name="backups"></a>Cópias de segurança

 - Tarefas de cópia de segurança de execução enquanto o sistema está em execução, portanto, não sem períodos de indisponibilidade para as aplicações de utilizador ou experiências de gestão. Esperar que as tarefas de cópia de segurança demore 20 e 40 minutos para uma solução que está sob carga razoável.
 - Utilizar fornecido instruções de OEM, comutadores de rede manualmente cópias de segurança e o anfitrião de ciclo de vida do hardware (HLH) devem ser armazenados na mesma partilha de cópia de segurança em que o controle de arquivos do controlador de cópia de segurança de infra-estrutura do plano de dados de cópia de segurança. Considere armazenar o comutador e configurações de HLH na pasta de região. Se tiver várias instâncias do Azure Stack na mesma região, considere utilizar um identificador para cada configuração que pertence a uma unidade de escala.

### <a name="folder-names"></a>Nomes de pastas

 - Infraestrutura cria a pasta MASBACKUP automaticamente. Esta é uma partilha gerida pela Microsoft. Pode criar partilhas no mesmo nível como MASBACKUP. Não é recomendada a criação de pastas ou os dados de armazenamento dentro MASBACKUP que não crie Azure Stack. 
 -  Utilizador FQDN e região, em seu nome de pasta para diferenciar os dados de cópia de segurança de nuvens diferentes. O nome de domínio completamente qualificado (FQDN) da sua implementação do Azure Stack e pontos de extremidade é a combinação do parâmetro de região e o parâmetro de nome de domínio externo. Para obter mais informações, consulte [integração no datacenter do Azure Stack - DNS](azure-stack-integrate-dns.md).

Por exemplo, a partilha de cópia de segurança é AzSBackups alojadas em fileserver01.contoso.com. Em que a partilha de ficheiros pode ser uma pasta de implementação do Azure Stack com o nome de domínio externo e uma subpasta que utiliza o nome da região. 

FQDN: contoso.com  
Região: nyc


    \\fileserver01.contoso.com\AzSBackups
    \\fileserver01.contoso.com\AzSBackups\contoso.com
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\MASBackup

Pasta de MASBackup é onde o Azure Stack armazena os respetivos dados de cópia de segurança. Não deve utilizar esta pasta para armazenar os seus dados. OEM não deve utilizar esta pasta para armazenar quaisquer dados de cópia de segurança seja. 

OEMs são encorajados a armazenar os dados de cópia de segurança de seus componentes sob a pasta de região. Comutadores de rede, cada anfitrião de ciclo de vida do hardware (HLH) etc., podem ser armazenados na sua própria subpasta. Por exemplo:

    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\HLH
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\Switches
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\DeploymentData
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\Registration

### <a name="monitoring"></a>Monitorização

O sistema suporta os seguintes alertas:

| Alerta                                                   | Descrição                                                                                     | Remediação                                                                                                                                |
|---------------------------------------------------------|-------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Cópia de segurança falhou porque a partilha de ficheiros é sem capacidade | Partilha de ficheiros é sem capacidade e o controlador de cópia de segurança não é possível exportar ficheiros de cópia de segurança para a localização. | Adicionar mais capacidade de armazenamento e tente novamente a cópia de segurança. Elimine cópias de segurança existentes (a partir dos mais antigos em primeiro lugar) para libertar espaço.                    |
| Cópia de segurança falhou devido a problemas de conectividade.             | Rede entre o Azure Stack e o ficheiro de partilha está a ter problemas.                          | Resolver o problema de rede e tente novamente a cópia de segurança.                                                                                            |
| Cópia de segurança falhou devido a uma falha no caminho                | O caminho de partilha de ficheiros não é possível resolver                                                          | Mapear a partilha de um computador diferente para garantir que a partilha está acessível. Terá de atualizar o caminho, se já não é válido.       |
| Cópia de segurança falhou devido a problema de autenticação               | Pode haver um problema com as credenciais ou um problema de rede que tem impacto sobre a autenticação.    | Mapear a partilha de um computador diferente para garantir que a partilha está acessível. Terá de atualizar as credenciais, se já não são válidas. |
| Cópia de segurança falhou devido a uma falha geral                    | O pedido com falha pode dever um problema intermitente. Tente novamente efetuar cópias de segurança.                    | Contacte o suporte                                                                                                                               |

## <a name="next-steps"></a>Passos Seguintes

Reveja o material de referência para o [serviço de cópia de segurança da infraestrutura](azure-stack-backup-reference.md)

Ativar o [serviço de cópia de segurança de infra-estrutura](azure-stack-backup-enable-backup-console.md)
