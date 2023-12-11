<p align="center">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/banner.png">
</p>

<div align="center">

![](https://img.shields.io/github/license/hexagonix/xfnt.svg)
![](https://img.shields.io/github/stars/hexagonix/xfnt.svg)
![](https://img.shields.io/github/issues/hexagonix/xfnt.svg)
![](https://img.shields.io/github/issues-closed/hexagonix/xfnt.svg)
![](https://img.shields.io/github/issues-pr/hexagonix/xfnt.svg)
![](https://img.shields.io/github/issues-pr-closed/hexagonix/xfnt.svg)
![](https://img.shields.io/github/downloads/hexagonix/xfnt/total.svg)
![](https://img.shields.io/github/release/hexagonix/xfnt.svg)
[![](https://img.shields.io/twitter/follow/hexagonixOS.svg?style=social&label=Follow%20%40HexagonixOS)](https://twitter.com/hexagonixOS)

</div>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

# Fontes gráficas do Hexagonix

<div align="center">

<img src="https://github.com/hexagonix/Doc/blob/main/Img/HexagonixSourceHeader.png">

</div>

<div align="justify">

A instalação padrão do Hexagonix também fornece uma série de fontes que podem ser carregadas pelo aplicativo de configurações ou utilitário de fontes (fnt). Esses arquivos são utilizados para alterar a fonte de exibição em modo gráfico do Hexagonix.

As fontes de modo gráfico para Hexagon são desenvolvidas como um bitmap em Assembly que, compiladas, geram uma imagem binária da fonte com representações de cada caractere. Os códigos das fontes padrão do Hexagonix já foram liberados como código livre (BSD-3-Clause) e estão disponíveis no [repositório de fontes do Hexagonix](https://github.com/hexagonix/xfnt).

</div>

## Como criar sua própria fonte gráfica

<div align="justify">

Sinta-se a vontade para realizar o download da fonte [modelo](https://github.com/hexagonix/xfnt/blob/main/modelo.asm), que já vem estruturada mas com os caracteres em branco, e desenhar a sua própria fonte gráfica! Para isso, você precisa saber algumas informações técnicas sobre elas:

* As fontes apresentam um altura de 16 e largura de 8 pixels. Essa informação é necessária para garantir que a sua fonte não apresente problemas durante o uso.
* Você pode dividir livremente os espaços reservados para acentuação e outras características gráficas de cada caractere, como cedilha ou porções que ficam abaixo da linha de caracteres, como y, vírgula e etc.
* As fontes são desenhadas no formato bitmap. Sendo assim, cada caractere é um mapa de pixels composto de 0s e 1s. Os 0s simbolizam áreas que não serão exibidas do caractere, enquanto os 1s representam os pixels do caractere que serão exibidos ao usuário. Você deve alterar cada matriz de caractere na fonte modelo adicionando os 1s onde deseja que os pixels sejam exibidos para formar o caractere. Abaixo, você verá um exemplo de um caractere em branco e a mesma representação deste caractere pronto para a fonte.

O código abaixo mostra a representação em bitmap da cerquilha (#) na fonte [modelo](https://github.com/hexagonix/xfnt/blob/main/modelo.txt) e a implementação na fonte [aurora](https://github.com/hexagonix/xfnt/blob/main/aurora.asm).

```assembly
;; Representação em branco do caractere cerquilha (#), no modelo 
.cerquilha: 

	db 00000000b ;; Dois pixels de altura para caracteres acima da letra
	db 00000000b
	
	db 00000000b
	db 00000000b
	db 00000000b
	db 00000000b
	db 00000000b
	db 00000000b
	db 00000000b
	db 00000000b
	db 00000000b
	db 00000000b ;; Cinco pixels de altura para caracteres acima da letra
	db 00000000b
	db 00000000b
	db 00000000b
	db 00000000b

;; Representação em branco do caractere cerquilha (#), na fonte hint 

.cerquilha: 
	db 00000000b
	db 00000000b
	
	db 00000000b
	db 00000000b
	db 00100010b
	db 00100010b
	db 01111111b
	db 00100010b
	db 01111111b
	db 00100010b
	db 00100010b
	db 00000000b
	db 00000000b
	db 00000000b
	db 00000000b
	db 00000000b
```

Você já deve conseguir visualizar, dentro da matriz, a cerquilha, marcada pela presença de 1s.

Você pode desenvolver quantas fontes quiser e brincar com designs diferentes e inovadores, bem como estender os caracteres disponíveis (utilizando a referência ASCII).

</div>

<div align="center">
   
[![Hexagonix-xfnt](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=xfnt&theme=dark)](https://github.com/hexagonix/xfnt)

</div>
