---
title: Explorando buffer overflow em C
date: 2026-01-14
draft: false
tags:
  - C
  - Linux
  - Hacking
  - tutorial
---
# A velha guarda antes dos GC
Em tempos de [GC](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)) nos anos 80/90 código em alto nível como C sempre era um problema se o programador não souber o que estava fazendo com a integridade dos dados pois é crucial a necessidade de alocar da maneira mais segura possível e com [tradeoff](https://pt.wikipedia.org/wiki/Trade-off) bem afiado para não desenvolver um monstro e deixar tudo a cargo do compilador que eventualmente irá deixar seu binário de aplicação lerdo. 
# Explorando Buffer Overflow 0x320
![Image Description](/cblog/images/Pasted%20image%2020260114165629.png)
Estou indo para a 3 vez em que leio esse [livro](https://github.com/imrk51/hacking-books/blob/master/Jon%20Erickson%20-%20Hacking%20Art%20of%20Exploitation.pdf) ele é antigo eu gosto mais dele por como ele se aprofunda em programação C, ASM, ShellScript do que realmente hacking. 
No capitulo 0x320 somos introduzido ao exemplo:
```clike
#include <stdio.h>
#include <string.h>

int main(int argc, char *argv[]) {
  int value = 5;
  char primeiro_buffer[8], segundo_buffer[8];

  strcpy(primeiro_buffer, "um"); /* Coloca "one" em primeiro_buffer. */
  strcpy(segundo_buffer, "dois"); /* Coloca "two" em segundo_buffer. */

  printf("[BEFORE] segundo_buffer está em %p e contém '%s'\n", segundo_buffer, segundo_buffer);
  printf("[BEFORE] primeiro_buffer está em %p e contém '%s'\n", primeiro_buffer, primeiro_buffer);
  printf("[BEFORE] value está em %p e é %d (0x%08x)\n", &value, value, value);

  printf("\n[STRCPY] copiando %d bytes para segundo_buffer\n\n", strlen(argv[1]));
  strcpy(segundo_buffer, argv[1]); /* Copia o primeiro argumento para segundo_buffer. */

  printf("[AFTER] segundo_buffer está em %p e contém '%s'\n", segundo_buffer, segundo_buffer);
  printf("[AFTER] primeiro_buffer está em %p e contém '%s'\n", primeiro_buffer, primeiro_buffer);
  printf("[AFTER] value está em %p e é %d (0x%08x)\n", &value, value, value);
}
```
Vamos destrinchar o código  por partes:
![Image Description](/cblog/images/Pasted%20image%2020260114172148.png)
- Aqui nós iniciamos uma variável value um inteiro com valor 5.
- E aqui o mais importante abrimos dois arrays de Strings-buffers com tamanho fixo de 8.
Logo se o tamanho do array é 8 então abaixo podemos colocar strings dentro deles podem até ser maior como na foto usamos a lib strcpy ( não recomendada ) e atribuímos Strings ao nosso buffer
![Image Description](/cblog/images/Pasted%20image%2020260114172825.png)
Agora vamos para o meio do código:
![Image Description](/cblog/images/Pasted%20image%2020260114173847.png)
Para simplificar o exemplo usa printf *hardcoded* para ser prático mas é possível ver tudo isso pelo GBD. Vamos as legendas
- VERMELHO: **%p** esse parâmetro no printf mostra aonde está o ponteiro/endereço de memoria.
- VERDE: **%s** esse parâmetro mostra Strings. E por ultimo vemos aquele inteiro de valor 5 só de referencia.
Se a gente rodar o código com os parâmetros 123456789 é assim que fica o output:
![Image Description](/cblog/images/Pasted%20image%2020260114174612.png)
Aqui nada fora do normal o segundo buffer está no ponteiro com final **==69c==** com a string 'abcdefg' e o primeiro buffer está no ponteiro com final ==6a4== com a string 'um'. 
( Lembre desse valores vamos voltar neles )
*Não é uma boa prática se referencia apenas pelo final do ponteiro mas para ser didático e como eles são do mesmo tipo de dados não vai ter muita diferença no endereço só o final.*
![Image Description](/cblog/images/Pasted%20image%2020260114175458.png)
Agora vamos para a ~~sacanagem~~ primeiro é um print usando strlen para contar quantos parâmetros tem nosso argv que é passado na hora de executar o código. No nosso caso é '1234567890' 10 espaços.
![Image Description](/cblog/images/Pasted%20image%2020260114180123.png)
E embaixo usando strcpy *injetamos* os 10 itens para um array/buffer de apenas 8 espaços, e nos perguntamos e agora o que acontece?? ( Vamos ver na proxima foto )
Antes de mostrar o resto do codigo e o resultado nós temos que pensar como se fosse os anos 80/90 aonde não existe um GC inteligente que não deixe você rodar um codigo assim ou no caso de linguagens interpretadas ~~python~~ quebrar a execução sem terminar a execução.
![Image Description](/cblog/images/Pasted%20image%2020260114180024.png)
Aqui é o finalzinho do código nada novo é só para nós conseguirmos ver as alterações nos valores do ponteiro.
Olha como seria uma execução normal respeitando o tamanho do array.
![Image Description](/cblog/images/Pasted%20image%2020260114180551.png)
O valor continua normalmente no primeiro buffer e atribuímos o valor do tamanho normal ao segundo buffer.
![Image Description](/cblog/images/Pasted%20image%2020260114180308.png)
Agora olha que incrível se a gente atribuir 10 bytes ao segundo buffer sobra os 2 bytes que literalmente sobrescreve o próximo ponteiro em memoria que pela ordem é o primeiro buffer e assim de valor string 'um' se torna '90'. 
## Aqui vemos como um "BUG" de um usuário mal intencionado ou apenas mal informado pode corromper não só a execução do seu programa como também pode até sobrescrever dados.
*Imagina se a gente conseguisse fazer isso no banco e adicionar 90$ a uma conta com valor 0$.* Eu acredito que bancos não funcionam assim, mas eu lembro que no livro mais para frente temos um exemplo estourando buffer e criando créditos de um script em C de caça-níquel que eu achei muito dahora e talvez eu traga para o blog. ( Depende do engajamento =! esforço )
## Código final e comandos para compilação e execução.
https://github.com/carlinhoshk/Exemplo-de-Buffer-Overflow
