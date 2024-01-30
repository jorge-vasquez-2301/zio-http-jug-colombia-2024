---
info: Slides para la presentación en el Barranquilla JUG, Enero 2024
theme: 'default'
presenter: false
download: false
exportFilename: 'presentation'
export:
  format: pdf
  timeout: 30000
  dark: auto
  withClicks: true
  withToc: true
highlighter: 'shiki'
lineNumbers: true
monaco: false
remoteAssets: true
selectable: true
record: false
colorSchema: 'dark'
aspectRatio: '16/9'
canvasWidth: 980
favicon: 'zioIcon.png'
drawings:
  enabled: false
  persist: false
  presenterOnly: false
  syncAll: true

transition: slide-left
background: /web.jpg
---

### ZIO HTTP: Programación Funcional en Acción con Scala!

<b>Enero 2024</b>

---
transition: slide-left
layout: image-right
image: /yo.jpg
class: "flex items-center text-2xl"
---

<div>
  <p>Jorge Vásquez</p>
  <p>Scala Developer</p>
  <p><b>@Ziverge</b></p>
</div>

---
transition: slide-left
layout: image-right
image: ./agenda.jpg
---

## **Agenda**

<div class="mt-4 flex h-3/5 w-full items-center gap-5 text-justify">
  <ul>
    <li v-click>Conceptos básicos de <b>Programación Funcional (PF)</b></li>
    <li v-click>Conceptos básicos de <b>ZIO</b></li>
    <li v-click>Conceptos básicos de <b>ZIO HTTP</b></li>
    <li v-click>Ejemplo práctico: <b>Shopping Cart</b></li> 
  </ul>
</div>


---
transition: slide-left
layout: image
image: /wondering.jpg
class: "justify-end"
---

## ¿Qué es la **Programación Funcional?**

---
transition: slide-left
layout: default
---

## ¿Qué es la **Programación Funcional?**

<div class="flex h-4/5 w-full items-center justify-center text-center">
  <span class="text-3xl leading-relaxed">Es un Paradigma de Programación, donde un Programa es una <b>Composición de Funciones Puras</b></span>
</div>

---
transition: slide-left
layout: image-right
image: /checklist.jpg
---

## Características de una **Función Pura**

<div class="flex h-2/5 w-full items-center text-justify">
  <ul>
    <li v-click><b>Total</b></li>
    <li v-click><b>Determinística</b> y <b>depende sólo de sus entradas</b></li>
    <li v-click><b>NO</b> puede tener <b>efectos colaterales</b></li>  
  </ul>
</div>

---
transition: slide-left
layout: default
---

## Una **Función Pura** debe ser **Total**

<div class="flex h-4/5 w-full items-center justify-center text-center">
  <span class="text-3xl leading-relaxed">Para cada <b>entrada</b> que se provea a la función, debe haber una <b>salida</b> definida</span>
</div>

---
transition: slide-left
layout: default
---

## Una **Función Pura** debe ser **Total**

<div class="mt-5 flex flex-col h-4/5 w-full justify-center">
<div class="flex-1">
```scala {none|1|3|5} {maxHeight:'400px'}
def divide(a: Int, b: Int): Int = a / b

divide(5, 0)

// java.lang.ArithmeticException: / by zero
```
</div>
<div v-click class="flex w-full h-full justify-center items-center">
  <div><img src="/boom.jpg" class="h-50 rounded-md"/></div>
</div>
</div>

---
transition: slide-left
layout: default
---

## Una **Función Pura** debe ser **Total**

<div class="mt-4 flex flex-col h-2/5 w-full justify-center">
<div class="flex-1">
```scala {all} {maxHeight:'400px'}
def divide(a: Int, b: Int): Int = a / b
```
</div>
<div>
  <ul>
    <li v-click>El <b>encabezado</b> de esta función <b>dice una mentira</b></li>
    <li v-click>Cada vez que llamemos a la función tendremos que ser <b>bastante cuidadosos</b></li>
    <li v-click><b>Excepciones</b> pueden ocurrir en <b>tiempo de ejecución</b>: El compilador nada puede hacer para ayudarnos a evitar esto</li>  
  </ul>
</div>
</div>

---
transition: slide-left
layout: default
---

## Una **Función Pura** debe ser **Total**

<div class="mt-5 flex flex-col h-4/5 w-full justify-center">
<div class="flex-1">
```scala {none|1-2|4|6} {maxHeight:'400px'}
def divide(a: Int, b: Int): Option[Int] =
  if (b != 0) Some(a / b) else None

divide(5, 0)

// None
```
</div>
<div v-click class="flex w-full h-full justify-center items-center">
  <div><img src="/ahoraSi.jpg" class="h-50 rounded-md"/></div>
</div>
</div>

---
transition: slide-left
layout: default
---

## Una **Función Pura** debe ser **Total**

<div class="mt-4 flex flex-col h-2/5 w-full justify-center">
<div class="flex-1">
```scala {all} {maxHeight:'400px'}
def divide(a: Int, b: Int): Option[Int] =
  if (b != 0) Some(a / b) else None
```
</div>
<div>
  <ul>
    <li v-click>El <b>encabezado</b> de esta función claramente comunica que <b>algunas entradas no pueden ser manejadas</b></li>
    <li v-click><b>El compilador nos forzará</b> a considerar el caso en el cual el resultado no está definido</li>
    <li v-click>No más <b>excepciones en tiempo de ejecución</b></li>  
  </ul>
