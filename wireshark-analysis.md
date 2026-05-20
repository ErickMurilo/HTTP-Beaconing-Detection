# Objetivo 

Simular comportamento de HTTP Beaconing e analisar padrões de comunicação utilizando o Wireshark.

Ambiente 

- Máquina virtual Kali Linux
- Host Windons 
- Rede doméstica/laboratório
- Comunicação HTTP simulada para example.com
- Captura e análise realizadas no Wireshark

# Execução da simulação

- Loop com curl
- Requisição a cada 5 segundos
- Geração de tráfego
- Captura de pacotes

<img width="778" height="96" alt="image" src="https://github.com/user-attachments/assets/d7673bd0-3594-4dda-b5c1-4c553dfac56b" />

while true; do curl http://example.com; sleep 5; done

Simulação de comunicação entre um computador infectado e um invasor: é um comando simples que irá fazer o computador"chamar" um site automaticamente a cada 5 segundos.Isso simula o comportamento do Beaconing, informando que está esperando comando.

# Análise e evidências

# Identificação do tráfego HTTP

<img width="768" height="168" alt="image" src="https://github.com/user-attachments/assets/b3c749d9-a229-4c9b-a856-83db06f6b717" />

Com a utilização do filtro "HTTP" é possível isolar comunicações, e vizualizar que as requisições GET acontecem em intervalos fixos (5s). Esse é um indício de que o tráfego é gerado por uma máquina.
Também é possível notas as requisições GET e a resposta do site (200 OK), acontecem em um intervalo de ocmunicação é sempre o mesmo.

# Isolamento de domínio
<img width="771" height="135" alt="Sem título" src="https://github.com/user-attachments/assets/db9805f6-f172-4e8b-90f0-d88bf83abde2" />

Utilizei o filtro host.contais para filtrar apenas o tráfego destinado a esse domínio. É possível ter uma melhor visuazlização do intervalo de tempo , pois o filtro isolou o alvo exato sem interferência de outros sites.

# Filtro utilizado para isolar requisições
<img width="779" height="125" alt="image" src="https://github.com/user-attachments/assets/c0fb8c54-5151-4375-b87b-28203cf7ed1b" />
Filtro utilizado para isolar apenas as requisições de entrada, eliminando as respostas doservidor e focando exclusivamente nas ações enviadas pela máquina local, facilicanto a análise.

# Análise do handshake TCP
<img width="779" height="116" alt="image" src="https://github.com/user-attachments/assets/0da3d648-4f04-4ff7-bf2d-d128aa1027ef" />

Utilizando o filtro de endereço de origem (IPV6.SRC - APPLY AS FILTER) é possível isolar toda atividade vindo da máquina de origem. Permite visualização não somente do pedido HTTP , mas também do TCP handshake, que acontece logo antes.
É possível ver o ciclo completo da conexão : o computador pede para se comunicar (SYN), o site aceita (ACK), os dados são enviados (GET) e a conexão é encerrada (FIN).

# Filtro para vizualizar solicitações
<img width="620" height="126" alt="image" src="https://github.com/user-attachments/assets/838b93f4-1070-4610-9437-38f2b79b08e1" />
<img width="775" height="117" alt="image" src="https://github.com/user-attachments/assets/f4381d17-dc63-4115-b9a5-eef3348ddc9f" />

Utilizando esse filtro isolei os pacote s de sincronização que dão início a conexão. Note que cada batimento do beacon o computador abre uma nova conversa técnica com o servidor , as mensagem em vermelho [TCP Port numbers reused] ocorrem porque o script faz conexões tão rápidas e frequentes que o sistema operacional acaba reutilizando portas que foram fechados recentemente , comportamento comum em automação de rede.

O wireshark marca em vermelho para avisar que a porta 4090 já foi usada antes, como meu script while true roda sem parar , ele esgota as portas novas e começa a repetir as antigas.

# Análise do User - Agent
<img width="484" height="172" alt="image" src="https://github.com/user-attachments/assets/d4c570e6-94f2-41d7-a59a-3f4a1761e409" />

Detalhes da camada de aplicação HTTP. Ao analisar o campo User-Agent , identificamos a assinatura curl/8.17.0. Revelando que a conexão está sendo feita por uma ferramenta de automação via linha de comando, e não por um navegador comum,o  que reforça a evidência de um script beaconing.

# Fluxo de comunicação
<img width="436" height="184" alt="image" src="https://github.com/user-attachments/assets/eebaa012-6137-4c45-b002-10f99d82aad1" />

Aqui vizualizo os endereços e ip envolvidos . O Source addres indentifica a máquina infectada e o Destination Addres aponta para o servidor externo.

<img width="543" height="371" alt="image" src="https://github.com/user-attachments/assets/2a169e20-ecbf-4528-b472-56a18c3ec757" />
A source Port (47624 - porta privada) é uma porta aleatório aberto pelo host enviar o pedido, enquanto a Destination Port (80) mostra o momento em que os dados do beaconing são enviados e recebidos, confirmando comunicação  ativa e bem sucedida entre o malware simulado e o servidor.

# Análise dados Hexadecimal
<img width="772" height="152" alt="image" src="https://github.com/user-attachments/assets/7ed1b2c0-6d37-40db-88c0-584ef9bf7030" />

Nessa sessão o wireshark traduz os dados brutos e traduz para texto legível, onde consigo confirmar que o destino era o site example.com o qual o meu script estava enviando.

Next sequence number é fundamental para o controle de fluxo. Ele indica a ordem dos dados e confirma que 75 bytes de informação foram enviados com sucesso, aguardando o próximo segmento da conversa.

As flags[PSH,ACK] indicam que os dados do beacon foram enviados imediatamente para aplicação, garantindo que o "sinal de vida" do malware chegue rápido ao destino.

# Hierarchy e Conversation
<img width="715" height="164" alt="image" src="https://github.com/user-attachments/assets/e9abc385-dac6-418f-9268-7c697dba0a91" />

A hierarquia mostra os protoclos que foram usados na conversa e a % de tráfego real, usamos para enterder a composição do tráfego capturado. Vizualizamos que o protocolo HTTP representa 12.2% de todos os pacotes.

<img width="768" height="130" alt="image" src="https://github.com/user-attachments/assets/7f21499e-9f7c-4929-bb3f-7a997cb06ea0" />

Em conversations identifiquei o fluxo de comunicação entre os dispositivos.As duas primeiras linhas mostram um volume desproporcional de pacotes enviados para endereços específicos em comparação ao restante da rede, permite identificar o endereço ip do dispositivo infectado e do servidor de destino.

A coluna Bytes mostra que o valor vaixo (15/16 kb) em mais de 100 pacotes, isso indica um comportamento de beaconing (check -in), onde o malware envia sinais frequentes , para manter a conexão ativo com o servidor, sem despertar suspeita por alto consumo de banda.

# IOCS (Indicators of Compromise)
- Requisições HTTP periódicas
- Intervalo fixo de 5 segundos
- User -Agent : curl/ 8.x
- Comunicação persistente com example.com
- Pequeno volume recorrente de dados
- Reutilização de portas Tcp
- Múltiplas conexões HTTP automatizada

# Conclusão
O ataque se caracteriza beaconing pois o beaconing é uma conexão periódica da máquina infectada com o servidor esperando um comando.

# Mitigações
- bloqueio de ip/domínio
- Automatizar alertas de siem








