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
<img width="774" height="110" alt="image" src="https://github.com/user-attachments/assets/563a4dda-7737-4f07-958a-016c37737a1c" />
<img width="775" height="117" alt="image" src="https://github.com/user-attachments/assets/f4381d17-dc63-4115-b9a5-eef3348ddc9f" />










