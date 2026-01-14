---
title: Instalando tmux em busca da performance
date: 2026-01-12
draft: true
tags:
  - bash
  - Linux
  - tmux
  - shellscript
---

# O que é Tmux e porque ele te da um pouco de performance?
![[Pasted image 20260112111819.png]]
Tmux é um emulador de terminal open source Unix like. Ele permite você ter vários *terminais* em *instancias* como também pode ter varias *instancias*. o Tmux não se resume a só isso mas vamos focar nessas duas coisas.
Se você utiliza o terminal uma hora ou outra sua necessidade por multiplas janeras irá crescer ou a vontade de ter 1 *workspace/instancia* para uma sessão SSH que nunca desconecta, 2 workspace para seu projeto frontend, 3 workspace para o projeto backend, 4 workspace para debugar um app linux que você use diariamente como discord ou browser... assim por diante.
![[Pasted image 20260112112702.png]]  
Nesse exemplo da [documentação](https://github.com/tmux/tmux/wiki/Getting-Started) official vemos como é um ambiente tmux outside terminal seria 
- outside terminal: O primeiro terminal base da sua maquina, assim que podemos colocar um parametro na config zshrc para o tmux abrir automaticamente veremos mais a frente. 
- active pane border: É a sinalização em qual **pane** você está, ele fica da cor verde ou cinza
- pane: É seu terminal atual, você pode abrir outra workspace/Windows ou pode abrir varias panes
- 