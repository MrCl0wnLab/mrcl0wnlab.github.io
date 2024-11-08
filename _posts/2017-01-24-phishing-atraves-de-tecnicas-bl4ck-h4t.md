---
layout: post
title: Phishing através de técnicas BLACK HAT SEO
type: post
parent_id: "0"
published: true
password: ""
status: publish
categories:
  - Hacking
tags: [ AdWords, Analysis, Black Hat SEO, Dompdf, Keyword Stuffing, Link Farm, OCR, pdf, Phishing, Spear Phishing ]
permalink: /hacking/phishing-atraves-de-tecnicas-bl4ck-h4t.html
excerpt: Técnicas de phishing sofrem mutação a cada momento, seja por conta de um novo exploit ou pelo fato de determinada técnica não funcionar
description: Técnicas de phishing sofrem mutação a cada momento, seja por conta de um novo exploit ou pelo fato de determinada técnica não funcionar
image:
  path: "/assets/img/posts/phishing-atraves-de-tecnicas-black-hat-seo/black hat seo.png"
preview: "/assets/img/posts/phishing-atraves-de-tecnicas-black-hat-seo/black hat seo.png"
---

## O Novo Arsenal Phisher


> 
 Em 2016 foi o ano que fiz minha pesquisa sobre a utilização de motores de busca no cenário de phishing onde com base em artigos, testes e um pouco de malicia consegui realizar tal conceito. 
{: .prompt-tip }

>
 Minha pesquisa consiste em mostrar algumas tricks que atacantes usam para gerar trafego legitimo em suas paginas maliciosas e ludibriar seu target ao download.
{: .prompt-tip }


### O método:

![](/assets/img/posts/phishing-atraves-de-tecnicas-black-hat-seo/metodo_forjando_documentos_pdf.png)


Tal forma simples ainda garante muitos acessos quando se trata de **forjar** palavras chaves e concatenar o maior numero de técnicas **Black Hat SEO** em um arquivo / url, efetuado o upload do maior numero de arquivos PDF’s **“legítimos”** com palavras chaves direcionados para um grupo de interesse.

### Para entender o uso do PDF:

![](/assets/img/posts/phishing-atraves-de-tecnicas-black-hat-seo/Optical_Character_Recognition.png)

Em 2008, o Google anunciou em seu blog oficial que, a partir de agora, através da **Optical Character Recognition (OCR)**, Que permite converter imagens com texto em documentos de texto usando algoritmos de
computação automatizados. As imagens podem ser processadas individualmente (arquivos `.jpg, .png e .gif`) ou em documentos PDF com várias páginas (`.pdf`).

**A leitura e indexação de arquivos PDF não é novidade para ninguém**, a tal ponto que criminosos também começaram a usar os benefícios de **indexar documentos PDF para manipular resultados do Google**.
Os fraudadores usam documentos PDF fakes com palavras chaves , links e imagens para assim atingir o seu objetivo que seria destacar-se nos motores de busca. Em comparação com as páginas HTML comuns, **O Google parece confiar mais e punir bem menos páginas PDF**.

### O procurado click:

![](/assets/img/posts/phishing-atraves-de-tecnicas-black-hat-seo/Captura%20de%20ecrã%20de%202017-01-24%2002_53_07.png)

O que seria fins maliciosos ?
Seria direcionar o target para propagandas, exploração de vetores em seu navegador, ataques em seu roteador entre outras artimanhas.

**O intuito muitas vezes não é só ludibriar o target para efetuar um simples download, mas sim lucrar o máximo com seu click**, se o mesmo não executar o artefato assim garante uma forma de retorno financeiro.

### Técnica Keyword Stuffing:

![](/assets/img/posts/phishing-atraves-de-tecnicas-black-hat-seo/KeywordStuffing.png)

Tal forma de manipulação é bem antiga, mas ainda bem eficaz, mas hoje a sua evolução que pude perceber no decorrer da pesquisa é o seu uso como se fosse um **Spear Phishing**, sim isso mesmo um Spear Phishing ele é feito de forma mais aberto porem restringindo suas palavras chaves, padrão de url’s e até mesmo imagens a um determinado grupo.

O diferencial dessa técnica é que o pescador é passivo o mesmo não executa uma ação mais intrusiva, ele espera o acesso do seu target assim garantindo mais **sucesso de infecções**.

### Gerando arquivos em massa:

![](/assets/img/posts/phishing-atraves-de-tecnicas-black-hat-seo/toolExec.png)

Quando se trata de ações na web um atacante não quer perder tempo. Armado de seu arsenal sempre atualizado para otimizar seus processos. Por esse fato criei uma pequena tool que vai criar e injetar um conjunto de palavras chaves em nosso PDF’s, para termos noção de uma estrutura final do arquivo de isca.

### Algumas palavras chaves que serão usadas no processo:

