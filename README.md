# TrabajoConsulta2B2-PFR - Conexión base de datos relacional
## ¿Qué es JDBC y cuáles son sus componentes?
### Qué es JDBC

JDBC **(Java Database Connectivity)** es una API estándar de Java que permite conectar aplicaciones Java con bases de datos SQL. Sus principales características son:

- Permite acceder a múltiples tipos de bases de datos desde Java de forma independiente del proveedor.
- Define un conjunto de interfaces y clases para interactuar con bases de datos.

Específicamente permite:
  - Establecer conexión con una base de datos
  - Ejecutar consultas SQL 
  - Procesar conjuntos de resultados

### Componentes principales de JDBC

Los componentes centrales de JDBC son:

1. Paquetes *java.sql* y *javax.sql*:
   Contienen las clases e interfaces que definen la interfaz entre Java y la base de datos.

2. Drivers JDBC:
   - Son clases que implementan la interfaz java.sql.Driver
   - Permiten conectar con bases de datos específicas
   - Pueden ser:
     - Puente JDBC-ODBC 
     - API nativa/Controlador parcial de Java
     - Controlador Java puro
     - Controlador Java de protocolo nativo

3. DriverManager:
   Clase que define operaciones básicas para manipular drivers y establecer conexiones.

4. Interfaces principales:
   - Connection: Representa una sesión con la base de datos
   - Statement/PreparedStatement: Ejecutan consultas SQL  
   - ResultSet: Almacena y procesa los resultados de una consulta

## Librerías de Scala que permitan conectarse a una base de datos relacional
### Slick

**Slick** (Scala Language Integrated Connection Kit) es una biblioteca funcional para acceder a bases de datos relacionales desde Scala. Se destaca por su enfoque tipo seguro, que reduce significativamente la posibilidad de errores comunes en el acceso a datos. Además, ofrece una API intuitiva para definir esquemas de bases de datos, realizar consultas complejas y ejecutar acciones de forma segura.

**Características principales:**

- **Tipo seguro:** Garantiza la integridad de los datos al compilar tiempo.
- **Funcional:** Utiliza un estilo funcional para definir consultas y acciones, lo que facilita la composición y la legibilidad del código.
- **Query DSL:** Proporciona un lenguaje específico para consultas (DSL) que permite escribir consultas de forma concisa y expresiva.
- **Soporte para múltiples bases de datos:** Funciona con una amplia variedad de bases de datos, incluyendo MySQL, PostgreSQL, SQL Server y más.
- **Integración con el ecosistema Scala:** Se integra fácilmente con otros componentes del ecosistema Scala, como el framework web Play Framework.

### Doobie
**Doobie** es una librería pura de Scala para trabajar con bases de datos JDBC, orientada a la funcionalidad y la seguridad en tiempo de compilación.  

**Características principales:**  
  - Trabaja directamente con JDBC, proporcionando control explícito sobre las conexiones y transacciones.
  - Totalmente funcional, con soporte para efectos y manejo de errores en tiempo de compilación.
  - Se integra bien con librerías funcionales como Cats y Cats Effect.

**Ventajas:**  
  - Diseño puro y funcional, ideal para proyectos funcionales.
  - Más control sobre las operaciones JDBC.
  - Fácil integración con herramientas de manejo de efectos.

**Desventajas:**  
  - Necesita más código manual en comparación con Slick.
  - Menos abstracción para consultas SQL complejas.

### Comparación: **Slick vs. Doobie**

| **Característica**            | **Slick**                                               | **Doobie**                                             |
|---------------------------    |---------------------------------------------------------|--------------------------------------------------------|
| **Paradigma**                 | Mixto (declarativo y funcional)                         | Funcional puro                                         |
| **Nivel de abstracción**      | Alto: DSL que abstrae las consultas SQL                 | Bajo: consultas SQL explícitas                         |
| **Facilidad de uso**          | Más amigable para principiantes                         | Más adecuado para usuarios con experiencia funcional   |
| **Control sobre JDBC**        | Limitado, abstrae detalles internos                     | Total control sobre JDBC y transacciones               |
| **Tamaño del ecosistema**     | Mayor, con amplia integración con Play y Akka           | Más pequeño, pero con buena integración con Cats       |
| **Gestión de consultas**      | Genera esquemas y consultas tipeadas automáticamente    | Consultas SQL escritas manualmente                     |
| **Rendimiento**               | Ligeramente menos eficiente por la abstracción          | Muy eficiente al estar cerca del nivel de JDBC         |
| **Documentación**             | Extensa y enfocada en ejemplos prácticos                | Completa pero orientada a usuarios funcionales         |
| **Conexión a bases de datos** | Soporta múltiples bases de datos                        | Soporta múltiples bases de datos                       |


## Cómo establecer una conexión a una base de datos relacional (mysql)
### Genere una base de datos en mysql

