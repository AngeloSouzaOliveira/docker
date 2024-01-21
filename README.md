
## O que é o Docker?
Docker é uma tecnologia poderosa, sendo uma ferramenta que promete encapsular facilmente o processo de criação de um artefato distribuível para qualquer aplicativo, implantando-o em escala em qualquer ambiente e simplificando o fluxo de trabalho e a capacidade de resposta das organizações de software Agile.

-  Foi a primeira ferramenta amplamente acessível construída sobre uma tecnologia muito mais recente chamada conteinerização.
- Alterou significativamente as expectativas de todos sobre como um fluxo de trabalho de integração contínua e entrega contínua (CI/CD) deveria funcionar. 
- A sua combinação da facilidade de teste de aplicativos e ferramentas de implantação, além de fornecer interfaces que facilitam a implementação da automação e orquestração do fluxo de trabalho, o Docker fornece um conjunto de recursos muito bom.
- Promove aplicações mais escaláveis e mais confiáveis, onde os aplicativos em contêineres são obrigados a seguir as boas práticas

## Benefícios do fluxo de trabalho
Quando bem implementado, beneficia organizações, equipes, desenvolvedores e engenheiros de diversas maneiras, tornando as decisões arquitetônicas mais simples porque todos os aplicativos têm essencialmente a mesma aparência externa, da perspectiva do sistema de hospedagem.
- Empacotar software de uma forma que aproveite as habilidades que os desenvolvedores já possuem (padrão Open Container Initiative - OCI);
- Agrupar software de aplicação e sistemas de arquivos de sistema operacional necessários em um único formato de imagem padronizado;
- Usar artefatos empacotados para testar e entregar exatamente o mesmo artefato para todos os sistemas em todos os ambientes;
- Abstrair aplicativos de software do hardware sem sacrificar recursos; e
-  Os contêineres utilizam menos recursos do sistema que a uma VM(hipervisor), mas devem ser baseados no mesmo sistema operacional subjacente (por exemplo, Linux).

### Cliente Docker
- [docker] - comando usado para controlar a maior parte do fluxo de trabalho do Docker e se comunicar com servidores Docker remotos.

### Servidor Docker
- [dockerd] comando que é usado para iniciar o processo do servidor Docker que cria e inicia contêineres por meio de um cliente.

### Imagens Docker ou OCI
- As imagens Docker e OCI consistem de uma ou mais camadas do sistema de arquivos e alguns metadados importantes que representam todos os arquivos necessários para executar um aplicativo em contêiner. Uma imagem Docker é qualquer imagem compatível com o conjunto de ferramentas Docker, enquanto uma imagem OCI é especificamente uma imagem que atende ao padrão Open Container Initiative e tem garantia de funcionamento com qualquer ferramenta compatível com OCI.

### Contêiner Linux
- Este é um contêiner que foi instanciadode uma imagem Docker ou OCI. Um contêiner específico só pode existir uma vez; no entanto, você pode criar facilmente vários contêineres a partir da mesma imagem. O termo contêiner Docker é um nome impróprio, pois o Docker simplesmente aproveita a funcionalidade do contêiner do sistema operacional.

### Host atômico ou imutável
- Um host atômico ou imutável é uma imagem de sistema operacional pequena e bem ajustada, como Fedora CoreOS , que suporta hospedagem de contêineres e atualizações atômicas de sistema operacional.

### Containerd
- [containerd], que é o tempo de execução certificado pela OCI de alto nível nas versões modernas do Docker e Kubernetes. 

Os tempos de execução a seguir são usados pela [containerd] para gerenciar e criar contêineres:
- [runc] - é frequentemente usado como tempo de execução de nível inferior padrão pelo [containerd];
- [crun] - é escrito em C e projetado para ser rápido e ocupar pouco espaço de memória;
- **Kata Containers** da Intel, Hyper e OpenStack Foundation é um tempo de execução virtualizado que pode executar uma combinação de contêineres e máquinas virtuais;
- **gVisor do Google** é um tempo de execução em sandbox, implementado inteiramente no espaço do usuário; e 
- **Nabla Containers** fornece outro tempo de execução em sandbox projetado para reduzir significativamente a superfície de ataque de contêineres Linux.

### Fluxo básico
Várias são as peças estão por trás da API Docker, incluindo [containerd] e [runc], mas a interação básica do sistema é um cliente conversando por meio de uma API com um servidor.  Este é o fluxo básico de comunicação entre os componentes do Docker. 

