---
title: Powershell
---

## Command history

Current:

`get-history`

Saved:

`%userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt`

## Prompt e perfil

Personalizar o prompt

```ps1
function prompt {
    Write-Host ("[" + $(Get-Location) + "]") -NoNewLine -ForegroundColor 9
    Write-Host (" [" + $(Get-Date) + "]") -ForegroundColor 10
    Write-Host "<" -NoNewLine -ForegroundColor 0 -BackgroundColor 7
    Write-Host " PS " -NoNewLine -ForegroundColor 4 -BackgroundColor 7
    Write-Host ">" -NoNewLine -ForegroundColor 0 -BackgroundColor 7
    return " "
}
```

Prompt padrão

```ps1
function prompt {
    Write-Host ("PS " + $(Get-Location) + ">") -NoNewLine 
    return " "
}
```

Ver localização do script padrão do usuário local

```ps1
echo $PROFILE
```

Editar script e habilitar execução

```ps1
mkdir $(split-path $PROFILE -parent)
notepad $PROFILE
Set-ExecutionPolicy RemoteSigned
```

## Permissão de execução de scripts

Verificar

```ps1
Get-ExecutionPolicy
```

Permitir execução de scripts criados localmente não-assinados ou baixados da internet assinados

```ps1
Set-ExecutionPolicy RemoteSigned
```

## Manipulando arquivos e strings

Lê o conteúdo de um arquivo e transforma em um array para processamento

```ps1
Get-Content caminho\arquivo
```

Lê apenas uma linha de um arquivo

```ps1
Get-Content -TotalCount 1 caminho\arquivo
```

Grava o array de entrada na forma de um arquivo de saída

```ps1
Set-Content caminho\arquivo
```

Executa o bloco de comandos para cada objeto de entrada e devolve o processamento na saída

```ps1
Foreach-Object { bloco de comandos }
```

Representa o objeto atual da entrada, sobre o qual é possível aplicar uma série de processamentos

```ps1
$_
```

Atribuir uma string a uma variável

```ps1
$a = "Um texto qualquer."
```

Retorna verdadeiro se $a terminar com "qualquer."

```ps1
$a -match "qualquer.$"
```

Faz a substituição de um texto

```ps1
$a = $a -replace " texto", "a frase"
```

Concatena 2 strings se $a for tipo string ou soma 2 números se $a for numérico !!!

```ps1
$c = $a + $b
```

Extrai um trecho de uma string a partir da 3a. posição (começa com 0) e os próximos 10 caracteres

```ps1
$sub = $a.substring(2, 10)
```

Exibindo um valor por linha ou vários valores concatenados

```ps1
Echo "ABC"
Echo $string
Echo "Cada um" "em uma" "linha"
Echo ("Todos " + "juntos " + "numa " + "só!")
```

## Substituição de texto

Substituir um texto (substitui "velho" por "novo" em um arquivo novo):

```ps1
Get-Content C:\Caminho\Entrada.txt |
Foreach-Object {$_ -replace "velho", "novo"} |
Set-Content C:\Caminho\Saida.txt
```

Substituir um texto no mesmo arquivo (os parênteses obrigam a carregar o arquivo primeiro)

```ps1
(Get-Content C:\Caminho\Mesmo.txt) |
Foreach-Object {$_ -replace "velho", "novo"} |
Set-Content C:\Caminho\Mesmo.txt

Extrair um trecho da primeira linha de uma lista de arquivos:

```ps1
# A lista de arquivo é formatada aqui como um array
( "G:\Meios\Arquivos\0067\processado\conc_full_op_121109_230833.ret",
  "G:\Meios\Arquivos\0067\processado\conc_full_rt_130125_021503.ret",
  "G:\Meios\Arquivos\0067\processado\conc_full_rt_130130_021704.ret"
) |
Foreach-Object {
  # $_ contém o nome do arquivo, armazena-o
  $nomearq = $_
  # Extrair a primeira linha do arquivo e a substring desejada (começando do 0)
  $chave = (Get-Content -TotalCount 1 $nomearq).substring(2,25)
  # Concatena e exibe formatado o resultado
  Echo ($nomearq + ";" + $chave)
}
```

Verificando quais arquivos de um diretório contém uma determinada string:

```ps1
# método 1 (potencialmente mais lento pois busca no arquivo inteiro)
select-string *.cob -simplematch -pattern HD0063, HD0064, HD0065, HD0071, HD0072, 
  HD19XR, HD19XS, HD19XT, HD19XU, HD19XV, HD19XW, HD19XX, HD19XY, HD19XZ, HD19Y0 |
  sort-object -property filename -unique | 
  select-object filename

