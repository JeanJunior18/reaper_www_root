# VS - Jean

Controle remoto do REAPER pelo celular, feito para tocar VS (playback) ao vivo: transporte, mapa das regiões da música, saltos sincronizados no tempo, loop de região, BPM e mixer das tracks.

É uma única página (`vs-jean.html`) servida pela interface web do próprio REAPER. Ela usa o `main.js` que já vem com o REAPER — não precisa instalar nada além disso.

---

## Sumário

1. [Instalação](#1-instalação)
2. [Configurando o REAPER](#2-configurando-o-reaper)
   - [Smooth seek (obrigatório para os saltos)](#21-smooth-seek-obrigatório-para-os-saltos)
   - [BPM e grade do projeto](#22-bpm-e-grade-do-projeto)
   - [Regiões](#23-regiões)
   - [Projetos e marcadores](#24-projetos-e-marcadores)
3. [Funções da tela](#3-funções-da-tela)
4. [Como usar no show](#4-como-usar-no-show)
5. [Limitações conhecidas](#5-limitações-conhecidas)

---

## 1. Instalação

1. Coloque `vs-jean.html` na pasta de páginas web do REAPER:
   `%APPDATA%\REAPER\reaper_www_root\`
2. No REAPER: **Options → Preferences → Control/OSC/web → Add**.
   - **Control surface mode:** `Web browser interface`
   - **Port:** por exemplo `8080`
   - **Default interface:** `vs-jean.html`
3. No celular (na mesma rede Wi-Fi do computador), abra o endereço que o REAPER mostra nessa janela, por exemplo `http://192.168.0.10:8080`.
4. Dica: use **"Adicionar à tela inicial"** no navegador do celular. A página abre em tela cheia, sem barra de endereço.

> Se depois de atualizar o arquivo o celular continuar mostrando a versão antiga, feche a aba e abra de novo (ou limpe o cache do navegador).

---

## 2. Configurando o REAPER

### 2.1 Smooth seek (obrigatório para os saltos)

É isso que faz os saltos entre regiões caírem no tempo certo, sem atraso de rede.

**Options → Preferences → Audio → Playback → Seek settings:**

- ☑ **Do not change playback position immediately when seeking (smooth seek)**
  - 🔘 **Play to end of `1` more measures before seeking** ← esta
  - ⚪ Play to next project marker, end of current region… ← **não** esta

Com isso, qualquer salto pedido durante a reprodução acontece na virada do compasso. A página cuida de esperar o fim da região quando é isso que você quer (veja [Mapa de regiões](#mapa-de-regiões)).

> Se o salto "no fim da região" acontecer **um compasso depois** do esperado, troque o `1` por `0`.

### 2.2 BPM e grade do projeto

Os saltos no tempo, o BPM mostrado na tela e a contagem de compassos dependem de a **grade do REAPER estar alinhada com a música**.

1. **Defina o andamento e a fórmula de compasso** em **File → Project settings** (`Alt+Enter`) → *Project BPM* e *Time signature*.
   Ou dê duplo clique no BPM no painel de transporte.
2. **Alinhe o início da música ao compasso 1.** O primeiro tempo forte do áudio precisa cair exatamente numa linha de compasso.
   Se o áudio tiver um silêncio ou contagem antes, mova o item até que o tempo 1 da música coincida com uma linha de compasso.
3. **Confira com o metrônomo** (botão *Metrônomo* no menu ☰ da página, ou no REAPER): ele deve bater junto com a música do começo ao fim.
4. **Música com variação de andamento:** insira marcadores de tempo (*Insert → Tempo/time signature change marker*, atalho `Shift+C`) nos pontos em que o andamento ou a fórmula de compasso mudam.

> Não sabe o BPM? Selecione exatamente alguns compassos da música no áudio (do tempo 1 de um compasso ao tempo 1 de outro) e procure na lista de ações (`?`) por **"tempo from time selection"**: o REAPER calcula o BPM a partir de quantos tempos/compassos a seleção tem.

### 2.3 Regiões

As regiões são o coração da página: viram o mapa da música, o "a seguir", os saltos e o loop.

1. **Ative o snap** (botão *Snap* no menu ☰ da página, ou o ícone de ímã no REAPER) para que as seleções grudem na grade.
2. Para cada parte da música (Intro, Verso, Refrão, Interlúdio, Final…):
   1. Selecione o trecho na régua, **do início de um compasso ao início de outro**.
   2. **Insert → Region from time selection** (`Shift+R`).
   3. Dê duplo clique na região para **dar um nome** e **escolher uma cor**. O nome e a cor aparecem no celular.
3. **Regiões devem começar e terminar em linhas de compasso.** Se uma região terminar no meio de um compasso, o salto "no fim da região" acontece só na virada do compasso seguinte.
4. Evite deixar buracos entre as regiões: fora de qualquer região a página mostra "fora de região" e o botão LOOP fica desativado.
5. Nomes curtos ficam mais legíveis no mapa (cada bloco tem no mínimo 72 px).

### 2.4 Projetos e marcadores

- **Uma música por aba de projeto.** Os botões ◀ Projeto / Projeto ▶ trocam entre as abas abertas no REAPER — abra o repertório na ordem do show.
- **Marcadores** (`M`) servem para pontos de referência dentro das regiões (ex.: "entra voz", "breque"). Eles aparecem no painel de marcadores e os botões ◀ ▶ param neles.

---

## 3. Funções da tela

### Topo

| Elemento | O que faz |
|---|---|
| **☰** | Abre/fecha o menu de opções: *Undo*, *Redo*, *Metrônomo*, *Snap*, *Limpar clip* (limpa os indicadores de pico no REAPER). Metrônomo e Snap acendem quando ligados. |
| **Relógio** | Posição atual, no formato da régua do REAPER. Fica vermelho durante a gravação. **Toque** para digitar uma posição e ir até ela (desativado durante a gravação). |
| **BPM** | Andamento atual do projeto (veja [Limitações](#5-limitações-conhecidas)). |
| **Compasso** | Fórmula de compasso (ex.: 4/4). |
| **Luzes de tempo** | Uma luz por tempo do compasso; a acesa é o tempo atual. |

### Projetos

| Botão | O que faz |
|---|---|
| **◀ Projeto** / **Projeto ▶** | Vai para a aba de projeto anterior/seguinte no REAPER. |

### Mapa de regiões

| Elemento | O que faz |
|---|---|
| **Faixa fina (visão geral)** | A música inteira em proporção real, com a posição atual. Só aparece quando o mapa não cabe na tela. |
| **Mapa** | Uma caixa por região, com nome e cor. A região atual fica destacada e uma linha branca mostra a posição. Com muitas regiões o mapa rola sozinho acompanhando a música; você pode arrastar com o dedo para olhar (ele volta a acompanhar após 3 s). |
| **Nome da região atual** | Em destaque, com a cor da região e o **tempo que falta** para ela acabar (ex.: `−0:23`). |
| **Barra de progresso** | Quanto da região atual já passou. |
| **A seguir** | A próxima região e em quanto tempo ela começa. Fica amarelo nos últimos 8 s. |
| **⟳ LOOP** | Coloca a região atual em loop (seleção na região + repeat ligado). O botão fica azul e o bloco ganha uma faixa azul embaixo. Toque de novo para sair do loop. Usa a ação `43102` do REAPER. |

**Saltos pelo mapa — tocar numa região:**

| Situação | Resultado |
|---|---|
| Música **parada** | Vai direto para o início da região. |
| Tocando, **1 toque** | A região fica na fila (borda amarela). A música termina a região atual e então salta. O "a seguir" mostra: *"Refrão no fim da região (0:14) · toque de novo: próximo compasso"*. Tocar em outra região troca a escolha. |
| Tocando, **2º toque** na mesma região | Salta na **virada do próximo compasso** — para corrigir na hora quando alguém entrou errado. |
| Gravando | Desativado, para evitar saltos acidentais. |

### Marcadores

| Elemento | O que faz |
|---|---|
| **◀ / ▶** | Vai para o marcador (ou início/fim de região) anterior/seguinte. Com o smooth seek, o salto acontece na virada do compasso. |
| **Três células** | Marcador anterior, o que está na posição atual e o próximo (H = início do projeto, E = fim). |
| **+ marcador** | Aparece quando não há marcador na posição atual; cria um ali. |

### Transporte

| Botão | O que faz |
|---|---|
| **■ Stop** | Para. |
| **▶ Play** | Toca (fica verde). |
| **❚❚ Pause** | Pausa. |
| **Repeat** | Liga/desliga o repeat do REAPER. |
| **● Rec** | Grava. Se nenhuma track estiver armada, pede confirmação. |
| **⏮ Início** | Volta ao início do projeto. |

### Tracks

| Elemento | O que faz |
|---|---|
| **Cabeçalho** | Nome, número e volume (dB) da track, na cor da track. Tracks-pasta mostram um ícone de pasta. **R** acende se a track recebe envios; **S** se ela envia. |
| **● (arm)** | Arma/desarma a track para gravação. |
| **M / S** | Mute e solo. Quando a track está armada, o M dá lugar ao **MON** (monitoramento: MON / AUTO). |
| **Tocar no nome** | Abre a track: aparece o **fader de volume** e os **envios** (cada um com seu fader e mute). Os faders só se movem arrastando a "cabeça", para não mudar volume sem querer ao rolar a tela. |
| **MASTER** | Volume e mute do master. |

### Layout

- **Celular em pé:** tudo em uma coluna.
- **Tablet/celular deitado (≥ 768 px):** controles à esquerda, tracks à direita.

---

## 4. Como usar no show

Checklist antes de cada música:

- [ ] BPM e fórmula de compasso conferidos com o metrônomo
- [ ] Música começando numa linha de compasso
- [ ] Regiões nomeadas, coloridas e alinhadas aos compassos
- [ ] Smooth seek em **"Play to end of 1 more measures"**
- [ ] Projetos abertos em abas, na ordem do repertório

Situações comuns:

| Quero… | Faça |
|---|---|
| Repetir o interlúdio até o líder dar o sinal | Durante o interlúdio, toque **LOOP**. No sinal, toque numa região à frente (1 toque) e depois **LOOP** de novo para sair do loop. |
| Pular para o refrão final quando a parte atual acabar | **1 toque** no refrão final. |
| Corrigir porque o vocalista entrou errado | **2 toques** na região certa — salta no próximo compasso. |
| Ir para a próxima música | **Projeto ▶**. |

---

## 5. Limitações conhecidas

- **BPM é medido, não lido.** A interface web do REAPER não informa o andamento; a página calcula a partir da posição em segundos e em tempos.
  - Tocando: atualiza a cada ~0,5 s e acompanha mudanças de andamento.
  - Parado (antes do primeiro play): mostra a média desde o início — exata só se o andamento for fixo. No ponto 0:00 aparece `--`.
  - Em compassos compostos (6/8, 12/8) o valor pode aparecer em dobro, dependendo de como o projeto conta os tempos.
- **Regiões e marcadores** criados ou editados direto no REAPER levam até 2 s para aparecer no celular (trocar de projeto ou criar marcador pela página atualiza na hora).
- **Tracks escondidas no REAPER** continuam aparecendo na página (o sinal que o REAPER manda para isso não é confiável — fazia tracks-pasta piscarem durante o play).
- **Rede:** a página consulta o REAPER 25×/s para o relógio, 4×/s para as tracks, 1×/s para metrônomo/snap e a cada 2 s para regiões/marcadores. Toques são enviados em até ~20 ms.

---

## ☕ Apoie este projeto

Desenvolvi esta ferramenta a partir de uma necessidade real da minha própria igreja, com o objetivo de facilitar o trabalho de quem opera playbacks durante cultos e apresentações.

Agora, compartilho gratuitamente para abençoar outras igrejas.

Se o projeto te ajudou, considere pagar um café ao desenvolvedor. Sua contribuição incentiva a manutenção e a criação de novas ferramentas.

**Toda contribuição é bem-vinda!**

**Pix:** `pix@asherlabs.com.br`

---

Desenvolvido por **Jean** · [@asher.labs](https://www.instagram.com/asher.labs)
