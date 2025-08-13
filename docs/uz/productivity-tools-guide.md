# Ma’lumot bilan ishlash uchun samaradorlik vositalari

## Mundarija

- [Vositalar jadvali](#vositalar-jadvali)
- [Ma’lumotlarni saqlash bo‘yicha muhim tavsiya](#malumotlarni-saqlash-boyicha-muhim-tavsiya)
- [Qisqa sharhlar](#qisqa-sharhlar)
- [Obsidian — Graf shaklidagi bilim bazasi](#obsidian--graf-shaklidagi-bilim-bazasi)
- [VS Code + Markdown — Kod sifatida bilim](#vs-code--markdown--kod-sifatida-bilim)
- [Todoist — Vazifalar va loyihalarni boshqarish](#todoist--vazifalar-va-loyihalarni-boshqarish)
- [Kontekstlarni almashtirish bo‘yicha umumiy tavsiyalar](#kontekstlarni-almashtirish-boyicha-umumiy-tavsiyalar)
- [Bilim va vazifalarni boshqarish metodologiyalari](#bilim-va-vazifalarni-boshqarish-metodologiyalari)
- [Shaxsiy metodologiyani rivojlantirish](#shaxsiy-metodologiyani-rivojlantirish)
- [O‘rganish uchun tekshiruv ro‘yxati (chop etish uchun)](#organish-uchun-tekshiruv-royxati-chop-etish-uchun)
- [Obsidian sozlamalari (sozlamalar va plaginlar)](#obsidian-sozlamalari-sozlamalar-va-plaginlar)
- [VSCode kengaytmalari (qisqa tugmalar va kengaytmalar)](#vscode-kengaytmalari-qisqa-tugmalar-va-kengaytmalar)
- [Todoist funksiyalari (funksiyalar va integratsiyalar)](#todoist-funksiyalari-funksiyalar-va-integratsiyalar)

## Vositalar jadvali

| Vosita | Rasmiy sayt | Mashhurlik | Bilim turi | Saqlash | Tuzilma | Kontekstni almashtirish | Asosiy maqsad |
|-------|--------------|------------|------------|--------|---------|-------------------------|---------------|
| **Obsidian** | [obsidian.md](https://obsidian.md/) | ⭐⭐⭐⭐⭐ | Bilim grafigi | Mahalliy .md fayllar | Havolalar, teglar, graf | Qidiruv, backlinklar, graf | Uzoq muddatli bilim bazasi |
| **VS Code + Markdown** | [code.visualstudio.com](https://code.visualstudio.com/) | ⭐⭐⭐⭐⭐ | Fayl arxitekturasi | Papkalar + Git versiyalash | Papkalar, TODO Tree, xatcho‘plar | Qisqa tugmalar, navigatsiya | Texnik hujjatlar |
| **Todoist** | [todoist.com](https://todoist.com/) | ⭐⭐⭐⭐⭐ | Vazifa boshqaruvi | Bulut + mahalliy kesh | Loyihalar, yorliqlar, filtrlar | Tez qo‘shish, bildirishnomalar | Vazifalarni boshqarish |

Mashhurlik sabablari:
- **Obsidian** (⭐⭐⭐⭐⭐): Shaxsiy bilim bazasi yaratishda juda mashhur; katta hamjamiyat va boy plagin ekotizimi.
- **VS Code** (⭐⭐⭐⭐⭐): Dunyodagi eng ommabop kod muharriri; kuchli hamjamiyat va doimiy rivojlanish.
- **Todoist** (⭐⭐⭐⭐⭐): Millionlab foydalanuvchilarga ega yetakchi vazifa menejeri.

## Ma’lumotlarni saqlash bo‘yicha muhim tavsiya

Asosiy bilim bazasi sifatida bulutli ilovalardan foydalanmang.

G‘oyalarni tezda yig‘ish uchun Todoist, Telegram va boshqa bulutli xizmatlardan foydalanishingiz mumkin, lekin asosiy bilim bazangiz quyidagicha bo‘lishi shart:

- Kompyuteringizda mahalliy saqlansin.
- Git ostida versiyalansin.
- GitHub — https://github.com yoki GitLab.com — https://gitlab.com dagi shaxsiy repozitoriylarda zaxira nusxalari bo‘lsin.

Nega muhim: bulutdagi ma’lumot o‘chirilishi yoki yopilishi mumkin; siyosatlar o‘zgaradi, akkauntlar bloklanadi, servislar yopiladi. Bilimingiz — sizning kapitalingiz; uni o‘zingiz nazorat qiling.

Tavsiya etilgan ish oqimi:
1. Fikrlarni mobil ilovalarda (Todoist, Telegram) tez yozib oling.
2. Muhimlarini muntazam ravishda mahalliy bazaga (Obsidian, VS Code) ko‘chiring.
3. O‘zgarishlarni Git’da commit qiling va shaxsiy repozitoriylarga push qiling.
4. Qurilmalarni Syncthing — https://syncthing.net orqali sinxronlang (bulutsiz, versiyalash bilan).

## Qisqa sharhlar

### Obsidian — Graf shaklidagi bilim bazasi

Rasmiy sayt: [https://obsidian.md/](https://obsidian.md/)

Saqlash va tuzilma: Obsidian ma’lumotni mahalliy Markdown fayllarda saqlaydi. Asosiy xususiyati — `[[...]]` havolalari orqali bilim grafigini yaratish.

Kontekstni almashtirish: to‘liq matnli qidiruv, bэкlinklar orqali navigatsiya va interaktiv graf. Ochiq yozuvlar o‘rtasida tez o‘tish paneli.

Rus tilidagi manbalar:
- [@obsidianrus Telegram kanali](https://t.me/obsidianrus) — sozlamalar, plaginlar, yangiliklar.
- [Rasmiy ruscha qo‘llanma](https://publish.obsidian.md/help-ru/)
- [Habr maqolalar to‘plami](https://habr.com/ru/articles/710508/) — Obsidian’da bilimlarni boshqarish.

---

### VS Code + Markdown — Kod sifatida bilim

Rasmiy sayt: [https://code.visualstudio.com/](https://code.visualstudio.com/)

Saqlash va tuzilma: Ierarxik papkalar, Git versiyalash va nomlash qoidalari. “Bilim — kod” yondashuvi: har bir yozuv — fayl, har bir papka — mavzu.

Kontekstni almashtirish: kuchli qisqa tugmalar, bir nechta fayl bilan ishlash, integratsiyalashgan terminal. TODO Tree kengaytmasi loyihadagi vazifalarga tez kirish imkonini beradi.

Rus tilidagi manbalar:
- [Hujjatlar uchun 28 ta VS Code kengaytmasi](https://habr.com/ru/articles/698702/)
- [VS Code’ning TOP-10 kengaytmalari](https://proglib.io/p/10-vscode-extensions)

---

### Todoist — Vazifalar va loyihalarni boshqarish

Rasmiy sayt: [https://todoist.com/](https://todoist.com/)

Saqlash va tuzilma: Bulutli saqlash, mahalliy kesh va oflayn rejim. Loyihalar, yorliqlar va filtrlar bo‘yicha tashkil etiladi. Tabiiy til bilan sanalarni aniqlaydi.

Kontekstni almashtirish: tez kiritish, vaqt va geolokatsiya bo‘yicha eslatmalar, kontekstlarni tez almashtirish uchun filtrlar. Karma tizimi orqali gamifikatsiya.

Rus tilidagi manbalar:
- [Sky.pro sharhi](https://sky.pro/wiki/lifestyle/obzor-todoist-kak-organizovat-svoi-zadachi/)
- [Todoist’da ishni tashkil etish usullari](https://vc.ru/u/660293-julia-cores/185637-upravlenie-zadachami-sposoby-organizacii-zadach-i-raboty-v-todoist)

## Kontekstlarni almashtirish bo‘yicha umumiy tavsiyalar

### Mahalliy (local‑first) tamoyili

Ma’lumot xavfsizligi: asosiy bilim bazangiz mahalliy bo‘lsin — bulutli xizmatlar yo‘qolishi yoki kirishni cheklashi mumkin. Bulutdan faqat nusxalarni sinxronlash uchun foydalaning.

Versiyalash uchun Git: asosiy bazani Git ostida yuriting va shaxsiy repozitoriylarda zaxira qiling (GitHub — https://github.com / GitLab.com — https://gitlab.com).

### Qurilmalar o‘rtasida sinxronlash

Syncthing: Syncthing — https://syncthing.net orqali noutbuk, server, planshet va telefonni sinxronlang. Versiyalashni ta’minlaydi va bulutsiz ishlaydi.

Amaliy tuzilma misoli:
- Todoist — tez eslatmalar va kichik vazifalar.
- Obsidian — batafsil, asosiy bilim bazasi.
- Syncthing — Obsidian’ni barcha qurilmalarda (noutbuk, server, planshet, telefon) avtomatik versiyalash bilan sinxronlash.
- Natija — internetga qaram bo‘lmagan holda har qanday qurilmadan to‘liq kirish.

Mobil kirish: yo‘lda — Todoist’ga qayd qiling, so‘ng muhimlarini asosiy bazaga ko‘chiring.

### Kontekstni almashtirish strategiyalari

Vositalar ierarxiyasi:
1. Todoist — fikrlar va vazifalarni tez yig‘ish.
2. Asosiy bilim bazasi (Obsidian/VS Code) — tuzilgan saqlash.
3. Git — versiyalash va uzoq muddatli saqlash.

Emotsional posting: fikr paydo bo‘lishi bilanoq yozib qo‘ying — voqea, kurs, yangi bilimdan so‘ng darhol qayd qiling.

Tezkor qaydlar: mayda yozuvlarni Todoist yoki shaxsiy Telegram chatda saqlang, lekin muhimlarini muntazam ravishda asosiy bazaga ko‘chiring.

---

## Obsidian sozlamalari (sozlamalar va plaginlar)

### Hamjamiyatlar va manbalar

Telegram kanallari:
- [@obsidianrus](https://t.me/obsidianrus) — sozlamalar, plaginlar, maslahatlar, yangiliklar.
- [@Second_brain_ru](https://t.me/second_brain_ru) — shaxsiy bilim bazalarini qurish usullari.

O‘quv materiallari:
- [Rasmiy ruscha qo‘llanma](https://publish.obsidian.md/help-ru/Начните+здесь)
- [Habr maqolalar to‘plami](https://habr.com/ru/articles/710508/) — Obsidian’da bilimlarni boshqarish.
- [Oddiydan murakkab struktura sari](https://habr.com/ru/articles/796899/) — amaliy tajriba.

### Asosiy imkoniyatlar

Bilim grafigi: yozuvlar o‘rtasidagi bog‘lanishlarni interaktiv graf orqali ko‘rsatish — yashirin aloqalarni topishga yordam beradi.

Backlinklar: teskari havolalarni avtomatik kuzatish — joriy yozuvga ishora qilgan barcha yozuvlarni ko‘rasiz.

Mahalliy saqlash: barcha ma’lumot .md fayllarda saqlanadi. Sozlamalar `.obsidian/` katalogida.

Plaginlar: kalendardan tortib tashqi integratsiyalargacha — turli ehtiyojlar uchun kengaytmalar.

### Tavsiya etilgan plaginlar

- [Templater](https://github.com/SilentVoid13/Templater) — shablonlar va tez yozuvlar.
- [Calendar](https://github.com/liamcain/obsidian-calendar-plugin) — kalendar ko‘rinishi.
- [Tag Wrangler](https://github.com/pjeby/tag-wrangler) — teglarni boshqarish.
- [Advanced Tables](https://github.com/tgrosinger/advanced-tables-obsidian) — jadvallar bilan qulay ishlash.

---

## VSCode kengaytmalari (qisqa tugmalar va kengaytmalar)

### Navigatsiya

Asosiy qisqa tugmalar:
- `Ctrl+B` — yon panelni yoqish/o‘chirish.
- `Ctrl+Shift+P` — ota blokka o‘tish.
- `Alt+Shift+-` — navigatsiya tarixida orqaga.
- `Alt+Shift+=` — navigatsiya tarixida oldinga.
- `Shift+F10` — Debug: kursorgacha ishga tushirish.
- `Alt+Shift+F12` — barcha murojaatlarni topish.
- `Ctrl+J` — pastki panelni yoqish/o‘chirish.
- `Ctrl+I` — panelni maksimal qilish/rejimdan chiqish.
- `Ctrl+L` — terminalni tozalash (terminal fokusida).

Kod bilan ishlash:
- `Ctrl+Shift+[` — blokni yig‘ish.
- `Ctrl+Shift+]` — blokni yozish.
- `Alt+↑` — qatorni yuqoriga ko‘chirish.
- `Alt+↓` — qatorni pastga ko‘chirish.

Panellar:
- `Alt+Shift+L` — Source Control oynasiga fokus.
- `Alt+Shift+D` — Run and Debug oynasini ko‘rsatish.
- `Alt+Shift+G` — GitHub oynasini ko‘rsatish.
- `Alt+Shift+E` — Explorer oynasini ko‘rsatish.
- `Alt+Shift+F` — Qidiruv oynasini ko‘rsatish.
- `Alt+Shift+O` — Outline oynasiga fokus.

### Markdown uchun tavsiya etilgan kengaytmalar

Asosiylar:
- Markdown All in One — qisqa tugmalar, TOC, formulalar.
- GitLens — ilg‘or Git vositalari.
- Bookmarks — kod ichida xatcho‘p.

Loyihalarda ish:
- GitHub Pull Requests — PR’lar bilan ishlash.
- ESLint — kod lintingi.
- Search node_modules — qaramliklar bo‘yicha qidiruv.

Hujjatlar:
- SQL Formatter — SQL formatlash.
- Todo Tree — loyiha bo‘ylab TODO’larni kuzatish.
- YAML — YAML qo‘llab‑quvvatlash.

### Buyruqlar

Foydali buyruqlar:
- Open file on remote — masofadagi hostdagi faylni ochish.
- Copy line down — joriy qatorni nusxalash.

---

## Todoist funksiyalari (funksiyalar va integratsiyalar)

### Asosiy imkoniyatlar

GTD metodologiyasi: Todoist [Getting Things Done](https://gettingthingsdone.com/) uchun yaratilgan — aniq tashkil etilgan vazifalar miyaning resurslarini bo‘shatadi.

Aqlli kiritish: tabiiy til bilan sanalarni tanish — “ertaga 15:00”, “har dushanba”.

Gamifikatsiya: karma tizimi; kunlik maqsad va yutuqlar muntazam ishlatishga undaydi.

### Tuzilma

Loyihalar: hayot va ish sohalari bo‘yicha guruhlash.

Yorliqlar: kontekst bo‘yicha — @qo‘ng‘iroqlar, @kompyuter, @uy.

Filtrlar: ko‘rinishlarni tez almashtirish — “bugun”, “muddat o‘tgan”, “ustuvor”.

### Integratsiyalar va sinxronlash

Ko‘p platformalilik: iOS, Android, Windows, macOS, veb.

Oflayn rejim: internet bo‘lmasa ham ishlash, ulanish bilan sinxronlash.

Bildirishnomalar: vaqt, geolokatsiya, qurilmalar bo‘ylab.

Fayllar: Google Drive, Dropbox orqali biriktirish.

### Boshqa vositalar bilan qo‘llash

G‘oyalarni yig‘ish: fikrlarni tez yozing, so‘ng asosiy bilim bazasiga ko‘chiring.

Kontekst boshqaruvi: ish/shaxsiy/proektlar o‘rtasida tez o‘tish uchun filtrlar.

Emotsional posting: bir zumlik g‘oyalarni yozib, keyin Obsidian yoki VS Code’da tuzilmaga soling.

---

## Bilim va vazifalarni boshqarish metodologiyalari

### Getting Things Done (GTD)
- Rasmiy sayt: [https://gettingthingsdone.com/](https://gettingthingsdone.com/)
- Rus tilidagi manba: [Habr’da GTD](https://habr.com/ru/articles/50821/)
- Tavsif: vazifalarni eslab qolish zaruratidan “boshni bo‘shatadigan” tizim.

### Kanban
- Rasmiy sayt: [https://kanbanflow.com/](https://kanbanflow.com/)
- Rus tilidagi manba: [Habr’da Kanban](https://habr.com/ru/articles/488304/)
- Tavsif: doskalar va kartochkalar asosidagi vizual boshqaruv.

### Zettelkasten (kartochkalar tizimi)
- Rasmiy sayt: [https://zettelkasten.de/](https://zettelkasten.de/)
- Rus tilidagi manba: [Habr’da Zettelkasten](https://habr.com/ru/articles/508672/)
- Tavsif: bog‘langan yozuvlar orqali bilimlarni jamlash va rivojlantirish usuli.

## Shaxsiy metodologiyani rivojlantirish

### Ehtiyojni aniq ifodalash

Asosiy g‘oya: tayyor vositalarga moslashmasdan, o‘z ehtiyojingizni aniqlang va shunga mos vositalarni tanlang.

Iteratsion sikl:

1. Muammolarni aniqlash
   - Joriy jarayonda aynan nimalar ishlamayapti?
   - Qayerda vaqt yoki ma’lumot yo‘qoladi?
   - Qaysi vazifalar eng ko‘p qarshilik tug‘diradi?

2. Ehtiyojni formulalash
   - “Loyihaga oid bog‘liq yozuvlarni tez topishim kerak.”
   - “Uzoq muddatli maqsadlar bo‘yicha progressni kuzatishim kerak.”
   - “Bulutsiz, qurilmalar o‘rtasida vazifalarni sinxronlashim kerak.”

3. Yechimlarni qidirish
   - Avval mavjud vositalarda (sozlamalar, plaginlar).
   - So‘ng alternativ yondashuvlar.
   - Oxirgi chora — o‘zingizga mos yechim yaratish.

4. Iteratsiya va moslashtirish
   - 2–4 hafta sinov.
   - Samaradorlik tahlili.
   - Moslashtirish yoki yondashuvni almashtirish.

### Metodologiyani rivojlantirish misollari

Ehtiyoj: “Proektlar o‘rtasida tez o‘tish”
- Yechim 1: VS Code’da proekt papkalarini ochish uchun qisqa tugmalar.
- Yechim 2: Har turdagi proektlar uchun Obsidian shablonlari.
- Yechim 3: Kontekstlarni filtrlash uchun Todoist yorliqlari.

Ehtiyoj: “Ta’lim va progressni kuzatish”
- Yechim 1: Todoist’da “O‘qish/O‘rganish” proekti va quyi vazifalar.
- Yechim 2: Obsidian’da mavzulararo havolali o‘quv jurnali.
- Yechim 3: Kombinatsiya — maqsadlar Todoist’da, batafsil yozuvlar Obsidian’da.

### Samarali metodologiya mezonlari

Joriy etish osonligi: boshlash uchun minimal kuch.  
Tabiiy moslik: fikrlash uslubingizga mos kelishi.  
Masshtablanuvchanlik: ma’lumot hajmi oshganda ham ishlashi.  
Ishonchlilik: tashqi servis va texnologiyalarga minimal qaramlik.

### Shaxsiy tajribani hujjatlashtirish

Eksperiment jurnalini yuriting:
- Boshlanish sanasi.
- Muammo bayoni.
- Tanlangan yechim.
- 2–4 haftadan keyingi natija.
- Qaror: davom ettirish/sozlash/voz kechish.

Bu sizga aynan o‘zingiz uchun ishlaydigan usullar bo‘yicha shaxsiy bilim bazasini yaratadi.

---

## O‘rganish uchun tekshiruv ro‘yxati (chop etish uchun)

### Asosiy vositalar (majburiy)

□ Obsidian — Bilim bazasi
- [ ] O‘rnatish va sozlash (30 daqiqa)
- [ ] [Rasmiy ruscha qo‘llanma](https://publish.obsidian.md/help-ru/) (2 soat)
- [ ] [Habr maqolalar to‘plami](https://habr.com/ru/articles/710508/) (1 soat)
- [ ] Amaliyot: ilk yozuvlar (1 soat)
- Jami: 4,5 soat

□ VS Code + Markdown — Texnik hujjatlar
- [ ] VS Code o‘rnatish (15 daqiqa)
- [ ] Markdown kengaytmalarini o‘rnatish (15 daqiqa)
- [ ] [Hujjatlar uchun 28 ta kengaytma](https://habr.com/ru/articles/698702/) (30 daqiqa)
- [ ] Qisqa tugmalarni sozlash (30 daqiqa)
- [ ] Amaliyot: hujjatlar loyihasini yaratish (1 soat)
- Jami: 2,5 soat

□ Todoist — Vazifalarni boshqarish
- [ ] Ro‘yxatdan o‘tish va o‘rnatish (15 daqiqa)
- [ ] [Sky.pro sharhi](https://sky.pro/wiki/lifestyle/obzor-todoist-kak-organizovat-svoi-zadachi/) (30 daqiqa)
- [ ] [Tashkil etish usullari](https://vc.ru/u/660293-julia-cores/185637-upravlenie-zadachami-sposoby-organizacii-zadach-i-raboty-v-todoist) (30 daqiqa)
- [ ] Loyihalar va yorliqlarni sozlash (30 daqiqa)
- [ ] Amaliyot: vazifa tizimini yaratish (45 daqiqa)
- Jami: 2,5 soat

### Metodologiyalar (tanlab o‘rganing)

□ Getting Things Done (GTD)
- [ ] [Habr’da GTD](https://habr.com/ru/articles/50821/) (45 daqiqa)
- [ ] Amaliyot: GTD tamoyillarini joriy etish (1 soat)
- Jami: 1,75 soat

□ Kanban
- [ ] [Habr’da Kanban](https://habr.com/ru/articles/488304/) (30 daqiqa)
- [ ] Amaliyot: kanban-doskani yaratish (30 daqiqa)
- Jami: 1 soat

□ Zettelkasten
- [ ] [Habr’da Zettelkasten](https://habr.com/ru/articles/508672/) (45 daqiqa)
- [ ] Amaliyot: bog‘langan yozuvlar (1 soat)
- Jami: 1,75 soat

### Qo‘shimcha sozlamalar (zaruratga ko‘ra)

□ Syncthing — Bulutsiz sinxronlash
- [ ] [Syncthing.net](https://syncthing.net/) — o‘rnatish (30 daqiqa)
- [ ] Qurilmalar o‘rtasida sinxronlash (45 daqiqa)
- Jami: 1,25 soat

□ Git + shaxsiy repozitoriylar
- [ ] Hujjatlar uchun Git sozlash (30 daqiqa)
- [ ] Shaxsiy repozitoriy yaratish (15 daqiqa)
- [ ] Bilim bazasining birinchi commit’i (15 daqiqa)
- Jami: 1 soat

---

To‘liq o‘zlashtirish uchun vaqt: 15–20 soat.  
Minimal to‘plam (Obsidian + Todoist): 7 soat.

---
