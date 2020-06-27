Serviço - Robot Controller
==================

Este programa corresponde a um módulo componente do Sistema MobiLysa – Serviço de Localização e Navegação do Robô Lysa para Ambientes Internos. Este sistema corresponde a um conjunto de módulos que juntos permitem que o cão-guia robô Lysa seja localizado em um ambiente interno e que, a partir do destino desejado, um planejamento de navegação seja realizado. Tanto a localização, quanto navegação são efetuadas em tempo real até que o robô atinja o seu destino final. O ambiente deve estar equipado com uma rede de câmeras IP e possuir cobertura de WiFi por toda a área onde o robô deve se movimentar.

Especificamente, este módulo é responsável calcular sinais de controle (velocidade linear e angular) necessários para executar uma dada tarefa. Tal tarefa pode corresponder ao posicionamento do robô em uma dada posição, ou no segmento de uma trajetória descrita por um conjunto de pontos e velocidades. O controlador utilizado é do tipo de cinemática inversa para robôs com tração diferencial. A localização do robô controlado é estimada utilizando a localização tridimensional de um marcador fixado junto ao robô, que é obtida através das imagens por um outro módulo. A estimativa pode levar em consideração a informação de um conjunto de câmeras instalados no ambiente e, por isso, utiliza um Filtro de Kalman para realizar a filtragem e agregação das diversas informações de localização do padrão. Além de enviar comandos para o robô no qual deseja-se controlar, para cada comando enviado também gerada uma informação correspondente ao status da execução da tarefa, contendo posição atual e desejada do robô, além dos sinais de controle para ele enviados. Os comandos para execução de uma tarefa, informações de localização, envio de sinais de controle para o robô e mensagens de status são enviados e recebidos através de um barramento de mensagens.

O módulo é composto por um script principal, o service.py, responsável por carregar os outros componentes e estabelecer as comunicações necessárias com os outros módulos. Os componentes são listados abaixo junto de sua ut:

* subscription_manager.py: responsável por controlar o recebimento de mensagens contendo as localizações do marcador localizado no robô, baseado nas câmeras disponíveis no espaço inteligente.
* pose_estimation.py: possui a classe PoseEstimation, que basedo nas informações de localização recebidas, realiza a estimativa da pose utilizando um Filtro de Kalman. Este utiliza a estimativa anterior que também é armazenada neste componente, além de uma descrição programática do modelo cinemático do robô.
* inverse_kinematics_controller.py: possui a classe InverseKinematicsController, que utiliza a informação de localização estimada pela classe PoseEstimation, além de um conjunto de parâmetros do controlador de cinemática inversa, para determinar os valores de velocidade linear e angular que devem ser enviados ao robô.
* basic_move_task.py: possui a classe BasicMoveTask responsável por abstrair uma tarefa, seja ela de posicionamento ou de trajetória, armazenando a próxima posição desejada, além de calcular valores utilizados pela classe InverseKinematicsController para calcular os sinais de controle.
* extended_kalman_filter.py: possui a classe ExtendedKalmanFilter responsável por realizar as etapas de predição e correção do Filtro de Kalman, baseado nas localizações do robô recebidas.

Dependências:
-----
Este serviço possui dependências dos seguintes serviços:
* [is-broker-events](https://github.com/labviros/is-broker-events): Utilizado para verificar qual câmera está disponível.
* [is-frame-transformation](https://github.com/labviros/is-frame-transformation): Utilizado para coletar as transformações do referencial do robô para o referencial do mundo, utilizando as câmeras disponíveis. NOTA: As transformações devem estar disponíveis de alguma forma, isto pode implicar na dependência de um outro serviço, como [is-aruco-detector](https://github.com/labviros/is-aruco-detector).
* [is-robots](https://github.com/labviros/is-robots): Este serviço espera que o robô tenha um gateway compatível com a API padrão. Particularmente, espera-se que o robô tenha implementado a comunicação via tópico **RobotGateway.{robot_id}.SetConfig**.

Eventos:
--------
<img width=850/> ⇒ Mensagem recebida | <img width=850/> Encaminha ⇒ | <img width=500/> Descrição  
:------------ | :-------- | :----------
:incoming_envelope: **tópico:** `RobotController.{robot_id}.SetTask` <br> :gem: **tipo de mensagem:** [RobotTaskRequest] | :incoming_envelope: **tópico:** `{request.reply_to}` <br> :gem: **tipo de mensagem:** [RobotTaskReply] | `[RPC] Configura a tarefa atual a ser executada.`

[RobotTaskRequest]: https://github.com/labviros/is-msgs/tree/master/docs#is.robot.RobotTaskRequest
[RobotTaskReply]: https://github.com/labviros/is-msgs/tree/master/docs#is.robot.RobotTaskReply
```
{robot_id} = número identificador do robô, passado no arquivo de configuração
```

Configuração:
----------------
A configuração do serviço é realizada através de um arquivo JSON, passado como argumento. Um exemplo de configuração pode ser encontrado em: [`etc/conf/options.json`]

Exemplos:
------------
Um exemplo, de como configurar uma tarefa e monitorar o seu progresso pode ser encontrado em [`examples/client.py`](examples/client.py).


