# Contexto do projeto: site do Naya Ramp — LigaFest 2026

## O que é isso

Cauê joga Naya Ramp (RGW) na final Pauper do Circuito LigaMagic, no LigaFest 2026 (11-12 de julho, Shopping Frei Caneca, São Paulo). Esse projeto é um site de referência rápida (deck list + estudo de metagame + guia de sideboard) pra consultar durante o torneio, construído a partir de uma sessão de preparação no Claude (Cowork).

## Arquivos já existentes nesta pasta

- `naya_ramp_guia.html` — o site em si. Arquivo único (HTML+CSS+JS inline, sem build, sem dependência externa). Tem 3 abas: Metagame (tabela de %), Lista (deck completo), Sideboard (guia com busca em tempo real por arquétipo). Os dados do sideboard estão num array JS (`const data = [...]`) dentro do próprio arquivo — pra editar um matchup, é só editar esse array.
- `guia_sideboard_naya_ramp_ligafest.md` — versão markdown do mesmo conteúdo (mais fácil de ler/editar em texto puro, foi a fonte original antes de virar HTML).

## O que fazer a partir daqui

Continue evoluindo `naya_ramp_guia.html` como ferramenta viva durante o fim de semana de torneio. Coisas plausíveis pra fazer:
- Adicionar uma seção de "log de partidas" (rodada, adversário, resultado, o que deu certo/errado) — Cauê vai reportar resultado partida a partida.
- Refinar o guia de sideboard conforme adversários reais aparecerem (o guia atual é baseado em arquétipos do metagame, não em oponentes específicos já enfrentados).
- Melhorar UI/UX pra uso rápido no celular entre rodadas (o site já é mobile-friendly, mas dá pra iterar).
- Se pedirem pra publicar online (não só arquivo local), é só HTML/CSS/JS puro — sobe direto em GitHub Pages, Netlify ou Vercel sem nenhuma mudança.

## A lista (60 cartas, 17 terrenos)

**Criaturas (26):** 4 Arbor Elf, 4 Avenging Hunter, 2 Boarding Party, 4 Eldrazi Repurposer, 4 Jewel Thief, 2 Nyxborn Hydra, 2 Sagu Wildling, 4 Writhing Chrysalis
**Encantamentos (9):** 2 Armadillo Cloak, 4 Utopia Sprawl, 3 Wild Growth
**Instantâneos (4):** 1 Pulse of Murasa, 3 Thraben Charm
**Feitiços (4):** 4 Malevolent Rumble
**Terrenos (17):** 13 Forest, 2 Mountain, 2 Plains

**Sideboard (15):** 1 Ancient Grudge, 3 Breath Weapon, 3 Deglamer, 1 Flaring Pain, 2 Ram Through, 2 Tamiyo's Safekeeping, 3 Weather the Storm

## Decisões de deckbuilding e por quê (importante pra não desfazer sem querer)

- **Naya, não Gruul puro:** confirmado como arquétipo real via página de arquétipos do MTGGoldfish (21 listas de torneio rastreadas). O splash branco (Armadillo Cloak + Thraben Charm) veio pra resolver o problema real do jogador: perder pra decks vermelhos rápidos (Mono Red Madness).
- **Terrenos 13 Forest / 2 Mountain / 2 Plains (17 total), não cortar Forest:** simulação Monte Carlo (Python, 400-500k mãos) mostrou que cortar 2 Forest custa ~2 pontos percentuais de chance de jogada no turno 1. A correção certa foi ACRESCENTAR um terreno (16→17) mantendo os 13 Forest intactos, em vez de trocar Forest por Mountain/Plains. Essa config bate ou supera levemente o Gruul Ramp tradicional em "chance de conseguir 3 mana no turno 2".
- **3 Wild Growth, não 4:** bate com a média real de listas de torneio de Naya Ramp (~2.5). A 4ª cópia é upgrade de velocidade pura, mas custa um slot de carta que ajudaria mais a consistência de cor.
- **Pegadinha de regra que gerou bug de simulação:** Arbor Elf, Utopia Sprawl E Wild Growth custam `{G}` pra serem lançados — Forest é a ÚNICA fonte verde do deck. Uma versão inicial da simulação esqueceu que Wild Growth também precisa de Forest disponível (não só "qualquer terreno"). Se for mexer na base de mana de novo, sempre exigir Forest ≥1 como pré-requisito pros três aceleradores, não só pros que "enchant Forest" explicitamente.

## Regra de ouro do guia de sideboard (não repetir o erro)

Todo swap de sideboard deve ser derivado de "esse oponente está me correndo (dano) ou me travando (recursos/remoção)?" — nunca de heurística genérica tipo "aura arrisca 2-por-1" ou "corte o topo de curva contra qualquer coisa".

- **Erro já cometido e corrigido:** cortar Armadillo Cloak contra Mono Red Madness. Errado — o Cloak (trample + ganha vida igual ao dano causado) é a resposta certa contra um deck que tenta te correr até zero, porque dá ganho de vida recorrente a cada combate. Ele fica dentro em toda matchup de corrida (Madness, Rally, Rakdos Madness, White Aggro, Boros Bully, Burn).
- **Erro já cometido e corrigido:** cortar Boarding Party contra Caw-Gates (deck de controle). Errado — contra controle você quer mais vantagem de carta e ameaças resilientes (o Cascade do Boarding Party já é 2-por-1 de graça), não menos.
- Detalhe útil: Breath Weapon ("2 dano a cada criatura não-Dragão") não acerta Avenging Hunter nem Sagu Wildling — os dois são do tipo Dragão. Pode limpar o board com eles em jogo sem medo de fogo amigo.

## Log de partidas do torneio (até agora)

- R1: perdeu 0-2 vs Mono Red Madness. Maior dificuldade recorrente do jogador — decks agressivos rápidos que correm antes do ramp estabilizar.
- R2: perdeu 1-2 vs Ruby Storm (combo, pode matar no turno 3). Sem interação real no sideboard atual pra essa matchup — plano é pressão rápida e mulligan agressivo.

## Estudo de metagame (fontes)

O guia combina % global do metagame (MTGGoldfish, dados de ~2 meses, julho 2026) com tendências específicas do tabletop brasileiro (LigaMagic, análise de 3 eventos: Nacional Pauper 80 jogadores, Liga Gaúcha de Pauper 125 jogadores, Paupergeddon Itália 951 jogadores como referência de evento grande). Achado principal: Mono Red domina o tabletop brasileiro (>25% em um evento), Blue Terror venceu a Liga Gaúcha, Caw-Gates venceu 2 dos 3 eventos analisados, e o espelho Gruul/Naya Ramp aparece Top 3-5 com frequência bem maior nas finais grandes do Brasil do que a % global (0.7%) sugere — vale se preparar mais pra ele do que a estatística pura indicaria.

Fontes originais: [MTGGoldfish metagame](https://www.mtggoldfish.com/metagame/pauper/full), [LigaMagic — Análise do Metagame Tabletop](https://www.ligamagic.com.br/?view=artigos/view&aid=5596), [LigaMagic — Campeões do Circuito 15ª Edição](https://www.ligamagic.com.br/?view=artigos/view&aid=5616).

## Como abrir isso no Claude Code

Na pasta do projeto (`Preparação CLM`), rode `claude` no terminal. O Claude Code lê este `CLAUDE.md` automaticamente ao iniciar, então já vai ter todo esse contexto sem precisar copiar/colar nada.
