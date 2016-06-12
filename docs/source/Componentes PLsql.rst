PL/SQL
=======

En esta sección del manual se definen algunos conceptos básicos pero muy importantes a la hora del desarrollo de los procedimientos  almacenados, funciones, trigggers y demás componentes del PL/SQL. También algunos ejemplos de los componentes PL/SQL  antes mencionados y su documentación dentro de un  catálogo. La documentación de estos componentes es muy importante por lo tanto se recomienda realizar un trabajo ordenado para facilitar el mantenimiento de  estos componentes en la base de datos. Otro aspecto importante es que los procesos y funciones almacenas  son de gran ayuda en muchas empresas ya que les facilita el trabajo en cuanto se refiere a realizar algunos cálculos empresariales, además un aspecto aún más importantes es que muchos de estos resultados que muestran las bases de datos por medio de estos componentes de programación son vitales para la toma de decisiones en muchas empresas ya que pueden indicar si una empresa está teniendo un déficit de ventas por lo cuál sea necesario  realizar un reajuste para mejorar dicha situación, esto para un empresa que se dedique a las ventas. Otro ejemplo es el uso del lanzamiento de triggers para realizar bitácoras lo cual es de bastante ayuda en casos de pérdida de información.También en una base de datos de un hospital puede ayudar a realizar estadísticas sobre enfermedades que ayuden en un futuro a combatirlas de una mejor manera. Es por ello que se hace el énfasis en realizar un buen trabajo en esta parte de la base de datos.

**PL/SQL**  es el lenguaje de programación que proporciona Oracle para extender el SQL estándar con otro tipo de instrucciones y elementos propios de los lenguajes de programación.

**Construyendo bloques de programas PL/SQL**
---------------------------------------------
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

PL/SQL es un lenguaje estructurado con bloques. Un bloque PL/SQL es definido por las palabras clave ``DECLARE``, ``BEGIN``, ``EXCEPTION``, y ``END``, que dividen el bloque en tres secciones

**1. Declarativa**: sentencias que declaran variables, constantes y otros elementos de código, que después pueden ser usados dentro del bloque

**2. Ejecutable:** sentencias que se ejecutan cuando se ejecuta el bloque

**3. Manejo de excepciones**: una sección especialmente estructurada para atrapar y manejar cualquier excepción que se produzca durante la ejecución de la sección ejecutable


Unidades que se pueden programar en una base de datos Oracle:

•	Procedimientos almacenados
•	Funciones
•	Triggers


**Conceptos Básicos**
-----------------------
^^^^^^^^^^^^^^^^^^^^^^^

**•	DELIMITADOR:** Es un símbolo, simple o compuesto, que tiene una función especial en PL/SQL:

- Operadores Aritméticos
- Operadores Lógicos
- Operadores Relacionales

**•	IDENTIFICADOR:** Son empleados para nombrar objetos de programas en PL/SQL, así como a unidades dentro del mismo: 

- Constantes
- Cursores
- Variables
- Subprogramas
- Excepciones
- Paquetes

**•	LITERAL:** Es un valor de tipo numérico, carácter, cadena o lógico no representado por un identificador (es un valor explícito).


**•	COMENTARIO:** Es una aclaración que el programador incluye en el código. Son soportados dos estilos de comentarios, el de línea simple y de multilínea, para lo cual son empleados ciertos caracteres especiales.


**Tipos de Datos**
-------------------
^^^^^^^^^^^^^^^^^^^

PL/SQL proporciona una variedad de tipos de datos para especificar el formato de almacenamiento, restricciones y rango de valores válidos de constantes y variables.A continuación una lista de los tipos de datos más comunes:

•	``NUMBER`` (numérico): Almacena números enteros o de punto flotante..


•	*CHAR* (carácter): Almacena datos de tipo carácter con un tamaño máximo de 32.767 bytes y cuyo valor de longitud por defecto es 1.


•	``VARCHAR2`` (carácter de longitud variable): Almacena datos de tipo carácter empleando sólo la cantidad necesaria aun cuando la longitud máxima sea mayor.

•	``BOOLEAN`` (lógico): Almacena valores TRUE o FALSE.


•	``DATE`` (fecha): Almacena datos de tipo fecha. Las fechas se almacenan internamente como datos numéricos, por lo cual se puede realizar operaciones aritméticas con ellas.

•	Atributos de tipo. Un atributo de tipo PL/SQL es un modificador que puede ser usado para obtener información de un objeto de la base de datos. El atributo ``%TYPE`` permite conocer el tipo de una variable, constante o campo de la base de datos. El atributo ``%ROWTYPE`` permite obtener los tipos de todos los campos de una tabla de la base de datos, de una vista o de un cursor.

