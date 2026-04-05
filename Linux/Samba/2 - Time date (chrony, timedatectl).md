---
tags:
  - timedatectl
  - chrony
aliases:
  - timezone
  - timezones
  - timedate
---
# Modos de sincronizar date e hora no linux


- ## [[#^7d1dff|Comando timedatectl]]
	 O **`timedatectl`** é um comando usado para **gerenciar a data, hora e fuso horário do sistema**, integrado ao **systemd** _(default de muitas distro - "já vem instalado")_
	
	 unit -> **``systemd-timesyncd`**
	
- ## [[#^c6990f|chrony]]
	O **`chrony`** é uma solução moderna para **sincronização de data e hora no Linux**, baseada no protocolo **Network Time Protocol**. Ele foi criado para ser **mais preciso, rápido e confiável** do que implementações antigas de NTP.
	
	 unit -> **`chrony.service`**



> **Dica de mestre:** Se for um servidor de produção e você puder se dar ao luxo de uma pequena janela de manutenção, um **reboot** é a forma mais segura de garantir que _absolutamente todos_ os processos (inclusive os que você esqueceu) assumam a nova configuração.


---

# Comando `timedatectl` 

^7d1dff

Esse é um comando do **systemd** usado para **gerenciar data, hora e fuso horário no Linux**.
Ele substitui métodos antigos como `date` e `hwclock` em muitos sistemas modernos.

Ele controla o relógio do sistema através do:

 	 **`systemd-timesyncd`** (sincronização NTP)

---
### 1. Verificar data e hora

```bash
timedatectl
```

#### Exemplo de saída:

```bash
Local time: Tue 2026-03-24 09:00:00
Universal time: Tue 2026-03-24 12:00:00
RTC time: Tue 2026-03-24 12:00:00
Time zone: America/Sao_Paulo (BRT, -0300)
System clock synchronized: yes
NTP service: active
RTC in local TZ: no
```


**Entendendo cada linha**

- **Local time** → hora local do sistema
- **Universal time (UTC)** → padrão mundial
- **RTC time** → relógio da BIOS
- **Time zone** → fuso horário
- **NTP service** → sincronização automática
- **System clock synchronized** → se está sincronizado

---

### 2. Ver fusos horários disponíveis

```bash
timedatectl list-timezones
```

**Para filtrar:**

```bash
timedatectl list-timezones | grep Sao_Paulo
```

---

### 3. Definir fuso horário

```bash
sudo timedatectl set-timezone America/Sao_Paulo
```

> Muito importante para logs e serviços

---

### 4. Ajustar data e hora manualmente

```bash
sudo timedatectl set-time "2026-03-24 10:30:00"
```

---

### 5. Ativar sincronização automática (NTP)

```bash
sudo timedatectl set-ntp true
```

Desativar:

```bash
sudo timedatectl set-ntp false
```

---

### 6. Ver status detalhado do NTP

```bash
timedatectl status
```

---

#  Conceito importante

### NTP (Network Time Protocol)
É o protocolo que mantém o relógio do sistema correto automaticamente.

Sem isso:

- logs ficam errados
- autenticação (ex: Kerberos) pode falhar
- serviços distribuídos quebram

---
---

# 💥 Problemas comuns

### 1. Hora errada mesmo com NTP ativo

Verifique:

```bash
timedatectl status
```

E também:

```bash
systemctl status systemd-timesyncd
```

---

### 2. Fuso errado

Corrija com:

```bash
timedatectl set-timezone
```

---

# 3. Exemplo prático (laboratório)

```bash
timedatectl set-timezone America/Sao_Paulo
timedatectl set-ntp true
timedatectl
```

---

### Resumo:

| Função       | Comando          |
| ------------ | ---------------- |
| Ver hora     | `timedatectl`    |
| Listar fusos | `list-timezones` |
| Definir fuso | `set-timezone`   |
| Ajustar hora | `set-time`       |
| Ativar NTP   | `set-ntp true`   |

%% %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%% %%


# Chrony

^c6990f

O _**chrony**_ é uma solução moderna para **sincronização de data e hora no Linux**, baseada no protocolo **Network Time Protocol**. Ele foi criado para ser **mais preciso, rápido e confiável** do que implementações antigas de NTP.

Esse conjunto de ferramentas mantém o relógio do sistema sempre correto, mesmo em condições difíceis, como:

- rede instável
- máquina desligada por longos períodos
- ambientes virtualizados (VMs)

Ele é composto por dois principais componentes:

- **`chronyd`** → o serviço (daemon) que faz a sincronização
- **`chronyc`** → ferramenta de linha de comando para controle e diagnóstico

### Funcionamento:

O chrony:

1. Consulta servidores NTP (na internet ou internos)
2. Mede o atraso da rede (latência)
3. Calcula o desvio do relógio local
4. Ajusta o tempo **de forma gradual e inteligente**

> Isso evita “saltos bruscos” no tempo, que podem quebrar aplicações

###  Chrony é mais usado em:

- Servidores Linux em produção
- Ambientes corporativos
- Infraestrutura com:
    - autenticação (Kerberos)
    - diretórios (LDAP)
    - domínio (Samba AD)

> Nesses casos, tempo correto é **crítico**

---

### Instalação `chrony`

	**`apt install chrony`**

---

### 📁 Arquivo de configuração

- **`/etc/chrony/chrony.conf`**

**Exemplo básico:**

server 0.pool.ntp.org iburst  
server 1.pool.ntp.org iburst

> consulte https://ntp.br/guia/linux/ para uma configuração indicada (**chrony**)

---

- ### Comandos principais

	- **Ver status**
		`systemctl status chrony`
		`systemctl status chronyd` // dependo da distro

	- **Ver status geral**
		`chronyc tracking`

	- **Iniciar serviço**
		`systemctl start chrony`

	- **Habilitar no boot**
		`systemctl enable chrony`

	- **Reiniciar**
		`systemctl restart chrony`

	- **Ver logs**
		`journalctl -u chrony`

	- **Ver servidores utilizados**
		`chronyc sources`

	- **Visualizando nome da unit (profissionalmente)**
		`systemctl |grep "chrony"

---

- ### Forçar sincronização imediata

	**`chronyc makestep`**
	


