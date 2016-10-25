#OCAML

##INSTALACIÓN DE OCAML 

Para instalar Ocaml lo realice a través de [homebrew](http://brew.sh/index_es.html):
```
brew install ocaml
brew install opam
 ``` 
 
##HELLO WORD!

Usa un editor para crear `hello.ml`que contiene la siguiente línea :

    print_string "Hello world!\n";;

Ahora compilaremos y ejecutaremos el programa de la siguiente manera:

    ocamlc -o hello hello.ml
    ./hello

Listo realizamos nuestro primer hola mundo!

Otra alternativa es usar el interprete de OCAML que es como una gran calculadora para entrar:  en la terminal tipea `ocaml` se mostrará lo siguiente: 

    MacBook-Pro-de-x:~ x$ ocaml 
            OCaml version 4.03.0
    
    # 

ahora escribimos nuestra línea de código y se mostrará el resultado 

    # print_string "Hello world!";;
    Hello world!- : unit = ()
    # 
Para finalizar la sesion interactiva escribe `ctrl+D` o llama a la función `exit` del tipo `int -> unit`: 
	 	
    exit 0;;
El interprete también pude ser usado en batch mode, para correr los scripts. El nombre del archivo que contiene el código para ser interpretado es pasado como argumento en la línea de comandos del interprete:
  	

    ocaml hello.ml 	 	
    Hello World

Nota la diferencia entre el comando anterior y el siguiente: 
 	

    ocaml < hello.ml
        	 	
            Objective Caml version 4.03.0
    
    # Hello World
    - : unit = ()
    #