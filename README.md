# springsecurity-etude
Estudo sobre o spring security

## Um Endpoint simples
Antes de começar a implementação do Spring security será necessário implementar algo. Não haverá necessidade de implementar um Controller complexo e bem estruturado, por isso criaremos um pacote chamado Student e nele colocaremos uma classe de Dominio chamado **Student** e um controller chamado **StudentController**

Nas imagens a seguir está a implementação de cada.


![Imagem pacote Student](../assets/pacoteStudent-image1.PNG?raw=true)
<BR />
*Imagem 1 - Pacote Student contendo uma classe chamada **Student** e outra chamada **StudentController***


### Classe Student

```
public class Student {
    private final Integer studentId;
    private final String studentName;

    public Student(Integer studentId, String studentName) {
        this.studentId = studentId;
        this.studentName = studentName;
    }

    public Integer getStudentId() {
        return studentId;
    }

    public String getStudentName() {
        return studentName;
    }
}
```

### Classe StudentController
```
@RestController
@RequestMapping("api/v1/students")
public class StudentController {

    private static final List<Student> STUDENTS = Arrays.asList(
            new Student(1, "Maria"),
            new Student(2, "João"),
            new Student(3, "Pedro")
    );
    @GetMapping(path = "{studentId}")
    public Student getStudent(@PathVariable("studentId") Integer studentId){
        return STUDENTS.stream().filter(student->studentId.equals(student.getStudentId()))
                .findFirst()
                .orElseThrow(()-> new IllegalStateException("Student" + studentId + "Not found"));
    }
}

```
## Adicionando o Spring Security
Para adicionar o Spring Security, utilizaremos a seguinte depedência abaixo. Basta adicionar o seguinte bloco de código em nosso POM.XML como na imagem 02.

```
<dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
 </dependency>

```
![Imagem depedencia spring security](../assets/depedencia-springsecurity.png?raw=true)
<BR />
*Imagem 2 - Depedência **spring-boot-starter-security** no arquivo POM.XML*


## Basic Auth
O protocolo HTTP é comumente usado para a navegação na WEB sendo relacionado a utilização de URLs para acessar paginas, porém é importante destacar que sua utilição e seus recursos são mais abrangentes, suportando Métodos e Cabeçalhos (Tenembaum).

### Cabeçalhos(HEADER)
Cabeçalhos permitem a passagem de informações entre o cliente e servidor, podendo ser em uma solicitação ou resposta HTTP, assim podemos fazer a assimilação de que um cabeçalho sejam comparados a passagem de parametros para uma função. Cabeçalhos são formados por seu nome seguido por dois pontos ':' e seu valor. Exemplo:

Content-Type: text/html; charset=utf-8
Veja mais sobre esse cabeçalho: https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Headers/Content-Type

Não fugindo do foco da explicação a respeito do Basic Auth, o HTTP nos fornece uma framework geral para a o controle de de acesso e autentição. O cabeçalho utilizado para essa tarefa é o Authorization.

Sintaxe:
Authorization: <tipo> <credenciais>

<Tipo>: Basic

<credenciais>: nome:senha

As credencias(nome:senha) são codificadas em base64 e tornam algo similar a isso:


De acordo com a imagem 3, podemos ver o fluxo de autentição com o HTTP. 


Refêrencias:
https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Authentication
https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Headers/Authorization
TANEMBAUM, Andrew S. Redes de Computadores.
https://datatracker.ietf.org/doc/html/rfc7235#section-4.1
https://www.vivaolinux.com.br/imagens/artigos/comunidade/Protocolo%20HTTP.pdf
https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Headers
https://www.w3.org/International/questions/qa-headers-charset.pt-br.html
https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Headers/Content-Type
