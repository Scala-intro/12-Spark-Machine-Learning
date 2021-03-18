# 12-Spark-Machine-Learning
1. [Arrancar Zeppelin ](#schema1)
2. [Importación de librerías ](#schema2)
3. [Convertir los datos a tipo Rating](#schema3)
4. [Empezamos a aplicarle el algoritmo y el proceso matemático](#schema4)
5. [ Evaluación del modelo](#schema5)




6. [Creamos el contador de palabras](#schema6)
7. [Ponemos a la escucha el programa](#schema7)



<hr>

<a name="schema1"></a>

# 1. Arrancar Zeppelin
Navegamos en la consola hasta llegar donde tenemos descargados la carpeta Zeppelin y ejecutamos:
~~~
bin/zeppelin-daemon.sh start
~~~

Seguidamente abrimos un página en el navegador y vamos a `http://localhost:8080`, se nos abre zeppelin y creamos un nuevo notebook, llamado Temperatura Sensor y como intérprete elegimos `spark2`
<hr>

<a name="schema2"></a>

# 2. Importación de librerías

~~~scala
import org.apache.spark.{SparkConf, SparkContext}
import org.apache.spark.mllib.recommendation.ALS
import org.apache.spark.mllib.recommendation.MatrixFactorizationModel
import org.apache.spark.mllib.recommendation.Rating
~~~
<hr>

<a name="schema3"></a>

# 3. Convertir los datos a tipo Rating

~~~scala
val ratings = data.map(_.split(",") match {
    case Array(user,item,rate) => Rating(user.toInt, item.toInt, rate.toDouble)
})
~~~
<hr>

<a name="schema4"></a>

# 4. Empezamos a aplicarle el algoritmo y el proceso matemático

`rank` características que quieren medir
`numeroIteraciones` cuantas veces se hace el proceso
`modelo` modelo que se va a realizar
`0.01` grado de precisión
~~~scala
val rank = 10
val numeroIteraciones = 10
val modelo= ALS.train(ratings, rank, numeroIteraciones, 0.01)
~~~


<hr>

<a name="schema5"></a>

# 5. Evaluación del modelo


~~~scala
val productoUsuarios = ratings.map{
    case Rating(user, product,rate) => (user, product)
}

val predicciones = modelo.predict(productoUsuarios).map {
    case Rating(user, product, rate) => ((user,product),rate)
}
~~~