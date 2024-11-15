---
layout: post
title: Fraude Segura
type: post
parent_id: "2"
published: true
password: ""
status: publish
categories:
  - Hacking
tags:
  - FBI
  - https
  - security
  - ssl
  - phishing
permalink: /hacking/2019-06-16-fraude-segura.html
excerpt: "Por tempos ouvimos de “especialistas” em segurança a famosa frase: sempre antes de fornecer seus dados verifique se o site tem https e o cadeadinho verde."
description: "Por tempos ouvimos de “especialistas” em segurança a famosa frase: sempre antes de fornecer seus dados verifique se o site tem https e o cadeadinho verde."
image:
  path: /assets/img/posts/fraude-segura/genius.jpg
preview: /assets/img/posts/fraude-segura/genius.jpg
---

### Cadeadinho verde

Atualmente, somente a presença do cadeado verde não é mais um indicador confiável de segurança em sites. A proliferação de phishings com certificados SSL torna necessário uma analise mais profunda.

Dentro do contexto fraudulento, onde a vítima é induzida a fornecer os dados pelo simples fato de existir o **cadeadinho verde**. Chamo isso de **Fraude Segura**.

Além disso, qualquer analista de infosec que, após 2020, ainda considere um site seguro apenas por ter https, demonstra **leiguice** ou **má-fé**. Acredito que a segurança online exige critérios mais rigorosos e atualizados.

> O ícone de cadeado carregou muito mais peso anos atrás, e para obter um certificado SSL/TLS foi um processo mais difícil, mas esses certificados agora são gratuitos e podem ser adquiridos por qualquer pessoa. Os invasores estão cada vez mais se certificando de que seus sites de phishing tenham certificados autênticos para imitar sites legítimos.
- **Stu Sjouwerman (CEO knowbe4 )**
{: .prompt-info }

Devemos modificar nossa visão de como olhar um possível site malicioso, o fato de ter um https ou famoso selo (site blindado) não diz nada.

![](/assets/img/posts/fraude-segura/IwTWTsUzmIicM.webp)


Hoje em dia, o HTTPS se tornou uma ferramenta cada vez mais utilizada por fraudadores para criar uma falsa sensação de segurança em suas técnicas de engenharia social. De acordo com métricas recentes, cerca de 80% dos sites de phishing agora utilizam HTTPS, tornando essa característica obsoleta como indicador de confiabilidade.

O cadeadinho verde, que antes era considerado o grande protetor da internet, não é mais um garantidor de segurança. É essencial adotar um olhar mais rigorosas e atualizado para ser não ownador por aí.

As estatísticas de **Phishing + HTTPS** podem variar de acordo com a fonte e o período, mas aqui estão algumas referências:

- 80% dos sites de phishing utilizam HTTPS (Fonte: APWG - Anti-Phishing Working Group, 2022)
  - <https://docs.apwg.org/reports/apwg_trends_report_q2_2020.pdf>
- 74% dos sites de phishing utilizam HTTPS (Fonte: PhishLabs, 2020)
  - <https://info.phishlabs.com/blog/abuse-of-https-on-nearly-three-fourths-of-all-phishing-sites>


### FBI dando update em algumas recomendações

Seguindo essa ideia e se atualizando o The FBI’s Internet Crime Complaint Center (IC3)  publicou uma nota informando que invasores estão explorando a confiança das pessoas em sites que usam HTTPS.

![](/assets/img/posts/fraude-segura/fbi.gif)

**Cyber Actors Exploit ‘Secure’ Websites In Phishing Campaigns**

Websites with addresses that start with “https” are supposed to provide privacy and security to visitors. After all, the “s” stands for “secure” in HTTPS: Hypertext Transfer Protocol Secure. In fact, cybersecurity training has focused on encouraging people to look for the lock icon that appears in the web browser address bar on these secure sites.

The presence of “https” and the lock icon are supposed to indicate the web traffic is encrypted and that visitors can share data safely. Unfortunately, cyber criminals are banking on the public’s trust of “https” and the lock icon. They are more frequently incorporating website certificates—third-party verification that a site is secure—when they send potential victims emails that imitate trustworthy companies or email contacts. These phishing schemes are used to acquire sensitive logins or other information by luring them to a malicious website that looks secure.


**Recommendations:**

The following steps can help reduce the likelihood of falling victim to HTTPS phishing:

- Do not simply trust the name on an email: question the intent of the email content. 
- If you receive a suspicious email with a link from a known contact, confirm the email is legitimate by calling or emailing the contact; do not reply directly to a suspicious email.
- Check for misspellings or wrong domains within a link (e.g., if an address that should end in “.gov” ends in “.com” instead).
- Do not trust a website just because it has a lock icon or “https” in the browser address bar.


**Victim Reporting**

The FBI encourages victims to report information concerning suspicious or criminal activity to their local FBI field office, and file a complaint with the IC3 at www.ic3.gov. If your complaint pertains to this particular scheme, please note “HTTPS phishing” in the body of the complaint.


### Resumindo

Não confie em um site apenas porque ele exibe um diabo de ícone com **cadeadinho verde** ou **https** na barra de endereços do navegador. Embora esses indicadores signifiquem que a conexão entre seu dispositivo e o servidor é criptografada, eles não garantem que o site seja legítimo ou seguro. **Em casos de fraude no maximo seus dados vão trafegar de forma segura no servidor mailicioso.**

### Referências

- <https://www.ic3.gov/PSA/2019/PSA190610>
- <https://blog.axur.com/pt/phishing-pode-se-esconder-atras-do-https>
- <https://olhardigital.com.br/fique_seguro/noticia/sites-falsos-estao-exibindo-o-cadeado-verde-para-enganar-usuarios/80195>
- <https://medium.com/security-thoughts/https-medium-com-ajazevedo-site-falso-nao-tem-o-cadeado-c8af3cafe1b1>
- <https://www.oficinadanet.com.br/seguranca/24343-atencao-metade-dos-sites-de-phishing-apresenta-cadeado-de-seguranca>
- <https://olhardigital.com.br/fique_seguro/noticia/o-que-e-para-que-serve-e-como-funciona-o-cadeado-verde-no-seu-navegador/65866>


## Post feito ao som de

{% include embed/youtube.html id='U9453CNeZ08' %}
