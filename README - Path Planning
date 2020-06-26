Serviço - Path Planning
==================

Este programa corresponde a um módulo componente do Sistema MobiLysa – Serviço de Localização e Navegação do Robô Lysa para Ambientes Internos. Este sistema corresponde a um conjunto de módulos que juntos permitem que o cão-guia robô Lysa seja localizado em um ambiente interno e que, a partir do destino desejado, um planejamento de navegação seja realizado. Tanto a localização, quanto navegação são efetuadas em tempo real até que o robô atinja o seu destino final. O ambiente deve estar equipado com uma rede de câmeras IP e possuir cobertura de WiFi por toda a área onde o robô deve se movimentar.

Especificamente, este módulo tem como objetivo fornecer um serviço capaz de calcular rotas e planejar caminhos para diversos robôs introduzidos no sistema. O serviço recebe como inicialização o mapa do ambiente como um conjunto de pontos e um conjunto de pontos dos obstáculos, que serão evitados pelo robô. Com estas informações, o serviço consome mensagens do tópico “.GetPath”  do broker esperando uma requisição  de caminho  com o ponto onde está localizado o agente que fez a requisição e o ponto final desejado. Após recebida a requisição, o serviço calcula o campo potencial do ambiente utilizando o mapa inicializado como configuração e, consequentemente, calcula a trajetória seguindo os menores potenciais entre o ponto inicial e o ponto final desejado. Tendo calculado a trajetória, o serviço publica tal sequência de pontos no tópico que estiver sendo consumido pelo controlador do robô e espera por uma confirmação do mesmo. 
O módulo é composto por dois scripts  sendo o  pathplanner.py  responsavel por definir a classe do planejador de caminhos e suas funcionalidades e o script service.py responsável por prover a funcionalidade de planejamento de caminhos como um serviço no espaço inteligente. coordenadas, realiza a conversão para coordenadas e as envia para o serviço PathPlanner. A conversão para coordenadas é feita através da correspondência da palavra recebida com um dicionário, contendo palavras em português, inglês e suas respectivas coordenadas. Caso haja correspondência, uma mensagem do tipo is_msg.PathRequest é criada contendo as informações do gateway e, posteriormente, enviada para o serviço PathPlanner. Após o envio, o serviço PI retorna ao estado de espera de mensagem, para repetir o ciclo ao receber uma nova mensagem.


Dependências:
-----
Este serviço não possui dependências em relação a outros serviços.

Eventos:
--------
<img width=850/> ⇒ Mensagem recebida | <img width=850/> Encaminha ⇒ | <img width=500/> Descrição  
:------------ | :-------- | :----------
:incoming_envelope: **tópico:** `PathPlanner.AreaID.{area_id}.GetPath` <br> :gem: **Tipo de mensagem:** [PathRequest] | :incoming_envelope: **tópico:*RobotController.{robot_id}.SetTask* `` <br> :gem: **Tipo de mensagem::** [RobotTaskRequest] | `Planejamento e envio de rota para o robô`

[RobotTaskRequest]: https://github.com/labviros/is-msgs/tree/master/docs#is.robot.RobotTaskRequest
[PathRequest]: https://github.com/labviros/is-msgs/tree/master/docs#is.robot.PathRequest
```
{area_id} = número identificador do ambiente, passado no arquivo de configuração.
{robot_id} = número identificador do robô, recebido na mensagem `PathPlanner.AreaID.{area_id}.GetPath`
```

Configuração:
----------------
A configuração do serviço é realizada através de um arquivo JSON, passado como argumento. Um exemplo de configuração pode ser encontrado em: [`etc/conf/options.json`]

Exemplos:
------------
Um exemplo, que simula um ambiente, planeja uma rota de acordo com o cliente e envia a rota a ser seguida para o robô, pode ser encontrado em [`examples/client.py`](examples/client.py).


