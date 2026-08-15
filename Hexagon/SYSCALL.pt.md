<p align="center">
<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/banner.png">
</p>

<div align="center">

![](https://img.shields.io/github/license/hexagonix/Hexagon.svg)
![](https://img.shields.io/github/stars/hexagonix/Hexagon.svg)
![](https://img.shields.io/github/issues/hexagonix/Hexagon.svg)
![](https://img.shields.io/github/issues-closed/hexagonix/Hexagon.svg)
![](https://img.shields.io/github/issues-pr/hexagonix/Hexagon.svg)
![](https://img.shields.io/github/issues-pr-closed/hexagonix/Hexagon.svg)
![](https://img.shields.io/github/downloads/hexagonix/Hexagon/total.svg)
![](https://img.shields.io/github/release/hexagonix/Hexagon.svg)
[![](https://img.shields.io/twitter/follow/hexagonixOS.svg?style=social&label=Follow%20%40HexagonixOS)](https://twitter.com/hexagonixOS)

</div>

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

<div align="justify">

# Chamadas de sistema do Hexagon

O Hexagon fornece uma série de chamadas de sistema bem documentadas para o desenvolvimento de utilitários e aplicativos. As chamadas de sistema podem ser acessadas no ambiente de usuário por meio de uma interrupção de software. Quando um aplicativo ou utilitário deseja solicitar um serviço disponibilizado pelo Hexagon, chama uma chamada de sistema, que interrompe a execução do processo, transfere o controle da execução de volta ao Hexagon, que executa as operações privilegiadas e transfere a execução novamente ao utilitário. Esse mecanismo de transferência de execução do utilitário para o kernel é chamado de [troca de contexto](https://pt.wikipedia.org/wiki/Troca_de_contexto). Uma troca de contexto é essencial para que o utilitário solicite operações privilegiadas ao kernel, como realizar operações de I/O em dispositivos ou iniciar um novo processo, bem como operações de manipulação de arquivos. Vale ressaltar que os utilitários não são autorizados a realizar operações de I/O diretamente em modo usuário. Além disso, toda a lógica para manipulação de arquivos e discos/volumes está no kernel. No Hexagon, as funções disponibilizadas na chamada de sistema podem ser solicitadas pela interrupção de software de número `128` em decimal ou `80h` em hexadecimal, o mesmo vetor tradicionalmente usado por sistemas Unix-like para chamadas de sistema. Notadamente, a notação mais utilizada é a de hexadecimal, para chamadas de sistema em diversos sistemas operacionais. Ao solicitar uma chamada de sistema, você também deve fornecer parâmetros que identifiquem a função escolhida, bem como os parâmetros exigidos para ela. Você pode encontrar os parâmetros exigidos para elas na tabela abaixo.

Um exemplo de como solicitar uma chamada de sistema:

```assembly

    push numero_função

    mov eax, parâmetro1
    mov ebx, parâmetro2
    mov ecx, parâmetro3
    mov edx, parâmetro4
    mov esi, parâmetro5
    mov edi, parâmetro6

    int 80h 

```

Vá até a [tabela de funções](#tabela-de-funções-disponibilzadas-pelo-hexagon) disponibilizadas pelo Hexagon para obter mais informações sobre cada uma delas. Você também pode visualizar um [exemplo de código](#exemplo-de-código) em `Assembly x86`.

O número e parâmetros de uma função na chamada de sistema são sempre mantidas conservados dentro de uma versão do Hexagonix. Desta forma, qualquer aplicativo desenvolvido mirando a versão H2 deverá funcionar dentro de todas as revisões e lançamentos da versão. Uma mudança poderia ocorrer, entretanto, em uma versão futura, como H3. A ABI e API do Hexagonix têm um ciclo de vida que se inspira no ciclo de vida destes no FreeBSD.

</div>

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

<div align="justify">

## Tabela de funções disponibilzadas pelo Hexagon

Agora, uma tabela com as funções da chamada de sistema do Hexagonix, com todas as funções reunidas em uma única tabela e classificadas por categoria através da coluna Grupo.

> Vale lembrar que uma tabela de funções, padronizada segundo as funções disponíveis no Version 7 UNIX, está sendo desenvolvida. Nesse caso, não existe o objetivo de pareamento de número de função junto ao UNIX, mas conformidade no nome das funções. Por exemplo, `alocarMemoria` se tornaria `malloc`, e `retornarVersao`, `uname`. No futuro, ambas as nomenclaturas estarão disponíveis para permitir a migração de aplicativos e utilitários. Venha novamente nesse arquivo mais tarde para checar atualizações.

| Número da função | Nome | Grupo | Entrada | Saída | Descrição |
|:----------------:|:----:|:-------:|:------:|:----:|:---------:|
| 1 | hx.malloc | Gerenciamento de memória e processos | EBX = Tamanho da memória solicitada, em bytes | EAX = 0 em caso de falha, diferente de zero caso contrário; EBX = Ponteiro para a memória alocada | Aloca memória para o processo|
| 2 | hx.free | Gerenciamento de memória e processos | EBX = Ponteiro para a memória alocada; ECX = Tamanho da memória alocada | Sem saída | Libera a memória alocada previamente|
| 3 | hx.exec | Gerenciamento de memória e processos | ESI = Nome do programa; EDI = Argumentos; EAX = 0 se não forem passados argumentos| CF definido em caso de erro ou imagem não encontrada | Carrega e executa imagem presente no volume|
| 4 | hx.exit | Gerenciamento de memória e processos | EAX = Código de erro, caso exista; EBX = 0 se apenas terminar a execução; EBX = 0x1234 para manter residente | Sem saída | FInaliza a execução de um processo |
| 5 | hx.pid | Gerenciamento de memória e processos | Sem entrada | EAX = PID do processo atual | Obtêm o PID do processo em execução |
| 6 | hx.spawn | Gerenciamento de memória e processos | ESI = Nome do arquivo a executar | EAX = PID do novo processo; CF definido em caso de erro (limite de processos atingido, imagem não encontrada ou incompatível, ou memória insuficiente) | Cria um novo processo sem bloquear o processo chamador, que segue em execução imediatamente após o spawn. Ainda não aceita argumentos, ao contrário de hx.exec|
| 7 | hx.kill | Gerenciamento de memória e processos | EBX = PID do processo alvo | CF definido se não existir processo com o PID informado; EAX = 05h | Encerra um processo arbitrário pelo PID, podendo ser chamado por qualquer processo contra qualquer outro (diferente do encerramento do processo em primeiro plano pela tecla especial)|
| 8 | hx.memoryUsage | Gerenciamento de memória e processos | Sem entrada | EAX = Memória utilizada, em bytes; EBX = Memória total disponível para uso, em bytes; ECX = Memória total disponível para uso, em Mbytes (menos preciso); EDX = Memória reservada para o Hexagon, em bytes; ESI = Memória total alocada (resevada+processos), em kbytes| Obter o uso detalhado de memória pelo sistema|
| 9 | hx.getProcesses | Gerenciamento de memória e processos | Sem entrada | ESI = Lista de processos; EAX = Número de processos em execução | Obtêm os processos em execução|
| 10 | hx.getErrorCode | Gerenciamento de memória e processos | Sem entrada | EAX = Código de erro (0 para sem erro)| Obtêm o código retornado pelo último processo em execução|
| 11 | hx.getenv | Gerenciamento de memória e processos | ESI = Nome da variável de ambiente | ESI = Ponteiro para o valor; CF definido se a variável não estiver definida | Lê uma variável de ambiente do processo chamador|
| 12 | hx.setenv | Gerenciamento de memória e processos | ESI = Nome da variável de ambiente; EDI = Valor | CF definido se não houver espaço suficiente no ambiente para a nova entrada | Define ou substitui uma variável de ambiente do processo chamador|
| 13 | hx.unsetenv | Gerenciamento de memória e processos | ESI = Nome da variável de ambiente | CF definido se a variável não estiver definida | Remove uma variável de ambiente do processo chamador|
| 14 | hx.environ | Gerenciamento de memória e processos | Sem entrada | ESI = Ponteiro para o bloco de ambiente bruto, terminado por dois NUL; EAX = Número de variáveis definidas | Obtém todas as variáveis de ambiente do processo chamador|
| 15 | hx.open | Gerenciamento de arquivos e dispositivos | ESI = Ponteiro para o nome do dispositivo, ou para o caminho do arquivo a abrir (uma `/` inicial é absoluto, caso contrário é relativo ao diretório atual); EDI = Endereço de carregamento, em caso de arquivo| CF definido quando o nome do dispositivo for inválido ou arquivo não existir | Abre um canal de leitura/escrita em um dispositivo solicitado ou arquivo comum presente no disco (dispositivos e discos são tratados como arquivos). Em caso de arquivo no disco, um endereço de carregamento deve ser fornecido|
| 16 | hx.write | Gerenciamento de arquivos e dispositivos |  ESI = Ponteiro com o buffer contendo os dados | CF definido em caso de erro ou nenhum dispositivo aberto | Envia dados para o dispositivo aberto|
| 17 | hx.close | Gerenciamento de arquivos e dispositivos | Sem entrada | Sem saída | Fecha o último dispositivo aberto pelo processo atual|
| 18 | hx.create | Gerenciamento de arquivos e dispositivos |  ESI = Ponteiro para o caminho do arquivo a criar (absoluto ou relativo ao diretório atual); EDI = Ponteiro para o conteúdo; EAX = Tamanho do arquivo | CF definido em caso de erro, arquivo já presente, ou uma pasta do caminho não existir (EAX = `IO.pathNotFound`) | Salva um arquivo no volume montado|
| 19 | hx.touch | Gerenciamento de arquivos e dispositivos | ESI = Ponteiro para o caminho do arquivo a criar (absoluto ou relativo ao diretório atual) | CF definido se o arquivo já existir, uma pasta do caminho não existir, ou o nome do arquivo tiver mais de 12 caracteres; EAX = `IO.operationDenied` para um usuário sem privilégio | Cria um novo arquivo vazio no volume montado, diferente do `hx.create` acima, que grava conteúdo em uma única etapa|
| 20 | hx.unlink | Gerenciamento de arquivos e dispositivos | ESI = Ponteiro para o caminho do arquivo a remover | CF definido em caso de erro ou arquivo não existente | Remove um arquivo no volume montado |
| 21 | hx.rename | Gerenciamento de arquivos e dispositivos | ESI = Ponteiro para o caminho do arquivo original; EDI = Ponteiro para o novo nome (apenas seu último componente é usado; renomear para outra pasta não é suportado) | CF definido em caso de erro ou erro na atualização de nome | Atualiza o nome de um arquivo no volume montado |
| 22 | hx.listFiles | Gerenciamento de arquivos e dispositivos | Sem entrada | ESI = Ponteiro para a lista de arquivos; EAX = Total de arquivos | Obtêm lista de arquivos presentes no diretório atual |
| 23 | hx.fileExists | Gerenciamento de arquivos e dispositivos | ESI = Ponteiro para o caminho do arquivo a checar (absoluto ou relativo ao diretório atual) |  EAX = Tamanho do arquivo; CF definido se o arquivo não existir ou uma pasta do caminho não existir | Checar se um arquivo existe no volume |
| 24 | hx.getVolume | Gerenciamento de arquivos e dispositivos | Sem entrada | ESI = Nome do dispositivo; EDI = Rótulo do volume utilizado | Obtêm informações do disco montado em `/`|
| 25 | hx.mkdir | Gerenciamento de arquivos e dispositivos | ESI = Caminho do diretório a criar (absoluto ou relativo ao diretório atual) | CF definido em caso de erro; EAX = `IO.operationDenied` para um usuário sem privilégio, `IO.pathNotFound` se uma pasta do caminho não existir | Cria um novo diretório vazio no volume montado|
| 26 | hx.rmdir | Gerenciamento de arquivos e dispositivos | ESI = Caminho do diretório a remover (absoluto ou relativo ao diretório atual) | CF definido em caso de erro; EAX = `IO.operationDenied` para um usuário sem privilégio, `IO.directoryNotEmpty` se o diretório ainda tiver entradas além de `.` e `..` | Remove um diretório vazio do volume montado|
| 27 | hx.changeDirectory | Gerenciamento de arquivos e dispositivos | ESI = Caminho do diretório para o qual mudar (absoluto ou relativo, pode ter vários componentes separados por `/`) | CF definido se o caminho for vazio ou algum componente não for encontrado ou for inválido. Em caso de erro, o diretório atual permanece inalterado | Altera o diretório de trabalho atual|
| 28 | hx.lock | Gerenciamento de usuário e permissões | Sem entrada | Sem saída | Bloqueia o sinal de término de processo em primeiro plano por tecla especial|
| 29 | hx.unlock | Gerenciamento de usuário e permissões | Sem entrada | Sem saída | Habilita o sinal de término de processos em primeiro plano por uso de tecla especial|
| 30 | hx.setUser | Gerenciamento de usuário e permissões | EAX = ID do usuário | Sem saída | Registra o id do usuário agora logado, fornecido pelo gerenciador de login. O kernel não armazena nem resolve nomes de usuário; esse mapeamento vive inteiramente no userland, através do banco `/etc/shadow`|
| 31 | hx.getUser | Gerenciamento de usuário e permissões | Sem entrada | EAX = ID do usuário | Obtêm o id do usuário logado na sessão atual|
| 32 | hx.uname | Serviços do Hexagon | Sem entrada | EAX = Número da versão; EBX = Número da subversão; ECX = Número de revisão; EDX = Arquitetura; ESI = Nome do kernel; EDI = Data/hora de build do kernel| Retorna a versão do Hexagon para os aplicativos|
| 33 | hx.getRandom | Serviços do Hexagon | EAX = Máximo | EAX = Número | Obtêm um número aleatório|
| 34 | hx.feedRandom | Serviços do Hexagon | EAX - Número para criar entropia | Sem saída | Alimenta com entropia o gerador de números aleatórios do kernel|
| 35 | hx.sleep | Serviços do Hexagon | ECX = Tempo em unidades de contagem para causar atraso | Sem saída | Causa um atraso (delay) em operações |
| 36 | hx.installISR | Serviços do Hexagon | EAX = Número da interrupção; ESI = Ponteiro para o manipulador | Sem saída | Instala rotina de serviço de interrupção|
| 37 | hx.restart | Gerenciamento de energia | Sem entrada | Sem saída | Solicita o reinicio do dispositivo|
| 38 | hx.shutdown | Gerenciamento de energia | Sem entrada | Sem saída | Solicita o desligamento do dispositivo|
| 39 | hx.print | Serviços de vídeo e gráficos | EAX = Conteúdo numérico, se este for o caso, respeitando os formatos designados. Os formatos devem ser informados; ESI = Ponteiro para a string à ser impressa, se este for o caso; EBX = Tipo de entrada (01h - inteiro decimal; 02h - inteiro hexadecimal; 03h - inteiro binário; 04h - string)| Sem saída | Envia um conteúdo definido para um dispositivo de saída|
| 40 | hx.clearConsole | Serviços de vídeo e gráficos | Sem entrada | Sem saída | Limpa o console atual|
| 41 | hx.clearLine | Serviços de vídeo e gráficos | AL = Número da linha  | Sem saída | Limpa uma linha específica no console|
| 42 | hx.scrollConsole | Serviços de vídeo e gráficos | Sem entrada | Sem saída | Rola o console para baixo uma linha|
| 43 | hx.setCursor | Serviços de vídeo e gráficos | DL = posição no eixo X; DH = posição no eixo Y | Sem saída | Define o cursor em uma posição específica|
| 44 | hx.drawCharacter | Serviços de vídeo e gráficos |  EAX = posição no eixo X; EBX = posição no eixo Y; EDX = Cor em hexadecimal | Sem saída | Coloca um pixel no console|
| 45 | hx.drawBlock | Serviços de vídeo e gráficos | EAX = posição no eixo X; EBX = posição no eixo Y; ESI = Comprimento; EDI = Altura; EDX = Cor em hexadecimal | Sem saída | Desenha um bloco de cor específica|
| 46 | hx.printCharacter | Serviços de vídeo e gráficos | AL = Caractere; EBX = 01h para reposicionar cursor | Sem saída | Imprimir caractere no console na posição do cursor|
| 47 | hx.setColor | Serviços de vídeo e gráficos | EAX = Cor da fonte (RGB em hexadecimal); EBX = Cor do plano de fundo (RGB em hexadecimal); ECX = 1234h para alterar o tema padrão para os valores solicitados; Em modo texto, apenas preto e branco são permitidos | Sem saída | Define cor de fundo e primeiro plano|
| 48 | hx.getColor | Serviços de vídeo e gráficos | Sem entrada | EAX = Cor da fonte (RGB em hexadecimal); EBX = Cor do plano de fundo (RGB em hexadecimal); ECX = 1234h para alterar o tema padrão para os valores solicitados; Em modo texto, apenas preto e branco são permitidos | Obtêm cor de fundo e primeiro plano|
| 49 | hx.getConsoleInfo | Serviços de vídeo e gráficos | Sem entrada | EAX = Resolução X (bits 0..15), Y (bits 16..31); EBX = Colunas (bit 0..7), Linhas (8..15), Bits por pixel (16..23); EDX = Endereço do início do frame de vídeo; CF definido em caso de modo texto | Obtêm informações do console atual|
| 50 | hx.updateScreen | Serviços de vídeo e gráficos | Sem entrada | Sem saída | Atualiza o console primário com conteúdo do primeiro console virtual|
| 51 | hx.setResolution | Serviços de vídeo e gráficos | EAX = Número relativo a resolução à ser utilizada (1 = Resolução de 800x600 pixels; 2 - Resolução de 1024x768 pixels; 3 - Alterar para modo texto)| Sem saída | Define a resolução do console principal|
| 52 | hx.getResolution | Serviços de vídeo e gráficos | Sem entrada | EAX = Número relativo a resolução à ser utilizada (1 = Resolução de 800x600 pixels; 2 - Resolução de 1024x768 pixels) | Ontêm a resolução utilizadapelo console principal|
| 53 | hx.getCursor | Serviços de vídeo e gráficos | Sem entrada | DL = Eixo X; DH = Eixo Y | Obtêm a posição do cursor|
| 54 | hx.waitKeyboard | Serviços de dispositivos PS/2 | Sem entrada | AL = Caractere; AH - Scancode | Espera o pressionamento de uma tecla no teclado|
| 55 | hx.getString | Serviços de dispositivos PS/2 | AL = Máximo de caracteres para obter | EBX = Presença ou não de eco durante a digitação (1234h para sem eco e qualquer valor para ativar); ESI = String | Obtêm uma string do teclado|
| 56 | hx.getKeyState | Serviços de dispositivos PS/2 | Sem entrada | EAX = Status das teclas especiais (bit 0: Tecla Control; bit 1: Tecla Shift; bit 2-31: Reservado) | Obtêm o estado das teclas especiais, como Control e Shift|
| 57 | hx.changeConsoleFont | Serviços de vídeo e gráficos | ESI = Ponteiro para o buffer contendo o nome do arquivo que contêm a fonte compatível com o Hexagonix | CF definido em caso de arquivo não encontrado ou incompatível | Altera a fonte padrão de exibição do sistema|
| 58 | hx.changeLayout | Serviços de dispositivos PS/2 | ESI = Arquivo contendo um leiaute de teclado válido | CF definido em caso de arquivo não encontrado ou incompatível | Altera o leiaute do teclado|
| 59 | hx.waitMouse | Serviços de dispositivos PS/2 | Sem entrada |  EAX = Posição no eixo X; EBX = Posição no eixo Y; EDX = Botões | Aguarda por evento do mouse|
| 60 | hx.getMouse | Serviços de dispositivos PS/2 | Sem entrada | EAX = Posição no eixo X; EBX = Posição no eixo Y; EDX = Botões | Obtêm posição atual do mouse e estado dos botões|
| 61 | hx.setMouse | Serviços de dispositivos PS/2 | EAX = Posição no eixo X; EBX = Posição no eixo Y | Sem saída | Define nova posição do mouse|
| 62 | hx.compareWordsString | Serviços de manipulação e conversão de dados | ESI = Primeira string; EDI = Segunda string | CF definido se iguais | Compara primeiras words de duas strings|
| 63 | hx.removeCharacterString | Serviços de manipulação e conversão de dados | ESI = String; EAX = Posição do caractere | Sem saída | Remove um caractere em uma posição específica de uma string|
| 64 | hx.insertCharacter | Serviços de manipulação e conversão de dados |  ESI = String; EDX = Posição; AL = Caractere para inserir | Sem saída | Insere um caractere em posição específica da string|
| 65 | hx.stringSize | Serviços de manipulação e conversão de dados | ESI = String | EAX = Tamanho da string | Obtêm o tamanho de uma string|
| 66 | hx.compareString | Serviços de manipulação e conversão de dados | ESI = Primeira string; EDI = Segunda string | CF definido se as duas forem iguais | Compara de todos os caracteres de uma string são iguais|
| 67 | hx.stringToUppercase | Serviços de manipulação e conversão de dados | ESI = String | String convertida | Converte uma string para caracteres maiúsculos|
| 68 | hx.stringToLowercase | Serviços de manipulação e conversão de dados | ESI = String | String convertida | Converte uma string para caracteres minúsculos|
| 69 | hx.trimString | Serviços de manipulação e conversão de dados | ESI = String | String cortada | Remove espaços em branco da string|
| 70 | hx.findCharacter | Serviços de manipulação e conversão de dados | ESI = String, AL = Caractere para encontrar | EAX = Número de ocorrências do caractere; CF definido se caractere não encontrado | Encontra caractere específico na string|
| 71 | hx.stringToInt | Serviços de manipulação e conversão de dados | ESI = String | EAX = Inteiro; CF definido em caso de número inválido | Converte um número string para número inteiro|
| 72 | hx.toString | Serviços de manipulação e conversão de dados | EAX = Inteiro à ser convertido | ESI = Ponteiro para o buffer contendo a string | Converte um número inteiro em uma string|
| 73 | hx.emitSound | Serviços de saída de som | AX = Frequência a ser reproduzida | Sem saída | Toca um tom no alto-falante interno do computador|
| 74 | hx.turnOffSound | Serviços de saída de som| Sem entrada | Sem saída | Desliga o alto-falante interno do computador, interrompendo qualquer emissão de som em progresso|
| 75 | hx.sendMessageHexagon | Serviço de mensagens | ESI = Mensagem; EAX = Código de erro, se houver; EBX = Prioridade | Sem saída | Envia uma mensagem de alta prioridade do Hexagon|
| 76 | hx.date | Serviços de relógio em tempo real | EAX = Dia, em ASCII;  EBX = Mês, em ASCII; ECX = Século, em ASCII; EDX = Ano, em ASCII  | Sem saída | Retorna informações de relógio em tempo real em formato ASCII (String). Conversão para número pode ser necessária|
| 77 | hx.time | Serviços de relógio em tempo real | EAX = Hora, em ASCII; EBX = Minuto, em ASCII; ECX = Segundo, em ASCII | Sem saída | Retorna informações de relógio em tempo real em formato ASCII (String). Conversão para número pode ser necessária|

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Exemplo de código

Abaixo, temos um exemplo de aplicativo em modo texto que utiliza uma série de funções disponibilizadas pelo Hexagon. Você verá o cabeçalho HAPP montado pela macro `appHeader` da [libasm](https://github.com/hexagonix/lib/blob/main/fasm/HAPP.s), a inclusão das chamadas de sistema e macros de console, a limpeza do console, a impressão de uma mensagem, a espera por uma tecla e o encerramento do processo. Esses macros podem ser encontrados e estudados diretamente no repositório da [libasm](https://github.com/hexagonix/lib/blob/main/fasm/hexagon.s). O exemplo abaixo foi escrito em `Assembly x86` com sintaxe Intel, visando o montador fasm (flat assembler).

```assembly
format binary as "app" ;; Especifica o formato e a extensao do arquivo

use32

include "HAPP.s" ;; Struc que monta o cabecalho HAPP

;; Instancia | Estrutura | Arquitetura | Versao | Subversao | Ponto de entrada | Tipo de imagem
appHeader headerHAPP HAPP.Architectures.i386, 1, 6, applicationStart, 01h

;;*************************************************************

include "hexagon.s" ;; Macro hx.syscall e as constantes hx.*
include "console.s" ;; Macros de saida no console (fputs, puts...)
include "macros.s"  ;; finishProcess e outras macros de proposito geral

;;*************************************************************

;; Variaveis e constantes

mensagem: db 10, 10, "Este e um exemplo de aplicativo HAPP para o Hexagonix!", 10, 0

;;*************************************************************

applicationStart:

;; Limpa o console atual e imprime a mensagem de saudacao

    hx.syscall hx.clearConsole

    puts mensagem ;; fputs, se voce nao quiser a quebra de linha ao final

;; Aguarda o pressionamento de uma tecla antes de encerrar

    hx.syscall hx.waitKeyboard

;; Encerra o processo (codigo de erro 0, sem manter residente)

    finishProcess 0, 0
```

Além deste exemplo, você pode estudar aplicativos reais que utilizam a chamada de sistema do Hexagon diretamente na árvore de código-fonte do Hexagonix, no repositório [Unix-Apps](https://github.com/hexagonix/Unix-Apps). Os utilitários `testa` e `testb`, em particular, são pequenos aplicativos de diagnóstico (não fazem parte da distribuição) escritos para exercitar `hx.spawn`: `testa` solicita a criação não bloqueante de `testb` e registra, através de `log.s`, que continua em execução imediatamente após o spawn, provando que a chamada não bloqueia o processo chamador.

</div>