</div>
</div>

---
transition: slide-left
layout: default
---

## Una **Función Pura** debe ser **Determinística** y debe **depender sólo de sus entradas**

<div class="flex h-4/5 w-full items-center justify-center text-center">
  <span class="text-3xl leading-relaxed">Para <b>cada entrada</b> que se provea a la función, <br/>la <b>misma salida debe ser retornada</b>, <br/>
sin importar cuántas veces la función sea llamada</span>
</div>

---
transition: slide-left
layout: default
---

## Una **Función Pura** debe ser **Determinística** y debe **depender sólo de sus entradas**

<div class="mt-5 flex flex-col h-4/5 w-full justify-center">
<div class="flex-1">
```scala {none|1|3|5} {maxHeight:'400px'}
def generateRandomInt(): Int = (new scala.util.Random).nextInt // Dependencia de un valor oculto!

generateRandomInt() // Result: -272770531

generateRandomInt() // Result: 217937820
```
</div>
<div v-click class="flex w-full h-full justify-center items-center">
  <div><img src="/cambio.jpg" class="h-50 rounded-md"/></div>
</div>
</div>

---
transition: slide-left
layout: default
---

## Una **Función Pura** debe ser **Determinística** y debe **depender sólo de sus entradas**

<div class="mt-4 flex flex-col h-2/5 w-full justify-center">
<div class="flex-1">
```scala {all} {maxHeight:'400px'}
def generateRandomInt(): Int = (new scala.util.Random).nextInt
```
</div>
<div>
  <ul>
    <li v-click>Claramente esta función <b>no es determinística</b></li>
    <li v-click>El <b>encabezado de la función</b> es engañoso, porque hay una dependencia oculta hacia un objeto <code>scala.util.Random</code></li>
    <li v-click><b>Difícil de testear</b> porque nunca podemos estar realmente seguros de cómo se comportará la función</li>  
  </ul>
</div>
</div>

---
transition: slide-left
layout: default
---

## Una **Función Pura** debe ser **Determinística** y debe **depender sólo de sus entradas**

<div class="flex h-4/5 w-full items-center">
<div class="flex-1">
```scala {none|1-10|12|13|14|15} {maxHeight:'400px'}
final case class RNG(seed: Long) {
  def nextInt: (Int, RNG) = {
    val newSeed = (seed * 0x5DEECE66DL + 0xBL) & 0xFFFFFFFFFFFFL
    val nextRNG = RNG(newSeed)
    val n       = (newSeed >>> 16).toInt
    (n, nextRNG)
  }
}

def generateRandomInt(random: RNG): (Int, RNG) = random.nextInt

val random        = RNG(10)
val (n1, random1) = generateRandomInt(random)  // n1 = 3847489,    random1 = RNG(252149039181)
val (n2, random2) = generateRandomInt(random)  // n2 = 3847489,    random2 = RNG(252149039181)
val (n3, random3) = generateRandomInt(random2) // n3 = 1334288366, random3 = RNG(87443922374356)
```
</div>
</div>

---
transition: slide-left
layout: default
---

## Una **Función Pura** NO debe tener **Efectos Colaterales**

<div class="flex h-4/5 w-full items-center justify-center gap-5">
  <div class="w-1/3 flex justify-center"><img src="/drakeNo.jpeg" class="h-50 rounded-md"/></div>
  <div class="w-1/3">
    <ul>
      <li v-click><b>Mutaciones</b> de memoria compartida</li>
      <li v-click><b>Imprimir</b> mensajes por consola</li>
      <li v-click>Llamar a una <b>API externa</b></li>
      <li v-click>Consultar a una <b>base de datos</b></li>
    </ul>
  </div>
</div>

---
transition: slide-left
layout: default
---

## Una **Función Pura** NO debe tener **Efectos Colaterales**

<div class="flex h-4/5 w-full items-center justify-center gap-5">
  <div class="w-1/3 flex justify-center"><img src="/drakeYes.jpeg" class="h-50 rounded-md"/></div>
  <div class="w-1/3">
    <ul>
      <li v-click>Trabajar con valores <b>inmutables</b></li>
      <li v-click><b>Retornar</b> una salida para una entrada correspondiente</li>
    </ul>
  </div>
</div>

---
transition: slide-left
layout: default
---

## Una **Función Pura** NO debe tener **Efectos Colaterales**

<div class="flex h-3/5 w-full items-center">
<div class="flex-1">
```scala {1-5|1|3|7-10|8} {maxHeight:'400px'}
var a = 0
def increment(inc: Int): Int = {
  a + = inc // ¡Mutación de memoria!
  a
}

def add(a: Int, b: Int): Int = {
  println(s"Adding two integers: $a and $b") // ¡Interacción con el mundo exterior!
  a + b
}
```
</div>
</div>

---
transition: slide-left
layout: default
---

## Una **Función Pura** equivale a un **Mapa/Diccionario**

<div class="flex flex-col h-4/5 w-full items-center justify-center text-center gap-5">
  <span class="text-2xl">Si una función puede ser reemplazada por un <b>Mapa/Diccionario (posiblemente infinito)...</b></span>
  <span v-click class="text-3xl">Es una <b>función pura</b></span>
</div>

