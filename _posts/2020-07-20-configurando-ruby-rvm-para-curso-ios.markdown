---
layout: post
title: "Como configurar Ruby e RVM"
date: 2020-07-25 10:10:10 +0300
description: Como configurar Ruby e RVM para o curso de iOS # Add post description (optional)
img: server.jpg # Add image post (optional)
fig-caption: programador # Add figcaption (optional)
tags: [Ruby, Rails, RVM, Iniciante, Beginner]

---
# Como configurar o Ruby e RVM

Este post é um post excepcional dedicado aos alunos do curso de SwiftUI com Combine, atualmente ministrado na Digital Innovation One.
Um pré-requisito é que você tenha a ferramenta `curl` instalada na sua máquina.

Se você chegou aqui através do link no curso então os passos que você deve seguir são esses:

1. Clonar o projeto insta_server no github. [Link do Projeto](https://github.com/thiagoramos23/insta_server.git)
2. Entrar nesse site para instalar o rvm: [Instalar RVM](https://rvm.io/rvm/install)

3. Você pode seguir os passos do site que basicamente são esses comandos:
  * `gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB`
  * `\curl -sSL https://get.rvm.io | bash`

4. Em um terminal, entre na pasta do projeto. Você deve ver a mensagem:
  * `Required ruby-2.5.1 is not installed. To install do: 'rvm install "ruby-2.5.1"'`

5. Execute o comando `rvm install ruby-2.5.1`. Este passo deve demorar um pouco

6. Saia da pasta do projeto

7. Volte para a pasta do projeto.

8. Rode o comando: `gem install bundler:2.1.4`

9. Rode o comando: `bundle install`
	Neste ponto você pode ter problemas com a gem do postgres.
	Como resolver este problema depende muito de como você instalou o Postgres na sua máquina.
	Eu uso o [postgres app](https://postgresapp.com/downloads.html1). Aconselho usar o app porque é mais fácil de configurar e roda do mesmo jeito.
	* Após baixar e instalar o app. Rode este comando:
	  * `gem install pg -v '1.1.4' -- --with-pg-config=/Applications/Postgres.app/Contents/Versions/12/bin/pg_config`

	* Após isso rode novamente o comando `bundle install`

10. Rode o comando: `rake db:create && rake db:migrate && rake db:seed`

11. Rode o comando: `rails server`. Você deve ver algo do tipo:
```bash
	=> Booting Puma
	=> Rails 6.0.2.1 application starting in development
	=> Run `rails server --help` for more startup options
	Puma starting in single mode...
	* Version 4.3.5 (ruby 2.5.1-p57), codename: Mysterious Traveller
	* Min threads: 5, max threads: 5
	* Environment: development
	* Listening on tcp://127.0.0.1:3000
	* Listening on tcp://[::1]:3000
	Use Ctrl-C to stop
```

Qualquer problema pode falar comigo através das redes sociais.

* Youtube: [Youtube Channel](https://www.youtube.com/thiagoramosal)
* Twitter: [@thramosal](https://twitter.com/thramosal)
* Instagram: [@thiagoramosal](https://instagram.com/thiagoramosal)
