# CV — bilingv RO/RU + interactivitate minimală

**Data:** 2026-05-29
**Autor:** Nichita Ciobu (cu Claude)
**Status:** draft pentru aprobare

## 1. Context

CV-ul `cv-nichita-ciobu.html` e deja deployat la `https://cv.nikki.software` cu un design „editorial newspaper" pe 2 coloane (sidebar + main), font Fraunces + Hanken Grotesk, paletă paper/accent cărămiziu. Are funcția nativă „Salvează ca PDF" prin `window.print()` și print stylesheet curat.

Target: trimitere la **mai mulți angajatori moldoveni** care pot prefera RO sau RU. CV-ul rămâne CV (nu portofoliu) — interactivitate minimală, curată, fără să distragă HR-ul tradițional.

## 2. Scop

Adăugăm:
1. **Informații reale** care înlocuiesc placeholderele existente
2. **Comutare RO↔RU** în același document (instant, fără reload)
3. **Comutare dark/light**
4. **Polish subtil** (hover-uri, scroll smooth, tranziții)

NU adăugăm: fotografie, metrici cu numere, showcase de proiecte cu screenshoturi, contact form, secțiune referințe. (Utilizatorul a confirmat scope minimal.)

## 3. Modificări de conținut

### 3.1 Înlocuiri în CV-ul existent

| Loc în HTML | Înainte | După |
|---|---|---|
| `.contacts` item 3 | `+373 [telefon]` | `+373 69 440 135` (wrapped în `<a href="tel:+37369440135">`) |
| `.contacts` item 4 | `[email]` | `nichitaciobu0@gmail.com` (wrapped în `<a href="mailto:...">`) |
| `.contacts` item 5 | `[link rețea socială]` | Două itemi separați: `Telegram @ImNikki` (link `https://t.me/ImNikki`) și `GitHub imnikkl` (link `https://github.com/imnikkl`) |
| Job 2 → companie | `[Companie]` | `Nicons · nicons.md` |
| Job 2 → perioadă | `[perioadă]` | `~4 luni` |
| Job 2 → bullets | 2 generice | 3 bullets: (1) creat și administrat `nicons.md`, (2) optimizare SEO + update constant, (3) anunțuri de vânzări + administrare generală |

### 3.2 Traducere RU

Toate textele user-facing primesc `data-ru="..."` cu traducerea. Nume proprii (Laravel, Meta Ads, n8n, CEITI, IT Step Academy, Tilda, Canva, Photoshop, Google Docs/Sheets, CRM, HTML/CSS) rămân **neschimbate** în RU. Termeni traduși:

| RO | RU |
|---|---|
| Nichita Ciobu | Никита Чобу |
| Candidatură · Asistent Coordonator | Кандидатура · Помощник координатора |
| Coordonare · Marketing Digital · SMM · Organizare | Координация · Цифровой маркетинг · SMM · Организация |
| Horodca, Ialoveni · Moldova | Хородка, Яловень · Молдова |
| Salvează ca PDF | Сохранить как PDF |
| Limbi / Competențe / Instrumente / Educație / Profil / Experiență / Proiecte & Automatizări | Языки / Навыки / Инструменты / Образование / Профиль / Опыт / Проекты и автоматизации |
| Română / Rusă / Engleză / Nativ / Avansat / Mediu-avansat | Румынский / Русский / Английский / Родной / Продвинутый / Средний-продвинутый |
| în curs / în prezent | в процессе / в настоящее время |

Textul de profil + bullets se traduce integral, păstrând emphasis-ul cu `<b>` în interior.

## 4. Mecanism RO↔RU

### 4.1 Markup

Fiecare element cu text user-facing capătă `data-ro` și `data-ru` care conțin textul (poate include `<b>` HTML inline). Exemplu:

```html
<h1 class="name"
    data-ro='Nichita <span class="last">Ciobu</span>'
    data-ru='Никита <span class="last">Чобу</span>'>
  Nichita <span class="last">Ciobu</span>
</h1>
```

Conținutul inițial al elementului = `data-ro` (deci dacă JS e dezactivat, CV-ul se vede în RO).

### 4.2 Funcție de switch