# método 2 (deve ser mais rápido pois busca somente na primeira linha do arquivo)
get-childitem *.cob | foreach {
  $arquivo = $_
  if ( get-content $arquivo -totalcount 1 | 
       select-string -simplematch -quiet -pattern HD0063, HD0064, HD0065, HD0071, HD0072,  
          HD19XR, HD19XS, HD19XT, HD19XU, HD19XV, HD19XW, HD19XX, HD19XY, HD19XZ, HD19Y0
     )
  {
     write-output $arquivo.filename
  }
} > ..\lista_2011.txt
# grava resultado em um arquivo
```

## Reflexão


Obtendo as propriedades de um objeto. Ex:

```ps1
$string = "ABC123"
$string | Get-Member -MemberType property
```

Obtendo os métodos de e um objeto e a definição de um método. Ex:

```ps1
$string = "ABC123"
$string | Get-Member -MemberType method
$string | Get-Member substring | Select Definition
```


## Listar fontes TrueType instaladas

- Referência: <https://technet.microsoft.com/en-us/library/ff730944.aspx>

```ps1
[void] [System.Reflection.Assembly]::LoadWithPartialName("System.Drawing")

$objFonts = New-Object System.Drawing.Text.InstalledFontCollection
$objFonts.Families
```

## Substituto do grep

```ps1
get-content somefile.txt | where { $_ -match "expression"}
```

Equivalente de GREP... busca uma linha/padrão em todos os arquivos de uma pasta recursivamente, ignora os erros e grava saída em arquivo .log

```ps1
get-childitem -recurse C:\Projeto\s-mc\Net\smcsi\smcsi\ | foreach {
  get-content $_.fullname | select-string -simplematch -pattern smc_form_select_populate
} 2> out-null > smc_form_select_populate.log
```

## Substituto do sed

```ps1
powershell -c 'set | %{$_ -replace "^","define ENV_"}'

```

## Trocar cores do console

```ps1
[console]::ForegroundColor = "Green"
[console]::BackgroundColor = "black"

```

## Listar links de uma página

```ps1
(wget -Uri "http://www.6502asm.com/examples").Links.Href
```

## Fazer download de um arquivo

```ps1
wget -uri http://www.example.com/examples/example.txt -outfile example.txt
```

## Primeiro caracter da primeira linha de vários arquivos

```ps1
get-childitem C:\CAMINHO\*.txt | foreach {
    $arquivo = $_
    $conteudo = (get-content $arquivo | select -first 1).substring(0, 1)
    echo "$arquivo -> $conteudo"
}
```

## Classifica arquivos pelo cabeçalho

```ps1
get-childitem c:\temp\teste_ps\*.RET | foreach {

    $arquivo = $_
    $linha = (get-content $arquivo | select -first 1 -skip 1)
    $carteira = $linha.substring(20, 2)
    $conta = $linha.substring(23, 12)
    $dest = ""
    
    echo "arquivo  = $arquivo.name"
    echo "linha    = $linha"
    echo "carteira = $carteira"
    echo "conta    = $conta"
    
    if ( $carteira -eq "02" ) {
    
        if ( $conta -eq "123456789101" ) {
            $dest = "//fileserver-matriz/Z:/Sistema123/Arquivo/0505" 
        }
        
        if ( $conta -eq "987654321012" ) {
            $dest = "//fileserver-matriz/P:/Sistema456/Arquivos/Banco/CNAB" 
        }
        
    }
    
    if ( $carteira -eq "16" ) {
    
        if ( $conta -eq "778899112233" ) {
            $dest = "//fileserver-matriz/P:/Sistema789/Arquivo/0505F" 
        }
        
    }

    if ( $dest -ne "") {
        # mkdir $dest
        # copy-item $arquivo $dest
        echo "dest    = $dest"
        echo "arquivo = $arquivo"
    }    
}
```