---
transition: slide-left
layout: default
---

## Una **Función Pura** equivale a un **Mapa/Diccionario**

<div class="flex h-3/5 w-full items-center">
<div class="flex-1">
```scala {1|3-9} {maxHeight:'400px'}
def add(a: Int, b: Int): Int = a + b

val add =
  Map(
    (0, 0) -> 0,
    (0, 1) -> 1,
    (0, 2) -> 2,
    ...
  )
```
</div>
</div>

---
transition: slide-left
layout: default
---

## Una **Función Pura** equivale a un **Mapa/Diccionario**

<div class="mt-5 flex flex-col h-4/5 w-full justify-center">
<div class="flex-1">
```scala {all} {maxHeight:'400px'}
def generateRandomInt(): Int = (new scala.util.Random).nextInt
```

</div>
<div v-click class="flex w-full h-full justify-center items-center">
  <div><img src="/confusedMap.jpg" class="h-50 rounded-md"/></div>
</div>
</div>

---
transition: slide-left
layout: default
---

## En la **Programación Funcional**, las Funciones son **Ciudadanos de Primera Clase**

<div class="flex h-3/5 w-full items-center text-justify gap-5">
  <div>
    <ul>
      <li v-click>Pueden ser <b>almacenadas en variables</b></li>
      <li v-click>Pueden ser <b>pasadas como parámetros a otras funciones</b></li>
      <li v-click>Pueden ser <b>retornadas por otras funciones</b></li>
    </ul>
  </div>
  <div><img v-click src="/primeraClase.jpg" class="h-50 rounded-md"/></div>
</div>

---
transition: slide-left
layout: default
---

<div class="flex w-full h-full justify-center items-center">
  <div><img src="/valor.jpg" class="h-90 rounded-md"/></div>
</div>

---
transition: slide-left
layout: default
---

<div class="flex w-full h-full justify-center items-center">
  <div><img src="/valores.jpg" class="h-70 rounded-md"/></div>
</div>

---
transition: slide-left
layout: default
---

<div class="flex h-4/5 w-full items-center justify-center text-center">
  <span class="text-3xl leading-relaxed"><i>Programación Funcional</i> = <b>"Programación Orientada a Valores"</b></span>
</div>

---
transition: slide-left
layout: image-right
image: ./checklist.jpg
---

## Beneficios de la **Programación Funcional**

<div class="flex h-3/5 w-full items-center">
  <ul>
    <li v-click><b>Razonamiento local</b></li>
    <li v-click>Transparencia Referencial -> <b>¡Refactorización sin miedo!</b></li>
    <li v-click>Concisión -> <b>¡Menos bugs!</b></li>
  </ul>
</div>

---
transition: slide-left
layout: image-right
image: ./checklist.jpg
---

## Beneficios de la **Programación Funcional**

<div class="flex h-3/5 w-full items-center">
  <ul>
    <li v-click>Más fácil de <b>testear</b></li>
    <li v-click>Las aplicaciones se comportan más <b>predeciblemente</b></li>
    <li v-click>Nos permite escribir <b>programas paralelos y concurrentes correctos</b></li>
  </ul>
</div>

---
transition: slide-left
layout: default
---

<div class="flex w-full h-full justify-center items-center">
  <div><img src="/necesito.jpg" class="h-70 rounded-md"/></div>
</div>

---
transition: slide-left
layout: default
---

<div class="flex w-full h-full justify-center items-center">
  <div><img src="/noNecesitas.jpg" class="h-70 rounded-md"/></div>
</div>

---
transition: slide-left
layout: default
---

## Solución: **Efectos Funcionales**

<div class="flex h-3/5 w-full items-center gap-5">
  <div>
    <ul>
      <li v-click><b>Descripciones</b> de interacciones con el mundo exterior</li>
      <li v-click><b>Valores inmutables</b> que pueden funcionar como entradas y salidas de funciones puras</li>
      <li v-click><b>Workflows</b></li>
      <li v-click>Son ejecutados solamente en el <b>Fin del Mundo</b></li>
    </ul>
  </div>
  <div v-click><img src="/interesante.jpg" class="h-50 rounded-md"/></div>
</div>

---
transition: slide-left
layout: default
---

## Con ustedes... **¡ZIO!**

<div class="flex flex-col h-4/5 w-full items-center justify-center text-center gap-5">
  <span class="text-2xl">Librería que nos permite construir <b>aplicaciones modernas</b>, usando los principios de la <b>Programación Funcional</b></span>
</div>

---
transition: slide-left
layout: default
---

## **ZIO** para crear aplicaciones modernas...

<div class="flex h-4/5 w-full items-center gap-5">
  <div>
    <ul>
      <li v-click><b>Asíncronas y Concurrentes</b> -> Modelo basado en Fibras/Virtual Threads (incluso sin Loom!)</li>
      <li v-click><b>Resilientes</b> -> Aprovechando todo el poder del sistema fuertemente tipado de Scala</li>
      <li v-click><b>Eficientes</b> -> Aplicaciones que nunca desperdician recursos</li>
      <li v-click><b>Fáciles de entender y de testear</b> -> Gracias a una composicionalidad superior</li>
    </ul>
  </div>
  <div v-click><img src="/moderno.jpg" class="h-50 rounded-md"/></div>
</div>

---
transition: slide-left
layout: default
---

