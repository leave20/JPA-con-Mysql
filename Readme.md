# PROYECTO JPA CON MYSQL 

Para crear este proyecto necesitaremos las siguientes herramientas:

  - MySQL Workbench
  - Xamp
  - Apache Netbeans IDE

### Creando nuestro proyecto
Nuevo proyecto
File > New Project>Java Proyect
Modificamos nuestro archivo de Maven abriendo el POM.xml
Y hacemos las siguientes modificaciones:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>mx.com.gm.sga</groupId>
    <artifactId>holamundo-jpa</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>
    <dependencies>
        <dependency>
            <groupId>javax</groupId>
            <artifactId>javaee-api</artifactId>
            <version>8.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>javax.persistence</groupId>
            <artifactId>javax.persistence-api</artifactId>
            <version>2.2</version>            
        </dependency>
        <dependency>
            <groupId>org.eclipse.persistence</groupId>
            <artifactId>eclipselink</artifactId>
            <version>2.7.4</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.20</version>
        </dependency>
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-api</artifactId>
            <version>2.12.1</version>
        </dependency>
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-core</artifactId>
            <version>2.12.1</version>
        </dependency>
    </dependencies>
    
</project>
```
Cambiamos la versión del jdk en 'maven.compiler.target' por el 1.8
Tambien agregamos las siguientes dependencias:
- Javax
- Javaee-api
- 8.0.1

Agregamos tambien:
- javax.persistence
- javax.persistence-api
- 2.2

Y con esto ya tenemos la libreria de JPA
Tambien debemos agregaruna implementación del API JDA que es eclipselink:
- org.eclipse.persistence
- eclipselink
- 2.7.4

Agregamos la dependencia del controlador de MySQL:
- mysql
- mysql-connector-java
- 8.0.20

Agregamos la dependencia de log4j:
- org.apache.logging.log4j
- log4j-api
- 2.12.1

Agregamos la implementacion:
- org.apache.logging.log4j
- log4j-core
- 2.12.1

Para descargar todas estas librerías deberemos dar click derecho en nuestro proyecto y luego 'clean and build'
Ahora vamos a necesitar la carpeta resources
Sobre Nuestro proyecto , creamos New , folder , hacemos click en Browse , luego seleccionamos la ruta src >main y dentro de main , seleccionamos la carpeta y creamos el nombre del forlder de resources y damos en finish.
Dentro creamos un archivo XML con el nombre log4j2 , le damos click en Next y finish
Llenamos el archivo con lo siguiente:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <Appenders>
        <Console name="Console" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{hh:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>

        </Console>
    </Appenders>
    <Loggers>
        <Root level="debug">
            <AppenderRef ref="Console"/>
        </Root>
    </Loggers>
</configuration>
```

Luego de terminado el archivo log4j , creamos en source package , new java class con el nombre de Personay el nombre del paquete es mx.com.gm.sga.domain y damos click en finalizar.
La clase la convertiremos en una entidad como lo tenemos en la descripción:
```java
package mx.com.gm.sga.domain;

import java.io.Serializable;
import javax.persistence.*;

@Entity
public class Persona implements Serializable{
    private static final long serialVersionUID=1L;
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name="idpersona")
    private int idPersona;
    private String nombre;
    private String apellido;
    private String email;
    private String telefono;
    public  Persona(){
    }
    public Persona( String nombre, String apellido, String email, String telefono) {
        this.nombre = nombre;
        this.apellido = apellido;
        this.email = email;
        this.telefono = telefono;
    }
    public int getIdPersona() {
        return idPersona;
    }

    public void setIdPersona(int idPersona) {
        this.idPersona = idPersona;
    }

    public String getNombre() {
        return nombre;
    }

    public void setNombre(String nombre) {
        this.nombre = nombre;
    }

    public String getApellido() {
        return apellido;
    }

    public void setApellido(String apellido) {
        this.apellido = apellido;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getTelefono() {
        return telefono;
    }

    public void setTelefono(String telefono) {
        this.telefono = telefono;
    }

    @Override
    public String toString() {
        return "Persona{" + "idPersona=" + idPersona + ", nombre=" + nombre + ", apellido=" + apellido + ", email=" + email + ", telefono=" + telefono + '}';
    }
}
```
Agregamos la configuracion del JPA dentro del folder other sources >src/main/resources creamos el folder META-INF y damos en Finish
y dentro crearemos el archivo de tipo XML con el nombre de persistence , le damos siguiente y finish
LLenamos el archivo con lo siguiente:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence
    http://xmlns.jcp.org/xml/ns/persistence/persistence_2_2.xsd" version="2.2"
