---
marp: true
theme: default
paginate: true
size: 16:9
style: |
  section { font-family: -apple-system, "Segoe UI", Helvetica, Arial, sans-serif; font-size: 30px; padding: 60px 80px; }
  section h1 { font-size: 64px; line-height: 1.1; }
  section h2 { font-size: 46px; margin-bottom: 0.4em; }
  section.lead { text-align: left; display: flex; flex-direction: column; justify-content: center; }
  section.lead h1 { font-size: 78px; }
  section.big { display: flex; flex-direction: column; justify-content: center; text-align: center; }
  section.big h2 { font-size: 60px; }
  section.big p { font-size: 40px; }
  section .muted { color: #777; }
  section .todo { color: #b00; font-size: 0.7em; }
  section table { font-size: 26px; }
  section pre { font-size: 22px; }
  section blockquote { border-left: 6px solid #999; padding-left: 0.6em; color: #444; font-style: italic; }
  section .cols { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 28px; }
  section .cols > div { border: 2px solid #ddd; border-radius: 12px; padding: 18px 22px; font-size: 26px; }
  section .cols h3 { margin-top: 0; font-size: 30px; }
  section.demo { background: #111; color: #eee; }
  section.demo h2 { color: #fff; }
  section.demo code { background: #333; color: #ffd866; }
  section footer, section header { color: #999; }
---

<!-- _class: lead -->
<!-- _paginate: false -->

# Your AI Knows PHP.<br>But Does It Know Your Project?

Symfony AI Mate w praktyce, na przykładzie Syliusa

<span class="muted">Michał Pysiak · PHPers Bydgoszcz · 16.09.2026</span>

<!--
Jedno zdanie tezy w formie pytania. Model zna PHP i Symfony z treningu. Ale czy zna TWÓJ projekt? Dziś pokażę, że różnicę robi nie kolejny lepszy model, tylko to, co dostaje w momencie potrzeby.
-->

---

## O mnie

- Michał Pysiak, Sylius
- Robię **developer experience dla AI** w ekosystemie Syliusa
- To, co dziś pokażę, to **case study** z ostatnich miesięcy: co działało, co nie i dlaczego

<!--
30 sekund, bez CV. „Robię DX dla AI w Syliusie, to jest case study."
-->

---

## Jak to szło u nas

**Era gołego promptu**

- wymyślone nazwy serwisów i helperów Twiga
- `cache:clear` jako uniwersalna naprawa
- kod, który się kompiluje, ale łamie konwencje (DI, mailer, hooki)
- model pisząc **nasz skill** wymyślił 5 tooli, których nigdy nie było

**Era coraz dłuższych instrukcji**

- `SKILL.md` ~250 linii, zawsze w kontekście
- `anti-patterns.md` ~1 700 linii, 55 sekcji

<span class="todo">TODO: 3–4 konkretne wpadki jako anegdoty (nazwa, co zrobił, co się stało)</span>

<!--
Anegdoty z pierwszych miesięcy. Najpierw goły prompt: model zgaduje. Potem reakcja: piszemy instrukcje. CLAUDE.md, AGENTS.md, w repo, w katalogach, w home. Proza rosła, a zgadywanie nie znikało. Zapamiętajcie ten slajd — wrócimy do niego jako do strony „same skille".
-->

---

## Proza nie skaluje

- Kontekst ma budżet. Instrukcje nieadekwatne do zadania i tak są czytane w każdej turze.
- Statyczny tekst opisuje projekt **„jak był"**, nie „jak jest".
- Model i tak musi **wywnioskować** stan: który plugin, który dekorator, jaki hook.

> Guess in → guess out.
> <span class="muted">— Dave Liddament (via Johannes Wachter, SymfonyLive Berlin)</span>

|  | Zgadywanie | Jedno wywołanie |
|---|---|---|
| Podejście | czytaj 10+ plików, grep, wnioskuj | jeden celowany call |
| Tokeny | tysiące, głównie szum | kompaktowa struktura |
| Powtarzalność | niedeterministyczne | deterministyczne |
| Da się poprawić? | nie | **tak — poprawiasz tool** |

<!--
Diagnoza. Fakt o projekcie powinien przyjść w momencie potrzeby, zmierzony, tani. Nie siedzieć w kontekście na zapas. Tabela z Berlina: jedyna rzecz, którą da się optymalizować, to tool.
-->

---

<!-- _class: demo -->

## DEMO · przebieg 1: goły agent

Sklep na Syliusie. Na liście produktów pokazujemy warianty, rozmiary i stan magazynowy.

```
Strona https://127.0.0.1:8000/en_US/taxons/t-shirts/men
działa mi wolno - napraw
```

Bez Mate. Bez instrukcji. Sam model i repo.

<!--
Odpalam prompt i przechodzę do następnych slajdów — agent mieli w tle. Twardy limit 4 min, potem nagranie.
-->

---

## Harness: pętla i warstwy

**Pętla:** model ↔ narzędzia ↔ reguły. Część rzeczy egzekwuje **środowisko**, nie model.

| Warstwa | Co to jest | Kto egzekwuje |
|---|---|---|
| Instrukcje | proza | model |
| Skille | proza ładowana na żądanie | model |
| Toole | deterministyczny kod, wynik = fakt | **kod** |
| Hooki | kod odpalany przez agenta | **harness** |

Trzy pierwsze daje Mate. Hooki: w harnessach agentów (Claude Code, Cursor, Codex) — w Mate jeszcze nie.

<span class="muted">Piramida Liddamenta: deterministyczne rzeczy na dole. Nie zgadują — wiedzą.</span>

<!--
Mówione, gdy agent pracuje. Harness = wszystko wokół modelu. Im niżej w tabeli, tym mniej zależy od dobrej woli modelu.
-->

---

<!-- _class: big -->

## Proza mówi *jak myśleć*.<br>Tool mówi *jak jest*.

Odpowiedź z runtime'u zamiast inferencji.
Powtarzalna. Weryfikowalna. Nie zużywa kontekstu, dopóki nie jest potrzebna.

<span class="muted">Model przestaje być encyklopedią projektu. Zostaje decydentem.</span>

<!--
Wartość deterministycznego toola. Potem wracam do terminala: wynik przebiegu 1.
-->

---

<!-- _class: demo -->

## Przebieg 1 · wynik

- Dojdzie. **W końcu.** Pytanie: za ile tur, plików i minut.
- Zauważcie: on **wie**, że profiler istnieje — i dobiera się do niego ręcznie.

<span class="muted">Bez toola dojdzie. Pytanie: za ile.</span>

<!--
Pokazuję terminal: licznik tur i plików, czas. Beat: model zna Symfony, wie o profilerze, próbuje curl na /_profiler, czytać pliki z var/cache. Wiedza jest. Brakuje ścieżki.
-->

---

## Symfony AI Mate\*

„Serwer MCP dla projektu Symfony"\* — tak wszedł do świata, tak go znacie z Berlina.

```bash
composer require --dev symfony/ai-mate symfony/ai-symfony-mate-extension
```

Dev dependency. Dla Syliusa robi się to samo — za chwilę.

<span class="muted">\* wrócimy do gwiazdki na końcu</span>

<!--
Zajawka, 30 sekund. Co jest w środku, pokażę po demo.
-->

---

<!-- _class: demo -->

## `mate init` na żywo

Paczki już są w `vendor/`. Agent ich **nie widzi**.

```bash
vendor/bin/mate init
vendor/bin/mate tools:list
```

Co ląduje w projekcie:

- blok w `AGENTS.md` (+ `@AGENTS.md` w `CLAUDE.md`)
- `mate/` — config, rejestr ekstensji, `AGENT_INSTRUCTIONS.md`
- 5 skilli w `.agents/skills/` — 2 z rdzenia, 3 z rozszerzenia Symfony

<!--
Zero sieci, sekundy. Pokazuję blok w AGENTS.md i skille. tools:list: tak agent widzi, czym może pytać.
-->

---

<!-- _class: demo -->

## DEMO · przebieg 2: ten sam prompt, nowa sesja

```
Strona https://127.0.0.1:8000/en_US/taxons/t-shirts/men
działa mi wolno - napraw
```

Ten sam model. Ten sam projekt. Ten sam prompt.

<!--
Pierwszy ruch agenta: skill profilera, potem tool profilera. Nie grep. Demo kończy się na tym wywołaniu — fix dochodzi w tle.
-->

---

## Co dostał agent

```json
{
  "query_count": 111,
  "distinct_statements": 19,
  "duplicates": [
    {
      "sql": "SELECT ... FROM sylius_product_option_value ... WHERE variant_id = ?",
      "count": 45
    }
  ]
}
```

Jedno wywołanie. 111 zapytań, 45 identycznych. **Profiler zawsze wie. Zgadywanie nigdy.**

<span class="todo">TODO: wkleić realny output toola z przebiegu 2 (format z bridge'a)</span>

<!--
Kompaktowy, ustrukturyzowany, actionable. Agent nie musi czytać kodu, żeby wiedzieć, gdzie jest problem.
-->

---

## Co właśnie widzieliście

1. W obu przypadkach agent **dojdzie**. Kwestia czasu i tokenów.
2. W obu poszedł do **profilera** — jak developer.
   **Wiedza była w obu. Ścieżka w jednym.**
   Goły: kilka tur łopatologii. Z Mate: pierwszy ruch, bo skill powiedział *kiedy*, a tool dał *jak*.
3. Agent nie jest mądrzejszy. Ten sam model, prompt, projekt. **Inne otoczenie.**

<!--
Sam tekst. Tury i czas widzieliście na żywo. O tokenach tylko jakościowo: każda tura czytania plików to kontekst i pieniądze. Jeśli test Haiku+Mate wyjdzie: jedno zdanie tutaj.
-->

---

## Mate: ekstensje, własne toole, własne skille

- **Component `symfony/ai`**, dev-only, własny kontener obok aplikacji
- Agent pyta przez CLI: `tools:list` · `tools:inspect` · `tools:call`
- **Ekstensja = zwykła paczka Composera** z `extra.ai-mate` (scan-dirs, instructions, skills), wykrywana przy instalacji
  Symfony (profiler, kontener) · Monolog · społeczność · **Sylius**
- **Własny tool bez ekstensji:** metoda z `#[MateTool(name, description)]` w `mate/src/` — `init` już dodał scan-dir
- **Własny skill:** katalog w ekstensji → ląduje w `.agents/skills/mate-*`

<span class="muted">To, co widzieliście po `mate init` — tylko wasze.</span>

<!--
Teraz, gdy sala widziała efekt: co jest w środku. Tool = klasa PHP z atrybutem, DI działa. INSTRUCTIONS.md mówi agentowi kiedy. Skill = katalog obok.
-->

---

## Skille + toole > każde z osobna

<div class="cols">
<div>

### Same skille
Proza puchnie.
Model dalej zgaduje.
<span class="muted">(slajd 3)</span>

</div>
<div>

### Same toole
Binarka w `vendor/`.
Agent nie wie, że ma czym pytać.
<span class="muted">Mate Lab: goły CLI **0/10**</span>

</div>
<div>

### Razem
Skill mówi **kiedy i po co**.
Tool mówi **jak jest**.
<span class="muted">Mate Lab: **10/10**</span>

</div>
</div>

Widzieliście to na żywo: skill profilera + tool profilera.
Dlatego Mate od 0.13 dystrybuuje skille natywnie — a my wersjonujemy skill z toolami w jednej paczce.

<!--
Sedno środka. Ten sam model, Haiku, 10 przebiegów: bez warstwy plikowej 0/10, z nią 10/10. Decyduje widoczność.
-->

---

## Nasz case: extension = fakty

Ten sam mechanizm, nasza domena: `sylius/sylius-mate-extension`

- **29 tooli nad zbootowanym kernelem** aplikacji — agent *pyta* kontener, nie zgaduje
- `sylius_project_profile` · `sylius_installed_plugins` (z mapą dekoratorów) · hooki · gridy · resource'y
- scaffoldy bez kernela
- „dlaczego mój blok nie renderuje się na stronie produktu?" → `sylius_hooks_find_for_template` zamiast grepa po szablonach

<span class="todo">TODO: realny output `tools:call sylius_project_profile` z przygotowanego checkoutu</span>

<!--
Nie profiler jest magiczny, tylko mechanizm zmierzonego faktu. Ten sam kształt argumentu dla naszej domeny.
-->

---

## Nasz case: skill = osąd

Rodzina sześciu skilli `mate-sylius-*`

- **kiedy który tool** — zamiast opisywać projekt, mówimy, jak go zapytać
- verify pass, bramka Playwright
- **55 refuse-rules**: pułapki DI, mailera, hooków — jako reguły, nie esej

Wersjonowane razem z toolami, w jednej paczce. Upstream doszedł do tego samego.

<span class="todo">TODO: screenshot `.agents/skills/` z sześcioma mate-sylius-*</span>

<!--
Skill skurczony do osądu. Toole = fakty, skill = osąd.
-->

---

## Your project

- Zacznij od **jednego toola**, który zastępuje najczęstsze zgadywanie
- i **jednego skilla**, który mówi, kiedy go użyć
- **Zmierz**, czy agent go używa. Nie zakładaj.

<br>

## Harness engineering + context engineering > model

*Context:* co i kiedy trafia do kontekstu. *Harness:* co egzekwuje środowisko.

**Przestań czekać na lepszy model. Zbuduj mu lepsze otoczenie.**
Przy dzisiejszych modelach różnica między harnessami jest większa niż różnica między modelami.

<!--
Takeaway + klamra. Nazwać obie dyscypliny. Ostatnie zdanie = teza.
-->

---

## \* A propos MCP…

Berlin, kwiecień 2026 — slajd Johannesa: **„Why MCP, Not Just a CLI?"**
interactive context · standard · per-tool zgody

Ktoś zauważył, że `mate init` **nie stworzył `.mcp.json`**?

W 0.13 autor **usunął serwer MCP**. Został CLI + pliki.
Mate Lab: MCP **3/10** · CLI + pliki **10/10** → decyduje **widoczność, nie transport**.

> Ilość pracy włożonej w architekturę mówi zaskakująco mało o tym, czy powinna przetrwać kolejną.

<span class="muted">Ostatni dowód tezy: nawet transport tooli okazał się kwestią harnessu.</span>

<!--
Ciekawostka na koniec. Jego argumenty sprzed pięciu miesięcy, obalone jego własnym pomiarem. Koszt: uprawnienia z shella zamiast per-tool.
-->

---

<!-- _class: lead -->
<!-- _paginate: false -->

# Dzięki!

- `sylius/sylius-ai-dev-tools` · `sylius/sylius-mate-extension`
- symfony.com/doc/current/ai/components/mate.html
- johanneswachter.dev/blog — *mate-lab*, *the-hardest-code-to-delete*

<span class="todo">TODO: QR + slajdy online</span>

<!--
Zostaje na ekranie na Q&A.
-->
