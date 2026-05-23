# APEX Landing Page — Contexto do Projeto

## Visão Geral
Landing page premium para Personal Trainer / Clube de Treinamento de elite.
Nome fictício da marca: **APEX.**
Stack: HTML + CSS + Vanilla JS (sem framework). Tudo em arquivo único `index.html` + assets.

## Repositório
- GitHub: `gfloliveira/Aura-LP-PT-CT`
- Branch de trabalho: `claude/lp-ct-KMlrW`
- Push sempre com: `git push origin HEAD:claude/lp-ct-KMlrW`
- GitHub Pages ativo em: `https://gfloliveira.github.io/Aura-LP-PT-CT/`

## Arquivos do Projeto
```
index.html     — toda a aplicação (CSS + HTML + JS embutidos)
athlete.png    — foto fictícia do atleta (bodybuilder barbudo, fundo branco removido via canvas)
CLAUDE.md      — este arquivo
```

## Design System

### Paleta (variáveis CSS em `:root`)
| Variável | Valor | Uso |
|---|---|---|
| `--color-bg` | `#0A0A0A` | Fundo principal |
| `--color-primary` | `#FF4500` | Laranja vibrante — destaques, CTAs, sparks |
| `--color-text` | `#F2F2F2` | Texto principal |
| `--color-text-secondary` | `rgba(242,242,242,0.5)` | Texto de suporte |
| `--color-text-muted` | `rgba(242,242,242,0.2)` | Labels, metadados |
| `--color-border` | `rgba(242,242,242,0.07)` | Linhas divisórias |

### Tipografia
- **Display / Títulos:** `ClashDisplay` (Fontshare CDN) → fallback `Teko`
- **Corpo / UI:** `Inter` (Google Fonts)

### Bibliotecas (CDN, sem npm)
- **GSAP 3.12.5** + ScrollTrigger — todas as animações
- **Lenis 1.1.14** — smooth scroll

### Breakpoints
| Nome | `max-width` | Mudanças principais |
|---|---|---|
| Tablet | `1100px` | Padding reduzido, fontes menores |
| Mobile | `768px` | Layout vertical, hamburger menu |
| Small mobile | `390px` | Ajustes mínimos de espaçamento |

---

## Estado Atual do Desenvolvimento

### ✅ Sessão 01 — Hero Section (CONCLUÍDA)

**Layout:**
- Desktop: split 54% (conteúdo) / 46% (atleta) em `flex-row`
- Mobile: `flex-column` — painel do atleta no topo (~52vw), conteúdo abaixo

**Componentes construídos:**
- Loader animado (barra de progresso 0→100%, saída `yPercent: -100`)
- Nav fixa com links + botão CTA desktop / hamburger mobile
- Menu mobile overlay (clip-path reveal GSAP, lenis.stop durante abertura)
- Cursor customizado dot + ring (apenas em `hover:hover + pointer:fine`)
- Hero title com char-split stagger (`y: 110% → 0`, `rotateX: -80 → 0`)
- Botão magnético (área de detecção `padding: 28px`, elastic.out no release)
- Parallax de mouse no `hero-bg` e `#athleteVisual`
- Scroll cue animado + badge orbital

**Flow Field (Canvas):**
- 200 partículas desktop / 80 mobile
- Noise sin-based com 3 frequências sobrepostas
- Partículas com trilha comet + 5.5% são sparks laranja com halo glow
- `flowState.convergence` controlado por GSAP durante o reveal:
  - Desktop: converge para `tx = W * 0.77`, `ty = H * 0.52`
  - Mobile: converge para `tx = W * 0.5`, `ty = H * 0.14`

**Atleta (painel direito/topo):**
- Foto: `athlete.png` — bodybuilder barbudo (fictício, substituir futuramente)
- Fundo branco removido via **flood-fill iterativo 4-conectado** em canvas
  - Threshold: RGB > 232 → transparente
  - Segunda passagem: fade alpha em pixels de anti-aliasing (210–255)
- CSS: `filter: contrast(1.1) brightness(0.82) saturate(0.78)`
- Reveal: `clip-path: inset(100% → 0%)` sincronizado após convergência das partículas
- Overlays: spotlight laranja do chão (`.athlete-light`), scanlines, glow ring pulsante
- Fades de borda: left / top / bottom que fundem com o fundo preto

**Touch detection:**
- `window.matchMedia('(hover:none),(pointer:coarse)')` desabilita cursor, magnético e parallax em touch

---

