---
title: Habilitando novamente o suspend no Omarchy-Menu
date: 2026-01-11
draft: false
tags:
  - tutorial
  - Linux
  - Omarchy
  - bash
---
# Primeiro o que aconteceu com o botão Suspend? 
![Image Description](/cblog/images/Pasted%20image%2020260102172909.png)
Para quem utiliza versões anteriores percebeu que no commit em 8 de dezembro na atualização para a versão [v3.2.3](https://github.com/basecamp/omarchy/releases/tag/v3.2.3)
![Image Description](/cblog/images/Pasted%20image%2020260101211929.png)
Foi retirado o menu para suspender o que gerou bastante comentários da comunidade como é possível ver nos comentários do corpo do [commit](https://github.com/basecamp/omarchy/commit/fc04525f032656ceb81f10045ae702e00356e8c5) 
![Image Description](/cblog/images/Pasted%20image%2020260101212148.png)
E abaixo vemos a solução do DHH sobre adicionar uma bind para suspender o que eu achei valido, mas é ruim tirar hábitos depois de 2 versões usando o Omarchy Menu -> System -> Suspend.
```bash
bindd = SUPER CTRL, X, Suspend computer, exec, systemctl suspend
```
![Image Description](/cblog/images/Pasted%20image%2020260101212644.png)
Porem como eu gosto muito de modificar essa distro arch que tem uma grande comunidade e talvez chegue a novos usuários é uma porta de entrada para quem cansou do windows e que aprender mais de desenvolvimento de software ( mesmo não tendo mercado ).
Mas como psicólogos e vendedor de curso nós temos que buscar hobbies, eu gosto de pegar distro linux e ir brincando e organizando do meu jeito. 
## Então como voltar com o menu Suspend? 
![Image Description](/cblog/images/Pasted%20image%2020260102171559.png)
Usando o commit de base podemos saber o PATH de nosso arquivo Shell-Script que nomeado a pasta bin pois ele é um script que faz apenas uma função.
![Image Description](/cblog/images/Pasted%20image%2020260102171833.png)
Como na instalação do omarchy o repo é clonado dentro da pasta share e lá achamos o dir bin e podemos abrir ele em nosso editor de código.
![Image Description](/cblog/images/Pasted%20image%2020260102172046.png)
Aonde podemos comparar com o codigo fonte no github
![Image Description](/cblog/images/Pasted%20image%2020260102172124.png)
vamos apenas 
o que foi removido em nosso arquivo local.
Basicamente colocamos a *String* "Suspend" novamente e um case para *Suspend* que roda o comando **systemctl** suspend que usamos normalmente pela CLI.
e nosso arquivo fica assim bem simples né? 
![Image Description](/cblog/images/Pasted%20image%2020260102172513.png)
## Finalizando
![Image Description](/cblog/images/Pasted%20image%2020260102172625.png)
Agora temos nossa opção de menu Suspend novamente. 
Bom nesse tutorial vemos um pouco primeiro sobre versionamento de uma aplicação e no seu código fonte. Como vemos a força do Linux, Distros Opensource aonde podemos fazer edições customizadas se não gostar de como o [criador]() está dirigindo a OS.
Eu acho que o DDH usa mais desktop por isso suspend pode não ser tão necessario quanto Eu usuario de notebook.  