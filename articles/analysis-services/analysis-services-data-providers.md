---
title: "Bibliotecas de cliente necessárias para ligar ao Analysis Services do Azure | Microsoft Docs"
description: "Descreve as bibliotecas de cliente necessárias para aplicações de cliente e ferramentas ligar do Azure Analysis Services"
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/21/2018
ms.author: owend
ms.openlocfilehash: 2149330eb711fea76a144f5ec748ae6760c7746a
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/22/2018
---
# <a name="client-libraries-for-connecting-to-azure-analysis-services"></a>Bibliotecas de cliente para ligar ao Azure Analysis Services

Bibliotecas de cliente são necessárias para aplicações de cliente e ferramentas ligar aos servidores de Analysis Services. 

## <a name="download-the-latest-client-libraries"></a>Transferir as bibliotecas de cliente mais recentes  

|Transferência  |Versão  | 
|---------|---------|
|[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)    |    15.0.300.129.01      |
|[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)     |    15.0.300.129.01      |
|[AMO](https://go.microsoft.com/fwlink/?linkid=829578)     |   15.0.300.129.01      |
|[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)     |    115.0.300.129.01      |

## <a name="amo-and-adomd-on-nuget"></a>AMO e ADOMD no NuGet

Bibliotecas de cliente do Analysis Services Management Objects (AMO) e ADOMD estão disponíveis como pacotes instaláveis do [NuGet.org](https://www.nuget.org/). 

|Pacote  |Versão  | 
|---------|---------|
|[AMO](https://www.nuget.org/packages/Microsoft.AnalysisServices.retail.amd64/)    |    15.0.2      |
|[ADOMD](https://www.nuget.org/packages/Microsoft.AnalysisServices.AdomdClient.retail.amd64/)     |   15.0.2      |

Recomenda-se que migrar as referências de NuGet em vez de utilizar o instalador do MSI. 

As assemblagens de pacote NuGet AssemblyVersion siga versioning semântica: principais. SECUNDÁRIAS. PATCH. Referências do NuGet carregar a versão esperada, mesmo se existir uma versão diferente na GAC (resultante da instalação MSI). PATCH é aumentada para cada versão. Versões AMO e ADOMD são mantidas na sincronização.

## <a name="understanding-client-libraries"></a>Compreender as bibliotecas de cliente

Do Analysis Services utilizar três bibliotecas de cliente, também conhecido como fornecedores de dados. ADOMD.NET Analysis Services Management Objects (AMO), sendo bibliotecas de cliente gerido. O Analysis Services OLE DB Provider (MSOLAP DLL) é uma biblioteca de cliente nativo. Normalmente, são instaladas todas as três ao mesmo tempo. **Serviços de análise do Azure requer as versões mais recentes de todas as bibliotecas de três**. 

Aplicações de cliente da Microsoft como o Excel e o Power BI Desktop instalar todas as bibliotecas de cliente três e atualização-las quando existem novas versões. Consoante a versão ou a frequência de atualizações, algumas bibliotecas de cliente não podem ser as versões mais recentes necessárias pelo Azure Analysis Services. O mesmo se aplica a aplicações personalizadas ou a outras interfaces, como sCmd, TOM ou ADOMD.NET. Estas aplicações requerem manualmente ou programaticamente instalar as bibliotecas. As bibliotecas de cliente para a instalação manual estão incluídas nos pacotes de funcionalidades do SQL Server como distributable pacotes. No entanto, estas bibliotecas de cliente estão associadas a versão do SQL Server e não podem ser a versão mais recente.  

Bibliotecas de cliente para ligações de cliente são diferentes de fornecedores de dados necessárias para ligar a partir de um servidor de Analysis Services do Azure a uma origem de dados. Para saber mais sobre ligações de origem de dados, consulte [Datasource ligações](analysis-services-datasource.md).

## <a name="client-library-types"></a>Tipos de biblioteca de cliente

### <a name="analysis-services-ole-db-provider-msolap"></a>Fornecedor de serviços OLE DB do Analysis Services (MSOLAP) 

 Analysis Services OLE DB Provider (MSOLAP) é a biblioteca de cliente nativo para ligações de base de dados do Analysis Services. É utilizado indiretamente pelo ADOMD.NET e AMO, delegar a pedidos de ligação para o fornecedor de dados. Também pode chamar o fornecedor de OLE DB diretamente a partir do código da aplicação.  
  
 O Analysis Services OLE DB Provider é instalado automaticamente pela maioria das ferramentas e aplicações de cliente utilizadas para aceder a bases de dados do Analysis Services. Tem de ser instalado em computadores utilizados para aceder a dados do Analysis Services.  
  
 Fornecedores de OLE DB, muitas vezes, são especificadas nas cadeias de ligação. Uma cadeia de ligação do Analysis Services utiliza um nomenclature diferentes de consultar o fornecedor de OLE DB: MSOLAP. \<versão >. dll.

### <a name="amo"></a>AMO  

 AMO é uma biblioteca de cliente geridos utilizada para administração de servidor e a definição de dados. Tem instalados e utilizados por ferramentas e aplicações de cliente. Por exemplo, o SQL Server Management Studio (SSMS) utiliza AMO para ligar ao Analysis Services.  
  
 Uma ligação com o AMO é normalmente mínima, constituída por `“data source=\<servername>”`. Depois de é estabelecida uma ligação, utilize a API para trabalhar com coleções de base de dados e os objetos principais. SSDT e SSMS utilizar AMO para ligar a uma instância do Analysis Services.  

  
### <a name="adomd"></a>ADOMD

 ADOMD.NET é uma biblioteca de cliente de dados geridos utilizada para consultar os dados de Analysis Services. Tem instalados e utilizados por ferramentas e aplicações de cliente. 
  
 Ao ligar a uma base de dados, as propriedades de cadeia de ligação para todas as bibliotecas de três são semelhantes. Praticamente qualquer cadeia de ligação, a definir para ADOMD.NET utilizando [Microsoft.AnalysisServices.AdomdClient.AdomdConnection.ConnectionString](https://msdn.microsoft.com/library/microsoft.analysisservices.adomdclient.adomdconnection.connectionstring.aspx) também funciona para AMO e a análise serviços OLE DB Provider (MSOLAP). Para obter mais informações, consulte [propriedades de cadeia de ligação &#40; Do Analysis Services &#41; ](https://docs.microsoft.com/sql/analysis-services/instances/connection-string-properties-analysis-services).  

  
##  <a name="bkmk_LibUpdate"></a> Como determinar a versão da biblioteca de cliente   
  
### <a name="oleddb-msolap"></a>OLEDDB (MSOLAP)  
  
1.  Aceda a `C:\Program Files\Microsoft Analysis Services\AS OLEDB\140`. Se tiver mais do que uma pasta, selecione o número mais alto.
  
2.  Clique com botão direito **msolap.dll** > **propriedades** > **detalhes**. Se a dll é denominada msolap140.dll, é mais antiga que a versão mais recente e deve ser atualizado.
    
    ![Detalhes da biblioteca de cliente](media/analysis-services-data-providers/aas-msolap-details.png)
    
  
### <a name="amo"></a>AMO

1. Aceda a `C:\Windows\Microsoft.NET\assembly\GAC_MSIL\Microsoft.AnalysisServices\v4.0_14.0.0.0__89845dcd8080cc91`.
2. Clique com botão direito **Microsoft.AnalysisServices** > **propriedades** > **detalhes**.  

### <a name="adomd"></a>ADOMD

1. Aceda a `C:\Windows\Microsoft.NET\assembly\GAC_MSIL\Microsoft.AnalysisServices.AdomdClient\v4.0_14.0.0.0__89845dcd8080cc91`.
2. Clique com botão direito **Microsoft.AnalysisServices.AdomdClient** > **propriedades** > **detalhes**.  


## <a name="next-steps"></a>Passos Seguintes
[Estabelecer ligação com o Excel](analysis-services-connect-excel.md)    
[Ligar com o Power BI](analysis-services-connect-pbi.md)