Pode-se pensar como consistindo de duas partes: o cliente e o servidor/daemon, ou seja, o cliente é uma interface que os usuários utilizam para interagir com o Docker, enquanto o servidor/daemon é responsável por gerenciar os contêineres, imagens e volumes. Essas duas partes trabalham juntas para permitir que os usuários criem, gerenciem e executem aplicativos em contêineres de forma eficiente. contudo o Docker é um pouco diferente em estrutura de alguns outros softwares cliente/servidor. Ele tem um [docker] cliente e um [dockerd] servidor, mas em vez de ser totalmente monolítico, o servidor orquestra alguns outros componentes nos bastidores em nome do cliente, incluindo [containerd-shim-runc-v2], que é usado para interagir com [runc] e [containerd]. No entanto, o Docker esconde qualquer complexidade por trás da API de servidor simples, portanto, você pode considerá-lo apenas como um cliente e servidor simples para a maioria dos propósitos. Cada host Docker normalmente terá um servidor Docker em execução que pode gerenciar qualquer número de contêineres. Você pode então usar a [docker] ferramenta de linha de comando para conversar com o servidor, seja do próprio servidor ou, se estiver devidamente protegido, de um cliente remoto.

A [docker] ferramenta de linha de comando e [dockerd] o daemon podem conversem entre si por meio de soquetes Unix e portas de rede. Docker, Inc., registrou três portas na Internet Assigned Numbers Authority (IANA) para uso pelo daemon e cliente Docker: **porta TCP 2375** para tráfego não criptografado, **porta 2376** para conexões SSL criptografadas e **porta 2377** para modo Docker Swarm.  A configuração padrão para o instalador do Docker é usar apenas um soquete Unix para comunicação com o daemon Docker local. Isso garante que o sistema tenha como padrão a instalação mais segura possível. Isso também é facilmente configurável, mas é altamente recomendável que as portas de rede não sejam usadas com o Docker, devido à falta de autenticação do usuário e controles de acesso baseados em funções no daemon do Docker. O soquete Unix pode estar localizado em caminhos diferentes em sistemas operacionais diferentes, mas na maioria dos casos, pode ser encontrado em: /var/run/docker.sock . Se você tiver fortes preferências por um local diferente, geralmente poderá especificar isso no momento da instalação ou simplesmente alterar a configuração do servidor posteriormente e reiniciar o daemon.

obs: **Daemon do Docker** é o processo em segundo plano que gerencia a execução dos containers Docker. Ele é responsável por receber e executar comandos do usuário, monitorar o estado dos containers, gerenciar os recursos do sistema e garantir a segurança e isolamento entre os containers. Em resumo, o daemon do Docker é o componente central que permite a criação, execução e gerenciamento dos containers Docker.

### API Docker Engine
O daemon Docker possui uma API, ou seja uma ferramenta de linha de comando do Docker usa para se comunicar com o daemon. A maioria das coisas que você pode fazer com as ferramentas de linha de comando do Docker são suportadas de forma relativamente fácil por meio da API. Duas exceções notáveis ​​são os endpoints que exigem streaming ou acesso ao terminal.

### Rede de contêineres
A maioria das pessoas executa seus contêineres na configuração padrão, chamada **modo bridge**. Para entender o modo bridge, é mais fácil pensar em cada um dos seus contêineres Linux como se comportando como um host em uma rede privada. O servidor Docker atua como uma ponte virtual e os contêineres são clientes por trás dele.

[Docker0] é uma interface de rede virtual criada pelo Docker para permitir a comunicação entre contêineres e a rede externa. Ela é usada para encaminhar o tráfego de rede entre os contêineres e a rede host, permitindo que os contêineres se comuniquem com outros contêineres e com a rede externa. Isso significa que, por padrão, todos os contêineres estão juntos em uma rede e podem se comunicar diretamente entre si. Mas para chegar ao host ou ao mundo externo, eles passam pela docker0interface da ponte virtual. Vale destacar que é possivel pode desligar toda a camada de rede virtual de um contêiner usando a [--net=host].

### O fluxo de trabalho do Docker
#### Controle de revisão
o Docker oferece duas formas de controle de revisão. Um deles é usado para rastrear as camadas do sistema de arquivos que compõem cada imagem Docker e o outro é um sistema de marcação para essas imagens.

- **Camadas do sistema de arquivos**: compostos de camadas empilhadas de sistemas de arquivos, cada um identificado por um hash exclusivo, onde cada novo conjunto de alterações feitas durante o processo de construção é colocado sobre as alterações anteriores.

