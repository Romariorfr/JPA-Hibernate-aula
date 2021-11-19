# Aula JPA e Hibernate
###### Estudo focado no aprendizado de mapeamento objeto-relacional com JPA.

## JPA
Java Persistence API (JPA) é a especificação padrão da plataforma Java EE (pacote javax.persistence) para mapeamento objeto-relacional e persistência de dados.
JPA é apenas uma especificação (JSR 338):
http://download.oracle.com/otn-pub/jcp/persistence-2_1-fr-eval-spec/JavaPersistence.pdf

Para trabalhar com JPA é preciso incluir no projeto uma implementação da API (ex: Hibernate).

Arquitetura de uma aplicação que utiliza JPA:

![myImage](https://github.com/Romariorfr/aula-JPA--Hibernate/blob/master/img-arquitetura-jpa.png)



### Principais classes:

#### EntityManager
https://docs.oracle.com/javaee/7/api/javax/persistence/EntityManager.html

Um objeto EntityManager encapsula uma conexão com a base de dados e serve para efetuar operações de acesso a dados (inserção, remoção, deleção, atualização) em entidades (clientes, produtos, pedidos, etc.) por ele monitoradas em um mesmo contexto de persistência.

Escopo: tipicamente mantem-se uma instância única de EntityManager para cada thread do sistema (no caso de aplicações web, para cada requisição ao sistema). 

#### EntityManagerFactory
https://docs.oracle.com/javaee/7/api/javax/persistence/EntityManagerFactory.html

Um objeto EntityManagerFactory é utilizado para instanciar objetos EntityManager.

Escopo: tipicamente mantem-se uma instância única de EntityManagerFactory para toda aplicação.

## 1. Metodos e funcionalidades 🎁
### 1.1 Descrição:
Foi criado uma classe chamado pessoa contendo os atributos id,nome,email, e feito o mapeamento objeto relacional(ORM) dessa classe com as anotações do JPA.

```Java
@Entity
public class Pessoa implements Serializable {
		
	private static final long serialVersionUID = 1L;
	
	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	private Integer id;
	private String nome;
	private String email;
	
	public Pessoa() {
		
	}
```

No programa principal, foi instanciada três pessoas e também um objeto EntityManagerFactory e um EntityManager. Apartir do metodo persist do EntityManager, é passado o objeto pessoa, que será posteriormente salvo no banco de dados.

```Java
	public static void main(String[] args) {

		Pessoa p1 = new Pessoa(null, "Carlos da Silva", "carlos@gmail.com");
		Pessoa p2 = new Pessoa(null, "Joaquim Torres", "joaquim@gmail.com");
		Pessoa p3 = new Pessoa(null, "Ana Maria", "ana@gmail.com");

		EntityManagerFactory emf = Persistence.createEntityManagerFactory("exemplo-jpa");
		EntityManager em = emf.createEntityManager();
		em.getTransaction().begin();
		em.persist(p1);
		em.persist(p2);
		em.persist(p3);
		em.getTransaction().commit();

		System.out.println("Pronto!");
		em.close();
		emf.close();

	}
```

