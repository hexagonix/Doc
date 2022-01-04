# Inicialização do Hexagon

O Hexagon Boot (HBoot) é responsável por carregar, configurar e executar o Hexagon, bem como oferecer outros recursos.

## Aviso de direitos autorais

* Sistema Operacional Hexagonix e Sistema Operacional Andromeda. Copyright © 2016-2022 Felipe Miguel Nery Lunkes. Todos os direitos reservados.
* Kernel Hexagon. Copyright © 2016-2022 Felipe Miguel Nery Lunkes. Todos os direitos reservados.
* Gerenciador de Boot Saturno. Copyright © 2016-2022 Felipe Miguel Nery Lunkes. Todos os direitos reservados.
* Hexagon Boot (HBoot). Copyright © 2020-2022 Felipe Miguel Nery Lunkes. Todos os direitos reservados.

## Saturno

O primeiro componente do Hexagonix/Andromeda é o Saturno. Ele é responsável por receber o controle do processo de inicialização realizado pelo BIOS/UEFI e procurar no volume o segundo estágio de inicialização. Para isso, ele implementa um driver para leitura de um sistema de arquivos FAT16. O segundo estágio de inicialização (ver adiante) pode implementar drivers para outros sistemas de arquivos e é responsável por encontrar o Hexagon, carregar módulos HBoot ou carregar um sistema do tipo DOS compatível (versão BETA).

## Hexagon Boot (HBoot)

O Hexagon Boot (HBoot) é um componente desenvolvido permitir a inicialização do kernel Hexagon. Até então, a inicialização era realizada por apenas um estágio, que definia um ambiente bem básico, carregava o Hexagon na memória e imediatamente passava o controle para ele, fornecendo um conjunto bem pequeno e limitado de parâmetros, uma vez que o código desse estágio fica restrito a 512 bytes, o que limita a realização de diversos testes e processamento de dados. Como o HBoot, foi possível expandir o número de tarefas realizadas antes da execução do Hexagon, além da possibilidade de fornecer mais informações a respeito do ambiente da máquina e de inicialização. Isso é particularmente importante para permitir a criação de uma árvore de dispositivos que pode ser utilizada pelo Hexagon para decidir como manipular cada dispositivo identificado. O HBoot é capaz de verificar quais unidades de disco estão disponíveis na máquina, emitir um tom de inicialização, obter a quantidade de memória RAM disponível instalada e permitir ou não o seguimento do processo de boot de acordo com essa informação. Caso nenhuma interação do usuário seja detectada 3 segundos após todos os testes e atividades essenciais para criar um ambiente de inicialização para o Hexagon, o sistema irá carregar e executar o Hexagon (presente em um arquivo no volume nomeado de **HEXAGON.SIS**), sendo descarregado da memória. A interação com o HBoot se dá pelo pressionamento da tecla F8 após a respectiva mensagem surgir na tela. 

### Outras funções disponíveis

* O HBoot permite o carregamento de módulos no formato HBoot, que podem ser úteis, no futuro, para permitir testes de hardware, como testes de memória e disco, caso os módulos estejam disponíveis no disco. Os módulos podem ser utilizados também para extender as funções do HBoot. A especificação do formato já está disponível e um exemplo pode ser encontrado abaixo. Esses módulos podem ser utilizados para testar dispositivos específicos, obter informações do hardware ou carregar arquivos em sistemas de arquivos não suportados originalmente pelo HBoot.
* No contexto do desenvolvimento do Hexagonix, o HBoot também pode carregar diretamente, a partir de um módulo atualmente built-in (essa função será movida para um módulo standalone o quanto antes) o núcleo do sistema operacional de código livre FreeDOS[^1], para que ferramentas utilitárias já estabelecidas e robustas que sejam executadas em ambiente DOS possam ser executadas sobre o volume e arquivos Hexagonix/Andromeda. O FreeDOS foi escolhido devido a sua característica de kernel composto por um único arquivo, geralmente "KERNEL.SYS"[^2], além da sua distribuição livre e gratuita. Já outros DOS, como o MS-DOS, anterior a versão 7.0, utilizam dois arquivos que devem estar contíguos no disco, e isso não é possível aqui, visto que a instalação do FreeDOS ocorre já em um volume Hexagonix, com a cópia do kernel, interpretador de comando e outros utilitários DOS, sendo que o sistema operacional principal é o Hexagonix/Andromeda, com iniciação opcional do FreeDOS para alguma atividade em especial[^3]. Caso os componentes de sistema do FreeDOS não estejam presentes no disco (a cópia dos arquivos do FreeDOS não faz parte da imagem padrão), a inicialização em modo de compatibilidade DOS não irá ocorrer.

