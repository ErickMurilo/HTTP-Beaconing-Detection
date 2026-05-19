#Objetivo 

Simular comportamento de HTTP Beaconing e analisar padrões de comunicação utilizando o Wireshark.

Ambiente 

- Máquina virtual Kali Linux
- Host Windons 
- Rede doméstica/laboratório
- Comunicação HTTP simulada para example.com
- Captura e análise realizadas no Wireshark

#Execução da simulação

- Loop com curl
- Requisição a cada 5 segundos
- Geração de tráfego
- Captura de pacotes

<img width="778" height="96" alt="image" src="https://github.com/user-attachments/assets/d7673bd0-3594-4dda-b5c1-4c553dfac56b" />

while true; do curl http://example.com; sleep 5; done

Simulação de comunicação entre um computador infectado e um invasor: é um comando simples que irá fazer o computador"chamar" um site automaticamente a cada 5 segundos.Isso simula o comportamento do Beaconing, informando que está esperando comando.