- **Tags de imagem**: o Docker oferece um mecanismo de marcação de imagem que facilita a identificação da versão anterior do aplicativo implantado. Em ambientes de produção escalonados, cada aplicativo lida de forma única com as revisões de implantação, o que pode gerar confusão. Com o Docker, é possível deixar várias revisões do aplicativo no servidor, tornando a reversão trivial. Isso simplifica a comunicação entre equipes e padroniza as expectativas em relação à marcação das aplicações, tornando as ferramentas de implantação mais simples.
**obs**:  A tag "latest" para imagens de contêiner é útil para exemplos iniciais, mas não é recomendada para produção devido à possibilidade de atualizações e dificuldade de reversão. É aconselhável marcar compilações de CI/CD com identificadores exclusivos do commit do código-fonte e utilizar versionamento semântico, como 1.4.3, 2.0.0, ao lançar imagens.

- **Build**: a ferramenta de linha de comando do Docker utiliza um Dockerfile para produzir uma imagem Docker, onde cada comando gera uma nova camada na imagem. A padronização do Dockerfile permite que engenheiros modifiquem a construção de aplicativos de forma consistente. Além disso, o Dockerfile é verificado em um sistema de controle de revisão, simplificando o rastreamento de alterações na compilação. As compilações modernas do Docker de vários estágios permitem definir o ambiente de compilação separadamente da imagem final do artefato, proporcionando uma grande capacidade de configuração para o ambiente de construção.
Muitas compilações do Docker geram um único artefato, a imagem do contêiner, e a lógica de construção está contida no Dockerfile. Empresas como eBay padronizaram contêineres Linux para construir imagens a partir do Dockerfile. Ofertas de construção SaaS como Travis CI e CodeShip têm suporte para construções Docker. Além disso, o suporte mais recente do **BuildKit** no Docker permite a criação automatizada de várias imagens que suportam diferentes arquiteturas de computação.

- **Teste**: Os contêineres Linux oferecem vantagens para os testes, apesar de o Docker em si não incluir uma estrutura integrada para testes. O Docker facilita testes mais confiáveis, garantindo que o artefato testado seja o mesmo enviado para produção. Os contêineres incluem todas as dependências, tornando os testes executados neles muito confiáveis. 
Os contêineres incluem todas as dependências, tornando os testes executados neles muito confiáveis.
Isso permite testar toda a pilha antes de ser enviada para produção, o que não é fácil com a maioria das outras tecnologias. Além disso, os contêineres Linux podem ser uma verdadeira tábua de salvação para desenvolvedores que precisam lidar com chamadas de API entre microsserviços.
As organizações que executam contêineres Linux em produção podem realizar testes de integração automatizada para extrair um conjunto versionado de contêineres Linux para diferentes serviços, correspondendo às versões implantadas atualmente. Isso se torna simples de implementação devido à padronização fornecida pelos contêineres Linux.

- **Compilação**: As compilações do Docker resultam em uma imagem única e portátil que pode ser tratada como um único artefato de construção, independentemente do idioma do aplicativo ou da distribuição do Linux. Isso facilita a reutilização de ferramentas entre aplicativos e torna os aplicativos muito portáteis, podendo ser facilmente implantados em qualquer sistema com um servidor Docker em execução na mesma arquitetura.

- **Implatação**: O Docker simplifica as implantações, oferecendo suporte a uma estratégia de implantação simples e de uma linha para colocar uma compilação em um host e colocá-la em funcionamento, mas há uma grande variedade de ferramentas disponíveis que facilitam a implantação em um cluster de Docker ou outros hosts de contêiner Linux compatíveis. . Além disso, a padronização do Docker permite que as compilações sejam implantadas em qualquer sistema compatível, com baixa complexidade para as equipes de desenvolvimento.

