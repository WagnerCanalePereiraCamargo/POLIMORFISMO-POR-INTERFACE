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

