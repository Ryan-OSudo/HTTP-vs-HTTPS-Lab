# 🛡️ Network Traffic Analysis: HTTP vs. HTTPS Security Lab

![Wireshark](https://img.shields.io/badge/Tool-Wireshark-blue?style=for-the-badge&logo=wireshark)
![Security](https://img.shields.io/badge/Focus-Network%20Security-red?style=for-the-badge)
![Protocol](https://img.shields.io/badge/Protocols-HTTP%20%7C%20TLS-green?style=for-the-badge)

## 🎯 Objetivos Técnicos
* **Packet Sniffing:** Captura de tráfego em tempo real utilizando Wireshark.
* **Protocol Analysis:** Inspeção profunda de pacotes nas camadas de Transporte e Aplicação.
* **Security Auditing:** Identificação de exposição de credenciais em redes inseguras.
* **Traffic Filtering:** Aplicação de filtros avançados para isolamento de eventos críticos.

---

## 🔬 Cenário 1: Exposição de Dados em HTTP (Inseguro)
Neste cenário, analisei o tráfego de um formulário de login em uma aplicação rodando sobre o protocolo **HTTP (Porta 80)**.

### Metodologia de Investigação
1.  **Captura:** A interface de rede foi monitorada durante o envio do formulário.
2.  **Filtragem:** Utilizei o filtro `http.request.method == "POST"` para encontrar o pacote de envio de dados.
3.  **Reconstrução:** Através do recurso *Follow TCP Stream*, a conversa completa foi reconstruída.

### Resultado da Análise
> [!CAUTION]
> **Vulnerabilidade Identificada:** O protocolo HTTP não criptografa os dados. Como resultado, o nome de usuário e a senha foram capturados em **texto claro (plaintext)** diretamente do payload do pacote.

**<img width="1914" height="92" alt="image" src="https://github.com/user-attachments/assets/73626f1e-467b-4fa0-80df-ad23ef26b903" />**
*Figura 1: Usando o filtro para encontrar o pacote.*

**<img width="1255" height="830" alt="image" src="https://github.com/user-attachments/assets/cde7b39d-f807-48b6-84fd-dc35b3af4438" />**
*Figura 2: Credenciais expostas em cima da resposta do PKT do servidor.*

---

## 🔐 Cenário 2: Proteção via TLS/HTTPS (Seguro)
Para fins comparativos, realizei a mesma análise em uma conexão protegida por **HTTPS (Porta 443)**.

### Observações Técnicas
* **Handshake TLS:** Foi possível observar o processo de troca de certificados e negociação de cifras (Cipher Suites).
* **Criptografia de Dados:** Ao contrário do HTTP, o conteúdo da aplicação (*Application Data*) tornou-se ilegível sem a chave privada.
* **Conclusão:** A integridade e confidencialidade dos dados foram mantidas, impedindo ataques de interceptação (*Sniffing*).

**<img width="1918" height="507" alt="image" src="https://github.com/user-attachments/assets/9a1021d8-0b5e-4967-8bd5-8dc8473367cd" />**
*Figura 3: Filtrando para achar o Handshake TLS*


**<img width="1252" height="463" alt="image" src="https://github.com/user-attachments/assets/b470dd63-8e9d-4fc6-ab2b-342456d0bf56" />**
*Figura 4: Payload (do Reddit) criptografado e ilegível para observadores externos.*

---

## 🛠️ Ferramentas & Filtros Utilizados

| Filtro | Finalidade |
| :--- | :--- |
| `http.request.method == "POST"` | Isolar envios de formulários de login. |
| `tcp.port == 80` | Monitorar tráfego HTTP padrão. |
| `tls.handshake.type == 1` | Identificar o início de conexões seguras (Client Hello). |
| `ip.addr == [SEU_IP]` | Filtrar tráfego de/para uma máquina específica. |

---

## 💡 Conclusões
Este projeto reforça a necessidade crítica da adoção do protocolo **HTTPS** em qualquer sistema que manipule dados sensíveis. A análise técnica demonstra que, em redes desprotegidas, a interceptação de dados é trivial caso protocolos legados sejam utilizados.

---

**Desenvolvido por Ryan G**
* Futuro Analista de Redes / Segurança em formação.
* LinkedIn: https://www.linkedin.com/in/ryangoncalves/
