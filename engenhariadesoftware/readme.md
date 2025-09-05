# Atividade 01
## 1. Comentário sobre o primeiro trecho do livro Software Engineering at Google, Oreilly 
O trecho diferencia bem programação (escrever código) de engenharia de software (práticas e teoria pra construir um software mais confiável). Faz a comparação com engenharias tradicionais pra dizer que quando o erro pode matar ou quebrar coisas físicas, as regras são rígidas; software ainda vive numa zona cinza. Na minha opinião, especialmente no Brasil, tem muita confusão entre nomes e responsabilidades (Ciências da Computação, Sistemas de Informação, Engenharia da Computação, etc.), e isso acaba deixando a profissão menos regulamentada e mais “informal”.

## 2. Comentário sobre o segundo trecho do livro Software Engineering at Google, Oreilly
Deve ser de compreensão geral que a engenharia de software é muito mais do que só programação; é também tempo, escala, trade-offs e tudo o que determinada organização utiliza para escrever e manter o código. Eu imagino que a longa experiência do Google e de outras big techs poderá ser um dos principais fatores na evolução da engenharia de software nos próximos anos, encontrando a solução de grandes problemas como o desenvolvimento de melhores práticas para a sustentabilidade do software a longo prazo. 

## 3. Listar e explicar 3 exemplos de trade-offs
1. Escalabilidade x Entrega -> investir tempo e esforço para arquitetar o sistema de modo a aguentar muito tráfego; priorizar a entrega rápida de uma funcionalidade/produto ao usuário.

2. Segurança × Agilidade -> endurecer processos (revisões, testes, approval) reduz velocidade de entrega; agilizar entrega pode abrir brechas.

3. Simplicidade × Flexibilidade -> código simples/vertical resolve momentaneamente e é fácil de manter; uma arquitetura mais flexível permite mudanças futuras mas aumenta a complexidade inicial.

## 4. Diagramas de Classes UML
<img width="1266" height="648" alt="image" src="https://github.com/user-attachments/assets/256f0623-b455-4ba4-81f0-229f17fcbb21" />

## 5. Código Java
```Java
package Main;

public class Livro {

    private String nome;
    private boolean disponivel;


    public String getNome() {
        return nome;
    }

    public void setNome(String nome) {
        this.nome = nome;
    }

    public boolean getDisponivel() {
        return disponivel;
    }

    public void setDisponivel(boolean disponivel) {
        this.disponivel = disponivel;
    }

    public Livro(String nome, boolean disponivel) {
        this.nome = nome;
        this.disponivel = disponivel;
    }
}
```

```Java
package Main;

public class Usuario {

    private String nome;


    public String getNome() {
        return nome;
    }

    public void setNome(String nome) {
        this.nome = nome;
    }


    public Usuario(String nome) {
        this.nome = nome;

    }
}
```

```Java
package Main;

public class Emprestimo {

    private String data;
    private Livro livro;
    private Usuario usuario;


    public String getData() {
        return data;
    }

    public void setData(String data) {
        this.data = data;
    }

    public Livro getLivro() {
        return livro;
    }

    public void setLivro(Livro livro) {
        this.livro = livro;
    }

    public Usuario getUsuario() {
        return usuario;
    }

    public void setUsuario(Usuario usuario) {
        this.usuario = usuario;
    }


    public Emprestimo(String data, Livro livro, Usuario usuario) {
        this.data = data;
        this.livro = livro;
        this.usuario = usuario;
    }
}
```

