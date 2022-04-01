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


