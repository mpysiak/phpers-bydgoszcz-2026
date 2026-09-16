# Your AI Knows PHP. But Does It Know Your Project?

Symfony AI Mate w praktyce, na przykładzie Syliusa.
PHPers Bydgoszcz, 16.09.2026 — Michał Pysiak (Sylius).

**Slajdy:** https://mpysiak.github.io/phpers-bydgoszcz-2026/deck.html
(klawisze: strzałki, `n` = notatki prelegenta, `f` = pełny ekran)

## Nagrania z demo (katalog `media/`)

Ten sam prompt („Strona … działa mi wolno - znajdź przyczynę"), ten sam projekt
(czysty Sylius Standard 2.2 z jedną customizacją listingu robiącą N+1),
po jednym przebiegu na wariant:

| plik | model | otoczenie | do wniosku „to N+1" |
| --- | --- | --- | --- |
| `demo-1-bare.webm` | Claude Fable 5.1 | bez Mate | ~5 min (nagranie przyspieszone ×5) |
| `demo-2-mate.webm` | Claude Fable 5.1 | z Mate + Symfony bridge | 1:20 |
| `demo-3-haiku-mate.webm` | Claude Haiku 4.5 | z Mate + Symfony bridge | 1:20 |

Między nagraniem 1 a 2 zmieniło się tylko to, co zostawia `vendor/bin/mate init`
(blok w `AGENTS.md`, `mate/`, skille w `.agents/skills`). Każdy przebieg to nowa
sesja Claude Code, bez pamięci z poprzednich.

## Linki z prezentacji

- Symfony AI Mate — dokumentacja: https://symfony.com/doc/current/ai/components/mate.html
- `sylius/sylius-ai-dev-tools` — pack dla Syliusa: https://github.com/Sylius/sylius-ai-dev-tools
- `sylius/sylius-mate-extension` — 29 tooli + 6 skilli: https://github.com/Sylius/sylius-mate-extension
- Blog Johannesa Wachtera (autora Mate): https://johanneswachter.dev/blog
  - Mate Lab — pomiary widoczności tooli: https://johanneswachter.dev/blog/mate-lab
  - The hardest code to delete — dlaczego zniknął serwer MCP: https://johanneswachter.dev/blog/the-hardest-code-to-delete
  - Kill the MCP: https://johanneswachter.dev/blog/kill-the-mcp
- Deck Johannesa z SymfonyLive Berlin 2026: https://johanneswachter.dev/symfony-mate-berlin

## Instalacja Mate w projekcie Symfony (jak w demo)

```
composer require --dev symfony/ai-mate symfony/ai-symfony-mate-extension
vendor/bin/mate init
vendor/bin/mate tools:list
```

W projekcie Sylius zamiast tego: `composer require --dev sylius/sylius-ai-dev-tools`.