```text
Compre_seu,Carro_novo,usado,condicoes,gerais,semi_novo,Comprar,condições_gerais,
porto_seguro,automóvel,Carro,Nossos,carros,Pick,up, SUV,Gol,VW,Up,Golf,Novo,Fox,
computador,bordo,rodas,liga,leve,FRANQUIA,airbag,passageiro,freios,ABS,retrovisores,
elétricos,Câmbio,automatico,automatizado,manual,Ofertas,Preços,Simular,financiamento,
Fotos,Ficha,técnica,Tiguan,EXTRA…
```

### Execução da ferramenta:

![](/assets/img/posts/phishing-atraves-de-tecnicas-black-hat-seo/toolExec2.png)

A lógica da ferramenta funciona da seguinte forma.
Sua base é um arquivo preenchido com diversas palavras chaves direcionadas para um determinado grupo. geramos um PDF para cada palavra chave e cada palavra chave é vinculada ao nome do próprio arquivo e a ferramenta já cria links internos referenciando outros arquivos gerados, essa técnica pode ser usada como LINK FARM.

### PDF forjado:

![](/assets/img/posts/phishing-atraves-de-tecnicas-black-hat-seo/PDFforjado.png)

Usando elementos básicos de SEO é forjado o PDF isca. É este arquivo que o motor de busca vai indexar.


### Garantindo o acesso:

Para "hackear" de forma expressiva e também garante o acesso das vítimas a suas (`URL’S \|\| arquivos maliciosos`), O criminoso pode refinar o arquivo `.htaccess` de forma que o Bot de indexação **(Web crawler)** tenha acesso ao seu conteúdo, mas não o usuário alvo pois o mesmo é direcionado para site de terceiros.

### Arquivo .htaccess modificado:

![](/assets/img/posts/phishing-atraves-de-tecnicas-black-hat-seo/htaccess.png)

Lógica do atacante: isso é usado para que o motor de busca acesse o PDF forjado, porem o target só vê o cache do mesmo nos resultados assim garantindo uma engenharia social gerada pelo próprio motor de busca.

### Busca infectada:

![](/assets/img/posts/phishing-atraves-de-tecnicas-black-hat-seo/R3SULTADO_ORGANICO.png)

![](/assets/img/posts/phishing-atraves-de-tecnicas-black-hat-seo/R3SULTADO_PATROCINADO.png)

É uma exploração de relação de confiança entre usuário e seu aplicativo web favorita, pois ele não vai oferecer o conteúdo “carros usados”, para alguém que de suma maioria curtir páginas(facebook) ou pesquisa keywords para cinema ou faz pesquisa de Corte e Costura ou seja o target ativa o gatilho.

### O uso do Google AdWords:

![](/assets/img/posts/phishing-atraves-de-tecnicas-black-hat-seo/GoogleAdwords.png)


A grande trick referente conjunto de palavras chaves e potencializar o acesso é o anuncio pago que tal técnica não é só usada em motores de busca, mas também em redes sociais como Facebook.

O patrocínio /anuncio pago de palavras chaves vinculadas aos seus arquivos forjados potencializa em mais de 50% as chaves de vitimas acessarem tal url infectada.

Com investimento de `R$50,00` por dia filtrando alvos no estado de São Paulo o atacante pode ter alcance potencial de `1K cliques` + `57k` de impressões.
Achei o valor de `1k` inflado pelo Google, mas suponhamos que tal valor real é de `500` cliques por `R$50,00` analisando cenário comparativo com spammer comum o `Black Hat SEO` tem grande potencial spear pelo filtro avançado que própria ferramenta de propaganda oferece e pela assertividade de direcionar para pessoa certa.

Em minha humilde opinião redes sociais e motores de busca serão o futuro dos spammers, tanto pela assertividade filtro avançado de perfil quanto pela facilidade de atingir milhares de pessoas facilmente.

Um exemplo recente de Link patrocinado malicioso é o  `Fake BSOD warning`, onde o usuário ao clicar no link erá direcionado para uma página que simulava a tela azul de erro `Windows` (famosa tela da morte).
O Fake warning exibia uma mensagem orientando suas vitimas a discar o numero do “suporte“ técnico, para obter a devida “ajuda“.

