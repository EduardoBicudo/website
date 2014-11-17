---
title: Abyss
redirect_from:
  - /FastCGI_Abyss/
---

Informa��o de como configurar o [FastCGI] (/docs/web/fastcgi/) com suporte ao Abys server.

Introdu��o
------------

[Abyss Web Server](http://www.aprelium.com/) � um servidor web rico em recursos e f�cil de usar. No entanto de fonte fechado , O servidor X1 � *"um software gr�tis e funcional : sem telas promocionais, sem limita��o de tempo, sem spyware, e sem propagandas."* ([Download](http://www.aprelium.com/abyssws/download.php))

 Ele realmente foca no "uso f�cil", Abyss foi o servidor mais f�cil de configurar. Ele foi preponderante em sua simplicidade e centro de controle integrado.

### Configurado e testado em:

-   [OpenSuSE 10.2](http://en.opensuse.org/OpenSUSE_News/10.2-Release) (Abyss X1)\</li\>
-   *se voc� tiver testado alguma configura��o adicional,por favor envie-me um email  [brian.nickel@gmail.com](mailto:brian.nickel@gmail.com).*

Alertas Gerais
----------------

Antes de fazer qualquer coisa, voc� deve ler a se��o [FastCGI#Important_Information](/docs/web/fastcgi/#important-information) no site principal do FastCGI.

### Geral Passo 1: Configurar o Interpretador

Ao iniciar o servidor web Abyss, um centro de controle do servidor web ser� iniciado por padr�o no endere�o
<http://localhost:9999>. Simplesmente abra seu navegador neste endere�o e siga os passos abaixo:

1.  Clique em "Configure" no host que voc� deseja adicionar suporte a ASP.NET
2.  Clique em "Scripting Parameters".
3.  Clique em "Add" na caixa "Interpreters".
4.  Voc� est� agora na p�gina para adicionar o interpretador ASP.NET. As duas op��es que eu poderia recomendar s�o "FastCGI (Local - Pipes)" e "FastCGI (Remote - TCP/IP sockets)":
    -   **FastCGI (Local - Pipes)** - Abyss vai iniciar o servidor mono automaticamente usando um piped socket. Pipes s�o a maneira mais r�pida de se comunicar e ter o Abyss repassando o seu pr�prio servidor, o que significa que voc� n�o ter� que fazer isto manualmente. Esta � possivelmente a op��o mais f�cil.
        Se voc� estiver usando esta op��o, Apenas configure o "Interpreter" para "/usr/bin/fastcgi-mono-server" ou "/usr/bin/fastcgi-mono-server2".
    -   **FastCGI (Remote - TCP/IP sockets)** - Abyss vai procurar pelo servidor mono em um endere�o IP e porta espec�ficos. Voc� pode usar isto para rodar o servidor em uma outra m�quinae redistribuir o processo de carga. O �nico alerta � que voc� precisar� iniciar o servidor mono manualmente em outro computador, usando um comando como "`fastcgi-mono-server2 /socket=tcp:8002`"
        Se estiver usando esta op��o, simplesmente configure o "Remote server IP Address" para o IP da m�quina que voc� estiver rodando o servidor do Mono, e a porta que voc� usou na linha de comando. Pela linha de comando que indicamos acima, esta deveria ser 8002.

5.  Desmarque a op��o "Check for file existence before execution". Esta op�o otimiza a performance mas pode corromper o ASP.NET 2.0 assim como algumas vezes usa caminhos que n�o necess�riamente existem como WebResource.axd.
6.  Desmarque a op��o "Use the associated extensions to automatically update the Script Paths".
7.  Adicione "\*" � "Extensions". Isto n�o � uma verdade, mas ser� usado para garantir que todas as requisi��es chegem ao FastCGI do servidor Mono.
8.  Clique em "OK".
9.  Voc� dever� ser automaticamente direcionado para "Scripting Parameters". Clique em "Add" na caixa "Script Paths".
10. Entre "/\*" no "Virtual Path".
11. Clique "OK".

### Geral Passo 2: Extender o tempo de vida do servidor

Ao completar o passo anterior, voc� deve ter automaticamente retornado ao "Scripting Parameters". Clique em "Edit..." pr�ximo a "FastCGI Parameters". A op��o "FastCGI Processes Timeout" especifica o numero de segundos depois da ultima requisi��o voc� vai quere aguardar antes de desligar o Servidor Mono FasCGI(ou qualquer outro). Porque p�ginas ASP.NET precisam ser recompiladas a AppDomains precisam sere recriados toda a vez que o servidor inicia. Voc� configura este para um valor arbitrariamente alto. 604800 � um numero de segundos de uma semana e o valor que escolhi para meu servidor. Apenas ap�s ter setado este valor clique em "OK". 


### Geral Passo 3: Desabilitando listar diret�rios

Ao completar o passo anterior, voc� dever� ter automaticamente retornadoao "Scripting Parameters". Clique bot�o "OK" rodap� da p�gina para voltar para a pagina de configura��o do host. Apenas l�, clique em "Directory Listing" e configure "Type" para "Disabled". Se a listagem de diret�rios estiver habilitada, os paths n�o ser�o automaticamente enviados para o servidor FastCGI Mono.

Usando Extens�es
----------------

### Alertas de Extens�es

**Usar extens�es ao inv�s de caminhos N�O � recomendado.** por favor consulte [FastCGI#Paths_vs._Extensions](/docs/web/fastcgi/#paths-vs-extensions) na p�gina principal por uma explica��o detalhada. Se voc� decidir usar esta configura��o, por favor tenha em mente que � menos seguro, sofre desvantegens adicionais quando comparado com o uso de paths.

### Exten��es Passo 1: Configurando o Interpretador

Ao iniciar o servidor web Abyss, um centro de controle do servidor web ser� iniciado por padr�o no endere�o
<http://localhost:9999>. Simplesmente abra seu navegador neste endere�o e siga os passos abaixo:teps outlined below:

1.  Clique em "Configure" no host que voc� deseja adicionar suporte a ASP.NET
2.  Clique em "Scripting Parameters".
3.  Clique em "Add" na caixa "Interpreters".
4.  Voc� est� agora na p�gina para adicionar o interpretador ASP.NET. As duas op��es que eu poderia recomendar s�o "FastCGI (Local - Pipes)" e "FastCGI (Remote - TCP/IP sockets)":
    -   **FastCGI (Local - Pipes)** - Abyss vai iniciar o servidor mono automaticamente usando um piped socket. Pipes s�o a maneira mais r�pida de se comunicar e ter o Abyss repassando o seu pr�prio servidor, o que significa que voc� n�o ter� que fazer isto manualmente. Esta � possivelmente a op��o mais f�cil.
        Se voc� estiver usando esta op��o, Apenas configure o "Interpreter" para "/usr/bin/fastcgi-mono-server" ou "/usr/bin/fastcgi-mono-server2".
    -   **FastCGI (Remote - TCP/IP sockets)** - Abyss vai procurar pelo servidor mono em um endere�o IP e porta espec�ficos. Voc� pode usar isto para rodar o servidor em uma outra m�quinae redistribuir o processo de carga. O �nico alerta � que voc� precisar� iniciar o servidor mono manualmente em outro computador, usando um comando como "`fastcgi-mono-server2 /socket=tcp:8002`"
        Se estiver usando esta op��o, simplesmente configure o "Remote server IP Address" para o IP da m�quina que voc� estiver rodando o servidor do Mono, e a porta que voc� usou na linha de comando. Pela linha de comando que indicamos acima, esta deveria ser 8002.

5.  Desmarque a op��o "Check for file existence before execution". Esta op�o otimiza a performance mas pode corromper o ASP.NET 2.0 assim como algumas vezes usa caminhos que n�o necess�riamente existem como WebResource.axd.
6.  Adicione as extens�es abaixo:
    -   aspx
    -   asmx
    -   ashx
    -   asax
    -   ascx
    -   soap
    -   rem
    -   axd
    -   cs
    -   config
    -   dll

7.  Clique em "OK".

### Extens�es Passo 2: Extendendo o Tempo de vida do servidor

Ao completar o passo anterior, voc� deve ter automaticamente retornado ao "Scripting Parameters". Clique em "Edit..." pr�ximo a "FastCGI Parameters". A op��o "FastCGI Processes Timeout" especifica o numero de segundos depois da ultima requisi��o voc� vai quere aguardar antes de desligar o Servidor Mono FasCGI(ou qualquer outro). Porque p�ginas ASP.NET precisam ser recompiladas a AppDomains precisam sere recriados toda a vez que o servidor inicia. Voc� configura este para um valor arbitrariamente alto. 604800 � um numero de segundos de uma semana e o valor que escolhi para meu servidor. Apenas ap�s ter setado este valor clique em "OK". 


### Extens�es Passo 3: Adicionando P�ginas Index

Ao completar o passo anterior,voc� deve automaticamente retornar para "Scripting Parameters". Clique em "OK" no rodap� da p�gina para retornar para a p�gina de configura��o do host. Apenas l�, clique em "Index Files" e continue adicionando `index.aspx`, `default.aspx`, e `Default.aspx`. Clique em "OK" e entao em "Reset" para reiniciar o servidor.

Sucesso
-------
Agora voc� deve estar com ASP.NET funcionando com o Servidor Web Abys. Divirta-se!

