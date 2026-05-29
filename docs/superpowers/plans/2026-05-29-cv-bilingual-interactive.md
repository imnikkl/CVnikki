# CV Bilingv RO/RU + Interactivitate Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Transformăm CV-ul static `cv-nichita-ciobu.html` într-o pagină bilingvă (RO/RU) cu mod întunecat și interactivitate minimală, păstrând identic export-ul PDF.

**Architecture:** Tot inline într-un singur fișier HTML — fără build step, fără dependențe noi. Traducerile sunt stocate ca `data-ro` / `data-ru` pe elementele user-facing. Un script de ~50 linii gestionează limba (RO/RU) și tema (light/dark) cu persistență `localStorage` + auto-detectare la prima vizită. Print stylesheet forțează light mode pentru PDF.

**Tech Stack:** HTML5 + CSS3 (custom properties) + Vanilla JS (ES2017+). Zero dependențe.

**Testing strategy:** Static HTML fără framework de test. Verificare manuală per task — deschidere browser + DevTools + print preview. Checklistul complet de testing e la final (Task 9), bazat pe secțiunea 9 din spec.

**Spec:** [`docs/superpowers/specs/2026-05-29-cv-bilingual-interactive-design.md`](../specs/2026-05-29-cv-bilingual-interactive-design.md)

---

## File Structure

**Modify only:** `cv-nichita-ciobu.html` (single file, all changes inline)

Nu creăm fișiere noi. Nu adăugăm dependențe. Nu există build step.

---

## Task 1: Înlocuiește placeholderele cu conținut real (RO)

**Files:**
- Modify: `cv-nichita-ciobu.html` (linii 201-208 contact, 295-307 job 2)

**Rationale:** Conținut real înainte de orice altă schimbare. Risk minim — dacă ne oprim aici, CV-ul e deja mai bun decât era. Nu introduce JS sau structuri noi.

- [ ] **Step 1: Înlocuiește contact bar (header)**

Găsește în `cv-nichita-ciobu.html` blocul:

```html
<div class="contacts">
  <span><i class="dot"></i>Horodca, Ialoveni · Moldova</span>
  <span><i class="dot"></i><a href="https://nikki.software" target="_blank" rel="noopener">nikki.software</a></span>
  <span><i class="dot"></i>+373 [telefon]</span>
  <span><i class="dot"></i>[email]</span>
  <span><i class="dot"></i>[link rețea socială]</span>
</div>
```

Înlocuiește cu:

```html
<div class="contacts">
  <span><i class="dot"></i>Horodca, Ialoveni · Moldova</span>
  <span><i class="dot"></i><a href="https://nikki.software" target="_blank" rel="noopener">nikki.software</a></span>
  <span><i class="dot"></i><a href="tel:+37369440135">+373 69 440 135</a></span>
  <span><i class="dot"></i><a href="mailto:nichitaciobu0@gmail.com">nichitaciobu0@gmail.com</a></span>
  <span><i class="dot"></i><a href="https://t.me/ImNikki" target="_blank" rel="noopener">Telegram @ImNikki</a></span>
  <span><i class="dot"></i><a href="https://github.com/imnikkl" target="_blank" rel="noopener">GitHub imnikkl</a></span>
</div>
```

- [ ] **Step 2: Înlocuiește job 2 (Administrator Web)**

Găsește blocul:

```html
<div class="job">
  <div class="row">
    <div>
      <div class="title">Administrator Web</div>
      <div class="org">[Companie]</div>
    </div>
    <div class="when">[perioadă]</div>
  </div>
  <ul>
    <li>Întreținerea și actualizarea conținutului site-ului, organizarea fluxurilor de lucru și respectarea termenelor.</li>
    <li>Coordonarea sarcinilor curente și menținerea ordinii în procesele de lucru.</li>
  </ul>
</div>
```

Înlocuiește cu:

```html
<div class="job">
  <div class="row">
    <div>
      <div class="title">Administrator Web</div>
      <div class="org">Nicons · nicons.md</div>
    </div>
    <div class="when">~4 luni</div>
  </div>
  <ul>
    <li>Am creat și administrat pagina web <b>Nicons.md</b>.</li>
    <li>Am lucrat asupra <b>optimizării SEO</b> și actualizam site-ul constant.</li>
    <li>Adăugam <b>anunțuri de vânzări</b> și mă ocupam de administrarea generală a platformei.</li>
  </ul>
</div>
```

- [ ] **Step 3: Verifică vizual**

Deschide `cv-nichita-ciobu.html` în Chrome:
```bash
open /Users/m1/Desktop/nikkiCV/cv-nichita-ciobu.html
```

Așteptat:
- Contact bar arată 6 itemi (locație, nikki.software, telefon, email, Telegram, GitHub) — toate link-uri unde e cazul
- Job „Administrator Web" arată „Nicons · nicons.md", „~4 luni", 3 bullets noi cu Nicons.md, SEO, anunțuri
- Layout-ul nu s-a stricat (contact bar se wrap pe 2 rânduri dacă e nevoie — este normal)

- [ ] **Step 4: Commit**

```bash
cd /Users/m1/Desktop/nikkiCV
git add cv-nichita-ciobu.html
git commit -m "content: înlocuiește placeholdere cu contact real + detalii Nicons"
```

---

## Task 2: Adaugă toolbar nou cu butoane RO/RU și tema, CSS pentru ele

**Files:**
- Modify: `cv-nichita-ciobu.html` (CSS în `<head>`, markup `.toolbar` în `<body>`)

