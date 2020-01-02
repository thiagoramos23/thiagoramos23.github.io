---
layout: post
title: Pattern Matching com Elixir
date: 2019-09-12 13:32:20 +0300
description: Explicando o que você precisa saber sobre pattern matching com elixir # Add post description (optional)
img: pattern-matching.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Elixir, Pattern Matching]
---
Se você ja escreveu alguma equação matemática(x = a + 1), você sabe bem que o "=" não quer dizer atribuição (assignment), na verdade quer dizer que você está dizendo que a expressão x tem o mesmo valor que a expressão "a + 1". Sabendo o valor de x facilmente você saberia o valor de a e vice-versa. Joe Armstrong falava que em algum momento você teve que desaprender o significado algébrico do "=" quando você aprendeu a sua primeira linguagem de programação. 

Com Elixir você vai reaprender ou pela menos desaprender a forma errada e voltar a usar a forma algébrica, pelo menos em partes.

Este pequeno artigo está dividido em sete partes:
* O operador de Match (=)
* Match com tuplas
* Match com listas
* Ignorando variáveis com underscore
* O operador Pin(^)
* Match em retorno de funções
* Match em parâmetros de funções

# O operador de Match (=)
Quando começamos com Elixir esse operador geralmente causa muita confusão porque estamos acostumados a pensar nele como uma operador de atribuição de valor. Por isso de vez em quando podemos cair em algum problema onde não sabemos o motivo pelo qual algo aconteceu, e esse problema pode ter sido causado por falta de conhecimento em como o pattern matching e esse operador "=" funciona.
Para explicar melhor vamos à alguns exemplos:
### Isto é um match
```elixir
a = 1
```

Pode parecer que estamos fazendo uma atribuição de um valor 1 para a variável "a" e é isto que pode causar muita confusão porque na verdade não estamos. O que estamos fazendo é um match do valor 1 com a variàvel "a" que agora estará "bound" à esse valor. 
Ahh Thiago mas qual é a diferença entre isso e o que eu faço no Ruby ou no Java. 
Em termos práticos, no momento, não há diferença mas o que você deve ter em mente é que o que está acontecendo aqui é que a expressão "a = 1" é equivalente à uma equação onde o Elixir combina os dois valores e os compara, se tudo der certo ele retorna o resultado dessa equação. Se você rodar isso no console verá impresso o valor final. No caso: 1.
Uma das formas de testar isso é testar o match contrário:
Isto é um match também
```elixir
1 = a
```
O que está expressão vai imprimir é 1.

Mas se testarmos:
### Isto é um match também porém vai dar erro
```elixir
2 = a
```

E o erro que teremos é do tipo:
```elixir
(MatchError) no match of right hand side value: 1
```

Isso porque, préviamente, tínhamos feito um "bind" da variável a com o valor 1.
Pode não parecer muita coisa pra você agora mas até o final do artigo você vai ficar igual à esse cara aqui:
Ou pode ficar que nem esse cara aqui, você decide:
O poder do pattern matching reside no fato de que com ele podemos procurar e/ou verificar match desde padrões simples até estruturas de dados e funções. Continue lendo e não desista que vai dar tudo certo. :))

# Match com tuplas
Como eu disse anteriormente o pattern matching pode ser aplicado em vários tipos de estrutura de dados como em tuplas:
### Isto é um match também
```elixir
{a, b} = {20, 50}
```
Nesse caso estamos fazendo um match no pattern "{a, b}" o que faz com que as variáveis a e b sejam bindadas aos valores 20 e 50.

Se tentarmos agora modificar o valor da variável a usando o mesmo pattern {a, b} não teria problema.
### Isto é um rebound da variável a para o novo valor.
```elixir
{a, b} = {25, 50}
```

Nós ainda poderíamos mudar o valor da variável a:
```elixir
a = 200
{a, b}
> {200, 50}
```
Mais uma vez isso é um rebound. Estamos bindando a variável a um novo valor.

# Match com Listas
Podemos fazer algumas coisas interessantes com o pattern matching usando listas.
Exemplo:
### Este exemplo imprime [1, 2, 3]
```elixir
list = [1, 2, 3]
[1, 2, 3] = list
> [1, 2, 3]
# Aqui estamos imprimindo a variavel b que recebeu o valor do segundo item da lista (list)
[1, b, 3] = list
> [1, 2, 3]
b
> 2
```
Neste exemplo nós substituímos o segundo elemento pela letra "b" fazendo assim com que o segundo elemento da lista "list" fosse bindado à variável b. Quando tentarmos imprimir o valor de b teríamos o 2 como resultado.

