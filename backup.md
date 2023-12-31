#### Windows layout

~~~
C:\ ──+
      ├── Program Files
      ├── Program Files (x86)
      ├── Users
            └── student
            └── bart
      ├── System
      ├── System32
      ├── SysWOW64
      ...
D:\ ──+
      ├── een_folder
      ...
~~~


#### Linux layout

~~~
/ ──+
    ├── bin -> usr/bin (link)
    ├── boot
    ├── dev
    ├── etc
    └── home
          └── student
          └── bart
    ├── media
    ├── mnt
    ├── opt
    ├── proc
    ├── root
    ├── run
    ├── sbin -> usr/sbin (link)
    ├── srv
    ├── sys
    ├── tmp
    └── usr
    └── var
    ...
~~~

## Werken met een "File Explorer" (verkenner)

Deze bestanden kan je via een "fileexplorer" bekijken en openen.

### Windows "File Explorer"

De Windows kan je deze openen via het menu:

![drawing](windows_fileexplorer_nav.png)

En kan je via deze explorer navigeren naar directories en bestanden openen

![](windows_fileexplorer.png)

### Linux "File Explorer"

Op Linux heb je - afhankelijk van je distributie - vergelijkbare tools.  
Hieronder zie je op Linux Mint het gebruik van Nemo-fileexplorer

![](linux_fileexplorer.png)

### Mac OSx "File Explorer" (Finder)

Mac OSx gebruik het dan weer het reeds geïnstalleerde **Finder**

![](mac_fileexplorer.jpg)

#### MacOSx

#### Structuur van commando's


## Pipes en redirection

### In- en output van programma's

Programma's en scripts hebben meestal in- en output nodig op te kunnen werken.  
Op de command-line zijn er 3 belangrijke elementen

* Input bij de **start** van het **programma**: **argumenten**
* In- en output tijdens de **uitvoering** van het programma: **STDIN, STDOUT en STDERR**
* Output bij het einde van het programma

~~~
START PROGRAMMA:       argumenten   
                           |
                           V                        (1)
(0)                 +------+-----+----> standard output
standard input ---->|  processs  |  
                    +------+-----+---->  standard error
                           |                        (2)
                           V
EINDE PROGRAMMA:       exit-code    
~~~

#### Input bij de start: argumenten (en opties)

Het eerste element hebben we al een aantal maal toegepast bij het gebruiken van diverse commando's.  
Als je en programma aanroept kan je daar namelijk een aantal extra argumenten aan toevoegen.

~~~
START PROGRAMMA:       argumenten   
                           |
                           V      
                    +------+-----+
                    |  processs  |  
                    +------+-----+
~~~

Een voorbeeld:

~~~
student@studentdeb:~$ ls -l hello.sh 
-rwxr--r-- 1 student student 60 Mar 13 20:04 hello.sh
student@studentdeb:~$
~~~

In dit geval is "-l" en "hello.sh" beide argumenten.  

* "-l" in dit geval is optie (een speciaal soort argument voorafgegaan door een koppelteken)
* "hello.sh" als 2de (en eigenlijk) argument

#### Output bij het einde van het programma: Exit-code

Voorgaande was bij de **start** van het programma om eventuele parameter of argumenten door te geven.  

~~~
START PROGRAMMA:       argumenten   
                           |
                           V      
                    +------+-----+
                    |  processs  |  
                    +------+-----+
                           |
                           V
EINDE PROGRAMMA:       exit-code    
~~~

Elke applicatie (binnen linux) zal bij het beeindigen van het programma een code teruggeven.  
Deze code noemen we ook exit-code en heeft als bedoeling informatie mee te geven over het al dan niet successvol uitvoeren van het commando.

Deze commando kan je vanuit de shell opvragen via een speciale variable **$?**.  
Bij normale uitvoering  - **zonder error** of **warnings** - zal deze waarde (bij conventie) **0** zijn.

~~~
student@studentdeb:~$ ls -l hello.sh 
-rwxr--r-- 1 student student 60 Mar 13 20:04 hello.sh
student@studentdeb:~$ echo $?
0
~~~

Als we echter een **foutje** maken bij de uitvoering zal de applicatie (bij conventie) een code opwerpen **verschillend** van **0**  
In onderstaand voorbeeld geven we de naam van een nieu bestaand bestand door aan ls waarop dat deze een exit-code 2 opwerpt.

