
# Laboratório de Redes: diagnóstico básico de conectividade no Windows

Prática de diagnóstico de rede com comandos do Prompt de Comando do Windows (`ipconfig`, `ping`, `nslookup`), realizada em 19/10/2026 na minha rede local (Wi-Fi). Objetivo: entender como localizar onde está um problema de "sem internet" testando cada etapa da conexão.

## Ambiente
- Windows 10/11 (Prompt de Comando)
- Conexão Wi-Fi doméstica
- IP privado do computador: `192.168.2.103` | Máscara: `255.255.255.0` | Gateway: `192.168.2.1`

## Método: testar do mais perto para o mais longe
| Ordem | Comando | O que testa |
|---|---|---|
| 1 | `ipconfig` | IP, máscara e gateway recebidos |
| 2 | `ping <gateway>` | Acesso ao roteador (rede local) |
| 3 | `ping 8.8.8.8` | Acesso à internet |
| 4 | `ping google.com` | Resolução de nomes (DNS) |
| 5 | `nslookup google.com` | Servidor DNS e IPs retornados |

## Cenário 1: conexão normal
- `ping 192.168.2.1`: 4 enviados, 4 recebidos, 0% de perda, tempo de 0 a 4 ms.
- `ping 8.8.8.8`: 0% de perda, média de 56 ms.
- `ping google.com`: 0% de perda, média de 51 ms.
- `nslookup google.com`: DNS `192.168.2.1`, retornou endereços IPv4 e IPv6.

**Conclusão:** rede local, internet e DNS funcionando.

## Cenário 2: Wi-Fi desligado (falha simulada)
- `ping 192.168.2.1`: "Falha geral", 100% de perda.
- `ping 8.8.8.8`: "Host de destino inacessível", com resposta vinda do próprio computador (`192.168.2.103`). Nenhum pacote chegou ao destino, apesar de a contagem mostrar 1 recebido.

**Conclusão:** como o gateway não responde, a falha está na conexão local (Wi-Fi/adaptador), antes do roteador. Não é problema de internet nem de DNS.

## Cenário 3: renovação de IP
- `ipconfig /release`: o adaptador Wi-Fi perdeu o IPv4 e o gateway. Os adaptadores desconectados (Ethernet, Bluetooth) mostraram a mensagem de que a operação não pode ser executada, o que é esperado.
- `ipconfig /renew`: o Wi-Fi recebeu novamente `192.168.2.103` com gateway `192.168.2.1`.

**Conclusão:** o servidor DHCP do roteador devolveu a configuração de rede.

## O que aprendi
- Como separar um problema local, de internet e de DNS testando em ordem.
- A diferença entre IP privado e público.
- A ler a saída do `ping`, inclusive respostas que parecem sucesso e não são.

## Próximos passos
- Repetir o laboratório no Cisco Packet Tracer.
- Estudar `tracert` e `ipconfig /all`.
