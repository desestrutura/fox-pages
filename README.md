# 🦊 fox-pages

Páginas HTML estáticas pessoais: roadmaps de games, wishlists e algumas ferramentas pequenas.

**No ar:** <https://desestrutura.github.io/fox-pages/>

## Páginas

| Página | O que é |
| --- | --- |
| [`index.html`](index.html) | Página inicial com links para todas as outras |
| [`roadmap-games-2026.html`](roadmap-games-2026.html) | 🎮 Roadmap de jogos de 2026: ordem mês a mês, progresso, FoxScore e co-op |
| [`roadmap-teddy-2026.html`](roadmap-teddy-2026.html) | 🦑 Roadmap de 2026 da Teddy, no mesmo formato |
| [`games-bucket-list.html`](games-bucket-list.html) | 🗂️ Jogos "um dia", sem prazo e fora do roadmap |
| [`wishlist.html`](wishlist.html) | 🎁 Lista de desejos: compras, presentes e ingressos de shows |
| [`wishlist-cds.html`](wishlist-cds.html) | 💿 Wishlist de CDs, por ordem de prioridade |
| [`wishlist-cds-japoneses.html`](wishlist-cds-japoneses.html) | 💿 Wishlist de CDs japoneses, por ordem de prioridade |

### Ferramentas

| Página | O que é |
| --- | --- |
| [`duelo-de-posicoes.html`](duelo-de-posicoes.html) | ⚔️ Reordena uma lista colada comparando dois itens por vez e depois compara com a ordem original |

## Como funciona

Não há build, dependências ou framework. Cada página é um único `.html` com `<style>` e `<script>` embutidos.

Para ver localmente, abra o arquivo no navegador. Se as imagens não carregarem via `file://`, sirva a pasta com:

```bash
python3 -m http.server 8765
```

Depois acesse <http://localhost:8765>.

## Estrutura

```
.
├── *.html          # uma página por arquivo
└── img/
    ├── games/      # capas de jogos (IGDB, t_cover_big)
    ├── cds/        # capas de CDs (~400px)
    └── shows/      # fotos de bandas/artistas para os ingressos (~1000px)
```

As imagens ficam salvas no repositório e as páginas usam caminhos relativos. A exceção são as fotos de produtos da wishlist: elas ficam hotlinkadas, porque uma imagem quebrada indica que o item saiu de linha.

## Editando

- Edite o HTML direto e confira no navegador antes de commitar. Erros de CSS ou JS só aparecem visualmente.
- Cada página tem sua própria paleta de cores em `:root`. Não há CSS compartilhado entre elas.
- Ao mudar o conteúdo de uma página, atualize o "Atualizado em …" no rodapé dela.
- As convenções detalhadas de cada página estão em [`CLAUDE.md`](CLAUDE.md). Por exemplo, nos roadmaps o resumo visual e os cards são mantidos à mão, assim como os contadores do topo e as regras de spoiler.

O site é publicado pelo GitHub Pages a partir do `main`.
