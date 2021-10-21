---
title: Instalando o GitLab via Docker e criando um repositório
subtitle:
image: assets/img/portfolio/05-full.jpg
alt:

caption:
  title: Instalando o GitLab via Docker e criando um repositório
  subtitle:
  thumbnail: assets/img/portfolio/05-thumbnail.jpg
---
# Instalando o Docker

O capítulo 8 do livro aborda sobre o versionamento da documentação, configuração e do código desenvolvido em Puppet.

Para ajudar a praticar o versionamento, este post foi criado para mostrar a instalação do GitLab via Docker.

Se você não sabe o que é Docker, recomendo começar lendo os links a seguir. É um longo caminho, mas vale a pena conhecer essa tecnologia.

* http://blog.aeciopires.com/palestra-transportando-as-aplicacoes-entre-varios-ambientes-com-docker/
* http://blog.aeciopires.com/primeiros-passos-com-docker/

O Docker pode ser instalado no CentOS, Debian, Red Hat e Ubuntu seguindo as instruções dos respectivos tutoriais:

* https://docs.docker.com/engine/installation/linux/docker-ce/centos/
* https://docs.docker.com/engine/installation/linux/docker-ce/debian/
* https://docs.docker.com/engine/installation/linux/docker-ee/rhel/
* https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/

Para instalá-lo em outras distribuições GNU/Linux acesse o link abaixo.

* https://docs.docker.com/engine/installation/

# Instalando o GitLab

Na página https://docs.gitlab.com/omnibus/docker/README.html tem várias informações a cerca das imagens [Docker](http://docker.com) oficiais do [GitLab](https://gitlab.com) (ferramenta web para versionamento usando o Git).

Neste tutorial, o Gitlab será executado usando um único conteiner para fornecer todo o ambiente necessário para executá-lo, o que deixa a instalação bem simples e rapidamente você tem o serviço funcionando.

O GitLab requer um host com, pelo menos, 4 GB de memória RAM. A lista completa de requisitos de hardware e software está na página: https://docs.gitlab.com/ce/install/requirements.html#hardware-requirements.

Para não consumir os recursos de memória da sua VM, use o serviço **Playground Docker** (http://labs.play-with-docker.com), que fornece gratuitamente até 4 horas e 5 Docker hosts para brincar à vontade. E o melhor, ele já cria as instâncias com o Docker instalado e pronto para ser usado.

1) Acesse a URL http://labs.play-with-docker.com e crie uma nova sessão. Em seguida, clique no botão **+ ADD NEW INSTANCE**. No console de comandos da nova instância, execute os comandos a seguir.

2) Crie o diretório de dados, log e configuração do GitLab.

```bash
mkdir -p /opt/docker/gitlab/config
mkdir -p /opt/docker/gitlab/logs
mkdir -p /opt/docker/gitlab/data
```

3) Baixe a última versão da imagem docker do GitLab.

```bash
docker pull gitlab/gitlab-ce:latest
```

4) Inicie o conteiner docker do GitLab.

```bash
docker run --detach \
--hostname git.domain.com.br \
--publish 8843:443 \
--publish 8880:80 \
--publish 2022:22 \
--name gitlab \
--restart always \
--volume /opt/docker/gitlab/config:/etc/gitlab \
--volume /opt/docker/gitlab/logs/:/var/log/gitlab \
--volume /opt/docker/gitlab/data/:/var/opt/gitlab \
gitlab/gitlab-ce:latest
```

5) O log pode ser visualizado com o comando abaixo.

```bash
docker logs gitlab
```

O serviço pode demorar até 5 minutos para ser iniciado e configurado da primeira vez. Aguarde até tudo estar pronto.

6) Quando estiver pronto, acesse o GitLab na URL a ser disponibilizada automaticamente após clicar no link 8880 mostrado no topo da página, um pouco mais a direita na linha que é exibido o IP interno da instância. Ao acessar, será solicitado que você crie uma senha com no mínimo 8 caracteres para o usuário **root**. Depois disso é só acessar com a conta **root** e a senha recém criada na interface web do Gitlab.

Exemplo da URL: ``http://pwd10-0-34-3-8880.host1.labs.play-with-docker.com``

Mais informações sobre o GitLab e como configurá-lo, acesse os links abaixo.

* https://docs.gitlab.com/omnibus/docker/README.html
* https://docs.gitlab.com/omnibus/README.html
* https://docs.gitlab.com/omnibus/settings/nginx.html#enable-https
* https://hub.docker.com/r/gitlab/gitlab-ce/
* https://docs.gitlab.com/omnibus/docker/README.html#after-starting-a-container

Lembrando que os dados do banco, repositórios de código, configurações e logs são persistidos no diretório ``/opt/docker/gitlab`` do Docker Host no qual o conteiner está sendo executado.

# Criando um repositório no GitLab

Acesse o GitLab na URL a ser disponibilizada automaticamente após clicar no link 8880 mostrado no topo da página, um pouco mais a direita na linha que é exibido o IP interno da instância. Faça login a conta **root** e a senha recém criada na interface web do Gitlab.

Exemplo da URL: ``http://pwd10-0-34-3-8880.host1.labs.play-with-docker.com``

Crie um novo projeto clicando no botão **New Project**. Defina um nome do projeto como sendo **production** e clique no botão **Create Project**. A URL do repositório Git do repositório Git será mostrada na página inicial do projeto junto com uma pequena documentação de como iniciar o versionamento.

Fonte: https://docs.gitlab.com/ce/gitlab-basics/create-project.html
