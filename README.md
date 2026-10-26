# JogoDaVelha2
Projeto da faculdade usando Objetos.

Parte principal do código (Main) 

import java.util.Scanner;

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("Bem vindo ao Jogo da Velha");
        System.out.print("Digite o nome do Jogador 1 (X): ");
        String nome1 = scanner.nextLine();
        System.out.print("Digite o nome do Jogador 2 (O): ");
        String nome2 = scanner.nextLine();

        // Vetor heterogêneo/estruturado de jogadores contendo nome, pontos e peça
        Jogador[] jogadores = {
                new Jogador(nome1, "X"),
                new Jogador(nome2, "O")
        };

        boolean continuarJogando = true;

        while (continuarJogando) {
            Tabuleiro tabuleiro = new Tabuleiro();
            int jogadorAtualIndex = 0;
            int totalJogadas = 0;
            boolean temVencedor = false;

            while (totalJogadas < 9 && !temVencedor) {
                tabuleiro.exibirTabuleiro();

                Jogador jogadorAtual = jogadores[jogadorAtualIndex];
                String simbolo = jogadorAtual.getPeca().getSimbolo();
                String nomeAtual = jogadorAtual.getNome();

                System.out.println("Vez de " + nomeAtual + " (" + simbolo + ")");

                int linha, coluna;

                while (true) {
                    System.out.print("Escolha a linha (0, 1 ou 2): ");
                    linha = scanner.nextInt();
                    System.out.print("Escolha a coluna (0, 1 ou 2): ");
                    coluna = scanner.nextInt();

                    if (linha >= 0 && linha <= 2 && coluna >= 0 && coluna <= 2) {
                        if (tabuleiro.estaVazio(linha, coluna)) {
                            break;
                        } else {
                            System.out.println("Ops! Essa posição já está ocupada. Tente novamente.");
                        }
                    } else {
                        System.out.println("Posição inválida! Use valores entre 0 e 2.");
                    }
                }

                tabuleiro.fazerJogada(linha, coluna, simbolo);
                totalJogadas++;

                temVencedor = tabuleiro.verificarVitoria(simbolo);

                if (temVencedor) {
                    tabuleiro.exibirTabuleiro();
                    System.out.println("Fim de jogo! " + nomeAtual + " venceu esta partida!");
                    jogadorAtual.incrementarPontos();
                } else if (totalJogadas == 9) {
                    tabuleiro.exibirTabuleiro();
                    System.out.println("Fim de jogo! O tabuleiro está cheio. Empate!");
                } else {
                    jogadorAtualIndex = 1 - jogadorAtualIndex; // Alternância de jogadores
                }
            }

            System.out.println("\n Placar Atual: ");
            System.out.println(jogadores[0].getNome() + ": " + jogadores[0].getPontos() + " ponto(s)");
            System.out.println(jogadores[1].getNome() + ": " + jogadores[1].getPontos() + " ponto(s)");
            System.out.println("--------------------");

            System.out.print("\nDeseja jogar outra partida? (s/n): ");
            char resposta = scanner.next().toLowerCase().charAt(0);
            if (resposta != 's') {
                continuarJogando = false;
            }
        }

        System.out.println("\nObrigado por jogar! Placar final encerrado.");
        scanner.close();
    }