- **O ecossistema Docker**: Uma ampla comunidade formada em torno do Docker, facilitando as melhores ferramentas para aplicar código a problemas operacionais. Onde há lacunas nas ferramentas fornecidas pelo Docker, outras empresas e indivíduos assumem a responsabilidade, muitas delas também de código aberto. A empresa comercial Docker contribuiu com grande parte do código-fonte principal para a comunidade de código aberto e encorajou outras empresas a contribuir de volta para os esforços de código aberto.
    - **Orquestração**: a primeira categoria de ferramentas que ampliam as funcionalidades do Docker e da experiência de contêineres Linux, focando em ferramentas de orquestração e implantação em massa. As primeiras ferramentas, como Centurion, Helios e Ansible Docker, funcionam como implantação tradicional, mas aproveitam o contêiner como artefato de distribuição, proporcionando benefícios do Docker com simplicidade. Entretanto, muitas foram substituídas por opções mais robustas como Kubernetes. Opções mais avançadas incluem agendadores automáticos como Kubernetes e Apache Mesos com o agendador Marathon, que assumem controle quase total sobre um conjunto de hosts. Outras opções comerciais, como Nomad, DC/OS e Rancher, também estão disponíveis, e o ecossistema de opções gratuitas e comerciais continua a crescer rapidamente;

    - **Hosts atômicos imutáveis**: uma ideia para aprimorar a experiência no Docker são os hosts atômicos imutáveis, que permitem baixar uma nova imagem fina do sistema operacional e reinicializar o servidor, revertendo facilmente em caso de problemas. Isso é aplicado em distribuições de host atômico baseadas em Linux, como Fedora CoreOS da Red Hat e Bottlerocket OS, proporcionando alta consistência e resiliência para toda a pilha de software. As características incluem uma pegada mínima, suporte a contêineres Linux e Docker, e atualizações e reversões atômicas do sistema operacional controladas por ferramentas de orquestração multihost.

    - **Ferramentas adicionais**: o Docker possui um amplo ecossistema de ferramentas que oferecem funcionalidades adicionais, como monitoramento com Prometheus e orquestração com Ansible, que aproveitam a API Docker. Além disso, existem plug-ins que atendem a uma especificação para interagir com o Docker.


### Ubunto
    ```
        docker container run --rm -it docker.io/ubuntu:latest bash

    ```

### Fedora 
    ```
        docker run --rm -it docker.io/fedora:latest bash

    ```

### Alpine Linux 
    ```
        docker container run --rm -it docker.io/alpine:latest bash
        
    ```
-  Caso a imagem já exista:
    ```
        docker  run --rm -it alpine:latest 
    ```

[docker.io/ubuntu:latest], [docker.io/fedora:latest] e [docker.io/alpine:latest] todos representam um repositório de imagens Docker, seguido por um nome de imagem e uma tag de imagem.


