# Axborot oqimlari va raqamli muhit bilan ishlash

Ma’lumot, xavfsizlik va unumdorlik ustidan nazoratni saqlash uchun qisqa va amaliy ko‘rsatmalar.

## Mundarija
- [Axborot oqimlari va raqamli muhit bilan ishlash](#axborot-oqimlari-va-raqamli-muhit-bilan-ishlash)
  - [Mundarija](#mundarija)
  - [Asboblar tashkiloti](#asboblar-tashkiloti)
  - [OT tanlovi va xavfsizlik gigiyenasi](#ot-tanlovi-va-xavfsizlik-gigiyenasi)
  - [Smartfonlar: xavfsizlik va unumdorlik](#smartfonlar-xavfsizlik-va-unumdorlik)
  - [Diskni shifrlash](#diskni-shifrlash)
  - [Shaxsiy bilim bazasi](#shaxsiy-bilim-bazasi)
  - [Parollar menejeri](#parollar-menejeri)
  - [Axborot oqimlari bilan ishlash](#axborot-oqimlari-bilan-ishlash)
  - [Tezkor qidiruv misollari](#tezkor-qidiruv-misollari)
  - [Qisqa tekshiruv ro‘yxati](#qisqa-tekshiruv-royxati)
  - [Nega Git faqat dasturchilar uchun emas](#nega-git-faqat-dasturchilar-uchun-emas)
  - [Minnatdorchilik](#minnatdorchilik)

## Asboblar tashkiloti
- Asosiy qurilma: noutbuk — maksimal nazorat; smartfon — yordamchi vosita.

## OT tanlovi va xavfsizlik gigiyenasi
- Linux: ko‘proq nazorat; macOS: barqaror, ammo cheklovli; Windows: qo‘shimcha himoyalash zarur.
- Har doim yangilanishlar va zaxira nusxalar; ilova va havolalar bilan ehtiyot bo‘ling.
- Antivirus: Windows’da majburiy; Linux/macOS’da xavf darajasiga qarab.

## Smartfonlar: xavfsizlik va unumdorlik
- Tizim va xavfsizlik yamoqlarini yangilab boring.
- Bildirishnomalarni minimal darajada qoldiring; faqat muhim kanallar va qo‘ng‘iroqlar.
- Ilovalarni faqat ishonchli manbalardan o‘rnating.

## Diskni shifrlash
- Noutbuklarda to‘liq diskni shifrlang: LUKS (Linux), FileVault (macOS), BitLocker (Windows). Tiklash kalitlarini oflayn saqlang.

## Shaxsiy bilim bazasi
- Mahalliy Markdown bazani Git ostida saqlang; shaxsiy repozitoriylarga zaxiralang.
- Shuningdek qarang: [Samaradorlik vositalari qo‘llanmasi](./productivity-tools-guide.md).

## Parollar menejeri
- KeePassXC (mahalliy baza), kuchli master-parol va oflayn zaxiralar.

## Axborot oqimlari bilan ishlash
- Noaktiv chatlarni arxivlang, bildirishnomalarni cheklang, chuqur ish oynalarini himoya qiling. Brauzer profillari/konteynerlari va klaviatura yorliqlaridan foydalaning.

## Tezkor qidiruv misollari
- Manzil satridagi qisqa prefikslar kerakli xizmat bo‘yicha darhol qidirishni beradi.

Qanday sozlash:
1) Chrome → Sozlamalar → Qidiruv tizimi → Qidiruv tizimlari va sayt qidiruvini boshqarish.
2) “Qo‘shish” tugmasi → Nomi, kalit so‘zi va `%s` o‘rnini bosuvchi URL’ni kiriting.

Misollar (nusxa olish uchun qulay jadval):

| Nom | Kalit | URL |
|---|---|---|
| Private Meet | pmt | https://meet.google.com/new |
| Private Mail | pm | https://mail.google.com/mail/u/0/#inbox |
| Private Calendar | pc | https://calendar.google.com/calendar/u/0/r |
| Yandex Mail | ym | https://mail.yandex.ru/ |
| GitHub Issues | gi | https://github.com/search?q=%s&type=issues |
| Stack Overflow | so | https://stackoverflow.com/search?q=%s |
| Reverso Conjugator | cj | https://conjugator.reverso.net/conjugation-english-verb-%s.html |
| Yandex Translate | tt | https://translate.yandex.ru/?text=%s |
| Yandex Search | yy | https://ya.ru/search/?text=%s |
| npms | ns | https://npms.io/search?q=%s |
| DevDocs | d | https://devdocs.io/#q=%s |
| Merriam‑Webster | mw | https://www.merriam-webster.com/dictionary/%s |
| Cambridge Dictionary | cd | https://dictionary.cambridge.org/dictionary/english-russian/%s |

<a id="qisqa-tekshiruv-royxati"></a>
## Qisqa tekshiruv ro‘yxati
- Mahalliy, Git bilan versiyalangan bilim bazasi va rivojlanish rejasi.
- Yangilanishlar va zaxiralar yoqilgan ([3‑2‑1 qoidasi](https://www.backblaze.com/blog/the-3-2-1-backup-strategy/)).
- Disk shifrlangan; tiklash kalitlari oflayn saqlangan.
- Kuchli master-parolli mahalliy parol menejeri va zaxira nusxasi.

## Nega Git faqat dasturchilar uchun emas
Git — https://git-scm.com — dasturiy ta’minotdan tashqarida ham keng qo‘llaniladigan sanoat standarti.

- Tizim administratorlari: konfiguratsiyalar, avtomatlashtirish skriptlari va infratuzilma hujjatlarini versiyalash; barqaror holatlarga tez qaytish.
- DevOps muhandislari: IaC boshqaruvi (Terraform — https://www.terraform.io, Ansible — https://www.ansible.com), CI/CD pipeline’lari va konteyner konfiguratsiyalari.
- Test muhandislari: test-keslar, avtotestlar, ma’lumot to‘plamlari va hisobotlarni saqlash; rivojlanish bilan uyg‘un qoplamani kuzatish.
- Tahlilchilar/data‑siyentistlar: SQL, pipeline, modellar va hisobotlarni versiyalash; tajribalarni takrorlanishini ta’minlash.
- Texnik yozuvchilar va product‑menejerlar: hujjatlar, talablar va spetsifikatsiyalar (Markdown) — o‘zgarishlar tarixi va hamkorlik bilan.
- Asosiy afzalliklar: o‘zgarishlar tarixi, hamkorlik, zaxiralash, tajribalar uchun tarmoqlanish va standartlashtirilgan jamoaviy ish oqimi — Git IT’ning lingua franca’si.

Minimal ko‘nikmalar: klonlash/commit/branch, PR/MR ochish, konfliktlarni hal qilish; shaxsiy bilimlar va konfiguratsiyalarni repozitoriylarda saqlang.

<a id="bagishlov"></a>
## Minnatdorchilik
  Loyiha xonim Allaning tashabbusi bilan boshlandi. O‘zbekcha lokalizatsiya janob Davlatbek sharafiga bag‘ishlangan.