**Rationale:** Markup-ul vizibil dar fără behavior. CV-ul rămâne funcțional, doar capătă butoane noi în colțul dreapta sus. Funcționalitatea reală vine în Task 7.

- [ ] **Step 1: Adaugă CSS pentru lang-switch în `<style>` (după regulile existente `.toolbar`)**

Găsește în CSS:
```css
.toolbar button:hover{background:var(--accent);border-color:var(--accent);transform:translateY(-1px)}
```

Adaugă imediat după:
```css
.lang-switch{
  display:flex;border:1px solid var(--ink);border-radius:2px;overflow:hidden;
}
.lang-btn{
  font-family:"Hanken Grotesk",sans-serif;font-size:12px;font-weight:600;
  letter-spacing:.04em;text-transform:uppercase;cursor:pointer;
  border:none;background:transparent;color:var(--ink);
  padding:10px 14px;transition:.18s ease;
}
.lang-btn.active{background:var(--ink);color:var(--paper)}
.lang-btn:not(.active):hover{background:var(--line-soft);color:var(--accent)}

.theme-btn{
  font-family:"Hanken Grotesk",sans-serif;font-size:14px;cursor:pointer;
  border:1px solid var(--ink);background:transparent;color:var(--ink);
  width:40px;height:40px;border-radius:2px;transition:.18s ease;
  display:inline-flex;align-items:center;justify-content:center;
}
.theme-btn:hover{background:var(--ink);color:var(--paper)}
```

- [ ] **Step 2: Înlocuiește markup `.toolbar`**

Găsește:
```html
<div class="toolbar">
  <button onclick="window.print()">Salvează ca PDF</button>
</div>
```

Înlocuiește cu:
```html
<div class="toolbar">
  <div class="lang-switch" role="group" aria-label="Limbă / Язык">
    <button class="lang-btn active" data-lang="ro" aria-label="Română">RO</button>
    <button class="lang-btn"        data-lang="ru" aria-label="Русский">RU</button>
  </div>
  <button class="theme-btn" aria-label="Comută temă" title="Comută temă">☾</button>
  <button class="pdf-btn" onclick="window.print()">Salvează ca PDF</button>
</div>
```

- [ ] **Step 3: Verifică vizual**

Reîncarcă pagina în browser. Așteptat:
- În colțul dreapta-sus apar 3 grupuri: `[RO|RU]` (RO highlight închis, RU bordură), butonul `☾`, butonul „Salvează ca PDF"
- Hover pe RU → fundal crem deschis, text portocaliu
- Hover pe `☾` → fundal închis, iconă deschis
- Click pe RU / ☾ încă nu face nimic (e ok — vine în Task 7)
- Click pe „Salvează ca PDF" → tot deschide print dialog

- [ ] **Step 4: Commit**

```bash
git add cv-nichita-ciobu.html
git commit -m "toolbar: adaugă butoane lang-switch RO/RU + theme toggle (UI fără behavior)"
```

---

## Task 3: Adaugă `data-ro` și `data-ru` la HEADER

**Files:**
- Modify: `cv-nichita-ciobu.html` (linii ~197-208 — masthead + contacts)

**Rationale:** Începem cu header pentru că e cel mai vizibil. Doar markup — niciun text nu se schimbă vizual (innerHTML inițial = RO). Verificarea: pagina arată identic.

- [ ] **Step 1: Adaugă atribute pe eyebrow, name, role**

Găsește:
```html
<header class="masthead">
  <div class="eyebrow">Candidatură · Asistent Coordonator</div>
  <h1 class="name">Nichita <span class="last">Ciobu</span></h1>
  <div class="role">Coordonare · Marketing Digital · SMM · Organizare</div>
```

Înlocuiește cu:
```html
<header class="masthead">
  <div class="eyebrow"
       data-ro="Candidatură · Asistent Coordonator"
       data-ru="Кандидатура · Помощник координатора">Candidatură · Asistent Coordonator</div>
  <h1 class="name"
      data-ro='Nichita <span class="last">Ciobu</span>'
      data-ru='Никита <span class="last">Чобу</span>'>Nichita <span class="last">Ciobu</span></h1>
  <div class="role"
       data-ro="Coordonare · Marketing Digital · SMM · Organizare"
       data-ru="Координация · Цифровой маркетинг · SMM · Организация">Coordonare · Marketing Digital · SMM · Organizare</div>
```

- [ ] **Step 2: Adaugă atribute pe locația din contact bar**

Găsește:
```html
<span><i class="dot"></i>Horodca, Ialoveni · Moldova</span>
```

Înlocuiește cu:
```html
<span data-ro='<i class="dot"></i>Horodca, Ialoveni · Moldova'
      data-ru='<i class="dot"></i>Хородка, Яловень · Молдова'><i class="dot"></i>Horodca, Ialoveni · Moldova</span>
```

(Celelalte spans din contact bar — nikki.software, telefon, email, Telegram, GitHub — sunt aceleași în ambele limbi, deci nu primesc `data-*`.)

- [ ] **Step 3: Verifică vizual**

Reîncarcă pagina. Așteptat:
- Header arată **identic** cu înainte
- DevTools → Inspect element pe eyebrow → vezi `data-ro` și `data-ru` în atribute

- [ ] **Step 4: Commit**

```bash
git add cv-nichita-ciobu.html
git commit -m "i18n: data-ro/data-ru pe header (eyebrow, nume, rol, locație)"
```

