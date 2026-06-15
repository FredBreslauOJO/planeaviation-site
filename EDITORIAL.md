# Plane Aviation — Guia de Edição de Conteúdo

Guia para Pedro, Fred e equipe editarem o site sem tocar em CSS ou lógica.

---

## 1. Onde editar cada coisa

### Textos fixos do site (nav, rodapé, botões, títulos de seção)
**Arquivo:** `i18n.js`

Cada texto traduzível tem este formato:
```js
nome_da_chave: { pt:"Texto em português", en:"English text", es:"Texto en español" },
```
Edite apenas os valores entre aspas. **Não mude as chaves** (a parte antes dos dois pontos).

Exemplo — mudar o texto do botão de contato no menu:
```js
// Antes:
nav_cta: { pt:"Falar com um Especialista", en:"Talk to a Specialist", es:"Hablar con un Especialista" },

// Depois:
nav_cta: { pt:"Fale com um Consultor", en:"Talk to a Consultant", es:"Hable con un Consultor" },
```

---

### Textos das páginas (parágrafos, descrições longas)
**Arquivo:** o `.html` da página correspondente.

Procure pelo atributo `data-i18n` no elemento. Se o texto não tiver esse atributo, edite diretamente o conteúdo do elemento.

Exemplo em `sobre.html`:
```html
<!-- Com data-i18n: edite em i18n.js -->
<h1 data-i18n="sobre_title">Sobre a Plane Aviation</h1>

<!-- Sem data-i18n: edite aqui mesmo -->
<p>Há mais de 20 anos, a Plane Aviation é...</p>
```

---

### Aeronaves à venda e Frota Cirrus
**Acesse:** `https://pedro-ojo.github.io/planeaviation/admin.html`

Login com seu e-mail e senha do Firebase. Tudo pelo painel.

---

### Informações de contato (endereço, telefone, e-mail)
**Arquivo:** `site.js`, na seção `FOOTER`. Procure por:
```js
<p>Hangar Plane Aviation</p>
<p>Aeroporto de Jundiaí — SP</p>
<p>contato@planeaviation.com.br</p>
<p>+55 (11) 9999-0000</p>
```
Edite os valores entre as tags `<p>`.

---

## 2. Como mudar cores, fontes e espaçamentos

**Tudo em um só lugar:** `styles.css`, seção `2. TOKENS` no topo do arquivo.

### Cores
```css
--background:       0 0% 97%;   /* fundo da página — mais claro = mais próximo de 100% */
--foreground:       0 0% 10%;   /* texto principal — mais escuro = mais próximo de 0% */
--muted:            0 0% 94%;   /* fundo de seções cinza claro */
--muted-foreground: 0 0% 45%;   /* textos secundários */
--border:           0 0% 82%;   /* cor das bordas */
```
Formato: `H S% L%` (matiz, saturação, luminosidade).
Para adicionar cor ao site (ex: azul primário), defina uma nova variável:
```css
--accent-blue: 220 80% 55%;
```
E use nos elementos: `color: hsl(var(--accent-blue));`

### Fonte
```css
--font: 'Inter', ui-sans-serif, system-ui, sans-serif;
```
Para trocar para outra Google Font:
1. Adicione o `@import` no topo do arquivo (substitua o atual)
2. Atualize `--font` com o novo nome

### Tamanhos de texto
```css
--font-size-body:  1rem;        /* 16px — parágrafos */
--font-size-small: .875rem;     /* 14px — textos secundários */
--font-size-xs:    .75rem;      /* 12px — labels, legendas */
--font-size-hero:  clamp(1.875rem, 5vw, 3.75rem); /* título hero — responsivo */
--font-size-h2:    clamp(1.5rem, 3vw, 2.25rem);
```

