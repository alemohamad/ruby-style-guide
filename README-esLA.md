# Preludio

> Los modelos a seguir son importantes. <br/>
> -- Officer Alex J. Murphy / RoboCop

Una cosa siempre me ha molestado como desarrollador Ruby - los
desarrolladores Python tienen una gran referencia de estilo de
programación ([PEP-8](http://www.python.org/dev/peps/pep-0008/)) y
nosotros nunca tuvimos una guía oficial, documentando estilos de
codificación y mejores prácticas de Ruby. Y yo creo que los estilos
importan. También creo que una gran comunidad de hackers, tales como
tiene Ruby, debería ser muy capaz de producir este codiciado documento.

Esta guía comenzó su vida como nuestra guía interna de codificación de
Ruby para nuestra compañía (escrito por su servidor). En algún momento
decidí que el trabajo que estaba haciendo podría ser interesante para los
miembros de la comunidad Ruby en general y que el mundo no tenía la
necesidad de otra guía interna de una empresa. Pero sin duda el mundo
podría beneficiarse de un conjunto de prácticas, lenguajes y estilos para
la programación de Ruby dirigido por la comunidad y sancionado por la
comunidad.

Desde el inicio de la guía he recibido un montón de comentarios de los
miembros de la excepcional comunidad Ruby alrededor de todo el mundo.
¡Gracias por todas las sugerencias y el apoyo! Juntos podemos hacer un
recurso beneficioso para todos y cada uno de los desarrolladores Ruby.

Por cierto, si estás interesado en Rails es posible que desees ver la [Guía
de Estilos de Ruby on Rails 3](https://github.com/bbatsov/rails-style-guide)
complementaria.

# La Guía de Estilos de Ruby

Esta guía de estilo de Ruby recomienda las mejores prácticas a fin de que
los programadores Ruby del mundo real pueden escribir código que pueda ser
mantenido por otros programadores Ruby del mundo real. Una guía de estilo que
refleje los usos del mundo real es utilizada, y una guía de estilo que se aferra
a un ideal que ha sido rechazado por las personas supone que lo mejor es no
utilizarla para nada &ndash; no importa lo bueno que sea.

La guía está dividida en varias secciones de reglas relacionadas. Intenté
agregar la racionalización de las normas (si está omitido asumí que es bastante
obvio).

No inventé todas las reglas desde la nada misma - ellas se basan sobre todo en
mi extensa carrera como ingeniero de software profesional, junto con los
comentarios y sugerencias de los miembros de la comunidad Ruby y diversos recursos
conocidos de programación de Ruby, como
["Programming Ruby 1.9"](http://pragprog.com/book/ruby4/programming-ruby-1-9-2-0)
y ["The Ruby Programming Language"](http://www.amazon.com/Ruby-Programming-Language-David-Flanagan/dp/0596516177).

La guía todavía es un trabajo en progreso - a algunas reglas le faltan ejemplos, y
algunas reglas no tienen ejemplos que ilustren con la suficiente claridad. En su
debido tiempo se abordarán estos temas - sólo ténganlos en cuenta por ahora.

Podés generar un PDF o una versión HTML de esta guía usando
[Transmuter](https://github.com/TechnoGate/transmuter).

[RuboCop](https://github.com/bbatsov/rubocop) es un validador de código, basado en
esta guía de estilo.

Traducciones de esta guía están disponibles en los siguientes idiomas:

* [Inglés](https://github.com/bbatsov/ruby-style-guide/blob/master/README.md) (versión original)
* [Chino Simplificado](https://github.com/JuanitoFatas/ruby-style-guide/blob/master/README-zhCN.md)
* [Chino Tradicional](https://github.com/JuanitoFatas/ruby-style-guide/blob/master/README-zhTW.md)
* [Francés](https://github.com/porecreat/ruby-style-guide/blob/master/README-frFR.md)

## Tabla de Contenidos

* [Estructura del Código Fuente](#estructura-del-c%C3%B3digo-fuente)
* [Sintaxis](#sintaxis)
* [Nombres](#nombres)
* [Comentarios](#comentarios)
    * [Apuntes de Comentarios](#apuntes-de-comentarios)
* [Clases](#clases-y-m%C3%B3dulos)
* [Excepciones](#excepciones)
* [Colecciones](#colecciones)
* [Strings](#strings)
* [Expresiones Regulares](#expresiones-regulares)
* [Porcentajes Literales](#porcentajes-literales)
* [Metaprogramación](#metaprogramaci%C3%B3n)
* [Varios](#varios)
* [Herramientas](#herramientas)

## Estructura del Código Fuente

> Casi todo el mundo está convencido de que todos los estilos
> excepto los propios son feos e ilegibles. Deja de lado el
> "excepto los propios", y probablemente tengan razón... <br/>
> -- Jerry Coffin (sobre indentación)

* Usa `UTF-8` como la codificación del archivo fuente.
* Usa dos **espacios** por cada nivel de identación. No tabs.

    ```Ruby
    # mal - cuatro espacios
    def some_method
        do_something
    end

    # bien
    def some_method
      do_something
    end
    ```

* Usa finales de línea estilo Unix. (*por defecto los usuarios BSD/Solaris/Linux/OSX
  están protegidos, los usuarios de Windows tienen que tener mucho cuidado.)
    * Si estás usando Git es posible que quieras agregar la siguiente
    configuración para proteger tu proyecto de los finales de línea
    de Windows para que no aparezcan solos:

    ```bash
    $ git config --global core.autocrlf true
    ```

* No uses `;` para separar declaraciones y expresiones.
  Por convención - usá una expresión por línea.

    ```Ruby
    # mal
    puts 'foobar'; # superfluous semicolon

    puts 'foo'; puts 'bar' # dos expresiones en la misma línea

    # bien
    puts 'foobar'

    puts 'foo'
    puts 'bar'

    puts 'foo', 'bar' # esto aplica a puts en particular
    ```

* Prefiere un formato de única línea por cada definición de clase
  sin cuerpo.

    ```Ruby
    # mal
    class FooError < StandardError
    end

    # casi bien
    class FooError < StandardError; end

    # bien
    FooError = Class.new(StandardError)
    ```

* Evita métodos de una sola línea. Aunque son algo popular, hay
  algunas particularidades sobre su sintaxis para definirlos que
  hacen que su uso indeseable. En cualquier caso - no debe
  existir más de una expresión en un método de una sola línea.

    ```Ruby
    # mal
    def too_much; something; something_else; end

    # casi bien - el primer ; es necesario
    def no_braces_method; body end

    # casi bien - el segundo ; es opcional
    def no_braces_method; body; end

    # casi bien - la sintaxis es válida, pero al no usar ; hace que sea un poco difícil de leer
    def some_method() body end

    # bien
    def some_method
      body
    end
    ```

    Una excepción a la regla son los métodos vacíos.

    ```Ruby
    # bien
    def no_op; end
    ```

* Usa espacios entre operadores, después de las comas, dos puntos y puntos
  y comas, luego de `{` y antes de `}`. Los espacios en blanco pueden (en
  su mayoría) irrelevantes para el intérprete de Ruby, pero su uso adecuado
  es la clave para escribir código fácil de leer.

    ```Ruby
    sum = 1 + 2
    a, b = 1, 2
    1 > 2 ? true : false; puts 'Hi'
    [1, 2, 3].each { |e| puts e }
    ```

    La única excepción, con respecto a los operadores, es el operador exponente:

    ```Ruby
    # mal
    e = M * c ** 2

    # bien
    e = M * c**2
    ```

    `{` y `}` merecen una aclaración especial, ya que se utilizan
    para bloques y hash literales, así como las expresiones
    embebidas en strings. Dos estilos se consideran aceptables
    para los hash literales.

    ```Ruby
    # bien - espacio luego de { y antes de }
    { one: 1, two: 2 }

    # bien - sin espacio luego de { y antes de }
    {one: 1, two: 2}
    ```

    La primera variante es un poco más fácil de leer (y posiblemente
    más popular en la comunidad de Ruby en general). La segunda
    variante tiene la ventaja de tener diferenciación visual entre
    los bloques y los hash literales. Cualquiera que elijas -
    usalo de forma consistente.

    En cuanto a las expresiones embebidas, también hay dos formas
    aceptables:

    ```Ruby
    # bien - sin espacios
    "string#{expr}"

    # ok - podría decirse que es más legible
    "string#{ expr }"
    ```

    El primer estilo es extremadamente más popular y generalmente
    se aconseja que lo elijas. Por otro lado, el segundo es
    (posiblemente) un poco más legible. Al igual que con los hashes
    - escogé un estilo y usalo de forma consistente.

* Sin espacios luego de `(`, `[` o antes de `]`, `)`.

    ```Ruby
    some(arg).other
    [1, 2, 3].length
    ```

* Sin espacios luego de `!`.

    ```Ruby
    # mal
    ! something

    # bien
    !something
    ```

* Indenta `when` tan profundo como `case`. Sé que muchos no estarán
  de acuerdo con esto, pero es el estilo establecido tanto en "The
  Ruby Programming Language" y "Programming Ruby".

    ```Ruby
    # mal
    case
      when song.name == 'Misty'
        puts 'Not again!'
      when song.duration > 120
        puts 'Too long!'
      when Time.now.hour > 21
        puts "It's too late"
      else
        song.play
    end

    # bien
    case
    when song.name == 'Misty'
      puts 'Not again!'
    when song.duration > 120
      puts 'Too long!'
    when Time.now.hour > 21
      puts "It's too late"
    else
      song.play
    end
    ```

* Al asignar el resultado de una expresión condicional a una variable, conservá la alineación de su ramificación.

    ```Ruby
    # mal - bastante complejo
    kind = case year
    when 1850..1889 then 'Blues'
    when 1890..1909 then 'Ragtime'
    when 1910..1929 then 'New Orleans Jazz'
    when 1930..1939 then 'Swing'
    when 1940..1950 then 'Bebop'
    else 'Jazz'
    end

    result = if some_cond
      calc_something
    else
      calc_something_else
    end

    # bien - es aparente qué está pasando
    kind = case year
           when 1850..1889 then 'Blues'
           when 1890..1909 then 'Ragtime'
           when 1910..1929 then 'New Orleans Jazz'
           when 1930..1939 then 'Swing'
           when 1940..1950 then 'Bebop'
           else 'Jazz'
           end

    result = if some_cond
               calc_something
             else
               calc_something_else
             end

    # bien (y con un espacio más eficiente)
    kind =
      case year
      when 1850..1889 then 'Blues'
      when 1890..1909 then 'Ragtime'
      when 1910..1929 then 'New Orleans Jazz'
      when 1930..1939 then 'Swing'
      when 1940..1950 then 'Bebop'
      else 'Jazz'
      end

    result =
      if some_cond
        calc_something
      else
        calc_something_else
      end
    ```

* Usa líneas vacías entre definiciones de métodos y también para romper un método en
  párrafos lógicos internos.

    ```Ruby
    def some_method
      data = initialize(options)

      data.manipulate!

      data.result
    end

    def some_method
      result
    end
    ```

* Usa espacios alrededor del operador `=` cuando asignes valores predeterminados a los
  parámetros del método:

    ```Ruby
    # mal
    def some_method(arg1=:default, arg2=nil, arg3=[])
      # do something...
    end

    # bien
    def some_method(arg1 = :default, arg2 = nil, arg3 = [])
      # do something...
    end
    ```

    Mientras que varios libros de Ruby sugieren el primer estilo, el segundo es mucho
    más utilizado en la práctica (y hasta se podría decirse que es un poco más fácil
    de leer).

* Evita usar la contínuación de línea con '\' cuando no sea necesario. En la práctica, evita el uso de continuación de línea en cualquier caso, excepto para la concatenación de strings.

    ```Ruby
    # mal
    result = 1 - \
             2

    # bien (pero todavía se ve horrible)
    result = 1 \
             - 2

    long_string = 'First part of the long string' \
                  ' and second part of the long string'
    ```

* Al continuar una invocación de método encadenado en otra línea mantener el `.` en la segunda línea.

    ```Ruby
    # mal - es necesario leer la primer línea para entender la segunda línea
    one.two.three.
      four

    # bien - inmediatamente se ve qué está pasando en la segunda línea
    one.two.three
      .four
    ```

* Alinea los parámetros de una llamada a un método si ocupan más de una
  línea. Cuando se alinean los parámetros no es apropiado que sea con
  más indentación de lo debido, y utilizar un indentado único para las
  líneas luego del primer parámetro también es aceptable.

    ```Ruby
    # punto de partida (la línea es muy larga)
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com', from: 'us@example.com', subject: 'Important message', body: source.text)
    end

    # mal (doble indentado)
    def send_mail(source)
      Mailer.deliver(
          to: 'bob@example.com',
          from: 'us@example.com',
          subject: 'Important message',
          body: source.text)
    end

    # bien
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com',
                     from: 'us@example.com',
                     subject: 'Important message',
                     body: source.text)
    end

    # bien (indentado normal)
    def send_mail(source)
      Mailer.deliver(
        to: 'bob@example.com',
        from: 'us@example.com',
        subject: 'Important message',
        body: source.text
      )
    end
    ```

* Alinea los elementos de arrays literales que ocupen múltiples líneas.

    ```Ruby
    # mal - indentado simple
    menu_item = ["Spam", "Spam", "Spam", "Spam", "Spam", "Spam", "Spam", "Spam",
      "Baked beans", "Spam", "Spam", "Spam", "Spam", "Spam"]

    # bien
    menu_item = [
      "Spam", "Spam", "Spam", "Spam", "Spam", "Spam", "Spam", "Spam",
      "Baked beans", "Spam", "Spam", "Spam", "Spam", "Spam"
    ]

    # bien
    menu_item =
      ["Spam", "Spam", "Spam", "Spam", "Spam", "Spam", "Spam", "Spam",
       "Baked beans", "Spam", "Spam", "Spam", "Spam", "Spam"]
    ```

* Agrega guiones bajos para números literales grandes para mejorar su lectura.

    ```Ruby
    # mal - cuantos 0s hay ahi?
    num = 1000000

    # bien - mucho más fácil de leer por el cerebro humano
    num = 1_000_000
    ```

* Usa RDoc y sus convenciones para la documentación de APIs. No dejes
  una línea en blanco entre el bloque de comentario y el `def`.
* Limitá las líneas a 80 caracteres.
* Evita los espacios en blanco.
* No uses los comentarios de bloque. Ellos no pueden tener espacios en blanco
  antes y no son tan fáciles de entenderlos como comentarios.

    ```Ruby
    # mal
    == begin
    comment line
    another comment line
    == end

    # bien
    # comment line
    # another comment line
    ```

## Sintaxis

* Usa `::` sólo para referenciar constantes (esto incluye clases
y módulos) y construcciones (como `Array()` o `Nokogiri::HTML()`).
Nunca uses `::` para la invocación de métodos.

    ```Ruby
    # mal
    SomeClass::some_method
    some_object::some_method

    # bien
    SomeClass.some_method
    some_object.some_method
    SomeModule::SomeClass::SOME_CONST
    SomeModule::SomeClass()
    ```

* Usa `def` con paréntesis cuando tengas argumentos. Omite los
  paréntesis cuando el método no acepta ningún argumento.

     ```Ruby
     # mal
     def some_method()
       # body omitted
     end

     # bien
     def some_method
       # body omitted
     end

     # mal
     def some_method_with_arguments arg1, arg2
       # body omitted
     end

     # bien
     def some_method_with_arguments(arg1, arg2)
       # body omitted
     end
     ```

* Nunca uses `for`, a menos que sepas exactamente para qué lo usás. En su
  lugar deberías usar los iteradores la mayor parte del tiempo. `for` se
  debe implementar en forma de `each` (asi estás agregando un nivel de
  indirección), pero con una peculiaridad - `for` no introduce un nuevo
  scope (a diferencia de `each`) y las variables que se definen dentro
  de su bloque son visibles fuera del mismo.

    ```Ruby
    arr = [1, 2, 3]

    # mal
    for elem in arr do
      puts elem
    end

    # elem puede ser leída fuera del loop for
    elem #=> 3

    # bien
    arr.each { |elem| puts elem }

    # elem no puede ser leída fuera del bloque each
    elem #=> NameError: undefined local variable or method `elem'
    ```

* Nunca uses `then` para `if/unless` con multilíneas.

    ```Ruby
    # mal
    if some_condition then
      # body omitted
    end

    # bien
    if some_condition
      # body omitted
    end
    ```

* Siempre escribe la condición en la misma línea para los condicionales `if`/`unless` con multilíneas.

    ```Ruby
    # mal
    if
      some_condition
      do_something
      do_something_else
    end

    # bien
    if some_condition
      do_something
      do_something_else
    end
    ```

* Prefiere el operador ternario (`?:`) en lugar de las construcciones
  `if/then/else/end`. Es más común y obviamente más conciso.

    ```Ruby
    # mal
    result = if some_condition then something else something_else end

    # bien
    result = some_condition ? something : something_else
    ```

* Usa una expresión por fila por operador ternario. Esto también
  significa que los operadores ternarios no deben anidarse. Es
  preferible utilizar construcciones `if/else` en estos casos.

    ```Ruby
    # mal
    some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else

    # bien
    if some_condition
      nested_condition ? nested_something : nested_something_else
    else
      something_else
    end
    ```

* Nunca uses `if x: ...` - fue eliminado en Ruby 1.9. Usa el
  operador ternario en su lugar.

    ```Ruby
    # mal
    result = if some_condition: something else something_else end

    # bien
    result = some_condition ? something : something_else
    ```

* Nunca uses `if x; ...`. Usa el operador ternario en su lugar.

* Usa `when x then ...` para condicionales de una línea. La sintaxis
  alternativa `when x: ...` fue eliminada en Ruby 1.9.

* Nunca uses `when x; ...`. Mirá la regla anterior.

* Usa `!` en lugar de `not`.

    ```Ruby
    # mal - los paréntesis son necesarios por la precedencia de operador
    x = (not something)

    # bien
    x = !something
    ```

* Evita el uso de `!!`.

    ```Ruby
    # mal
    x = 'test'
    # obscure nil check
    if !!x
      # body omitted
    end

    x = false
    # double negation is useless on booleans
    !!x # => false

    # bien
    x = 'test'
    if !x.nil?
      # body omitted
    end
    ```

* Las palabras `and` y `or` están prohibidas. Simplemente no valen
  la pena. En su lugar, siempre usá `&&` y `||`.

    ```Ruby
    # mal
    # boolean expression
    if some_condition and some_other_condition
      do_something
    end

    # control flow
    document.saved? or document.save!

    # bien
    # boolean expression
    if some_condition && some_other_condition
      do_something
    end

    # control flow
    document.saved? || document.save!
    ```

* Evita usar `?:` (operador ternario) en multilíneas; en su lugar usá `if/unless`.

* Favorecé al uso del modificador `if/unless` cuando tengas que escribir en una línea.
  Otra buena alternativa es el uso del control de flujo con `&&/||`.

    ```Ruby
    # mal
    if some_condition
      do_something
    end

    # bien
    do_something if some_condition

    # otra buena opción
    some_condition && do_something
    ```

* Favorecé `unless` por sobre `if` para condiciones negativas (o control
  de flujo con `||`).

    ```Ruby
    # mal
    do_something if !some_condition

    # mal
    do_something if not some_condition

    # bien
    do_something unless some_condition

    # otra buena opción
    some_condition || do_something
    ```

* Nunca uses `unless` con `else`. Reescribe para que el caso positivo esté primero.

    ```Ruby
    # mal
    unless success?
      puts 'failure'
    else
      puts 'success'
    end

    # bien
    if success?
      puts 'success'
    else
      puts 'failure'
    end
    ```

* No uses paréntesis alrededor de la condición de `if/unless/while/until`.

    ```Ruby
    # mal
    if (x > 10)
      # body omitted
    end

    # bien
    if x > 10
      # body omitted
    end
    ```

* Nunca uses `while/until condition do` para un `while/until` multilínea.

    ```Ruby
    # mal
    while x > 5 do
      # body omitted
    end

    until x > 5 do
      # body omitted
    end

    # bien
    while x > 5
      # body omitted
    end

    until x > 5
      # body omitted
    end
    ```

* Favorecé el uso del modificador `while/until` cuando puedas escribir la
  comparación en una línea.

    ```Ruby
    # mal
    while some_condition
      do_something
    end

    # bien
    do_something while some_condition
    ```

* Favorecé `until` por sobre `while` para condiciones negativas.

    ```Ruby
    # mal
    do_something while !some_condition

    # bien
    do_something until some_condition
    ```

* Usa `Kernel#loop` con break en lugar de `begin/end/until` o `begin/end/while`
  para validar al final de cada loop.

   ```Ruby
   # mal
   begin
     puts val
     val += 1
   end while val < 0

   # bien
   loop do
     puts val
     val += 1
     break unless val < 0
   end
   ```

* Omite los paréntesis alrededor de los parámetros para los métodos
  que forman parte de un DSL interno (ejemplo: Rake, Rails, RSpec),
  métodos que tengan "keyword" status en Ruby (ejemplo: `attr_reader`,
  `puts`) y métodos accesores de atributos. Usa paréntesis alrededor
  de los argumentos de todas las otras llamadas a métodos.

    ```Ruby
    class Person
      attr_reader :name, :age

      # omitted
    end

    temperance = Person.new('Temperance', 30)
    temperance.name

    puts temperance.age

    x = Math.sin(y)
    array.delete(e)

    bowling.score.should == 0
    ```

* Omite las llaves externas alrededor de las opciones implícitas
  de un hash.

    ```Ruby
    # mal
    user.set({ name: 'John', age: 45, permissions: { read: true } })

    # bien
    user.set(name: 'John', age: 45, permissions: { read: true })
    ```

* Omite tanto las llaves externas como los paréntesis para métodos que formen
  parte de un DSL interno.

    ```Ruby
    class Person < ActiveRecord::Base
      # mal
      validates(:name, { presence: true, length: { within: 1..10 } })

      # bien
      validates :name, presence: true, length: { within: 1..10 }
    end
    ```

* Omite los paréntesis para llamadas a métodos sin argumentos.

    ```Ruby
    # mal
    Kernel.exit!()
    2.even?()
    fork()
    'test'.upcase()

    # bien
    Kernel.exit!
    2.even?
    fork
    'test'.upcase
    ```

* Elige `{...}` por sobre `do...end` para bloques de una línea. Evita
  el uso de `{...}` para bloques multilíneas (encadenamiento de multilínea
  siempre es horrible). Siempre usá `do...end` para "contorl de flujo" y
  "definiciones de métodos" (e.g. en Rakefiles y algunos DSLs). Evita usar
  `do...end` cuando estés encadenando métodos.

    ```Ruby
    names = ['Bozhidar', 'Steve', 'Sarah']

    # mal
    names.each do |name|
      puts name
    end

    # bien
    names.each { |name| puts name }

    # mal
    names.select do |name|
      name.start_with?('S')
    end.map { |name| name.upcase }

    # bien
    names.select { |name| name.start_with?('S') }.map { |name| name.upcase }
    ```

    Puede ser que algunas personas piensen que el encadenamiento en multilínea se vería bien con
    el uso de {...}, pero en realidad deberían preguntarse a sí mismos - es el código realmente
    legible y los contenidos de los bloques pueden ser extraídos con métodos elegantes?

* Evita usar `return` cuando no se requiera realizar control de flujo.

    ```Ruby
    # mal
    def some_method(some_arr)
      return some_arr.size
    end

    # bien
    def some_method(some_arr)
      some_arr.size
    end
    ```

* Evita usar `self` cuando no es necesario. (Solo se necesita cuando se llama a un accesor de escritura propio.)

    ```Ruby
    # mal
    def ready?
      if self.last_reviewed_at > self.last_updated_at
        self.worker.update(self.content, self.options)
        self.status = :in_progress
      end
      self.status == :verified
    end

    # bien
    def ready?
      if last_reviewed_at > last_updated_at
        worker.update(content, options)
        self.status = :in_progress
      end
      status == :verified
    end
    ```

* Como convención, evita oscurecer métodos con variables locales, excepto que ambos sean lo mismo.

    ```Ruby
    class Foo
      attr_accessor :options

      # ok
      def initialize(options)
        self.options = options
        # both options and self.options are equivalent here
      end

      # mal
      def do_something(options = {})
        unless options[:when] == :later
          output(self.options[:message])
        end
      end

      # bien
      def do_something(params = {})
        unless params[:when] == :later
          output(options[:message])
        end
      end
    end
    ```

* No uses el valor de retorno de `=` (asignación) en expresiones
  condicionales a menos que la asignación se encuentre entre paréntesis.
  Esta es una expresión bastante popular entre los Rubyistas que se
  refiere a veces como *asignación segura en condiciones*.

    ```Ruby
    # mal (+ una advertencia)
    if v = array.grep(/foo/)
      do_something(v)
      ...
    end

    # bien (MRI todavía se quejaría, per RuboCop no)
    if (v = array.grep(/foo/))
      do_something(v)
      ...
    end

    # bien
    v = array.grep(/foo/)
    if v
      do_something(v)
      ...
    end
    ```

* Usa `||=` libremente para inicializar variables.

    ```Ruby
    # set name to Bozhidar, only if it's nil or false
    name ||= 'Bozhidar'
    ```

* No uses `||=` para inicializar variables booleanas. (Considera qué
pasaría si el valor actual fuese `false`.)

    ```Ruby
    # mal - asignaría true a enabled aunque ya era false
    enabled ||= true

    # bien
    enabled = true if enabled.nil?
    ```

* Evita el uso explícito del operador de igualdad idéntica `===`.
  Como su nombre indica, está destinado a ser utilizado
  implícitamente por expresiones `case` y fuera de ellas provee
  código bastante confuso.

    ```Ruby
    # mal
    Array === something
    (1..100) === 7
    /something/ === some_string

    # bien
    something.is_a?(Array)
    (1..100).include?(7)
    some_string =~ /something/
    ```

* Evita usar variables especiales de tipo Perl (como `$:`, `$;`, etc.). Son bastante crípticas y su uso en cualquier lugar excepto scripts de una línea está desalentado. Usa los alias amigables para humanos proporcionados por la librería `English`.

    ```Ruby
    # mal
    $:.unshift File.dirname(__FILE__)

    # bien
    require 'English'
    $LOAD_PATH.unshift File.dirname(__FILE__)
    ```

* Nunca uses un espacio entre el nombre de un método y la apertura de un paréntesis.

    ```Ruby
    # mal
    f (3 + 2) + 1

    # bien
    f(3 + 2) + 1
    ```

* Si el primer argumento de un método comienza con un paréntesis abierto,
  siempre utilizá paréntesis en la invocación del método. Por ejemplo,
  escribe `f ((3 + 2) + 1)`.

* Siempre ejecutá el intérprete de Ruby con la opción `-w`, para que
avise si se olvida alguna de las reglas anteriores!

* Usa la nueva sintaxis de lambda literal para bloques de una sola línea.
  Usa el método `lambda` para bloques multilínea.

    ```Ruby
    # mal
    l = lambda { |a, b| a + b }
    l.call(1, 2)

    # correcto, pero se ve extremadamente incómodo
    l = ->(a, b) do
      tmp = a * 7
      tmp * b / 50
    end

    # bien
    l = ->(a, b) { a + b }
    l.call(1, 2)

    l = lambda do |a, b|
      tmp = a * 7
      tmp * b / 50
    end
    ```

* Elige `proc` por sobre `Proc.new`.

    ```Ruby
    # mal
    p = Proc.new { |n| puts n }

    # bien
    p = proc { |n| puts n }
    ```

* Elige `proc.call()` por sobre `proc[]` o `proc.()` tanto para lambdas y procs.

    ```Ruby
    # mal - se ve similar a un acceso de Enumeración
    l = ->(v) { puts v }
    l[1]

    # también mal - sintaxis no común
    l = ->(v) { puts v }
    l.(1)

    # bien
    l = ->(v) { puts v }
    l.call(1)
    ```

* Usa `_` para los parámetros sin usar de bloques.

    ```Ruby
    # mal
    result = hash.map { |k, v| v + 1 }

    # bien
    result = hash.map { |_, v| v + 1 }
    ```

* Usa `$stdout/$stderr/$stdin` en lugar de
  `STDOUT/STDERR/STDIN`. `STDOUT/STDERR/STDIN` son constantes, y
  mientras que puedas reasignar constantes (posiblemente para redirigr
  un proceso) en Ruby, vas a tener una advertencia del intérprete si
  hacés eso.

* Usa `warn` en lugar de `$stderr.puts`. Aparte de ser más conciso y
claro, `warn` te permite suprimir advertencias si lo necesitás
(seteando el nivel de advertencia a 0 via `-W0`).

* Elige usar `sprintf` y su alias `format` por sobre el método
  críptico `String#%`.

    ```Ruby
    # mal
    '%d %d' % [20, 10]
    # => '20 10'

    # bien
    sprintf('%d %d', 20, 10)
    # => '20 10'

    # bien
    sprintf('%{first} %{second}', first: 20, second: 10)
    # => '20 10'

    format('%d %d', 20, 10)
    # => '20 10'

    # bien
    format('%{first} %{second}', first: 20, second: 10)
    # => '20 10'
    ```

* Elige el uso de `Array#join` por sobre el críptico `Array#*` con
  un argumento string.

    ```Ruby
    # mal
    %w(one two three) * ', '
    # => 'one, two, three'

    # bien
    %w(one two three).join(', ')
    # => 'one, two, three'
    ```

* Usa `[*var]` o `Array()` en lugar de la verificación explícita `Array`,
  cuando trabajes con una variable que quieras tratar como un Array, pero
  no estás seguro que sea un array.

    ```Ruby
    # mal
    paths = [paths] unless paths.is_a? Array
    paths.each { |path| do_something(path) }

    # bien
    [*paths].each { |path| do_something(path) }

    # bien (y un poquito más legible)
    Array(paths).each { |path| do_something(path) }
    ```

* Usa rangos o `Comparable#between?` en lugar de una comparación lógica
  compleja cuando sea posible.

    ```Ruby
    # mal
    do_something if x >= 1000 && x <= 2000

    # bien
    do_something if (1000..2000).include?(x)

    # bien
    do_something if x.between?(1000, 2000)
    ```

* Elige el uso de métodos subyacentes en lugar de las comparaciones
  explícitas con `==`. Comparaciones numéricas están OK.

    ```Ruby
    # mal
    if x % 2 == 0
    end

    if x % 2 == 1
    end

    if x == nil
    end

    # bien
    if x.even?
    end

    if x.odd?
    end

    if x.nil?
    end

    if x.zero?
    end

    if x == 0
    end
    ```

* Evita el uso de bloques `BEGIN`.

* Nunca uses bloques `END`. En su lugar usá `Kernel#at_exit`.

    ```ruby
    # mal

    END { puts 'Goodbye!' }

    # bien

    at_exit { puts 'Goodbye!' }
    ```

* Evita el uso de flip-flops.

* Evita el uso de condicionales anidados para control de flujo.
  Elige una cláusula de guardia (guard clause) cuando puedas afirmar datos inválidos.
  Una cláusula de guardia es un condicional al principio de una función que trata de
  salir de ella tan pronto como pueda.

    ```Ruby
    # mal
      def compute_thing(thing)
        if thing[:foo]
          update_with_bar(thing)
          if thing[:foo][:bar]
            partial_compute(thing)
          else
            re_compute(thing)
          end
        end
      end

    # bien
      def compute_thing(thing)
        return unless thing[:foo]
        update_with_bar(thing[:foo])
        return re_compute(thing) unless thing[:foo][:bar]
        partial_compute(thing)
      end
    ```

## Nombres

> Las únicas dificultades reales en programación son invalidación de
> caché y nombrar cosas. <br/>
> -- Phil Karlton

* Nombres de referencia en Inglés.

    ```Ruby
    # mal - referencia no está utilizando caracteres ascii
    заплата = 1_000

    # mal - la referencia es una palabra búlgara, escrita con caracteres latinos (en lugar de Cirílico)
    zaplata = 1_000

    # bien
    salary = 1_000
    ```

* Usa `snake_case` para los símbolos, métodos y variables.

    ```Ruby
    # mal
    :'some symbol'
    :SomeSymbol
    :someSymbol

    someVar = 5

    def someMethod
      ...
    end

    def SomeMethod
     ...
    end

    # bien
    :some_symbol

    def some_method
      ...
    end
    ```

* Usa `CamelCase` para clases y módulos.  (Mantené en mayúsculas los
  acrónimos como HTTP, RFC, XML.)

    ```Ruby
    # mal
    class Someclass
      ...
    end

    class Some_Class
      ...
    end

    class SomeXml
      ...
    end

    # bien
    class SomeClass
      ...
    end

    class SomeXML
      ...
    end
    ```

* Usa `SCREAMING_SNAKE_CASE` para las constantes.

    ```Ruby
    # mal
    SomeConst = 5

    # bien
    SOME_CONST = 5
    ```

* Los nombres de los métodos que afirman (métodos que devuelven un
  valor booleano) deben terminar con un signo de pregunta.
  (ejemplo: `Array#empty?`). Los métodos que no devuelvan un booleano,
  no deben terminar con un signo de pregunta.
* Los nombres de métodos potencialmente *peligrosos* (ejemplo: métodos
  que modifican `self` o los argumentos, `exit!` - no ejecuta
  finalizadores como lo hace `exit`, etc.) deben terminar con un signo
  de exclamación solo si existe una versión segura de ese método
  *peligroso*.

    ```Ruby
    # mal - no hay ningún método 'seguro' que se llame igual
    class Person
      def update!
      end
    end

    # bien
    class Person
      def update
      end
    end

    # bien
    class Person
      def update!
      end

      def update
      end
    end
    ```

* Define un método non-bang (seguro) en relación al método bang
  (peligroso) si es posible.

    ```Ruby
    class Array
      def flatten_once!
        res = []

        each do |e|
          [*e].each { |f| res << f }
        end

        replace(res)
      end

      def flatten_once
        dup.flatten_once!
      end
    end
    ```

* Cuando se usa `reduce` con bloques chicos, nombrá los argumentos
  `|a, e|` (accumulator, element).
* Cuando definas operadores binarios, nombrá el argumento como `other`
  (`<<` y `[]` son excepciones a la regla, ya que su semántica es
  diferente).

    ```Ruby
    def +(other)
      # body omitted
    end
    ```

* Elige `map` por sobre `collect`, `find` por sobre `detecet`, `select`
  por sobre `find_all`, `reduce` por sobre `inject` y `size` por sobre
  `length`. No es un requerimiento difícil; si el uso de alias realza
  la legibilidad, está bien usarlos. Los métodos de rima son heredados
  de Smalltalk y no son comunes en otros lenguajes de programación. La
  razón para usar `select` por sobre `find_all` es porque va muy bien
  junto con `reject` y su nombre es bastante auto-explicativo.

* Usa `flat_map` en lugar de `map` + `flatten`.
  Esto no se aplica a los arrays con profundidad mayor a 2, ejemplo:
  si tenemos `users.first.songs == ['a', ['b','c']]`, entonces hay que
  usar `map + flatten` en lugar de `flat_map`. `flat_map` achata el
  array a 1 nivel, mientras que `flatten` achata el array del todo.

    ```Ruby
    # mal
    all_songs = users.map(&:songs).flatten.uniq

    # bien
    all_songs = users.flat_map(&:songs).uniq
    ```

* Usa `reverse_each` en lugar de `reverse.each`. `reverse_each` no
  realiza una asignación de array y eso es algo bueno.


    ```Ruby
    # mal
    array.reverse.each { ... }

    # bien
    array.reverse_each { ... }
    ```

## Comentarios

> El buen código es su mejor documentación. Cuando estés a punto de
> agregar un comentario, preguntate: "¿Cómo puedo mejorar el código
> para que no sea necesario este comentario?" Mejorá el código y luego
> documentalo para que hacerlo aún más claro. <br/>
> -- Steve McConnell

* Escribe código autodocumentado e ignorá el resto de esta sección. En serio!
* Escribe tus comentarios en Inglés para evitar problemas con los caracteres
  especiales.
* Usa un espacio entre el primer caracter `#` del comentario y el texto
  propio del comentario.
* Los comentarios que son más largos que una palabra son capitalizados y usan
  puntuación. Usa [un espacio](http://es.wikipedia.org/wiki/Espacio_entre_oraciones) luego de los puntos.
* Evita comentarios supérfluos.

    ```Ruby
    # mal
    counter += 1 # Increments counter by one.
    ```

* Mantené los comentarios existentes actualizados. Un comentario viejo es peor
que no utilizar comentarios.

> El buen código es como un buen chiste - no necesita explicación <br/>
> -- Russ Olsen

* Evita escribir comentarios para explicar código malo. Refactorizá el código
  para hacerlo más auto-explicativo. ()
* Avoid writing comments to explain bad code. Refactor the code to
  make it self-explanatory. (Hacé o no hacé - no hay intentar. --Yoda)

### Apuntes de Comentarios

* Generalmente los apuntes se tienen que escribir inmediatamente en la primer
  línea encima del código a comentar.
* La palabra clave del apunte es seguida de dos puntos y un espacio, y luego
  una nota que describe el problema.
* Si se son necesarias varias líneas para describir el problema, las líneas
  subsiguientes tienen que tener una sangría de dos espacios después del `#`.

    ```Ruby
    def bar
      # FIXME: This has crashed occasionally since v3.2.1. It may
      #   be related to the BarBazUtil upgrade.
      baz(:quux)
    end
    ```

* En los casos donde el problema es tan obvio que cualquier documentación
  fuese redundante, los apuntes se pueden dejar al final de esa línea, sin
  ninguna nota. Su uso debe ser la excepción y no la regla.

    ```Ruby
    def bar
      sleep 100 # OPTIMIZE
    end
    ```

* Usa `TODO` para resaltar funcionalidades faltantes o funcionalidades que
  deben agregarse más adelante.
* Usa `FIXME` para resaltar código roto que necesita ser arreglado.
* Usa `OPTIMIZE` para resaltar código lento o ineficiente que pueda causar
  problemas de performance.
* Usa `HACK` para resaltar código que utilice prácticas cuestionables que
  no se vean bien y que debería ser refactorizado lo antes posible.
* Usa `REVIEW` para resaltar cualquier cosa que debe ser revisada para
  confirmar que está funcionando como debería. Por ejemplo: `REVIEW: Are
  we sure this is how the client does X currently?`
* Usa otra palabra como apunte si sentís que sea apropiado, pero asegurate
  de documentarlas en el `README` de tu proyecto o similar.

## Clases y Módulos

* Usa una estructura coherente para definicior tu clase.

    ```Ruby
    class Person
      # extend and include go first
      extend SomeModule
      include AnotherModule

      # constants are next
      SOME_CONSTANT = 20

      # afterwards we have attribute macros
      attr_reader :name

      # followed by other macros (if any)
      validates :name

      # public class methods are next in line
      def self.some_method
      end

      # followed by public instance methods
      def some_method
      end

      # protected and private methods are grouped near the end
      protected

      def some_protected_method
      end

      private

      def some_private_method
      end
    end
    ```

* Elige módules de clases únicamente con métodos de clases. Las clases
  deben ser utilizadas únicamente cuando tiene sentido crear instancias
  fuera de ellos.

    ```Ruby
    # mal
    class SomeClass
      def self.some_method
        # body omitted
      end

      def self.some_other_method
      end
    end

    # bien
    module SomeClass
      module_function

      def some_method
        # body omitted
      end

      def some_other_method
      end
    end
    ```

* Elige el uso de `module_function` por sobre `extend self` cuando
  quieras cambiar los métodos de instancia de un módulo en un método
  de clase.

    ```Ruby
    # mal
    module Utilities
      extend self

      def parse_something(string)
        # do stuff here
      end

      def other_utility_method(number, string)
        # do some more stuff
      end
    end

    # bien
    module Utilities
      module_function

      def parse_something(string)
        # do stuff here
      end

      def other_utility_method(number, string)
        # do some more stuff
      end
    end
    ```

* Cuando diseñes jerarquías de clases, asegurate de que se ajuseten al
  [Principio de Sustitución de Liskov](http://es.wikipedia.org/wiki/Principio_de_sustituci%C3%B3n_de_Liskov).
* Tratá de hacer tus clases tan
  [SOLID](http://es.wikipedia.org/wiki/SOLID_(object-oriented_design))as
  como sea posible.
* Siempre proporcioná un método `to_s` para clases que representen
  objetos de dominio.

    ```Ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def to_s
        "#{@first_name} #{@last_name}"
      end
    end
    ```

* Usa la familia de funciones `attr` para definir accesores triviales
o mutators.

    ```Ruby
    # mal
    class Person
      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def first_name
        @first_name
      end

      def last_name
        @last_name
      end
    end

    # bien
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end
    ```

* Evita el uso de `attr`. En su lugar usá `attr_reader` y `attr_accessor`.

    ```Ruby
    # mal - crea un único accesor de atributo (deprecado en 1.9)
    attr :something, true
    attr :one, :two, :three # behaves as attr_reader

    # bien
    attr_accessor :something
    attr_reader :one, :two, :three
    ```

* Considera usar `Struct.new`, el cual define por vos los accesores
triviales, constructor y operadores de comparación.

    ```Ruby
    # bien
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end

    # mejor
    Person = Struct.new(:first_name, :last_name) do
    end
    ````

* No extiendas un `Struct.new` - ya de por si es una clase nueva. Extendiéndolo introduce un nivel de clase superfluo y también puede introducir errores extraños si el archivo es requerido múltiples veces.

* Considera agregar un método factory para proveer más formas sensibles
de crear instancias de una clase en particular.

    ```Ruby
    class Person
      def self.create(options_hash)
        # body omitted
      end
    end
    ```

* Prefiere [duck-typing](http://es.wikipedia.org/wiki/Duck_typing) en lugar de herencia.

    ```Ruby
    # mal
    class Animal
      # abstract method
      def speak
      end
    end

    # extend superclass
    class Duck < Animal
      def speak
        puts 'Quack! Quack'
      end
    end

    # extend superclass
    class Dog < Animal
      def speak
        puts 'Bau! Bau!'
      end
    end

    # bien
    class Duck
      def speak
        puts 'Quack! Quack'
      end
    end

    class Dog
      def speak
        puts 'Bau! Bau!'
      end
    end
    ```

* Evita el uso de variables de clas (`@@`) debido a sus comportamientos
"sucios" en la herencia.

    ```Ruby
    class Parent
      @@class_var = 'parent'

      def self.print_class_var
        puts @@class_var
      end
    end

    class Child < Parent
      @@class_var = 'child'
    end

    Parent.print_class_var # => will print "child"
    ```

    Como puedas ver todas las clases en una jerarquía de clases en realidad
    comparten una variable de clase. Por lo general las variables de instancia
    de clase deben ser preferidas a las variables de clase.

* Asigna niveles de visibilidad adecuados para los métodos (`private`,
  `protected`) de acuerdo con su correcto uso. No vayas por ahi dejando
  todo `public` (que es el estado predeterminado). Después de todo
  ahora estamos programando en *Ruby*, no en *Python*.
* Indenta las palabas `public`, `protected`, y `private` tanto como los
  métodos a los que se aplican. Dejá una línea en blanco antes y después
  del modificador de visibilidad, en orden de enfatizar que eso aplica a
  todos los métodos que se encuentran debajo.

    ```Ruby
    class SomeClass
      def public_method
        # ...
      end

      private

      def private_method
        # ...
      end

      def another_private_method
        # ...
      end
    end
    ```

* Usa `def self.method` para definir métodos singleton. Eso hace el
  código más fácil de refactorizar, debido a que el nombre de la clase
  no está repetido.

    ```Ruby
    class TestClass
      # mal
      def TestClass.some_method
        # body omitted
      end

      # bien
      def self.some_other_method
        # body omitted
      end

      # También es posible y conveniente cuando
      # quieras definir muchos métodos singleton.
      class << self
        def first_method
          # body omitted
        end

        def second_method_etc
          # body omitted
        end
      end
    end
    ```

## Excepciones

* Señaliza las excepciones utilizando el método `fail`. Usa `raise`
  solo cuando quieras atrapar una excepción y quieras volver a
  llamarlo (porque no está fallando, sino que está lanzando una
  excepción de forma explícita y a propósito).

    ```Ruby
    begin
      fail 'Oops'
    rescue => error
      raise if error.message != 'Oops'
    end
    ```

* No especifiques explícitamente `RuntimeError` en la versión de dos argumentos de
  `fail/raise`.

    ```Ruby
    # mal
    fail RuntimeError, 'message'

    # bien - señaliza un RuntimeError por defecto
    fail 'message'
    ```

* Elige suministrar una clase de excepción y un mensaje como dos
  argumentos separados para `fail/raise`, en lugar de una instancia
  de excepción.

    ```Ruby
    # mal
    fail SomeException.new('message')
    # No hay una forma de hacer `fail SomeException.new('message'), backtrace`.

    # bien
    fail SomeException, 'message'
    # Consistente con `fail SomeException, 'message', backtrace`.
    ```

* Nunca retornes desde un bloque `ensure`. Si retornás de forma explícita
  desde un método dentro de un bloque `ensure`, el retorno va a tomar
  precedente sobre cualquier excepción que sea llamada, y el método va a
  retornar como si ninguna excepción hubiera sido llamada. De hecho, la
  excepción va a ser desestimada en silencio.

    ```Ruby
    def foo
      begin
        fail
      ensure
        return 'very bad idea'
      end
    end
    ```

* Usa *bloques implícitos de begin* siempre que sea posible.

    ```Ruby
    # mal
    def foo
      begin
        # main logic goes here
      rescue
        # failure handling goes here
      end
    end

    # bien
    def foo
      # main logic goes here
    rescue
      # failure handling goes here
    end
    ```

* Mitigá la proliferación de bloques `begin` utilizando
  *métodos de contingencia* (un término elegido por Avdi Grimm).

    ```Ruby
    # mal
    begin
      something_that_might_fail
    rescue IOError
      # handle IOError
    end

    begin
      something_else_that_might_fail
    rescue IOError
      # handle IOError
    end

    # bien
    def with_io_error_handling
       yield
    rescue IOError
      # handle IOError
    end

    with_io_error_handling { something_that_might_fail }

    with_io_error_handling { something_else_that_might_fail }
    ```

* No suprimas las excepciones.

    ```Ruby
    # mal
    begin
      # an exception occurs here
    rescue SomeError
      # the rescue clause does absolutely nothing
    end

    # mal
    do_something rescue nil
    ```

* Evita usar `rescue` en su forma de modificador.

    ```Ruby
    # mal - esto atrapa una excepción de la clase StandardError y sus clases hijas
    read_file rescue handle_error($!)

    # bien - esto atrapa solo las excepciones de la clase Errno::ENOENT y sus clases hijas
    def foo
      read_file
    rescue Errno::ENOENT => ex
      handle_error(ex)
    end
    ```


* No uses excepciones para control de flujo.

    ```Ruby
    # mal
    begin
      n / d
    rescue ZeroDivisionError
      puts 'Cannot divide by 0!'
    end

    # bien
    if d.zero?
      puts 'Cannot divide by 0!'
    else
      n / d
    end
    ```

* Evita rescatar la clase `Exception`. Esto va a atrapar la señal y va a
  llamar a `exit`, siendo necesario que pases `kill -9` al proceso.

    ```Ruby
    # mal
    begin
      # calls to exit and kill signals will be caught (except kill -9)
      exit
    rescue Exception
      puts "you didn't really want to exit, right?"
      # exception handling
    end

    # bien
    begin
      # a blind rescue rescues from StandardError, not Exception as many
      # programmers assume.
    rescue => e
      # exception handling
    end

    # también está bien
    begin
      # an exception occurs here

    rescue StandardError => e
      # exception handling
    end

    ```

* Escribe excepciones más específicas primero, de otra forma nunca
  van a poder ser atrapadas.

    ```Ruby
    # mal
    begin
      # some code
    rescue Exception => e
      # some handling
    rescue StandardError => e
      # some handling
    end

    # bien
    begin
      # some code
    rescue StandardError => e
      # some handling
    rescue Exception => e
      # some handling
    end
    ```

* Cierra recursos externos abiertos por tu programa en un bloque
ensure.

    ```Ruby
    f = File.open('testfile')
    begin
      # .. process
    rescue
      # .. handle error
    ensure
      f.close unless f.nil?
    end
    ```

* Prefiere el uso de excepciones de la standard library en lugar
de crear nuevas clases de excepciones.

## Colecciones

* Prefiere el uso de la notación para arrays literales y creación de hashes
(excepto que necesites pasar parámetros a sus constructores).

    ```Ruby
    # mal
    arr = Array.new
    hash = Hash.new

    # bien
    arr = []
    hash = {}
    ```

* Prefiere usar `%w` en lugar de la sintaxis array literal cuando
necesites un array de palabras (strings no-vacías sin espacios ni
caracteres espaciles en cada uno). Aplicá esta regla solo en los arrays
de dos o más elementos.

    ```Ruby
    # mal
    STATES = ['draft', 'open', 'closed']

    # bien
    STATES = %w(draft open closed)
    ```

* Prefiere `%i` en lugar de la sintaxis de array literal cuando
necesites un array de símbolos (y no necesitás mantener compatibilidad
con Ruby 1.9). Aplicá esta regla sólo para arrays con dos o más
elementos.

    ```Ruby
    # mal
    STATES = [:draft, :open, :closed]

    # bien
    STATES = %i(draft open closed)
    ```

* Evita la creación de grandes espacios en arrays.

    ```Ruby
    arr = []
    arr[100] = 1 # y así tenés un array con un montón de nils
    ```

* Cuando estés accediendo al primer o último elmento de un array, preferí
usar `first` o `last` en lugar de `[0]` o `[-1]`.

* Usa `Set` en lugar de `Array` cuando estés tratando con elementos
  únicos. `Set` implementa una colección de valores desordenados sin
  duplicados. Esto es un híbrido de la vacilidad interoperacional
  intuitiva de `Array`, y velocidad de lectura de `Hash`.
* Prefiere símbolos en lugar de strings y hash keys.

    ```Ruby
    # mal
    hash = { 'one' => 1, 'two' => 2, 'three' => 3 }

    # bien
    hash = { one: 1, two: 2, three: 3 }
    ```

* Evita el uso de objetos mutables como hash keys.
* Usa la sintaxis de hash literal cuando tus hash keys sean símbolos.

    ```Ruby
    # mal
    hash = { :one => 1, :two => 2, :three => 3 }

    # bien
    hash = { one: 1, two: 2, three: 3 }
    ```

* Usa `Hash#key?` en lugar de `Hash#has_key?` y `Hash#value?` en lugar de
  `Hash#has_value?`. Como dice Matz
  [aquí](http://blade.nagaokaut.ac.jp/cgi-bin/scat.rb/ruby/ruby-core/43765),
  Las funciones largas están consideradas deprecadas.

    ```Ruby
    # mal
    hash.has_key?(:test)
    hash.has_value?(value)

    # bien
    hash.key?(:test)
    hash.value?(value)
    ```

* Usa `Hash#fetch` cuando estés tratando con hash keys que deben estar
presentes.

    ```Ruby
    heroes = { batman: 'Bruce Wayne', superman: 'Clark Kent' }
    # mal - si generamos un error, puede que no lo veamos rápido
    heroes[:batman] # => "Bruce Wayne"
    heroes[:supermann] # => nil

    # bien - fetch genera un KeyError, haciendo obvios los problemas
    heroes.fetch(:supermann)
    ```

* Utiliza valores por defecto para hash keys usando `Hash#fetch`, opuesto a generar tu
propia lógica.

   ```Ruby
   batman = { name: 'Bruce Wayne', is_evil: false }

   # mal - si solo usamos el operador || y tenemos un error medio falso, no vamos a tener el resultado esperado
   batman[:is_evil] || true # => true

   # bien - fetch trabaja mejor con valores medio falsos
   batman.fetch(:is_evil, true) # => false
   ```

* Prefiere el uso de bloques en lugar del formato por defecto de `Hash#fetch`.

   ```Ruby
   batman = { name: 'Bruce Wayne' }

   # mal - si usamos el valor por defecto, se va a evaluar en el momento
   # por lo que vamos a relantizar el programa, si se utiliza múltiples veces
   batman.fetch(:powers, get_batman_powers) # get_batman_powers is an expensive call

   # bien - los bloques se evalúan sin bloquer el proceso, asi solo se llama en caso de una excepción KeyError
   batman.fetch(:powers) { get_batman_powers }
   ```

* Confiá en el hecho de que desde Ruby 1.9 los hashes están ordenados.
* Nunca modifiques una colección mientras la estés recorriendo.

## Strings

* Prefiere interpolación de strings en lugar de concatenación de strings:

    ```Ruby
    # mal
    email_with_name = user.name + ' <' + user.email + '>'

    # bien
    email_with_name = "#{user.name} <#{user.email}>"
    ```

* Considera el uso de interpolación de string con espacio. Hace que sea más claro
  para separar el código del string.

    ```Ruby
    "#{ user.last_name }, #{ user.first_name }"
    ```

* Prefiere escribir los con una sola comilla cuando no tengás la necesidad de
  realizar interpolación o usar símbolos especiales como `\t`, `\n`, `'`, etc.

    ```Ruby
    # mal
    name = "Bozhidar"

    # bien
    name = 'Bozhidar'
    ```

* No uses el caracter literal de sintaxis `?x`. Desde Ruby 1.0 esto
  se hizo redundante - `?x` se interpreta como `'x'` (un string con
  solo un caracter dentro).

    ```Ruby
    # mal
    char = ?c

    # bien
    char = 'c'
    ```

* No dejes de usar `{}` alrededor de las variables de instancia o
  globales, siendo interpolados dentro de un string.

    ```Ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      # mal - válido, pero raro
      def to_s
        "#@first_name #@last_name"
      end

      # bien
      def to_s
        "#{@first_name} #{@last_name}"
      end
    end

    $global = 0
    # mal
    puts "$global = #$global"

    # bien
    puts "$global = #{$global}"
    ```

* Evita usar `String#+` cuando necesites construir un pedazo grande de datos.
  En su lugar usá `String#<<`. Concatenación muta la instancia del string en el
  lugar y siempre es más rápido que `String#+`, el cual crea un grupo de nuevos
  objetos de strings.

    ```Ruby
    # bien y rápido además
    html = ''
    html << '<h1>Page title</h1>'

    paragraphs.each do |paragraph|
      html << "<p>#{paragraph}</p>"
    end
    ```

* Cuando estés usando heredocs para strings multi-línea no te olvides
  del hecho de que ellos necesitan espacios en blanco. Es una buena
  práctica utilizar algo de margen basado en cómo hay que recortar
  el espacio en blanco excesivo.

    ```Ruby
    code = <<-END.gsub(/^\s+\|/, '')
      |def test
      |  some_method
      |  other_method
      |end
    END
    #=> "def test\n  some_method\n  other_method\nend\n"
    ```

## Expresiones Regulares

> Algunas personas, cuando se encuentran un problema, piensan
> "Ya se, voy a usar expresiones regulares." Ahora tienen dos problemas.<br/>
> -- Jamie Zawinski

* No uses expresiones regulares si solo necesitás buscar texto plano en un string:
  `string['text']`
* Para construcciones simples puedas usar regexp directamente a través de un índice de string.

    ```Ruby
    match = string[/regexp/]             # get content of matched regexp
    first_group = string[/text(grp)/, 1] # get content of captured group
    string[/text (grp)/, 1] = 'replace'  # string => 'text replace'
    ```

* Usa grupos que no capturen código cuando no uses resultados capturados con paréntesis.

    ```Ruby
    /(first|second)/   # mal
    /(?:first|second)/ # bien
    ```

* No uses las variables crípticas de Perl, que denoten las pocisiones de los resultados de regex
  (`$1`, `$2`, etc). En su lugar usá `Regexp.last_match[n]`.

    ```Ruby
    /(regexp)/ =~ string
    ...

    # mal
    process $1

    # bien
    process Regexp.last_match[1]
    ```


* Evita usar grupos numerados, ya que puede ser difícil de decir qué contienen. En su lugar
  deben usarse grupos con nombre.

    ```Ruby
    # mal
    /(regexp)/ =~ string
    ...
    process Regexp.last_match[1]

    # bien
    /(?<meaningful_var>regexp)/ =~ string
    ...
    process meaningful_var
    ```

* Clases de caracteres únicamente tienen caracteres especiales que te deberían importar:
  `^`, `-`, `\`, `]`, por lo que no debes escapar `.` o llaves en `[]`.

* Tené cuidado con `^` y `$`, ya que ellos se igualan con el inicio/final de la línea,
  no el final del string. Si querés igualar el string completo usá: `\A` y `\z` (no
  confundir con `\Z` el cual es el equivalente de `/\n?\z/`).

    ```Ruby
    string = "some injection\nusername"
    string[/^username$/]   # matches
    string[/\Ausername\z/] # don't match
    ```

* Usa el modificador `x` para regexps complejos. Esto los hace más legibles y
  vas a poder agregar mejores comentarios. Pero tené cuidado, que los espacios
  son ignorados.

    ```Ruby
    regexp = %r{
      start         # some text
      \s            # white space char
      (group)       # first group
      (?:alt1|alt2) # some alternation
      end
    }x
    ```

* Para cambios complejos se pueden usar `sub`/`gsub` con un bloque o un hash.

## Porcentajes Literales

* Usa `%()` (es un alias para `%Q`) para un string de una línea, el cual requiere tanto
  interpolación y uso de comillas dobles. Para strings multi-línea, es preferible usar heredocs.

    ```Ruby
    # mal (no necesita interpolación)
    %(<div class="text">Some text</div>)
    # should be '<div class="text">Some text</div>'

    # mal (no tiene comillas dobles)
    %(This is #{quality} style)
    # Debería ser "This is #{quality} style"

    # mal (múltiples líneas)
    %(<div>\n<span class="big">#{exclamation}</span>\n</div>)
    # debería ser un heredoc.

    # bien (requiere interpolación, tiene comillas, y es una sola línea)
    %(<tr><td class="name">#{name}</td>)
    ```

* Evita `%q`, excepto que tengas un string con `'` y `"` dentro.
  Los strings literales son más legibles y deberían ser elegidos,
  excepto que tengamos que escapar un montón de caracteres internos.

    ```Ruby
    # mal
    name = %q(Bruce Wayne)
    time = %q(8 o'clock)
    question = %q("What did you say?")

    # bien
    name = 'Bruce Wayne'
    time = "8 o'clock"
    question = '"What did you say?"'
    ```

* Usa `%r` solo para expresiones regulares que igualen *a más de un* caracter `/`.

    ```Ruby
    # mal
    %r(\s+)

    # todavía está mal
    %r(^/(.*)$)
    # debería ser /^\/(.*)$/

    # bien
    %r(^/blog/2011/(.*)$)
    ```

* Evita el uso de `%x`, excepto que estés invocando un comando con comillas contrarias (que es bastante inusual).

    ```Ruby
    # mal
    date = %x(date)

    # bien
    date = `date`
    echo = %x(echo `date`)
    ```

* Evita el uso de `%s`. Parece que la comunidad decidió que `:"some string"`
  es la forma preferida para crear un símbolo con espacios dentro.

* Prefiere `()` como delimitadores para todos los literales `%`, excepto
  para `%r`. Ya que las llaves aparecen seguido dentro de las expresiones
  regulares en varios escenarios, va a ser menos común que aparezca el
  caracter `{` y va a ser una mejor elección para usar como delimitador,
  dependiendo en el contenido de la regexp.

    ```Ruby
    # mal
    %w[one two three]
    %q{"Test's king!", John said.}

    # bien
    %w(one two three)
    %q("Test's king!", John said.)
    ```

## Metaprogramación

* Evita metaprogramación innecesaria.

* No hagas lío con las clases core cuando estés escribiendo librerías.
  (No parchees como un mono.)

* La forma de bloque de `class_eval` es preferible en forma de interpolación de string.
  - cuando uses la forma de interpolación de string, siempre usá `__FILE__` y `__LINE__`,
  asi la búsqueda de código tiene sentido:

    ```ruby
    class_eval 'def use_relative_model_naming?; true; end', __FILE__, __LINE__
    ```

  - `define_method` es mejor que `class_eval{ def ... }`

* Cuando uses `class_eval` (u otro `eval`) con interpolación de string, agregá un bloque de comentario que muestra su apariencia si está interpolada (una práctica que aprendí con el código de Rails):

    ```ruby
    # from activesupport/lib/active_support/core_ext/string/output_safety.rb
    UNSAFE_STRING_METHODS.each do |unsafe_method|
      if 'String'.respond_to?(unsafe_method)
        class_eval <<-EOT, __FILE__, __LINE__ + 1
          def #{unsafe_method}(*args, &block)       # def capitalize(*args, &block)
            to_str.#{unsafe_method}(*args, &block)  #   to_str.capitalize(*args, &block)
          end                                       # end

          def #{unsafe_method}!(*args)              # def capitalize!(*args)
            @dirty = true                           #   @dirty = true
            super                                   #   super
          end                                       # end
        EOT
      end
    end
    ```

* Evita usar `method_missing` para metaprogramación, ya que hace dificil leer el código, el comportamiento no está listado en `#methods`, y las llamadas a métodos mal escritas pueden funcionar silienciosamente, ejemplo: `nukes.launch_state = false`. En su lugar, considerá usar delegation, proxy o `define_method`. Si es necesario, usá `method_missing`:

  - Está seguro de [también definir `respond_to_missing?`](http://blog.marc-andre.ca/2010/11/methodmissing-politely.html)
  - Solo atrapá métodos con un prefix bien definido, como `find_by_*` -- hacé tu código lo más asertivo posible.
  - Llamá a `super` al final de tu definición
  - Delegá en métodos asertivos, no mágicos:

    ```ruby
    # mal
    def method_missing?(meth, *args, &block)
      if /^find_by_(?<prop>.*)/ =~ meth
        # ... lots of code to do a find_by
      else
        super
      end
    end

    # bien
    def method_missing?(meth, *args, &block)
      if /^find_by_(?<prop>.*)/ =~ meth
        find_by(prop, *args, &block)
      else
        super
      end
    end

    # best of all, though, would to define_method as each findable attribute is declared
    ```

## Varios

* Escribe código seguro con `ruby -w`.
* Evita los hashes como un parámetro opcional. Acaso el método hace demasiado? (Los
  inicializadores de objetos son excepciones a esta regla).
* Evita métodos mayores que 10 LOC (lines of code). Idealmente, la mayoría de los métodos van
  a ser menores que 5 LOC. Líneas vacías no cuentan como LOC relevantes.
* Evita listas de parámetros mayores que tres o cuatro parámetros.
* Si realmente necesitás métodos "globales", agregalos a tu Kernel
  y convertilos en `private`.
* Usa variables de instancia en módulos en lugar de variables globales.

    ```Ruby
    # mal
    $foo_bar = 1

    # bien
    module Foo
      class << self
        attr_accessor :bar
      end
    end

    Foo.bar = 1
    ```

* Evita `alias` cuando `alias_method` hace mejor el trabajo.
* Usa `OptionParser` para parsear líneas de opciones de comando complejas
y `ruby -s` para líneas de opciones de comando triviales.
* Prefiere `Time.now` por sobre `Time.new` cuando estés leyendo la hora del sistema.
* Escribe código en forma funcional, evitando mutación cuando eso tenga sentido.
* No mutes argumentos excepto que ese sea el propósito del método.
* Evita más de tres niveles de anidación de bloques.
* Se consistente. En un mundo ideal, se consistente con estas guías.
* Usa el sentido común.

## Herramientas

Aquí hay algunas herramientas que pueden ayudarte a validar el código
Ruby de forma automática con esta guía.

### RuboCop

[RuboCop](https://github.com/bbatsov/rubocop) es un validador de código
Ruby basado en esta guía. RuboCop actualmente cubre una significante parte
de esta Guía, soportando tanto MRI 1.9 y MRI 2.0, y ya tiene integración
con Emacs.

### RubyMine

El código de inspección de [RubyMine](http://www.jetbrains.com/ruby/) está
[parcialmente basado](http://confluence.jetbrains.com/display/RUBYDEV/RubyMine+Inspections)
en esta guía.

# Aportes

Nada de lo que está en esta guía está escrito en piedra. Es mi deseo que
trabajemos juntos con todos los que estén interesados con el estilo de
código en Ruby, para que podamos crear un recurso que pueda beneficiar a
toda la comunidad de Ruby.

Siéntanse libres de abrir tickets o enviar pull requests con mejoras. ¡Desde
ya muchas gracias por su ayuda!

# Licencia

![Creative Commons License](http://i.creativecommons.org/l/by/3.0/88x31.png)
Este trabajo está licenciado bajo [Creative Commons Attribution 3.0 Unported License](http://creativecommons.org/licenses/by/3.0/deed.en_US)

# Corre la Voz

Una guía de estilos pensada para la comunidad es de poca ayuda para
esa comunidad si no conoce su existencia. Escribe tweets sobre la guía,
compartila con tus amigos y compañeros de trabajo. Cada comentario,
sugerencia u opinión hace que la guía sea un poco mejor. Y lo que más
queremos que la mejor guía posible, ¿verdad?

Saludos,<br/>
[Bozhidar](https://twitter.com/bbatsov)