~~~
student@studentdeb:~$ ls -l hello.sh.not 
ls: cannot access 'hello.sh.not': No such file or directory
student@studentdeb:~$ echo $?
2
student@studentdeb:~$
~~~

Deze code is niet voor elke commando (of zelfs fout binnen een commando) identiek.  
Het 2de voorbeeld - cd naar een niet bestaande directory - bevestigd dit...

~~~
student@studentdeb:~$ cd bestaatniet
bash: cd: bestaatniet: No such file or directory
student@studentdeb:~$ echo $?
1
student@studentdeb:~$ echo $?
0
student@studentdeb:~$ 
~~~

Laatste belangrijke bemerking is dat deze code altijd wordt overschreven, het bevat enkel de exit-code van het laatste commando (ongeacth of deze 0 of verschillend is).  
Bij het volgende commando zal deze terug worden overschreven, in bovenstaand voorbeeld zal de echo zelf de exit-code terug op 0 zetten.

> Nota: als je gewoon een enter drukt zal deze exit-code niet worden overschreven omdat er geen applicatie of commando is uitgevoerd

##### exit-code bij scripts

Vanuit een script kan je trouwens ook de exit-code bepalen.  
Dat kan je via het commando exit gevolgd door een integer (geheel getal)

~~~bash
#!/bin/bash

echo "Hello exit-demo"
exit 25
~~~

Als je dit uitprobeert zie ja dat er inderdaad 25 door het script wordt doorgegeven.

~~~
student@studentdeb:~$ ./exit_code_demo.sh
Hello exit-demo
student@studentdeb:~$ echo $?
25
student@studentdeb:~$ 
~~~

#### Tijdens de uitvoering: stdin, stdout en stderr

Een proces binnen een Linux-distro (maar ook UNIX- en ander POSIX-compliant OS)
heeft altijd automatich **3 files** of **streams** ter beschikking

* **STDIN**: standard input
* **STDOUT**: standard output
* **STDERR**: standard error

Dit zijn datastromen die je een applicatie **"standaard"** zal **doorgeven** (STDOUT en STDERR) aan de **shell**.  
Vanuit de shell echter kan je deze **datastromen** doorgeven aan **andere** **applicaties** via een aantal operatoren (>, >>, <, |)

##### STDOUT

De eerste is STDOUT, dit is de tekst/output dat je applicatie produceert

~~~
START PROGRAMMA:       argumenten   
                           |
                           V                        (1)
                    +------+-----+----> standard output
                    |  processs  |  
                    +------+-----+
                           |
                           V
EINDE PROGRAMMA:       exit-code    
~~~

In onderstaand voorbeeld zal de standard-output van het commando "ls" naar de shell doorsturen

~~~
student@studentdeb:~$ ls -l hello.sh 
-rwxr--r-- 1 student student 60 Mar 13 20:04 hello.sh
student@studentdeb:~$
~~~

##### Redirection operator > (overwrite)

Deze output kan je echter "redirect"-en naar een een file.  
Dit doe je door na het commando een **```>```** teken te plaatsen gevolgd naar welk file je wil schrijven zoals hieronder geillustreerd.

~~~
student@studentdeb:~$ ls -l hello.sh > lsout
student@studentdeb:~$ cat lsout
-rwxr--r-- 1 student student 60 Mar 13 20:04 hello.sh
student@studentdeb:~$
~~~

Een 2de voorbeeld is als je een file wil aanmaken met reeds wat tekst in:

~~~
student@studentdeb:~$ echo "Hello World" > helloworld 
student@studentdeb:~$ cat helloworld
Hello world
student@studentdeb:~$
~~~

##### Redirection operator >> (append)

De >-operator zal een file overschrijven, al er reeds een file bestaat zal deze worden overschreven met de volledige output van het commando.

~~~
student@studentdeb:~$ ls -l hello.sh > lsout
student@studentdeb:~$ cat lsout
-rwxr--r-- 1 student student 60 Mar 13 20:04 hello.sh
student@studentdeb:~$ 
~~~

Als je echter het bestand niet wil overschrijven gebruik je de **>>-redirection-operator**.  
Om te vermijden dat we voorgaande input niet overschrijven gebruiken we deze operator.

