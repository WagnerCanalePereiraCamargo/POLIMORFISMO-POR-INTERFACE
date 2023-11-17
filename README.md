# POLIMORFISMO-POR-INTERFACE

Vamos adaptar o exemplo do CRUD com arquivos texto para um sistema que guarda links úteis por assunto, utilizando polimorfismo por interface. Neste caso, podemos criar uma interface chamada `GerenciadorLinks` que define métodos para adicionar, editar, excluir e exibir links. Em seguida, implementamos essa interface em uma classe específica para manipular links por assunto.

Aqui está um exemplo simplificado em Java:

```java
import java.io.*;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

// Interface para o gerenciamento de links
interface GerenciadorLinks {
    void adicionarLink(String assunto, String link);
    void editarLink(String assunto, int indice, String novoLink);
    void excluirLink(String assunto, int indice);
    void exibirLinks(String assunto);
}

// Implementação da interface GerenciadorLinks para links úteis
class GerenciadorLinksUteis implements GerenciadorLinks {
    private List<AssuntoLinks> listaAssuntos = new ArrayList<>();

    // Adiciona um link para um assunto
    @Override
    public void adicionarLink(String assunto, String link) {
        AssuntoLinks assuntoLinks = obterAssunto(assunto);
        assuntoLinks.adicionarLink(link);
        salvarArquivo();
    }

    // Edita um link de um assunto
    @Override
    public void editarLink(String assunto, int indice, String novoLink) {
        AssuntoLinks assuntoLinks = obterAssunto(assunto);
        assuntoLinks.editarLink(indice, novoLink);
        salvarArquivo();
    }

    // Exclui um link de um assunto
    @Override
    public void excluirLink(String assunto, int indice) {
        AssuntoLinks assuntoLinks = obterAssunto(assunto);
        assuntoLinks.excluirLink(indice);
        salvarArquivo();
    }

    // Exibe todos os links de um assunto
    @Override
    public void exibirLinks(String assunto) {
        AssuntoLinks assuntoLinks = obterAssunto(assunto);
        assuntoLinks.exibirLinks();
    }

    // Carrega os dados do arquivo ao inicializar
    public void carregarArquivo() {
        try {
            ObjectInputStream ois = new ObjectInputStream(new FileInputStream("links.dat"));
            listaAssuntos = (List<AssuntoLinks>) ois.readObject();
            ois.close();
        } catch (IOException | ClassNotFoundException e) {
            // O arquivo ainda não existe ou ocorreu um erro durante a leitura
        }
    }

    // Salva os dados no arquivo
    private void salvarArquivo() {
        try {
            ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("links.dat"));
            oos.writeObject(listaAssuntos);
            oos.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    // Obtém um assunto específico ou cria um novo se não existir
    private AssuntoLinks obterAssunto(String assunto) {
        for (AssuntoLinks assuntoLinks : listaAssuntos) {
            if (assuntoLinks.getAssunto().equalsIgnoreCase(assunto)) {
                return assuntoLinks;
            }
        }

        // Se o assunto não existir, cria um novo
        AssuntoLinks novoAssunto = new AssuntoLinks(assunto);
        listaAssuntos.add(novoAssunto);
        return novoAssunto;
    }
}

// Classe que representa um assunto com links
class AssuntoLinks implements Serializable {
    private String assunto;
    private List<String> links = new ArrayList<>();

    public AssuntoLinks(String assunto) {
        this.assunto = assunto;
    }

    public String getAssunto() {
        return assunto;
    }

    public void adicionarLink(String link) {
        links.add(link);
    }

    public void editarLink(int indice, String novoLink) {
        if (indice >= 0 && indice < links.size()) {
            links.set(indice, novoLink);
        }
    }

    public void excluirLink(int indice) {
        if (indice >= 0 && indice < links.size()) {
            links.remove(indice);
        }
    }

    public void exibirLinks() {
        System.out.println("Links para o assunto '" + assunto + "':");
        for (int i = 0; i < links.size(); i++) {
            System.out.println((i + 1) + ". " + links.get(i));
        }
    }
}

public class Main {
    public static void main(String[] args) {
        GerenciadorLinks gerenciador = new GerenciadorLinksUteis();
        ((GerenciadorLinksUteis) gerenciador).carregarArquivo();

        Scanner scanner = new Scanner(System.in);

        int opcao;
        do {
            System.out.println("\nMenu:");
            System.out.println("1. Adicionar link");
            System.out.println("2. Editar link");
            System.out.println("3. Excluir link");
            System.out.println("4. Exibir links

# explicação do codigo em questão

Vamos analisar e explicar o código linha por linha:

1. **`import java.io.*;`**: Importa todas as classes do pacote `java.io`, que são necessárias para manipulação de arquivos.

2. **`import java.util.ArrayList;`**: Importa a classe `ArrayList` do pacote `java.util`, utilizada para criar listas dinâmicas.

3. **`import java.util.List;`**: Importa a interface `List` do pacote `java.util`, que é uma interface para coleções de elementos ordenados.

4. **`interface GerenciadorLinks { ... }`**: Declaração da interface `GerenciadorLinks`, que contém métodos para adicionar, editar, excluir e exibir links. Serve como contrato para implementações específicas.

5. **`class GerenciadorLinksUteis implements GerenciadorLinks { ... }`**: Implementação da interface `GerenciadorLinks` na classe `GerenciadorLinksUteis`, que gerencia links úteis por assunto.

6. **`List<AssuntoLinks> listaAssuntos = new ArrayList<>();`**: Declaração e inicialização de uma lista de objetos `AssuntoLinks` para armazenar diferentes assuntos e seus links.

7. **`@Override public void adicionarLink(String assunto, String link) { ... }`**: Implementação do método para adicionar um link a um assunto específico.

8. **`@Override public void editarLink(String assunto, int indice, String novoLink) { ... }`**: Implementação do método para editar um link de um assunto específico.

9. **`@Override public void excluirLink(String assunto, int indice) { ... }`**: Implementação do método para excluir um link de um assunto específico.

10. **`@Override public void exibirLinks(String assunto) { ... }`**: Implementação do método para exibir todos os links de um assunto específico.

11. **`public void carregarArquivo() { ... }`**: Método para carregar os dados do arquivo ao inicializar.

12. **`private void salvarArquivo() { ... }`**: Método privado para salvar os dados no arquivo.

13. **`private AssuntoLinks obterAssunto(String assunto) { ... }`**: Método privado para obter um assunto específico ou criar um novo se não existir.

14. **`class AssuntoLinks implements Serializable { ... }`**: Declaração da classe `AssuntoLinks`, que representa um assunto com links. Implementa a interface `Serializable` para permitir a serialização.

15. **`public class Main { ... }`**: Classe principal que contém o método `main` onde o programa é iniciado.

16. **`GerenciadorLinks gerenciador = new GerenciadorLinksUteis();`**: Criação de uma instância de `GerenciadorLinksUteis` para gerenciar os links.

17. **`((GerenciadorLinksUteis) gerenciador).carregarArquivo();`**: Chama o método `carregarArquivo` para carregar os dados do arquivo ao iniciar o programa.

18. **`Scanner scanner = new Scanner(System.in);`**: Criação de um objeto `Scanner` para leitura de entrada do usuário.

19. **`int opcao;`**: Declaração da variável `opcao` que será utilizada para armazenar a escolha do usuário no menu.

20. **`do { ... } while (...)`**: Início de um loop `do-while` que apresenta um menu ao usuário e executa as operações correspondentes até que o usuário escolha sair.

21. **`System.out.println("\nMenu:"); ...`**: Exibe as opções do menu para o usuário.

22. **`} while (opcao != 5);`**: Finaliza o loop quando a opção escolhida pelo usuário é sair.

Este é um resumo simplificado do código, destacando os principais elementos e funcionalidades.

#  O polimorfismo por interface é uma abordagem que permite que objetos de classes diferentes sejam tratados de maneira uniforme, desde que implementem a mesma interface. Essa técnica oferece flexibilidade e desacoplamento, pois as interfaces definem contratos, especificando métodos que as classes que as implementam devem fornecer. Isso promove um baixo acoplamento, tornando o código mais flexível em termos de hierarquia de classes.

Uma vantagem significativa do polimorfismo por interface é a capacidade de uma classe implementar várias interfaces. Isso é útil quando diferentes comportamentos precisam ser incorporados a uma única classe, possibilitando maior modularidade e reutilização de código.

Em termos de manutenção e evolução do sistema, o polimorfismo por interface demonstra sua eficácia. Mudanças nas interfaces não afetam as classes existentes, desde que essas classes continuem a implementar a interface. Isso facilita a evolução do sistema sem impactar a implementação existente.

Outro ponto a ser considerado é o princípio da composição sobre herança, favorecido pelo polimorfismo por interface. Essa abordagem promove a construção de classes complexas combinando funcionalidades através de interfaces, ajudando a evitar os problemas associados à herança excessiva.

O uso de polimorfismo por interface é especialmente adequado para ambientes onde a flexibilidade, a manutenção e a evolução são cruciais, como em grandes sistemas corporativos e em projetos de longa duração. Contratos explícitos estabelecidos pelas interfaces tornam claro o que é esperado de cada classe que as implementa.

Em comparação com o polimorfismo por herança, o polimorfismo por interface oferece uma abordagem mais flexível e modular. No entanto, a escolha entre essas abordagens dependerá do contexto específico do projeto e dos requisitos do sistema. Em muitos casos, uma combinação equilibrada de ambas as técnicas pode ser a solução mais apropriada, permitindo a criação de sistemas robustos e de fácil manutenção.
