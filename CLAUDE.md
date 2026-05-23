# APEX Landing Page — Contexto do Projeto

## Visão Geral
Landing page premium para Personal Trainer / Clube de Treinamento de elite.
Nome fictício da marca: **APEX.**
Stack: HTML + CSS + Vanilla JS (sem framework). Tudo em arquivo único `index.html` + assets.

## Repositório
- GitHub: `gfloliveira/Aura-LP-PT-CT`
- Branch de trabalho: `claude/lp-ct-KMlrW`
- Branch local: `claude/oi-KMlrW` (tracking → `origin/claude/lp-ct-KMlrW`)
- Push sempre com: `git push origin claude/oi-KMlrW:claude/lp-ct-KMlrW`
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

### 🔲 Sessões Pendentes (escopo original do projeto)

**Sessão 02 — A Filosofia (Manifesto do Treinador)**
- Layout minimalista, foto P&B do treinador em alto contraste
- Efeito scrub GSAP: texto do manifesto revela palavra por palavra conforme scroll
- Opacidade 20% → 100% conforme entra no centro da tela

**Sessão 03 — Programas de Treinamento (Horizontal Scroll)**
- Cartões: Hipertrofia / Resistência / Elite
- Scroll vertical converte em horizontal (seção pinned via ScrollTrigger)
- Efeito glow/neon laranja ao focar no cartão

**Sessão 04 — Resultados / Prova Social (Galeria)**
- Slider infinito e suave de depoimentos ou antes/depois
- Parallax suave nas imagens dentro dos cartões

**Sessão 05 — Footer e CTA Final**
- Título colossal "COMECE AGORA"
- Mask reveal: footer parece ser desocultado pela seção anterior
- Links sociais com hover de sublinhado animado

---

## Decisões Técnicas Importantes

- **`cursor: none`** apenas em `@media(hover:hover)and(pointer:fine)` — não quebra mobile
- **`min-height: 100dvh`** com fallback `100vh` para address bar dinâmica em mobile
- **Canvas do flow field** começa a rodar imediatamente (antes do loader fechar) — quando a hero aparece o campo já está vivo
- **Flood-fill** usa fila iterativa (não recursiva) para evitar stack overflow em imagens grandes
- **`lenis.stop()`** durante menu mobile aberto, `lenis.start()` ao fechar
- Para trocar a foto do atleta: substituir `src="athlete.png"` em `#athleteRaw` — o canvas processing e o reveal funcionam automaticamente

## Próxima Ação Sugerida
Iniciar a **Sessão 02 (Manifesto)** com scroll-triggered text reveal usando GSAP ScrollTrigger scrub.
