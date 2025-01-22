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



# Bibliografía:

1. insightsoftware. (n.d.). *What is JDBC?* Recuperado de [https://insightsoftware.com/es/blog/what-is-jdbc/](https://insightsoftware.com/es/blog/what-is-jdbc/)  
2. Flanagan, M. (2005). *JDBC: Conexión entre Java y bases de datos.* Recuperado de [http://flanagan.ugr.es/docencia/2005-2006/2/servlets/jdbc.html](http://flanagan.ugr.es/docencia/2005-2006/2/servlets/jdbc.html)
3. Alura. (n.d.). *Conociendo el JDBC.* Recuperado de [https://www.aluracursos.com/blog/conociendo-el-jdbc](https://www.aluracursos.com/blog/conociendo-el-jdbc)
4. Code and Coke. (n.d.). *JDBC: Apuntes.* Recuperado de [https://datos.codeandcoke.com/apuntes:jdbc](https://datos.codeandcoke.com/apuntes:jdbc)  
