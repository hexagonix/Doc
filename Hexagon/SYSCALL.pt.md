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

O Hexagon fornece uma série de chamadas de sistema bem documentadas para o desenvolvimento de utilitários e aplicativos. As chamadas de sistema podem ser acessadas por meio da interrupção de software de número `69h`. Ao solicitar uma chamada de sistema, você também deve fornecer parâmetros que identifiquem a função escolhida, bem como os parâmetros exigidos para ela. Você pode encontrar os parâmetros exigidos ara elas na tabela abaixo.

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

Agora, uma tabela com as funções da chamada de sistema do Hexagonix.

> Vale lembrar que uma tabela de funções, padronizada segundo as funções disponíveis no Version 7 UNIX, está sendo desenvolvida. Nesse caso, não existe o objetivo de pareamento de número de função junto ao UNIX, mas conformidade no nome das funções. Por exemplo, `alocarMemoria` se tornaria `free`, e `retornarVersao`, `uname`. No futuro, ambas as nomenclaturas estarão disponíveis para permitir a migração de aplicativos e utilitários. Venha novamente nesse arquivo mais tarde para checar atualizações. 


<details title="Gerenciamento de memória e processos" align='left'>
<br>
<summary align='left'>Funções de gerenciamento de memória e processos</summary>

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
| 9 | abrir | Gerenciamento de arquivos e dispositivos | ESI = Ponteiro para o buffer que contêm o nome convencionado; EDI = Endereço de carregamento, em caso de arquivo| CF definido quando o nome do dispositivo for inválido ou arquivo não existir | Unix-like | Abre um canal de leitura/escrita em um dispositivo solicitado ou arquivo comum presente no disco (dispositivos e discos são tratados como arquivos). Em caso de arquivo no disco, um endereço de carregamento deve ser fornecido|
| 10 | escrever | Gerenciamento de arquivos e dispositivos |  ESI = Ponteiro com o buffer contendo os dados | CF definido em caso de erro ou nenhum dispositivo aberto | Unix-like | Envia dados para o dispositivo aberto|
| 11 | fechar | Gerenciamento de arquivos e dispositivos | Sem entrada | Sem saída | Unix-like | Fecha o último dispositivo aberto pelo processo atual|
| 13 | salvarArquivo | Gerenciamento de arquivos e dispositivos |  ESI = Ponteiro para o nome do arquivo; EDI = Ponteiro para o conteúdo; EAX = Tamanho do arquivo | CF definido em caso de erro ou arquivo já presente | Unix-like | Salva um arquivo no volume montado|
| 14 | deletarArquivo | Gerenciamento de arquivos e dispositivos | ESI = Ponteiro para o nome do arquivo | CF definido em caso de erro ou arquivo não existente | Unix-like | Remove um arquivo no volume montado |
| 15 | listarArquivos | Gerenciamento de arquivos e dispositivos | Sem entrada | ESI = Ponteiro para a lista de arquivos; EAX = Total de arquivos | Unix-like | Obtêm lista de arquivos presentes no volume |
| 16 | arquivoExiste | Gerenciamento de arquivos e dispositivos | ESI = Nome do arquivo para checar |  EAX = Tamanho do arquivo; CF definido se o arquivo não existir | Hexagonix | Checar se um arquivo existe no volume |
| 17 | obterDisco | Gerenciamento de arquivos e dispositivos | Sem entrada | ESI = Nome do dispositivo; EDI = Rótulo do volume utilizado | Hexagonix | Obtêm informações do disco montado em `/`|

</details>

<details title=" Gerenciamento de usuário e permissões" align='left'>
<br>
<summary align='left'>Funções de gerenciamento de usuário e permissões</summary>

| Número da função | Nome | Grupo | Entrada | Saída | Família da função| Descrição |
|:----------------:|:----:|:-------:|:------:|:----:|:----------------:|:---------:|
| 18 | travar | Gerenciamento de usuário e permissões | Sem entrada | Sem saída | Unix-like | Bloqueia o sinal de término de processo em primeiro plano por tecla especial|
| 19 | destravar | Gerenciamento de usuário e permissões | Sem entrada | Sem saída | Unix-like | Habilita o sinal de término de processos em primeiro plano por uso de tecla especial|
| 20 | definirUsuario | Gerenciamento de usuário e permissões | EAX = ID do grupo; ESI = Nome do usuário | Sem saída | Hexagonix | Define um usuário para a sessão atual|
| 21 | obterUsuario | Gerenciamento de usuário e permissões | Sem entrada | EAX = ID do grupo; ESI = Nome do usuário| Hexagonix | Obtêm dados do usuário logado para a sessão atual|

