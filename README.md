# Your AI Knows PHP. But Does It Know Your Project?

Symfony AI Mate w praktyce, na przykładzie Syliusa.
PHPers Bydgoszcz, 16.09.2026 — Michał Pysiak (Sylius).

**Slajdy:** https://mpysiak.github.io/phpers-bydgoszcz-2026/deck.html
(klawisze: strzałki, `n` = notatki prelegenta, `f` = pełny ekran)

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
