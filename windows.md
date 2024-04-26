---
title: Windows
---

Exibição
----------

Atualizar papel de parede

    RUNDLL32.EXE USER32.DLL,UpdatePerUserSystemParameters 1, True

Obter resolução na linha de comando:

    wmic desktopmonitor get screenheight, screenwidth

Alternar displays na linha de comando:

    DisplaySwitch.exe /internal
    DisplaySwitch.exe /clone
    DisplaySwitch.exe /extend
    DisplaySwitch.exe /external

Via atalho de teclado:

    Winkey + P



Prompt de comando
----------

Prompt colorido com logotipo do windows

    setx prompt $E[1;34;41m_$E[1;42;33m_$E[0m$S$E[1;36m$P$G$E[0m$S

Alterar tamanho da janela (Aviso: vai limpar o conteúdo da janela):

    mode con:cols=80 lines=40

Obter tamanho da janela

    for /f "tokens=1,2 delims=: " %a in ('mode con^|findstr "Linhas Colunas"') do @echo set %a=%b

Alterar título da janela:

    title "Título desejado"

Alterar cor desejada (primeiro dígito é o fundo e o segundo é a letra):
color 1f

    0 = Preto        8 = Cinza
    1 = Azul         9 = Azul claro
    2 = Verde        A = Verde claro
    3 = Verde-água   B = Verde-água claro
    4 = Vermelho     C = Vermelho claro
    5 = Roxo         D = Lilás
    6 = Amarelo      E = Amarelo claro
    7 = Branco       F = Branco brilhante

Código de página

ANSI:

    CHCP 1252

UTF-8:

    CHCP 65001


Para habilitar seleção colorida no Command Prompt, editar no registro:

    [HKEY_CURRENT_USER\Console]
    "EnableColorSelection"=dword:00000001

Atalho | Cor
-------|-------
Ctrl+1 | Cinza
Ctrl+2 | Chumbo
Ctrl+3 | Azul
Ctrl+4 | Verde
Ctrl+5 | Ciano
Ctrl+6 | Vermelho
Ctrl+7 | Roxo
Ctrl+8 | Amarelo
Ctrl+9 | Branco
Ctrl+0 | Laranja


Variáveis de ambiente padrão
----------

Windows Directory

    %WINDIR%
    %SYSTEMROOT%

Hard Drive That Contains OS

    %HOMEDRIVE%

Users Home Directory

    %HOMEPATH%
    %USERPROFILE%

Default Temporary Directory

    %TEMP%
    %TMP%

Program Files

    %PROGRAMFILES%

Current Users Application Data Directory

    %APPDATA%


Editar variáveis de ambiente

    rundll32 sysdm.cpl,EditEnvironmentVariables

Ou, pressionar a tecla WIN e buscar por "ambiente" ou "environment".


Caminho dos atalhos da barra de tarefas:

    %AppData%\Microsoft\Internet Explorer\Quick Launch\User Pinned\TaskBar


Caminho dos atalhos de "Enviar para":

    %AppData%\Microsoft\Windows\SendTo
    - ou -
    shell:sendto

Outros caminhos úteis:

    shell:desktop
    shell:sendto

Extensões do Chrome

    %userprofile%\AppData\Local\Google\Chrome\User Data\Default\Extensions

    
Desligar/reiniciar/etc.
----------

Shutdown Computer

    Shutdown.exe -s -t 00

Restart Computer

    Shutdown.exe -r -t 00

Lock Workstation

    Rundll32.exe User32.dll,LockWorkStation

Hibernate Computer

    rundll32.exe PowrProf.dll,SetSuspendState

Sleep Computer (só funciona se a hibernação estiver desativada, para desativar hibernação: powercfg -hibernate off)

    rundll32.exe powrprof.dll,SetSuspendState 0,1,0

Adiar hibernação ou permitir cancelamento:

    timeout /t 600 /nobreak && echo shutdown /h
    choice /n /t 3600 /d S /m "Abortar hibernacao?" & if errorlevel 2 echo shutdown -h

Instalação sem acesso de administrador
----------

OBS: Não funciona em todos os casos!

    msiexec /a arquivo.msi /qb TARGETDIR=c:\caminho\instalacao


Rede
----

Listar as portas TCP que estão abertas ou que estão conectadas:

    netstat -na | find "LISTENING"
    netstat -na | find "ESTABLISHED"

Listar interfaces de rede:

    ipconfig /all

Ping sem parar:

    ping -t  127.0.0.1

Ping uma vez:

    ping -n 1 127.0.0.1

Diagnóstico

    TRACERT

DNS

    nslookup

    nslookup
    > set q=mx
    > google.com

    nslookup
    > set q=txt
    > google.com

Força um "refresh" em todas as unidades de rede:

    for /f "skip=1" %a in ('wmic logicaldisk get caption') do dir %a > nul

Ou:

    (for %i in (a b c d e f g h i j k l m n o p q r s t u v w x y z) do dir %i: >nul 2>nul) >nul


Firewall
----------

Exibir perfis

    netsh advfirewall show allprofiles

Todas as regras:
    
    netsh advfirewall firewall show rule name=all

Criar regra

    netsh advfirewall firewall add rule name="sqldeveloper" dir=in action=allow program="C:\Oracle\sqldeveloper_4.0.3\sqldeveloper.exe"



Manipulando strings com FOR
---------------------------

Converte a lista de processos em um CSV

    for /f "tokens=1,2,3,4,5,6" %a in ('tasklist') do @echo %a;%b;%c;%d;%e;%f

Converte listagem de arquivos (excluindo diretórios) da pasta atual num CSV

    for /f "tokens=1,2,3,4" %a in ('dir /a-d ^| find "/"') do @echo %a;%b;%c;%d

Lê um CSV, inverte a posição das colunas e troca o delimitador para ;. Obs: Não funciona bem se o conteúdo tiver "," entre aspas

    for /f "tokens=1-6 delims=," %a in ('type test.csv') do @echo %f;%e;%d;%c;%b;%a

O valor passado em IN é uma string literal e não um comando (usebackq)

    for /f "usebackq tokens=1-4 delims=;" %a in ('valor1;valor2;valor3;valor4;valor5') do @echo %a %b %c %d

Processando nomes de arquivos:

    (for /r %i in ("*.txt") do @for /f "tokens=1 delims=_" %a in ("%~ni") do @echo INSERT INTO tmp VALUES ^('%a'^); ) > inserts.sql

Processa todos os arquivos em subdiretórios do caminho atual com extensão *.txt:

    for /r %i in ("*.txt") do ...

Separa o nome do arquivo da extensão ("%~ni") e depois separa o nome do arquivo em tokens usando _ como delimitador:

    @for /f "tokens=1 delims=_" %a in ("%~ni") do ...

Gera um comando de inserção SQL com a string extraída (usar ^ para escape):

    @echo INSERT INTO tmp VALUES ^('%a'^);

Captura toda a saída e grava em arquivo:

    (...) > inserts.sql

OBS: O @ suprime a exibição do comando na saída.



Registro
----------

Para criar um arquivo .reg com a chave a ser importada, iniciar com o cabeçalho conforme exemplo abaixo:

    Windows Registry Editor Version 5.00

    [HKEY_CURRENT_USER\ExemploChave]
    "ExemploVariavel"=dword:00000001

Abrir um arquivo com um programa específico no menu de contexto do explorer:

    [HKEY_CURRENT_USER\Software\Classes\*\shell\Abrir com Notepad++\command]
    @="\"C:\\Local\\Programas\\Notepad++\\notepad++.exe\" \"%1\""


Variáveis de ambiente:

    [HKEY_CURRENT_USER\Environment]
    "SQLPATH"="C:\\Local\\Oracle\\"

Executar ao iniciar:

    [HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run]
    "GoogleChromeAutoLaunch_FC484C2889B2A608BBE0AD98B7223509"="\"C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe\" --no-startup-window"
    "OneDrive"="\"C:\\Users\\felipo_soranz\\AppData\\Local\\Microsoft\\OneDrive\\OneDrive.exe\" /background"

Remover papel de parede:

    [HKEY_CURRENT_USER\Control Panel\Desktop]
    "Wallpaper"=""

Colocar um papel de parede específico:

    [HKEY_CURRENT_USER\Control Panel\Desktop]
    "Wallpaper"="C:\\Users\\FELIPO~1\\AppData\\Local\\Temp\\BGInfo.bmp"

NOTA: Para atualizar o papel de parede imediatamente

    RUNDLL32.EXE USER32.DLL,UpdatePerUserSystemParameters 1, True


Scripting
----------

Exibir um alerta em popup a partir de um .BAT ou do prompt:

    mshta javascript:alert("Line 1\n Line 2");close();

    - ou -

    msg %USERNAME% "Mensagem desejada !!!"


Opções interativas

    choice /c ABCDEFGHIJKLMNOPQRSTUVWXYZ /n /m "Selecione uma letra:"
    choice /c 1234567890 /n /m "Selecione uma número:"

    choice /c SN /n /m "Sim/Não:"
    if %ERRORLEVEL% equ 1 (echo Selecionado SIM)
    if %ERRORLEVEL% equ 2 (echo Selecionado NÃO)

Contador regressivo

    timeout /t -1 /nobreak
    timeout /t 5 /nobreak
    timeout /t -1 
    timeout /t 5


Compilar scripts `JScript.NET`. Exemplo:

~~~js
// hello.js

print("Hello World!");
~~~

Compilar com:

    jsc hello.js


Processos
----------

Iniciar processos

    start cmd /k echo New Window!
    start /wait cmd /k echo New Window!
    start S:my\path\to\folder

Listar processos

    tasklist
    tasklist /fi "memusage gt 100000"
    tasklist /fi "memusage gt 100000" /fo csv

Matar processos

    taskkill
    taskkill /FI "memusage gt 100000"

Kill Chrome:
    
    taskkill /F /IM Chrome.exe

Kill process with pid 3125:

    taskkill -pid 3125


Arquivos
----------

Comparar arquivos ASCII:

    Fc /l File1.txt File2.txt 

Comparar arquivos binários:

    Fc /b Picture1.jpg Picture2.jpg

Associação de arquivos

    assoc 

Cópia "robusta" de arquivos

    robocopy

Cópia (e atualização) completa de um diretório e subpastas (incluso arquivos ocultos):

    xcopy /c /d /y /e /r /i /h /f f:\Temp c:\Temp\

Compactar vários diretórios em arquivos zip separados (usando 7z)

    for /d %a in (*.*) do (
        @"C:\Program Files\7-Zip\7z.exe" a -tzip ..\Backup_savegame\"%a.zip" "%a"\*.*
    )




Drivers 
----------

Listar drivers instalados

    driverquery -v


Obter drivers antigos:

- Buscar por drivers antigos ou descontinuados da Microsoft:
<https://www.catalog.update.microsoft.com/Home.aspx>

- Exemplo para a webcam VX-1000
<https://www.catalog.update.microsoft.com/Search.aspx?q=vx-1000>



Políticas
---------

Verificando políticas ativas para o usuário:

    gpresult /Scope User /v

Para a estação:

    gpresult /Scope Computer /v


WMI
---

É possível fazer consultas ao WMI interativamente com o programa: WBEMTest.exe

Exemplos de consultas:

    Select * From Win32_ComputerSystem
    Select * From Win32_TimeZone
    SELECT * FROM Win32_NTLogEvent WHERE SourceName = 'Microsoft-Windows-Kernel-General' AND EventCode = '12' AND TimeGenerated >= '' AND TimeGenerated <= ''

Também é possível obter algumas informações com o comando WMIC.exe



Comandos diversos
----------

Comando                   | Descrição
--------------------------|------------------------
COLOR                     | Set COLOR of Window
CLS                       | Clear Screen
ECHO                      | Echo
DIR                       | List all directory
attrib                    | Change attribute of file and folder
TIME                      | Change Time on Windows
DATE                      | Change Date on Windows
IPCONFIG                  | Know your IP
NETSTAT                   | Active Connection
PING                      | Ping
SHUTDOWN                  | Shutdown using command
SYSTEMINFO                | Get system information
SFC                       | System File Checker
SCHTASKS                  | Schedule Tasks
DRIVERQUERY               | List all installed driver
TASKLIST                  | List task based on conditon
TASKKILL                  | Kill Process
NSLOOKUP                  | Find IP address of a domain
FC                        | Compare files
ASSOC                     | Find file association for each file extension
TRACERT                   | Trace a request
ROBOCOPY                  | Robust File Copy for Windows
COPY                      | Copy files
XCOPY                     | Copy files and directories
START                     | Open new command window
TIMEOUT                   | Run commands one after another
MKDIR or MD / RMDIR or rd | Create New Folder and Remove Folder
TREE                      | List all files and directory
DISKPART                  | Disk Administration and Partion Disk
CHKDSK                    | Check Errors in Hard Disk
DEL                       | Delete files easily
RENAME                    | Rename file or folder


Equivalência com Unix/Linux
---------------------------

Linux/Unix              |  Windows
------------------------|----------------------------
ls                      |  dir /b
ls -l                   |  dir
ls -x                   |  dir /w
ps                      |  tasklist
cat                     |  type
cut                     |  for /f
grep abc                |  find "abc"
grep abc caminho.ext    |  find "abc" caminho.ext
grep -v naotemstring    |  find /v "naotemstring"
grep -i IgNoRaCaSe      |  find /i "IgNoRaCaSe"
grep -n linha           |  find /n "linha"


    grep [opcoes] "padrao" [arquivo...]
    findstr [opcoes] "padrao" [arquivo...]


findstr|grep   |descrição
-------|-------|----------------------------------------------------
/i     |-i     |ignora maiúscula/minúscula
/l     |-F     |busca padrão como literal
/r     |-G     |busca padrão como expressão regular (padrão no grep)
/v     |-v     |inverte filtro
/g     |-f     |lê padrões do arquivo
/n     |-n     |número de linha
/s     |-r     |recursiva / sub-pastas
/f     |       |lista de arquivos a pesquisar
/b     |       |busca padrão no início da linha (equivale a ^)
/e     |       |busca padrão no fim da linha (equivale a $)


Teclado Multi-idiomas
----------------

Keyboard shortcuts

- ALT-~ (tilde): Cycles between kana and direct input mode
- ALT-SHIFT: Cycles through available languages
- ALT-CAPS_LOCK:  Switches to katakana input mode
- CTRL-CAPS_LOCK: Switches to hiragana input mode
- SHIFT-ALT:  Switches between English mode and Japanese mode.
- Control - ~: Switches between the current input mode and the English mode within the Japanese IME.

TIP: Switching between Hiragana input mode and Direct Input mode through the language bar is tedious.
Instead you can switch by pressing Alt-Tilde (the key below ESC on your keyboard). 

So, if you need to type Japanese, press Alt-Tilde and start typing. 
When you are done press Alt-Tilde again to switch back to English. 
To achieve the same result when in Japanese mode, press Control - ~.


Teclas multi-midia
-----------------

Fonte: https://www.tenforums.com/tutorials/167135-how-customize-disable-app-keys-keyboard-windows-10-a.html

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\AppKey\

Use "ShellExecute" to Set Key to Run Specified App or Command

Key|Description
---|-------------
1	 |Back (Web browser)
2	 |Forward (Web browser)
3	 |Refresh (Web browser)
4	 |Stop (Web browser)
5	 |Search
6	 |Favorites (Web browser)
7	 |Web Home (Web browser)
8	 |Mute Volume
9	 |Volume Down
10 |Volume Up
11 |Next Track (media)
12 |Previous Track (media)
13 |Stop (media)
14 |Pause/Play (media)
15 |Mail
16 |Media Select
17 |This PC or My Computer
18 |Calculator
19 |Bass Down
20 |Bass Boost
21 |Bass Up
22 |Treble Down
23 |Treble Up
24 |Mute Microphone
25 |Volume Down Microphone
26 |Volume Up Microphone
27 |Help
28 |Find
29 |New
30 |Open
31 |Close
32 |Save
33 |Print
34 |Undo
35 |Redo
36 |Copy
37 |Cut
38 |Paste
39 |Reply (mail)
40 |Forward (mail)
41 |Send (mail)
42 |Spell Check
43 |Toggle Dictation on/off
44 |Toggle Microphone on/off
45 |Correction List
46 |Play (media)
47 |Pause (media)
48 |Record (media)
49 |Fast Forward (media)
50 |Rewind (media)
51 |Channel Up (media)
52 |Channel Down (media)
53 |Delete
54 |Flip 3D


Limpar pasta WinSxS
------------------

    Dism.exe /online /Cleanup-Image /StartComponentCleanup /ResetBase
    
