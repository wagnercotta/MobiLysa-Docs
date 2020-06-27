Serviço - Lysa Gateway
==================

Este programa corresponde a um módulo componente do Sistema MobiLysa – Serviço de Localização e Navegação do Robô Lysa para Ambientes Internos. Este sistema corresponde a um conjunto de módulos que juntos permitem que o cão-guia robô Lysa seja localizado em um ambiente interno e que, a partir do destino desejado, um planejamento de navegação seja realizado. Tanto a localização, quanto navegação são efetuadas em tempo real até que o robô atinja o seu destino final. O ambiente deve estar equipado com uma rede de câmeras IP e possuir cobertura de WiFi por toda a área onde o robô deve se movimentar.

Especificamente, este módulo tem como objetivo fazer a interface entre um robô, no caso o robô Lysa, e o espaço inteligente. Para isso, o módulo deve ser executado no robô e importar as classes de controle do robô, envelopando o driver do próprio na linguagem python, abstraindo as possíveis funcionalidades do robô para que o mesmo receba apenas os valores de velocidade linear e angular, e se comunique com o espaço inteligente com os valores medidos de velocidades.
O módulo possui três scripts. O  lysa_driver.py  é responsável por envelopar o driver do robô, implementando as funções necessárias para utilizar o robô no espaço usando o driver do mesmo. Diferentes robôs devem ter esta peça de software substituída para adaptar seus drivers proprietários. O script gateway.py  é o responsável por adaptar o driver do robô para utilizar as mensagens do espaço inteligente e independe do robô utilizado, recebendo apenas uma instância do driver como atributo. Por fim, o script service.py  realiza a interligação do driver  e gateway  do robô, conectando o gateway ao broker e deixando o robô disponível para controle pelo espaço inteligente.

Dependências:
-----
Este serviço não possui dependências em relação a outros serviços.

Eventos:
--------
<img width=850/> ⇒ Mensagem recebida | <img width=850/> Encaminha ⇒ | <img width=500/> Descrição  
:------------ | :-------- | :----------
:incoming_envelope: **tópico:** `RobotGateway.{robot_id}.SetConfig` <br> :gem: **tipo de mensagem:** [RobotConfig] | :incoming_envelope: **tópico:** `{request.reply_to}` <br> :gem: **tipo de mensagem:** Empty | `Define a configuração do robô, por exemplo, velocidade atual`
:incoming_envelope: **tópico:** `RobotGateway.{robot_id}.GetConfig` <br> :gem: **tipo de mensagem:** Empty | :incoming_envelope: **tópico:** `{request.reply_to}` <br> :gem: **tipo de mensagem:** [RobotConfig] | `Retorna a configuração atual`

[Empty]: https://github.com/protocolbuffers/protobuf/blob/master/src/google/protobuf/empty.proto
[RobotConfig]: https://github.com/labviros/is-msgs/tree/master/docs#is.robot.RobotConfig
```
{robot_id} = número identificador do robô, passado no arquivo de configuração
```

Configuração:
----------------
A configuração do serviço é realizada através de um arquivo JSON, passado como argumento. Um exemplo de configuração pode ser encontrado em: [`etc/conf/options.json`]

Exemplos:
------------
Um exemplo, que define a configuração do robô com uma dada velocidade e depois solicita o retorno da configuração atual do robô, pode ser encontrado em [`examples/client.py`](examples/client.py).


