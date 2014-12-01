---
layout: "docpage"
title: Esfor�o de Tradu��o
redirect_from:
  - /Contributing/Translation
navgroup: Brasil
---

Os participantes do curso de [C#](/Brasil) est�o 'pagando' ajudando a traduzir este site.

*Mas quem quiser ajudar com a tradu��o � s� se integrar ao time.*

O processo, para quem deseja participar do esfor�o, � mais ou menos o seguinte:

1. Tudo acontece no [GitHub](https://github.com), ent�o o primeiro passo � voc� criar a sua conta l�. Como � s� para reposit�rios abertos � sem custo. 
2. Depois voc� tem criar que um fork seu do [reposit�rio deste site](https://github.com/MonoBrasil/website). � s� clicar no bot�o ```Fork``` no canto superior direito.
3. Existe uma [planilha de controle](https://docs.google.com/spreadsheets/d/1B_iFGvaDhm8jSC0STXHdNgQAX57DRaY8F3wd-7czSjA/edit#gid=1820428232), alocando os trabalhos de tradu��o e de revis�o. Os caminhos na planilha s�o os caminhos relativos dentro do reposit�rio e a maioria das p�ginas est�o numa [variante da linguagem MarkDown](https://help.github.com/articles/github-flavored-markdown/) com [front-matter](https://help.github.com/articles/using-jekyll-with-pages/#frontmatter-is-required) obrigat�ria, embora algumas poucas estejam em html.
4. Para p�ginas pequenas d� para editar diretamente no GitHub, navegando at� o arquivo dentro do seu reposit�rio, e clicando no link de edi��o que � um pequeno �cone de uma canetinha.
5. **Cuidado** para n�o alterar incorretamente o cabe�alho (front-matter) exigido pelo processador de gera��o do site (Jekyll). Normalmente o que est� no topo entre duas linhas marcadas com --- deve ser preservado, apenas o texto que est� ap�s o prefixo title: deve ser traduzido.
6. Cada vez que voc� salvar as altera��es estar� fazendo um _commit_ no seu fork.
7. Para trabalhos maiores, � interessante fazer um clone do seu reposit�rio em um ambiente que suporte git (Eclipse, Visual Studio Community, Xamarin Studio, em linha de comando no Linux, GitHub for Windows) para poder fazer as altera��es em um editor de texto como o Notepad++, Vim, etc... ou na pr�pria IDE, s� que a� tem que fazer em tr�s etapas: [staging](https://www.kernel.org/pub/software/scm/git/docs/git-add.html), [commit local](https://www.kernel.org/pub/software/scm/git/docs/git-commit.html) e [push](https://www.kernel.org/pub/software/scm/git/docs/git-push.html) para o seu fork.
8. Quando quiser enviar o que voc� realizou at� o momento, voc� deve fazer um ```Pull Request``` (use o link ```Compare``` no topo da p�gina raiz do seu reposit�rio e depois o bot�o ```Create Pull Request```).
9. Envie um email do seu PR para o revisor correspondente na planilha, com c�pia para a lista do [MonoBrasil](monobrasil@googlegroups.com) que voc� dever� ter previamente se [inscrito](https://groups.google.com/forum/#!forum/monobrasil). 
10. O revisor dever� colocar coment�rios para a melhoria da tradu��o no pr�prio PR (como coment�rio de linha, de prefer�ncia) e ao final marc�-lo com um ```:shipit:```, que n�s commiters vamos olhar e comitar a contribui��o da dupla. 
11. Para fazer as corre��es pedidas pelo revisor � melhor usar uma c�pia local do seu reposit�rio para fazer um ```git commit -amend``` (que reincorpora as altera��es diretamente no PR j� aberto). A� atualizaremos o percentual do seu trabalho na planilha e � s� partir para o abra�o.
11. Caso o revisor n�o d� sinal de vida em um prazo de 2 ou 3 dias, voc� manda um email aqui na lista para olharmos o seu PR e a coisa n�o estancar.