---

## Task 4: Adaugă `data-ro` și `data-ru` la SIDEBAR

**Files:**
- Modify: `cv-nichita-ciobu.html` (linii ~211-263 — aside.sidebar)

- [ ] **Step 1: Section headers (h)**

Găsește fiecare `<div class="h">...</div>` din sidebar și adaugă atribute:

```html
<div class="h" data-ro="Limbi" data-ru="Языки">Limbi</div>
```
```html
<div class="h" data-ro="Competențe" data-ru="Навыки">Competențe</div>
```
```html
<div class="h" data-ro="Instrumente" data-ru="Инструменты">Instrumente</div>
```
```html
<div class="h" data-ro="Educație" data-ru="Образование">Educație</div>
```

- [ ] **Step 2: Limbi**

Găsește:
```html
<div class="lang">
  <div class="top"><span class="nm">Română</span><span class="lv">Nativ</span></div>
  <div class="bar"><i style="width:100%"></i></div>
</div>
<div class="lang">
  <div class="top"><span class="nm">Rusă</span><span class="lv">Avansat · C1</span></div>
  <div class="bar"><i style="width:92%"></i></div>
</div>
<div class="lang">
  <div class="top"><span class="nm">Engleză</span><span class="lv">Mediu-avansat · B2</span></div>
  <div class="bar"><i style="width:72%"></i></div>
</div>
```

Înlocuiește cu:
```html
<div class="lang">
  <div class="top">
    <span class="nm" data-ro="Română" data-ru="Румынский">Română</span>
    <span class="lv" data-ro="Nativ" data-ru="Родной">Nativ</span>
  </div>
  <div class="bar"><i style="width:100%"></i></div>
</div>
<div class="lang">
  <div class="top">
    <span class="nm" data-ro="Rusă" data-ru="Русский">Rusă</span>
    <span class="lv" data-ro="Avansat · C1" data-ru="Продвинутый · C1">Avansat · C1</span>
  </div>
  <div class="bar"><i style="width:92%"></i></div>
</div>
<div class="lang">
  <div class="top">
    <span class="nm" data-ro="Engleză" data-ru="Английский">Engleză</span>
    <span class="lv" data-ro="Mediu-avansat · B2" data-ru="Средний-продвинутый · B2">Mediu-avansat · B2</span>
  </div>
  <div class="bar"><i style="width:72%"></i></div>
</div>
```

- [ ] **Step 3: Competențe (skills bullets)**

Găsește `<ul class="skills">...</ul>` și înlocuiește cu:
```html
<ul class="skills">
  <li data-ro="Organizare și gestionarea sarcinilor, control al termenelor"
      data-ru="Организация и управление задачами, контроль сроков">Organizare și gestionarea sarcinilor, control al termenelor</li>
  <li data-ro="Multitasking și lucru autonom, de la distanță"
      data-ru="Многозадачность и автономная удалённая работа">Multitasking și lucru autonom, de la distanță</li>
  <li data-ro="Coordonarea executanților (designeri, targetologi, creatori de conținut)"
      data-ru="Координация исполнителей (дизайнеры, таргетологи, контент-мейкеры)">Coordonarea executanților (designeri, targetologi, creatori de conținut)</li>
  <li data-ro="Comunicare cu clienți, parteneri și furnizori"
      data-ru="Коммуникация с клиентами, партнёрами и поставщиками">Comunicare cu clienți, parteneri și furnizori</li>
  <li data-ro="Copywriting pentru postări, afișe și oferte comerciale"
      data-ru="Копирайтинг для постов, афиш и коммерческих предложений">Copywriting pentru postări, afișe și oferte comerciale</li>
  <li data-ro="Atenție la detalii și gândire sistemică"
      data-ru="Внимание к деталям и системное мышление">Atenție la detalii și gândire sistemică</li>
</ul>
```

- [ ] **Step 4: Instrumente — singurul tag care diferă**

În `<div class="tags">`, găsește:
```html
<span>Instrumente AI</span>
```

Înlocuiește cu:
```html
<span data-ro="Instrumente AI" data-ru="AI-инструменты">Instrumente AI</span>
```

(Celelalte tag-uri — Tilda, Canva, Google Docs, Google Sheets, Meta Ads Manager, CRM, n8n, Photoshop, HTML / CSS — sunt nume proprii / aceleași în ambele limbi.)

- [ ] **Step 5: Educație**

Găsește:
```html
<section class="block edu">
  <div class="h">Educație</div>
  <div class="item">
    <div class="deg">Dezvoltarea aplicațiilor web</div>
    <div class="org">CEITI — Centrul de Excelență în Informatică și Tehnologii Informaționale · grupa W-2513</div>
    <div class="yr">în curs</div>
  </div>
  <div class="item">
    <div class="deg">Certificări IT</div>
    <div class="org">IT Step Academy — Photoshop · HTML / CSS / JavaScript · Animație video</div>
  </div>
</section>
```