```Java
package Main;

import java.util.ArrayList;
import java.util.List;

public class Biblioteca {

    private List<Livro> estoqueLivros = new ArrayList<>();
    private List<Emprestimo> emprestimosRealizados = new ArrayList<>();
    private List<Usuario> usuariosCadastrados = new ArrayList<>();


    public List<Livro> getEstoqueLivros() {
        return estoqueLivros;
    }

    public List<Emprestimo> getEmprestimosRealizados() {
        return emprestimosRealizados;
    }

    public List<Usuario> getUsuariosCadastrados() {
        return usuariosCadastrados;
    }


    public void adicionarLivro(Livro livro) {
        this.estoqueLivros.add(livro);
        System.out.println("Livro '" + livro.getNome() + "' adicionado ao estoque.");
    }

    public void cadastrarUsuario(Usuario usuario) {
        this.usuariosCadastrados.add(usuario);
        System.out.println("Usuário '" + usuario.getNome() + "' cadastrado no sistema.");
    }

    public void emprestarLivro(Livro livro, Usuario usuario, String data) {
        if (livro.getDisponivel()) {
            if (this.usuariosCadastrados.contains(usuario)) {
                livro.setDisponivel(false);
                Emprestimo emprestimo = new Emprestimo(data, livro, usuario);
                emprestimosRealizados.add(emprestimo);

                System.out.println("\n----Empréstimo realizado----");
                System.out.println("Usuário: " + emprestimo.getUsuario().getNome());
                System.out.println("Livro: " + emprestimo.getLivro().getNome());
                System.out.println("Data: " + emprestimo.getData());
            }
            else {
                System.out.println("\n----Falha no empréstimo----");
                System.out.println("Usuário não cadastrado.");
            }
        }
        else {
            System.out.println("\n----Falha no empréstimo----");
            System.out.println("Livro indisponível no momento.");
        }
    }

}
```

```Java
package Main;

public class Main {
    public static void main(String[] args) {

        Biblioteca biblioteca = new Biblioteca();

        Livro livro1 = new Livro("Entendendo Algoritmos", true);
        Livro livro2 = new Livro("Código Limpo", false);
        Usuario usuario1 = new Usuario("Gustavo");
        Usuario usuario2 = new Usuario("Visitante");

        biblioteca.adicionarLivro(livro1);
        biblioteca.adicionarLivro(livro2);
        biblioteca.cadastrarUsuario(usuario1);

        biblioteca.emprestarLivro(livro1, usuario1, "17/11/2024");

    }
}
```

## 6. Testes Automatizados
```Java
package Test;

import Main.Biblioteca;
import Main.Livro;
import Main.Usuario;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.*;

class BibliotecaTest {

    private Biblioteca biblioteca;
    private Livro livroDisponivel;
    private Livro livroIndisponivel;
    private Usuario usuarioCadastrado;
    private Usuario usuarioNaoCadastrado;


    @BeforeEach
    void setUp() {

        biblioteca = new Biblioteca();
        livroDisponivel = new Livro("Entendendo Algoritmos", true);
        livroIndisponivel = new Livro("Código Limpo", false);
        usuarioCadastrado = new Usuario("Gustavo");
        usuarioNaoCadastrado = new Usuario("Visitante");

        biblioteca.adicionarLivro(livroDisponivel);
        biblioteca.adicionarLivro(livroIndisponivel);
        biblioteca.cadastrarUsuario(usuarioCadastrado);

    }

    @Test
    void deveEmprestarLivroComSucesso_QuandoLivroEstaDisponivelEUsuarioCadastrado() {

        biblioteca.emprestarLivro(livroDisponivel, usuarioCadastrado, "17/11/2024");

        assertEquals(1, biblioteca.getEmprestimosRealizados().size());
        assertFalse(livroDisponivel.getDisponivel());

    }

    @Test
    void naoDeveEmprestarLivro_QuandoLivroEstaIndisponivel() {

        biblioteca.emprestarLivro(livroIndisponivel, usuarioCadastrado, "17/11/2024");

        assertTrue(biblioteca.getEmprestimosRealizados().isEmpty());
        //assertEquals(0, biblioteca.getEmprestimosRealizados().size()); -> alternativa
    }

    @Test
    void naoDeveEmprestarLivro_QuandoUsuarioNaoEstaCadastrado() {

        biblioteca.emprestarLivro(livroDisponivel, usuarioNaoCadastrado, "17/11/2024");

        assertTrue(biblioteca.getEmprestimosRealizados().isEmpty());
        assertTrue(livroDisponivel.getDisponivel());

    }

    @Test
    void deveAdicionarUmLivroCorretamente() {

        Biblioteca novaBiblioteca = new Biblioteca();
        Livro novoLivro = new Livro("O Programador Pragmático", true);

        novaBiblioteca.adicionarLivro(novoLivro);

        assertEquals(1, novaBiblioteca.getEstoqueLivros().size());
        assertTrue(novaBiblioteca.getEstoqueLivros().contains(novoLivro));

    }
}
```
