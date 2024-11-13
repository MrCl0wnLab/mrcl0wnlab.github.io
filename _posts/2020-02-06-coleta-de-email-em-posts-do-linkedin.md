---
layout: post
title: "Information Gathering: Coleta de email em Posts do Linkedin"
type: post
parent_id: "4"
published: true
password: ""
status: publish
categories:
  - Hacking
  - PHP
  - Programação
tags:
  - Dork
  - Information Gathering
  - Linkedin
  - Email
permalink: /hacking/coleta-de-email-em-posts-do-linkedin.html
excerpt: As redes sociais são um buraco sem fim quando se trata de usuários expondo dados pessoais.
description: As redes sociais são um buraco sem fim quando se trata de usuários expondo dados pessoais.
image:
  path: /assets/img/posts/coleta-de-email-em-posts-do-linkedin/banner-linkedin.jpg
preview: /assets/img/posts/coleta-de-email-em-posts-do-linkedin/banner-linkedin.jpg
---

O comportamento de interação representam um desafio constante para a privacidade dos usuários, pois muitos expõem voluntariamente dados pessoais sensíveis, tornando-se vulneráveis dentro do contexto de engenharia social e tal característica  pode ser usado como uma fonte rica para ataques direcionados.

![](/assets/img/posts/coleta-de-email-em-posts-do-linkedin/Planilha%20para%20Controle%20de%20Gastos%20Pessoais1.png)

Criou-se um padrão de comportamento recorrente em posts do LinkedIn, onde um 'influenciador' compartilha apenas uma parte do conteúdo (X) e, para acessar o restante ou receber uma "planilha mágica" o tal conteúdo "ouro", os usuários são incentivados a comentar com seu e-mail. Esse comportamento pode sair caro, quando tratamos dados com bens e ativos constantemente procurados por fraudadores.

![](/assets/img/posts/coleta-de-email-em-posts-do-linkedin/troca.jpg)



### Técnica

Basicamente encontramos um padrão de URL nos posts do LinkedIn e com tal informação é possível criar dorks de busca e extrair os emails de forma massiva.

**Exemplo de URLS**
- <https://www.linkedin.com/pulse/planilha-de-controle-ordem-produção-marcos-rieper/>
- <https://www.linkedin.com/pulse/planilha-para-avaliação-de-desempenho-e-competências-plano-garcia/>
- <https://www.linkedin.com/pulse/planilha-teste-para-estagiárioxlsdownload-gratuito-arthur/>

Com padrão de string Identificando `www.linkedin.com/pulse/`, é possível criar a dorks para filtro dos targets.

Dork criada

```text
site:linkedin.com "linkedin.com/pulse/" "Planilha"
```

Resultado

![](/assets/img/posts/coleta-de-email-em-posts-do-linkedin/Screenshot_2020-02-05%20site%20linkedin%20com%20linkedin%20com%20pulse%20Planilha%20-%20Pesquisa%20Google.png)

Extração de target

![](/assets/img/posts/coleta-de-email-em-posts-do-linkedin/Screenshot_2020-02-05%20(21)%20Planilha%20para%20Controle%20de%20Gastos%20Pessoais%20(Orçamento)%20LinkedIn(2).png)

A extração é bem simples, usei mais do mesmo: RR ( Regex + Request ), o único diferencial é pegar um ID criado no Body do post que possibilita pegar todos e-mails do post sem necessidade de paginação.

### Review script de coleta

Request pegando ID

```php
exec("curl -kg --user-agent '{$user_agent}' '{$url_target}'>tmp");
```

GREP permalink and ID_URN
```php
$data_article_permalink =  'cat tmp | grep -oP '(?<=data-article-permalink=").*?(?=">)'';")'
$data_article_urn =  'cat tmp | grep -oP '(?<=data-article-urn="urn:li:article:).*?(?=")'';
```

URL request
```php
$url_request_dump_email = "https://www.linkedin.com/content-guest/article/comments/urn%3Ali%3Aarticle%3A{$return[0]}?count=100&start=0&articlePermalink={$return[1]}";
```

Dump files
```php
$file_dump_html = "dump_html_tmp_{$return[0]}";
$file_dump_email = "emails_{$return[0]}";
```

Request URL dump de comentários
```php
exec("curl -kg --user-agent '{$user_agent}' '{$url_request_dump_email}'>'{$file_dump_html}'");
```

GREP de e-mails
```php
exec('cat "'.$file_dump_html.'" | grep -E -o 'b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+.[A-Za-z]{2,6}b' | sort | uniq >'.$file_dump_email);
```

Código completo
```php
<?php
$targets = array_unique(explode("n",file_get_contents("posts.targets")));
$user_agent = 'Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:47.0) Gecko/20100101 Firefox/47.0';

foreach ($targets as $key => $url_target) {
   
   #REQUEST PEGANDO ID
   exec("curl -kg --user-agent '{$user_agent}' '{$url_target}'>tmp");
   
   #GREP  ID
   $data_article_permalink =  'cat tmp | grep -oP '(?<=data-article-permalink=").*?(?=">)'';
   $data_article_urn =  'cat tmp | grep -oP '(?<=data-article-urn="urn:li:article:).*?(?=")'';
   $return[] = exec("{$data_article_urn}");
   $return[] = exec("{$data_article_permalink}");

   #SLEEP
   $sleep = random_int(1,5);
   sleep($sleep);

   #URL REQUEST
   $url_request_dump_email = "https://www.linkedin.com/content-guest/article/comments/urn%3Ali%3Aarticle%3A{$return[0]}?count=100&start=0&articlePermalink={$return[1]}";
   $file_dump_html = "dump_html_tmp_{$return[0]}";
   $file_dump_email = "emails_{$return[0]}";
   
   #REQUEST URL COMENTÁRIOS
   exec("curl -kg --user-agent '{$user_agent}' '{$url_request_dump_email}'>'{$file_dump_html}'");
   
   #GREP EMAILS
   exec('cat  "'.$file_dump_html.'" | grep -E -o 'b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+.[A-Za-z]{2,6}b' | sort | uniq >'.$file_dump_email);
   echo " DUMP HTML: {$file_dump_html} n EMAIL: {$file_dump_email}n URL: {$url_request_dump_email}nn";
   $log = "n-------------------------------------------n";
   $log.= "nDATA:".date('d-m-Y H:i:s');
   $log.= "nURL:".$url_request_dump_email;
   $log.= "n-------------------------------------------n";
   file_put_contents("{$file_dump_email}",$log,FILE_APPEND);

   #SLEEP
   sleep($sleep);
   unset($return);
}
```

Resultado do GREP

![](/assets/img/posts/coleta-de-email-em-posts-do-linkedin/Captura%20de%20tela%20de%202020-02-06%2000-47-19.png)


### Script completo
- <https://gist.github.com/MrCl0wnLab/eb26ff29d9b07e79b18bfface7f790d8>



## Post feito ao som de

{% include embed/youtube.html id='tpMC31WXbuk' %}