Înlocuiește cu (păstrând `data-ro/ru` adăugat anterior la `<div class="h">`):
```html
<section class="block edu">
  <div class="h" data-ro="Educație" data-ru="Образование">Educație</div>
  <div class="item">
    <div class="deg" data-ro="Dezvoltarea aplicațiilor web"
                     data-ru="Разработка веб-приложений">Dezvoltarea aplicațiilor web</div>
    <div class="org" data-ro="CEITI — Centrul de Excelență în Informatică și Tehnologii Informaționale · grupa W-2513"
                     data-ru="CEITI — Центр совершенствования в информатике и информационных технологиях · группа W-2513">CEITI — Centrul de Excelență în Informatică și Tehnologii Informaționale · grupa W-2513</div>
    <div class="yr" data-ro="în curs" data-ru="в процессе">în curs</div>
  </div>
  <div class="item">
    <div class="deg" data-ro="Certificări IT" data-ru="IT-сертификации">Certificări IT</div>
    <div class="org" data-ro="IT Step Academy — Photoshop · HTML / CSS / JavaScript · Animație video"
                     data-ru="IT Step Academy — Photoshop · HTML / CSS / JavaScript · Видеоанимация">IT Step Academy — Photoshop · HTML / CSS / JavaScript · Animație video</div>
  </div>
</section>
```

- [ ] **Step 6: Verifică vizual**

Reîncarcă. Sidebar identic vizual. Inspect element → atribute prezente.

- [ ] **Step 7: Commit**

```bash
git add cv-nichita-ciobu.html
git commit -m "i18n: data-ro/data-ru pe sidebar (limbi, competențe, instrumente, educație)"
```

---

## Task 5: Adaugă `data-ro` și `data-ru` la MAIN COLUMN

**Files:**
- Modify: `cv-nichita-ciobu.html` (linii ~267-323 — main)

- [ ] **Step 1: Section headers main**

Găsește în `<main class="main">`:
```html
<div class="h">Profil</div>
```
Schimbă în:
```html
<div class="h" data-ro="Profil" data-ru="Профиль">Profil</div>
```

```html
<div class="h">Experiență</div>
```
→
```html
<div class="h" data-ro="Experiență" data-ru="Опыт">Experiență</div>
```

```html
<div class="h">Proiecte &amp; Automatizări</div>
```
→
```html
<div class="h" data-ro="Proiecte &amp; Automatizări" data-ru="Проекты и автоматизации">Proiecte &amp; Automatizări</div>
```

- [ ] **Step 2: Profile paragraph**

Găsește:
```html
<p class="profile">Specialist tânăr, <b>organizat și autonom</b>, cu experiență practică în marketing digital, dezvoltare web și automatizări cu instrumente AI. Duc proiectele de la idee la rezultat: campanii publicitare pe Meta Ads, conținut și materiale vizuale, site-uri și comunicarea directă cu clienții. <b>Bilingv română–rusă</b>, obișnuit cu multitasking-ul și termenele strânse. Caut un rol într-un mediu de afaceri activ, unde pot susține organizarea evenimentelor, coordonarea echipei și procesele de zi cu zi.</p>
```

Înlocuiește cu:
```html
<p class="profile"
   data-ro="Specialist tânăr, <b>organizat și autonom</b>, cu experiență practică în marketing digital, dezvoltare web și automatizări cu instrumente AI. Duc proiectele de la idee la rezultat: campanii publicitare pe Meta Ads, conținut și materiale vizuale, site-uri și comunicarea directă cu clienții. <b>Bilingv română–rusă</b>, obișnuit cu multitasking-ul și termenele strânse. Caut un rol într-un mediu de afaceri activ, unde pot susține organizarea evenimentelor, coordonarea echipei și procesele de zi cu zi."
   data-ru="Молодой специалист, <b>организованный и автономный</b>, с практическим опытом в цифровом маркетинге, веб-разработке и автоматизациях с AI-инструментами. Веду проекты от идеи до результата: рекламные кампании в Meta Ads, контент и визуальные материалы, сайты и прямая коммуникация с клиентами. <b>Двуязычный румынский–русский</b>, привык к многозадачности и сжатым срокам. Ищу роль в активной бизнес-среде, где могу поддерживать организацию мероприятий, координацию команды и ежедневные процессы.">Specialist tânăr, <b>organizat și autonom</b>, cu experiență practică în marketing digital, dezvoltare web și automatizări cu instrumente AI. Duc proiectele de la idee la rezultat: campanii publicitare pe Meta Ads, conținut și materiale vizuale, site-uri și comunicarea directă cu clienții. <b>Bilingv română–rusă</b>, obișnuit cu multitasking-ul și termenele strânse. Caut un rol într-un mediu de afaceri activ, unde pot susține organizarea evenimentelor, coordonarea echipei și procesele de zi cu zi.</p>
```

- [ ] **Step 3: Job 1 — Marketing Digital & Dezvoltare Web (freelance)**

Găsește blocul `<div class="job">` cu titlul „Marketing Digital & Dezvoltare Web" și înlocuiește integral cu:

```html
<div class="job">
  <div class="row">
    <div>
      <div class="title"
           data-ro="Marketing Digital &amp; Dezvoltare Web"
           data-ru="Цифровой маркетинг и веб-разработка">Marketing Digital &amp; Dezvoltare Web</div>
      <div class="org"
           data-ro="Activitate freelance · Moldova"
           data-ru="Фриланс · Молдова">Activitate freelance · Moldova</div>
    </div>
    <div class="when"
         data-ro="în prezent"
         data-ru="в настоящее время">în prezent</div>
  </div>
  <ul>
    <li data-ro="Administrez <b>campanii de publicitate plătită pe Meta Ads</b> (Facebook / Instagram) pentru afaceri locale — configurare, optimizarea bugetului, raportare și soluționarea problemelor de facturare (autentificare 3DS, plăți recurente)."
        data-ru="Веду <b>платные рекламные кампании в Meta Ads</b> (Facebook / Instagram) для местного бизнеса — настройка, оптимизация бюджета, отчётность и решение проблем с оплатой (3DS-аутентификация, регулярные платежи).">Administrez <b>campanii de publicitate plătită pe Meta Ads</b> (Facebook / Instagram) pentru afaceri locale — configurare, optimizarea bugetului, raportare și soluționarea problemelor de facturare (autentificare 3DS, plăți recurente).</li>
    <li data-ro="Comunic <b>direct cu clienții</b>: clarific cerințele, pregătesc propuneri și mențin relația pe tot parcursul proiectului."
        data-ru="Общаюсь <b>напрямую с клиентами</b>: уточняю требования, готовлю предложения и поддерживаю отношения на протяжении всего проекта.">Comunic <b>direct cu clienții</b>: clarific cerințele, pregătesc propuneri și mențin relația pe tot parcursul proiectului.</li>
    <li data-ro="Creez și administrez site-uri și landing pages (Laravel, PHP, TypeScript), inclusiv integrări precum autentificarea Google."
        data-ru="Создаю и администрирую сайты и лендинги (Laravel, PHP, TypeScript), включая интеграции вроде Google-аутентификации.">Creez și administrez site-uri și landing pages (Laravel, PHP, TypeScript), inclusiv integrări precum autentificarea Google.</li>
    <li data-ro="Pregătesc materiale vizuale, afișe și texte pentru postări (Canva, Photoshop)."
        data-ru="Готовлю визуальные материалы, афиши и тексты для постов (Canva, Photoshop).">Pregătesc materiale vizuale, afișe și texte pentru postări (Canva, Photoshop).</li>
  </ul>
</div>
```

- [ ] **Step 4: Job 2 — Administrator Web (Nicons)**

Găsește blocul `<div class="job">` cu „Administrator Web" și înlocuiește cu:

```html
<div class="job">
  <div class="row">
    <div>
      <div class="title" data-ro="Administrator Web" data-ru="Веб-администратор">Administrator Web</div>
      <div class="org" data-ro="Nicons · nicons.md" data-ru="Nicons · nicons.md">Nicons · nicons.md</div>
    </div>
    <div class="when" data-ro="~4 luni" data-ru="~4 месяца">~4 luni</div>
  </div>
  <ul>
    <li data-ro="Am creat și administrat pagina web <b>Nicons.md</b>."
        data-ru="Создал и администрировал веб-страницу <b>Nicons.md</b>.">Am creat și administrat pagina web <b>Nicons.md</b>.</li>
    <li data-ro="Am lucrat asupra <b>optimizării SEO</b> și actualizam site-ul constant."
        data-ru="Работал над <b>SEO-оптимизацией</b> и регулярно обновлял сайт.">Am lucrat asupra <b>optimizării SEO</b> și actualizam site-ul constant.</li>
    <li data-ro="Adăugam <b>anunțuri de vânzări</b> și mă ocupam de administrarea generală a platformei."
        data-ru="Добавлял <b>объявления о продажах</b> и занимался общим администрированием платформы.">Adăugam <b>anunțuri de vânzări</b> și mă ocupam de administrarea generală a platformei.</li>
  </ul>
</div>
```

- [ ] **Step 5: Proiecte & Automatizări — bullets**

Găsește `<section class="block">` cu „Proiecte & Automatizări":

```html
<section class="block">
  <div class="h" data-ro="Proiecte &amp; Automatizări" data-ru="Проекты и автоматизации">Proiecte &amp; Automatizări</div>
  <div class="job">
    <ul>
      <li data-ro="Am construit o <b>platformă imobiliară</b> completă (Laravel): anunțuri, autentificare, structură optimizată SEO."
          data-ru="Построил полноценную <b>платформу недвижимости</b> (Laravel): объявления, аутентификация, SEO-оптимизированная структура.">Am construit o <b>platformă imobiliară</b> completă (Laravel): anunțuri, autentificare, structură optimizată SEO.</li>
      <li data-ro="Am creat <b>fluxuri de automatizare în n8n</b> cu instrumente AI — scorarea lead-urilor, asistent de suport și recuperarea abonamentelor."
          data-ru="Создал <b>автоматизированные потоки в n8n</b> с AI-инструментами — скоринг лидов, ассистент поддержки и восстановление подписок.">Am creat <b>fluxuri de automatizare în n8n</b> cu instrumente AI — scorarea lead-urilor, asistent de suport și recuperarea abonamentelor.</li>
      <li data-ro="Portofoliu și proiecte personale disponibile la <b>nikki.software</b>."
          data-ru="Портфолио и личные проекты доступны на <b>nikki.software</b>.">Portofoliu și proiecte personale disponibile la <b>nikki.software</b>.</li>
    </ul>
  </div>
</section>
```

- [ ] **Step 6: Buton PDF din toolbar (label-ul)**

Găsește în toolbar:
```html
<button class="pdf-btn" onclick="window.print()">Salvează ca PDF</button>
```

Înlocuiește cu:
```html
<button class="pdf-btn"
        data-ro="Salvează ca PDF"
        data-ru="Сохранить как PDF"
        onclick="window.print()">Salvează ca PDF</button>
```

- [ ] **Step 7: Verifică vizual**

Reîncarcă. Tot CV-ul arată **identic** vizual cu starea de după Task 1. Inspect element → atribute prezente pe toate elementele.

- [ ] **Step 8: Commit**

