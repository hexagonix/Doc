<p align="center">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/banner.png">
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

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

<div align="justify">

# Chamadas de sistema do Hexagon

O Hexagon fornece uma série de chamadas de sistema bem documentadas para o desenvolvimento de utilitários e aplicativos. As chamadas de sistema podem ser acessadas no ambiente de usuário por meio de uma interrupção de software. Quando um aplicativo ou utilitário deseja solicitar um serviço disponibilizado pelo Hexagon, chama uma chamada de sistema, que interrompe a execução do processo, transfere o controle da execução de volta ao Hexagon, que executa as operações privilegiadas e transfere a execução novamente ao utilitário. Esse mecanismo de transferência de execução do utilitário para o kernel é chamado de [troca de contexto](https://pt.wikipedia.org/wiki/Troca_de_contexto). Uma troca de contexto é essencial para que o utilitário solicite operações privilegiadas ao kernel, como realizar operações de I/O em dispositivos ou iniciar um novo processo, bem como operações de manipulação de arquivos. Vale ressaltar que os utilitários não são autorizados a realizar operações de I/O diretamente em modo usuário. Além disso, toda a lógica para manipulação de arquivos e discos/volumes está no kernel. No Hexagon, as funções disponibilizadas na chamada de sistema podem ser solicitadas pela interrupção de software de número `105` em decimal ou `69h` em hexadecimal. Notadamente, a notação mais utilizada é a de hexadecimal, para chamadas de sistema em diversos sistemas operacionais. Ao solicitar uma chamada de sistema, você também deve fornecer parâmetros que identifiquem a função escolhida, bem como os parâmetros exigidos para ela. Você pode encontrar os parâmetros exigidos para elas na tabela abaixo.

Um exemplo de como solicitar uma chamada de sistema:

```assembly

    push numero_função

    mov eax, parâmetro1
    mov ebx, parâmetro2
    mov ecx, parâmetro3
    mov edx, parâmetro4
    mov esi, parâmetro5
    mov edi, parâmetro6

    int 69h 

```

Vá até a [tabela de funções](#tabela-de-funções-disponibilzadas-pelo-hexagon) disponibilizadas pelo Hexagon para obter mais informações sobre cada uma delas. Você também pode visualizar um [exemplo de código](#exemplo-de-código) em `Assembly x86`.

O número e parâmetros de uma função na chamada de sistema são sempre mantidas conservados dentro de uma versão do Hexagonix. Desta forma, qualquer aplicativo desenvolvido mirando a versão H2 deverá funcionar dentro de todas as revisões e lançamentos da versão. Uma mudança poderia ocorrer, entretanto, em uma versão futura, como H3. A ABI e API do Hexagonix têm um ciclo de vida que se inspira no ciclo de vida destes no FreeBSD.

</div>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

<div align="justify">

## Tabela de funções disponibilzadas pelo Hexagon

Agora, uma tabela com as funções da chamada de sistema do Hexagonix. As funções estão classificadas em categorias. As funções também foram classificadas em `Unix-like`, quando existem versões análogas no UNIX ou em sistema semelhantes, e `Hexagonix`, quando a função implementada não tem uma versão semelhante em outros sistemas UNIX ou Unix-like.

> Vale lembrar que uma tabela de funções, padronizada segundo as funções disponíveis no Version 7 UNIX, está sendo desenvolvida. Nesse caso, não existe o objetivo de pareamento de número de função junto ao UNIX, mas conformidade no nome das funções. Por exemplo, `alocarMemoria` se tornaria `free`, e `retornarVersao`, `uname`. No futuro, ambas as nomenclaturas estarão disponíveis para permitir a migração de aplicativos e utilitários. Venha novamente nesse arquivo mais tarde para checar atualizações.

### Funções de gerenciamento de memória e processos

| Número da função | Nome | Grupo | Entrada | Saída | Família da função| Descrição |
|:----------------:|:----:|:-------:|:------:|:----:|:----------------:|:---------:|
| 1 | alocarMemoria | Gerenciamento de memória e processos | EAX = Tamanho da memória solicitada, em bytes | EBX = Ponteiro para a memória alocada | Unix-like| Aloca memória para o processo|
| 2 | liberarMemoria | Gerenciamento de memória e processos | EBX = Ponteiro para a memória alocada; ECX = Tamanho da memória alocada | Sem saída | Unix-like | Libera a memória alocada previamente|
| 3 | iniciarProcesso | Gerenciamento de memória e processos | ESI = Nome do programa; EDI = Argumentos; EAX = 0 se não forem passados argumentos| CF definido em caso de erro ou imagem não encontrada | Unix-like | Carrega e executa imagem presente no volume|  
| 4 | encerrarProcesso | Gerenciamento de memória e processos | EAX = Código de erro, caso exista; EBX = 0 se apenas terminar a execução; EBX = 0x1234 para manter residente | Sem saída | Unix-like | FInaliza a execução de um processo |
| 5 | obterPID | Gerenciamento de memória e processos | Sem entrada | EAX = PID do processo atual | Unix-like | Obtêm o PID do processo em execução |
| 6 | usoMemoria | Gerenciamento de memória e processos | Sem entrada | EAX = Memória utilizada, em bytes; EBX = Memória total disponível para uso, em bytes; ECX = Memória total disponível para uso, em Mbytes (menos preciso); EDX = Memória reservada para o Hexagon®, em bytes; ESI = Memória total alocada (resevada+processos), em kbytes| Unix-like | Obter o uso detalhado de memória pelo sistema|
| 7 | obterProcessos | Gerenciamento de memória e processos | Sem entrada | ESI = Lista de processos; EAX = Número de processos em execução | Unix-like | Obtêm os processos em execução|
| 8 | obterCodigoErro | Gerenciamento de memória e processos | Sem entrada | EAX = Código de erro (0 para sem erro)| Hexagonix | Obtêm o código retornado pelo último processo em execução|

### Gerenciamento de arquivos e dispositivos

| Número da função | Nome | Grupo | Entrada | Saída | Família da função| Descrição |
|:----------------:|:----:|:-------:|:------:|:----:|:----------------:|:---------:|
| 9 | abrir | Gerenciamento de arquivos e dispositivos | ESI = Ponteiro para o buffer que contêm o nome convencionado; EDI = Endereço de carregamento, em caso de arquivo| CF definido quando o nome do dispositivo for inválido ou arquivo não existir | Unix-like | Abre um canal de leitura/escrita em um dispositivo solicitado ou arquivo comum presente no disco (dispositivos e discos são tratados como arquivos). Em caso de arquivo no disco, um endereço de carregamento deve ser fornecido|
| 10 | escrever | Gerenciamento de arquivos e dispositivos |  ESI = Ponteiro com o buffer contendo os dados | CF definido em caso de erro ou nenhum dispositivo aberto | Unix-like | Envia dados para o dispositivo aberto|
| 11 | fechar | Gerenciamento de arquivos e dispositivos | Sem entrada | Sem saída | Unix-like | Fecha o último dispositivo aberto pelo processo atual|
| 13 | salvarArquivo | Gerenciamento de arquivos e dispositivos |  ESI = Ponteiro para o nome do arquivo; EDI = Ponteiro para o conteúdo; EAX = Tamanho do arquivo | CF definido em caso de erro ou arquivo já presente | Unix-like | Salva um arquivo no volume montado|
| 14 | deletarArquivo | Gerenciamento de arquivos e dispositivos | ESI = Ponteiro para o nome do arquivo | CF definido em caso de erro ou arquivo não existente | Unix-like | Remove um arquivo no volume montado |
| 15 | listarArquivos | Gerenciamento de arquivos e dispositivos | Sem entrada | ESI = Ponteiro para a lista de arquivos; EAX = Total de arquivos | Unix-like | Obtêm lista de arquivos presentes no volume |
| 16 | arquivoExiste | Gerenciamento de arquivos e dispositivos | ESI = Nome do arquivo para checar |  EAX = Tamanho do arquivo; CF definido se o arquivo não existir | Hexagonix | Checar se um arquivo existe no volume |
| 17 | obterDisco | Gerenciamento de arquivos e dispositivos | Sem entrada | ESI = Nome do dispositivo; EDI = Rótulo do volume utilizado | Hexagonix | Obtêm informações do disco montado em `/`|

### Funções de gerenciamento de usuário e permissões

| Número da função | Nome | Grupo | Entrada | Saída | Família da função| Descrição |
|:----------------:|:----:|:-------:|:------:|:----:|:----------------:|:---------:|
| 18 | travar | Gerenciamento de usuário e permissões | Sem entrada | Sem saída | Unix-like | Bloqueia o sinal de término de processo em primeiro plano por tecla especial|
| 19 | destravar | Gerenciamento de usuário e permissões | Sem entrada | Sem saída | Unix-like | Habilita o sinal de término de processos em primeiro plano por uso de tecla especial|
| 20 | definirUsuario | Gerenciamento de usuário e permissões | EAX = ID do grupo; ESI = Nome do usuário | Sem saída | Hexagonix | Define um usuário para a sessão atual|
| 21 | obterUsuario | Gerenciamento de usuário e permissões | Sem entrada | EAX = ID do grupo; ESI = Nome do usuário| Hexagonix | Obtêm dados do usuário logado para a sessão atual|

### Serviços do Hexagon

| Número da função | Nome | Grupo | Entrada | Saída | Família da função| Descrição |
|:----------------:|:----:|:-------:|:------:|:----:|:----------------:|:---------:|
| 22 | retornarVersao | Serviços do Hexagon | Sem entrada | EAX = Número da versão; EBX = Número da subversão; CH = Caractere de revisão; EDX = Arquitetura; ESI = Nome do Kernel; EDI = Build do Kernel| Unix-like | Retorna a versão do Hexagon para os aplicativos|
| 23 | obterAleatorio | Serviços do Hexagon | EAX = Máximo | EAX = Número | Hexagonix | Obtêm um número aleatório|
| 24 | alimentarAleatorio | Serviços do Hexagon | EAX - Número para criar entropia | Sem saída | Hexagonix | Alimenta com entropia o gerador de números aleatórios do kernel|
| 25 | causarAtraso | Serviços do Hexagon | ECX = Tempo em unidades de contagem para causar atraso | Sem saída | Hexagonix | Causa um atraso (delay) em operações |
| 26 | instalarISR | Serviços do Hexagon | EAX = Número da interrupção; ESI = Ponteiro para o manipulador | Sem saída | Hexagonix | Instala rotina de serviço de interrupção|

### Serviços de gerenciamento de energia

| Número da função | Nome | Grupo | Entrada | Saída | Família da função| Descrição |
|:----------------:|:----:|:-------:|:------:|:----:|:----------------:|:---------:|
| 27 | reiniciarPC | Gerenciamento de energia | Sem entrada | Sem saída | Unix-like | Solicita o reinicio do dispositivo|
| 28 | desligarPC | Gerenciamento de energia | Sem entrada | Sem saída | Unix-like | Solicita o desligamento do dispositivo|

### Serviços de vídeos e gráficos

| Número da função | Nome | Grupo | Entrada | Saída | Família da função| Descrição |
|:----------------:|:----:|:-------:|:------:|:----:|:----------------:|:---------:|
| 29 | imprimir | Serviços de vídeos e gráficos | EAX = Conteúdo numérico, se este for o caso, respeitando os formatos designados. Os formatos devem ser informados; ESI = Ponteiro para a string à ser impressa, se este for o caso; EBX = Tipo de entrada (01h - inteiro decimal; 02h - inteiro hexadecimal; 03h - inteiro binário; 04h - string)| Sem saída | Hexagonix | Envia um conteúdo definido para um dispositivo de saída
| 30 | limparTela | Serviços de vídeos e gráficos | Sem entrada | Sem saída | Hexagonix | Limpa o console atual|
| 31 | limparLinha | Serviços de vídeos e gráficos | AL = Número da linha  | Sem saída | Hexagonix | Limpa uma linha específica no console|
| 33 | rolarTela | Serviços de vídeos e gráficos | Sem entrada | Sem saída | Hexagonix | Rola o console para baixo uma linha|
| 34 | definirCursor | Serviços de vídeo e gráficos | DL = posição no eixo X; DH = posição no eixo Y | Sem saída | Hexagonix | Define o cursor em uma posição específica|
| 35 | desenharCaractere | Serviços de vídeo e gráficos |  EAX = posição no eixo X; EBX = posição no eixo Y; EDX = Cor em hexadecimal | Sem saída | Hexagonix | Coloca um pixel no console| 
| 36 | desenharBloco | Serviços de vídeo e gráficos | EAX = posição no eixo X; EBX = posição no eixo Y; ESI = Comprimento; EDI = Altura; EDX = Cor em hexadecimal | Sem saída | Hexagonix | Desenha um bloco de cor específica|
| 37 | imprimirCaractere | Serviços de vídeo e gráficos | AL = Caractere; EBX = 01h para reposicionar cursor | Sem saída | Hexagonix | Imprimir caractere no console na posição do cursor| 
| 38 | definirCor | Serviços de vídeo e gráficos | EAX = Cor da fonte (RGB em hexadecimal); EBX = Cor do plano de fundo (RGB em hexadecimal); ECX = 1234h para alterar o tema padrão para os valores solicitados; Em modo texto, apenas preto e branco são permitidos | Sem saída | Hexagonix | Define cor de fundo e primeiro plano|
| 39 | obterCor | Serviços de vídeo e gráficos | Sem entrada | EAX = Cor da fonte (RGB em hexadecimal); EBX = Cor do plano de fundo (RGB em hexadecimal); ECX = 1234h para alterar o tema padrão para os valores solicitados; Em modo texto, apenas preto e branco são permitidos | Hexagonix | Obtêm cor de fundo e primeiro plano|
| 40 | obterInfoTela | Serviços de vídeo e gráficos | Sem entrada | EAX = Resolução X (bits 0..15), Y (bits 16..31); EBX = Colunas (bit 0..7), Linhas (8..15), Bits por pixel (16..23); EDX = Endereço do início do frame de vídeo; CF definido em caso de modo texto | Hexagonix | Obtêm informações do console atual|
| 41 | atualizarTela | Serviços de vídeo e gráficos | Sem entrada | Sem saída | Hexagonix | Atualiza o console primário com conteúdo do primeiro console virtual|
| 42 | definirResolucao | Serviços de vídeo e gráficos | EAX = Número relativo a resolução à ser utilizada (1 = Resolução de 800x600 pixels; 2 - Resolução de 1024x768 pixels; 3 - Alterar para modo texto)| Sem saída | Hexagonix | Define a resolução do console principal|
| 43 | obterResolucao | Serviços de vídeo e gráficos | Sem entrada | EAX = Número relativo a resolução à ser utilizada (1 = Resolução de 800x600 pixels; 2 - Resolução de 1024x768 pixels) | Hexagonix | Ontêm a resolução utilizadapelo console principal|
| 44 | obterCursor | Serviços de vídeo e gráficos | Sem entrada | DL = Eixo X; DH = Eixo Y | Hexagonix | Obtêm a posição do cursor|

### Serviços de manipulação de teclado PS/2

| Número da função | Nome | Grupo | Entrada | Saída | Família da função| Descrição |
|:----------------:|:----:|:-------:|:------:|:----:|:----------------:|:---------:|
| 45 | aguardarTeclado | Serviços de manipulação de teclado PS/2 | Sem entrada | AL = Caractere; AH - Scancode | Hexagonix | Espera o pressionamento de uma tecla no teclado|
| 46 | obterString | Serviços de manipulação de teclado PS/2 | AL = Máximo de caracteres para obter | EBX = Presença ou não de eco durante a digitação (1234h para sem eco e qualquer valor para ativar); ESI = String | Hexagonix | Obtêm uma string do teclado|
| 47 | obterEstadoTeclas | Serviços de manipulação de teclado PS/2 | Sem entrada | EAX = Status das teclas especiais (bit 0: Tecla Control; bit 1: Tecla Shift; bit 2-31: Reservado) | Hexagonix | Obtêm o estado das teclas especiais, como Control e Shift|
| 48 | alterarFonte | Serviços de manipulação de teclado PS/2 | ESI = Ponteiro para o buffer contendo o nome do arquivo que contêm a fonte compatível com o Hexagonix | CF definido em caso de arquivo não encontrado ou incompatível | Hexagonix | Altera a fonte padrão de exibição do sistema|
| 49 | alterarLeiaute | Serviços de manipulação de teclado PS/2 | ESI = Arquivo contendo um leiaute de teclado válido | CF definido em caso de arquivo não encontrado ou incompatível | Hexagonix | Altera o leiaute do teclado|

### Serviços de manipulação de mouse PS/2

| Número da função | Nome | Grupo | Entrada | Saída | Família da função| Descrição |
|:----------------:|:----:|:-------:|:------:|:----:|:----------------:|:---------:|
| 50 | aguardarMouse | Serviços de manipulação de mouse PS/2 | Sem entrada |  EAX = Posição no eixo X; EBX = Posição no eixo Y; EDX = Botões | Hexagonix | Aguarda por evento do mouse|
| 51 | obterMouse | Serviços de manipulação de mouse PS/2 | Sem entrada | EAX = Posição no eixo X; EBX = Posição no eixo Y; EDX = Botões | Hexagonix | Obtêm posição atual do mouse e estado dos botões|
| 52 | definirMouse | Serviços de manipulação de mouse PS/2 | EAX = Posição no eixo X; EBX = Posição no eixo Y | Sem saída | Hexagonix | Define nova posição do mouse|

## Serviços de manipulação e conversão de dados

| Número da função | Nome | Grupo | Entrada | Saída | Família da função| Descrição |
|:----------------:|:----:|:-------:|:------:|:----:|:----------------:|:---------:|
| 53 | compararPalavrasString | Serviços de manipulação e conversão de dados | ESI = Primeira string; EDI = Segunda string | CF definido se iguais | Hexagonix | Compara primeiras words de duas strings|
| 54 | removerCaractereString | Serviços de manipulação e conversão de dados | ESI = String; EAX = Posição do caractere | Sem saída | Hexagonix | Remove um caractere em uma posição específica de uma string|
| 55 | inserirCaractere | Serviços de manipulação e conversão de dados |  ESI = String; EDX = Posição; AL = Caractere para inserir | Sem saída | Hexagonix |  Insere um caractere em posição específica da string|
| 56 | tamanhoString | Serviços de manipulação e conversão de dados | ESI = String | EAX = Tamanho da string | Hexagonix | Obtêm o tamanho de uma string|
| 57 | compararString | Serviços de manipulação e conversão de dados | ESI = Primeira string; EDI = Segunda string | CF definido se as duas forem iguais | Hexagonix | Compara de todos os caracteres de uma string são iguais|
| 58 | stringParaMaiusculo | Serviços de manipulação e conversão de dados | ESI = String | String convertida | Hexagonix | Converte uma string para caracteres maiúsculos|
| 59 | stringParaMinusculo | Serviços de manipulação e conversão de dados | ESI = String | String convertida | Hexagonix | Converte uma string para caracteres minúsculos|
| 60 | cortarString | Serviços de manipulação e conversão de dados | ESI = String | String cortada | Hexagonix |  Remove espaços em branco da string|
| 61 | encontrarCaractere | Serviços de manipulação e conversão de dados | ESI = String, AL = Caractere para encontrar | EAX = Número de ocorrências do caractere; CF definido se caractere não encontrado | Hexagonix | Encontra caractere específico na string|
| 62 | stringParaInt | Serviços de manipulação e conversão de dados | ESI = String | EAX = Inteiro; CF definido em caso de número inválido | Hexagonix | Converte um número string para número inteiro|
| 63 | paraString | Serviços de manipulação e conversão de dados | EAX = Inteiro à ser convertido | ESI = Ponteiro para o buffer contendo a string | Hexagonix | Converte um número inteiro em uma string|

### Serviços de saída de som

| Número da função | Nome | Grupo | Entrada | Saída | Família da função| Descrição |
|:----------------:|:----:|:-------:|:------:|:----:|:----------------:|:---------:|
| 64 | emitirSom | Serviços de saída de som | AX = Frequência a ser reproduzida | Sem saída | Hexagonix | Toca um tom no alto-falante interno do computador|
| 65 | desligarSom | Serviços de saída de som| Sem entrada | Sem saída | Hexagonix |  Desliga o alto-falante interno do computador, interrompendo qualquer emissão de som em progresso|

### Serviço de mensagens

| Número da função | Nome | Grupo | Entrada | Saída | Família da função| Descrição |
|:----------------:|:----:|:-------:|:------:|:----:|:----------------:|:---------:|
| 66 | enviarMensagemHexagon | Serviço de mensagens | ESI = Mensagem; EAX = Código de erro, se houver; EBX = Prioridade | Sem saída | Hexagonix | Envia uma mensagem de alta prioridade do Hexagon|

### Serviços de relógio em tempo real

| Número da função | Nome | Grupo | Entrada | Saída | Família da função| Descrição |
|:----------------:|:----:|:-------:|:------:|:----:|:----------------:|:---------:|
| 67 | retornarData | Serviços de relógio em tempo real | EAX = Dia, em ASCII;  EBX = Mês, em ASCII; ECX = Século, em ASCII; EDX = Ano, em ASCII  | Sem saída | Hexagonix | Retorna informações de relógio em tempo real em formato ASCII (String). Conversão para número pode ser necessária|
| 68 | retornarHora | Serviços de relógio em tempo real | EAX = Hora, em ASCII; EBX = Minuto, em ASCII; ECX = Segundo, em ASCII | Sem saída | Hexagonix | Retorna informações de relógio em tempo real em formato ASCII (String). Conversão para número pode ser necessária|

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

## Exemplo de código

Abaixo, temos um exemplo de aplicativo que utiliza uma série de funções disponibilizadas pelo Hexagon. Você verá funções para obter informações e limpar o console, abrir dispositivos para escrita, exibir conteúdo no console e finalizar um processo em execução. Você verá que alguns macros são utilizados. Esses macros podem ser encontrados e estudados diretamente no repositório da [libasm](https://github.com/hexagonix/lib/blob/main/fasm/hexagon.s). O exemplo abaixo foi escrito em `Assembly x86` com sintaxe Intel, visando o montador fasm (flat assembler).

```assembly
format binary as "app" ;; Especifica o formato e extensao do arquivo

use32

cabecalhoAPP:

.assinatura:      db "HAPP"    ;; Assinatura
.arquitetura:     db 01h       ;; Arquitetura (i386 = 01h)
.versaoMinima:    db 1         ;; Versao minima do Hexagon(R)
.subversaoMinima: db 00        ;; Subversao minima do Hexagon(R)
.pontoEntrada:    dd inicioAPP ;; Offset do ponto de entrada
.tipoImagem:      db 01h       ;; Imagem executavel
.reservado0:      dd 0         ;; Reservado (Dword)
.reservado1:      db 0         ;; Reservado (Byte)
.reservado2:      db 0         ;; Reservado (Byte)
.reservado3:      db 0         ;; Reservado (Byte)
.reservado4:      dd 0         ;; Reservado (Dword)
.reservado5:      dd 0         ;; Reservado (Dword)
.reservado6:      dd 0         ;; Reservado (Dword)
.reservado7:      db 0         ;; Reservado (Byte)
.reservado8:      dw 0         ;; Reservado (Word)
.reservado9:      dw 0         ;; Reservado (Word)
.reservado10:     dw 0         ;; Reservado (Word)

;;*************************************************************

include "hexagon.s" ;; Incluir as chamadas de sistema
include "estelar.s" ;; Inclui funcoes de criacao de interfaces

;;*************************************************************

;; Variaveis e constantes

;; Agora vamos criar uma instancia da estrutura de controle de interfaces
;; Sintaxe: estrutura para o app (instancia), estrutura original

Andromeda.Interface Andromeda.Estelar.Interface

;; Dentro de gapp estarao todos os dados de texto que serao exibidos ao usuario.

VERSAO equ "1.1" ;; Versao do aplicativo

gapp:

.mensagemOla: db 10, 10, "Este e um exemplo de aplicativo HAPP grafico do Hexagonix(R)!", 10, 10
              db 10, 10, "Pressione qualquer tecla para finalizar este programa...", 10, 10, 0 

.TITULO:      db "Seja bem-vindo!", 0
.RODAPE:      db "[", VERSAO, "] | Pressione qualquer tecla para continuar...", 0                

.vd0:         db "vd0", 0 ;; Console principal

;;************************************************************************************

inicioAPP:

;; Vamos definir que queremos saida direta para vd0 (similar a tty0 no Linux)
;; Isso nem sempre e necessario. Se o shell foi utilizado para chamar o app, 
;; vd0 ja esta aberto. A menos que seja chamado por um app que esteja usando, por
;; exemplo, vd1. vd0 é o console principal, enquanto vd1-vdn são consoles virtuais.

    mov esi, gapp.vd0

    hx.syscall abrir ;; Abrir dispositivo

;; Pronto, agora vamos continuar. Primeiro, limpar a saida e obter informacoes
;; de resolucao

    hx.syscall limparTela

    hx.syscall obterInfoTela
    
    mov byte[Andromeda.Interface.numColunas], bl
    mov byte[Andromeda.Interface.numLinhas], bh

;; Vamos salvar o esquema de cores atual do Sistema, para consistencia
;; Isso e importante para definir se estamos em modo claro ou escuro de
;; interface

    hx.syscall obterCor

    mov dword[Andromeda.Interface.corFonte], eax
    mov dword[Andromeda.Interface.corFundo], ebx

;; Vamos criar a estrutura de interface com titulo e rodape

;; Formato de recebimento de parametros da funcao de criar interfaces:
;; Vale ressaltar que os parametros devem estar na ordem!
;;
;; titulo, rodape, cor do titulo, cor do rodape, cor do texto no titulo,
;; cor do texto no rodape, cor do texto inicial do app, cor de fundo inicial
;;
;; Voce pode utilizar '\' para quebrar a linha, caso esteja muito grande, como
;; abaixo

    Andromeda.Estelar.criarInterface gapp.TITULO, gapp.RODAPE, VERMELHO_TIJOLO,\
    VERMELHO_TIJOLO, BRANCO_ANDROMEDA, BRANCO_ANDROMEDA,\
    [Andromeda.Interface.corFonte], [Andromeda.Interface.corFundo]

;; Agora vamos imprimir na interface uma mensagem simples

    mov esi, gapp.mensagemOla

    imprimirString

;; Vamos aguardar interacao do usuario para finalizar o app

    hx.syscall aguardarTeclado

;; Interagiu? Ok, vamos finalizar o app

;; Formato:
;;
;; Codigo de erro (neste caso, 0), tipo de saida (neste caso, 0)

    Andromeda.Estelar.finalizarProcessoGrafico 0, 0
```

</div>