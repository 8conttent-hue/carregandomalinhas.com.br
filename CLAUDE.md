# CLAUDE.md — CARREGANDOMALINHAS

Site gerado pelo **SF (Site Factory)** em 15/04/2026. Migrado para o modelo Cloudflare + Supabase em 25/09/2026.

## Contexto do Site

**Nome:** CARREGANDOMALINHAS
**Nicho:** Viagens e Turismo
**Keywords:** Olaa pessoas queridas Missao Minha missao aqui com esse Blog e criar
**Paleta de cores:** slate | **Fonte:** playfair

Oláa pessoas queridas! Missão? Minha missão aqui com esse Blog é criar conteúdos e compartilhar experiências com pessoas, que assim como eu, adoram uma boa viagem. Além do mais, pessoas que são fissuradas ou adeptas de tecnologias e toda liberdade que ela trás para nós. E mesmo que você não goste muito de tecnologias e prefira apreciar a viagem em si, a natureza e etc, fique tranquilo que vou passar os conhecimentos e dicas de uma maneira bem descontraída e leve. Essa é minha missão! Conteúdo? Falo sobre viagens, nomadismo digital, relacionamentos, dinheiro, e tudo que cerca meu mundo e o de várias pessoas com esse mesmo DNA ou parecido. Meu propósito é gerar transformação na vida de cada pessoa que lê esse blog. Sejam muito bem-vindos! Espero que gostem e comentem meus conteúdos. Beijoss.

## Componentes visuais usados

| Seção | Variante |
|-------|----------|
| Header | Header-A |
| Hero | Hero-B |
| Features | Features-C |
| About Section | About-B |
| Posts | Posts-J |
| Footer | Footer-B |
| Página Sobre | Sobre-C |
| Página Contato | Contato-H |

## Estrutura do projeto

```
src/
  sections/        # Layout escolhido pelo SF — Header, Hero, Features, About, Posts, Footer, Sobre, Contato
  data/            # JSONs com todo o conteúdo editável
  lib/             # supabase.ts (cliente) e posts.ts (getPosts/getPostBySlug)
  components/      # Seo.astro (meta tags + JSON-LD)
  pages/           # Rotas Astro (index, sobre, contato, blog, privacidade, termos, [...slug])
  layouts/         # BaseLayout com fonte e cores dinâmicas
  styles/          # global.css com variáveis CSS de cor
public/
  images/          # hero.jpg, about.jpg, sobre.jpg
```

## O que editar

### Textos e conteúdo
- **`src/data/home.json`** — hero (título, subtítulo, botão), features (título, items), about section (título, desc, stats), posts
- **`src/data/sobre.json`** — conteúdo completo da página Sobre (hero, texto, missão)
- **`src/data/contato.json`** — título, subtítulo, email, tempo de resposta
- **`src/data/siteConfig.json`** — nome, slug, email, redes sociais, menu (título/descrição/OG/JSON-LD derivam daqui)

### Imagens
Imagens já estão em `public/images/` (via Pexels). Para substituir, mantenha os mesmos nomes de arquivo:
- `hero.jpg` — imagem de fundo do Hero (e og:image padrão)
- `about.jpg` — imagem da seção About (home)
- `sobre.jpg` — imagem de fundo da página Sobre

### Posts do blog
Os posts NÃO ficam mais em markdown local. São carregados do Supabase (tabela `network_posts`, filtrados por `domain = carregandomalinhas.com.br`).
- `src/lib/posts.ts` — `getPosts()` e `getPostBySlug()`; `formatContentToHtml()` converte markdown → HTML.
- Sem painel admin. Novos posts/posts editados entram pela plataforma 8links e publicam automaticamente (via Git/CF).

### Cores
Variáveis em `src/styles/global.css`: `--color-primary`, `--color-accent`, `--color-dark`.

## SEO

- `src/components/Seo.astro` injetado pelo `BaseLayout`: title, description, canonical, OG, Twitter, `name="robots"`, JSON-LD (WebSite nas páginas estáticas, BlogPosting nos artigos).
- `src/pages/robots.txt.ts` e `src/pages/sitemap.xml.ts` gerados dinamicamente (sitemap inclui posts com lastmod).

## Deploy

```bash
bun install
bun run build
# Publicar no Cloudflare: a pasta dist/ é servida como Worker (adaptador @astrojs/cloudflare)
# Envs opcionais no CF: SUPABASE_URL e SUPABASE_ANON_KEY (fallbacks embutidos no código)
```