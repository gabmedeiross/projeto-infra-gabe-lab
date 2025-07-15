
# 🚀 Projeto de Infraestrutura de TI

## 1. 📝 Visão Geral do Projeto

> Este repositório documenta a criação de uma infraestrutura de rede corporativa completa, desenvolvida como um projeto de portfólio para demonstrar competências em planejamento, virtualização, administração de sistemas Windows/Linux e segurança de redes. O objetivo é aplicar conceitos teóricos em um cenário prático e realista, simulando um ambiente de TI funcional, segmentado e seguro.

---

## 2. 🗺️ Topologia e Planejamento

A arquitetura da rede foi primeiramente desenhada no **Cisco Packet Tracer** para definir a segmentação, o plano de endereçamento IP e o fluxo de comunicação entre os componentes.

![Topologia da Rede Gabe.lab](https://i.imgur.com/LyrX3nV.png)


---

## 3. ⚙️ Configuração da Infraestrutura

A infraestrutura foi segmentada em duas redes principais para garantir segurança e organização: **Rede de Servidores (`10.1.0.0/24`)** e **Rede de Usuários (`192.168.1.0/24`)**. O roteamento e o firewall entre elas são gerenciados pelo pfSense.

### 🐧 Roteador `pfSense-FW`
| Atributo | Configuração |
| :--- | :--- |
| **VM** | Geração 1, 1GB RAM, 20GB HDD |
| **Interface WAN** | Conectada ao `Switch-Internet` (DHCP) |
| **Interface LAN** | IP Estático `192.168.1.1/24` (Conectada à `Switch-Usuarios`) |
| **Interface OPT1**| IP Estático `10.1.0.1/24` (Conectada à `Switch-Servidores`) |
| **Serviços** | Servidor DHCP (para LAN), Firewall com regras de permissão entre redes e para a WAN. |

### ↔️ Controlador de Domínio `DC-01`
| Atributo | Configuração |
| :--- | :--- |
| **VM** | Geração 2, 2GB RAM, 60GB HDD (Windows Server 2022) |
| **IP Estático** | `10.1.0.10/24` |
| **Gateway** | `10.1.0.1` (Apontando para o pfSense) |
| **DNS Primário** | `127.0.0.1` (Apontando para si mesmo) |
| **Funções** | AD DS para o domínio **`gabe.lab`**, Servidor DNS autoritativo. |

### 🗂️ Servidor de Arquivos `FS-01`
| Atributo | Configuração |
| :--- | :--- |
| **VM** | Geração 2, 1GB RAM, 50GB HDD (Windows Server 2022) |
| **IP Estático** | `10.1.0.20/24` |
| **Gateway** | `10.1.0.1` |
| **DNS Primário** | `10.1.0.10` (Apontando para o DC) |
| **Status** | Ingressado no domínio `gabe.lab`, servindo compartilhamentos com permissões NTFS. |

### 🌐 Servidor Web `WEB-01`
| Atributo | Configuração |
| :--- | :--- |
| **VM** | Geração 1, 2GB RAM, 20GB HDD (Ubuntu 24.04 LTS Desktop) |
| **IP Estático** | `10.1.0.30/24` |
| **Gateway** | `10.1.0.1` |
| **DNS Primário** | `10.1.0.10` |
| **Serviços** | Servidor web **Apache2** hospedando a página de portfólio. |

---

## 4. ✅ Validação Final e Resultados

A fase final consistiu na implantação da estação de trabalho `CLIENTE-01` (Windows 11) e na validação de todos os serviços a partir da perspectiva de um usuário do domínio (`ana.silva@gabe.lab`). **Todos os testes foram concluídos com sucesso:**

* **DHCP e Rede:** O cliente recebeu um IP (`192.168.1.100`) e as configurações de rede do pfSense.
* **Autenticação:** O cliente ingressou no domínio `gabe.lab` e o logon com usuário do AD foi validado.
* **Acesso à Internet:** O cliente conseguiu navegar em sites externos, confirmando o funcionamento do NAT e das regras de firewall.
* **Resolução DNS Interna:** O acesso ao servidor web pelo nome `http://portifolio.gabe.lab` foi bem-sucedido.
* **Acesso a Arquivos:** O acesso ao compartilhamento `\\FS-01\RH` funcionou, validando as permissões e o roteamento.

---

## 🛠️ Tecnologias e Ferramentas Utilizadas

<p align="left">
  <img src="https://img.shields.io/badge/Hyper--V-0078D6?style=for-the-badge&logo=hyper&logoColor=white" alt="Hyper-V"/>
  <img src="https://img.shields.io/badge/pfSense-C93F2B?style=for-the-badge&logo=pfsense&logoColor=white" alt="pfSense"/>
  <img src="https://img.shields.io/badge/Windows%20Server-0078D6?style=for-the-badge&logo=windows-server&logoColor=white" alt="Windows Server"/>
  <img src="https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white" alt="Ubuntu"/>
  <img src="https://img.shields.io/badge/Active%20Directory-00569E?style=for-the-badge&logo=windows&logoColor=white" alt="Active Directory"/>
  <img src="https://img.shields.io/badge/DNS-F37421?style=for-the-badge&logo=cloudflare&logoColor=white" alt="DNS"/>
  <img src="https://img.shields.io/badge/DHCP-0078D6?style=for-the-badge&logo=microsoft&logoColor=white" alt="DHCP"/>
  <img src="https://img.shields.io/badge/Apache-D22128?style=for-the-badge&logo=apache&logoColor=white" alt="Apache"/>
  <img src="https://img.shields.io/badge/Packet%20Tracer-00569E?style=for-the-badge&logo=cisco&logoColor=white" alt="Packet Tracer"/>
</p>

---

### Conclusão do Projeto

O projeto foi finalizado com êxito, resultando em uma infraestrutura de TI multisserviço, segmentada e funcional, que simula de forma eficaz um ambiente corporativo robusto e seguro.
