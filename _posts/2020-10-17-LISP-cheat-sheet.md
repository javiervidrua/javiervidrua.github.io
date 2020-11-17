---
layout: post
title:  "LISP cheat sheet"
date:   2020-10-17 00:00:00 +0200
categories: jekyll update
---
## Primitivas fundamentales
* *setf* - asigna valores a una lista
* *atom* - te dice si algo es átomo o no
* *print* - sacar algo por pantalla

```lisp
(print "[-] Error crítico: Explosión inminente! D:")
```

* *format* - sacar texto y datos por pantalla (como *printf* en *C*)

```lisp
(format t "~% Variable cadena tiene valor ~s ~%" cadena)
```

## Operaciones con listas
* *first* - devuelve un átomo que tiene como elemento el primer elemento de la lista
* *rest* - devuelve una lista con todos los elementos menos el primero
* *last* - devuelve una lista con el ultimo elemento

* *append* - combina los elementos de todas las listas
* *remove* - elimina un elemento de una lista
* *list* - hace una lista con lo que le pases
* *cons* - hace una lista con el primer elemento, y añade esa lista creada a otra lista seguida del resto de los elementos (1 2) -> ((1) 2)

* *length* - devuelve el número de elementos de una lista
* *reverse* - invierte el orden de los elementos de una lista

* *assoc* - como un hashmap

```lisp
(setf supra '((motor 2JZ-GTE) (turbo GT-40)))
(assoc 'motor supra)
```

## Procedimientos
* *defun* - definir funciones

``` lisp
	(defun miFuncion
		(parametro1 parametro2)
		(cons
			(first parametro1) (last parametro2)
		)
	)
```
## Condicionales
* *member* - verifica si el primer argumento forma parte del segundo - devuelve una lista a partir de ese elemento (incluído) en la lista en la que buscar
* *atom* - es un átomo?
* *numberp* - es un número?
* *symbolp* - es un símbolo?
* *listp* - es una lista?
* *null* - el argumento es una lista vacía? - acepta lo que sea
* *endp* - el argumentos es una lista vacía? - solamente acepta listas

	**Conlusión**
	() = '() = NIL = 'NIL -> **todos son átomos**

* *zerop* - es cero?
* *plusp* - es positivo?
* *minusp* - es negativo?
* *evenp* - es par?
* *oddp* - es impar?
* *\>* - orden descendente en todos los parámetros?
* *\>* - orden ascendente en todos los parámetros?

* *=* - los argumentos son el mismo número?
* *equalp* - los argumentos son iguales?
* *eq* - "los argumentos ocupan la misma cantidad de memoria?"

* *and* - como en C
* *or* - como en C
* *not* - como en C

* *if* - como en C

``` lisp
(if comparacion
	(hazEsto siCierto)
	(hazEsto siFalso)
)
```
* *when* - solo parte de lo cierto
* *unless* - solo parte de lo falso

* *cond* - como el switch en C, Java y el case en Bash

``` lisp
(cond
	(comparacion1
		hazEsto1
	)
	(comparacion2
		hazEsto2
	)
	...
	(comparacionN
		hazEstoN
	)
	(T hazEstoPorDefecto)
)
```

## Recursividad
``` lisp
(defun factorial (n)
	(if (= n 1)
		(* n (factorial (- n 1)))
	)
)
```

``` lisp
(defun fibonacci (n)
	(cond	((= n 0) 0)
		((= n 1) 1)
		(t (+ (fibonacci (- n 1) (fibonacci (- n 2)))
	)
)
```

## Iteratividad (necesario dar valor a resultado)
``` lisp
(dolist (elemento lista resultado)
	(+ resultado 1)
)
```

## Prog: Controlar flujo de ejecución + variables locales
``` lisp
(prog (var1 var2 ...)
	(setf valor (+ 1 1))
	(return valor)
)
```

## Cómo hacer la práctica
``` lisp
(defun es-variable (var)
	(cond ((atom var) T)
		((eq (first var) '?) T)
		(T NIL)
	)
)
```

**En CLISP todas las variables son globales excepto los parámetros que reciben las funciones**
