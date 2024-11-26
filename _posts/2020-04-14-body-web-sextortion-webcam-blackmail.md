---
layout: post
title: Body Web Sextortion (webcam blackmail) / Anti-Grep
type: post
parent_id: "5"
published: true
password: ""
status: publish
categories:
  - Hacking
tags: [ Balckmail, Bitcoin, Hacking, Phishing, Sextortion, Extorsão ]
permalink: /hacking/body-web-sextortion-webcam-blackmail.html
excerpt: Sextortion is back!! Sextorsão (do termo em inglês sextorsion) é o termo que designa a prática de extorsão a partir da ameaça de exposição de supostas fotos ou vídeos sexuais das vítimas na Internet.
description: Sextortion is back!! Sextorsão (do termo em inglês sextorsion) é o termo que designa a prática de extorsão a partir da ameaça de exposição de supostas fotos ou vídeos sexuais das vítimas na Internet.
image:
  path: "/assets/img/posts/body-web-sextortion-webcam-blackmail/nigeriano.png"
preview: "/assets/img/posts/body-web-sextortion-webcam-blackmail/nigeriano.png"
---

### Processo inicial

Os criminosos ameaçam divulgar informações comprometedoras para amigos e parentes caso a vítima não cumpra o favor pedido dentro de um curto período de tempo. Algumas vezes, os golpistas não têm qualquer conteúdo comprometedor da vítima em mãos, mas utilizam mecanismos bastante convincentes para que seu target acredite no golpe.


### Técnica usada

A vítima recebe um email com seguinte padrão exemplo: **“Estou bem ciente de que XXXXXXXXX é a sua senha”**. Com a diferença, é claro, que no lugar dos **X** está a sua combinação verdadeira de alguma senha vazada do usuário. E complemento informando ter um vídeo íntimo seu e que você tem 24 horas para salvar a sua pele.
Sextortion tem seu sucesso por usar dados vazados de vitimas assim adquirindo um contexto maior de veracidade da ameaça contida no email.


![](/assets/img/posts/body-web-sextortion-webcam-blackmail/base-usada-exemplo.png)

Tal técnica nada mais é que um e-mail marketing usando bases vazadas (isso usando contexto citado acima).

### Anti-GREP

Até o pokémon mais bosta evolui.

![](/assets/img/posts/body-web-sextortion-webcam-blackmail/evolucao.png)

Substituindo letras por seus pares unicode e assim dificultado filtros [Grep](https://pt.wikipedia.org/wiki/Grep) que buscam pelo padrão [strings](https://pt.wikipedia.org/wiki/Cadeia_de_caracteres) já bem conhecido por equipes de [threathunt](https://computerworld.com.br/2018/11/23/threat-hunting-o-que-e-e-quais-os-beneficios-para-empresas/).


![](/assets/img/posts/body-web-sextortion-webcam-blackmail/email.png)


Uma característica é o usar de `*` no endereço da carteira de Bitcoin para tentar bypassar algum alerta.

**Regular expression bypass**

![](/assets/img/posts/body-web-sextortion-webcam-blackmail/regex-bypass.png)

**Regular expression**

![](/assets/img/posts/body-web-sextortion-webcam-blackmail/regex-detect.png)

**Regex**
```bash
b([13][a-km-zA-HJ-NP-Z1-9]{25,34}|bc1[|*|ac-hj-np-zAC-HJ-NP-Z02-9]{11,71})b
```

**Endereço de bitcoin**

```text
bc1qjma8cz8ry8lr5x3zc8w44cwthstxknr4lpzsdk
```

- <https://www.blockchain.com/pt/btc/address/bc1qjma8cz8ry8lr5x3zc8w44cwthstxknr4lpzsdk>

![](/assets/img/posts/body-web-sextortion-webcam-blackmail/btc.png)

Text Blackmail
E-mail fraudulento coletado
- <https://gist.github.com/MrCl0wnLab/910ea726ab1b9f2a202d7166b5b8f119>

### Referências
- <http://mokagio.github.io/tech-journal/2014/11/21/regex-bitcoin.html>
- <https://www.regextester.com/24>
- <https://www.elpescador.com.br/blog/index.php/deolhonogolpe-relatorio-sobre-sextortion/>

