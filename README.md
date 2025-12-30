// =====================================================
// PROJETO: NEXA - PLATAFORMA DE INGLÊS
// TECNOLOGIA: JAVA + SPRING BOOT
// =====================================================


// ======================= pom.xml =======================
/*
<project xmlns="http://maven.apache.org/POM/4.0.0">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.nexa</groupId>
  <artifactId>nexa</artifactId>
  <version>1.0</version>

  <properties>
    <java.version>17</java.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>

    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <dependency>
      <groupId>com.mysql</groupId>
      <artifactId>mysql-connector-j</artifactId>
    </dependency>
  </dependencies>
</project>
*/


// ========== src/main/java/com/nexa/NexaApplication.java ==========
package com.nexa;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class NexaApplication {
    public static void main(String[] args) {
        SpringApplication.run(NexaApplication.class, args);
    }
}


// ========== src/main/java/com/nexa/model/Usuario.java ==========
package com.nexa.model;

import jakarta.persistence.*;

@Entity
public class Usuario {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String nome;
    private String email;
    private String senha;
    private int pontos;

    public Long getId() { return id; }
    public String getNome() { return nome; }
    public String getEmail() { return email; }
    public String getSenha() { return senha; }
    public int getPontos() { return pontos; }

    public void setNome(String nome) { this.nome = nome; }
    public void setEmail(String email) { this.email = email; }
    public void setSenha(String senha) { this.senha = senha; }
    public void setPontos(int pontos) { this.pontos = pontos; }
}


// ========== src/main/java/com/nexa/repository/UsuarioRepository.java ==========
package com.nexa.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import com.nexa.model.Usuario;

public interface UsuarioRepository extends JpaRepository<Usuario, Long> {
    Usuario findByEmail(String email);
}


// ========== src/main/java/com/nexa/service/UsuarioService.java ==========
package com.nexa.service;

import org.springframework.stereotype.Service;
import com.nexa.model.Usuario;
import com.nexa.repository.UsuarioRepository;

@Service
public class UsuarioService {

    private final UsuarioRepository repo;

    public UsuarioService(UsuarioRepository repo) {
        this.repo = repo;
    }

    public void salvar(Usuario usuario) {
        repo.save(usuario);
    }
}


// ========== src/main/java/com/nexa/controller/AuthController.java ==========
package com.nexa.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;
import com.nexa.model.Usuario;
import com.nexa.service.UsuarioService;

@Controller
public class AuthController {

    private final UsuarioService service;

    public AuthController(UsuarioService service) {
        this.service = service;
    }

    @GetMapping("/login")
    public String login() {
        return "login";
    }

    @GetMapping("/cadastro")
    public String cadastro() {
        return "cadastro";
    }

    @PostMapping("/cadastro")
    public String salvar(Usuario usuario) {
        usuario.setPontos(0);
        service.salvar(usuario);
        return "redirect:/login";
    }
}


// ========== src/main/java/com/nexa/controller/ConteudoController.java ==========
package com.nexa.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class ConteudoController {

    @GetMapping("/")
    public String index() {
        return "index";
    }

    @GetMapping("/aulas")
    public String aulas() {
        return "aulas";
    }

    @GetMapping("/exercicios")
    public String exercicios() {
        return "exercicios";
    }

    @GetMapping("/tarefas")
    public String tarefas() {
        return "tarefas";
    }
}


// ========== src/main/java/com/nexa/controller/ApiController.java ==========
package com.nexa.controller;

import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api")
public class ApiController {

    @GetMapping("/status")
    public String status() {
        return "Nexa API Online";
    }
}


// ======================= HTML =======================
// src/main/resources/templates/index.html
/*
<h1>Nexa - English Platform</h1>
<a href="/login">Login</a>
<a href="/cadastro">Cadastro</a>
<a href="/aulas">Aulas</a>
<a href="/exercicios">Exercícios</a>
<a href="/tarefas">Tarefas</a>
*/

// login.html
/*
<h2>Login</h2>
<form>
  Email: <input type="email"><br>
  Senha: <input type="password"><br>
  <button>Entrar</button>
</form>
*/

// cadastro.html
/*
<h2>Cadastro</h2>
<form method="post">
  Nome: <input name="nome"><br>
  Email: <input name="email"><br>
  Senha: <input name="senha" type="password"><br>
  <button>Cadastrar</button>
</form>
*/

// aulas.html
/*
<h2>Aulas</h2>
<p>Simple Present, Verb To Be</p>
*/

// exercicios.html
/*
<h2>Exercícios</h2>
<p>I ___ a student.</p>
*/

// tarefas.html
/*
<h2>Tarefas</h2>
<p>Write about your daily routine.</p>
*/


// ======================= CSS =======================
// src/main/resources/static/style.css
/*
body {
  font-family: Arial;
  text-align: center;
}
a {
  margin: 10px;
}
*/


// ======================= application.properties =======================
/*
spring.datasource.url=jdbc:mysql://localhost:3306/nexa
spring.datasource.username=root
spring.datasource.password=senha
spring.jpa.hibernate.ddl-auto=update
*/

