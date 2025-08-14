# Jogo da Memória  

**Integrantes:**  
- Anny Kateliny de Lima Oliveira (**20241054010035**)  
- Ayslla Gabrielli da Silva Amorim (**20241054010066**)  
- Gabriela Morena de Oliveira Moreira (**20241054010001**)  
- Lucas Eduardo Oliveira do Nascimento (**20241054010025**)  

## Descrição do Projeto  
Este projeto consiste em um jogo da memória multiplayer que permite a comunicação entre jogadores em redes IPv4/IPv6, utilizando uma arquitetura **P2P** ou **cliente-servidor**, dependendo da implementação.  

O objetivo é desenvolver uma aplicação que funcione em diferentes ambientes de rede, garantindo a compatibilidade tanto com infraestrutura IPv4 quanto com redes mais modernas baseadas em IPv6.

## Instruções para Compilar e Executar  

### Pré-requisitos  
- Python **3.6+** instalado.  
- Acesso a uma rede (local ou Internet) para teste multiplayer.  
- Dois dispositivos (ou duas instâncias do jogo no mesmo computador) para partidas multiplayer.  

### Executando o jogo local  
1. Abra um terminal ou prompt de comando.  
2. Navegue até a pasta onde o arquivo está salvo.  
3. Execute o comando:  
   python jogo_da_memoria(local).py
4. O jogo iniciará automaticamente em uma janela gráfica.  

**Como jogar:**  
- Clique nas cartas para revelá-las e encontrar os pares.  
- Dois jogadores alternam turnos no mesmo computador.  

### Executando o Servidor (Multiplayer)  
1. O servidor deve ser iniciado primeiro para permitir conexões dos jogadores.  
2. Abra um terminal.  
3. Navegue até a pasta do arquivo.  
4. Execute:  
   python Server.py
5. **Saída esperada:**  
   Servidor iniciado na porta 12345 (suporta TCP e UDP simultaneamente para IPv4 e IPv6)
6. O servidor ficará aguardando dois jogadores se conectarem.  

### Executando os Clientes (Jogadores Multiplayer)  
1. Repita este processo em dois dispositivos (ou abra duas instâncias no mesmo computador).  
2. Abra um terminal.  
3. Navegue até a pasta do arquivo.  
4. Execute:  
   python Players.py
5. Na janela do jogo:  
   - Selecione o protocolo (**TCP** ou **UDP**).  
   - Insira o IP do servidor:  
     - IPv4 local: 127.0.0.1 
     - IPv6 local: ::1 
     - Rede externa: use o IP real do servidor.  
   - Clique em **"Conectar"**.  
6. Quando dois jogadores estiverem conectados, a partida começará automaticamente.  

## Descrição do Protocolo de Camada de Aplicação  

O protocolo desenvolvido define como clientes e servidor se comunicam durante a partida. Ele:  
- Opera sobre **TCP** ou **UDP**.  
- Suporta **IPv4/IPv6**.  
- Utiliza mensagens de texto simples para sincronizar o estado do jogo.  

### Estrutura das Mensagens  
COMANDO [argumento1] [argumento2] [...] [argumentoN]

- **COMANDO**: Palavra-chave que identifica o tipo de mensagem.  
- **Argumentos**: Dados adicionais separados por espaços.  

### Comandos de Controle  
- ID [número] → Atribui identificador ao jogador.  
- CONNECT → Solicitação de conexão inicial (UDP).  
- VEZ [jogador] → Indica de quem é a vez de jogar.  

### Comandos de Jogo  
- CLIQUE [índice] → Jogador seleciona carta.  
- MOSTRAR [índice] [emoji] → Revela carta no tabuleiro.  
- ESCONDER [índice1] [índice2] → Oculta cartas não pareadas.  
- ACERTO [índice1] [índice2] [jogador] → Confirma par encontrado.  

### Comandos de Finalização  
- FIM [vencedor] [mensagem] → Encerra a partida.  

## Fluxo da Conexão  

**Estabelecimento:**  
1. Cliente envia CONNECT (UDP) ou conecta via TCP.  
2. Servidor responde com ID [1/2].  
3. Quando dois jogadores conectam, servidor envia VEZ [1/2].  

**Durante a partida:**  
1. Servidor notifica VEZ [jogador].  
2. Jogador envia CLIQUE [índice].  
3. Servidor responde com:  
   - MOSTRAR [índice] [emoji] para revelar carta.  
   - ACERTO ou ESCONDER conforme o caso.  

**Finalização:**  
1. Quando todas as cartas são descobertas, servidor envia FIM.  
2. Clientes exibem mensagem final e encerram a conexão.  

## Tratamento de Erros  
- **Timeout:** 30 segundos para respostas.  
- **Mensagens inválidas:** ignoradas pelo destinatário.  
- **Conexões perdidas:** a partida é interrompida.  

## Exemplo de Sessão  
[Cliente1 → Servidor] CONNECT  
[Servidor → Cliente1] ID 1  
[Cliente2 → Servidor] CONNECT  
[Servidor → Cliente2] ID 2  
[Servidor → Todos] VEZ 1  
[Cliente1 → Servidor] CLIQUE 5  
[Servidor → Todos] MOSTRAR 5 🍎  
[...]  
[Servidor → Todos] FIM 1 "Jogador 1 venceu!"