```mysql
-- Crear la base de datos
CREATE DATABASE testdb;

-- Usar la base de datos
USE testdb;

-- Crear una tabla con datos de prueba
CREATE TABLE employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    position VARCHAR(100),
    salary DECIMAL(10, 2)
);

-- Insertar datos de prueba
INSERT INTO employees (name, position, salary)
VALUES
    ('John Doe', 'Software Engineer', 75000.00),
    ('Jane Smith', 'Project Manager', 85000.00),
    ('Sam Wilson', 'Data Analyst', 65000.00);
```

### Genere una tabla con datos de prueba
![image](https://github.com/user-attachments/assets/c1671e8c-79b1-4fea-a387-3547b55ac755)

### Desde Scala establezca la conexión a la base datos
Ajustes necesarios para el *sbt* para el correcto funcionamiento del proyecto

```scala
ThisBuild / version := "0.1.0-SNAPSHOT"

ThisBuild / scalaVersion := "2.13.12"

lazy val root = (project in file("."))
  .settings(
    name := "untitled",
    libraryDependencies ++= Seq(
      "mysql" % "mysql-connector-java" % "8.0.32" // Version de Conector SQL
    )
  )

```

Codigo utilizado para la consulta de datos desde SQL en Scala:

```scala
import java.sql.{Connection, DriverManager, ResultSet}

object MySQLConnectionExample {

  def main(args: Array[String]): Unit = {
    // Configuración de la conexión
    // estructura del url: jdbc:mysql://<host>:<port>/<nombre del database>?<parameters>
    val url = "jdbc:mysql://localhost:3306/testdb?useSSL=false"
    val username = "root" // Cambia seegun el usuario ('root')
    val password = "utpl" // Cambia esto segun la contraseña de la conexion

    // Variables para la conexión y recursos
    var connection: Connection = null

    try {
      // Cargar el driver MySQL (opcional en versiones modernas)
      Class.forName("com.mysql.cj.jdbc.Driver")

      // Establecer la conexión
      connection = DriverManager.getConnection(url, username, password)
      println("Conexión exitosa a la base de datos.")

      // Crear un Statement
      val statement = connection.createStatement()

      // Consulta de todos los datos en la tabla 'employees'
      val query = "SELECT * FROM employees"
      val resultSet: ResultSet = statement.executeQuery(query)

      // Procesar y mostrar los resultados
      println("ID | Name          | Position          | Salary")
      println("-----------------------------------------------")
      while (resultSet.next()) {
        val id = resultSet.getInt("id")
        val name = resultSet.getString("name")
        val position = resultSet.getString("position")
        val salary = resultSet.getBigDecimal("salary")
        println(s"$id  | $name | $position | $salary")
      }

    } catch {
      case e: Exception =>
        e.printStackTrace() // Manejo de errores
    } finally {
      // Cerrar la conexión
      if (connection != null) {
        connection.close()
        println("Conexión cerrada.")
      }
    }
  }
}
```

### Muestra de la consulta de la tabla usada de prueba en Scala

![image](https://github.com/user-attachments/assets/2c2007da-f131-4a4f-bb62-83e2cb7d20dd)


# Bibliografía:

1. insightsoftware. (n.d.). *What is JDBC?* Recuperado de [https://insightsoftware.com/es/blog/what-is-jdbc/](https://insightsoftware.com/es/blog/what-is-jdbc/)  
2. Flanagan, M. (2005). *JDBC: Conexión entre Java y bases de datos.* Recuperado de [http://flanagan.ugr.es/docencia/2005-2006/2/servlets/jdbc.html](http://flanagan.ugr.es/docencia/2005-2006/2/servlets/jdbc.html)
3. Alura. (n.d.). *Conociendo el JDBC.* Recuperado de [https://www.aluracursos.com/blog/conociendo-el-jdbc](https://www.aluracursos.com/blog/conociendo-el-jdbc)
4. Code and Coke. (n.d.). *JDBC: Apuntes.* Recuperado de [https://datos.codeandcoke.com/apuntes:jdbc](https://datos.codeandcoke.com/apuntes:jdbc)
5. SoftwareMill. (n.d.). *Comparing Scala relational database access libraries.* Recuperado de [https://softwaremill.com/comparing-scala-relational-database-access-libraries/](https://softwaremill.com/comparing-scala-relational-database-access-libraries/)
6. Mustapha, M. (n.d.). *Relational DB with Scala and Slick.* Recuperado de [https://mustaphamichael.github.io/relational-db-with-scala-and-slick/](https://mustaphamichael.github.io/relational-db-with-scala-and-slick/)
7. Rock the JVM. (n.d.). *Getting started with Scala Slick.* Recuperado de [https://rockthejvm.com/articles/getting-started-with-scala-slick](https://rockthejvm.com/articles/getting-started-with-scala-slick) 
