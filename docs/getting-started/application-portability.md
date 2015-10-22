---
title: Application Portability
redirect_from:
  - /Guidelines:Application_Portability/
  - /Guide:Writing_Cross_Platform_applications/
---

Este documento descreve algumas praticas uteis na construção de software através dos sistemas Windows e Unix usando .NET e Mono.

Além dessa página, há alguns recursos que são uteis quando portamos softwares para rodar no Mono:

-   [ASP.NET em Mono](/docs/web/aspnet/)
-   [Implementação Windows.Forms em Mono](/docs/gui/winforms/)

Migrando Estratégia
================

Hoje o Mono contem o núcleo de biblioteca de desenvolvimento  e ferramentas de implantação. A maioria dessas ferramentas são em linhas de comando e compiladore, nao há uma substituição completa do Visual Studio para fazer aplicações do Windows.Forms e ASP.NET nativa no Linux.

Existe uma série de estratégias que você pode adotar dependendo o quão confortável você esteja com o uso do Linux.

Desenvolvendo no Windows, rodando no Unix
--------------------------------------

Você pode continuar o uso do Visual Studio para desenvolver suas aplicações no Windows, os binários produzidos pelo Visual Studio são binários compatíveis com Mono, então você só precisa obter os arquivos pro seu servidor  Linux/Unix.

Você pode fazer isso manualmente copiando os arquivos(usando o comando XCopy que é provavelmente mais fácil), mas o ideal seria você ter com compartilhamento de rede entre Unix e Windows.

-   Você pode montar no Linux, compartilhar no Windows.
-   Você pode montar no Windows, compartilhar no Linux.

**Importante** O Visual Studio pode produzir executáveis "incrementais", são executáveis que não tem suas tabelas comprimidas, e são usados pelo Visual Studio durante a construção do aplicativo.

Esses assemblies não funcionam com o Mono, como não os suporta, você precisa desabilitar "construções  incrementais".

A opção de incremento pode ser configurada em Configurações do Projeto/Propriedades Avançadas.

Desenvolvendo Nativamente no Linux
----------------------------

Existem algumas IDE's que podem ser usadas no Linux, com vários graus de funcionalidades:

