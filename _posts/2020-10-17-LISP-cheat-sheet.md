---
layout: post
title:  "LISP cheat sheet"
date:   2020-10-17 00:00:00 +0200
categories: jekyll update
---
## PRIMITIVAS FUNDAMENTALES
* *setf* - asigna valores a una lista
* *atom* - te dice si algo es átomo o no

## OPERACIONES CON LISTAS:
* *first* - devuelve un átomo que tiene como elemento el primer elemento de la lista
* *rest* - devuelve una lista con todos los elementos menos el primero
* *last* - devuelve una lista con el ultimo elemento

* *append* - combina los elementos de todas las listas
* *list* - hace una lista con lo que le pases
* *cons* - hace una lista con el primer elemento, y añade esa lista creada a otra lista seguida del resto de los elementos (1 2) -> ((1) 2)

* *length* - devuelve el numero de elementos de una lista
* *reverse* - invierte el orden de los elementos de una lista

* *assoc* - como un hashmap - (setf javier '((estatura 1.74) (peso 66))) -> (assoc 'estatura javier)

## PROCEDIMIENTOS
```
defun - definir funciones
	(defun miFuncion
		(parametro1 parametro2)
		(cons
			(first parametro1) (last parametro2)
		)
	)
```
## CONDICIONALES
* *member* - verifica si el primer argumento forma parte del segundo - devuelve una lista a partir de ese elemento en la lista en la que buscar
* *atom* - es un átomo?
* *numberp* - es un número?
* *symbolp* - es un símbolo?
* *listp* - es una lista?
* *null* - el argumento es una lista vacía? - acepto que me metas lo que quieras
* *endp* - el argumentos es una lista vacía? - solo acepto listas
	**CONCLUSION**
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

* if - como en C

```
(if argumento (hazEsto siCierto)
	(hazEsto siFalso)
)
```
* *when* - solo parte de lo cierto
* *unless* - solo parte de lo falso

* cond - como el switch en C, Java y el case en Bash

```
(cond (argumento1 hazEsto1)
	(argumento2 hazEsto2)
	...
	(argumentoN hazEstoN)
	(t hazEstoPorDefecto)
)
```

## RECURSIVIDAD
FUNCTION FACTORIAL HECHA POR MI SIN MIRAR APUNTES :·D
```
(defun factorial (número)
	(if (= número 1)
		(* número (factorial (- número 1)))
	)
)
```

```
(defun fibonacci (número)
	(cond	((= número 0) 0)
		((= número 1) 1)
		(t (+ (fibonacci (- n 1) (fibonacci (- n 2)))
	)
)
```

## ITERATIVIDAD
```
(dolist (elemento lista resultadoQueDevuelveDolist)
	(+ resultadoQueDevuelveDolist 1)
)
```

## COMO HACER LA PRÁCTICA
```
(defun atomo (var)
	(cond ((atom var) T)
		((eq (first var) '?) T)
		(T NIL)
	)
)
```

## PROG - controlar el flujo de ejecución
```
(prog (var1 var2 ...)
	(cuerpo (return valor))
)
```

**EN LISP TODAS LAS VARIABLES SON GLOBALES EXCEPTO LOS PARÁMETROS QUE RECIBEN LAS FUNCIONES**