~~~
tudent@studentdeb:~$ ls -l hello.sh >> lsout
student@studentdeb:~$ ls -l hello.sh >> lsout
student@studentdeb:~$ cat lsout
-rwxr--r-- 1 student student 60 Mar 13 20:04 hello.sh
-rwxr--r-- 1 student student 60 Mar 13 20:04 hello.sh
-rwxr--r-- 1 student student 60 Mar 13 20:04 hello.sh
student@studentdeb:~$ 
~~~

Als gevolg hiervan zien we dat de file is aangevuld...

##### STDERR

Naast **STDOUT** is er echter nog een **2de** output-stream, namelijk **STDERR**.  

~~~
START PROGRAMMA:       argumenten   
                           |
                           V                        (1)
                    +------+-----+----> standard output
                    |  processs  |  
                    +------+-----+---->  standard error 
                           |                        (2)
                           V
EINDE PROGRAMMA:       exit-code    
~~~

Een **applicatie** zal **foutboodschappen** doorsturen naar de **STDERR**, niet naar **STDOUT**.
Bij volgende voorbeeld proberen we express een nietbestaande file op te lijsten en de outputweg te schrijven naar een file.

~~~
student@studentdeb:~$ ls -l hello.sh.not > lsout
ls: cannot access 'hello.sh.not': No such file or directory
student@studentdeb:~$ cat lsout 
student@studentdeb:~$ 
~~~

Hier zien we dat de **file leeg** is, dit is omdat de enige output van het ls-commando de foutboodschap was die je op de console zag verschijnen.  

##### Redirect van STDERR via 2>

Als je er echter wil voor zorgen dat de **error-output** naar een file wordt weggeschreven kan je dit door een nummer toe te voegen aan het redirect-symbool, voor de **error-stream** is dit altijd **2**

~~~
student@studentdeb:~$ ls -l hello.sh.not 2> lserr
student@studentdeb:~$ cat lserr 
ls: cannot access 'hello.sh.not': No such file or directory
student@studentdeb:~$ 
~~~

Je kan ook zorgen dat **beide** streams **tegelijkertijd** worden weggeschreven.  
In onderstaand voorbeeld:

* lijsten zowel een bestaande als niet bestaande file op
* de error-output gaat naar lserr
* de gewone output gaat naar lsout 

~~~
student@studentdeb:~$ ls -l hello.sh hello.sh.not >lsout 2> lserr
student@studentdeb:~$ cat lserr 
ls: cannot access 'hello.sh.not': No such file or directory
student@studentdeb:~$ cat lsout
-rwxr--r-- 1 student student 60 Mar 13 20:04 hello.sh
student@studentdeb:~$ 
~~~

##### Redirect van beide STDOUT en STDERR via &>

Als je beide tegelijkertijd will redirecten en je **&>** gebruiken in de plaats hiervan.  
In het **voorbeeld** hieronder zullen **beide streams** naar **lsall** worden weggeschreven.

~~~
student@studentdeb:~$ ls -l hello.sh hello.sh.not &> lsall
student@studentdeb:~$ cat lsout 
-rwxr--r-- 1 student student 60 Mar 13 20:04 hello.sh
ls: cannot access 'hello.sh.not': No such file or directory
student@studentdeb:~$ 
~~~

##### STDIN

Een 3de stream is STDIN, dit is de standaard input die een applicatie meekrijgt vanaf de shell.

~~~
START PROGRAMMA:       argumenten   
                           |
                           V                        (1)
(0)                 +------+-----+----> standard output
standard input ---->|  processs  |  
                    +------+-----+---->  standard error
                           |                        (2)
                           V
EINDE PROGRAMMA:       exit-code    
~~~

Om dit de demonstreren gebruiken we het **commando wc**.  
wc is de afkorting voor **wordcount** en geeft (standaard zonder argumenten) 3 outputs:

* Aantal lijnen
* Aantal woorden
* Aantal karakters

Als je dit commando uitvoert zonder argumenten zal dit commando wachten op input van de console,
namelijk STDIN.  
In onderstaand voorbeeld typen we wat tekst gevolgd, om deze tekst (STDIN) te beeindigen gebruiken we "Ctrl + D"

~~~
student@studentdeb:~$ wc
hello
world
greetings from the shell
      3       6      37
student@studentdeb:~$ 
~~~

##### Redirection vanuit een file naar STDIN via <