## El Tipo de Datos **ZIO**

<div class="mt-20 flex flex-col h-1/5 w-full justify-center gap-5">
<div class="flex-1">
```scala {all} {maxHeight:'400px'}
ZIO[-R, +E, +A]
```
</div>
<div>
  <ul>
    <li v-click><b>Tipo de Datos central</b> de la librería ZIO</li>
    <li v-click><b>Efecto Funcional</b></li>
  </ul>
</div>
</div>

---
transition: slide-left
layout: default
---

## El Tipo de Datos **ZIO**

<div class="mt-30 flex flex-col h-1/5 w-full justify-center gap-5">
<span>Un buen <b>modelo mental</b> es:</span>
<div class="flex-1">
```scala {all} {maxHeight:'400px'}
ZEnvironment[R] => Either[E, A]
```
</div>
<span v-click>Esto significa que un efecto ZIO:</span>
<div>
  <ul>
    <li v-click>Necesita un <b>entorno</b> de tipo <code>R</code> para ser ejecutado</li>
    <li v-click>Podría fallar con un <b>error</b> de tipo <code>E</code></li>
    <li v-click>O podría <b>ejecutarse exitosamente</b>, retornando un valor de tipo <code>A</code></li>
  </ul>
</div>
</div>

---
transition: slide-left
layout: default
---

<div class="flex w-full h-full justify-center items-center">
  <div><img src="/descubrir.jpeg" class="h-70 rounded-md"/></div>
</div>

---
transition: slide-left
layout: default
---

## El **Ecosistema** de ZIO

<div class="flex h-4/5 w-full items-center gap-5">
  <div>
    <ul>
      <li v-click><b>REST APIs</b> -> ZIO HTTP</li>
      <li v-click><b>GraphQL</b> -> Caliban</li>
      <li v-click><b>Bases de datos</b> -> ZIO JDBC, ZIO SQL, ZIO Quill</li>
      <li v-click><b>JSON</b> -> ZIO Json</li>
      <li v-click><b>Configuración</b> -> ZIO Config</li>
      <li v-click><b>Logging</b> -> ZIO Logging</li>
      <li v-click><b>Métricas</b> -> ZIO Metrics Connectors</li>
      <li v-click><b>Tests</b> -> ZIO Test, ZIO Mock, ZIO Testcontainers</li>
    </ul>
  </div>
</div>

---
transition: slide-left
layout: default
---

## El **Ecosistema** de ZIO

<div class="flex h-4/5 w-full items-center gap-5">
  <div>
    <ul>
      <li v-click><b>Metaprogramming sin Macros</b> -> ZIO Schema</li>
      <li v-click><b>Parsing</b> -> ZIO Parser</li>
      <li v-click><b>Apps de línea de comandos</b> -> ZIO CLI</li>
      <li v-click><b>AWS</b> -> ZIO AWS, ZIO DynamoDB, ZIO S3, ZIO SQS</li>
      <li v-click><b>Kafka</b> -> ZIO Kafka</li>
      <li v-click><b>Redis</b> -> ZIO Redis</li>
      <li v-click><b>Kubernetes</b> -> ZIO K8s</li>
    </ul>
  </div>
</div>

---
transition: slide-left
layout: default
---

<div class="flex w-full h-full justify-center items-center">
  <div><img src="/finalmente.jpg" class="h-70 rounded-md"/></div>
</div>

---
transition: slide-left
layout: image-right
image: /checklist.jpg
---

### Con ustedes... **¡ZIO HTTP!**

<div class="flex h-4/5 w-full items-center gap-5">
  <ul>
    <li v-click><b>ZIO HTTP</b> es una librería para construir aplicaciones HTTP</li>
    <li v-click>Basada en <b>ZIO</b> y <b>Netty</b></li>
    <li v-click><b>Alto performance:</b> Usa árboles de prefijos para enrutamiento</li>
  </ul>
</div>

---
transition: slide-left
layout: image-right
image: /checklist.jpg
---

### **ZIO HTTP** ofrece 2 APIs

<div class="flex h-4/5 w-full items-center gap-5">
  <ul>
    <li v-click><b>Routes API:</b> Bajo nivel</li>
    <li v-click><b>Endpoints API:</b> Alto nivel</li>
  </ul>
</div>

---
transition: slide-left
layout: image-right
image: /shoppingCart.jpg
---

## Ejemplo: <br/> **Shopping cart API**

<div class="mt-8 grid grid-cols-[33%_1fr] h-3/5 w-full text-sm">
  <b v-click>Initialize cart:</b> <span v-after><code>POST /cart/{userId}</code></span>
  <b v-click>Add item:</b> <span v-after><code>POST /cart/{userId}/item</code></span>
  <b v-click>Delete item:</b> <span v-after><code>DELETE /cart/{userId}/item/{itemId}</code></span>
  <b v-click>Update item:</b> <span v-after><code>PUT /cart/{userId}/item/{itemId}</code></span>
  <b v-click>Get cart contents:</b> <span v-after><code>GET /cart/{userId}</code></span>
</div>

---
transition: slide-left
layout: default
---

## Shopping Cart usando la **Routes API**

<div class="flex h-4/5 w-full items-center">
```scala {all} {maxHeight:'400px'}
// build.sbt
libraryDependencies ++= Seq(
  "dev.zio" %% "zio-http" % zioHttpVersion // 3.0.0-RC4
)
```
</div>

