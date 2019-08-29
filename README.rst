#############
Minicurso SDN
#############

Instalação
==========

Dependências
------------

Antes de iniciar a instalação do Kytos, verifique sua versão do Python, se for inferior a 3.6,
execute os comandos a seguir:

.. code-block:: shell

  $ sudo add-apt-repository ppa:jonathonf/python-3.6
  $ sudo apt update

Algumas bibliotecas devem ser instaladas para a execução correta do Kytos:

.. code-block:: shell

  $ sudo apt install git libpython3.6-dev python3.6 python3-venv mininet

Criar novo ambiente virutal
---------------------------

Para instalar o projeto Kytos, recomendamos o uso de ambientes virtuais.
O principal motivo dessa recomendação é manter as dependências de diferentes
projetos em locais separados. Os comandos abaixo são responsáveis por esta etapa:

.. code-block:: shell

   $ python3.6 -m venv kytos-environment

Ative o ambiente virutal:

.. code-block:: shell

   $ source kytos-environment/bin/activate

Instalar Kytos
--------------

Primeiro, atualize o pacote pip que já está instalado no ambiente, com
setuptools e wheel:

.. code-block:: shell

    (kytos-environment) $ pip install --upgrade pip setuptools wheel

Clone os projetos python-openflow, kytos-utils e kytos:

.. code-block:: shell

  for repo in python-openflow kytos-utils kytos; do
    (kytos-environment) $ git clone https://github.com/kytos/${repo}
  done

Então, execute os comandos a seguir para instalar o Kytos:

.. code-block:: shell

    for repo in python-openflow kytos-utils kytos; do
      (kytos-environment) $ cd ${repo}
      (kytos-environment) $ python3 setup.py develop
      (kytos-environment) $ cd ..
    done

Execução
========

Rode o comando a seguir para executar o Kytos:

.. code-block:: console

  $ kytosd -f
        _          _
       | |        | |
       | | ___   _| |_ ___  ___
       | |/ / | | | __/ _ \/ __|
       |   <| |_| | || (_) \__ \
       |_|\_\__,  |\__\___/|___/
              __/ |
             |___/

      Welcome to Kytos SDN Platform!

      We are making a huge effort to make sure that this console will work fine
      but for now it's still experimental.

      Kytos website.: https://kytos.io/
      Documentation.: https://docs.kytos.io/
      OF Address....: tcp://0.0.0.0:6653
      WEB UI........: http://0.0.0.0:8181/
      Kytos Version.: 2019.1

  kytos $>

Então, abra outro terminal, ative novamente o ambiente virtual e rode o comando abaixo
para instalar algumas NApps:

.. code-block:: shell

    (kytos-environment) $ kytos napps install kytos/of_core kytos/of_l2ls kytos/of_lldp kytos/topology

Agora, você pode verificar se as NApps foram instaladas através do comando:

.. code-block:: shell

    (kytos-environment) $ kytos napps list

Se elas estiverem instaladas, você pode ativá-las:

.. code-block:: shell

    (kytos-environment) $ kytos napps enable kytos/of_core kytos/of_l2ls kytos/of_lldp kytos/topology

Por fim, você pode construir uma simples rede com o seguinte comando:

.. code-block:: shell

  (kytos-environment) $ sudo mn --topo linear,2 --mac --controller=remote,ip=127.0.0.1 --switch ovsk,protocols=OpenFlow13

Depois de executar este comando, a saída mininet mostrará que dois hosts e dois switches foram criados. Então o
o console mininet será ativado e você poderá enviar comandos para cada switch ou host conectado. Por exemplo, para
verificar o endereço IP do host1 (`h1`), você pode usar o comando abaixo:

.. code-block:: console

  mininet> h1 ifconfig
  h1-eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
           inet 10.0.0.1  netmask 255.0.0.0  broadcast 10.255.255.255
           inet6 fe80::200:ff:fe00:1  prefixlen 64  scopeid 0x20<link>
           ether 00:00:00:00:00:01  txqueuelen 1000  (Ethernet)
           RX packets 20  bytes 2394 (2.3 KiB)
           RX errors 0  dropped 0  overruns 0  frame 0
           TX packets 13  bytes 1018 (1018.0 B)
           TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

  lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
           inet 127.0.0.1  netmask 255.0.0.0
           inet6 ::1  prefixlen 128  scopeid 0x10<host>
           loop  txqueuelen 1  (Local Loopback)
           RX packets 0  bytes 0 (0.0 B)
           RX errors 0  dropped 0  overruns 0  frame 0
           TX packets 0  bytes 0 (0.0 B)
           TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

  mininet>

Como o Kytos está executando com as NApps necessárias, sua topologia deve estar totalmente funcional até agora.
Você pode testá-lo com ping (protocolo ICMP) executando, no mininet:

.. code-block:: console

  mininet> h1 ping h2
  PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.
  64 bytes from 10.0.0.2: icmp_seq=1 ttl=64 time=62.6 ms
  64 bytes from 10.0.0.2: icmp_seq=2 ttl=64 time=0.271 ms
  64 bytes from 10.0.0.2: icmp_seq=3 ttl=64 time=0.099 ms
  64 bytes from 10.0.0.2: icmp_seq=4 ttl=64 time=0.140 ms

Bom trabalho!