</details>

<details title="Serviços do Hexagon" align='left'>
<br>
<summary align='left'>Serviços do Hexagon</summary>

| Número da função | Nome | Grupo | Entrada | Saída | Família da função| Descrição |
|:----------------:|:----:|:-------:|:------:|:----:|:----------------:|:---------:|
| 22 | retornarVersao | Serviços do Hexagon | Sem entrada | EAX = Número da versão; EBX = Número da subversão; CH = Caractere de revisão; EDX = Arquitetura; ESI = Nome do Kernel; EDI = Build do Kernel| Unix-like | Retorna a versão do Hexagon para os aplicativos| 
| 23 | obterAleatorio | Serviços do Hexagon | EAX = Máximo | EAX = Número | Hexagonix | Obtêm um número aleatório|
| 24 | alimentarAleatorio | Serviços do Hexagon | EAX - Número para criar entropia | Sem saída | Hexagonix | Alimenta com entropia o gerador de números aleatórios do kernel|
| 25 | causarAtraso | Serviços do Hexagon | ECX = Tempo em unidades de contagem para causar atraso | Sem saída | Hexagonix | Causa um atraso (delay) em operações |
| 26 | instalarISR | Serviços do Hexagon | EAX = Número da interrupção; ESI = Ponteiro para o manipulador | Sem saída | Hexagonix | Instala rotina de serviço de interrupção|

</details>

<details title="Serviços do gerenciamento de energia" align='left'>
<br>
<summary align='left'>Serviços de gerenciamento de energia</summary>

| Número da função | Nome | Grupo | Entrada | Saída | Família da função| Descrição |
|:----------------:|:----:|:-------:|:------:|:----:|:----------------:|:---------:|
| 27 | reiniciarPC | Gerenciamento de energia | Sem entrada | Sem saída | Unix-like | Solicita o reinicio do dispositivo|
| 28 | desligarPC | Gerenciamento de energia | Sem entrada | Sem saída | Unix-like | Solicita o desligamento do dispositivo|

</details>

<details title="Serviços de vídeo e gráficos" align='left'>
<br>
<summary align='left'>Serviços de vídeos e gráficos</summary>

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

</details>

<details title="Serviços de manipulação de teclado PS/2" align='left'>
<br>
<summary align='left'>Serviços de manipulação de teclado PS/2</summary>

| Número da função | Nome | Grupo | Entrada | Saída | Família da função| Descrição |
|:----------------:|:----:|:-------:|:------:|:----:|:----------------:|:---------:|
| 45 | aguardarTeclado | Serviços de manipulação de teclado PS/2 | Sem entrada | AL = Caractere; AH - Scancode | Hexagonix | Espera o pressionamento de uma tecla no teclado|
| 46 | obterString | Serviços de manipulação de teclado PS/2 | AL = Máximo de caracteres para obter | EBX = Presença ou não de eco durante a digitação (1234h para sem eco e qualquer valor para ativar); ESI = String | Hexagonix | Obtêm uma string do teclado|
| 47 | obterEstadoTeclas | Serviços de manipulação de teclado PS/2 | Sem entrada | EAX = Status das teclas especiais (bit 0: Tecla Control; bit 1: Tecla Shift; bit 2-31: Reservado) | Hexagonix | Obtêm o estado das teclas especiais, como Control e Shift|
| 48 | alterarFonte | Serviços de manipulação de teclado PS/2 | ESI = Ponteiro para o buffer contendo o nome do arquivo que contêm a fonte compatível com o Hexagonix | CF definido em caso de arquivo não encontrado ou incompatível | Hexagonix | Altera a fonte padrão de exibição do sistema| 
| 49 | alterarLeiaute | Serviços de manipulação de teclado PS/2 | ESI = Arquivo contendo um leiaute de teclado válido | CF definido em caso de arquivo não encontrado ou incompatível | Hexagonix | Altera o leiaute do teclado|

</details>

<details title="Serviços de manipulação de mouse PS/2" align='left'>
<br>
<summary align='left'>Serviços de manipulação de mouse PS/2</summary>

