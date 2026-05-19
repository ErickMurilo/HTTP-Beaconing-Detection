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




