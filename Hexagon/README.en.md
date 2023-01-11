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

# Hexagon kernel

<p align="center">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/LogoHexagon.png" width="200" height="200">
</p>

<div align="justify">

Hexagon is a monolithic `kernel` running in 32-bit `protected mode`, developed purely in Assembly for the PC (x86) architecture. It is a kernel written from scratch, aiming for the speed and compatibility of modern hardware, but also being able to run on older hardware (Pentium III or higher, with 32 MB of RAM or more). At the moment, it guarantees a single-user environment, despite the use of virtual terminals, and single-tasking, despite the ability to load, keep in memory and control more than one process at a time, in a chronological order execution stack. In the future, the kernel may support the execution of multiple processes in preemptive multitasking. Hexagon was designed to be a Unix-like kernel and forms the basis of `Hexagonix`, albeit independently of it. It runs executable images in the `HAPP` format, developed exclusively for Hexagon. It also implements a very sophisticated API accessible through a standardized and documented system call, as you can see below.

Some features of Hexagon:

- [x] Support for x86 processors (Pentium III or higher);
- [x] Support for devices with 32 MB of RAM or more;
- [x] User environment support;
- [x] System call with 68 sophisticated functions accessed by the user environment;
- [x] Own executable binary format (HAPP);
- [x] Unix-like;
- [x] Completely written in x86 Assembly;
- [x] Self-hosting (the assembler used to build the Hexagon can run on top of it);
- [x] Virtual file system;
- [x] Device abstraction;
- [x] Full support for reading and writing on FAT16 file systems;
- [x] VESA VBE and multi-resolution graphics support;
- [x] Text mode support;
- [x] Graphic font rendering engine, which can be changed by the user;
- [x] Real-time clock support;
- [x] Support for serial and parallel ports (serial communication, debug and printing);
- [x] Supports own boot loader (Hexagon Boot - HBoot);
- [x] Support for users and permissions.

Other features being developed:

- [ ] Search and enumeration of all PCI devices;
- [ ] Preemptive multitasking.
    
> You can help implement the above development functions!

<details title="License" align='left'>
<br>
<summary align='left'>Hexagon License</summary>

<div align="justify">

Hexagonix Operating System

BSD 3-Clause License

Copyright (c) 2015-2023, Felipe Miguel Nery Lunkes<br>
All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.

Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

Neither the name of the copyright holder nor the names of its contributors may be used to endorse or promote products derived from this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

</div>

</details>

</div>

<!-- Will work like <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

### History of development

<div align="justify">

The kernel was initially designed and written aiming at a structure and operation close to DOS (Disk Operating System) systems, such as MS-DOS, in the years 2015 to 2017. Therefore, many system calls and device names followed a syntax and names FROM. Over time, there was an interest in bringing the then core of Andromeda, which at that time had no name and was kept together with the distribution's code, closer to a structure and functioning closer to Unix-like systems, such as BSD or Linux. , for example. In this way, many parts of the kernel were re-implemented with the new purpose in mind. The core code was separated from the rest of the System and became independent, both in terms of development and operation, in addition to gaining a name, Hexagon. A hardware abstraction layer was written with the inclusion of system calls known in the Unix world, such as open(), close(), read() and write(). Devices were named and disk drives changed from DOS naming to Unix device names. The kernel then proceeds to follow a known initialization process, executing, with PID 1, the first user process, init, which then loads the rest of the components. Unix-like utilities were then written that used the kernel's Unix-like API, and several Unix-like tools have been written since then (2017 onwards).

</div>

<!-- Will work like <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

### Hexagon Documentation Sessions

<div align="justify">

Now, you'll find more relevant information about Hexagon's particulars, how to develop compatible utilities, and how to help develop the kernel.

</div>

<details title="Developing for Hexagon" align='left'>
<br>
<summary align='left'><strong>Developing for Hexagon</strong></summary>

<div align="justify">

In this session, you will find relevant documentation on how to develop Hexagon compatible utilities. Select the topic of interest to you below. You can also suggest new topics. Just open an `issue` with your proposal.

</div>

