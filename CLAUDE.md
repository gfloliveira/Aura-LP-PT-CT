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
- Desktop: 3 cards `position:sticky; top:0; height:100vh` em container `300vh`
- Mobile: cards verticais com ScrollTrigger por card

**Componentes construídos:**
- Intro animada (eyebrow + título outline + descrição, stagger via ScrollTrigger)
- **Layout: Full Viewport Stacked** — cada card é `position:sticky; top:0; height:100vh`
- 3 cards empilhados com z-index crescente (01→1, 02→2, 03→3)
- Card seguinte desliza por cima do anterior conforme scroll (efeito cover natural)
- `.prog-card-inner` recebe `scale(0.96) + brightness(0.65)` via GSAP scrub enquanto é coberto
- Conteúdo de cada card anima na entrada: `translateX → 0` (nível), `translateY → 0` (título), stagger nos stats/intensidade/CTA
- Card Elite: badge "Exclusivo" com dot pulsante, gradiente mais intenso
- Contador `01 / 03` fixo no canto superior direito de cada card
- Número watermark gigante no canto inferior direito
- Backgrounds reais: `assets/prog-01.png`, `assets/prog-02.png`, `assets/prog-03.png`
- Overlay gradiente: esquerda→transparente (desktop) / topo→baixo (mobile)
- Mobile: `position:relative`, animações por ScrollTrigger (toggleActions play/reverse)

**Imagens em `assets/`:**
- `prog-01.png` — atleta com barra (Hipertrofia)
- `prog-02.png` — atleta correndo (Resistência)
- `prog-03.png` — retrato atleta (Elite)

---

### ✅ Sessão 04 — Resultados / Prova Social (CONCLUÍDA)

**Layout:**
- Intro centralizada + stats strip 4 colunas + marquee duplo
- Mobile: stats grid 2×2, cards menores (280px), animações ajustadas

**Componentes construídos:**
- `.results-intro`: eyebrow com linha, título, descrição — animações ScrollTrigger (opacity + translateY)
- `.results-stats`: 4 contadores animados com count-up via `gsap.to({val:0}, {val:target})`
  - Formato: `toLocaleString('pt-BR')` para números ≥ 1000 (ex: 4.200)
  - Stats: 4.200 alunos / 98% satisfação / 340 transformações / 7 anos
- `.results-marquee`: 2 linhas de depoimentos em loop infinito (CSS `@keyframes`)
  - Linha 1 (`mqLeft`): `translateX(0 → -50%)`, 52s
  - Linha 2 (`mqRight`): `translateX(-50% → 0)`, 48s
  - Hover pausa a animação (`animation-play-state: paused`)
  - 8 depoimentos por linha × 2 duplicatas = loop seamless
- `.test-card`: cards 360px com avatar colorido, nome, resultado em laranja, texto, tag
- Fade-in geral do marquee via ScrollTrigger

**Técnica marquee:**
- HTML duplicado (conteúdo × 2) dentro de `.marquee-track` (width: max-content)
- CSS anima de 0 → -50% (= 1× set original), loop instantâneo cria ilusão de infinito

---

### ✅ Sessão 05 — CTA Final + Footer (CONCLUÍDA)

**Layout:**
- CTA Final: seção centralizada com título colossal + dois botões de ação
- Footer: grid 3 colunas (brand | nav | social), bottom bar com copyright + legal

**Componentes construídos:**
- `.cta-final`: eyebrow com linha animada, título com word-mask reveal (`translateY 110%→0%`)
  - `#ctaWord1` e `#ctaWord2` — cada palavra em `overflow:hidden` com `<span>` interno animado
  - `cta-final-title-word--accent` — palavra "AGORA." em laranja
- Subtítulo e bloco de ações: fade-in em stagger via GSAP timeline + ScrollTrigger
- `.cta-btn-primary`: botão WhatsApp com SVG + seta, efeito magnético no desktop (`padding:28px`)
- `.cta-btn-secondary`: borda sutil com hover reveal
- `.cta-final-glow`: radial gradient laranja central em opacidade mínima
- `.cta-final-noise`: textura noise SVG base64 para grão sutil
- Footer 3 colunas:
  - `.footer-brand`: logo APEX + tagline "Elite Training Club"
  - `.footer-nav`: 4 links com underline animado `::after` scaleX via `width:0→100%`
  - `.footer-social`: Instagram, YouTube, TikTok — ícones SVG inline, hover laranja
- `.footer-bottom`: copyright + links legais (Privacidade / Termos) com underline hover
- Footer aparece com `opacity:0 → 1 + translateY:24→0` via ScrollTrigger
- Mobile: grid 1 coluna, nav flex-wrap horizontal, social align-start

---

### 🔲 Sessões Futuras

**Polimento Global**
- Substituir `athlete.png` por foto real quando disponível
- Ajustar links de nav para scroll âncoras corretas
- Configurar link WhatsApp real no botão CTA
- SEO: meta tags, og:image, favicon

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
- **`filter` em sticky element** quebra z-index stacking — sempre aplicar em `.prog-card-inner`, não em `.prog-card`
- Para trocar a foto do atleta: substituir `src="athlete.png"` em `#athleteRaw` — todo o canvas processing (hero + manifesto) funciona automaticamente

## Próxima Ação Sugerida
Polimento global — substituir foto do atleta, configurar links reais (WhatsApp, redes sociais), scroll âncoras da nav, meta tags SEO.
