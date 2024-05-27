import java.io.*;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class Main {
    private static final String ARQUIVO_DADOS = "produtos.txt";
    private static List<Produto> produtos = new ArrayList<>();

    public static void main(String[] args) {
        carregarDados();
        exibirMenu();
    }

    private static void exibirMenu() {
        Scanner scanner = new Scanner(System.in);

        int opcao;
        do {
            System.out.println("\n=== MENU ===");
            System.out.println("1. Listar produtos");
            System.out.println("2. Adicionar produto");
            System.out.println("3. Atualizar produto");
            System.out.println("4. Deletar produto");
            System.out.println("5. Sair");
            System.out.print("Escolha uma opção: ");
            opcao = scanner.nextInt();

            switch (opcao) {
                case 1:
                    listarProdutos();
                    break;
                case 2:
                    adicionarProduto(scanner);
                    break;
                case 3:
                    atualizarProduto(scanner);
                    break;
                case 4:
                    deletarProduto(scanner);
                    break;
                case 5:
                    salvarDados();
                    System.out.println("Programa encerrado.");
                    break;
                default:
                    System.out.println("Opção inválida. Tente novamente.");
            }
        } while (opcao != 5);

        scanner.close();
    }

    private static void listarProdutos() {
        if (produtos.isEmpty()) {
            System.out.println("Nenhum produto cadastrado.");
        } else {
            System.out.println("=== LISTA DE PRODUTOS ===");
            for (Produto produto : produtos) {
                System.out.println(produto);
            }
        }
    }

    private static void adicionarProduto(Scanner scanner) {
        System.out.print("Nome do produto: ");
        String nome = scanner.next();

        System.out.print("Preço do produto: ");
        double preco = scanner.nextDouble();

        System.out.print("Quantidade do produto: ");
        int quantidade = scanner.nextInt();

        Produto produto = new Produto(nome, preco, quantidade);
        produtos.add(produto);
        System.out.println("Produto adicionado com sucesso.");
    }

    private static void atualizarProduto(Scanner scanner) {
        System.out.print("Digite o nome do produto a ser atualizado: ");
        String nome = scanner.next();

        for (Produto produto : produtos) {
            if (produto.getNome().equals(nome)) {
                System.out.print("Novo preço do produto: ");
                double novoPreco = scanner.nextDouble();
                produto.setPreco(novoPreco);

                System.out.print("Nova quantidade do produto: ");
                int novaQuantidade = scanner.nextInt();
                produto.setQuantidade(novaQuantidade);

                System.out.println("Produto atualizado com sucesso.");
                return;
            }
        }
        System.out.println("Produto não encontrado.");
    }

    private static void deletarProduto(Scanner scanner) {
        System.out.print("Digite o nome do produto a ser deletado: ");
        String nome = scanner.next();

        for (Produto produto : produtos) {
            if (produto.getNome().equals(nome)) {
                produtos.remove(produto);
                System.out.println("Produto deletado com sucesso.");
                return;
            }
        }
        System.out.println("Produto não encontrado.");
    }

    private static void carregarDados() {
        try (BufferedReader reader = new BufferedReader(new FileReader(ARQUIVO_DADOS))) {
            String linha;
            while ((linha = reader.readLine()) != null) {
                String[] partes = linha.split(";");
                String nome = partes[0];
                double preco = Double.parseDouble(partes[1]);
                int quantidade = Integer.parseInt(partes[2]);
                produtos.add(new Produto(nome, preco, quantidade));
            }
        } catch (IOException e) {
            System.out.println("Erro ao carregar dados: " + e.getMessage());
        }
    }

    private static void salvarDados() {
        try (PrintWriter writer = new PrintWriter(new FileWriter(ARQUIVO_DADOS))) {
            for (Produto produto : produtos) {
                writer.println(produto.getNome() + ";" + produto.getPreco() + ";" + produto.getQuantidade());
            }
        } catch (IOException e) {
            System.out.println("Erro ao salvar dados: " + e.getMessage());
        }
    }
}

class Produto {
    private String nome;
    private double preco;
    private int quantidade;

    public Produto(String nome, double preco, int quantidade) {
        this.nome = nome;
        this.preco = preco;
        this.quantidade = quantidade;
    }

    public String getNome() {
        return nome;
    }

    public void setNome(String nome) {
        this.nome = nome;
    }

    public double getPreco() {
        return preco;
    }

    public void setPreco(double preco) {
        this.preco = preco;
    }

    public int getQuantidade() {
        return quantidade;
    }

    public void setQuantidade(int quantidade) {
        this.quantidade = quantidade;
    }

    @Override
    public String toString() {
        return "Nome: " + nome + ", Preço: R$" + preco + ", Quantidade: " + quantidade;
    }
}
