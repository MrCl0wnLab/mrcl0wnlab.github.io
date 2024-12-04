---
layout: post
title: Class SenderMailgun in PHP
type: post
parent_id: "6"
published: true
password: ""
status: publish
categories: [ PHP, Programação ]
tags: [ Code, Mailgun, Marketing, API ]
permalink: /programacao/class-sendermailgun-in-php.html
excerpt: Class PHP criada para envio simples de email via API Mailgun. 
description: Class PHP criada para envio simples de email via API Mailgun. 
image:
  path: /assets/img/posts/class-sendermailgun-in-php/banner-mailgun.png
preview: /assets/img/posts/class-sendermailgun-in-php/banner-mailgun.png
---

### Mailgun

O Mailgun é um serviço de envio de e-mails em massa e automação de comunicação via e-mail, projetado para desenvolvedores e empresas que necessitam de uma solução confiável, escalável e segura para gerenciar suas comunicações por e-mail.

**Características Principais**

O Mailgun oferece uma ampla gama de recursos, incluindo envio de e-mails em massa, autenticação de e-mails avançada, análise detalhada de entrega, gerenciamento de listas de contatos e modelos de e-mail personalizados. Além disso, o serviço suporta integração com APIs, permitindo uma fácil incorporação em aplicativos e plataformas existentes.


### Class PHP para Envio de E-mails via API Mailgun

A classe PHP abaixo foi projetada para facilitar o envio de e-mails via API Mailgun. Para utilizá-la, é necessário cadastrar um domínio na plataforma Mailgun e configurar os registros DNS correspondentes.

**Pré-requisitos**

1. Cadastro no Mailgun <https://www.mailgun.com>.
2. Domínio registrado e configurado no Mailgun.
3. Valores de DNS records gerados pelo Mailgun (`MX`, `TXT`, `CNAME`, `SPF` e `DKIM`).
4. Configuração dos registros DNS no provedor de hospedagem ou zona DNS.

**Verificando seu domínio**

- <https://documentation.mailgun.com/en/latest/user_manual.html#verifying-your-domain>

**Acesse seus domínios**

- <https://app.mailgun.com/app/sending/domains>

**Acesse sua chave de API privada**

- <https://app.mailgun.com/app/account/security/api_keys>

### Implementação de código

- <https://gist.github.com/MrCl0wnLab/9e4cffb881a80ee06d3fd23c39a38ccb>

```php
<?php
require_once('SenderMailgun.php');

# Open file key
$key = file_get_contents('key');

# Instantiate the client.
SenderMailgun::$api_key = $key;

# Data params email.
$params = [
  'from'=> 'Your Name <you@maketing.your.ecommerce.br>',
  'to'=>'your-client@gmail.com',
  'subject'=>'Your subject',
  'html'=>'<h1>Black Friday!!!</h1>'
];

# Action sender.
SenderMailgun::send_mail('marketing.imaginarionerd.com.br',$params);
print_r(SenderMailgun::$result_send);
exit();

```


### Vídeo

[![asciicast](https://asciinema.org/a/nfHXNyMEv8HwVyWndQN21ST3S.png)](https://asciinema.org/a/nfHXNyMEv8HwVyWndQN21ST3S)

### Resultado

![](/assets/img/posts/class-sendermailgun-in-php/resultado-class-mailgun.png)

**E-mail**

![](/assets/img/posts/class-sendermailgun-in-php/resultado-class-mailgun-email.png)


### Download da Class
- <https://github.com/MrCl0wnLab/SenderMailgun>


### Post feito ao som de

<iframe style="border-radius:12px" src="https://open.spotify.com/embed/artist/3BwCvUfnT11B2jJtFZMtfH?utm_source=generator" width="100%" height="352" frameBorder="0" allowfullscreen="" allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture" loading="lazy"></iframe>