Tal anúncio patrocinado aparecia e destaque quando a palavra chave “youtube“ erá buscada no Google. Contatado pela [Malwarebytes](https://blog.malwarebytes.com/threat-analysis/2015/09/malvertising-via-google-adwords-leads-to-fake-bsod/), o Google AdWords removeu imediatamente os anúncios fraudulentos.

A tela fake usada no modelo BSOD de ataque é uma variação das telas `“Scan Virus Alert”` que consiste simular um escaneamento ou um simples alerta de vírus, posteriormente oferecer uma “Ferramenta” ou telefone  para auxiliar na remoção de tal vírus, apesar de não muito usada no brasil essa técnica é bem antiga.

Sua nova variação e mais avançada é voltada para publico mobile, algumas versões web pedem dados do cartão de credito e utilizam functions que podem fazer seu celular até mesmo vibrar.

### Scan alert sempre em evolução:

![](/assets/img/posts/phishing-atraves-de-tecnicas-black-hat-seo/Captura%20de%20ecrã%20de%202017-01-25%2001_19_14.png)

### O fluxo final seria dessa forma:

![](/assets/img/posts/phishing-atraves-de-tecnicas-black-hat-seo/fluxo.png)

O usuário toma ação de efetuar uma pesquisa, em seu resultado de busca aparece tais urls infectadas seja no orgânico ou patrocinado, depois do clique o target pode entrar em vários cenários seja de baixar um PDF infectado ou PE até mesmo ter vários vetores explorados em seu navegador ou roteador. Bom esse é um cenário simplista, mas é possivel ter noção do potencial dessa tipo de técnica. 

Posso afirmar que hoje meios pagos de publicidade como `Google AdWords`, se tornaram um dos maiores vetores que proporcionam ataques. Fraudes envolvendo em publicidade paga é outro level quando falamos de spear phishing tanto quando se trata de facilidade e retorno lucrativo.
Qualquer pessoa pode criar uma conta no `Google AdWords` ou Facebook sem nem um tipo de verificação se X é dono da marca Y. muitas vezes o atacante usa técnicas de [typosquatting](https://www.ambito-juridico.com.br/site/index.php?n_link=revista_artigos_leitura&artigo_id=3730) para ludibriar seu target.

Não só o target user é afetado por tal técnica, mas também a empresa que teve sua marca vinculada ao ataque.


> 
Ferramenta usada no post: `forjaPDF` (para estudos)
- <https://github.com/MrCl0wnLab/forjaPDF>
{: .prompt-info }

>
A ferramenta usada no artigo utiliza as class do projeto `Dompdf`
- <https://dompdf.github.com/>
{: .prompt-info }

### Referências:


- <https://docs.google.com/presentation/d/1VHibceRJBmLOw-szXc_ky_vpr3VFubGkaRlVaTTpzBU>
- <https://blog.malwarebytes.com/threat-analysis/2015/09/malvertising-via-google-adwords-leads-to-fake-bsod/>
- <https://noticias.terra.com.br/dino/fraudes-com-google-adwords-continuam-em-alta,2440988f8c4ccf9421e7115261e8745b9c5sojr6.html>
- <https://dompdf.github.com>
- <https://www.elpescador.com.br/blog/index.php/phishing-engenharia-social-entenda-porque-essas-tecnica>
- <https://support.google.com/drive/answer/176692?hl=pt-BR>
- <https://scholar.google.com.br/intl/pt-BR/scholar/publishers.html#tech2>
- <https://www.elpescador.com.br/blog/index.php/quatro-fatos-que-explicam-porque-o-phishing-e-a-maior-arma-do-cibercrime>
- <https://support.google.com/webmasters/answer/6001181?hl=pt-br>
- <https://httpd.apache.org/docs/2.2/pt-br/howto/htaccess.html>
- <https://www.rapid7.com/db/modules/exploit/windows/fileformat/adobe_pdf_embedded_exe>
- <https://www.offensive-security.com/metasploit-unleashed/client-side-exploits>
- <https://www.offensive-security.com/metasploit-unleashed/msfconsole>
- <https://www.elpescador.com.br/blog/index.php/games-online-um-campo-minado-de-phishing>
- <https://www.facebook.com/business/products/ads>
- <https://support.google.com/webmasters/answer/1061943?hl=pt-BR>
- <https://blog.malwarebytes.org/mobile-2/2013/12/android-pop-ups-warn-of-infection>
- <https://www.microsoft.com/en-us/security/pc-security/antivirus-rogue.aspx>
- <https://www.elpescador.com.br/blog/index.php/phishing-engenharia-social-entenda-porque-essas-tecnicas-estao-interligadas>
- <https://g1.globo.com/tecnologia/blog/seguranca-digital/post/golpe-com-falsa-tela-azul-da-morte-e-veiculado-em-anuncios-na-web.html>
- <https://www.agenciamestre.com/seo/link-farm>
- <https://help.adobe.com/livedocs/acrobat_sdk/10/Acrobat10_HTMLHelp/wwhelp/wwhimpl/common/html/wwhelp.htm?context=Acrobat10_SDK_HTMLHelp&file=JS_Dev_Overview.71.1.html>
- <https://partners.adobe.com/public/developer/en/acrobat/sdk/AcroJSGuide.pdf>
- <https://dl.packetstormsecurity.net/1411-exploits/googledoubleclick-redirect.txt>
- <https://support.google.com/analytics/answer/1033981>
