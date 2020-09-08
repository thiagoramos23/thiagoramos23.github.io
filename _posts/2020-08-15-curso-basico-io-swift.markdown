---
layout: post
title: Curso iOS e Swift
date: 2020-08-15  23:00:00 +0300
description: Primeiro curso que ministrei para a Digital Innovation One. # Add post description (optional)
img: mac.jpg # Add image post (optional)
fig-caption: curso # Add figcaption (optional)
tags: [Curso, iOS, Swift] # add tag
---

# Curso Básico de iOS e Swift

Faz um bom tempo que penso em devolver pra comunidade o que eu aprendi com ela. Seja fazendo cursos grátis ou pagando, seja vendo vídeos no youtube ou vendo vídeos no twitch, seja lendo artigos no dev.to ou em outros blogs, a questão é que sempre aprendi muito com a comunidade.

Pra quem não sabe eu programei para iphones durante muito tempo ao mesmo tempo em que também trabalhava com Java. Quando migrei para ruby acabei indo trabalhar em uma empresa como backend e isso me afastou do front e da programação mobile. Mas no último ano eu resolvi voltar a estudar e me dedicar de novo e até mais profundamente.

E a melhor forma de aprender é com certeza ensinando. Coincidentemente eu recebi um convite da Digital Innovation One para realizar o curso básico de Swift e eu aceitei o que me fez voltar a estudar com mais afinco.

Dito isto, você vai encontrar neste curso o básico pra começar a escrever as suas próprias aplicações. No curso eu tentei mostrar como fazer vários aplicativos do início ao fim porém eu não me preocupei em torná-los arquiteturalmente bons ou excelentes.

A idéia principal foi tornar o acessível o máximo que eu pudesse conseguir. Criar um curso básico e por muitas vezes muito mais complicado do que criar um curso mais avançado porque a gente nunca consegue saber se está explicando de uma forma que esteja clara para as pessoas que possam ou irão ver o conteúdo.

Bom, chega de enrolação. Este post vai acabar ficando bem longo porque vou postar todos os vídeos aqui um atrás do outro, porém vou postá-los conforme for postando no youtube.

Neste primeiro vídeo eu falo sobre auto layout e como ele afeta a posicionamento dos componentes gráficos em vários tamanhos de telas diferentes. Explico o porque ele existe de forma bem suscinta e mostro alguns exemplos de como usá-lo. Em uma futura aula eu mostro em um exemplo mais realista como usamos auto layout para desenhar uma tela e seus componentes. Espero que curtam essa primeira aula e se curtirem não esqueçam de dar aquele joinha e se inscrever no canal.

<center><iframe width="800" height="600" src="https://www.youtube.com/embed/-VGmS9GXtqw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></center>

No segundo vídeo da série nós começamos a criar o nosso primeiro aplicativo de To Do. Nesse vídeo eu mostro como usar Storyboards e mostro como usar o auto layout para criar uma célula em um TableView. Também confifguramos a parte inicial do projeto:

<center><iframe width="800" height="600" src="https://www.youtube.com/embed/5Z7TXUCbLaY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></center>

No terceiro vídeo da série nós vamos codar de verdade e vamos criar a nossa primeira `TableViewController`. Vamos criar o model Task, os controllers e vamos usar dados brutos para podermos implementar os métodos obrigatórios do UITableViewController que são UITableViewDelegate e UITableViewDataSource. Vamos aprender como criar uma classe para uma célula customizada, como associar, usando IBOutlet, os campos em um storyboard ou uma célula para a classe em Swift e vamos aprender como passar dados para um célula e como associar a célula no Storyboard diretamente com o controller. No final dessa aula você será capaz de criar um TableViewController com uma célula customizada e será capaz de implementar os métodos responsáveis por renderizar as células no Storyboard.

<center><iframe width="800" height="600" src="https://www.youtube.com/embed/kGiIYTaTSu0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></center>

No quarto vídeo da série. Nesta aula vamos iniciar a criação da tela onde nós vamos criar nossas tasks que serão exibidas na tela de listagem. 
Falaremos sobre como associar componentes do tipo TextField e UIPicker, como fazer uma célula interativa onde dentro da célula existe um componente de texto. 
Falaremos sobre como associar esses componentes e adicionar os eventos aos componentes de tela. Também criaremos todo o código para tornar a tela de criação de tasks.

<center><iframe width="800" height="600" src="https://www.youtube.com/embed/ZujPIRf5u7w" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></center>

Este é o quinto vídeo da série. Nesta aula vamos criar a tela para selecionar as categorias de cada task. Vamos também finalizar todo o código de interação entre as telas para que possamos
ter o aplicativo totalmente funcional.

<center><iframe width="800" height="600" src="https://www.youtube.com/embed/wEqmw8PXyHE" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></center>

Este é o sexto vídeo da série. Nesta aula vamos implementar o banco de dados local para que possamos salvar as nossas tasks de forma que os dados se mantenham persistidos depois de fecharmos a aplicação e reabrirmos.
Para isso iremos utilizar o Core Data.

<center><iframe width="800" height="600" src="https://www.youtube.com/embed/jHSIkPNtd-E" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></center>

Este é o sétimo vídeo da série. Nesta aula vamos continuar a implementação da framework de Core Data na nossa aplicação de tasks.
Especificamente iremos implementar toda a criação do Core Data Stack.

<center><iframe width="800" height="600" src="https://www.youtube.com/embed/eldf_YqE7nk" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></center>

Este é o oitavo vídeo da série. Nesta aula vamos finalizar a implementação do Core Data e finalizar o App. Ao final desta aula você terá aprendido como implementar a stack do Core Data e como salvar os seus objetos 
com Core Data.

<center><iframe width="800" height="600" src="https://www.youtube.com/embed/MTvUmkRGPkE" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></center>
  
Conforme os outros vídeos forem sendo lançados, eles serão postados aqui.

Se quiser me seguir e/ou conversar comigo pode me achar nesses links:

* Youtube: [Youtube](https://www.youtube.com/thiagoramosal)
* Twitter: [@thramosal](https://twitter.com/thramosal)
* Instagram: [@thiagoramosal](https://instagram.com/thiagoramosal)