<style>
  .slidev-code-wrapper {
    @apply w-full
  }
</style>

---
transition: slide-left
layout: default
---

## Shopping Cart usando la **Routes API**

```scala {1|3|4|6|8-15|17-34|36-40} {maxHeight:'400px'}
import zio.json._

// Paso 1: Definir modelos
type UserId = UUID

type ItemId = UUID

final case class Item(id: ItemId, name: String, price: Double, quantity: Int) {
  self =>
  def withQuantity(quantity: Int): Item = self.copy(quantity = quantity)
}

object Item {
  implicit val jsonCodec: JsonCodec[Item] = DeriveJsonCodec.gen[Item]
}

final case class Items(items: Map[ItemId, Item]) {
  self =>

  def +(item: Item): Items = Items(self.items + (item.id -> item))

  def -(itemId: ItemId): Items = Items(self.items - itemId)

  def take(n: Int): Items = Items(self.items.take(n))

  def updateQuantity(itemId: ItemId, quantity: Int): Items =
    Items(self.items.updatedWith(itemId)(_.map(_.withQuantity(quantity))))
}

object Items {
  val empty = Items(Map.empty)

  implicit val jsonCodec: JsonCodec[Items] = DeriveJsonCodec.gen[Items]
}

final case class UpdateItemRequest(quantity: Int)

object UpdateItemRequest {
  implicit val jsonCodec: JsonCodec[UpdateItemRequest] = DeriveJsonCodec.gen[UpdateItemRequest]
}
```

---
transition: slide-left
layout: default
---

## Shopping Cart usando la **Routes API**

```scala {1-3|5|6-13|8|9-12} {maxHeight:'400px'}
import zio._
import zio.http._
import zio.json._

// Paso 2: Definir rutas: segmentos, variables, validaciones y handlers
val routes =
    Routes(
      Method.POST / "cart" / uuid("userId")                             -> handler(handleInitializeCart _),
      Method.POST / "cart" / uuid("userId") / "item"                    -> handler(handleAddItem _),
      Method.DELETE / "cart" / uuid("userId") / "item" / uuid("itemId") -> handler(handleRemoveItem _),
      Method.PUT / "cart" / uuid("userId") / "item" / uuid("itemId")    -> handler(handleUpdateItem _),
      Method.GET / "cart" / uuid("userId")                              -> handler(handleGetCartContents _)
    )
```

---
transition: slide-left
layout: default
---

## Shopping Cart usando la **Routes API**

```scala {1-3|5|7-14|8|13|16-36|17|21-23|25-34|35|38-45|39|44|47-60|48|52-54|56-59|62-78|63|67-76|77} {maxHeight:'400px'}
import zio._
import zio.http._
import zio.json._

// Paso 3: Definir los handlers

// POST /cart/{userId}
def handleInitializeCart(userId: UserId, req: Request): ZIO[CartService, Nothing, Response] =
  ZIO.logSpan("initializeCart") {
    for {
      _ <- ZIO.logInfo("Initializing cart")
      _ <- CartService.initialize(userId)
    } yield Response.status(Status.NoContent) // Tenemos que crear un HTTP Response manualmente
  } @@ ZIOAspect.annotated("userId", userId.toString())

// POST /cart/{userId}/item
def handleAddItem(userId: UserId, req: Request): ZIO[CartService, Nothing, Response] =
  ZIO.logSpan("addItem") {
    for {
      _ <- ZIO.logInfo("Adding item to cart")
      // Tenemos que decodificar el body como JSON, manejando errores
      body   <- req.body.asString.orDie
      item   <- ZIO.fromEither(body.fromJson[Item]).orDieWith(new RuntimeException(_))
      items0 <- CartService.addItem(userId, item)
      // Tenemos que obtener y validar headers manualmente a partir del objeto `Request`
      allItems = req.headers.get("X-ALL-ITEMS")
      items <- allItems match {
                case Some(allItems) =>
                  ZIO.attempt(allItems.toBoolean).orDie.map {
                    case true  => items0
                    case false => Items.empty + item
                  }
                case None => ZIO.succeed(Items.empty + item)
              }
    } yield Response.json(items.toJson) // Tenemos que codificar manualmente el Response como JSON
  } @@ ZIOAspect.annotated("userId", userId.toString())

// DELETE /cart/{userId}/item/{itemId}
def handleRemoveItem(userId: UserId, itemId: ItemId, req: Request): ZIO[CartService, Nothing, Response] =
  ZIO.logSpan("removeItem") {
    for {
      _     <- ZIO.logInfo("Removing item from cart")
      items <- CartService.removeItem(userId, itemId)
    } yield Response.json(items.toJson) // Tenemos que codificar manualmente el Response como JSON
  } @@ ZIOAspect.annotated("userId" -> userId.toString(), "itemId" -> itemId.toString())

// PUT /cart/{userId}/item/{itemId}
def handleUpdateItem(userId: UserId, itemId: ItemId, req: Request): ZIO[CartService, Nothing, Response] =
  ZIO.logSpan("updateItem") {
    for {
      _ <- ZIO.logInfo("Updating item")
      // Tenemos que decodificar el body como JSON, manejando errores
      body              <- req.body.asString.orDie
      updateItemRequest <- ZIO.fromEither(body.fromJson[UpdateItemRequest]).orDieWith(new RuntimeException(_))
      items             <- CartService.updateItemQuantity(userId, itemId, updateItemRequest.quantity)
    } yield Response(
      Status.Ok,
      Headers(Header.ContentType(MediaType.application.json))
    ) // Tenemos que crear un HTTP Response manualmente
  } @@ ZIOAspect.annotated("userId" -> userId.toString(), "itemId" -> itemId.toString())

// GET /cart/{userId}
def handleGetCartContents(userId: UserId, req: Request): ZIO[CartService, Nothing, Response] =
  ZIO.logSpan("getCartContents") {
    for {
      _ <- ZIO.logInfo("Getting cart contents")
      // Tenemos que obtener y validar query params manualmente a partir del objeto `Request`
      limit = req.url.queryParams.get("limit").flatMap(_.headOption)
      items <- limit match {
                case Some(limit) =>
                  ZIO
                    .attempt(limit.toInt)
                    .orDie
                    .flatMap(limit => CartService.getContents(userId).map(_.take(limit)))
                case None => CartService.getContents(userId)
              }
    } yield Response.json(items.toJson) // Tenemos que codificar manualmente el Response como JSON
  } @@ ZIOAspect.annotated("userId" -> userId.toString())
```