```js
function setLang(lang) {
  if (lang !== 'ro' && lang !== 'ru') lang = 'ro';
  document.documentElement.lang = lang;
  document.title = lang === 'ru' ? 'CV — Никита Чобу' : 'CV — Nichita Ciobu';
  document.querySelectorAll('[data-ro]').forEach(el => {
    const val = el.dataset[lang] ?? el.dataset.ro;
    el.innerHTML = val;
  });
  document.querySelectorAll('.lang-btn').forEach(b => {
    b.classList.toggle('active', b.dataset.lang === lang);
  });
  try { localStorage.setItem('cv-lang', lang); } catch(_) {}
}
```

### 4.3 Auto-detectare la prima vizită

```js
function initialLang() {
  try {
    const stored = localStorage.getItem('cv-lang');
    if (stored === 'ro' || stored === 'ru') return stored;
  } catch(_) {}
  const nav = (navigator.language || '').toLowerCase();
  if (nav.startsWith('ru')) return 'ru';
  return 'ro';
}
```

Prioritate: `localStorage` > `navigator.language` > default RO.

### 4.4 Toolbar — markup

```html
<div class="toolbar">
  <div class="lang-switch" role="group" aria-label="Limbă / Язык">
    <button class="lang-btn active" data-lang="ro" aria-label="Română">RO</button>
    <button class="lang-btn"        data-lang="ru" aria-label="Русский">RU</button>
  </div>
  <button class="theme-btn" aria-label="Comută temă">☾</button>
  <button class="pdf-btn" data-ro="Salvează ca PDF" data-ru="Сохранить как PDF" onclick="window.print()">Salvează ca PDF</button>
</div>
```

Limba activă are clasa `.active` (stilizare: fundal `--ink`, text `--paper`).

## 5. Dark/light mode

### 5.1 CSS variables — paleta dark

```css
html.dark {
  --paper:    #15130f;
  --paper-2:  #1d1a14;
  --ink:      #f5efe2;
  --ink-soft: #c9c0b0;
  --muted:    #8a8073;
  --accent:   #d97a59;
  --accent-2: #e89a7c;
  --line:     #2e2820;
  --line-soft:#241f18;
}
html.dark body { background:#0e0c08; }
html.dark body::before { opacity:.06; }
```

### 5.2 Toggle

```js
function setTheme(theme) {
  if (theme !== 'dark' && theme !== 'light') theme = 'light';
  document.documentElement.classList.toggle('dark', theme === 'dark');
  const btn = document.querySelector('.theme-btn');
  if (btn) btn.textContent = theme === 'dark' ? '☀' : '☾';
  try { localStorage.setItem('cv-theme', theme); } catch(_) {}
}

function initialTheme() {
  try {
    const stored = localStorage.getItem('cv-theme');
    if (stored === 'dark' || stored === 'light') return stored;
  } catch(_) {}
  if (window.matchMedia?.('(prefers-color-scheme: dark)').matches) return 'dark';
  return 'light';
}
```

Prioritate: `localStorage` > `prefers-color-scheme` > default light.

### 5.3 Tranziții smooth la theme switch

```css
* { transition: background-color .25s ease, color .25s ease, border-color .25s ease; }
@media (prefers-reduced-motion: reduce) {
  * { transition: none !important; }
  html { scroll-behavior: auto !important; }
}
```

## 6. Polish interactiv

```css
html { scroll-behavior: smooth; }

.tags span { transition: border-color .18s, transform .18s, color .18s; }
.tags span:hover { border-color: var(--accent); color: var(--ink); transform: translateY(-1px); }

.skills li::before { transition: transform .25s; }
.skills li:hover::before { transform: rotate(90deg); }

.contacts a { transition: color .18s, border-color .18s; }

.job { transition: padding-left .2s; }
.job:hover { padding-left: 4px; }
.job::before {
  content: ''; position: absolute; left: -4px; top: 4px; width: 2px; height: calc(100% - 8px);
  background: var(--accent); opacity: 0; transition: opacity .2s;
}
.job:hover::before { opacity: 1; }
```

## 7. Print / PDF

### 7.1 Comportament dorit

- Limba activă (RO sau RU) iese în PDF
- Tema iese ÎNTOTDEAUNA în light (dark e suprimat)
- Toolbar e ascunsă (deja există în CSS-ul original)
- Hover-uri și tranziții nu se aplică

### 7.2 Implementare

```css
@media print {
  html.dark {
    --paper:#f8f4ec; --paper-2:#f2ece0; --ink:#1d1a15; --ink-soft:#46403a;
    --muted:#857c70; --accent:#b0492a; --accent-2:#c9714f;
    --line:#ddd5c6; --line-soft:#e8e1d4;
  }
  html.dark body { background:#fff; }
  /* + regulile existente @media print rămân: toolbar hidden, grain hidden, etc */
}
```

