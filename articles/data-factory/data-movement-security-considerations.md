---
title: "Considerações de segurança no Azure Data Factory | Microsoft Docs"
description: "Descreve a infraestrutura de básicos de segurança que utilizam serviços de movimento de dados no Azure Data Factory para ajudar a proteger os dados."
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: abnarain
ms.openlocfilehash: 3c8215ab4a1759efef3c2c13a5ac44f6944b53d7
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
#  <a name="security-considerations-for-data-movement-in-azure-data-factory"></a>Considerações de segurança para o movimento de dados no Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versão 1 - GA](v1/data-factory-data-movement-security-considerations.md)
> * [Versão 2 - Pré-visualização](data-movement-security-considerations.md)

Este artigo descreve a infraestrutura de básicos de segurança que utilizam serviços de movimento de dados no Azure Data Factory para ajudar a proteger os dados. Recursos de gestão de fábrica de dados são criados numa infraestrutura de segurança do Azure e utilizam todas as medidas de segurança possível oferecidas pelo Azure.

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço do Data Factory, o que é geralmente disponível (DG), consulte o artigo [considerações de segurança de movimento de dados do Data Factory versão 1](v1/data-factory-data-movement-security-considerations.md).

Numa solução do Data Factory, pode criar um ou mais [pipelines](concepts-pipelines-activities.md) de dados. Os pipelines são agrupamentos lógicos de atividades que, em conjunto, realizam uma tarefa. Estes pipelines de residir na região onde foi criada a fábrica de dados. 