| Número da função | Nome | Grupo | Entrada | Saída | Família da função| Descrição |
|:----------------:|:----:|:-------:|:------:|:----:|:----------------:|:---------:|
| 50 | aguardarMouse | Sem entrada |  EAX = Posição no eixo X; EBX = Posição no eixo Y; EDX = Botões | Hexagonix | Aguarda por evento do mouse|
| 51 | obterMouse | Sem entrada | EAX = Posição no eixo X; EBX = Posição no eixo Y; EDX = Botões | Hexagonix | Obtêm posição atual do mouse e estado dos botões|
| 52 | definirMouse | EAX = Posição no eixo X; EBX = Posição no eixo Y | Sem saída | Hexagonix | Define nova posição do mouse|

</details>

;;************************************************************************************
;;
;; Serviços de manipulação e conversão de dados do Hexagonix®
;;
;;************************************************************************************

compararPalavrasString = 53 ;; Comparar primeiras words de duas strings 
                            ;; Entrada: ESI - Primeira string; EDI - Segunda string 
                            ;; Saída: CF definido se iguais

removerCaractereString = 54 ;; Remover um caractere de uma posição específica na string 
                            ;; Entrada: ESI - String; EAX - Posição do caractere

inserirCaractere = 55   ;; Inserir um caractere em posição específica da string
                        ;; Entrada: ESI - String; EDX - Caractere para inserir; AL - Caractere para inserir
                                  
tamanhoString = 56      ;; Onter o tamanho de uma string 
                        ;; Entrada: ESI - String. 
                        ;; Saída: AX - Tamanho da string

compararString = 57     ;; Comparar duas strings 
                        ;; Entrada: ESI - Primeira string; EDI - Segunda string 
                        ;; Saída: CF definido se as duas forem iguais

stringParaMaiusculo = 58 ;; Converter string para maiúsculo
                         ;; Entrada: ESI - String

stringParaMinusculo = 59 ;; Converter string para minúsculo 
                         ;; Entrada: ESI - String 

cortarString = 60       ;; Remover espaços em branco da string
                        ;; Entrada: ESI - String.

encontrarCaractere = 61 ;; Encontrar caractere específico na string
                        ;; Entrada: ESI - String, AL - caractere para encontrar
                        ;; Saída: EAX - Número de ocorrências do caractere
                        ;; CF definido se caractere não encontrado
                              
stringParaInt = 62      ;; Converter um número string para número inteiro
                        ;; Entrada: ESI - String
                        ;; Saída: EAX - Inteiro
                        ;; CF definido em caso e número inválido

paraString = 63         ;; Converte um número inteiro em uma string
                        ;; Entrada: EAX - Inteiro à ser convertido
                        ;; Saída: ESI - Ponteiro para o buffer contendo o conteúdo 

;;************************************************************************************
;;
;;  Serviços de saída por som do Hexagonix®
;;
;;************************************************************************************  

emitirSom = 64         ;; Toca um tom no alto-falante interno do computador
                       ;; Entrada: AX - Frequência à ser reproduzida

desligarSom = 65       ;; Desliga o alto-falante interno do computador, interrompendo
                       ;; qualquer emissão de som em progresso                               

;;************************************************************************************
;;
;;  Serviços de mensagens do Hexagonix®
;;
;;************************************************************************************  

enviarMensagemHexagon = 66 ;; Envia uma mensagem de alta prioridade do Hexagon
                           ;; Entrada: ESI - Mensagem
                           ;;          EAX - Código de erro, se houver
                           ;;          EBX - Prioridade 

;;************************************************************************************                      

;;************************************************************************************
;;
;;  Serviço de relógio em tempo real do Hexagon®
;;
;;************************************************************************************  

retornarData = 67      ;; Retorna informações de relógio em tempo real em formato
                       ;; ASCII (String). Conversão para número pode ser necessária
                       ;; Saída: EAX - Dia, em ASCII
                       ;;        EBX - Mês, em ASCII
                       ;;        ECX - Século, em ASCII
                       ;;        EDX - Ano, em ASCII

retornarHora = 68      ;; Retorna informações de relógio em tempo real em formato
                       ;; ASCII (String). Conversão para número pode ser necessária
                       ;;        EAX - Hora, em ASCII
                       ;;        EBX - Minuto, em ASCII
                       ;;        ECX - Segundo, em ASCII
```

</div>