Net als we met > naar een file kunnen afleiden, kunnen we inhoud van een file afleiden naar STDIN.  
Dit kunnen we via de operator **<**.

We komen terug op ons voorgaand voorbeeld waar we een file aanmaken met 2 lijnen.

~~~
student@studentdeb:~$ ls -l hello.sh hello.sh.not &> lsall
student@studentdeb:~$ cat lsall
ls: cannot access 'hello.sh.not': No such file or directory
-rwxr--r-- 1 student student 60 Mar 13 20:04 hello.sh
~~~

Als we nu de inhoud van deze file willen afleiden naar het wc-commando kunnen we dit doen als hieronder.

~~~
student@studentdeb:~$ wc < lsall
  2  18 114
student@studentdeb:~$ 
~~~

##### Redirection van STDIN/STDOUT vanuit een ander process/commando naar STDIN via |

In bovenstaand voorbeeld werkten we nog altijd met een tussenfile - lsall - om de output
van het ls-commando te verbinden aan het wc-commando.

Er is echter een operator die de output-stream van het ene commando (ls) men de inputstream van het andere commando (wc).  
Om dit te doen moet een pipe-opetor of | tussen beide commando's plaatsen zoals hieronder geillustreerd.

~~~
bart@studentdeb:~$ ls -l hello.sh hello.sh.not | wc
ls: cannot access 'hello.sh.not': No such file or directory
      1       9      52
bart@studentdeb:~$
~~~

Bemerk hier wel dat deze pipe-operator enkel de stderror verbindt met de stdin.

~~~
            STDOUT    |    STDIN   STDOUT     CONSOLE:
    +------+-----+-------->+------+-----+---->  1       9      52 
    |  ls -l ... |         |     WC     |
    +------+-----+         +------+-----+
~~~


##### Redirection vanuit een ander process/commando naar STDIN via |&

Wil je toch beide proberen dan dien je dit te doen met een variant van de pike-operator die beide **STDERR en STDIN cobmineert**, namelijk **|&**

~~~
bart@studentdeb:~$ ls -l hello.sh hello.sh.not |& wc
      2      18     112
bart@studentdeb:~$ 
~~~

Met deze operator worden zowel de STDOUT als de STDERR bij elkaar gevoegd
en doorgegeven aan de STDIN van WC.

~~~
            STDOUT  |&      STDIN   STDOUT     CONSOLE:
    +------+-----+---+----->+------+-----+---->  2      18     112 
    |  ls -l ... |   |      |     WC     |
    +------+-----+---+      +------+-----+
            STDERR
~~~



## Top 10 commando's

### Powershell

### Bash

## Scripting

### Powershell scripting


## Uitvoeren van programma's

### Uitvoeren van programma's in Powershell

Naast het bekijken en manipuleren van je file-systeem kan je ook programma's uit voeren.  
Dit doe je door in een directory (waar)

~~~powershell
PS C:\Users\Bart>cd mijn_eerste_programma

PS C:\Users\Bart\mijn_eerste_programma>dir
 Volume in drive C is System
 Volume Serial Number is E687-8D34

 Directory of C:\Users\bart\mijn_eerste_programma

02/02/2017  14:35    <DIR>          .
02/02/2017  14:35    <DIR>          ..
02/02/2017  14:24                77 hello.c
02/02/2017  14:24                99 hello
               2 File(s)            176 bytes
               2 Dir(s)  123.094.589.440 bytes free

PS C:\Users\Bart\mijn_eerste_programma>hello
Hello World
~~~

### Uitvoeren van programma's in Bash

Naast het bekijken en manipuleren van je file-systeem kan je ook programma's uit voeren.  
Dit doe je door in een directory (waar)

~~~bash
demo@demohost ~/mijn_eerste_programma $ ls
hello.c hello
demo@demohost ~/mijn_eerste_programma $ ./hello
Hello World
~~~

### Error-levels

#### Error-levels in Powershell

Naast je eigen variabelen houdt je operating systeem ook een aantal variabelen bij.  

