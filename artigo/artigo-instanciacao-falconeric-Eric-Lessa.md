# Artigo - Instanciação
**Autor**: Eric Lessa - [falconeric](https://github.com/falconeric)

**Data**: // execute o Date.now() e coloque o timestamp aqui

Neste artigo sobre instanciação irei abordar alguns assuntos que podem ser confusos para desenvolvedores que estão iniciando sua carreira em javascript, falarei sobre hoisting, closure, variável global, variável por parâmetro e instanciação usando IIFE.

Antes, gostaria de falar sobre declaração de variáveis pois vai nos ajudar a compreender o assuntos citados acima.

Variáveis declaradas são processados antes que qualquer código seja executado. Segundo o MDN "o escopo de uma variável declarada com var é seu contexto atual em execçuão", ou seja, se uma variável é declarada dentro de uma função seu escopo será a função onde ela foi declarada, variáveis declaradas fora de uma função pertencem ao escopo global. Quando atribuímos um valor a uma variável que não foi inicializada anteriormente esta vai pertencer ao escopo global, vejamos o exemplo a seguir:

```
var a = 'Variável A';

function variaveis() {
  	b = 'Variável B'
	var c = 'Variável C'

	console.log(a) //imprime o valor da variável global
}

variaveis()

console.log(b) //imprime o valor de b
console.log(c) //lança a exceção "ReferenceError: c is not defined"
```
agora que entendemos o comportamento básico de variáveis vejamos o seguinte assunto:

## Hoisting
Hoisting literalmente significa: içar, levantar

É isso o que acontece quando declaramos variáveis, elas são elevadas ao inicio do seu escopo, lembre-se, sua declaração sim é elevada não sua inicialização. Como sabemos que as variáveis declaradas são processadas antes que qualquer código seja executado fica subentendido que, declarar variáveis em qualquer parte do código é equivalente a declarar no inicio, claro, respeitando seu escopo. Vamos ao exemplo:

```
var a = 1

function hoisting() {
	console.log(a)
	var a = 2
	console.log(a)
}

hoisting()

console.log(a)
```
a execução do código acima vai imprimir: undefined, 2, 1

WTF!!! por que isso chessus?

acabamos de ver na prática o que é hoisting e agora vou explicar o que aconteceu com as variáveis do código acima.

```
var a //a nossa variável *a* passa a ser delcarada no inicio do nosso código
a = 1 //em seguida ela é inicializada com o valor informado

function hoisting() {
	var a 	//dentro do escopo da nossa função a variável *a* que havia sido declarada e inicializada
			//no escopo global é sobreescrita ao ser declarada dentro da função, neste ponto como ela 
			//foi *hoisted* (içada, elevada para o inicio de seu escopo) ela passa a ser *undefined*

	console.log(a) //pelo motivo descrito acima aqui ele imprime *undefined*
	
	a = 2 //neste ponto a variável *a* recebe o valor 2
	console.log(a) //então aqui é impresso o valor 2 em nosso console
}

hoisting() //chamada da função

console.log(a) //aqui é impresso o valor definido na variavel global
```


## Closure

## Variável Global

## Variável por parâmetro

## Instanciação usando uma IIFE