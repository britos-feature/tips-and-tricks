# ***Nginx*** 

***Nginx*** (lê-se "engine-x"), é um **servidor web** de alto desempenho, que também pode funcionar como:

- **proxy reverso**
- **balanceador de carga**
- **cache HTTP**
- **servidor de e-mail (IMAP/POP3/SMTP) proxy**

Ele foi criado para lidar com milhares de conexões simultâneas com **baixo consumo de memória** e **alta eficiência**, sendo muito utilizado em sistemas de grande escala, como sites com muito tráfego.

### ***Casos de uso comuns***

- Servir sites estáticos (HTML, CSS, JS, imagens)  
- Servir como proxy reverso para servidores de aplicação (Node.js, Python, Java etc.)  
- Fazer balanceamento de carga entre múltiplos servidores  
- Melhorar performance com cache HTTP  
- Implementar SSL/TLS (HTTPS)


### ***Instalação***

```shell
sudo apt update
sudo apt install nginx
```


### ***Comandos básicos***

```shell
sudo systemctl start nginx        # Inicia o Nginx
sudo systemctl stop nginx         # Para o Nginx
sudo systemctl restart nginx      # Reinicia o Nginx
sudo systemctl enable nginx       # Ativa para iniciar com o sistema
sudo systemctl status nginx       # Verifica status
```


### ***Estrutura de configuração***

- Arquivo principal: `/etc/nginx/nginx.conf`
- Sites disponíveis: `/etc/nginx/sites-available/`
- Sites habilitados: `/etc/nginx/sites-enabled/`


### ***Exemplo de configuração simples (proxy reverso)***

Abaixo, uma configuração para o ***Nginx*** escutar na porta 80 e encaminhar o tráfego para uma aplicação rodando na porta 3000 (por exemplo, um app Node.js)


```shell
server {
    listen 80;
    server_name exemplo.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

#### ***Testar configuração***

<span style='color:red'><b>Antes de reiniciar o Nginx, sempre teste a configuração</b></span>

```shell
sudo nginx -t
```


### ***Reiniciar após mudanças***

```shell
sudo systemctl reload nginx
```



