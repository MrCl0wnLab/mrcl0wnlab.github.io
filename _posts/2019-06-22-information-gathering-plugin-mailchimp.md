---
layout: post
title: "Information Gathering: Plugin Mailchimp for WordPress"
type: post
parent_id: "3"
published: true
password: ""
status: publish
categories:
  - Hacking
  - PHP
  - Programação
tags:
  - CMS
  - Dork
  - Information Gathering
  - Mailchimp
  - Security
  - WordPress 
permalink: /hacking/information-gathering-plugin-mailchimp.html
excerpt: É possível coletar informações (E-mails) do log gerado pelo plugin Mailchimp for WordPress no CMS Wodpress.
description: É possível coletar informações (E-mails) do log gerado pelo plugin Mailchimp for WordPress no CMS Wodpress.
image:
  path: /assets/img/posts/information-gathering-plugin-mailchimp/banner-mailchimp.jpg
preview: /assets/img/posts/information-gathering-plugin-mailchimp/banner-mailchimp.jpg
---

### O plugin

Este problema não é uma falha intrínseca do plugin, mas sim resultado de uma configuração inadequada das pastas e arquivos. No entanto, pode ser classificado como um vazamento de informação ou *chame como bem entender, até mesmo de meu amorzinho se preferir*. A análise em questão foi realizada no plugin [MC4WP: Mailchimp for WordPress plugin](https://wordpress.org/plugins/mailchimp-for-wp/).


> O plugin "MC4WP: Mailchimp for WordPress" é uma extensão para WordPress que integra o serviço de marketing de e-mail Mailchimp com sites WordPress, permitindo a criação de formulários de inscrição, gerenciamento de subscrições e envio de notificações automáticas.
{: .prompt-tip }

### Issue - 281 

O comportamento errôneo de expor logs e e-mail foi reportado pelo user **slaFFik em Apr 3, 2016** via issue do github. O ticket descreve que o arquivo `/wp-content/uploads/mc4wp-debug.log`{: .filepath} estava expondo e-mails de usuários. 

1. Add migration to rename log file & insert PHP exit header. #281
  - <https://github.com/ibericode/mailchimp-for-wordpress/commit/df7c4929b928406583e2c2c03e2156d2257121b5>

2. Obfuscate emails which are logged with warning level. Relates to #281
  - <https://github.com/ibericode/mailchimp-for-wordpress/commit/12bd049d684ad51dd72a5d7d9bf1b505ca98765c>

![](/assets/img/posts/information-gathering-plugin-mailchimp/Screenshot_2019-06-21-libericode%20mailchimp-for-wordpress.png)


Exemplo de conteúdo do arquivo `/wp-content/uploads/mc4wp-debug.log`{: .filepath}


```bash
[2016-05-06 18:00:18] ERROR: Registration Form > MailChimp API Error: Error connecting to MailChimp. Operation timed out after 10005 milliseconds with 0 bytes received
[2016-06-03 15:50:01] ERROR: Registration Form > MailChimp API Error: Error connecting to MailChimp. Operation timed out after 10004 milliseconds with 0 bytes received
[2016-06-16 22:25:29] ERROR: Registration Form > MailChimp API Error: Error connecting to MailChimp. Operation timed out after 10001 milliseconds with 0 bytes received
[2017-05-27 13:30:20] ERROR: Registration Form > MailChimp API Error: List_RoleEmailMember: xxxxxxx@yopmail.com is an invalid email address and cannot be imported.
[2017-07-07 14:17:55] ERROR: Registration Form > MailChimp API Error: The MailChimp API server returned the following response: <em>403 Forbidden</em>.
[2017-11-18 16:32:09] ERROR: Registration Form > MailChimp API Error: Error connecting to MailChimp. cURL error 28: Operation timed out after 10000 milliseconds with 0 bytes received
[2017-11-30 09:08:17] ERROR: Registration Form > MailChimp API Error: Error connecting to MailChimp. cURL error 28: Operation timed out after 10000 milliseconds with 0 bytes received
[2018-03-18 14:16:14] ERROR: Registration Form > MailChimp API Error: List_RoleEmailMember: xxxxxxx@gmail.om is an invalid email address and cannot be imported.
[2018-08-08 12:03:00] ERROR: Registration Form > MailChimp API Error: Recipient "xxxxxxx@imunan.info" has too many recent signup requests
[2018-08-09 14:17:50] ERROR: Registration Form > MailChimp API Error: Recipient "xxxxxxx@imunan.info" has too many recent signup requests
[2018-12-03 16:39:47] ERROR: Registration Form > MailChimp API Error: Error connecting to MailChimp. cURL error 28: Operation timed out after 10001 milliseconds with 0 bytes received
[2019-07-01 11:59:30] ERROR: Registration Form > MailChimp API Error: The MailChimp API server returned the following response: <em>403 Forbidden</em>.
[2019-07-22 17:33:45] ERROR: Registration Form > MailChimp API Error: List_RoleEmailMember: contact.xxxxxxx@free.fr is an invalid email address and cannot be imported.
[2019-07-27 21:37:04] ERROR: Registration Form > MailChimp API Error: Recipient "xxxxxxx@gmx.com" has too many recent signup requests
```


### Dork de busca
Usando como base um arquivo exemplo coletado, é possível criar nossas buscas ou até mesmo algum checker de URL. É possível achar alguns logs indexados via nosso gostosinho **Google Hacking**.

- `"MailChimp API error: Recipient" ext:log`
  - <https://www.google.com/search?client=firefox-b-d&q=%22MailChimp+API+error%3A+Recipient+%22+ext%3Alog>
- `"MailChimp API error: Recipient"`
  - <https://www.google.com/search?client=firefox-b-d&q=%22MailChimp+API+error%3A+Recipient+%22>


![](/assets/img/posts/information-gathering-plugin-mailchimp/Screenshot_2019-06-21%20MailChimp%20API%20error%20Recipient%20-%20Pesquisa%20Google.png)

Arquivo encontrado:

![](/assets/img/posts/information-gathering-plugin-mailchimp/Screenshot_2019-06-21%20Screenshot.png)

## Referências
- <https://wordpress.org/plugins/mailchimp-for-wp/>
- <http://hookr.io/functions/mc4wp_get_debug_log/>
- <https://github.com/ibericode/mailchimp-for-wordpress/issues/281>

## Post feito ao som de

{% include embed/youtube.html id='l9yC-uOMs1U' %}
