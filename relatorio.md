# Relatório — Atividade Prática: Sockets TCP

**Disciplina:** Sistemas Distribuídos
**Aluno:** João Pedro Darabas cardoso
**Data:** 24/11/2026 

---

## Nível 1 — Inspecionar o código

### 1.1 Servidor (`servidor.py`)

Descreva com suas palavras o que cada chamada abaixo faz e por que ela
é necessária:

| Chamada | O que ela faz? |
|---------|---------------|
| `socket.socket(AF_INET, SOCK_STREAM)` | Cria um socket, o primeiro argumento define a família de endereços IPv4 e o segundo o protocolo (TCP) é necessário para estabelecer a comunicação de rede |
| `servidor_socket.bind((HOST, PORTA))` | Associa o socket a uma interface de rede e a uma porta local para que o SO saiva para qual processo encaminhar os pacotes |
| `servidor_socket.listen(5)` | Coloca o socket em modo de escuta pra receber conexões entrantes com até 5 conexões na fila |
| `servidor_socket.accept()` | Bloqueia a execução até que um cliente inicie uma conexão, retorna conexão (socket) e endereço quando ela ocorre |
| `conn.recv(TAMANHO_BUFFER)` | recebe os dados do cliente de acordo com o tamanho do buffer |
| `conn.sendall(resposta.encode("utf-8"))` | converte a string pra bytes usando utf-8 e envia tudo pelo socket |
| `conn.close()` | encerra a conexão com FIN/ACK |

### 1.2 Cliente (`cliente.py`)

| Chamada | O que ela faz? |
|---------|---------------|
| `socket.socket(AF_INET, SOCK_STREAM)` | Cria o socket para IPv4 e TCP, serve como ponto local pra tanto envio quanto recebimento dos dados |
| `cliente_socket.connect((HOST_SERVIDOR, PORTA_SERVIDOR))` | Inicia o handshake de 3 vias com o servidor no endereço e porta específicos |
| `cliente_socket.sendall(mensagem.encode("utf-8"))` | Codifica a mensagem em bytes e manda todos os dados para o servidor |
| `cliente_socket.recv(TAMANHO_BUFFER)` | Espera pela resposta e lê os bytes devolvidos até o limite definido pelo buffer |

### 1.3 Rede e contêineres

Por que o cliente usa o hostname `"servidor"` em vez de `"localhost"`
para se conectar? O que aconteceria se usasse `"localhost"`?

> _Resposta:_
> Porque no docker compose cada contêiner roda num namespace de rede isolado, pro cliente o "localhost" apontaria para ele mesmo. O Docker cria uma rede interna com DNS embutido que resolve o nome "servidor" para o endereço atribuido ao contêiner. 
---

## Nível 2 — Modificar o servidor

Descreva as mudanças que você fez no `servidor.py` para:

1. Devolver a mensagem **em maiúsculas**:

   > _O que foi alterado e por quê:_
   > Ao decodificar a mensagem recebida, é aplicado `.upper()` antes de codificar novamente e enviar para garantir a transformação dos caracteres antes da retransmissão

2. Exibir no log o **contador de mensagens recebidas** acumulado:

   > _O que foi alterado e por quê:_
   > A cada mensagem lida o contador da variável global `total_mensagens` agora é incrementado e exibido junto do `log()`.

3. Após a modificação, você precisou rodar `docker compose up --build`.
Por que o `--build` é necessário aqui?

> _Resposta:_
> Porque quando executa apenas o `docker compose up` o docker reutiliza as imagens do cache, para usar os códigos atualizados é necessária a flag `--build`

---

## Observações livres

Anote aqui qualquer comportamento inesperado que observaram, erros que
apareceram e como resolveram, ou dúvidas que ainda têm:

>

---

## Dúvida para a próxima aula

Escreva **uma pergunta** que surgiu durante a atividade e que vocês
gostariam de ver respondida na aula seguinte:

>