-   [X-Develop](http://www.omnicore.com/xdevelop.htm) da Omnicore, é um produto comercial que prove auto-complete, um design GUI multi-plataforma e funciona em Windows, Linux e OS/X.
-   [MonoDevelop](http://www.monodevelop.com) é uma IDE produzida pela comunidade Mono que tem suporte para desenvolvimento de aplicações gráficas com Gtk#, mas atualmente falta suporte para Windows.Forms ou ASP.NET.

Se você se sente confortável usando ferramentas de linhas de comando, e qualquer outro editor de Unix , também funcionaria muito bem.

Diretrizes Gerais
==================

Iniciando com Mono 1.1.18, nova funcionalidade, o  [IO Remapping](/docs/advanced/iomap/) que lida com maiúscula e minúscula nos sistemas  de arquivos e com os separadores de diretórios nos nomes dos arquivos.

Usando a funcionalidade [IOMap](/docs/advanced/iomap/) que é uma forma rápida de migrar suas aplicações, mas a praticidade continua util no qual ajudaria suas aplicações a não depender do remapeamento (já que o remapeamento tem um pequeno problema de desempenho).

Veja a pagina do [IOMap](/docs/advanced/iomap/) para mais informações e de como usa-lo.

Maiúscula e minúscula
----------------

Os sistemas Linux e Unix, tem diferenciação de maiúsculas e minúsculas

No Linux e Unix os arquivos "readme" e "README" são dois arquivos diferentes, isso é Linux tem diferença entre maiúscula e minúsculas em seu sistema de arquivos.

Isso é uma distinção importante paras muitas aplicações porque você pode ter criado uma pagina "Login.aspx", mas você a referencia como "login.aspx" ou "LOGIN.ASPX" no seu código fonte. Você precisa ter certeza que todas as referencias estão usando os nomes dos arquivos na mesma caixa.

Alternativamente, iniciando com o mono 1.1.18,voce pode configurar a variável ambiente MONO_IOMAP para "case" ou  "all" para eliminar esses problemas. Veja [IOMap](/docs/advanced/iomap/) para mais detalhes.

Separadores de caminhos
---------------

No Windows, o separador de caminhos é "\\" enquanto no Linux é "/", é possível de criar arquivos que contenha um "\\"  no nome dos arquivos no Linux.

Para a rápida migração dos aplicativos, iniciando com o Mono 1.1.18, voce pode configurar a variável de ambiente  MONO_IOMAP para "all", e ele cuidará disso. Veja mais detalhes sobre [IOMap](/docs/advanced/iomap/).

Para criar um software portável, você precisa certificar-se que usa o caractere  [http:/monodoc/P:System.IO.Path.DirectorySeparatorChar System.IO.Path.DirectorySeparatorChar] quando você precisa concatenar caminhos, melhor usar o metodo  [http:/monodoc/M:System.IO.Path.Combine(string,string) System.IO.Path.Combine] para combinar nomes de caminho de arquivos.

Além disso, o código examina manualmente por separadores de caminhos que precisam ser alterados, por exemplo:

``` csharp
int index = exePath.LastIndexOf("\\");
exeDir = exePath.Substring(0, index);
exeFile = exePath.Substring(index+1);
```

Para facilitar, pode ser alterado para:

``` csharp
exeDir = Path.GetDirectoryName (exePath);
exeFile = Path.GetFileName (exePath);
```

As variáveis de ambiente PATH e outras, têm diretórios separados por um ponto e virgula ";" no Windows e dois pontos ":" no Linux. Você pode usar System.IO.Path.PathSeparator para separar os caminhos nas variáveis de ambiente.

``` csharp
Console.WriteLine("Subdirectories found in the PATH environment variable:");
string path_env = Environment.GetEnvironmentVariable ("PATH");
string[] path_dirs = path_env.Split (Path.PathSeparator);
foreach (string pathdir in path_dirs)
    Console.WriteLine(pathdir);
```

Diretrizes:

1. Em geral, eu iria tentar encapsular "all" operações que operam em caminhos de arquivos. Maiúscula e minúscula, o PathSeparator,o DirectorySeparatorChar, etc, estão todas em questão. Mas sutilmente inclui oque é um caminho absoluto: /bin é absoluto no Linux mas precisa de um drive para fazer com que fique absoluto no Windows.
2. Para classes que manipulam caminhos, são uteis para usar a injeção de dependência para a plataforma, ao invés de ter que detecta-los. Dessa maneira, você pode testar cada plataforma sob apenas uma.
3. Para alguns testes, você pode usar caminhos como /a/b/c que trabalham em ambas as plataformas, mas cuidado: Métodos como Path.GetAbsolutePath() irão ter comportamento diferente em cada plataforma.

Nomes absolutos de caminhos
-------------------

Não use nomes absolutos de caminhos em sua aplicação, isso não funcionara em Windows e em Linux ao mesmo tempo.

Se você precisar encontrar alguns serviços, deve-se usar API's do .NET, por exemplo, para descobrir o arquivo de configuração da sua aplicação, poderá fazer isso com:

``` csharp
AppDomain.CurrentDomain.SetupInformation.ConfigurationFile
```

Platform Invocation (P/Invoke)
------------------------------

A maioria dos códigos contendo P/Invoke chamada para bibliotecas nativas do Windows (ao contrario que P/Invoke feito para sua própria biblioteca em C)  precisará ser adaptado para uma chamada equivalente no Linux , ou o código terá que ser reformulado para uma chamada diferente.

Uso do Registro
--------------

As classes de registro são fornecidas no Linux, mas são uteis apenas quando as opções do seu aplicativo a usa como referencia.

As classes de registros não irão fornecer informações uteis sobre qualquer configurações do sistema operacional.

Nome de chaves e valores são convertidos para a caixa baixa pelo Mono. Normalmente é case-insensitive, mas se sua aplicação obtiver os nomes dos registros e os comparar com um valor fixo, você precisará usar comparação case-insensitive. Isso se aplica mais frequentemente para testes de códigos, mas podem surgir outras situações.

Endianess
---------

Para fazer seu código rodar em varias plataformas, você pode deixar seu codigo Endiar-idependent. Isso significa que você pode não assumir a ordem dos bytes.

Veja [endianess](http://en.wikipedia.org/wiki/Endianess) no Wikipedia para mais detalhes.

Orientações sobre ASP.NET
==================

Caso queira familiarizar-se mais com as perguntas frequentes sobre ASP.NET no Mono, visite: [FAQ: ASP.NET](/docs/faq/aspnet/)

Orientações sobre Windows.Forms
========================

Caso queira se familiarizar-se mais com as perguntas frequentes sobre Windows.Forms no Mono, visite: [FAQ: Winforms](/docs/faq/winforms/).

Migração de banco de dados
==================

Você pode continuar usando seu Banco de dados SQL Server com Mono, nao precisa migrar. Mas se você deseja substituir o SQL Server por outro Banco de dados, Mono fornece uma ampla opção de conectores de banco de dados para MySQl,  PostgreSQL, Sysbase, Oracle, IBM, DB2, Firebird, ODBC, e também para o banco de dados embutido o SQLite.

Seu código e suas strings de conexão  já existentes, continuam funcionando. Apenas a única mudança necessária seria  o nome do servidor do banco de dados.

Falta de funcionalidade
=====================

Mono carece de uma série de recursos disponíveis na NET, aqui estão algumas das principais características que  faltam.

Sem Enterprise.Services
----------------------

Se sua aplicação requerer EnterproseServices, sua aplicação possivelmente não funcionará no Mono, nós não a implementamos.

Sem transações entre processos
-----------------------------

O Mono atualmente suporta apenas transações de processos locais.

Sem COM
------

COM não existe no Unix como parte de um sistema operacional e Mono atualmente não fornece suporte.

Se sua aplicação requerer COM's, você  precisará  fazer a chamada de uma [P/Invoke](/docs/advanced/pinvoke/).

Atualmente é um projeto de suporte em desenvolvimento, veja [COM Interop](/docs/advanced/com-interop/) para mais detalhes.

Interface do usuário
==============

Nessa seção nos iremos cobrir as diferenças entre Windows.Forms e Gtk# e mostrar as diferenças e como estruturar um aplicativo que tenha múltiplas Uis dependendo da plataforma.

Usando Windows.Forms como uma API Multi-Plataforma
-------------------------------------------

Usando uma UI nativa para cada plataforma
-----------------------------------

Uma UI por Sistema operacional, centrais de divisão UI, mensão gtk# , cocoa#.

Portando ferramentas
=============

MonoDevelop
-----------

A versão 0.14 do MonoDevelop é capaz de carregar as soluções do Visual Studio 2005 e pode compilar os projetos ou gerar makefiles do Unix.

Basta importar o arquivo .sln do Visual Studio 2005 pro MonoDevelop e compilar normalmente. Para gerar Makefiles, clique com o direito do mouse em soluções e selecione "Generate Autotools Framework".

installvsts
-----------

Você pode usar o programa **installvst** para instalar as variedades de kits que são distribuídos a partir do site  [http://asp.net](http://asp.net) site.

Prj2make
--------

Se você escolher mudar seu desenvolvimento para Linux, a ferramenta prj2make será útil.

Mono embarcou com uma ferramenta chamada prj2make que transforma soluções do Visual Studio 2003 nas séries do Makefiles. O programa é capaz de transformar 50% das soluções do Makefiles, e pode ser útil em dar o primeiro passo para a conversão do seu projeto para o Unix-makefiles.

Um problema que prj2make tem, é a incapacidade de lidar com nomes de arquivos diferentes, então tem a possibilidade das referencias do seu projetos referenciar "Core.cs" enquanto o arquivo salvo se chama "core.cs", caso de problema na compilação, verifique os nomes das referências.