<details title="Hexagon System Calls" align='left'>
<br>
<summary align='left'>Hexagon System Calls</summary>

<div align="justify">

Hexagon implements a series of functions that are exposed to the user environment, so that they can be used by developers to build utilities that use the Hexagon API. This API is accessible via system calls, or more easily via compatible development libraries such as [libasm](https://github.com/hexagonix/lib).

The number of system calls may vary with new Hexagon releases, as the tendency is for most non-critical functions to be moved to libraries, not staying in the core. However, with the natural evolution of the kernel, other functions and calls can be implemented.

At this time, there are 68 system calls that are exposed to the user environment by Hexagon. To do so, it implements an interrupt system that is accessible by any application via interrupt 69h (`int 69h`).

The format for passing parameters to the Hexagon interrupt handler is a mix of what is observed for what is implemented in MS-DOS and BSD systems. Some of the parameters are passed on the stack (as in BSD systems), while other parameters are passed through registers (as in MS-DOS), as follows:

* The desired function number is `always` supplied to Hexagon by the stack;
* The remaining parameters, which serve as input for the requested function, are provided exclusively by the registers, noting that each function accepts defined parameters and registers.

An example of a system call, to terminate the currently running process, can be seen below:

```assembly
    
    push 4     ;; Request function 4, to terminate process
    
    mov eax, 0 ;; Report error code 0
    
    int 69h    ;; call the hexagon
```
    
The `hexagon.s` file, present in the Hexagonix library by [libasm]() specifies all system calls currently supported by the current version of Hexagon, as well as lists the outputs and inputs for each requested function. Below you can see the system calls supported by Hexagon v1.0, extracted from `hexagon.s`. It is worth remembering that the calls below may change, so rely on libasm to identify which calls to use when writing an application.

```assembly
 
;;************************************************************************************
;;
;; Serviços de gerenciamento de memória e processos do Hexagonix®
;;
;;************************************************************************************

alocarMemoria = 1      ;; Alocar memória
                       ;; Entrada: EAX - Tamanho da memória solicitada, em bytes
                       ;; Saída: EBX - Ponteiro para a memória alocada

liberarMemoria = 2     ;; Liberar memória
                       ;; Entrada: EBX - Ponteiro para a memória alocada
                       ;; ECX - Tamanho da memória alocada

iniciarProcesso = 3    ;; Carregar programa do disco e o executar
                       ;; Entrada: ESI - Nome do programa; EDI - Argumentos; EAX = 0 se não forem passados argumentos
                       ;; Saída: CF definido em caso de erro ou programa não encontrado

encerrarProcesso = 4   ;; Terminar o processo atualmente em execução
                       ;; Entrada: EAX - Código de erro, caso exista
                       ;; EBX = 0 se apenas terminar a execução; EBX = 0x1234 para manter residente

obterPID = 5           ;; Retorna o indentificador do processo em execução
                       ;; Saída: EAX - PID do processo

usoMemoria = 6         ;; Retorna estatísticas de uso deste recurso, calculados pelo Sistema                                       
                       ;; Saída: EAX - Memória utilizada, em bytes
                       ;; EBX - Memória total disponível para uso, em bytes                        
                       ;; ECX - Memória total disponível para uso, em Mbytes (menos preciso)
                       ;; EDX - Memória reservada para o Hexagon®, em bytes
                       ;; ESI - Memória total alocada (resevada+processos), em kbytes
                                  
obterProcessos = 7     ;; Obtêm os processos presentes na pilha de execução                                       
                       ;; Saída: ESI - Lista de processos; EAX - Número de processos na pilha                                 

obterCodigoErro = 8    ;; Obtém o código retornado pelo último processo em execução.
                       ;; Saída: EAX - Código de erro (0 para sem erro/saída normal)

;;************************************************************************************
;;
;; Serviços de gerenciamento de arquivos e dispositivos do Hexagonix®
;;
;;************************************************************************************

abrir = 9              ;; Abre um canal de leitura/escrita com determinado dispositivo solicitado ou 
                       ;; arquivo comum presente no disco (dispositivos e discos são tratados como
                       ;; arquivos). Em caso de arquivo no disco, um endereço de carregamento deve ser 
                       ;; fornecido.                                                    
                       ;; Entrada: ESI - Ponteiro para o buffer que contêm o nome convencionado
                       ;; EDI - Endereço de carregamento, em caso de arquivo
                       ;; CF definido quando o nome do dispositivo for inválido ou arquivo não existir

escrever = 10          ;; Envia dados para o dispositivo aberto
                       ;; Entrada: SI - Ponteiro com o buffer contendo os dados
                       ;; Saída: CF definido em caso de erro ou nenhum dispositivo aberto
                       ;; Aviso! Futuramente, será utilizada para salvar arquivos.

fechar = 11            ;; Fecha o último dispositivo aberto

;;************************************************************************************
;;
;; Serviços de gerenciamento do Sistema de Arquivos e de volumes do Hexagonix®
;;
;;************************************************************************************

salvarArquivo = 13     ;; Salvar um arquivo no disco
                       ;; Entrada: ESI - Ponteiro para o nome do arquivo; EDI - Ponteiro para o conteúdo
                       ;; Entrada: EAX - Tamanho do arquivo
                       ;; Saída: CF definido em caso de erro ou arquivo já presente
						 
deletarArquivo	= 14   ;; Remover um arquivo do disco
                       ;; Entrada: ESI - Ponteiro para o nome do arquivo
                       ;; Saída: CF definido em caso de erro ou arquivo não existente

listarArquivos	= 15   ;; Obter lista de arquivos		
                       ;; Saída: ESI - Ponteiro para a lista de arquivos; EAX - Total de arquivos
						          
arquivoExiste = 16     ;; Checar se um arquivo existe no disco
                       ;; Entrada: ESI - Nome do arquivo para checar
                       ;; Saída: EAX - Tamanho do arquivo
                       ;; CF definido se o arquivo não existir				

obterDisco = 17        ;; Obtêm o disco utilizado pelo sistema
                       ;; Saída: ESI - Nome do dispositivo	
                       ;;        EDI - Rótulo do volume utilizado								  

;;************************************************************************************
;;
;; Serviços de gerenciamento de usuários do Hexagonix®
;;
;;************************************************************************************

travar = 18            ;; Bloqueia o processo em primeiro plano, impedindo que o mesmo seja terminado
                       ;; pelo usuário utilizando uma tecla especial ou combinação.
                       ;; A tecla F1 é no Hexagonix® a tecla "Matar processo".
                       ;; Esta tecla pode ter sua função removida com o tempo.

destravar = 19         ;; Habilita que o usuário mate o processo em execução pressionando uma tecla especial
                       ;; ou combinação de teclas. A tecla "Matar processo" (F1) se torna habilitada.
                       ;; Esta tecla pode ter sua função removida com o tempo.
								   
definirUsuario = 20    ;; Define um usuário para a sessão.
                       ;; Entrada: EAX - ID do grupo; ESI - Nome do usuário

obterUsuario = 21      ;; Obtêm dados do usuário logado na sessão
                       ;; Saída: EAX - ID do grupo; ESI - Nome do usuário

;;************************************************************************************
;;
;; Serviços oferecidos pelo Hexagonix®
;;
;;************************************************************************************

retornarVersao = 22    ;; Retorna a versão do Sistema para os aplicativos
                       ;; Saída: EAX - Número da versão; EBX - Número da subversão 
                       ;; CH - Caractere de revisão; EDX - Arquitetura
                       ;; ESI - Nome do Kernel 
                       ;; EDI - Build do Kernel

obterAleatorio = 23    ;; Obtêr um número aleatório
                       ;; Entrada: EAX - Máximo
                       ;; Saída: EAX - Número

alimentarAleatorio = 24 ;; Alimentar o gerador do números
                        ;; Entrada: EAX - Número


causarAtraso = 25      ;; Utilizada para causar um atraso (delay), utilizado para adaptar operações
                       ;; de memória, operações de disco e possibilitar leitura da tela por parte
                       ;; do usuário.
                       ;; Entrada: ECX - Tempo em unidades de contagem para causar atraso
 
instalarISR	= 26       ;; Instalar rotina de serviço de interrupção
                       ;; Entrada: EAX - Número da interrupção; ESI - Ponteiro para o manipulador

;;************************************************************************************
;;
;; Serviços de gerenciamento de energia do Hexagonix®
;;
;;************************************************************************************

reiniciarPC = 27       ;; Reiniciar o computador
					
desligarPC = 28        ;; Chama rotina da implementação APM do Hexagonix® para desligar o computador

;;************************************************************************************
;;
;; Serviços de saída em vídeo e gráficos do Hexagonix®
;;
;;************************************************************************************

imprimir = 29          ;; Imprimir um conteúdo definido em um dispositivo de saída
                       ;; Entrada:
                       ;;
                       ;; EAX - Conteúdo numérico, se este for o caso, respeitando os
                       ;;       formatos abaixo designados. Os formatos devem ser informados!
                       ;; ESI - Ponteiro para a string à ser impressa, se este for o caso.		
                       ;; EBX - Tipo de entrada, que pode ser:
                       ;;       01h - Inteiro decimal
                       ;;       02h - Inteiro hexadecimal
                       ;;       03h - Inteiro binário
                       ;;       04h - String        
                       ;; Dica! Utilize os macros no fim do arquivo para utilizar essa função                          							 	

limparTela = 30        ;; Limpa a tela		
						
limparLinha	= 31       ;; Limpa uma linha específica na tela
                       ;; Entrada: AL - Número da linha 
						
NULA  = 32             ;; Função nula, sem retorno ou função
                       ;; Mantida para compatibilidade

rolarTela = 33         ;; Rola a tela para baixo uma linha

definirCursor = 34     ;; Definir cursor em uma posição específica
                       ;; Entrada: DL - X; DH - Y

desenharCaractere = 35 ;; Colocar um pixel na tela
                       ;; Entrada: EAX - X; EBX - Y; EDX - Cor em hexadecimal

desenharBloco = 36     ;; Desenhar um bloco de cor específica
                       ;; Entrada: EAX - X; EBX - Y; ESI - Comprimento
                       ;; Entrada: EDI - Altura; EDX - Cor em hexadecimal

imprimirCaractere = 37 ;; Imprimir caractere na posição do cursor 
                       ;; Entrada: AL - Caractere; EBX - 01h para posicionar cursor					            

definirCor = 38        ;; Definir cor de fundo e primeiro plano
                       ;; Entrada: EAX - Cor da fonte (RGB em hexadecimal)
                       ;; EBX - Cor do plano de fundo (RGB em hexadecimal)
                       ;; ECX - 1234h para alterar o tema padrão para os valores solicitados
                       ;; Em modo texto, apenas preto e branco são permitidos
						 
obterCor = 39          ;; Obter cor de fundo e primeiro plano
                       ;; Saída: EAX - Plano de fundo (RGB em hexadecimal)
                       ;; EBX - Plano de fundo (RGB em hexadecimal)
                       ;; ECX - Cor padrão da fonte, no tema atual
                       ;; EDX - Cor padrão do plano de fundo, no tema atual
                       ;; Em modo texto, apenas preto e branco são permitidos

obterInfoTela = 40     ;; Obter informação da tela
                       ;; Saída: EAX - Resolução X (bits 0..15), Y (bits 16..31),
                       ;; EBX - Colunas (bit 0..7), Linhas (8..15), Bits por pixel (16..23),
                       ;; EDX - Endereço do início do frame de vídeo
                       ;; CF definido em caso de modo texto
										
atualizarTela = 41     ;; Atualizar a memória de vídeo com o conteúdo do Buffer
								
definirResolucao = 42  ;; Utilizado para definir a resolução à ser utilizada no vídeo
                       ;; Entrada: EAX - Número relativo a resolução à ser utilizada
                       ;;       1 - Resolução de 800x600 pixels
                       ;;       2 - Resolução de 1024x768 pixels 	
                       ;;       3 - Alterar para modo texto	

obterResolucao = 43    ;; Utilizado para obter o código relativo à resolução utilizada 
                       ;; no vídeo padrão
                       ;; Saída: EAX - Número relativo a resolução atualmente utilizada
                       ;;       1 - Resolução de 800x600 pixels
                       ;;       2 - Resolução de 1024x768 pixels
								   
obterCursor	= 44       ;; Obter posição do cursor
                       ;; Saída: DL - X, DH - Y
						            		

;;************************************************************************************
;;
;; Serviços de manipulação de teclado PS/2 do Hexagonix®
;;
;;************************************************************************************

aguardarTeclado	= 45   ;; Esperar pelo pressionamento de uma tecla no teclado
                       ;; Saída: AL - Caratere; AH - Scan code
							  
obterString	= 46       ;; Obter string do teclado
                       ;; Entrada: AL - Máximo de caracteres para receber
                       ;; EBX - Presença ou não de eco durante a digitação - 1234h
                       ;; para desativar o eco e <> 1234h para ativar
                       ;; Saída: ESI - String
						 
obterEstadoTeclas = 47 ;; Obter status das teclas especiais
                       ;; Saída: EAX - Status das teclas especiais
                       ;; 
                       ;; Formato:
                       ;;
                       ;; bit 0: Tecla Control
                       ;; bit 1: Tecla Shift
                       ;; bit 2-31: Reservado

alterarFonte = 48      ;; Altera a fonte padrão de exibição do sistema
                       ;; Entrada: ESI - Ponteiro para o buffer contendo o nome do arquivo
                       ;; que contêm a fonte compatível com o Sistema Operacional Hexagonix®
                       ;; Saída: CF definido em caso de arquivo não encontrado	                                        							  

alterarLeiaute = 49    ;; Altera o leiaute do teclado
                       ;; Entrada: ESI - Arquivo contendo um leiaute de teclado válido

;;************************************************************************************
;;
;; Serviços de manipulação de mouse PS/2 do Hexagonix®
;;
;;************************************************************************************

aguardarMouse = 50     ;; Aguardar por evento do mouse
                       ;; Saída: EAX - X; EBX - Y; EDX - Botões

obterMouse = 51        ;; Obter posição atual do mouse e estado dos botões
                       ;; Saída: EAX - X; EBX - Y; EDX - Botões

definirMouse = 52      ;; Definir nova posição do mouse
                       ;; Entrada: EAX - X; EBX Y

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

inserirCaractere = 55       ;; Inserir um caractere em posição específica da string
                            ;; Entrada: ESI - String; EDX - Caractere para inserir; AL - Caractere para inserir
								  
tamanhoString = 56          ;; Onter o tamanho de uma string 
                            ;; Entrada: ESI - String. 
                            ;; Saída: AX - Tamanho da string

compararString = 57         ;; Comparar duas strings 
                            ;; Entrada: ESI - Primeira string; EDI - Segunda string 
                            ;; Saída: CF definido se as duas forem iguais

stringParaMaiusculo = 58    ;; Converter string para maiúsculo
                            ;; Entrada: ESI - String

stringParaMinusculo	= 59    ;; Converter string para minúsculo 
                            ;; Entrada: ESI - String 

cortarString = 60           ;; Remover espaços em branco da string
                            ;; Entrada: ESI - String.

encontrarCaractere = 61     ;; Encontrar caractere específico na string
                            ;; Entrada: ESI - String, AL - caractere para encontrar
                            ;; Saída: EAX - Número de ocorrências do caractere
                            ;; CF definido se caractere não encontrado
							  
stringParaInt = 62          ;; Converter um número string para número inteiro
                            ;; Entrada: ESI - String
                            ;; Saída: EAX - Inteiro
                            ;; CF definido em caso e número inválido

paraString = 63             ;; Converte um número inteiro em uma string
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

;;************************************************************************************   
    
```
    
In the next section you can get more information on how to develop a simple application (text mode) for Hexagonix, using Hexagon services, libasm libraries and macros that make it easy to trigger system calls.

</div>

</details>

<details title="The HAPP executable format" align='left'>
<br>
<summary align='left'>The HAPP executable format</summary>

<div align="justify">

The HAPP executable image format was developed for Hexagon to allow the development of images that can be verified and validated for architecture and minimum kernel versions required for correct execution. The header also stores important information, allowing the developer to directly add an entry point, regardless of where it is inside the image, something that should have been redirected earlier when the executable image was in pure binary format. The HAPP image also allows you to validate that the image to be loaded is really an executable image, preventing unsupported files from being executed, even if they are not even executable files. It also allows the system to check code dependencies, such as the aforementioned architecture, as well as Hexagon version numbers, which must be equal to or greater than the minimum specified by the header. All HAPP images must have this full header, including reserved sessions, in order to function correctly in later versions of the System. HAPP images are always 32-bit.

In Assembly language, the system development language, the header, in its 2.0 specification:
    
```assembly
headerAPP:

.signature: db "HAPP"      ;; Signature
.architecture: db 01h      ;; Architecture (i386 = 01h)
.MinimumVersion: db 1      ;; Minimal version of Hexagon
.Minimum subversion: db 00 ;; Minimal Hexagon Subversion
.inputpoint: dd           ;; Input point offset (reference to main function here)
.ImageType: db 01h        ;; Executable image type (executable = 01h)
.reserved0: dd 0  ;; Reserved (Dword)
.reserved1: db 0  ;; Reserved (Byte)
.reserved2: db 0  ;; Reserved (Byte)
.reserved3: db 0  ;; Reserved (Byte)
.reserved4: dd 0  ;; Reserved (Dword)
.reserved5: dd 0  ;; Reserved (Dword)
.reserved6: dd 0  ;; Reserved (Dword)
.reserved7: db 0  ;; Reserved (Byte)
.reserved8: dw 0  ;; Reserved (Word)
.reserved9: dw 0  ;; Reserved (Word)
.reserved10: dw 0 ;; Reserved (Word)
```

Below is an implementation of a small application written as an example, which uses the Hexagon header and system calls, written in x86 assembly language in Intel syntax and assembled with the help of the flat assembler (FASM). This application sends a message to the terminal and then exits.

```assembly
;; This is a template for building a text mode app for
;; the Hexagonix!
;;
;; Written by Felipe Miguel Nery Lunkes on 12/04/2020
;;
;; You can generate an executable HAPP image using the assembler
;; FASM. To do this, use the command line below:
;;
;; fasmX tapp.asm
;; or
;; fasmX tapp.asm tapp.app

use32

headerAPP:

.signature: db "HAPP"      ;; Signature
.architecture: db 01h      ;; Architecture (i386 = 01h)
.MinimumVersion: db 1      ;; Minimal version of Hexagon
.Minimum subversion: db 00 ;; Minimal Hexagon Subversion
.EntryPoint: dd startAPP   ;; Entry point offset
.ImageType: db 01h         ;; Executable image
.reserved0: dd 0  ;; Reserved (Dword)
.reserved1: db 0  ;; Reserved (Byte)
.reserved2: db 0  ;; Reserved (Byte)
.reserved3: db 0  ;; Reserved (Byte)
.reserved4: dd 0  ;; Reserved (Dword)
.reserved5: dd 0  ;; Reserved (Dword)
.reserved6: dd 0  ;; Reserved (Dword)
.reserved7: db 0  ;; Reserved (Byte)
.reserved8: dw 0  ;; Reserved (Word)
.reserved9: dw 0  ;; Reserved (Word)
.reserved10: dw 0 ;; Reserved (Word)

;;************************************************ *************

include "hexagon.s" ;; Include system calls
include "macros.s"  ;; Includes macros

;;************************************************ *************

;; Variables and constants

msg: db 10, 10, "This is a template with a simple HAPP application example!", 10, 0

;;************************************************ *************

;; entry point

startAPP:

    mov esi, msg

    imprimirString ;; Here we have a macro that configures and calls an API function

    Hexagonix terminarProcesso ;; Another macro that asks which call to make
    
```
    
</div>

</details>

</details>

<!-- Will work like <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

### Looking for collaborators

<div align="justify">

Hexagon, like all of Hexagonix, seeks collaborators willing to contribute to its development. If you are interested in Assembly, C, helping with writing documentation or just helping with translation, feel free to get in touch! All are welcome!

</div>