[^1]: Você pode encontrar a página do projeto [aqui](https://www.freedos.org/).
[^2]: A inicialização em modo DOS foi possível após pesquisa na documentação do FreeDOS, especialmente no arquivo "SYS.C" (que pode ser encontrado [aqui](http://www.ibiblio.org/pub/micro/pc-stuff/freedos/files/dos/sys/2043/)), que indica em qual segmento o kernel espera ser carregado e quais os parâmetros são necessários. Cada sistema DOS apresenta um segmento de carregamento preferencial e esse carregamento de outras edições do DOS pode ser implementada futuramente com o auxílio dos módulos HBoot. Todo o código para o carregamento do núcleo foi desenvolvido do zero e não se baseia em algum existente.
[^3]: A iniciação em modo de compatibilidade DOS do HBoot pode ser útil para rodar ferramentas de verificação de erros no volume, desfragmentação do volume, particionador e outras ferramentas de diagnóstico, bem como de desenvolvimento, como compiladores e montadores que não são suportados pelo Hexagonix/Andromeda (as ferramentas de 16 bits, por exemplo).

### Exemplo de módulo HBoot

Abaixo é possível encontrar um exemplo de implementação de módulo HBoot:

```assembly
;;************************************************************************************
;;
;;    
;;                                Módulo do HBoot
;;        
;;                             Hexagon® Boot - HBoot
;;           
;;                 Copyright © 2020-2021 Felipe Miguel Nery Lunkes
;;                         Todos os direitos reservados
;;                                  
;;************************************************************************************

use16					

;; O módulo deve apresentar um cabeçalho especial de imagem HBoot
;; São 6 bytes, com assinatura (número mágico) e arquitetura alvo

cabecalhoHBoot:

.assinatura:  db "HBOOT"     ;; Assinatura, 5 bytes
.arquitetura: db 01h         ;; Arquitetura (i386), 1 byte

;; Configurar pilha e ponteiro

    cli				   ;; Desativar interrupções
    
    mov ax, 0x2000                 ;; Definir aqui os registradores de pilha
    mov ss, ax
    mov sp, 0
    
    sti				   ;; Habilitar interrupções
     
    clc 

    mov ax, 0x2000                 ;; Definir aqui os registradores de segmento
    mov ds, ax
    mov es, ax
    
    sti                            ;; Habilitar as interrupções

;; Seu código aqui

```

### Sistemas de arquivos suportados

* FAT16B
* FAT12 (em desenvolvimento)

Novos sistemas de arquivos serão implementados no futuro.

### Reportar bugs

O HBoot ganhou muita complexidade desde o início de seu desenvolvimento, em 2020. Devido a esse aumento de código e a natureza de sua operação (16-bit), bugs podem ser encontrados. Os mesmos podem ser reportados no repositório ou por email, disponível no final deste arquivo.

# Licença do software (código-fonte)

Sistema Operacional Hexagonix
Sistema Operacional Andromeda

Copyright (C) 2015-2022 Felipe Miguel Nery Lunkes. Todos os direitos reservados.

Licença de software Hexagonix versão 2.0 (2021)

Versão em português

Esta licença se aplica a quaisquer arquivos presentes neste repositório, 
incluindo qualquer diretório, que componham o sistema operacional Hexagonix/Andromeda,
bem como seus componentes, bibliotecas, arquivos de dados, imagens binárias e arquivos adjacentes.

Por meio desta, fica proibida a posse, publicação, uso, comercial ou não,
e redistribuição de quaisquer parte de código sob esta licença sem autorização 
explícita por escrito do desenvolvedor. Além disso, essa licença expressa a 
obrigatoriedade de destruição de quaiquer cópias de arquivos obtidos ilegalmente 
deste repositório, sob risco de sanções legais aplicáveis.

O SOFTWARE É FORNECIDO "COMO ESTÁ", SEM GARANTIA DE QUALQUER TIPO, EXPRESSA OU
IMPLÍCITA, INCLUINDO, MAS NÃO SE LIMITANDO ÀS GARANTIAS DE COMERCIALIZAÇÃO,
ADEQUAÇÃO A UMA FINALIDADE ESPECÍFICA E NÃO VIOLAÇÃO. EM NENHUMA HIPÓTESE O 
AUTORES OU TITULARES DE DIREITOS AUTORAIS SÃO RESPONSÁVEIS POR QUALQUER RECLAMAÇÃO,
DANOS OU OUTRAS RESPONSABILIDADES, SEJA EM AÇÃO DE CONTRATO, DELITO OU DE OUTRA FORMA,
DECORRENTE DE, FORA DE OU EM CONEXÃO COM O SOFTWARE OU O USO OU OUTRAS NEGOCIAÇÕES NO
SOFTWARE.

English version

Hexagonix Operating System
Andromeda Operating System

Copyright (C) 2015-2022 Felipe Miguel Nery Lunkes. All rights reserved.

Hexagonix Software License version 2.0 (2021)

This license applies to any files present in this repository,
including any directory, which make up the Hexagonix/Andromeda operating system,
as well as its components, libraries, data files, binary images and adjacent files.

Hereby, possession, publication, use, commercial or otherwise, is prohibited.
and redistribution of any code under this license without authorization
explicit written statement from the developer. In addition, this license expresses the
obligatory destruction of any copies of files obtained illegally
of this repository, at the risk of applicable legal sanctions.

THE SOFTWARE IS PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NON-INFRINGEMENT. UNDER NO EVENT
AUTHORS OR COPYRIGHT HOLDERS ARE RESPONSIBLE FOR ANY CLAIM,
DAMAGES OR OTHER LIABILITIES, WHETHER IN CONTRACT, TORT OR OTHERWISE,
ARISING OUT OF, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER NEGOTIATIONS ON
SOFTWARE.

Versão deste arquivo: 1.0