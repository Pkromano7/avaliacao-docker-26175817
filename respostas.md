# Respostas · Avaliação Prática de Docker · ViaSerra Transportes (Turma C)

Nome:Patrick Romano Silva
Matrícula:26175817
Usuário do GitHub:Pkromano7
Usuário do Docker Hub:patrickromano

Responda com as suas palavras e com o que aconteceu na SUA máquina. Resposta curta e certa vale mais
do que texto longo copiado. Resposta que contradiz o seu próprio Dockerfile vale zero.

## Parte 1 · Dockerfile do portal

1. Qual imagem base você usou e qual o tamanho final da imagem do portal (saída de `docker images`)?
R: nginx:1.27-alpine e 73.6MB em "Disk usage" e 21MB em "Content size" (patrickromano/viaserra-portal:1.0-26175817)

2. Em qual pasta do container o Nginx procura os arquivos do site? Mostre o comando que você usou para
conferir que o `index.html` está lá dentro.
R: Essa é a pasta: /usr/share/nginx/html - Esse é o comando que eu usei para conferir: docker exec teste-portal ls /usr/share/nginx/html
Isso foi oque apareceu depois de conferir: 50x.html, estilo.css e index.html


## Parte 2 · Docker Hub

3. Nome completo da imagem publicada e link público do repositório no Docker Hub.
R: patrickromano/viaserra-portal:1.0-26175817 e o link: https://hub.docker.com/r/patrickromano/viaserra-portal

4. Se você mudar o HTML, quais comandos precisa rodar para que a versão nova chegue ao Docker Hub?
R: Editar o portal/html/index.html e salvar junto com reconstruir a imagem:    docker build -t patrickromano/viaserra-portal:1.0-26175817 ./portal. Depois enviar para o Docker Hub:    docker push patrickromano/viaserra-portal:1.0-26175817


## Parte 3 · Página de manutenção

5. Preencha uma linha por defeito encontrado. Defeito inexistente listado aqui desconta pontos.

|#|Instrução|O que estava errado|O que você viu acontecer|Como corrigiu|
|-|-|-|-|-|
|1|COPY pagina/ .|a pasta de origem não existe, a pasta real se chama site|o build falhou com "/pagina": not found e a imagem não foi criada. O docker run seguinte também falhou com pull access denied, porque a imagem não existia|Apenas troquei pagina/ por site/|
|2|CMD \["nginx"]|o Nginx subia em segundo plano, então o processo principal do container terminava|o container apareceu Up por um instante e depois Exited (0), e o docker logs não mostrou nenhum erro|Digitei esse comando: CMD \["nginx", "-g", "daemon off;"]|
|3|WORKDIR /usr/share/nginx, junto com o COPY que copiava para a pasta atual (.)|o arquivo foi copiado para /usr/share/nginx, mas o Nginx serve os arquivos de /usr/share/nginx/html|o container ficou de pé, mas o navegador mostrou "Welcome to nginx!". O docker exec mostrou o seu index.html solto em /usr/share/nginx, e o padrão do Nginx dentro de html|mudei o destino do COPY para /usr/share/nginx/html/|

6. Qual a diferença entre `-p 7042:80` e `-p 80:7042` no `docker run`? Qual dos dois números é a porta do container?
R: A regra é o formato é sempre -p porta-do-host:porta-do-container, o número da esquerda é a porta do seu computador, a que você digita no navegador. Já o da direita é a porta de dentro do container, onde o Nginx está escutando.
no -p 7042:80 o navegador entra por localhost:7042 e chega no Nginx (porta 80 do container), e no -p 80:7042 o Docker mandaria para a porta 7042 do container, onde o Nginx não escuta, então o site não vai rodar.
No formato -p host:container, o da esquerda é a porta do seu computador e o da direita é a porta de dentro do container, assim como no exemplo da pergunta (-p 7042:80), a porta do container é a 80.

## Parte 4 · Primeiro docker-compose

7. Escreva os dois comandos `docker run` que fariam o mesmo que o seu `docker-compose.yml`.
R: Portal. O compose usa a imagem publicada na porta 8017: docker run -d --name portal -p 8017:80 patrickromano/viaserra-portal:1.0-26175817
Manutenção. O compose tem build: ./manutencao,ele constroi a imagem a partir da pasta e depois sobe, porem sem o compose são dois passos, o docker build e o docker run
o docker build da pasta ./manutencao com o nome manutencao:26175817
o docker run dessa imagem, na porta 7017

8. Qual comando derruba os dois containers de uma vez?
R: o comando: docker compose down


## Verificador

9. Código de conclusão impresso pelo verificador:

```
VIASERRA-26175817-A48CA935
```