---
transition: slide-left
layout: default
---

## Shopping Cart usando la **Routes API**

<div class="flex h-3/5 w-full items-center">
```scala {1-3|5|6|7|9-10} {maxHeight:'400px'}
import zio._
import zio.http._
import zio.json._

// Paso 4: Ejecutar el servidor
object ShoppingCart extends ZIOAppDefault {
  val routes = ...

  override val run =
    Server.serve(routes.sandbox.toHttpApp).provide(Server.default, CartServiceLive.layer)
}

```
</div>

---
transition: slide-left
layout: image-right
image: /checklist.jpg
---

### Características de la **Routes API**

<div class="flex h-4/5 w-full items-center gap-5">
  <ul>
    <li v-click>Se define la API como un mapeo <b>Método/Path -> Handler</b></li>
    <li v-click>Es de <b>bajo nivel</b></li>
    <li v-click>Permite control total del <b>Request</b> y el <b>Response</b></li>
    <li v-click>Impone tareas repetitivas de <b>codificación/decodificación</b></li>
  </ul>
</div>

---
transition: slide-left
layout: default
---

## Shopping Cart usando la **Endpoints API**

```scala {1-3|5|6|8|10-23|11|25-42|44-49} {maxHeight:'400px'}
import zio.json._
import zio.schema._
import zio.schema.annotation.description

// Paso 1: Definir modelos
type ItemId = UUID

type UserId = UUID

final case class Item(
  @description("The Item's unique identifier") id: ItemId,
  @description("The Item's name") name: String,
  @description("The Item's unit price") price: Double,
  @description("The Item's quantity") quantity: Int
) {
  self =>
  def withQuantity(quantity: Int): Item = self.copy(quantity = quantity)
}

object Item {
  implicit val jsonCodec: JsonCodec[Item] = DeriveJsonCodec.gen[Item]
  implicit val schema: Schema[Item]       = DeriveSchema.gen[Item]
}

final case class Items(@description("Map of item IDs to corresponding items") items: Map[ItemId, Item]) {
  self =>

  def +(item: Item): Items = Items(self.items + (item.id -> item))

  def -(itemId: ItemId): Items = Items(self.items - itemId)

  def take(n: Int): Items = Items(self.items.take(n))

  def updateQuantity(itemId: ItemId, quantity: Int): Items =
    Items(self.items.updatedWith(itemId)(_.map(_.withQuantity(quantity))))
}
object Items {
  val empty = Items(Map.empty)

  implicit val jsonCodec: JsonCodec[Items] = DeriveJsonCodec.gen[Items]
  implicit val schema: Schema[Items]       = DeriveSchema.gen[Items]
}

final case class UpdateItemRequest(@description("The new item quantity") quantity: Int)

object UpdateItemRequest {
  implicit val jsonCodec: JsonCodec[UpdateItemRequest] = DeriveJsonCodec.gen[UpdateItemRequest]
  implicit val schema: Schema[UpdateItemRequest]       = DeriveSchema.gen[UpdateItemRequest]
}
```

---
transition: slide-left
layout: default
---

## Shopping Cart usando la **Endpoints API**

```scala {1-4|6|7|8|9|10-11|13-15|14|15|17-21|18|19|20|21|23-25|27-30|32-35|34} {maxHeight:'400px'}
import zio.http._
import zio.http.codec._
import zio.http.codec.HttpCodec._
import zio.http.endpoint.Endpoint

// Paso 2: Definir Endpoints
val userId    = uuid("userId") ?? Doc.p("The unique identifier of a user")
val itemId    = uuid("itemId") ?? Doc.p("The unique identifier of an item")
val limit     = paramInt("limit").optional ?? Doc.p("The maximum number of items to obtain")
val xAllItems = HeaderCodec.name[Boolean]("X-ALL-ITEMS").optional ??
  Doc.p("Flag to indicate whether to return all items or just the new one")

val initializeCart =
  Endpoint(Method.POST / "cart" / userId)
    .out[Unit](Status.NoContent) ?? Doc.p("Initiliaze a user's cart")

val addItem =
  Endpoint(Method.POST / "cart" / userId / "item")
    .header(xAllItems)
    .in[Item](Doc.p("The item to be added"))
    .out[Items](Doc.p("The operation result")) ?? Doc.p("Add an item to a user's cart")

val removeItem =
  Endpoint(Method.DELETE / "cart" / userId / "item" / itemId)
    .out[Items](Doc.p("The cart items after removal")) ?? Doc.p("Removes an item from a user's cart")

val updateItem =
  Endpoint(Method.PUT / "cart" / userId / "item" / itemId)
    .in[UpdateItemRequest](Doc.p("The request object"))
    .out[Items](Doc.p("The cart items after updating")) ?? Doc.p("Updates an item")

val getCartContents =
  Endpoint(Method.GET / "cart" / userId)
    .query(limit)
    .out[Items](Doc.p("The cart items")) ?? Doc.p("Gets the contents of a user's cart")
```