### ✅ Sessão 02 — A Filosofia / Manifesto (CONCLUÍDA)

**Layout:**
- Desktop: grid 46% (foto sticky) / 54% (texto scrollável), `min-height: 220vh`
- Mobile: coluna única — foto no topo (62vw), texto abaixo
- `overflow: clip` no `.manifesto` (não `hidden` — quebraria o `position: sticky`)
- `align-items: start` no grid — obrigatório para sticky funcionar

**Componentes construídos:**
- Foto P&B do treinador: mesma `athlete.png`, processada via canvas com:
  - Flood-fill para remoção de fundo branco
  - Grayscale + S-curve de contraste + darken a 68%
- Coluna da foto sticky (`position: sticky; top: 0; height: 100vh`)
- Reveal da foto: `clip-path: inset(100% 0 0 0 → 0%)` via ScrollTrigger
- Texto do manifesto: palavras em `<span class="m-word">`, scrub GSAP
  - Opacidade 8% → 100% palavra por palavra conforme scroll
- Header, título e assinatura com animações de entrada (opacity + translateY)
- Número gigante de fundo (`02`) em opacidade mínima

**Personagem:** Carlos Mendes — Fundador & Head Coach

---

### ✅ Sessão 03 — Programas de Treinamento (CONCLUÍDA)

**Layout:**
- Desktop: seção pinned com scroll horizontal via GSAP ScrollTrigger
- Mobile: `overflow-x: scroll` nativo com `scroll-snap-type: x mandatory`

**Componentes construídos:**
- Intro animada (eyebrow + título outline + descrição, stagger via ScrollTrigger)
- `.programs-pin-wrap` pinned quando toca o topo da viewport
- `.programs-track` move `x: 0 → -totalMove` com `scrub: 1.2`
  - `totalMove` calculado dinamicamente: `(cards.length - 1) * (cardW + gap)`
  - `end: '+=' + (totalMove + innerHeight * 0.8)`
- **Layout: Full Viewport Stacked** — cada card é `position:sticky; top:0; height:100vh`
- 3 cards empilhados com z-index crescente (01→1, 02→2, 03→3)
- Card seguinte desliza por cima do anterior conforme scroll (efeito cover natural)
- `.prog-card-inner` recebe `scale(0.96) + brightness(0.65)` via GSAP scrub enquanto é coberto
- Conteúdo de cada card anima na entrada: `translateX → 0` (nível), `translateY → 0` (título), stagger nos stats/intensidade/CTA
- Card Elite: badge "Exclusivo" com dot pulsante, gradiente mais intenso
- Contador `01 / 03` fixo no canto superior direito de cada card
- Número watermark gigante no canto inferior direito
- Background: gradientes placeholder (substituir por imagens Gemini/Imagen quando prontas)
- Mobile: `position:relative`, cards empilhados verticalmente, animações desativadas

---

### 🔲 Sessões Pendentes

**Sessão 04 — Resultados / Prova Social (Galeria)**
- Slider infinito e suave de depoimentos ou antes/depois
- Parallax suave nas imagens dentro dos cartões
- Possível uso de imagens geradas via Gemini/Imagen (pré-geradas manualmente)

**Sessão 05 — Footer e CTA Final**
- Título colossal "COMECE AGORA"
- Mask reveal: footer desocultado pela seção anterior
- Links sociais com hover de sublinhado animado

---

## Decisões Técnicas Importantes

- **`cursor: none`** apenas em `@media(hover:hover)and(pointer:fine)` — não quebra mobile
- **`min-height: 100dvh`** com fallback `100vh` para address bar dinâmica em mobile
- **Canvas do flow field** começa a rodar imediatamente (antes do loader fechar)
- **Flood-fill** usa fila iterativa (não recursiva) para evitar stack overflow
- **`lenis.stop()`** durante menu mobile aberto, `lenis.start()` ao fechar
- **`overflow: clip`** (não `hidden`) em seções com `position: sticky` descendente
- **`align-items: start`** obrigatório no grid quando há coluna com `position: sticky`
- **`isMobile`** = `window.innerWidth < 768`, calculado uma vez no load — usado para desabilitar GSAP pin e parallax em touch
- Para trocar a foto do atleta: substituir `src="athlete.png"` em `#athleteRaw` — todo o canvas processing (hero + manifesto) funciona automaticamente

## Próxima Ação Sugerida
Iniciar a **Sessão 04 (Resultados / Prova Social)** — slider de depoimentos com parallax.
Antes: revisar e ajustar a Sessão 03 conforme feedback do usuário.
