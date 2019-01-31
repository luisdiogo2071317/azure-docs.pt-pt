---
title: Dimensione um titleSuffix de serviço web Machine Learning Studio: Descrição do Azure Machine Learning Studio: Saiba como aumentar a simultaneidade de um serviço web do Azure Machine Learning Studio através da adição de pontos finais adicionais.
services: machine-learning ms.service: machine-learning ms.subservice: studio ms.topic: article

autor: ericlicoding MS. Author: amlstudiodocs MS. Custom: seodec18, anterior ms.author=yahajiza, autor anterior = YasinMSFT MS: 01/23/2017
---
# <a name="scaling-an-azure-machine-learning-studio-web-service-by-adding-additional-endpoints"></a>Dimensionar um web service do Azure Machine Learning Studio através da adição de pontos finais adicionais
> [!NOTE]
> Este tópico descreve técnicas aplicáveis a uma **clássico** serviço web Machine Learning. 
> 
> 

Por predefinição, cada serviço Web publicado está configurado para suportar 20 pedidos simultâneos e pode ser tão elevado como 200 pedidos em simultâneo. O Azure Machine Learning otimiza automaticamente a definição para proporcionar o melhor desempenho do web Service e o valor de portal é ignorado. 

Se tiver de suportar plano para chamar a API com uma carga maior que um valor de chamadas simultâneas do máximo de 200, deve criar vários pontos de extremidade no mesmo serviço da Web. Em seguida, pode distribuir aleatoriamente a carga em todos os.

O dimensionamento de um serviço Web é uma tarefa comum. São algumas razões para dimensionar para suportar mais de 200 pedidos em simultâneo, aumentar a disponibilidade por meio de vários pontos de extremidade ou fornecer pontos de extremidade separados para o serviço web. Pode aumentar a escala ao adicionar pontos finais adicionais para o mesmo serviço da Web através da [serviço da Web do Azure Machine Learning](https://services.azureml.net/) portal.

Para obter mais informações sobre como adicionar novos pontos de extremidade, consulte [criando pontos de extremidade](create-endpoint.md).

Tenha em atenção que pode ser prejudicial se não estiver a chamar a API com uma taxa elevada do mesmo modo com uma contagem alta de simultaneidade. Poderá ver tempos limite esporádica e/ou picos na latência se colocar uma carga relativamente baixa numa API configurada para uma carga elevada.

As APIs síncronas são normalmente utilizadas em situações em que uma baixa latência for o pretendido. Latência aqui indica o tempo necessário para a API concluir um pedido e, não se responsabiliza por qualquer atrasos de rede. Digamos que tem uma API com uma latência de 50 ms. Para consumir totalmente a capacidade disponível com o nível de limitação do alto e máx. de chamadas simultâneas = 20, precisa chamar esta API 20 * 1000 / 50 = 400 horas por segundo. Expandir isso ainda mais, um máximo de chamadas simultâneas de 200 permite que chamar a API 4000 de vezes por segundo, supondo que uma latência de 50 ms.

<!--Image references-->
[1]: ./media/scaling-webservice/machlearn-1.png
[2]: ./media/scaling-webservice/machlearn-2.png