Uma lista em elixir é constituída por duas partes. A cabeça e o rabo (é engraçado escrever em português mas é a tradução correta). Mas em ingles como são boa parte das linguagens de programação é chamado head | tail.
O operador |(pipe) serve para que possamos separar o head do tail em uma lista.
Exemplo:
```elixir
list = [1, 2, 3, 4]
[head | tail] = list
head
> 1
tail
> [2, 3, 4]
```

Outra coisa que podemos fazer também é associar aos valores algumas variáveis como:
```elixir
list = [1, 2, 3, 4]
[a, b, c, 4] = list
a
> 1
b
> 2
...
```

Agora você já começa a ver um pouco mais sobre o poder do pattern matching. Estamos associando à variáveis, valores dentro de uma lista.
Neste caso não podemos fazer um match de tamanhos diferentes ou valores diferentes:
### Isso dá erro de match
```elixir
list = [1, 2, 3, 4]
[a, b, c, 5] = list
```
Dá erro porque o valor 5 não é igual ao valor do último item da lista.
Isso daria erro:
```elixir
list = [1, 2, 3, 4]
[a, b, c, 4] = list
list = [6, 7, 8, 9]
[a, b, c, d] = list
```

Porque estaríamos fazendo um rebound das variáveis e bindando um novo valor para a variável d.
Ignorando valores com underscore
O que acontece quando não queremos associar um valor à nada. Simplesmente não nos interessamos por um valor em uma lista ou em uma tupla por exemplo. 
Para isso usamos o underscore. Tudo com underscore do lado esquerdo em um pattern matching é desconsiderado.
```elixir
{a, _} = {30, "Thiago"}
a
> 30
```
O valor "Thiago" nunca será associado a nada já que foi ignorado quando usamos o `_` underscore. Se usarmos assim:
```elixir
{a, _name} = {30, "Thiago"}
a
> 30
```

O valor "Thiago" também seria ignorado. Porém ele não é. O Elixir deixa você usar a variável já que é um named variable porém lança um warning:
```elixir
warning: the underscored variable "_name" is used after being set. A leading underscore indicates that the value of the variable should be ignored. If this is intended please rename the variable to remove the underscore
```

Então para todos os sentidos não use underscore se quiser usar uma variável. Pra quem vem de outras linguagens pode achar estranho já que em outras linguagens poderíamos usar uma variável com underscore no início para indicar que ela não sera usado ou que ela é private.
Em elixir se não se importar com um valor mas precisar fazer o match use o underscore sozinho.

# O operador ^(pin)

Vamos supor que você tem:
```elixir
x = 10
```
Se você fizer:
```elixir
x = 20
```
Sua variável "x" será setada para o valor 20. Mas você pode simplesmente querer verificar o matching de x com o valor 20 ao invés de fazer um rebind.
Nesse caso você deve usar o operador pin(^):
```elixir
x = 10
^x = 20
(MatchError) no match of right hand side value: 20
```

O operador pin serve para que você faça o matching com uma variável que já tem um valor sem que ocorra o rebind da variável, ou seja, sem que ela mude de valor.

# Pattern Matching em Funções

É aqui que você, provavelmente vai fazer um bom uso do pattern matching em elixir. Veja isso:
```elixir
def hello(:bob), do: "Hi My dear friend Bob"
def hello(:mary), do: "Hi my lovely wife Mary"
def hello(name), do: "Hi #{name}"
```

Vejam que em outras linguagens teríamos que ter condicionais para que esse código pudesse ser executado corretamente, no mínimo uma lógica que nem de longe seria tão simples quanto a solução acima. 
O que está acontecendo aqui?
Quando chamamos o método "hello" passando o valor ":bob", Elixir, através do uso de pattern matching, vai chamar a função "hello" onde o valor :bob vai fazer matching com a variável declarada :bob e vai achar a função própria.
Se chamarmos o método "hello" passando "Ana" o matching não ocorrerá e a função encontrada será a terceira.
Vamos agora ver um matching mais sutíl:
```elixir
defmodule MyList do
  def length(list) do
    length(list, 0)
  end
  defp length([], count) do
    count
  end
  defp length([_|t], count) do
    length(t, count + 1)
  end
end
```
Nesse caso iremos chamar "MyList.length([1, 2, 3])" vejamos que temos um método publico "length" e dois métodos privados. Método privados são declarados com "defp". Da primeira vez que chamarmos o método público "length" ele irá chamar qual dos dois métodos privados abaixo dele? Conseguiu adivinhar? Isso é pattern matching em ação com esteróides. 
O primeiro método "length",
```elixir
defp length([], count) do
  count
end
```
faz matching com uma lista vazia e um outro valor que pode ser um número. 
O segundo método:
```elixir
defp length([_|t], count) do
  length(t, count + 1)
end
```
espera que a lista contenha um head que ele não se importa e um tail e mais uma variável que pode ser um número também chamada count.