Apesar de fábrica de dados só está disponível nos EUA leste, EUA Leste 2 e regiões da Europa Ocidental (pré-visualização versão 2), o serviço de movimento de dados está disponível [global em várias regiões](concepts-integration-runtime.md#azure-ir). Se o serviço de movimento de dados ainda não está implementado nessa região, o serviço fábrica de dados garante que dados deixe uma área geográfica ou região, a menos que o serviço para utilizar uma região alternativa instruir explicitamente. 

O Azure Data Factory não armazena quaisquer dados, exceto as credenciais de serviço ligado para os arquivos de dados de nuvem, que são encriptados utilizando certificados. Com o Data Factory, criar condicionada por dados fluxos de trabalho para orquestrar o movimento de dados entre [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats)e o processamento de dados utilizando [serviços de computação](compute-linked-services.md) noutras regiões ou num ambiente no local. Também pode monitorizar e gerir fluxos de trabalho utilizando SDKs e Monitor do Azure.

Movimento de dados utilizando o Data Factory tem foram certificado para:
-   [HIPAA/HITECH](https://www.microsoft.com/en-us/trustcenter/Compliance/HIPAA) 
-   [ISO/IEC 27001](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27001)  
-   [ISO/IEC 27018](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27018)
-   [CSA STAR](https://www.microsoft.com/en-us/trustcenter/Compliance/CSA-STAR-Certification)

Se estiver interessado na forma como o Azure protege a própria infraestrutura e de conformidade do Azure, visite o [Microsoft Trust Center](https://www.microsoft.com/trustcenter).

Neste artigo, vamos rever as considerações de segurança nos seguintes cenários de movimento de dados de duas: 

- **Cenário de nuvem**: neste cenário, a origem e o destino são acessíveis publicamente através da internet. Estes incluem os serviços de armazenamento na nuvem geridos, tais como o Storage do Azure, Azure SQL Data Warehouse, SQL Database do Azure, Azure Data Lake Store, Amazon S3, Amazon Redshift, serviços SaaS, como o Salesforce e protocolos de web, como o FTP e OData. Encontrar uma lista completa das origens de dados suportados no [arquivos de dados e formatos suportados](copy-activity-overview.md#supported-data-stores-and-formats).
- **Cenário híbrido**: neste cenário, a origem ou o destino é protegido por uma firewall ou dentro de uma rede empresarial no local. Em alternativa, o arquivo de dados está no privado rede ou de rede virtual (frequentemente de origem) e não está acessível publicamente. Servidores de base de dados alojadas em máquinas virtuais também abrangidos por este cenário.

## <a name="cloud-scenarios"></a>Cenários de nuvem
### <a name="secure-data-store-credentials"></a>Credenciais do arquivo de dados seguros
- **Armazenar credenciais encriptado num arquivo Azure Data Factory gerido**. Fábrica de dados ajuda a proteger as credenciais do arquivo de dados ao encriptá-los com certificados geridos pela Microsoft. Estes certificados rodam em dois anos (que inclui a migração de credenciais e renovação de certificado). As credenciais encriptadas estão armazenadas em segurança numa conta de armazenamento do Azure gerida pelos serviços de gestão do Azure Data Factory. Para obter mais informações sobre a segurança de armazenamento do Azure, consulte [descrição geral de segurança de armazenamento do Azure](../security/security-storage-overview.md).

- **Armazenar credenciais no Cofre de chaves do Azure**. Pode também armazenar credenciais do arquivo de dados no [Cofre de chaves do Azure](https://azure.microsoft.com/services/key-vault/). Fábrica de dados obtém a credencial durante a execução de uma atividade. Para obter mais informações, consulte [credencial de arquivo no Cofre de chaves do Azure](store-credentials-in-key-vault.md).

### <a name="data-encryption-in-transit"></a>Encriptação de dados em trânsito
Se o arquivo de dados na nuvem suporta HTTPS ou TLS, todos os dados transferidos entre os serviços de movimento de dados no Data Factory e um arquivo de dados de nuvem são através de canal seguro, HTTPS ou TLS.

> [!NOTE]
> Todas as ligações ao SQL Database do Azure e Azure SQL Data Warehouse exigem encriptação (SSL/TLS), enquanto os dados se encontram em trânsito para e da base de dados. Quando estiver a criar um pipeline utilizando JSON, adicione a propriedade de encriptação e defina-o como **verdadeiro** na cadeia de ligação. Para o Storage do Azure, pode utilizar **HTTPS** na cadeia de ligação.

### <a name="data-encryption-at-rest"></a>Encriptação de dados inativos
Alguns dados armazena suporte a encriptação de dados inativos. Recomendamos que ative o mecanismo de encriptação de dados para os arquivos de dados. 

#### <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse
Encriptação de dados transparente (TDE) no Azure SQL Data Warehouse ajuda a proteger contra a ameaça de atividade maliciosa efetuando em tempo real encriptação e desencriptação de dados inativos. Este comportamento é transparente para o cliente. Para obter mais informações, consulte [proteger uma base de dados no SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md).

#### <a name="azure-sql-database"></a>Base de Dados SQL do Azure
Base de dados SQL do Azure também suporta a encriptação de dados transparente (TDE), que ajuda a proteger contra a ameaça de atividade maliciosa efetuando em tempo real encriptação e desencriptação de dados, sem necessidade de alterações à aplicação. Este comportamento é transparente para o cliente. Para obter mais informações, consulte [encriptação transparente de dados para a base de dados SQL e do armazém de dados](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

#### <a name="azure-data-lake-store"></a>Azure Data Lake Store
O Azure Data Lake Store também proporciona a encriptação dos dados armazenados na conta. Quando ativada, o Data Lake Store é automaticamente encripta os dados antes de a persistência e desencripta antes da obtenção, tornando-a transparente para o cliente acede os dados. Para obter mais informações, consulte [segurança no Azure Data Lake Store](../data-lake-store/data-lake-store-security-overview.md). 

#### <a name="azure-blob-storage-and-azure-table-storage"></a>Armazenamento de Blobs do Azure e o Table storage do Azure
Armazenamento de Blobs do Azure e o Table storage do Azure suportam armazenamento serviço encriptação (SSE), que encripta os dados antes de a persistência para armazenamento e desencripta antes da obtenção automaticamente. Para obter mais informações, consulte [encriptação do serviço de armazenamento do Azure para dados Inativos](../storage/common/storage-service-encryption.md).

#### <a name="amazon-s3"></a>Amazon S3
Amazon S3 suporta o cliente e o servidor de encriptação de dados inativos. Para obter mais informações, consulte [proteger utilizando a encriptação de dados](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingEncryption.html).

#### <a name="amazon-redshift"></a>Amazon Redshift
Amazon Redshift suporta encriptação de cluster para dados inativos. Para obter mais informações, consulte [encriptação de base de dados do Amazon Redshift](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-db-encryption.html). 

#### <a name="salesforce"></a>Salesforce
Salesforce suporta a encriptação de plataforma de proteção que permite que a encriptação de todos os ficheiros, anexos e campos personalizados. Para obter mais informações, consulte [compreender o fluxo de autenticação do Web Server OAuth](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_web_server_oauth_flow.htm).  

## <a name="hybrid-scenarios"></a>Cenários híbridos
Cenários híbridos requerem integração personalizada alojada tempo de execução para ser instalada numa rede no local, no interior de uma rede virtual (Azure) ou dentro de uma nuvem privada virtual (Amazon). O tempo de execução automática alojada integração tem de ser capaz de aceder aos arquivos de dados local. Para mais informações sobre o tempo de execução automática alojada integração, consulte [como criar e configurar autoalojado integração runtime](https://docs.microsoft.com/en-us/azure/data-factory/create-self-hosted-integration-runtime). 

![canais de tempo de execução de integração personalizada alojada](media/data-movement-security-considerations/data-management-gateway-channels.png)

O canal de comando permite a comunicação entre os serviços de movimento de dados no Data Factory e integração personalizada alojada tempo de execução. A comunicação contém informações relacionadas com a atividade. O canal de dados é utilizado para transferir dados entre os arquivos de dados no local e arquivos de dados de nuvem.    

### <a name="on-premises-data-store-credentials"></a>Credenciais do arquivo de dados no local
As credenciais para os arquivos de dados no local são sempre encriptadas e armazenadas. Pode ser o armazenadas localmente na máquina de tempo de execução de integração personalizada alojada ou armazenados no storage do Azure Data Factory gerido (tal como as credenciais do arquivo de nuvem). 

- **Armazenar credenciais localmente**. Se pretende encriptar e armazenar credenciais localmente no tempo de execução automática alojada integração, siga os passos no [encriptar as credenciais para os arquivos de dados no local no Azure Data Factory](encrypt-credentials-self-hosted-integration-runtime.md). Todos os conectores de suportar esta opção. O tempo de execução de integração personalizada alojada utiliza o Windows [DPAPI](https://msdn.microsoft.com/library/ms995355.aspx) para encriptar as informações de credenciais e dados confidenciais. 

   Utilize o **New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential** cmdlet para encriptar credenciais de serviço ligado e confidenciais detalhes no serviço ligado. Em seguida, pode utilizar o JSON devolvido (com o **EncryptedCredential** elemento na cadeia de ligação) para criar um serviço ligado com o **AzureRmDataFactoryV2LinkedService conjunto** cmdlet.  

- **Arquivo no armazenamento do Azure Data Factory geridos**. Se utilizar diretamente a **conjunto AzureRmDataFactoryV2LinkedService** cmdlet com a ligação cadeias e credenciais inline no JSON, o serviço ligado é encriptado e armazenado no storage do Azure Data Factory geridos. As informações confidenciais ainda são encriptadas pelo certificado e a Microsoft gere automaticamente estes certificados.



#### <a name="ports-used-when-encrypting-linked-service-on-self-hosted-integration-runtime"></a>Portas utilizadas quando o serviço ligado no tempo de execução automática alojada integração de encriptação
Por predefinição, o PowerShell utiliza a porta 8050 na máquina com o tempo de execução automática alojada integração para comunicação segura. Se necessário, esta porta pode ser alterada.  

![Porta HTTPS para o gateway](media/data-movement-security-considerations/https-port-for-gateway.png)

 


### <a name="encryption-in-transit"></a>Encriptação em trânsito
Todas as transferências de dados são através de canal seguro HTTPS e TLS através de TCP para impedir ataques man-in-the-middle durante a comunicação com os serviços do Azure.

Também pode utilizar [IPSec VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md) ou [Azure ExpressRoute](../expressroute/expressroute-introduction.md) para proteger ainda mais o canal de comunicação entre a rede no local e o Azure.

Rede Virtual do Azure é uma representação lógica da rede na nuvem. Pode ligar uma rede no local à sua rede virtual ao configurar a VPN IPSec (site a site) ou ExpressRoute (peering privado).    

A tabela seguinte resume a rede e recomendações de configuração de tempo de execução automática alojada integração com base em diferentes combinações de origem e de destino localizações para o movimento de dados de híbrida.

| Origem      | Destino                              | Configuração da rede                    | Configuração do runtime de integração                |
| ----------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| Local | Máquinas virtuais e serviços em nuvem implementados em redes virtuais | VPN IPSec (ponto a site ou site a site) | O tempo de execução automática alojada integração pode ser instalado no local ou numa máquina virtual do Azure numa rede virtual. |
| Local | Máquinas virtuais e serviços em nuvem implementados em redes virtuais | ExpressRoute (peering privado)           | O tempo de execução automática alojada integração pode ser instalado no local ou numa máquina virtual do Azure numa rede virtual. |
| Local | Serviços baseados no Azure que tenham um ponto final público | ExpressRoute (peering público)            | O tempo de execução automática alojada integração tem de ser instalado no local. |

As imagens seguintes mostram a utilização de tempo de execução automática alojada integração para mover dados entre uma base de dados no local e serviços do Azure através do ExpressRoute e IPSec de VPN (com a Azure Virtual Network):

**ExpressRoute**

![Utilizar o ExpressRoute com o gateway](media/data-movement-security-considerations/express-route-for-gateway.png) 

**IPSec VPN**

![IPSec de VPN com o gateway](media/data-movement-security-considerations/ipsec-vpn-for-gateway.png)

### <a name="firewall-configurations-and-whitelisting-ip-addresses"></a>Configurações de firewall e a listas brancas de endereços IP

#### <a name="firewall-requirements-for-on-premisesprivate-network"></a>Requisitos da firewall da rede no local/privada  
Numa empresa, uma firewall empresarial é executada no router central da organização. Firewall do Windows é executado como um daemon no computador local em que o tempo de execução automática alojada integração está instalado. 

A tabela seguinte fornece os requisitos saídos porta e o domínio para firewalls empresariais:

| Nomes de domínio                  | Portas de saída | Descrição                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.servicebus.windows.net`    | 443, 80        | Necessário pelo runtime integração personalizada alojada se ligam aos serviços do movimento de dados no Data Factory. |
| `*.core.windows.net`          | 443            | Utilizada pelo runtime integração personalizada alojada para ligar à conta do storage do Azure ao utilizar o [testado cópia](copy-activity-performance.md#staged-copy) funcionalidade. |
| `*.frontend.clouddatahub.net` | 443            | Necessária para o tempo de execução automática alojada integração para ligar ao serviço Data Factory. |
| `*.database.windows.net`      | 1433           | (Opcional) É necessário quando copiar de ou para a SQL Database do Azure ou do Azure SQL Data Warehouse. Utilize a funcionalidade de teste cópia para copiar dados para a SQL Database do Azure ou do Azure SQL Data Warehouse sem abrir a porta 1433. |
| `*.azuredatalakestore.net`<br>`login.microsoftonline.com/<tenant>/oauth2/token`    | 443            | (Opcional) É necessário quando copiar de ou para o Azure Data Lake Store. |

> [!NOTE] 
> Poderá ter de gerir as portas ou domínios a listas brancas ao nível da firewall da empresa, conforme necessário, as origens de dados correspondentes. Esta tabela só utiliza a exemplos de SQL Database do Azure, o Azure SQL Data Warehouse e o Azure Data Lake Store.   

A tabela seguinte fornece os requisitos de porta de entrada para a Firewall do Windows:

| Portas de entrada | Descrição                              |
| ------------- | ---------------------------------------- |
| 8050 (TCP)    | Necessário para o cmdlet de encriptação do PowerShell, conforme descrito em [encriptar as credenciais para os arquivos de dados no local no Azure Data Factory](encrypt-credentials-self-hosted-integration-runtime.md)e a aplicação do Gestor de credenciais para definir as credenciais para os arquivos de dados no local de forma segura no tempo de execução automática alojada integração. |

![Requisitos de portas de gateway](media\data-movement-security-considerations/gateway-port-requirements.png) 

#### <a name="ip-configurations-and-whitelisting-in-data-stores"></a>Configurações de IP e a listas brancas nos arquivos de dados
Alguns arquivos de dados na nuvem necessitam também whitelist o endereço IP da máquina de aceder ao arquivo. Certifique-se de que o endereço IP da máquina de tempo de execução automática alojada integração na lista de permissões ou na firewall encontra corretamente configurado.

Arquivos de dados de nuvem seguintes requerem whitelist o endereço IP da máquina de tempo de execução automática alojada integração. Alguns destes arquivos de dados, por predefinição, poderão não requerer a listas brancas. 

- [Base de Dados SQL do Azure](../sql-database/sql-database-firewall-configure.md) 
- [Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md)
- [Azure Data Lake Store](../data-lake-store/data-lake-store-secure-data.md#set-ip-address-range-for-data-access)
- [BD do Cosmos para o Azure](../cosmos-db/firewall-support.md)
- [Amazon Redshift](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) 

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

**O tempo de execução automática alojada integração pode ser partilhado entre fábricas de dados diferentes?**

Iremos ainda não suporta esta funcionalidade. Estamos ativamente a trabalhar no mesmo.

**Quais são os requisitos de porta para o tempo de execução de integração personalizada alojada a funcionar?**

O tempo de execução automática alojada integração torna baseado em HTTP ligações de acesso à internet. As portas de saída 443 e 80 tem de ser abertas para o tempo de execução automática alojada integração tornar esta ligação. Abra a porta de entrada 8050 apenas no nível do computador (não o nível de firewall da empresa) para a aplicação do Gestor de credenciais. Se o SQL Database do Azure ou do Azure SQL Data Warehouse é utilizado como a origem ou de destino, terá de abrir a porta 1433 bem. Para obter mais informações, consulte o [configurações e os endereços IP a listas brancas Firewall](#firewall-configurations-and-whitelisting-ip-address-of-gateway) secção. 


## <a name="next-steps"></a>Passos Seguintes
Para obter informações sobre o desempenho de atividade de cópia de fábrica de dados do Azure, consulte [guia Otimização e de desempenho de atividade de cópia](copy-activity-performance.md).

 