## 8. Error handling

| Scenariu | Comportament |
|---|---|
| JS dezactivat | CV vizibil în RO + light. Toolbar ascunsă via `<noscript>`. |
| `localStorage` blocat | Switch funcționează per-sesiune; preferința nu persistă. `try/catch` în jurul fiecărui acces. |
| Element fără `data-ru` corespunzător | Fallback la `data-ro`. |
| `navigator.language` undefined | Fallback la RO. |
| Limbă necunoscută în localStorage | Validată în `setLang`; cădere la RO. |
| Browser fără `matchMedia` | Fallback la light. |
| Browser fără suport ES6 | CV-ul se vede în RO + light static; interactivitatea nu pornește (acceptabil pentru fallback). |

## 9. Testing — checklist manual

1. Deschidere locală a `cv-nichita-ciobu.html` în Chrome (cele mai recente) → CV se vede corect, default RO + light, toolbar cu 4 elemente (RO, RU, ☾, Salvează PDF).
2. Click pe „RU" → toate textele se schimbă în rusă, inclusiv `<title>`, `aria-label`-uri unde e cazul, eyebrow, role, contacte, section headers, profile, bullets job, educație.
3. Click pe ☾ → fundal se face întunecat, contrast OK la AA minimum, accent rămâne lizibil.
4. Refresh pagină → limba + tema rămân (verificat în DevTools că `localStorage` conține `cv-lang` și `cv-theme`).
5. `Cmd/Ctrl+P` în RU + dark → preview iese în RU + light, fără toolbar, layout intact.
6. Browser cu `ru-RU` în setări (incognito ca să nu folosească localStorage existent) → la prima vizită se încarcă direct RU.
7. Mobile (DevTools 375px) → layout pe o coloană, toolbar accesibilă, butoane atingibile.
8. VoiceOver / NVDA scurt → butoanele RO/RU au `aria-label` citit; `<html lang>` corect.
9. Test cu `prefers-reduced-motion: reduce` (DevTools) → tranziții și scroll smooth dezactivate.
10. Deploy: `git push` + `ssh root@209.38.217.142 cv-update` → verificat live `https://cv.nikki.software`.

## 10. Arhitectură pe scurt

```
cv-nichita-ciobu.html       ← un singur fișier, fără build step
├── <head><style>           ← CSS-ul existent + adăugiri (html.dark, hovers, transitions, print)
├── <body>
│   ├── <noscript><style>…  ← ascunde toolbar dacă JS off
│   ├── <div class="toolbar">  ← 4 elemente: RO btn, RU btn, theme btn, PDF btn
│   └── <div class="sheet">    ← CV-ul (structură actuală neschimbată, doar primește data-ro/data-ru)
└── <script>                ← ~50 linii: setLang, setTheme, init, event listeners
```

Tot inline. Nimic nou de hosted. Deploy = exact ca până acum: `git push` + `cv-update`.

## 11. Risk-uri identificate

1. **Calitate traducere RU.** O fac eu, dar utilizatorul (vorbitor nativ de rusă C1) trebuie să o revizuiască înainte de trimiterea CV-ului la angajatori. Termenii tehnici stay-as-is internațional.
2. **Lungime text RU vs RO.** Rusa are cuvinte ceva mai lungi. Verific în testing pasul 1 că nu sparge contact-bar-ul sau bullet-urile.
3. **Cache la `cv.nikki.software`.** Browser-ul HR-ului poate cache-ui versiunea veche. Pun `Cache-Control: no-cache` pe `.html` la nginx, sau adaug un comentariu cu versiune în HTML.
4. **Compatibilitate `dataset` la browsere foarte vechi (IE11).** Acceptat — IE11 e EOL; fallback este textul RO inițial care e deja în DOM.

## 12. Non-scope / decizii respinse

- ❌ Foto, metrici cu numere, showcase proiecte, referințe — refuzate de utilizator pentru a păstra scope-ul curat
- ❌ Limba engleză — singurul target sunt angajatorii moldoveni RO+RU
- ❌ Două fișiere HTML separate (`cv-ro.html`, `cv-ru.html`) — respins, dublează maintenance
- ❌ Dicționar JSON separat (`translations.js`) — respins pentru CV-ul curent (suficient `data-*` inline); poate fi extras dacă mai târziu adăugăm EN
- ❌ Hero animat / scroll-triggered animations / portfolio mode