---
transition: slide-left
layout: default
---

## Shopping Cart usando la **Endpoints API**

```scala {1-2|4|5-11|5|13-23|25-31|33-39|41-47} {maxHeight:'400px'}
import zio._
import zio.http._

// Paso 3: Definir handlers
def handleInitializeCart(userId: UserId): ZIO[CartService, Nothing, Unit] =
  ZIO.logSpan("initializeCart") {
    for {
      _ <- ZIO.logInfo("Initializing cart")
      _ <- CartService.initialize(userId)
    } yield ()
  } @@ ZIOAspect.annotated("userId", userId.toString())

def handleAddItem(userId: UserId, allItems: Option[Boolean], item: Item): ZIO[CartService, Nothing, Items] =
  ZIO.logSpan("addItem") {
    for {
      _      <- ZIO.logInfo("Adding item to cart")
      items0 <- CartService.addItem(userId, item)
      items   = allItems match {
                  case Some(true) => items0
                  case _          => Items.empty + item
                }
    } yield items
  } @@ ZIOAspect.annotated("userId", userId.toString())

def handleRemoveItem(userId: UserId, itemId: ItemId): ZIO[CartService, Nothing, Items] =
  ZIO.logSpan("removeItem") {
    for {
      _     <- ZIO.logInfo("Removing item from cart")
      items <- CartService.removeItem(userId, itemId)
    } yield items
  } @@ ZIOAspect.annotated("userId" -> userId.toString(), "itemId" -> itemId.toString())

def handleUpdateItem(userId: UserId, itemId: ItemId, updateItemRequest: UpdateItemRequest) =
  ZIO.logSpan("updateItem") {
    for {
      _     <- ZIO.logInfo("Updating item")
      items <- CartService.updateItemQuantity(userId, itemId, updateItemRequest.quantity)
    } yield items
  } @@ ZIOAspect.annotated("userId" -> userId.toString(), "itemId" -> itemId.toString())

def handleGetCartContents(userId: UserId, limit: Option[Int]): ZIO[CartService, Nothing, Items] =
  ZIO.logSpan("getCartContents") {
    for {
      _     <- ZIO.logInfo("Getting cart contents")
      items <- CartService.getContents(userId)
    } yield limit.fold(items)(items.take)
  } @@ ZIOAspect.annotated("userId" -> userId.toString())
```

---
transition: slide-left
layout: default
---

## Shopping Cart usando la **Endpoints API**

```scala {1-2|4|5|8-15|10|17} {maxHeight:'400px'}
import zio._
import zio.http._

// Paso 4: Ejecutar el servidor
object EndpointsServer extends ZIOAppDefault {
  ...

  val routes =
    Routes(
      initializeCart.implement(handler(handleInitializeCart _)),
      addItem.implement(handler(handleAddItem _)),
      removeItem.implement(handler(handleRemoveItem _)),
      updateItem.implement(handler(handleUpdateItem _)),
      getCartContents.implement(handler(handleGetCartContents _))
    )

  val run = Server.serve(routes.sandbox.toHttpApp).provide(Server.default, CartServiceLive.layer)
}
```

---
transition: slide-left
layout: image-right
image: /checklist.jpg
---

### Características de la **Endpoints API**

<div class="flex h-4/5 w-full items-center gap-5">
  <ul>
    <li v-click>API es un mapeo <b>Endpoint -> Handler</b></li>
    <li v-click>Es de <b>alto nivel</b></li>
    <li v-click>Elimina tareas repetitivas de <b>codificación/decodificación</b></li>
  </ul>
</div>

---
transition: slide-left
layout: default
---

<div class="flex w-full h-full justify-center items-center">
  <div><img src="/hayMas.jpg" class="h-70 rounded-md"/></div>
</div>

---
transition: slide-left
layout: default
---

### Bonus: Cliente usando la **Endpoints API**