Então, quando chamarmos o método length público ele chamará pela primeira vez o método que espera a lista com itens e este método fará uma chamada recursiva sempre pegando o "tail" da lista e incrementando o "count" até que a lista fique vazia e seja feito um matching com a função que espera uma lista vazia. E esta função, por sua vez, retorna o count apenas.
Pattern matching, como você pode ver, é uma ferramenta muito poderosa em Elixir. Mas ela ainda é usada de outra forma com funções, ela é muito usada no retorno das funções.

# Match com retorno de funções
Como a pattern matching é uma ferramenta tão poderosa, convencionou-se em Elixir que as funções retornassem não só valores, como também, em muitas vezes, como tuplas. Algo muito comum em elixir é ver esse tipo de código:

```elixir
def create(conn, %{"user" => user_params}) do
  case Accounts.create_user(user_params) do
    {:ok, user} ->
      conn
        |> put_flash(:info, "User created successfully.")
        |> redirect(to: Routes.user_path(conn, :show, user))
    {:error, %Ecto.Changeset{} = changeset} ->
      render(conn, "new.html", changeset: changeset)
  end
end
```

Veja que nesse caso a função "create_user" do módulo "Accounts" pode retornar dois tipos de tuplas, uma com o valor :ok e um user e outra com o valor :error e um struct do tipo "Ecto.Changeset". O que fazemos, normalmente, é fazer o pattern matching com o retorno do método "create_user" e executar a ação apropriada. Vejam como é poderoso o pattern matching e como ele é extensamente usado pela linguagem.
Um último tópico que quero mostrar é como é possível extrair apenas as variáveis que nos interessam em um struct, map ou usar parâmetros nomeados, quando passamos parâmetros em uma função.
Exemplo 1: (Usando pattern match em uma variável de um struct e pegando todo o struct através de outra variável)
```elixir
defmodule MyModule do
  def get_keys(%{name: 'Thiago'} = map) do
    Map.keys(map)
  end
end
```

Podemos usar:
```elixir
MyModule.get_keys(%{name: 'bob', last_name: 'Ramos'})
** (FunctionClauseError) no function clause matching in MyModule.get_keys/1
The following arguments were given to MyModule.get_keys/1:
# 1
 %{last_name: 'Ramos', name: 'Bob'}
iex:4: MyModule.get_keys/1
```
Isso acontece porque o matching entre a variável "name" e o valor falharam quando chamamos a função "get_keys" do módulo "MyModule".

Se quisermos usar a variável name dentro do método sem precisar pegar dentro do map poderemos fazer da seguinte forma:
```elixir
def test(%{name: name} = %{name: _}) do
  IO.puts name
end
```
Nesse caso poderemos chamar:
```elixir
test(%{name: "Carlos", age: 25})
Carlos
:ok
```

Isso pode ser feito para vários tipos de estrutura de dados e inclusive nós usamos no exemplo de recursão.
Como vocês podem ver, Pattern Matching é uma das ferramentas mais importantes em Elixir e é muito importante que você domine bem para que possa se adequar mais facilmente ao idioma da linguagem. Além, é claro, de entender bem como funciona o paradigma funcional.
Espero que tenham gostado e se quiserem, me sigam nas redes sociais:

* Youtube: [Meu canal no Youtube](https://www.youtube.com/thiagoramosal)
* Twitter: [@thramosal](https://twitter.com/thramosal)
* Instagram: [@thiagoramosal](https://instagram.com/thiagoramosal)

Se você quer ler mais sobre Elixir eu recomendo este site onde o autor está documentando todo o seu aprendizado com a linguagem:
[https://inquisitivedeveloper.com/tag/lwm-elixir/](https://inquisitivedeveloper.com/tag/lwm-elixir/)

Até a próxima.