**Nombres en una base Oracle**
-------------------------------
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Estas son las reglas para construir identificadores válidos en una base Oracle:

• El largo máximo es de 30 caracteres.
• El primer caracter debe ser una letra, pero cada caracter después del primero puede ser una letra, un número (0 a 9), un signo de pesos ($), un guión bajo (_), o un numeral (#). Todos los siguientes son identificadores válidos.

::
 
   hola_mundo
   hola$mundo
   hola#mundo

pero estos son inválidos:
::
 
 1hola_mundo
 hola%mundo

• PL/SQL es case-insensitive (no es sensitivo a mayúsculas y minúsculas) con respecto a los identificadores. PL/SQL trata todos los siguientes como el mismo identificador

::

  hola_mundo
  Hola_Mundo
  HOLA_MUNDO

Para ofrecer más flexibilidad, Oracle permite evitar las restricciones de la segunda y tercera regla, encerrando al identificador entre comillas dobles. Un quoted identifier (identificador encerrado entre comillas) puede contener cualquier secuencia de caracteres imprimibles excluyendo las comillas dobles; las diferencias entre mayúsculas y minúsculas serán además preservadas. Así, todos los siguientes identificadores son válidos y distintos:

::

 "Abc"
 "ABC"
 "a b c"

Estas mismas reglas aplican a los nombres de los objetos de base de datos como tablas, vistas y procedimientos, con una regla adicional: a menos que se encierren entre comillas los nombres de estos objetos, Oracle los mantendrá en mayúsculas.

**Procedimientos Almacenados**
-------------------------------
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

A continuación se muestran algunos procesos almacenados básicos pero que en en este caso funcionan para ejemplificar como se debería dar la documentación de los mismos.
A la hora de crear los procesos almacenados es muy importante la identificación de los  mismos es preferible que se utilice un prefijo  en el identificador similar para todos  los procesos almacenados, así como también para funciones y triggers esto por ayudará  en el mantenimiento de la base de datos ya que facilitará las búsquedas de los mismos cuando se requieran hacer auditorias o sea necesario reparar un problema que se esté dando. Para estos ejemplos  se utilizará el e siguiente prefijo SP  el cual ira seguido por un consecutivo por ejemplo ``SP001`` y ``SP002`` y así sucesivamente claro está que esta escogencia del prefijo quedará a cargo de los creadores de la base de datos.

La siguiente es una tabla llamada T1 la cual se usará para ejemplificar el desarrollo de algunos procedimientos almacenados los cuales a su vez modificarán el contenido de sus registros.
::

 Create table  t1(
 a int,
 b int,
 c int   );

**Insertar**

El siguiente método se utilizará para insertar  registros dentro base de datos, en este caso específicamente en la tabla t1 , se utiliza la recomendación anteriormente mencionada sobre la asignación del identificador utilizado para el proceso almacenado en este caso se utiliza ``SP001`` más adelante en los siguientes procesos se podrá ver como se continua con un prefijo de nombre similar para los demás procesos.

::

 create or replace Procedure SP001 (x int , y int  , z int) 
 AS
 BEGIN
 INSERT into t1 (a,b,c) values(x,y,z);
 Commit;
 END;
 /

Es importante no olvidar que después de escribir el procedimiento almacenado para finalizar la operación se coloca un  / y para ejecutar un proceso se utiliza execute nombre del Proceso (variable tipo).
Ejemplo:
::

 execute  SP001(1,2,3);


**Actualizar**

El siguiente método se utilizará para realizar una actualización  algún registro dentro base de datos, en este caso específicamente al atributo a con el valor igual a 1 de la tabla T1 para este caso el nombre que se le asigna es ``SP002``  continuando así con la secuencia de nombres asignados.
::

 create or replace Procedure SP002 ( x int )
 As
 Begin
 Update T1 set a = x
 where a = 1;
 Commit;
 End;


**Eliminar**

El siguiente método se utilizará para eliminar  algún dato dentro base de datos, en este caso específicamente al atributo a con el valor igual a la variable x la cuál le es enviada por parámetro. También como en los casos anteriores  se le asigna el mismo prefijo pero con diferente consecutivo ``SP003``.
::

 create or replace Procedure SP003 ( x int )
 As 
 Begin 
 Delete from t1 where a = x;
 Commit;
 End;



A continuación se dará una referencia de como documentar los procedimientos almacenados el cual también se podrá usar para documentar otros componentes como funciones almacenadas y triggers mediante un catálogo de objetos para este primer caso será un catálogo de procedimientos almacenados. Además de documentar de manera digital este catálogo también se debe documentar a nivel de la base datos mediante la creación de una tabla específicamente hecha para esta documentación de los procesos almacenados.
