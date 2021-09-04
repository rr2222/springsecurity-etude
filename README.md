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

