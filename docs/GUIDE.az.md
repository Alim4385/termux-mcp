<p align="center">
  <a href="GUIDE.en.md">🇬🇧 English</a> •
  <a href="GUIDE.az.md"><strong>🇦🇿 Azərbaycanca</strong></a> •
  <a href="GUIDE.tr.md">🇹🇷 Türkçe</a>
</p>

<p align="center"><a href="README.az.md">← Haqqında səhifəsinə qayıt</a></p>

# Tam Quraşdırma və İstifadə Təlimatı

Bu sənəd elə yazılıb ki, **Termux haqqında heç nə bilməsən belə**, başdan sona addım-addım gedərək hər şeyi qura biləsən. Tələsmə, hər addımı sırayla et.

> ⚠️ Bu server telefonuna qoşulan AI-yə **real bash shell girişi** verir. Aşağıdakı **Təhlükəsizlik** bölməsini mütləq oxu, xüsusən linki kiminləsə paylaşmazdan əvvəl.

## Məzmun

1. [Termux-u quraşdır](#1-termux-u-quraşdır)
2. [Layihəni telefonuna endir](#2-layihəni-telefonuna-endir)
3. [Serveri işə sal](#3-serveri-işə-sal)
4. [İkinci pəncərə (sessiya) aç](#4-ikinci-pəncərə-sessiya-aç)
5. [İnternetə tunel aç (cloudflared)](#5-internetə-tunel-aç-cloudflared)
6. [AI-ə qoşulma — Claude](#6-ai-ə-qoşulma--claude)
7. [AI-ə qoşulma — ChatGPT](#7-ai-ə-qoşulma--chatgpt)
8. [Təhlükəsizlik və İmkanlar](#8-təhlükəsizlik-və-i̇mkanlar)
9. [Məsuliyyətin Rədd Edilməsi](#9-məsuliyyətin-rədd-edilməsi)

---

## 1. Termux-u quraşdır

⚠️ **Google Play-dəki Termux-u YÜKLƏMƏ** — o versiya artıq yenilənmir və işləmir. Yalnız bu mənbələrdən birindən yüklə:

- [F-Droid (tövsiyə olunur)](https://f-droid.org/packages/com.termux/)
- [GitHub Releases](https://github.com/termux/termux-app/releases)

F-Droid-i telefonuna quraşdırmamısansa, əvvəlcə F-Droid tətbiqini yüklə (F-Droid özü bir app store-dur), sonra onun daxilindən Termux-u axtarıb quraşdır. Ya da birbaşa GitHub Releases səhifəsindən `.apk` faylını endirib qura bilərsən (quraşdırarkən telefon "naməlum mənbələrə icazə ver" deyə soruşacaq — buna razı ol).

Quraşdırdıqdan sonra Termux-u aç. Qara ekranda mətn görəcəksən — bu, terminal-dır, burada əmrlər yazacaqsan.

---

## 2. Layihəni telefonuna endir

Termux açıldıqda, aşağıdakıları **bir-bir** yaz və hər birindən sonra Enter-ə bas (kopyala-yapışdır da edə bilərsən):

```bash
pkg update && pkg upgrade -y
```
(Bu, Termux-un öz paketlərini yeniləyir. Bir az vaxt apara bilər, gözlə.)

```bash
pkg install nodejs git -y
```
(Bu, Node.js və git-i quraşdırır — server bunlarsız işləməyəcək.)

```bash
git clone https://github.com/Alim4385/termux-mcp.git
```
(Bu, layihəni GitHub-dan telefonuna köçürür.)

```bash
cd termux-mcp
```
(Bu, yaradılmış qovluğa daxil olur.)

---

## 3. Serveri işə sal

```bash
npm start
```

Ekranda bunun kimi bir şey görəcəksən:

```
⚠️  BETA — Termux MCP Server
🌐 http://127.0.0.1:3000/mcp
📁 /data/data/com.termux/files/home/claude_workspace
```

Bu görünürsə, **server işə düşüb** və bu pəncərədə açıq qalmalıdır. **Bu pəncərəni bağlama** — server dayanacaq. İndi növbəti addıma keçək: eyni anda başqa əmrlər yazmaq üçün ikinci bir pəncərə (sessiya) açmalısan.

---

## 4. İkinci pəncərə (sessiya) aç

Server işlədiyi pəncərəni bağlamadan, Termux-da **yeni bir sessiya** aça bilərsən:

1. Ekranın **sol kənarından sağa doğru sürüşdür (swipe)** — bu, gizli bir yan menyu (drawer) açacaq
2. Açılan menyuda **"NEW SESSION"** (Yeni sessiya) yazısını görəcəksən — ona bas
3. İndi tam yeni, boş bir terminal pəncərəsi açıldı — server hələ də arxa fonda birinci pəncərədə işləyir

(Əgər drawer açılmırsa, ekranın yuxarı sol küncündə kiçik ox/hamburger işarəsi ola bilər — ona basmaqla da eyni menyu açılır.)

Sessiyalar arasında keçid etmək üçün də eyni sürüşdürmə hərəkətini istifadə edirsən — hər açdığın sessiya siyahıda görünəcək, üstünə basıb keçə bilərsən.

---

## 5. İnternetə tunel aç (cloudflared)

İndi **yeni açdığın ikinci pəncərədə** aşağıdakıları yaz:

```bash
pkg install cloudflared -y
```

```bash
cloudflared tunnel --url http://127.0.0.1:3000
```

Bir neçə saniyə gözlə, ekranda bu formada bir sətir görəcəksən:

```
https://random-sözlər-buraya.trycloudflare.com
```

**Bu linki kopyala.** Bu, telefonunun internetdəki müvəqqəti ünvanıdır.

⚠️ **Bu pəncərəni də bağlama** — bağlasan, tunel dayanır, link ölür.

Kopyaladığın linkin **sonuna `/mcp` əlavə et**. Yəni:

```
https://random-sözlər-buraya.trycloudflare.com/mcp
```

Bu tam linkdir ki, AI-ə verəcəksən. Bunu bir yerə (qeydlərə, məsələn) yapışdır ki, itirməyəsən — hər dəfə `cloudflared`-i yenidən başladanda bu link **dəyişir**.

---

## 6. AI-ə qoşulma — Claude

**Tələb olunan:** claude.ai hesabı (pulsuz hesab da işləyir, amma yalnız 1 custom connector əlavə edə bilərsən; Pro/Max-da limit yoxdur).

1. [claude.ai](https://claude.ai)-a daxil ol (brauzerdən və ya mobil tətbiqdən)
2. Sol tərəfdəki menyudan **Settings → Connectors** bölməsinə get (mobil tətbiqdə: profil ikonuna bas → **Settings** → **Connectors**)
3. **"+"** düyməsinə bas
4. **"Add custom connector"** seç
5. Açılan pəncərədə:
   - **Name**: istədiyin ad, məs. `Mənim Telefonum`
   - **URL**: 5-ci addımda aldığın linki yapışdır (`.../mcp` ilə bitən)
6. **"Add"** bas

Qoşulduqdan sonra, hər söhbətdə bunu aktivləşdirmək lazımdır:
1. Söhbət ekranında aşağı sol küncdəki **"+"** düyməsinə bas
2. **"Connectors"** seç
3. Yaratdığın connector-un yanındakı düyməni aç (toggle)

İndi Claude-a yaz, məsələn: *"Telefonumda hansı fayllar var, bax"* — Claude sənin server-ə qoşulub bash əmri işlədəcək.

---

## 7. AI-ə qoşulma — ChatGPT

**Tələb olunan:** ChatGPT Plus, Pro, Team, Enterprise və ya Edu hesabı. **Pulsuz (Free) hesabda bu funksiya yoxdur.**

1. [chatgpt.com](https://chatgpt.com)-a daxil ol
2. Profil ikonuna bas (aşağı sol/sağ küncdə) → **Settings**
3. **Connectors** (və ya **Apps & Connectors**) bölməsinə get
4. **Advanced** (aşağıda) → **Developer mode**-u aktivləşdir (toggle) — bir xəbərdarlıq mesajı çıxacaq ki, "custom connector-lar üçüncü tərəf kodunu işlədir" — qəbul et
5. İndi **"Add custom connector"** (və ya **"Create connector"**) düyməsi görünəcək, ona bas
6. Doldur:
   - **Name**: istədiyin ad
   - **URL**: 5-ci addımda aldığın linki yapışdır (`.../mcp` ilə bitən)
   - **Authentication**: `No authentication` seç (bu server token istifadə etmir)
7. Yadda saxla

**Diqqət:** ChatGPT-də connector əlavə etmək kifayət etmir — **hər yeni söhbətdə** onu ayrıca aktivləşdirməlisən:
1. Söhbət qutusunda **"+"** düyməsinə bas
2. **"Developer mode"** (və ya **"More"**) seç
3. Connector-unu siyahıdan tap və aç (toggle)

İndi ChatGPT-yə yaz, o da server-ə qoşulub əmr işlədə biləcək. Adətən hər əmrdən əvvəl ChatGPT səndən təsdiq istəyəcək — bu normaldır, "Run"/"Icra et" kimi bir düyməyə basmalı olacaqsan.

---

## 8. Təhlükəsizlik və İmkanlar

Bu serverin yeganə aləti `exec`-dir — ixtiyari bash icrası. Bunun cihazın üçün *real olaraq* nə demək olduğu, başqa nə quraşdırılıb/aktivdirsə ona bağlıdır:

**Əgər [Termux:API](https://wiki.termux.com/wiki/Termux:API) quraşdırılıbsa** (`pkg install termux-api` + Termux:API tətbiqi), bu serverə qoşulan istənilən AI, prinsipcə, bu kimi əmrləri çağıra bilər:
- `termux-sms-list` / `termux-sms-send` — SMS oxumaq/göndərmək
- `termux-contact-list` — kontaktları oxumaq
- `termux-camera-photo` — şəkil çəkmək
- `termux-location` — GPS lokasiyasını almaq
- `termux-clipboard-get/set`, `termux-notification`, `termux-battery-status`, `termux-microphone-record` və s.

Termux:API-ni yalnız qoşulan AI-nin bunları potensial istifadə edə bilməsinə rahat olduğun halda quraşdır.

**Əgər cihaz root olunubsa:** root shell sistem partisiyalarını dəyişə, digər tətbiqlərin şəxsi datasına girə, firewall qaydalarını dəyişə bilər. Bu serveri root kimi işlətmək **tövsiyə olunmur**.

**ADB haqqında:** ADB girişi bu serverlə birbaşa əlaqəli deyil, amma telefonuna ADB girişi olan hər kəs demək olar ki tam nəzarətə malikdir (tətbiq qurmaq/silmək, faylları çıxarmaq, ekranı güzgüləmək). ADB girişini heç vaxt bu serverin tunel linki ilə birlikdə paylaşma.

**Tunel linki barədə:** `cloudflared` linki heç bir parolla qorunmur. Onu ictimai paylaşma (TikTok/screenshot-larda göstərəndə linki mütləq gizlət/qara xətt çək), server açıq olduqca linkin kimin əlində olduğunu bil.

---

## 9. Məsuliyyətin Rədd Edilməsi

Bu beta proqram təminatıdır. Müəllif bu serverin işlədilməsindən, şəbəkəyə açılmasından, ya da ona qoşulan hər hansı AI/client tərəfindən görülən əməliyyatlardan yaranan data itkisi, cihaz zədəsi, icazəsiz giriş və ya hər hansı başqa nəticəyə görə **heç bir məsuliyyət daşımır**. Onu necə konfiqurasiya etdiyinə, kimə giriş verdiyinə görə **sən məsuliyyət daşıyırsan**. Tam hüquqi mətn üçün [LICENSE](../LICENSE) faylına bax. Öz riskinlə istifadə et.

## AI Təhlükəsizlik Bələdçisi

Əgər bu serverə bir AI qoşursansa, [`prompts/ai-usage-guide.txt`](../prompts/ai-usage-guide.txt) bələdçisini onun sistem təlimatlarının bir hissəsi kimi ver — bu, AI-dən geri dönməz əməliyyatlardan əvvəl səndən təsdiq istəməsini xahiş edir.

<p align="center"><a href="README.az.md">← Haqqında səhifəsinə qayıt</a></p>