~~~powershell
PS C:\Users\Bart> echo $?                                    True                                                         PS C:\Users\Bart> ls qbsdfs                                  ls : Cannot find path 'C:\Users\Bart\qbsdfs' because it      does not exist.                                              At line:1 char:1                                             + ls qbsdfs                                                  + ~~~~~~~~~                                                      + CategoryInfo          : ObjectNotFound: (C:\Users\Bar     t\qbsdfs:String) [Get-ChildItem], ItemNotFoundException       + FullyQualifiedErrorId : PathNotFound,Microsoft.PowerS     hell.Commands.GetChildItemCommand                                                             
PS C:\Users\Bart>   
~~~

De variabele **$?** bijvoorbeeld houdt de error-code van de laatst uitgevoerde applicatie bij.

#### Error-levels in Bash

Naast je eigen variabelen houdt je operating systeem ook een aantal variabelen bij.  

~~~bash
demo@demohost ~ $ cd een_directory_die_niet_bestaat
demo@demohost ~ $ echo $?
1
demo@demohost ~ $ cd mijn_eerste_programma/
demo@demohost ~/mijn_eerste_programma $ echo $?
0
~~~

Deze variabele $? zal de error-code bijhouden die door het laatste programma was teruggegeven aan de shell.  

### Environment-variabelen bekijken

#### Environment-variabelen bekijken in Powershell

Als je alle variabelen willen zien moet je gewoon SET typen

~~~powershell
PS C:\Users\Bart\mijn_eerste_programma>SET
ALLUSERSPROFILE=C:\Documents and Settings\All Users
APPDATA=C:\Documents and Settings\bart\Application Data
CLIENTNAME=Console
CommonProgramFiles=C:\Program Files\Common Files
COMPUTERNAME=V-HJLVYI8TKYLPA
ComSpec=C:\WINDOWS\system32\cmd.exe
FP_NO_HOST_CHECK=NO
HOMEDRIVE=C:
HOMEPATH=\Documents and Settings\bart
KMP_DUPLICATE_LIB_OK=TRUE
LOGONSERVER=\\V-HJLVYI8TKYLPA
MKL_SERIAL=YES
NIEXTCCOMPILERSUPP=C:\Program Files\National Instruments\Shared\ExternalCompi
Support\C\
NUMBER_OF_PROCESSORS=1
OS=Windows_NT
Path=C:\WINDOWS\system32;C:\WINDOWS;C:\WINDOWS\System32\Wbem
PATHEXT=.COM;.EXE;.BAT;.CMD;.VBS;.VBE;.JS;.JSE;.WSF;.WSH;.py;.pyw
PROCESSOR_ARCHITECTURE=x86
PROCESSOR_IDENTIFIER=x86 Family 6 Model 42 Stepping 7, GenuineIntel
PROCESSOR_LEVEL=6
...
~~~

#### Environment-variabelen bekijken in Bash

Als je alle variabelen willen zien moet je gewoon het commando printenv typen

~~~bash
demo@demohost ~/mijn_eerste_programma $ printenv
LC_PAPER=de_BE.UTF-8
XDG_VTNR=8
SSH_AGENT_PID=2713
XDG_SESSION_ID=c1
LC_ADDRESS=de_BE.UTF-8
LC_MONETARY=de_BE.UTF-8
COMP_WORDBREAKS= 	
"'><;|&(:
QT_STYLE_OVERRIDE=gtk
GPG_AGENT_INFO=/home/bart/.gnupg/S.gpg-agent:0:1
TERM=xterm-256color
...
~~~

#### Exacte verwijzing 

Zoals eerder vermeld moest je het script prefixen met **.\\** als je dit uitvoert.  
Enkel de naam de naam van het typen volstaat niet zoals je hieronder ziet.  

~~~ps1
PS C:\Users\Bart\pstryout> Write-Hello.ps1
Write-Hello.ps1 : The term 'Write-Hello.ps1' is not recognized as the name of a cmdlet, function, script file,
or operable program. Check the spelling of the name, or if a path was included, verify that the path is
correct and try again.
At line:1 char:1
+ Write-Hello.ps1
+ ~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (Write-Hello.ps1:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException


Suggestion [3,General]: The command Write-Hello.ps1 was not found, but does exist in the current location. Windows PowerShell does not load commands from the current location by default. If you trust this command, instead type: ".\Write-Hello.ps1". See "get-help about_Command_Precedence" for more details.
PS C:\Users\bartvoe>
~~~

Dit komt omdat je een **exacte path** dient te gebruiken en in dit geval kan je **relatief verwijzen** door **.\**

TODO: PATH demonstreren...