```bash
git add cv-nichita-ciobu.html
git commit -m "i18n: data-ro/data-ru pe main column (profil, experiență, proiecte, PDF btn)"
```

---

## Task 6: Adaugă CSS pentru dark mode + tranziții smooth

**Files:**
- Modify: `cv-nichita-ciobu.html` (CSS în `<head>`)

**Rationale:** CSS pentru dark mode — încă fără JS. Verificarea: adaugi clasa `dark` manual din DevTools și vezi cum arată.

- [ ] **Step 1: Adaugă palette dark + transitions**

Găsește în CSS:
```css
:root{
    --paper:#f8f4ec;
    --paper-2:#f2ece0;
    --ink:#1d1a15;
    --ink-soft:#46403a;
    --muted:#857c70;
    --accent:#b0492a;
    --accent-2:#c9714f;
    --line:#ddd5c6;
    --line-soft:#e8e1d4;
  }
```

Adaugă imediat după blocul `:root`:

```css
  html.dark{
    --paper:#15130f;
    --paper-2:#1d1a14;
    --ink:#f5efe2;
    --ink-soft:#c9c0b0;
    --muted:#8a8073;
    --accent:#d97a59;
    --accent-2:#e89a7c;
    --line:#2e2820;
    --line-soft:#241f18;
  }
  html.dark body{background:#0e0c08}
  html.dark body::before{opacity:.06}

  /* Smooth theme transition pe containerele care își schimbă culoarea */
  html, body, .sheet, .sidebar, .main, .masthead, .h, .tags span,
  .contacts span, .contacts a, .skills li, .bar, .bar i, .divider,
  .lang-btn, .theme-btn, .pdf-btn{
    transition: background-color .25s ease, color .25s ease, border-color .25s ease;
  }
```

- [ ] **Step 2: Verifică în DevTools**

Reîncarcă, deschide DevTools (Cmd+Opt+I), în Elements selectează `<html>` și adaugă manual atributul `class="dark"`:
```
<html lang="ro" class="dark">
```

Așteptat:
- Fundal devine întunecat (negru-maroniu)
- Text se face crem
- Accent devine portocaliu mai cald
- Tranziție smooth ~0.25s între light și dark când scoți/pui clasa
- Contrast e suficient (textul se citește clar)

Scoate clasa `dark` — revine la light.

- [ ] **Step 3: Commit**

```bash
git add cv-nichita-ciobu.html
git commit -m "theme: adaugă paleta dark mode + transitions (fără JS toggle încă)"
```

---

## Task 7: Adaugă JS pentru language + theme switching cu auto-detect

**Files:**
- Modify: `cv-nichita-ciobu.html` (adaugă `<script>` înainte de `</body>`)

**Rationale:** Acum totul prinde viață. Un singur script care gestionează limbă, temă, persistență, auto-detect.

- [ ] **Step 1: Adaugă scriptul înainte de `</body>`**

Găsește la finalul fișierului:
```html
</div>
</body>
</html>
```

Înlocuiește cu:
```html
</div>

<script>
(function(){
  'use strict';

  // ----- Language -----
  function setLang(lang){
    if(lang !== 'ro' && lang !== 'ru') lang = 'ro';
    document.documentElement.lang = lang;
    document.title = lang === 'ru' ? 'CV — Никита Чобу' : 'CV — Nichita Ciobu';
    document.querySelectorAll('[data-ro]').forEach(function(el){
      var val = el.dataset[lang];
      if(val == null) val = el.dataset.ro;
      el.innerHTML = val;
    });
    document.querySelectorAll('.lang-btn').forEach(function(b){
      b.classList.toggle('active', b.dataset.lang === lang);
    });
    try { localStorage.setItem('cv-lang', lang); } catch(_){}
  }

  function initialLang(){
    try {
      var stored = localStorage.getItem('cv-lang');
      if(stored === 'ro' || stored === 'ru') return stored;
    } catch(_){}
    var nav = (navigator.language || '').toLowerCase();
    if(nav.indexOf('ru') === 0) return 'ru';
    return 'ro';
  }

  // ----- Theme -----
  function setTheme(theme){
    if(theme !== 'dark' && theme !== 'light') theme = 'light';
    document.documentElement.classList.toggle('dark', theme === 'dark');
    var btn = document.querySelector('.theme-btn');
    if(btn){
      btn.textContent = theme === 'dark' ? '☀' : '☾';
      btn.setAttribute('aria-label', theme === 'dark' ? 'Comută la temă deschisă' : 'Comută la temă întunecată');
    }
    try { localStorage.setItem('cv-theme', theme); } catch(_){}
  }

  function initialTheme(){
    try {
      var stored = localStorage.getItem('cv-theme');
      if(stored === 'dark' || stored === 'light') return stored;
    } catch(_){}
    if(window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) return 'dark';
    return 'light';
  }

  // ----- Wire up -----
  document.querySelectorAll('.lang-btn').forEach(function(b){
    b.addEventListener('click', function(){ setLang(b.dataset.lang); });
  });

  var themeBtn = document.querySelector('.theme-btn');
  if(themeBtn){
    themeBtn.addEventListener('click', function(){
      var current = document.documentElement.classList.contains('dark') ? 'dark' : 'light';
      setTheme(current === 'dark' ? 'light' : 'dark');
    });
  }

  // ----- Init -----
  setLang(initialLang());
  setTheme(initialTheme());
})();
</script>
</body>
</html>
```