```scala {1-5|7|13|14-16|17-23|27-32} {maxHeight:'400px'}
import zio._
import zio.http._
import zio.http.endpoint.EndpointExecutor

import java.util.UUID

object ShoppingCartClient extends ZIOAppDefault {
  ...

  val clientExample: URIO[EndpointExecutor[Unit], Unit] =
    ZIO.scoped {
      for {
        executor <- ZIO.service[EndpointExecutor[Unit]]
        userId   <- ZIO.succeed(UUID.randomUUID())
        itemId1  <- ZIO.succeed(UUID.randomUUID())
        itemId2  <- ZIO.succeed(UUID.randomUUID())
        _        <- executor(initializeCart(userId))
        _        <- executor(addItem(userId, None, Item(itemId1, "test-item-1", 10.0, 10))).debug("addItem result")
        _        <- executor(addItem(userId, Some(true), Item(itemId2, "test-item-2", 20.0, 20))).debug("addItem result")
        _        <- executor(removeItem(userId, itemId2)).debug("removeItem result")
        _        <- executor(updateItem(userId, itemId1, UpdateItemRequest(35))).debug("updateItem result")
        _        <- executor(addItem(userId, Some(true), Item(itemId2, "test-item-2", 20.0, 20))).debug("addItem result")
        _        <- executor(getCartContents(userId, Some(1))).debug("getCartContents result")
      } yield ()
    }

  val run =
    clientExample
      .provide(
        EndpointExecutor.make(serviceName = "cart_service"),
        Client.default
      )
}
```

---
transition: slide-left
layout: default
---

## Bonus: Documentación usando la **Endpoints API**

<div class="flex h-3/5 w-full items-center">
```scala {1-3|5|8|10} {maxHeight:'400px'}
import zio._
import zio.http.endpoint.openapi.OpenAPIGen
import zio.json._

object ShoppingCartDocs extends ZIOAppDefault {
  ...
  
  val docs = OpenAPIGen.fromEndpoints(initializeCart, addItem, removeItem, updateItem, getCartContents)

  val run = Console.printLine(docs.toJson)
}
```
</div>

---
transition: slide-left
layout: default
---

## Bonus: CLI usando la **Endpoints API**

<div class="flex h-4/5 w-full items-center">
```scala {all} {maxHeight:'400px'}
// build.sbt
libraryDependencies ++= Seq(
  "dev.zio" %% "zio-http-cli" % zioHttpVersion // 3.0.0-RC4
)
```
</div>

<style>
  .slidev-code-wrapper {
    @apply w-full
  }
</style>

---
transition: slide-left
layout: default
---

## Bonus: CLI usando la **Endpoints API**

<div class="flex h-3/5 w-full items-center">
```scala {1-2|4|7-18} {maxHeight:'400px'}
import zio.cli._
import zio.http.endpoint.cli.HttpCliApp

object ShoppingCartCli extends ZIOCliDefault {
  ...

  val cliApp =
    HttpCliApp
      .fromEndpoints(
        name = "shopping-cart",
        version = "0.0.1",
        summary = HelpDoc.Span.text("Shopping Cart Command Line Interface"),
        footer = HelpDoc.p("Copyright 2024"),
        host = "localhost",
        port = 8080,
        endpoints = Chunk(initializeCart, addItem, removeItem, updateItem, getCartContents)
      )
      .cliApp
}
```
</div>

---
transition: slide-left
layout: default
---

## Bonus: CLI usando la **Endpoints API**

<div class="mt-4 flex w-full h-full justify-center">
  <div><img src="/demo.gif" class="h-100 rounded-md"/></div>
</div>

---
transition: slide-left
layout: image-right
image: /summary.jpg
---

## **En resumen**

<div class="mt-4 flex h-3/5 w-full items-center">
  <ul>
    <li v-click>La <b>Programación Funcional</b> no es un tema sólo para entornos académicos</li>
    <li v-click>La <b>Programación Funcional</b> permite construir aplicaciones completas en el mundo real</li>
  </ul>
</div>

---
transition: slide-left
layout: image-right
image: /summary.jpg
---

## **En resumen**

<div class="mt-4 flex h-4/5 w-full items-center">
  <ul>
    <li v-click><b>ZIO</b> es una librería para Scala que permite construir aplicaciones asíncronas, concurrentes, resilientes, eficientes, fáciles de entender y testear</li>
    <li v-click>Existe todo un <b>ecosistema de librerías</b> alrededor de ZIO para diversas situaciones</li>
    <li v-click><b>ZIO HTTP</b> permite implementar servidores REST, autogenerar clientes, documentación y CLIs</li>
  </ul>
</div>

---
transition: slide-left
layout: image-right
image: /learn.jpg
---

### ¿Dónde **aprender** más?

<div class="flex h-3/5 w-full items-center">
  <ul>
    <li v-click><a href="https://github.com/zio" target="_blank">ZIO en GitHub</a></li>
    <li v-click><a href="https://zio.dev/" target="_blank">Sitio oficial de ZIO</a></li>
    <li v-click><a href="https://www.zionomicon.com/" target="_blank">Zionomicon</a></li>
    <li v-click><a href="https://jorgevasquez.blog/" target="_blank">Mi nuevo blog personal (jorgevasquez.blog)</a></li>
  </ul>
</div>

---
transition: slide-left
layout: image-right
image: /computer.png
---

## **¡Gracias!**

<div class="grid grid-cols-8 gap-4 items-center h-4/5 content-center text-2xl">
  <div class="col-span-1"><img src="/x.png" class="w-8" /></div> <div class="col-span-7">@jorvasquez2301</div>
  <div class="col-span-1"><img src="/linkedin.png" class="w-8" /></div> <div class="col-span-7">jorge-vasquez-2301</div>
  <div class="col-span-1"><img src="/email.png" class="w-8" /></div> <div class="col-span-7">jorge.vasquez@ziverge.com</div>
</div>