### Botões
```css
--btn-bg:       hsl(var(--foreground));  /* cor de fundo — padrão: preto */
--btn-fg:       hsl(var(--background));  /* cor do texto — padrão: branco */
--btn-padding:  .75rem 1.75rem;          /* tamanho interno */
--btn-weight:   700;                     /* espessura da fonte */
--btn-tracking: .06em;                   /* espaçamento entre letras */
```
Mudar `--btn-bg` muda a cor de **todos** os botões sólidos do site de uma vez.

### Header (vidro jateado)
```css
--glass-bg:     rgba(255, 255, 255, 0.72);  /* transparência (0 = invisível, 1 = sólido) */
--glass-blur:   18px;                        /* intensidade do blur */
--glass-border: rgba(255, 255, 255, 0.45);  /* cor da borda */
--glass-shadow: 0 4px 32px rgba(0,0,0,.10); /* sombra */
--header-top:   1.25rem;                     /* distância do topo da tela */
```

### Espaçamento entre seções
```css
--section-py:    5rem;    /* padding vertical das seções grandes */
--section-py-sm: 3.5rem;  /* seções menores */
```

---

## 3. Como adicionar imagens nas páginas estáticas

As páginas têm placeholders `<div class="ph">` onde imagens reais entrarão. Para substituir:

**Opção A — Imagem direta (recomendado):**
```html
<!-- Antes (placeholder): -->
<div class="ph" style="aspect-ratio:16/9">
  <span class="lbl">[Imagem aqui]</span>
</div>

<!-- Depois (imagem real): -->
<img src="caminho/para/imagem.jpg" alt="Descrição da imagem"
     style="width:100%;aspect-ratio:16/9;object-fit:cover">
```

**Hospedagem das imagens:**
- Coloque na pasta do repositório GitHub (ex: pasta `/images/`)
- Ou use um serviço externo (Cloudinary, ImgBB, etc.) e cole a URL

**Formatos:** JPG para fotos, PNG para logos/transparência, WebP para melhor performance.

---

## 4. Estrutura de arquivos

```
plane-aviation/
├── index.html          → Home
├── aeronaves.html      → Listagem de aeronaves
├── aeronave.html       → Detalhe de uma aeronave
├── universo-cirrus.html→ Universo Cirrus (frota demonstração)
├── sobre.html          → Sobre a Plane Aviation
├── servicos.html       → Serviços
├── contato.html        → Contato
├── configurador.html   → Configurador de aeronave
├── news.html           → News & Podcast
├── admin.html          → Painel administrativo (não editar)
│
├── styles.css          → ★ Design system (tokens, componentes)
├── i18n.js             → ★ Todos os textos traduzíveis (PT/EN/ES)
├── site.js             → Header, footer, menu — logos embutidas
├── firebase-config.js  → Chaves do Firebase (não editar)
│
├── firestore.rules     → Regras de segurança (publicar no console Firebase)
├── storage.rules       → Regras do Storage
└── SETUP.md            → Guia técnico de configuração Firebase
```

**Os arquivos marcados com ★ são os que você mais vai editar.**

---

## 5. Fluxo de publicação (GitHub Pages)

1. Edite os arquivos localmente ou direto no GitHub.com
2. Faça commit das alterações
3. O GitHub Pages publica automaticamente em ~1 minuto
4. Force refresh no navegador: **Ctrl+Shift+R** (Windows) / **Cmd+Shift+R** (Mac)

---

## 6. Dicas rápidas

| O que mudar | Onde |
|---|---|
| Textos do menu | `i18n.js` → chaves `nav_*` |
| Textos do rodapé | `i18n.js` → chaves `f_*` |
| Cor dos botões | `styles.css` → `--btn-bg` |
| Fonte do site | `styles.css` → `--font` + `@import` |
| Tamanho do título hero | `styles.css` → `--font-size-hero` |
| Transparência do header | `styles.css` → `--glass-bg` |
| Telefone/endereço no rodapé | `site.js` → seção `FOOTER` |
| Aeronaves à venda | Painel admin → `/admin.html` |
| Frota Cirrus | Painel admin → `/admin.html` |
| Regras de segurança | Firebase Console → Firestore → Regras |