- [ ] **Step 2: Verifică funcționalitatea**

Reîncarcă pagina. Așteptat:
- Default: RO + light (sau RU + dark dacă browser-ul tău are setări corespunzătoare)
- Click pe „RU" → toate textele se schimbă în rusă instant, RU se highlight, RO devine inactiv, `<title>` se schimbă în DevTools la „CV — Никита Чобу"
- Click înapoi pe „RO" → revine în română
- Click pe `☾` → fundalul se face întunecat, iconă devine `☀`
- Click iar pe `☀` → revine light, iconă `☾`
- Refresh (Cmd+R) → limba + tema rămân (verifică în DevTools → Application → Local Storage → cv-lang, cv-theme)

- [ ] **Step 3: Test auto-detectare**

Deschide DevTools → Application → Storage → Local storage → șterge `cv-lang` și `cv-theme`. Refresh pagină.

În Chrome DevTools → ⋮ (dreapta sus) → More tools → Rendering → găsește „Emulate CSS media feature prefers-color-scheme" → setează la „dark". Refresh.

Așteptat: pagina se încarcă direct în dark mode (auto-detect).

Pentru limbă: DevTools nu poate emula `navigator.language`. Verifică în setări Chrome dacă vrei: Settings → Languages → adaugă „Русский" pe primul loc. Acest test poate fi omis dacă nu vrei să-ți modifici setările.

- [ ] **Step 4: Test fallback fără localStorage**

DevTools → Application → Local Storage → click dreapta → „Clear". Reîncarcă pagina. Verifică că pagina pornește corect (auto-detect).

- [ ] **Step 5: Commit**

```bash
git add cv-nichita-ciobu.html
git commit -m "js: lang switch (RO/RU) + theme toggle cu localStorage + auto-detect"
```

---

## Task 8: Print overrides + polish CSS + noscript fallback

**Files:**
- Modify: `cv-nichita-ciobu.html` (CSS în `<head>`, `<noscript>` în `<body>`)

**Rationale:** Toate îmbunătățirile finale într-un singur task — sunt CSS-only sau markup minimal.

- [ ] **Step 1: Adaugă `<noscript>` style block**

Găsește în `<body>` (chiar înainte de `<div class="toolbar">`):
```html
<body>

<div class="toolbar">
```

Înlocuiește cu:
```html
<body>

<noscript><style>.toolbar{display:none}</style></noscript>

<div class="toolbar">
```

- [ ] **Step 2: Adaugă hover polish + smooth scroll + reduced-motion**

Găsește în CSS, după regulile pentru `.bar` (linia ~119):
```css
.bar i{display:block;height:100%;background:var(--accent)}
```

Adaugă imediat după:
```css
  /* Polish interactiv */
  html{scroll-behavior:smooth}

  .tags span{transition:border-color .18s, transform .18s, color .18s, background-color .18s}
  .tags span:hover{border-color:var(--accent);color:var(--ink);transform:translateY(-1px)}

  .skills li::before{transition:transform .25s}
  .skills li:hover::before{transform:rotate(135deg)}

  .contacts a{transition:color .18s, border-color .18s}

  .job{transition:padding-left .2s}
  .job:hover{padding-left:4px}
  .job::before{
    content:"";position:absolute;left:-4px;top:4px;width:2px;height:calc(100% - 8px);
    background:var(--accent);opacity:0;transition:opacity .2s;
  }
  .job:hover::before{opacity:1}

  @media (prefers-reduced-motion: reduce){
    *,*::before,*::after{transition:none !important;animation:none !important}
    html{scroll-behavior:auto !important}
  }
```

- [ ] **Step 3: Override `@media print` pentru dark mode**

Găsește în CSS:
```css
@media print{
    @page{size:A4;margin:0}
    html,body{background:#fff}
    body::before{display:none}
    .toolbar{display:none}
    .sheet{margin:0;box-shadow:none;width:100%;min-height:100vh}
    *{-webkit-print-color-adjust:exact;print-color-adjust:exact}
  }
```

Înlocuiește cu:
```css
@media print{
    @page{size:A4;margin:0}
    /* Forțează light mode la print, chiar dacă utilizatorul e pe dark */
    html.dark{
      --paper:#f8f4ec; --paper-2:#f2ece0; --ink:#1d1a15; --ink-soft:#46403a;
      --muted:#857c70; --accent:#b0492a; --accent-2:#c9714f;
      --line:#ddd5c6; --line-soft:#e8e1d4;
    }
    html,body,html.dark body{background:#fff}
    body::before{display:none}
    .toolbar{display:none}
    .sheet{margin:0;box-shadow:none;width:100%;min-height:100vh}
    *{-webkit-print-color-adjust:exact;print-color-adjust:exact}
  }
```

- [ ] **Step 4: Verifică polish**

Reîncarcă. Așteptat:
- Hover pe tag (Tilda, Canva etc.) → bordură portocalie, mic lift de 1px
- Hover pe bullet competențe → pătratul roșu se rotește 90° (de la 45° la 135°)
- Hover pe job (Marketing Digital sau Administrator Web) → apare bară roșie subțire stânga, conținut se mută 4px dreapta
- Hover pe link contact (telefon, email, Telegram, GitHub) → underline se evidențiază

- [ ] **Step 5: Verifică print preview**

Comută în dark mode (click pe `☾`). Apoi Cmd+P (sau Ctrl+P).

