# docker
*** Containers ***

docker ps mostra os containers ativos
docker ps -a mostra os containers existentes 
docker stop <ID> ou <NAME> parar container
docker start <ID> ou <NAME> iniciar container
docker run --name <NAME NOVO> <IMAGEM>
docker logs <ID> ou <NAME> verificar logs do coontainer
docker rm <ID> ou <NAME> -f remover container
docker run <imagem> roda a imagem docker e caso não exista ele baixa a imagem
docker run -it <imagem> roda de forma interativo, habilita o terminal da aplicação contida na imagem
docker start -it <Container> Inicia e roda de forma interativo, habilita o terminal da aplicação contida na imagem
docker run -d <imagem> "detached" roda o container sem ocupar o terminal
docker run -d -p 8085:8085 nginx Configurar porta que a imagem vai utilizar

*** Imagens ***
Docker file : Arquivo que cria/configura as imagens
docker image ls : verifica as imagens criadas
docker pull <imagem> : baixa uma imagem do docker e deixa em seu repositorio para uso
docker flag=(ex: run) --help : verificar opções de comandos disponiveis


--para criar uma imagem precisa ter um arquivo Docker file em uma pasta onde ficará o projeto

-- instruções para criar o arquivo
FROM: Informa que vem de uma imagem base
WORKDIR: Onde fica o diretório da Aplicação
EXPOSE: porta da aplicação
COPY: quais arquivos serão copiados para a imagem

-- Executar o arquivo
1) Precisa fazer o build
docker build <diretorio da imagem> pode se usar o . se já estiver no diretorio pelo terminal
2) Executar a imagem
docker run <imagem>

-- Nomear imagens
docker tag <ID> <NOME>
docker tag <NOME>:<TAG>


-- Nomear imagens na BUILD
docker build -t meunode_junior .

--Remover imagens
docker rmi <imagem>
docker rmi <imagem> -f

-- Limpeza geral
docker system prune


-- Remoção após parar o container
docker run --rm <container>

-- copia de arquivos
docker cp <container>:/origem/arquivo.js ./destino/

-- verificar analise e Processamento do container
docker top <container>
docker inspect
docker stats -> verifica processamento

-- Enviar imagens para o hub do docker
docker login
docker logout
docker push <NOME IMAGEM> 
docker push juniorbarros81/aulateste:tagname
docker pull 

-- Atualizando imagem
1) Fazer build com tag diferente
docker build -t juniorbarros81/aulateste:novaversao .
2) Fazer push da nova imagem
docker push juniorbarros81/aulateste:novaversao

*** Volumes ***

Maneira de persistir dados(manter dados) em aplicações e não depender dos containers pra isso

-- Tipos de Volumes --
Anônimos (flag -v) cria com nomes aleatórios
docker run -v /data
Nomeados ()
docker run volume_nomeado:/diretorio
Binds mounts () salvar o arquivo do volume no PC local fora do docker - atualiza o projeto em tempo real
docker run /dir/data:/data


docker volume ls -> verifica todos os volumes

docker run -d -p 8090:80 --name php_messages_container3 --rm -v volume_nomeado:/var/www/html/messages phpmessages
                portapc:portaimagem

docker run -d -p 8095:80 --name php_messages_container -v C:\Users\Junior\Documents\jr\docker\docker-php\messages:/var/www/html/messages --rm phpmessages

-- Criar volumes manualmente
docker volume create <nome>

-- Listar Volumes
docker volume ls

-- Checar um volume
docker volume inspect <nome>

-- Remover volumes
docker volume rm <nome>

-- Remover volumes não utilizados
docker volume prune

-- Criar volumes apenas consulta/Leitura ready/only
docker run -v volume:data:ro


*** Network ***

(Tipos de Conexão):

-- Externa (API ou Servidor)

-- Com o Host (Máquina que está executando o Docker)

-- Entre Containers (Utiliza o driver bridge permitindo a comunicação entre 2 ou mais containers)

(Tipos de Drivers):

-- Bridge -> é o driver padrão
   (Usado quando 2 containers precisam se conectar)
-- Host
   (Usado para conectar Container com o Host) 
-- macvlan
    (Usado para conexão através do MAC ADDRESS)
-- none
    (Usado para remover todas as conexões de rede do Container)
-- plugins
    (Permite extensões para simular/criar outras redes)

(Comandos):

-- Listar Redes
docker network ls

-- Criar Redes
docker network create <nome>
docker network create -d <driver> <nome>

-- Remover Redes
docker network rm <nome>
docker network prune //remove redes em massa

