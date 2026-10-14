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
| 2 | hx.free | Gerenciamento de memória e processos | EBX = Ponteiro devolvido antes por hx.malloc; ECX = Ignorado, mantido para que chamadores escritos para o contrato antigo continuem montando | Sem saída | Libera a memória alocada previamente|
| 3 | hx.exec | Gerenciamento de memória e processos | ESI = Nome do programa; EDI = Argumentos; EAX = 0 se não forem passados argumentos| CF definido em caso de erro ou imagem não encontrada | Carrega e executa imagem presente no volume|
| 4 | hx.exit | Gerenciamento de memória e processos | EAX = Código de erro, caso exista; EBX = 0 se apenas terminar a execução; EBX = 0x1234 para manter residente | Sem saída | FInaliza a execução de um processo |
| 5 | hx.pid | Gerenciamento de memória e processos | Sem entrada | EAX = PID do processo atual | Obtêm o PID do processo em execução |
| 6 | hx.spawn | Gerenciamento de memória e processos | ESI = Nome do arquivo a executar; EDI = Argumentos; EAX = 0 se não forem passados argumentos | EAX = PID do novo processo; CF definido em caso de erro (limite de processos atingido, imagem não encontrada ou incompatível, ou memória insuficiente) | Cria um novo processo sem bloquear o processo chamador, que segue em execução imediatamente após o spawn|
| 7 | hx.kill | Gerenciamento de memória e processos | EBX = PID do processo alvo | CF definido se não existir processo com o PID informado; EAX = 05h | Encerra um processo arbitrário pelo PID, podendo ser chamado por qualquer processo contra qualquer outro (diferente do encerramento do processo em primeiro plano pela tecla especial)|
| 8 | hx.memoryUsage | Gerenciamento de memória e processos | Sem entrada | EAX = Memória utilizada, em bytes; EBX = Memória total disponível para uso, em bytes; ECX = Memória total disponível para uso, em Mbytes (menos preciso); EDX = Memória reservada para o Hexagon, em bytes; ESI = Memória total alocada (resevada+processos), em kbytes| Obter o uso detalhado de memória pelo sistema|
| 9 | hx.getProcesses | Gerenciamento de memória e processos | ESI = Buffer do próprio chamador; ECX = Tamanho desse buffer, em bytes | EAX = Número de registros escritos, com CF limpo; EAX = Tamanho de buffer necessário, em bytes, com CF marcado, caso em que nada foi escrito. O tamanho do registro encabeça o buffer, em um dword, e é de 78 bytes nesta versão. Cada registro: `+0` PID; `+4` PID do processo pai (0 se iniciado pelo kernel); `+8` Estado; `+9` Nome, terminado em NUL, no máximo 64 bytes; `+73` Terminal virtual ao qual o processo está conectado; `+74` Memória que o processo ocupa, em bytes | Obtêm os processos em execução em um buffer que pertence ao chamador|
| 10 | hx.getErrorCode | Gerenciamento de memória e processos | Sem entrada | EAX = Código de erro (0 para sem erro)| Obtêm o código retornado pelo último processo em execução|
| 11 | hx.getenv | Gerenciamento de memória e processos | ESI = Nome da variável de ambiente | ESI = Ponteiro para o valor; CF definido se a variável não estiver definida | Lê uma variável de ambiente do processo chamador|
| 12 | hx.setenv | Gerenciamento de memória e processos | ESI = Nome da variável de ambiente; EDI = Valor | CF definido se não houver espaço suficiente no ambiente para a nova entrada | Define ou substitui uma variável de ambiente do processo chamador|
| 13 | hx.unsetenv | Gerenciamento de memória e processos | ESI = Nome da variável de ambiente | CF definido se a variável não estiver definida | Remove uma variável de ambiente do processo chamador|
| 14 | hx.environ | Gerenciamento de memória e processos | Sem entrada | ESI = Ponteiro para o bloco de ambiente bruto, terminado por dois NUL; EAX = Número de variáveis definidas | Obtém todas as variáveis de ambiente do processo chamador|
| 15 | hx.open | Gerenciamento de arquivos e dispositivos | ESI = Ponteiro para o nome do dispositivo, ou para o caminho do arquivo a abrir (uma `/` inicial é absoluto, caso contrário é relativo ao diretório atual); EDI = Endereço de carregamento, em caso de arquivo| CF definido quando o nome do dispositivo for inválido ou arquivo não existir | Abre um canal de leitura/escrita em um dispositivo solicitado ou arquivo comum presente no disco (dispositivos e discos são tratados como arquivos). Em caso de arquivo no disco, um endereço de carregamento deve ser fornecido|
| 16 | hx.write | Gerenciamento de arquivos e dispositivos |  ESI = Ponteiro com o buffer contendo os dados | CF definido em caso de erro ou nenhum dispositivo aberto | Envia dados para o dispositivo aberto|
| 17 | hx.close | Gerenciamento de arquivos e dispositivos | Sem entrada | Sem saída | Fecha o último dispositivo aberto pelo processo atual|
| 18 | hx.create | Gerenciamento de arquivos e dispositivos |  ESI = Ponteiro para o caminho do arquivo a criar (absoluto ou relativo ao diretório atual); EDI = Ponteiro para o conteúdo; EAX = Tamanho do arquivo | CF definido em caso de erro, arquivo já presente, ou uma pasta do caminho não existir (EAX = `IO.pathNotFound`) | Salva um arquivo no volume montado|
| 19 | hx.touch | Gerenciamento de arquivos e dispositivos | ESI = Ponteiro para o caminho do arquivo a criar (absoluto ou relativo ao diretório atual) | CF definido se o arquivo já existir, uma pasta do caminho não existir, ou o nome do arquivo tiver mais de 12 caracteres | Cria um novo arquivo vazio no volume montado, diferente do `hx.create` acima, que grava conteúdo em uma única etapa. Um arquivo criado por root sai protegido, pelo atributo SYSTEM do FAT16B. Veja `hx.chown`|
| 20 | hx.unlink | Gerenciamento de arquivos e dispositivos | ESI = Ponteiro para o caminho do arquivo a remover | CF definido em caso de erro, arquivo não existente, ou remoção de um arquivo protegido (veja `hx.chown`) por um usuário não-root | Remove um arquivo no volume montado |
| 21 | hx.rename | Gerenciamento de arquivos e dispositivos | ESI = Ponteiro para o caminho do arquivo original; EDI = Ponteiro para o novo nome (apenas seu último componente é usado; renomear para outra pasta não é suportado) | CF definido em caso de erro, erro na atualização de nome, ou renomeação de um arquivo protegido (veja `hx.chown`) por um usuário não-root | Atualiza o nome de um arquivo no volume montado |
| 22 | hx.listFiles | Gerenciamento de arquivos e dispositivos | ESI = Buffer do próprio chamador; ECX = Tamanho desse buffer, em bytes | EAX = Número de entradas escritas, com CF limpo; EAX = Tamanho de buffer necessário, em bytes, com CF marcado, caso em que nada foi escrito; EAX = 0 com CF marcado indica um erro do sistema de arquivos. Cada entrada termina em um byte separador de arquivo ou de diretório, com a lista inteira terminada em NUL | Obtêm lista de arquivos presentes no diretório atual em um buffer que pertence ao chamador|
| 23 | hx.stat | Gerenciamento de arquivos e dispositivos | ESI = Ponteiro para o caminho do arquivo a checar (absoluto ou relativo ao diretório atual) | EAX = Tamanho do arquivo; EBX = Valor de `Hexagon.Dev.DeviceClasses`, 0 se o caminho não for um device node; ECX = Byte de atributo (bit `10h` marca uma pasta); EDX = Major (DH) e minor (DL) do dispositivo, 0:0 se não for device node; CF definido se o arquivo, ou uma pasta do caminho, não existir | Checar se um arquivo existe no volume, e se é um device node|
| 24 | hx.mkdir | Gerenciamento de arquivos e dispositivos | ESI = Caminho do diretório a criar (absoluto ou relativo ao diretório atual) | CF definido em caso de erro; EAX = `IO.pathNotFound` se uma pasta do caminho não existir | Cria um novo diretório vazio no volume montado. Um diretório criado por root sai protegido, pelo atributo SYSTEM do FAT16B. Veja `hx.chown`|
| 25 | hx.rmdir | Gerenciamento de arquivos e dispositivos | ESI = Caminho do diretório a remover (absoluto ou relativo ao diretório atual) | CF definido em caso de erro; EAX = `IO.operationDenied` para um usuário não-root removendo um diretório protegido (veja `hx.chown`), `IO.directoryNotEmpty` se o diretório ainda tiver entradas além de `.` e `..` | Remove um diretório vazio do volume montado|
| 26 | hx.changeDirectory | Gerenciamento de arquivos e dispositivos | ESI = Caminho do diretório para o qual mudar (absoluto ou relativo, pode ter vários componentes separados por `/`) | CF definido se o caminho for vazio ou algum componente não for encontrado ou for inválido. Em caso de erro, o diretório atual permanece inalterado | Altera o diretório de trabalho atual|
| 27 | hx.getCurrentDirectory | Gerenciamento de Arquivos e Dispositivos | EDI = Buffer do chamador, com pelo menos 64 bytes | Buffer preenchido com o caminho do diretório atual, terminado em NUL | Copia o diretório atual do processo chamador para um buffer fornecido por ele|
| 28 | hx.mount | Gerenciamento de arquivos e dispositivos | ESI = Caminho do dispositivo, resolvido através de um mount devfs, como `/dev/hd1` para o disco inteiro ou `/dev/hd1p0` para uma de suas até 4 partições primárias MBR. O nome reservado `devfs` cria um mount devfs, sem nenhum dispositivo por trás; EDI = Caminho do ponto de montagem, que já deve existir como diretório | CF definido em caso de erro; EAX = `01h` permissão negada (somente root), `02h` dispositivo não encontrado, não é um dispositivo de bloco, ou nenhuma de suas partições é suportada, `03h` o ponto de montagem não existe, não é um diretório, ou seu caminho é longo demais, `04h` sem slots de montagem livres, `05h` falha ao ler o BPB da partição, `06h` ponto de montagem já em uso por outro dispositivo. Montar um dispositivo em um ponto de montagem que já o tem tem sucesso sem fazer nada, para que reler /etc/fstab continue idempotente | Monta um dispositivo ou uma de suas partições em um diretório existente|
| 29 | hx.unmount | Gerenciamento de arquivos e dispositivos | ESI = Caminho do ponto de montagem | CF definido em caso de erro; EAX = `01h` permissão negada (somente root), `02h` nada montado nesse caminho, `03h` recusa desmontar `/` | Desmonta um dispositivo previamente montado|
| 30 | hx.getMounts | Gerenciamento de arquivos e dispositivos | ESI = Buffer do próprio chamador, dimensionado para até 8 registros de 109 bytes cada (`+0` caminho do ponto de montagem, terminado em NUL, 64 bytes; `+64` caminho do dispositivo, terminado em NUL, 32 bytes; `+96` código do tipo de sistema de arquivos, 1 byte; `+97` rótulo do volume, terminado em NUL, 12 bytes) | EAX = Número de registros escritos | Lista todos os pontos de montagem atualmente em uso em um buffer que pertence ao chamador|
| 31 | hx.lock | Gerenciamento de usuário e permissões | Sem entrada | Sem saída | Bloqueia o sinal de término de processo em primeiro plano por tecla especial|
| 32 | hx.unlock | Gerenciamento de usuário e permissões | Sem entrada | Sem saída | Habilita o sinal de término de processos em primeiro plano por uso de tecla especial|
| 33 | hx.setUser | Gerenciamento de usuário e permissões | EAX = ID do usuário | Sem saída | Registra o id do usuário agora logado, fornecido pelo gerenciador de login. O kernel não armazena nem resolve nomes de usuário; esse mapeamento vive no userland, no banco `/etc/shadow`|
| 34 | hx.getUser | Gerenciamento de usuário e permissões | Sem entrada | EAX = ID do usuário | Obtêm o id do usuário logado na sessão atual|
| 35 | hx.uname | Serviços do Hexagon | EDI = Buffer do próprio chamador, com pelo menos 96 bytes | EAX = Número da versão; EBX = Número da subversão; ECX = Número de revisão; EDX = Arquitetura; EDI = Preservado. O nome do kernel é escrito no início do buffer e seu build no deslocamento 48, ambos terminados em NUL | Retorna a versão do Hexagon para os aplicativos|
| 36 | hx.sleep | Serviços do Hexagon | ECX = Tempo em unidades de contagem para causar atraso | Sem saída | Causa um atraso (delay) em operações |
| 37 | hx.installISR | Serviços do Hexagon | EAX = Número da interrupção; ESI = Ponteiro para o manipulador | Sem saída | Instala rotina de serviço de interrupção|
| 38 | hx.restart | Gerenciamento de energia | Sem entrada | Sem saída | Solicita o reinicio do dispositivo|
| 39 | hx.shutdown | Gerenciamento de energia | Sem entrada | Sem saída | Solicita o desligamento do dispositivo|
| 40 | hx.print | Serviços de vídeo e gráficos | EAX = Conteúdo numérico, se este for o caso, respeitando os formatos designados. Os formatos devem ser informados; ESI = Ponteiro para a string à ser impressa, se este for o caso; EBX = Tipo de entrada (01h - inteiro decimal; 02h - inteiro hexadecimal; 03h - inteiro binário; 04h - string; 05h - caractere único, obtido de AL e desenhado na coluna DL e na linha DH, sem mexer no cursor)| Sem saída | Envia um conteúdo definido para um dispositivo de saída|
| 41 | hx.clearConsole | Serviços de vídeo e gráficos | Sem entrada | Sem saída | Limpa o console atual|
| 42 | hx.clearLine | Serviços de vídeo e gráficos | AL = Número da linha  | Sem saída | Limpa uma linha específica no console|
| 43 | hx.scrollConsole | Serviços de vídeo e gráficos | Sem entrada | Sem saída | Rola o console para baixo uma linha|
| 44 | hx.setCursor | Serviços de vídeo e gráficos | DL = posição no eixo X; DH = posição no eixo Y | Sem saída | Define o cursor em uma posição específica|
| 45 | hx.drawCharacter | Serviços de vídeo e gráficos |  EAX = posição no eixo X; EBX = posição no eixo Y; EDX = Cor em hexadecimal | Sem saída | Coloca um pixel no console|
| 46 | hx.drawBlock | Serviços de vídeo e gráficos | EAX = posição no eixo X; EBX = posição no eixo Y; ESI = Comprimento; EDI = Altura; EDX = Cor em hexadecimal | Sem saída | Desenha um bloco de cor específica|
| 47 | hx.printCharacter | Serviços de vídeo e gráficos | AL = Caractere; EBX = 01h para reposicionar cursor | Sem saída | Imprimir caractere no console na posição do cursor|
| 48 | hx.setColor | Serviços de vídeo e gráficos | EAX = Cor da fonte (RGB em hexadecimal); EBX = Cor do plano de fundo (RGB em hexadecimal); ECX = 1234h para alterar o tema padrão para os valores solicitados; Em modo texto, apenas preto e branco são permitidos | Sem saída | Define cor de fundo e primeiro plano|
| 49 | hx.getColor | Serviços de vídeo e gráficos | Sem entrada | EAX = Cor da fonte (RGB em hexadecimal); EBX = Cor do plano de fundo (RGB em hexadecimal); ECX = 1234h para alterar o tema padrão para os valores solicitados; Em modo texto, apenas preto e branco são permitidos | Obtêm cor de fundo e primeiro plano|
| 50 | hx.getConsoleInfo | Serviços de vídeo e gráficos | Sem entrada | EAX = Resolução X (bits 0..15), Y (bits 16..31); EBX = Colunas (bit 0..7), Linhas (8..15), Bits por pixel (16..23); EDX = Endereço do início do frame de vídeo; CF definido em caso de modo texto | Obtêm informações do console atual|
| 51 | hx.updateScreen | Serviços de vídeo e gráficos | Sem entrada | Sem saída | Atualiza o console primário com conteúdo do console secundário (double buffering)|
| 52 | hx.getResolution | Serviços de vídeo e gráficos | Sem entrada | EAX = Número relativo a resolução à ser utilizada (1 = Resolução de 800x600 pixels; 2 - Resolução de 1024x768 pixels) | Ontêm a resolução utilizadapelo console principal|
| 53 | hx.getCursor | Serviços de vídeo e gráficos | Sem entrada | DL = Eixo X; DH = Eixo Y | Obtêm a posição do cursor|
| 54 | hx.waitKeyboard | Serviços de dispositivos PS/2 | Sem entrada | AL = Caractere; AH - Scancode | Espera o pressionamento de uma tecla no teclado|
| 55 | hx.getString | Serviços de dispositivos PS/2 | AL = Máximo de caracteres para obter; EBX = Presença ou não de eco durante a digitação (1234h para sem eco e qualquer valor para ativar); EDI = Buffer do próprio chamador, com espaço para esses caracteres mais um terminador | Buffer preenchido com a string digitada, terminada em NUL | Obtêm uma string do teclado|
| 56 | hx.getKeyState | Serviços de dispositivos PS/2 | Sem entrada | EAX = Status das teclas especiais (bit 0: Tecla Control; bit 1: Tecla Shift; bit 2-31: Reservado) | Obtêm o estado das teclas especiais, como Control e Shift|
| 57 | hx.changeConsoleFont | Serviços de vídeo e gráficos | ESI = Ponteiro para o buffer contendo o nome do arquivo que contêm a fonte compatível com o Hexagonix | CF definido em caso de arquivo não encontrado ou incompatível | Altera a fonte padrão de exibição do sistema|
| 58 | hx.changeLayout | Serviços de dispositivos PS/2 | ESI = Arquivo contendo um leiaute de teclado válido | CF definido em caso de arquivo não encontrado ou incompatível | Altera o leiaute do teclado|
| 59 | hx.waitMouse | Serviços de dispositivos PS/2 | Sem entrada |  EAX = Posição no eixo X; EBX = Posição no eixo Y; EDX = Botões | Aguarda por evento do mouse|
| 60 | hx.getMouse | Serviços de dispositivos PS/2 | Sem entrada | EAX = Posição no eixo X; EBX = Posição no eixo Y; EDX = Botões | Obtêm posição atual do mouse e estado dos botões|
| 61 | hx.setMouse | Serviços de dispositivos PS/2 | EAX = Posição no eixo X; EBX = Posição no eixo Y | Sem saída | Define nova posição do mouse|
| 62 | hx.emitSound | Serviços de saída de som | AX = Frequência a ser reproduzida | Sem saída | Toca um tom no alto-falante interno do computador|
| 63 | hx.turnOffSound | Serviços de saída de som| Sem entrada | Sem saída | Desliga o alto-falante interno do computador, interrompendo qualquer emissão de som em progresso|
| 64 | hx.sendMessageHexagon | Serviço de mensagens | ESI = Mensagem; EAX = Código de erro, se houver; EBX = Prioridade | Sem saída | Envia uma mensagem de alta prioridade do Hexagon|
| 65 | hx.date | Serviços de relógio em tempo real | EAX = Dia, em ASCII;  EBX = Mês, em ASCII; ECX = Século, em ASCII; EDX = Ano, em ASCII  | Sem saída | Retorna informações de relógio em tempo real em formato ASCII (String). Conversão para número pode ser necessária|
| 66 | hx.time | Serviços de relógio em tempo real | EAX = Hora, em ASCII; EBX = Minuto, em ASCII; ECX = Segundo, em ASCII | Sem saída | Retorna informações de relógio em tempo real em formato ASCII (String). Conversão para número pode ser necessária|
| 67 | hx.ioctl | Serviço de controle de dispositivo | EBX = Comando; ECX = Argumento do comando (o dispositivo deve estar aberto via hx.open) | CF definido se nada estiver aberto, ou se a classe do dispositivo aberto não tratar o comando | Controle específico de dispositivo que não se encaixa no fluxo de bytes de leitura e escrita, despachado para o tratador da classe do dispositivo aberto. Para uma tty, o comando 2 ativa aquele terminal virtual|
| 68 | hx.chown | Gerenciamento de arquivos e dispositivos | ESI = Ponteiro para o caminho; EBX = Modo solicitado (só o bit `0002h`, escrita para outros, é observado) | CF definido em caso de erro; EAX = `01h` permissão negada (só root), `02h` caminho não encontrado | Aplica um modo no estilo Unix a um caminho. Só root pode chamar. O que o modo muda depende do sistema de arquivos: o FAT16B não tem dono nem bits de permissão reais, então só deriva se o caminho passa a ser de escrita exclusiva de root, pelo seu atributo SYSTEM, do mesmo jeito que o driver msdosfs do FreeBSD reduz o chmod ao atributo read-only do DOS. O DEVFS ignora a chamada, sem erro|
| 69 | hx.getTTY | Serviço de gerenciamento de processos | Sem entrada | EAX = Índice da VT (0-7) | Informa ao processo chamador a qual terminal virtual ele está conectado, como o ttyname() do Unix|
| 70 | hx.sysctl | Serviço de diagnóstico do kernel | EBX = Número da função (apenas 1 existe: ler o buffer de log do kernel); ESI = Buffer do chamador; ECX = Tamanho do buffer do chamador | EAX = Bytes copiados; CF definido em caso de número de função desconhecido | Lê dados de diagnóstico do kernel em um buffer que pertence ao chamador. A função 1 retorna o buffer de log do próprio kernel, sem as mensagens de userland|

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Exemplo de código

Abaixo, temos um exemplo de aplicativo em modo texto que utiliza uma série de funções disponibilizadas pelo Hexagon. Você verá o cabeçalho HAPP montado pela macro `headerHAPP` da [libasm](https://github.com/hexagonix/lib/blob/main/fasm/HAPP.s), a inclusão das chamadas de sistema e macros de console, a limpeza do console, a impressão de uma mensagem, a espera por uma tecla e o encerramento do processo. Esses macros podem ser encontrados e estudados diretamente no repositório da [libasm](https://github.com/hexagonix/lib/blob/main/fasm/hexagon.s). O exemplo abaixo foi escrito em `Assembly x86` com sintaxe Intel, visando o montador fasm (flat assembler).

```assembly
format binary as "app" ;; Especifica o formato e a extensao do arquivo

use32

include "HAPP.s" ;; Macro que monta o cabecalho HAPP

;; Arquitetura | Versao | Subversao | Ponto de entrada | Tipo de imagem | Tamanho do BSS
headerHAPP HAPP.Architectures.i386, 1, 8, applicationStart, 01h, HAPP.bss.none

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