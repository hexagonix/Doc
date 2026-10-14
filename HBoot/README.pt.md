<!--

Copyright (c) 2015-2026 Felipe Miguel Nery Lunkes
Todos os direitos reservados.

-->

<p align="center">
<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/banner.png">
</p>

<div align="center">

![](https://img.shields.io/github/license/hexagonix/HBoot.svg)
![](https://img.shields.io/github/stars/hexagonix/HBoot.svg)
![](https://img.shields.io/github/issues/hexagonix/HBoot.svg)
![](https://img.shields.io/github/issues-closed/hexagonix/HBoot.svg)
![](https://img.shields.io/github/issues-pr/hexagonix/HBoot.svg)
![](https://img.shields.io/github/issues-pr-closed/hexagonix/HBoot.svg)
![](https://img.shields.io/github/downloads/hexagonix/HBoot/total.svg)
![](https://img.shields.io/github/release/hexagonix/HBoot.svg)
[![](https://img.shields.io/twitter/follow/hexagonixOS.svg?style=social&label=Follow%20%40HexagonixOS)](https://twitter.com/hexagonixOS)

</div>

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

# Inicialização do Hexagon

<div align="center">

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/HexagonixSourceHeader.png">

</div>

O processo de inicialização do Hexagonix se dá por dois componentes que entrem em ação antes do Hexagon (kernel). São eles `Saturno` (responsável por carregar o segundo estágio de inicialização) e o `HBoot`, que é responsável por carregar, configurar e executar o Hexagon, bem como oferecer outros recursos.

Vamos conhecer cada um deles?

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Saturno

O primeiro componente do Hexagonix é o Saturno. Ele é responsável por receber o controle do processo de inicialização realizado pelo BIOS/UEFI e procurar no volume o segundo estágio de inicialização. Para isso, ele implementa um driver para leitura de um sistema de arquivos FAT16. O segundo estágio de inicialização (ver adiante) pode implementar drivers para outros sistemas de arquivos e é responsável por encontrar o Hexagon, carregar módulos HBoot ou carregar um sistema do tipo DOS compatível (versão BETA).

<details title="Licença do Saturno" align='left'>
<summary align='left'>Licença do Saturno</summary>
<br>

<div align="justify">

Leia a [licença](https://github.com/hexagonix/Doc/blob/main/LICENSES/BSD-3) para mais informações sobre direitos autorais, propriedade de código e redistribuição que se aplicam aos arquivos disponíveis neste repositório. O Hexagonix é totalmente licenciado sob [BSD-3-Clause](https://opensource.org/licenses/BSD-3-Clause). Sempre fique atento ao arquivo `LICENSE` disponível em cada repositório para estar ciente dos direitos e obrigações legais, bem como à lista de contribuidores do projeto.

</div>

</details>

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## HBoot

O HBoot é um componente desenvolvido para permitir a inicialização do kernel Hexagon. Até então, a inicialização era realizada por apenas um estágio, que definia um ambiente bem básico, carregava o Hexagon na memória e imediatamente passava o controle para ele, fornecendo um conjunto bem pequeno e limitado de parâmetros, uma vez que o código desse estágio fica restrito a 512 bytes, o que limita a realização de diversos testes e processamento de dados. Como o HBoot, foi possível expandir o número de tarefas realizadas antes da execução do Hexagon, além da possibilidade de fornecer mais informações a respeito do ambiente da máquina e de inicialização. Isso é particularmente importante para permitir a criação de uma árvore de dispositivos que pode ser utilizada pelo Hexagon para decidir como manipular cada dispositivo identificado. O HBoot é capaz de verificar quais unidades de disco estão disponíveis na máquina, emitir um tom de inicialização, obter a quantidade de memória RAM disponível instalada e permitir ou não o seguimento do processo de boot de acordo com essa informação. Caso nenhuma interação do usuário seja detectada 3 segundos após todos os testes e atividades essenciais para criar um ambiente de inicialização para o Hexagon, o sistema irá carregar e executar o Hexagon (presente em um arquivo no volume nomeado de **HEXAGON.SIS** no Hexagonix H1 e **HEXAGON** no Hexagonix H2), sendo descarregado da memória. A interação com o HBoot se dá pelo pressionamento da tecla F8 após a respectiva mensagem surgir na tela.

<details title="Licença do HBoot" align='left'>
<summary align='left'>Licença do HBoot</summary>
<br>

<div align="justify">

Leia a [licença](https://github.com/hexagonix/Doc/blob/main/LICENSES/BSD-3) para mais informações sobre direitos autorais, propriedade de código e redistribuição que se aplicam aos arquivos disponíveis neste repositório. O Hexagonix é totalmente licenciado sob [BSD-3-Clause](https://opensource.org/licenses/BSD-3-Clause). Sempre fique atento ao arquivo `LICENSE` disponível em cada repositório para estar ciente dos direitos e obrigações legais, bem como à lista de contribuidores do projeto.

</div>

</details>

### Outras funções disponíveis

* O HBoot permite o carregamento de módulos no formato HBoot, que podem ser úteis, no futuro, para permitir testes de hardware, como testes de memória e disco, caso os módulos estejam disponíveis no disco. Os módulos podem ser utilizados também para extender as funções do HBoot. A especificação do formato já está disponível e um exemplo pode ser encontrado abaixo. Esses módulos podem ser utilizados para testar dispositivos específicos, obter informações do hardware ou carregar arquivos em sistemas de arquivos não suportados originalmente pelo HBoot.
* No contexto do desenvolvimento do Hexagonix, o HBoot também pode carregar diretamente, a partir de um módulo atualmente built-in (essa função será movida para um módulo standalone o quanto antes) o núcleo do sistema operacional de código livre FreeDOS[^1], para que ferramentas utilitárias já estabelecidas e robustas que sejam executadas em ambiente DOS possam ser executadas sobre o volume e arquivos Hexagonix. O FreeDOS foi escolhido devido a sua característica de kernel composto por um único arquivo, geralmente "KERNEL.SYS"[^2], além da sua distribuição livre e gratuita. Já outros DOS, como o MS-DOS, anterior a versão 7.0, utilizam dois arquivos que devem estar contíguos no disco, e isso não é possível aqui, visto que a instalação do FreeDOS ocorre já em um volume Hexagonix, com a cópia do kernel, interpretador de comando e outros utilitários DOS, sendo que o sistema operacional principal é o Hexagonix, com iniciação opcional do FreeDOS para alguma atividade em especial[^3]. Caso os componentes de sistema do FreeDOS não estejam presentes no disco (a cópia dos arquivos do FreeDOS não faz parte da imagem padrão), a inicialização em modo de compatibilidade DOS não irá ocorrer.

[^1]: Você pode encontrar a página do projeto [aqui](https://www.freedos.org/).
[^2]: A inicialização em modo DOS foi possível após pesquisa na documentação do FreeDOS, especialmente no arquivo "SYS.C" (que pode ser encontrado [aqui](http://www.ibiblio.org/pub/micro/pc-stuff/freedos/files/dos/sys/2043/)), que indica em qual segmento o kernel espera ser carregado e quais os parâmetros são necessários. Cada sistema DOS apresenta um segmento de carregamento preferencial e a execução de outras edições do DOS (como MS-DOS, DR-DOS, etc) pode ser implementada futuramente com o auxílio dos módulos HBoot. Todo o código para o carregamento do núcleo foi desenvolvido do zero e não se baseia em algum existente.
[^3]: A iniciação em modo de compatibilidade DOS do HBoot pode ser útil para rodar ferramentas de verificação de erros no volume, desfragmentação do volume, particionador e outras ferramentas de diagnóstico, bem como de desenvolvimento, como compiladores e montadores que não são suportados pelo Hexagonix (as ferramentas de 16 bits, por exemplo).

### Exemplo de módulo HBoot

Abaixo é possível encontrar um exemplo de implementação de módulo HBoot:

```assembly
;;************************************************************************************
;;
;;
;; Módulo do HBoot
;;
;; Hexagon Boot - HBoot
;;
;; Copyright © 2020-2026 Felipe Miguel Nery Lunkes
;; Todos os direitos reservados
;;
;;************************************************************************************

use16

;; O módulo deve apresentar um cabeçalho especial de imagem HBoot
;; São 16 bytes, com assinatura (número mágico), arquitetura alvo, versão,
;; subversão e nome interno, este último com até 8 bytes

cabecalhoHBoot:

.assinatura:  db "HBOOT"    ;; Assinatura, 5 bytes
.arquitetura: db 01h        ;; Arquitetura (i386), 1 byte
.versaoMod:   db 01h        ;; Versão
.subverMod:   db 00h        ;; Subversão
.nomeMod:     db "EXEMPLO " ;; Nome do módulo

SERVICOS_HBOOT = 20h ;; Interrupção pela qual o HBoot atende um módulo

;; O módulo começa a ser executado no deslocamento 10h, logo após o cabeçalho.
;; A pilha e o segmento de carregamento são fornecidos pelo HBoot, então basta
;; obter o segmento a partir de CS, que não pode ser copiado diretamente para
;; DS e ES

inicioModulo:

    mov ax, cs
    mov ds, ax
    mov es, ax

;; Seu código aqui. Exibir uma mensagem é um serviço do próprio HBoot, então o
;; módulo não precisa carregar a sua própria rotina de escrita na tela

    mov si, mensagem
    mov ah, 01h

    int SERVICOS_HBOOT

;; O retorno ao HBoot também é um serviço, e é o único caminho de volta

    mov ah, 05h

    int SERVICOS_HBOOT

mensagem:
db 13, 10, "Modulo iniciado com sucesso.", 13, 10, 0
```

### Serviços do HBoot para módulos

Um módulo recebe a máquina inteira para si e, sem ajuda, precisaria carregar sua própria cópia de tudo o que o HBoot já faz. Para evitar isso, o HBoot instala a interrupção `20h` antes de entregar o controle e atende o módulo por ela.

O contrato é o mesmo para todas as funções e não varia:

* Na entrada, `AH` contém o número da função;
* Na saída, `CF` estará limpo caso a função tenha tido sucesso e marcado caso tenha falhado, com `EAX` contendo o resultado da função ou, quando `CF` estiver marcado, o código do erro;
* Todos os demais registradores, incluindo os de segmento, retornam exatamente como entraram. O módulo pode chamar a interrupção a partir de qualquer pilha e de qualquer profundidade.

As funções disponíveis:

| Função | Descrição | Entrada | Saída |
| --- | --- | --- | --- |
| `00h` | Informações da interface | `ES:DI` aponta para um buffer de 8 bytes | Buffer preenchido e `EAX` = 0 |
| `01h` | Exibir mensagem | `DS:SI` aponta para o texto, terminado em 0, sem limite de tamanho | `EAX` = 0 |
| `02h` | Informações do volume | `ES:DI` aponta para um buffer de 12 bytes | Buffer preenchido e `EAX` = 0 |
| `03h` | Carregar arquivo | `DS:SI` aponta para o caminho, terminado em 0, e `ES` é o segmento de destino, a partir do deslocamento 0 | `EAX` contém o tamanho do arquivo em bytes |
| `04h` | Ler setores | `EBX` é o primeiro LBA, `CX` a quantidade de setores e `ES:DI` o buffer de destino | `EAX` = 0 |
| `05h` | Retornar ao HBoot | — | Não retorna |

Os códigos de erro devolvidos em `EAX`:

| Código | Significado |
| --- | --- |
| `01h` | Função inexistente |
| `02h` | Caminho que o volume não é capaz de representar |
| `03h` | Arquivo não encontrado no volume |
| `04h` | Função chamada com um valor que não pode ser utilizado |

O buffer preenchido pela função `00h` permite ao módulo verificar com qual interface está falando antes de depender dela:

```assembly
.assinatura:  db "HBSV" ;; 4 bytes
.versao:      db 01h
.subversao:   db 00h
.funcoes:     db 05h    ;; Maior número de função atendido
.reservado:   db 00h
```

O buffer preenchido pela função `02h` descreve o volume a partir do qual o HBoot foi iniciado:

```assembly
.BPB:          dd 0 ;; Endereço plano do BIOS Parameter Block
.particaoLBA:  dd 0 ;; LBA onde a partição começa
.unidade:      db 0 ;; Unidade da qual o HBoot foi carregado
.sistema:      db 0 ;; Código do sistema de arquivos
.reservado:    dw 0
```

A função `03h` recebe o caminho da mesma forma que o usuário o escreve na linha de comando do HBoot, como em `/boot/hexagon`. Um caminho pode nomear no máximo um diretório, e cada nome nele no máximo 8 caracteres mais uma extensão de 3.

O retorno ao HBoot é a função `05h` e é o único caminho de volta. Reiniciar a máquina ou saltar direto para o ponto de entrada do HBoot não funciona: o HBoot obtém o volume do qual foi carregado a partir de registradores que o módulo é livre para ter utilizado. Após a função `05h`, o HBoot retoma a inicialização de onde havia parado.

### Sistemas de arquivos suportados

* FAT16B
* FAT12 (em desenvolvimento)

Novos sistemas de arquivos serão implementados no futuro.

### Reportar bugs

O HBoot ganhou muita complexidade desde o início de seu desenvolvimento, em 2020. Devido a esse aumento de código e a natureza de sua operação (16-bit), bugs podem ser encontrados. Os mesmos podem ser reportados no repositório ou por email, disponível no final deste arquivo.
