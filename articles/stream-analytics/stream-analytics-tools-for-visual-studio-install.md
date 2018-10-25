---
title: Configurar as ferramentas do Azure Stream Analytics para Visual Studio
description: Este artigo descreve os requisitos de instalação e como configurar as ferramentas do Azure Stream Analytics para Visual Studio.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/22/2018
ms.openlocfilehash: 47bdbc2297fb1e1ef24f13710391b4e9d154d37c
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49983827"
---
# <a name="install-azure-stream-analytics-tools-for-visual-studio"></a>Instalar as ferramentas do Azure Stream Analytics para Visual Studio
Ferramentas do Azure Stream Analytics suportam o Visual Studio 2017, 2015 e 2013. Este artigo descreve como instalar e desinstalar as ferramentas.

Para obter mais informações sobre como utilizar as ferramentas, consulte [ferramentas do Stream Analytics para Visual Studio](stream-analytics-quick-create-vs.md).

## <a name="install"></a>Instalar
### <a name="visual-studio-2017"></a>Visual Studio 2017
* Baixe [Visual Studio 2017 (15.3 ou superior)](https://www.visualstudio.com/). As edições Enterprise (Ultimate/Premium), Professional e Community são suportadas. A edição Express não é suportada. 
* Ferramentas do Stream Analytics fazem parte dos **desenvolvimento do Azure** e **armazenamento de dados e processamento** cargas de trabalho no Visual Studio 2017. Ative uma destas duas cargas de trabalho como parte da sua instalação do Visual Studio.

Ativar a **armazenamento de dados e processamento** carga de trabalho conforme mostrado:

![Carga de trabalho de dados, armazenamento e processamento está selecionada](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-2017-install-01.png)

Ativar a **desenvolvimento do Azure** carga de trabalho conforme mostrado:

![Carga de trabalho de desenvolvimento do Azure está selecionada](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-2017-install-02.png)

* No menu Ferramentas, escolha **extensões e atualizações**. Ferramentas de localizar o Azure Data Lake e do Stream Analytics nas extensões instaladas e clique em **atualização** para instalar a extensão mais recente. 

![Extensões do Visual Studio e atualizações](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-extensions-updates.png)

### <a name="visual-studio-2013-2015"></a>O Visual Studio 2013, 2015
* Instale o Visual Studio 2015 ou Visual Studio 2013 atualização 4. As edições Enterprise (Ultimate/Premium), Professional e Community são suportadas. A edição Express não é suportada. 
* Instalar o Microsoft Azure SDK para .NET versão 2.7.1 ou superior ao utilizar o [instalador de plataforma Web](https://www.microsoft.com/web/downloads/platform.aspx).
* Instale [ferramentas do Azure Stream Analytics para Visual Studio](https://www.microsoft.com/en-us/download/details.aspx?id=49504).

## <a name="update"></a>Atualizar

### <a name="visual-studio-2017"></a>Visual Studio 2017
O novo lembrete de versão aparece a notificação do Visual Studio.

![Visual Studio novo versão lembrete](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-new-version-reminder-vs-tools.png)

### <a name="visual-studio-2013-and-visual-studio-2015"></a>Visual Studio 2013 e Visual Studio 2015
As ferramentas do Stream Analytics instaladas para o Visual Studio verificar novas versões automaticamente. Siga as instruções na janela de pop-up para instalar a versão mais recente. 


## <a name="uninstall"></a>Desinstalar

### <a name="visual-studio-2017"></a>Visual Studio 2017
Clique duas vezes o instalador do Visual Studio e selecione **modificar**. Limpar o **do Azure Data Lake e ferramentas do Stream Analytics** caixa de verificação a partir de qualquer uma a **processamento e armazenamento de dados** carga de trabalho ou o **desenvolvimento do Azure** carga de trabalho.

### <a name="visual-studio-2013-and-visual-studio-2015"></a>Visual Studio 2013 e Visual Studio 2015
Aceda ao painel de controlo e desinstalar **ferramentas do Microsoft Azure Data Lake e do Stream Analytics para Visual Studio**.