-- Conectar Container em uma rede
docker network connect <rede> <container>

-- Desconectar Container de uma rede
docker network disconnect <rede> <container>

-- Inspecionar Redes
network inspect <nome>


*** YAML ***

-- Linguagem utilizada para configuração, usada para configurar o Docker composer (.yml ou .yaml)


*** Docker Compose ***

-- Ferramenta para rodar múltiplos containers (múltiplos builds e runs) essencial para projetos maiores.

-- https://docs.docker.com/compose/install/ -> para instalar no Linux

(Comandos):

-- Rodando o Compose
docker compose up
CTRL+C -> fecha o Docker Composer
docker compose up -d
docker compose down -> para o serviço do docker compose que está rodando em background

*** Docker Swarm ***

-- Conceitos fundamentais

** Nodes: Instância(máquina) que participa do Swarm

** Manager Node: Node que gerencia os demais Nodes

** Worker Node: Nodes que trabalham em função do Manager

** Service: Um conjunto de Tasks que o Manager Node manda para o Work Node executar

** Task: Comandos que são executados nos Nodes

(Comandos):

docker swarm init  -> inicia serviço do swarm no docker e adiciona máquina com Node Manager
-- caso dê erro usar o comando abaixo
docker swarm init --advertise-addr <ip do host>

docker swarm leave -f -> forçar saída do swarm mesmo se for manager

docker node ls -> Listar Nodes(Máquinas) ativos
docker node rm <ID> -> remove Node do swarm


docker service ls -> Listar serviços ativos
docker service rm <nome ou ID> -> Remover serviços 

docker swarm join --token <token> <IP>:<porta> -> Adiciona nova máquina como Worker Node
EX: docker swarm join --token SWMTKN-1-57m2tmr4m6omjjlp4jvuz109b3xd7omuqgebedoljzk0a110bv-3byo0pgxndxqqnprjbd2vm5hf 192.168.0.8:2377

docker swarm join-token manager -> Verificar ou Recuperar token do manager

docker service create --name <nome> <imagem> -> Subir serviços no Swarm

EX: docker service create --name nginxswarm -p 80:80 nginx

Docker service create --name <name> --replicas <numero> <imagem> -> Subir serviços no Swarm com réplicas

EX: service create --name nginxswarm --replicas 3 -p 80:80 nginx

docker service inspect <ID> -> Verificar informações sobre o serviço

docker service ps <ID> -> Verifica quais containers estão seno ou já foram utilizados

---
docker stack deploy -c <ARQUIVO.YAML> <NOME> -> executa o arquivo do docker-compose

docker service scale <NOME>=<QTD REPLICAS> -> Criar Replcias do Work Nodes
---

docker node update --availability drain <ID> -> Não receber mais Tasks'ordens' do Manager
docker node update --availability active <ID>

docker service update --image <IMAGEM> <SERVICO> -> Atualiza status do NOde
EX: docker service update --image nginx:1.18.0 pdq

docker network create --driver overlay swarm -> criar rede no swarm
EX: usar comando --network no create para adicionar rede ao service

docker service update --network <REDE> <NOME> -> conectar um serviço em uma rede com o update
EX: docker service update --network-add <rede> <ID>  


*** Kubernetes ***

** Control Plane: Onde é gerenciado o controle dos processos dos Nodes ou seja é a Máquian central
** Nodes: Máquinas que são gerenciadas pelo Control Plane
** Deployment: A execução de uma imagem/projeto em um Pod
** Pod: um ou mais containers que estão em um Node
** Services: Serviços que expõe os Pods ao mundo externo
** kubectl: Cliente de linha de comando kubernetes

-- Recursos para utilizar Kubernetes
** Instalar Chocolatey
    -- Windows
       Acessar https://chocolatey.org/ -> getstarted - verificar passo a passo do site
       Abrir Power Shel como Administrador
** Documentação de instalação do client Kubernetes
    -- Windows
        https://kubernetes.io/docs/tasks/tools/
** Virtualbox, Hyper-V ou o Docker
** Instalar Minikube
        https://minikube.sigs.k8s.io/docs/start/ or choco install minikube

(Comandos)
minikube start --driver=docker -> Inicia minikube driver pode ser vitualbox,hyperv e docker
minikube stop -> Para o minikube
minikube status -> verifica o status
minikube dashboard -> visualiza dashboard do minikube
minikube dashboard --url -> visualizar a url da dashboard

-- Deployment -> Cria serviços que vão rodar nos Pods