Așteptat:
- Print preview afișează CV-ul **în light** (paper crem, text închis), **nu** în dark
- Toolbar nu apare
- Layout intact, font-uri corecte

Anulează print.

- [ ] **Step 6: Verifică `<noscript>`**

În Chrome DevTools, F1 → Settings → Debugger → bifează „Disable JavaScript". Reîncarcă pagina.

Așteptat:
- Toolbar dispare complet (ascunsă via `<noscript>`)
- CV-ul rămâne vizibil în RO + light
- Linkuri din contact bar (`tel:`, `mailto:`, social) tot funcționează

Re-activează JavaScript.

- [ ] **Step 7: Commit**

```bash
git add cv-nichita-ciobu.html
git commit -m "polish: hovers, smooth scroll, reduced-motion, print=light, noscript fallback"
```

---

## Task 9: Deploy + final manual test checklist

**Files:**
- No code changes — doar deploy și verificare

**Rationale:** Toate task-urile anterioare sunt completate. Acum testăm complet și deploy-uim live.

- [ ] **Step 1: Push la GitHub**

```bash
cd /Users/m1/Desktop/nikkiCV
git push
```

Expected: `main -> main` cu commit-urile noi.

- [ ] **Step 2: Update pe droplet**

```bash
ssh root@209.38.217.142 cv-update
```

Expected:
```
From https://github.com/imnikkl/CVnikki
   <hash>..<hash>  main       -> origin/main
Updating <hash>..<hash>
...
CV actualizat la <data>
```

- [ ] **Step 3: Verifică live HTTPS**

```bash
curl -sI https://cv.nikki.software/ | head -3
curl -s https://cv.nikki.software/ | grep -c 'data-ru'
```

Expected:
- `HTTP/2 200`
- Numărul de matches `data-ru` să fie ~25-30 (nu zero) — confirmă că versiunea nouă e live

- [ ] **Step 4: Manual test checklist (din spec, secțiunea 9)**

Deschide `https://cv.nikki.software` în Chrome.

Bifează manual (răspund DA pentru fiecare):

1. CV se vede corect la prima încărcare, default RO + light (sau ce auto-detect a ales), toolbar cu 4 elemente (RO, RU, ☾, Salvează PDF). ☐
2. Click pe RU → toate textele se schimbă în rusă, inclusiv `<title>`, `aria-label`-uri unde e cazul, eyebrow, role, contacte, section headers, profile, bullets job, educație. ☐
3. Click pe ☾ → fundalul se face întunecat, contrast OK, accent vizibil. ☐
4. Refresh pagină → limba + tema rămân (DevTools → Application → Local Storage → `cv-lang`, `cv-theme`). ☐
5. Cmd+P în RU + dark → preview iese în RU + light, fără toolbar. ☐
6. Incognito + browser cu setări `ru-RU` (sau emulează prin DevTools dacă posibil) → prima vizită încarcă direct RU. ☐
7. Mobile (DevTools 375px) → layout pe o coloană, toolbar accesibilă. ☐
8. VoiceOver / NVDA scurt: butoanele RO/RU au `aria-label` citit, `<html lang>` corect. ☐ (opțional)
9. `prefers-reduced-motion: reduce` în DevTools → fără tranziții, scroll instant. ☐
10. Disable JavaScript → CV-ul rămâne vizibil în RO + light, toolbar ascunsă. ☐

- [ ] **Step 5: Dacă vreun check eșuează — raportează & iterează**

Dacă vreun item din checklist eșuează, NU mark plan-ul ca complet. Notează ce nu merge, deschide un task de fix, re-deploy.

- [ ] **Step 6: Mark plan complete**

Dacă toate cele 10 checks trec, planul e gata.

---

## Self-Review

**Spec coverage (verificat punct-cu-punct):**

| Spec section | Acoperit în |
|---|---|
| 3.1 Înlocuiri conținut | Task 1 |
| 3.2 Traducere RU (tabel + bullets + profile) | Tasks 3, 4, 5 (header, sidebar, main) |
| 4.1 Markup data-ro/data-ru | Tasks 3, 4, 5 |
| 4.2 Funcție setLang | Task 7 |
| 4.3 Auto-detect lang | Task 7 (`initialLang`) |
| 4.4 Toolbar markup | Task 2 (markup) + Task 5 step 6 (data-* pe PDF btn) |
| 5.1 Dark palette CSS | Task 6 |
| 5.2 setTheme + auto-detect | Task 7 |
| 5.3 Tranziții smooth + reduced-motion | Task 6 (tranziții) + Task 8 (reduced-motion) |
| 6 Polish interactiv (hovers, scroll) | Task 8 |
| 7 Print override dark→light | Task 8 step 3 |
| 8 Error handling (noscript, localStorage try/catch, fallbacks) | Task 7 (try/catch în JS) + Task 8 (noscript) |
| 9 Testing checklist | Task 9 |

Tot ce e în spec are un task corespunzător. ✓

**Placeholder scan:** Nu există „TODO", „TBD", „implement later", „add error handling" sau alte placeholdere generice. Tot codul e scris explicit. ✓

**Type consistency:** Numele funcțiilor (`setLang`, `setTheme`, `initialLang`, `initialTheme`), atributele (`data-ro`, `data-ru`, `data-lang`), clasele (`.lang-btn`, `.lang-switch`, `.theme-btn`, `.pdf-btn`, `.dark`) sunt consistente între task-uri. ✓

**Ambiguități:** Toate task-urile au cod exact + comenzi exact + așteptări exact.
