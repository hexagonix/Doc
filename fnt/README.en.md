<p align="center">
<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/banner.png">
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

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

# Hexagonix graphic fonts

<div align="center">

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/HexagonixSourceHeader.png">

</div>

<div align="justify">

The default installation of Hexagonix also provides a number of fonts that can be loaded by the settings application or the font utility (fnt). These files are used to change the graphical display font for Hexagonix.

Graphics mode fonts for Hexagon are developed as a bitmap in Assembly which, when compiled, generate a binary image of the font with representations of each character. The standard Hexagonix fonts have now been released as open source and are available in the [Hexagonix font repository](https://github.com/hexagonix/xfnt).

</div>

## How to create your own graphic font

<div align="justify">

Feel free to download the font [modelo](https://github.com/hexagonix/xfnt/blob/main/modelo.asm), which is already structured but with blank characters, and draw the your own graphic font! For that, you need to know some technical information about them:

* Fonts have a height of 16 and a width of 8 pixels. This information is necessary to ensure that your font does not experience problems during use.
* You can freely divide the placeholders for accents and other graphic features of each character, such as cedilla or portions that fall below the character line, such as y, comma, etc.
* Fonts are drawn in bitmap format. Therefore, each character is a pixel map composed of 0s and 1s. The 0s symbolize undisplayed areas of the character, while the 1s represent the pixels of the character that will be displayed to the user. You must change each character array in the template font by adding the 1s where you want the pixels to appear to form the character. Below you will see an example of a blank character and the same representation of this character ready for the font.

The code below shows the bitmap representation of the hash sign (#) in the font [model](https://github.com/hexagonix/xfnt/blob/main/modelo.txt) and the implementation in the font [aurora](https: //github.com/hexagonix/xfnt/blob/main/aurora.asm).

```assembly

;; Blank representation of the hash (#) character in the template

.quill:

db 00000000b ;; Two pixels high for characters above the letter
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
db 00000000b ;; Five pixels tall for characters above the letter
db 00000000b
db 00000000b
db 00000000b
db 00000000b

;; Blank representation of the hash character (#), in aurora font

.quill:

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

You should already be able to see, inside the matrix, the hash mark, marked by the presence of 1s.

You can develop as many fonts as you like and play around with different and innovative designs, as well as extend the available characters (using the ASCII reference).

</div>

<div align="center">
   
[![Hexagonix-xfnt](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=xfnt&theme=dark)](https://github.com/hexagonix/xfnt)

</div>
