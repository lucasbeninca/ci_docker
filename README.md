# **CI_Docker**
## **DevOps: construindo e gerindo containers com o Docker**

* Comando para não precisar usar su ao trabalhar com docker `sudo usermod -aG docker $USER`

*  `docker exec -it <ubuntu_container> bash`: abre o bash no container.

* `docker run -d --name ubuntu_alura ubuntu sleep infinity`: criar um container ubuntu com tempo de execução infinito.

* `docker pause / docker unpause`: pausa e despausa um container.

* `docker rm <id_container>`: remove o container mas não a imagem.

### Como acessar Externamente um Container:

* Para acessar um container externamente precisamos especificar na criação do mesmo a interface de exposição externa.

* `docker run -d -P docker/example-voting-app-vote` faz o mapeamento de portas de forma automática.

* `docker run -d -p 3000:80 dcoker/example-voting-app-vote` especifica de forma manual.

* `docker stop $(docker container ls -q)` remove/para todos os containers em execução 

* `docker history <id> ou docker inspect <id>` para inspecionar as imagens.

## Criando imagem no docker:
```
FROM node:20
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
ENTRYPOINT npm start
```
1. Salvar ele como `Dockerfile`
2. Rodar `docker build -t <nome-da-imagem:version> . exemplo: docker build -t allbooks:1.0.0 .`

* ***ARTIGO: desvendando o [DockerFile](https://pages.github.com/).***

* ` docker tag <nome_da_imagem> <novo_nome>` 

## Persistindo dados
1. Usando **bind.**

* `docker run -it --mount type=bind,source=/home/ponto/Documents/projects/ci_docker/volume_docker,target=/app ubuntu bash` 
Explicando o comando: `-it para criar o container  de forma interativa type=bind especifica o tipo de persistencia, source= <o caminha da do diretório na minha maquina>, target= < pasta no conatiner que se liga a pasta na minha maquina>.`

2. Usando **Volumes.**
* `docker volume create <nome-do-volume>` cria um volume docker para persistencia de dados.
No caso de volumes para verificarmos os dados dentro de um volume precisamos criar um novo container.
* Exemplo: `docker run -it --mount source=novo-volume,target=/app ubuntu bash`
* `docker volume inspect <nome-do-volume` para ver as informações do volume.

3. Usando **TMPFS** (Funciona melhor no Linux).
* `docker run -it --tmpfs=/app ubuntu bash` é interesante usar tmpfs a fim de proteger dados sensiveis 

Exemplo de utilização:
* `docker run -it -v meu-volume-mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=sua_senha mysql` cria um volume para o banco mysql dentro do container