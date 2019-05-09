# ANOTACIONES_JPA
Anotaciones básicas para la conexión a una base de datos relacional.

ANOTACIONES JPA PARA CONECTAR EL PROYECTO A UNA BASE DE DATOS RELACIONAL
-------------------------------------------------------------------------

DEFINICIÓN:En la persistencia se crean entidades (ejemplo entidad “Usuario”) y
cada entidad tiene sus atributos o propiedades que están enlazados por arte 	
de magia de la persistencia en la base de datos (ejemplo private Integer usuario_id).
Las anotaciones o announces permiten definir la persistencia de forma fácil, bonita y barata.

ANOTACIONES BASICAS QUE SE USAN PARA LA CONEXION A BDRELACIONAL
---------------------------------------------------------------

@Entity -->Especifica que voy a crear una entidad.
Se coloca al inicio de la definición de la clase.

@Entity
@Table(name=”users”)
public class User implements Serializable {

@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
@Column(name=”user_id”, nullable=false)
private Integer userId = null;

----------------------------------------------------------------
@Id Primary key de la entidad.

@Entity
@Table(name=”users”)
public class User implements Serializable {

@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
@Column(name=”user_id”, nullable=false)
private Integer userId = null;

----------------------------------------------------------------
@GeneratedValue --> Indica que esa clave se auto genera por medio de auto increment

----------------------------------------------------------------
@Colulmn --> Sirve para especificar que la clave está asociada a un atributo de la tabla.
Aprovecharemos para indicar si  puede ser nulo, si existe un límite de caracteres, etc.

@Column (name=”code”, nullable=false, length=50, unique=true)
private String code = null;

----------------------------------------------------------------
@Temporal --> Se usa normalmente en fechas.
Si definimos un elemento de tipo Date, deberemos indicarle en temporal que el tipo es timestamp.

@Column(name=”modification_date”)
@Temporal(TemporalType.TIMESTAMP)
private Date modificationDate = null;

o de tipo date si no queremos obtener la hora

@Column (name=”analysis_date”)
@Temporal(TemporalType.DATE)
private Date analysisDate = null;

----------------------------------------------------------------
@OneToMany.  Indicar una relación unidireccional de muchos a uno.

Ejemplo User N – N Roles

Entity user

@OneToMany(cascade = CascadeType.ALL, mappedBy = “user”)
private List<UserRole> userRoles = null;

Entity users_roles

JoinColumn(name = “user_id”, referencedColumnName = “user_id”, insertable=false, updatable=false)
@ManyToOne(optional = false)
private User user = null;

Con mappedBy -->estamos indicando quién es el dueño de la relación. En este caso…. está claro, no? ¿Dónde quiero tener guardado los roles del usuario? Tiene sentido que se guarden en el usuario por si se quieren obtener desde esa misma entidad.

----------------------------------------------------------------
@JoinColumn Indicamos la tabla con la que queremos hacer relación.
Ver los ejemplos anteriores.

----------------------------------------------------------------
@ManyToOne --> Podemos indicar  desde users_rols que una relación de esta entidad 
tiene muchas entidades de usuarios y muchas entidades de roles. En este caso utilizaremos:

@JoinColumn(name = “user_id”, referencedColumnName = “user_id”, insertable=false, updatable=false)
@ManyToOne(optional = false)
private User user = null;

@JoinColumn(name = “role_id”, referencedColumnName = “role_id”, insertable=false, updatable=false)
@ManyToOne(optional = false)
private Role role = null;

Para que quede claro el ejemplo: 1 usuario – n roles necesitaremos hacer

Definir el listado de roles dentro de “user” e indicar la relación OneToMany
En “users_roles” indicar la relación ManyToOne con respecto a usuarios y ManyToOne a roles
En “rol” indicar… nada! no se entera de nada!

----------------------------------------------------------------
@ManyToMany y @OneToOne 

OneToOne podría utilizarse para indicar que un usuario tiene una dirección dentro.

ManyToMany… 
