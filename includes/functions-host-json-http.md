```json
{
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 20,
        "maxConcurrentRequests": 10,
        "dynamicThrottlesEnabled": false
    }
}
```

|Propriedade  |Predefinição | Descrição |
|---------|---------|---------| 
|routePrefix|api|O prefixo de rota que aplica-se a todas as rotas. Utilize uma cadeia vazia para remover o prefixo de predefinição. |
|maxOutstandingRequests|-1|O número máximo de pedidos pendentes que são guardados em qualquer momento. Este limite inclui pedidos que são colocados em fila, mas não iniciaram a executar, bem como qualquer no execuções de progresso. Quaisquer pedidos recebidos este limite são rejeitados com uma resposta "Demasiado ocupado" 429. Que permite que os chamadores recorrer estratégias de repetição baseados no tempo e também ajuda a controlar as latências de pedido máximo. Isto apenas controla a colocação em fila que ocorre dentro do caminho de execução do script anfitrião. Outras filas, tais como a fila de pedidos do ASP.NET continuarão a estar em vigor e afetada por esta definição. A predefinição é unbounded.|
|maxConcurrentRequests|-1|O número máximo de funções de http que será executado em paralelo. Isto permite-lhe simultaneidade de controlo, que pode ajudar a gerir a utilização de recursos. Por exemplo, poderá ter uma função de http que utiliza uma grande quantidade de recursos do sistema (memória / / sockets de cpu) que faz com que problemas quando a simultaneidade é demasiado elevada. Ou poderá ter uma função que faz pedidos de saída a terceiros serviço e as chamadas têm de ser taxa limitada. Nestes casos, pode ajudar a aplicar uma limitação aqui. A predefinição é unbounded.|
|dynamicThrottlesEnabled|False|Quando ativada, esta causas de definição do pipeline de processamento de pedidos para verificar periodicamente o desempenho do sistema contadores como threads/ligações/processos/cpu/memória/etc e se algum dos se esses contadores através de um limiar elevado incorporado (80%), os pedidos de será rejeitado com uma resposta "Demasiado ocupado" 429 até o counter(s) regressar à níveis normais.|
