---
title: Instalando tmux aprendendo basico
date: 2026-01-12
draft: false
tags:
  - bash
  - Linux
  - tmux
  - shellscript
---

# O que é Tmux

![Image Description](/cblog/images/Pasted%20image%2020260112111819.png)
Tmux é um emulador de terminal open source Unix like. Ele permite você ter vários _terminais_ em _instancias_ e _servers_.
o Tmux não se resume a só isso mas vamos focar nessas duas coisas.

# Porque usar?

Se você utiliza o terminal uma hora ou outra sua necessidade por múltiplas janelas irá crescer ou a vontade de ter

1. Workspace/instancia para uma sessão SSH que nunca desconecta,
2. Workspace para seu projeto frontend,
3. Workspace para o projeto backend,
4. Workspace para debugar um app linux que você use diariamente como discord ou browser...

# Um pouco sobre a interface

![Image Description](/cblog/images/Pasted%20image%2020260112112702.png)  
Não vamos se aprofundar muito para não ficar massivo de ler
Nesse exemplo da [documentação](https://github.com/tmux/tmux/wiki/Getting-Started) official vemos como é a interface tmux são eles:

- **outside terminal**: O primeiro terminal base da sua maquina, assim que podemos colocar um parâmetro na config zshrc para o tmux abrir automaticamente veremos mais a frente.
- **active pane border**: É a sinalização em qual **pane** você está, ele fica da cor verde ou cinza
- **pane**: É seu terminal atual, você pode abrir outra workspace/Windows ou pode abrir outras panes ao lado
- **status line**: É o "menu" de operações do seu tmux, ele muda de cor quando e avisa quantidade de workspaces, hora e data e é altamente configurável utilizando plugins e scripts.

# Criando nosso primeiro workspace e abrindo múltiplos panes

Logo após instalar o tmux via seu packager manager:

```bash
sudo pacman -Sy tmux  // ou // sudo apt-get install -y tmux
```

e rodar o comando "tmux" no terminal você irá dar de cara com a interface vazia só com o seu **status line**.
![Image Description](/cblog/images/Pasted%20image%2020260115194425.png)
Aqui vai uma pequena lista dos comandos para decorar de agora, usando o comando baseado na documentação ou use a bind "Control-b ?".

```bash
tmux lsk -N|morey
```

![Image Description](/cblog/images/Pasted%20image%2020260115194634.png)
Primeiros vemos a bind C-B essa é nosso [PREFIX](https://www.google.com/search?client=firefox-b-d&channel=entpr&q=tmux+prefix), então como visto toda bind preceder por C-b logo é necessário sempre digitar o PREFIX e depois sua opção.
Vamos Splitar a window/pane em vertical com a combinação:

```bash
PREFIX(CONTROL+B) + "
```

![Image Description](/cblog/images/Pasted%20image%2020260115201229.png)
Depois podemos quebrar na horizontal usando

```bash
PREFIX(CONTROL+B) + %
```

![Image Description](/cblog/images/Pasted%20image%2020260115201348.png)
se quiser a pane fechar é só usar

```bash
PREFIX(CONTROL+B) + x
```

![Image Description](/cblog/images/Pasted%20image%2020260115201716.png)
E vemos a cor da barra mudando para a confirmação se você quer matar o pane 2 e você pode confirmar digitando Y e dando enter.
![Image Description](/cblog/images/Pasted%20image%2020260115202203.png)
Bom nesse pequeno tutorial vemos os primeiros passos no tmux de uma forma prática e mais rápida sem entrar em assuntos complexos.
Com o engajamento do meu (blog =! esforço ) futuramente eu possa trazer meus dotfiles com a minha config baseado no [oh my tmux](https://github.com/gpakosz/.tmux).
Eu mesmo acho bem complexa e com configurações muito longas... porem o resultado de um bom Setup muda completamente como programar e navegar.