>
    <persistence-unit name="PersonaPU" transaction-type="RESOURCE_LOCAL">
        <class>mx.com.gm.sga.domain.Persona</class>
        <properties>
            <property name="javax.persistence.jdbc.url" value="jdbc:mysql://localhost:3306/clase_java?useSSL=false&amp;
userTimezone=true&amp;
serverTimezone=UTC"/>
            <property name="javax.persistence.jdbc.user" value="root"/>
            <property name="javax.persistence.jdbc.password" value=""/>
            <property name="javax.persistence.jdbc.driver" value="com.mysql.cj.jdbc.MysqlDataSource"/>
            <property name="eclipselink.logging.level.sql" value="FINE"/>
            <property name="eclipselink.logging.parameters" value="true"/>
        </properties>
    </persistence-unit>

</persistence>

```
Finalmente crearemos una clase de prueba dentro de Source Packages con el nombre de ClienteEntidadPersona con un paquete de nombre clase_java , luego llenamos la clase con el siguiente codigo:
```java
package clase_java;

import javax.persistence.*;
import mx.com.gm.sga.domain.Persona;
import org.apache.logging.log4j.*;


public class ClienteEntidadPersona {
    static Logger log=LogManager.getRootLogger();
    public static void main(String[] args) {
        EntityManagerFactory emf=Persistence.createEntityManagerFactory("PersonaPU");
        EntityManager em=emf.createEntityManager();
        EntityTransaction tx=em.getTransaction();
        
        //Inicia la transaccion
        tx.begin();
        
        //No se debe especificar el ID de la base de datos
        Persona persona1=new Persona("Craig","Castro","sdsdsd@gmail.com","852369854");
        log.debug("Objeto a persistir:"+ persona1);
        //Persistimos el objeto
        em.persist(persona1);
        //terminamos la transaccion
        tx.commit();
        log.debug("Objeto persistido" + persona1);
        em.close();
        
    }
}
```
Damos RUN para ejecutar la clase principal ClienteEntidadPersona y en la consola de nuestro IDE deberá aparecer el siguiente enunciado:
```md
Scanning for projects...
                                                                        
------------------------------------------------------------------------
Building holamundo-jpa 1.0-SNAPSHOT
------------------------------------------------------------------------

--- exec-maven-plugin:1.5.0:exec (default-cli) @ holamundo-jpa ---
[EL Info]: 2020-05-25 14:37:52.597--ServerSession(500230084)--EclipseLink, version: Eclipse Persistence Services - 2.7.4.v20190115-ad5b7c6b2a
[EL Info]: connection: 2020-05-25 14:37:53.188--ServerSession(500230084)--/file:/C:/Users/Craig/Documents/NetBeansProjects/holamundo-jpa/target/classes/_PersonaPU login successful
02:37:53 [main] DEBUG  - Objeto a persistir:Persona{idPersona=0, nombre=Craig, apellido=Castro, email=sdsdsd@gmail.com, telefono=852369854}
[EL Fine]: sql: 2020-05-25 14:37:53.229--ClientSession(1055104416)--Connection(1563634025)--INSERT INTO PERSONA (APELLIDO, EMAIL, NOMBRE, TELEFONO) VALUES (?, ?, ?, ?)
	bind => [Castro, sdsdsd@gmail.com, Craig, 852369854]
[EL Fine]: sql: 2020-05-25 14:37:53.291--ClientSession(1055104416)--Connection(1563634025)--SELECT LAST_INSERT_ID()
02:37:53 [main] DEBUG  - Objeto persistidoPersona{idPersona=4, nombre=Craig, apellido=Castro, email=sdsdsd@gmail.com, telefono=852369854}
------------------------------------------------------------------------
BUILD SUCCESS
------------------------------------------------------------------------
Total time: 9.221 s
Finished at: 2020-05-25T14:37:53-05:00
Final Memory: 7M/27M
------------------------------------------------------------------------

```
Para verificar los datos , deberemos revisar nuestro workbench haciendo una consulta: 
```SQL
SELECT * FROM clase_java.persona
```
Y    listo.