### Servidor Docker
Para executar o daemon Docker manualmente em um sistema Linux:
```
    sudo  dockerd  -H  unix:///var/run/docker.sock  \ --config-file  /etc/docker/daemon.json
```
É importante observar que esta seção pressupõe que  está no servidor Linux real ou na VM que está executando o daemon Docker. Se estiver usando o Docker Desktop no Windows ou Mac, não será capaz de interagir facilmente com o executável dockerd, pois ele está intencionalmente oculto do usuário final.
Este comando inicia o daemon Docker, cria e escuta um soquete de domínio Unix (-H unix:///var/run/docker.sock) e lê o restante da configuração de /etc/docker/daemon.json.

No docker desktop podemos editar as configuração do daemon do Docker em ** Preferências… → Docker Engine**.

### Imagens Docker 
 As imagens Docker ou Open Container Initiative (OCI) fornecem a base para tudo o que  implantará e executará com o Docker. Para iniciar um contêiner, deve baixar uma imagem pública ou criar a sua própria. É possivel  pensar na imagem como um único ativo que representa principalmente o sistema de arquivos do contêiner. No entanto, na realidade, cada imagem consiste em uma ou mais camadas de sistema de arquivos vinculadas que geralmente possuem um mapeamento direto um para um para cada etapa de construção usada para criar aquela imagem.
 Dockerfile é um arquivo descreve todas as etapas necessárias para criar uma imagem e geralmente está contido no diretório raiz do repositório de código-fonte do aplicativo.

#### Folder docker-node-hello
 Um Dockerfile típico pode ser parecido com o mostrado aqui, que cria um contêiner para um aplicativo baseado em Node.js:
 ```
    FROM node:18.13.0

    ARG email="anna@example.com"
    LABEL "maintainer"=$email
    LABEL "rating"="Five Stars" "class"="First Class"

    USER root

    ENV AP /data/app
    ENV SCPATH /etc/supervisor/conf.d

    RUN apt-get -y update

    # The daemons
    RUN apt-get -y install supervisor
    RUN mkdir -p /var/log/supervisor

    # Supervisor Configuration
    COPY ./supervisord/conf.d/* $SCPATH/

    # Application Code
    COPY *.js* $AP/

    WORKDIR $AP

    RUN npm install

    CMD ["supervisord", "-n"]
    ```

Cada linha em um Dockerfile cria uma nova camada de imagem que é armazenada pelo Docker. Esta camada contém todas as alterações resultantes da emissão desse comando. Isso significa que quando você cria novas imagens, o Docker só precisará construir camadas que se desviem das construções anteriores: você pode reutilizar todas as camadas que não foram alteradas.
No Docker Hub é posivél obter imagens oficiais.
A imagem base a seguir fornecerá uma imagem do Ubuntu Linux executando o Node 18.13.0
    ```
    FROM docker.io/node:18.13.0
    ```

O parâmetro **ARG** fornece uma maneira de definir variáveis ​​e seus valores padrão, que estão disponíveis apenas durante o processo de construção da imagem:
    ```ARG email="email@example.com````

A aplicação de rótulos **LABEL** a imagens e contêineres permite adicionar metadados por meio de pares chave/valor. Que podem ser usados ​​posteriormente para pesquisar e identificar imagens e contêineres do Docker.  Para ver os rótulos aplicados a qualquer imagem usando o comando **[docker image inspect]**.
    ```
    LABEL "maintainer"=$email
    LABEL "rating"="Five Stars" "class"="First Class"
    ```
O Docker executa todos os processos como root dentro do contêiner, mas você pode usar a instrução **USER** para alterar isso:
    ```
    USER root
    ```

Ao contrário da instrução **ARG**, a instrução **ENV** permite definir variáveis ​​de shell que podem ser usadas pela aplicação em execução para configuração, além de estarem disponíveis durante o processo de construção. As instruções ENV e ARG podem ser usadas para simplificar o Dockerfile e ajudar a mantê-lo mais SECO (Não se repita):
    ```
    ENV AP /data/app
    ENV SCPATH /etc/supervisor/conf.d
    ```
No código a seguir, você usará uma coleção de instruções **RUN** dependências: para iniciar e criar a estrutura de arquivos necessária e instalar alguns softwares necessários.
    ```
    RUN apt-get -y update

    # The daemons
    RUN apt-get -y install supervisor
    RUN mkdir -p /var/log/supervisor
    ```
Embora estejamos demonstrando isso aqui para simplificar, não é recomendado que você execute comandos como apt-get -y update ou dnf -y update no do seu aplicativo.

A instrução **COPY** é usada para copiar arquivos do sistema de arquivos local para sua imagem. Na maioria das vezes, isso incluirá o código do seu aplicativo e quaisquer arquivos de suporte necessários. Como COPY copia os arquivos na imagem, você não precisa mais acessar o sistema de arquivos local para acessá-los depois que a imagem for construída. Você também começará a usar as variáveis ​​de construção definidas na seção anterior para poupar um pouco de trabalho e ajudar a protegê-lo contra erros de digitação:
    ```
    # Supervisor Configuration
    COPY ./supervisord/conf.d/* $SCPATH/

    # Application Code
    COPY *.js* $AP/
    ```

Lembre-se de que cada instrução cria uma nova camada de imagem Docker, por isso geralmente faz sentido combinar alguns comandos agrupados logicamente em uma única linha. É ainda possível usar a instrução COPY em combinação com a instrução RUN para copiar um script complexo para sua imagem e então executar esse script com apenas dois comandos no Dockerfile.

Com a instrução **WORKDIR** as instruções de compilação restantes, você altera o diretório de trabalho na imagem para  o processo padrão que é iniciado com quaisquer contêineres resultantes.
    ```
    WORKDIR $AP

    RUN npm install
    ```
A ordem dos comandos em um Dockerfile pode ter um impacto muito significativo nos tempos de construção contínuos. Você deve tentar ordenar os comandos para que as coisas que mudam entre cada construção fiquem mais próximas do final. Isso significa que a adição de seu código e etapas semelhantes devem ser adiadas até o final. Ao reconstruir uma imagem, cada camada após a primeira alteração introduzida precisará ser reconstruída.

E finalmente, você termina com a instrução **CMD**, que define o comando que inicia o processo que você deseja executar dentro do contêiner:
    ```
    CMD ["supervisord", "-n"]
    ```
Embora não seja uma regra rígida, geralmente é considerada uma prática recomendada tentar executar apenas um único processo dentro de um contêiner. A ideia central é que um contêiner forneça uma única função para que seja fácil dimensionar horizontalmente funções individuais dentro de sua arquitetura.

O arquivo .dockerignore permite definir arquivos e diretórios que você não deseja enviar para o host Docker ao construir a imagem. Neste caso, o arquivo .dockerignore contém a seguinte linha:[.git]

O diretório supervisord contém os arquivos de configuração do supervisord que você usará para iniciar e monitorar o aplicativo.
Usar supervisord neste exemplo para monitorar o aplicativo é um exagero, mas seu objetivo é fornecer alguns insights sobre algumas das técnicas que você pode usar em um contêiner para fornecer mais controle sobre seu aplicativo e sua execução estado.

No final do comando build, você notará um ponto final. Isso se refere ao contexto de construção, que informa ao Docker quais arquivos ele deve enviar para o servidor para que possa construir nossa imagem. Em muitos casos, você simplesmente verá um . no final de um comando build, já que um único ponto representa o diretório atual. Este contexto de construção é o que o arquivo .dockerignore está filtrando para que não carreguemos mais do que o necessário.

O Docker assume que o Dockerfile está no diretório atual, mas se não estiver, você pode apontar diretamente para ele usando o argumento -f.

    ```
        docker image build -t example/docker-node-hello:latest .
    ```

Usar **[docker image build]** é funcionalmente igual a usar **[docker build]**.

- **obs**:  A **flag -t** no comando **docker image build** é usada para **especificar um nome e uma tag para a imagem Docker** que está sendo construída. A flag -t significa "tag" e é seguida pelo nome da imagem e pela tag.

    ```
        docker container run --rm -d -p 8080:8080 example/docker-node-hello:latest
    ```

Este comando diz ao Docker para criar um contêiner em execução em segundo plano a partir da imagem com a tag example/docker-node-hello:latest e, em seguida, mapear a porta 8080 no contêiner para a porta 8080 no host Docker . Se tudo correr conforme o esperado, o novo aplicativo Node.js deverá estar sendo executado em um contêiner no host. Para verificar 

    ```
        docker container ls // ou  docker ps
    ```

O **[docker container ls]** é usado para listar os containers Docker em execução no seu sistema. No entanto, o comando mais moderno e recomendado é **[docker ps]**, que é equivalente ao docker container ls. Ambos os comandos fazem a mesma coisa.


    ```
        docker container ls -a // ou  docker ps -a
    ```
Esse comando exibirá todos os containers, independentemente do seu estado (em execução, parado, etc.).

Geralmente, você pode determinar o endereço IP do host Docker examinando a entrada da **[docker context list]** que está marcada com um asterisco ou verificando o valor da variável de ambiente DOCKER_HOST se isso acontecer ser definido. Se o DOCKER ENDPOINT estiver configurado para um soquete Unix, então o endereço IP provavelmente será 127.0.0.1:

Obtenha o endereço IP e digite algo como http://127.0.0.1:8080/ (ou seu endereço Docker remoto, se for diferente) na barra de endereços do seu navegador ou use uma linha de comando ferramenta como curl. Você deverá ver o seguinte texto:

- Hello World. Wish you were here.

Se quiséssemos alterar o rótulo maintainer, poderíamos simplesmente executar novamente a compilação e fornecer um novo valor para o emailARG através do [--build-arg] argumento de linha de comando, assim:
    ```
        docker image build --build-arg email="me@example.com" -t example/docker-node-hello:latest .
    ```
Após a conclusão da construção, podemos verificar os resultados reinspecionando a nova imagem:
    ```
    docker image inspect example/docker-node-hello:latest | grep maintainer 
    ```

- docker container ls (para pegar o id do container)

    ```
        docker container stop [id]
    ```

- Usar **[docker container ls]** é funcionalmente equivalente a usar **[docker container list]**, **[docker container ps]** ou **[docker ps]**
- Usar **[docker container stop]** também é funcionalmente equivalente a usar **[docker stop]**.

#### Variáveis ​​de ambiente como configuração

Podemos reiniciar o container após adicionar uma única instância do argumento --env ao comando docker container run anterior


    ```
        docker run --rm -d --publish mode=ingress,published=8080,target=8080 --env WHO="Angelo Souza" example/docker-node-hello:latest
    ```
E a menssagem será: **Hello Angelo Souza. Wish you were here.**

Você pode encurtar o comando docker anterior para o seguinte, se desejar:

    ```
        docker run --rm -d -p 8080:8080 --env WHO="Angelo Souza" example/docker-node-hello:latest
    ```

O Alpine Linux é altamente otimizado para espaço, razão pela qual ele vem com /bin/sh em vez de /bin/bash, por padrão. No entanto, você também pode instalar glibc e bash no Alpine Linux se precisar, e isso geralmente é feito no caso de contêineres JVM.