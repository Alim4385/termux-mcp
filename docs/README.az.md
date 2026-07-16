<p align="center">
  <a href="README.en.md">🇬🇧 English</a> •
  <a href="README.az.md"><strong>🇦🇿 Azərbaycanca</strong></a> •
  <a href="README.tr.md">🇹🇷 Türkçe</a>
</p>

# Termux MCP Server (Beta)

Sıfır asılılıqlı [Model Context Protocol](https://modelcontextprotocol.io) server — Termux daxilində işləyir və qoşulan istənilən AI-yə (Claude, ChatGPT və s.) Android cihazında real bash shell girişi verir: fayl yaz/oxu, paket qur, script işlət və s.

> ⚠️ **Bu server heç bir daxili authentication təmin etmir.** Default olaraq yalnız `127.0.0.1`-ə bağlıdır. Əgər onu tunel (məs. cloudflared) ilə internetə açsan, linki əldə edən HƏR KƏS telefonunda shell girişi əldə edir. Bunu etməzdən əvvəl aşağıdakı **Təhlükəsizlik və İmkanlar** bölməsini oxu.

## Xüsusiyyətlər

- **Sıfır asılılıq** — yalnız Node.js-in daxili modulları (`http`, `child_process`, `fs`, `path`)
- **1 alət**: `exec` — istənilən bash əmrini icra edir
- **Avtomatik işçi qovluq (cwd) izləmə** — `cd` və zəncirlənmiş əmrlər (`cd x && npm run dev`) düzgün izlənir, çünki real `$PWD` markeri istifadə olunur, regex təxmini deyil
- **Restart-safe** — son işçi qovluq diskə yazılır və server yenidən başlayanda bərpa olunur (flash yaddaşı qorumaq üçün yazılar debounce edilir)
- **Timeout qoruması** — 60 saniyədən uzun sürən əmrlər avtomatik dayandırılır
- **Output kəsilməsi** — çox böyük çıxışlar token qənaəti üçün kəsilir, həm başlanğıc, həm son hissə göstərilir

## Sistem Tələbləri və Test Nəticələri

Bu rəqəmlər developer-in öz testlərindən götürülüb — hər cihaz üçün zəmanət kimi yox, istinad kimi qəbul et:

| | |
|---|---|
| **Android versiyası** | Android 9-dan 14-ə qədər sınaqdan keçib — sabit işləyir |
| **Sınaqdan keçmiş cihazlar** | Redmi 6A, Honor X8B |
| **RAM istifadəsi (server boşdaykən)** | Testlərdə ~28 MB müşahidə olunub |
| **Tövsiyə olunan boş RAM** | Rahat istifadə üçün minimum 500 MB – 1 GB, xüsusən AI daha ağır əmrlər işlədəcəksə (paket qurma, build və s.) |
| **Yaddaş sahəsi** | Node.js + bu skript — bir neçə MB; əlavə paketlər (`pkg`/`npm`) qursan artacaq |

## Quraşdırma

```bash
pkg install nodejs git -y
git clone https://github.com/Alim4385/termux-mcp.git
cd termux-mcp
npm start
```

Server `http://127.0.0.1:3000/mcp` ünvanında işə düşür.

Sağlamlıq yoxlaması:
```bash
curl http://127.0.0.1:3000/health
```

## AI-ə Qoşulma

### Lokal (eyni cihaz / eyni şəbəkə, MCP dəstəkləyən client)

```json
{
  "mcpServers": {
    "termux": {
      "url": "http://127.0.0.1:3000/mcp"
    }
  }
}
```

### Uzaqdan (tunel ilə, bulud əsaslı AI client-lər üçün)

[cloudflared](https://github.com/cloudflare/cloudflared) quraşdır və işə sal:

```bash
pkg install cloudflared -y
cloudflared tunnel --url http://127.0.0.1:3000
```

Sənə `https://xxxx.trycloudflare.com` kimi təsadüfi bir link veriləcək. Sonuna `/mcp` əlavə et (`https://xxxx.trycloudflare.com/mcp`) və bunu AI client-ində custom MCP connector kimi əlavə et (məsələn, ChatGPT-nin Developer Mode connector-ları, əgər planın dəstəkləyirsə, ya da remote MCP server dəstəkləyən istənilən client).

**Qeydlər:**
- Link hər dəfə `cloudflared`-i yenidən başlatanda dəyişir — bu, yeganə real qorumandır və zəifdir (URL-lər loglara, browser tarixçəsinə, screenshot-lara sıza bilər). Onu paylaşma və tuneli uzun müddət nəzarətsiz açıq saxlama.
- Daha güclü qoruma istəsən, `/mcp`-nin qarşısına özün bir shared-secret token yoxlaması əlavə edə bilərsən — bu server minimalist qalmaq üçün qəsdən onsuz göndərilir, amma sənə əlavə etməyə heç nə mane olmur.

## ⚠️ Təhlükəsizlik və İmkanlar

Bu serverin yeganə aləti `exec`-dir — ixtiyari bash icrası. Bunun cihazın üçün *real olaraq* nə demək olduğu, başqa nə quraşdırılıb/aktivdirsə ona bağlıdır:

**Əgər [Termux:API](https://wiki.termux.com/wiki/Termux:API) quraşdırılıbsa** (`pkg install termux-api` + Termux:API tətbiqi), bu serverə qoşulan istənilən AI, prinsipcə, bu kimi əmrləri çağıra bilər:
- `termux-sms-list` / `termux-sms-send` — SMS oxumaq/göndərmək
- `termux-contact-list` — kontaktları oxumaq
- `termux-camera-photo` — şəkil çəkmək
- `termux-location` — GPS lokasiyasını almaq
- `termux-clipboard-get/set`, `termux-notification`, `termux-battery-status`, `termux-microphone-record` və s.

Termux:API-ni yalnız qoşulan AI-nin bunları potensial istifadə edə bilməsinə rahat olduğun halda quraşdır, və linkə **kimin/nəyin sahib olduğunu həmişə bil**.

**Əgər cihaz root olunubsa:** root shell sistem partisiyalarını dəyişə, digər tətbiqlərin şəxsi datasına girə, firewall qaydalarını dəyişə və ümumiyyətlə Android-in normal app sandbox-unu keçə bilər. Bu serveri root kimi işlətmək istənilən səhvin və ya pis niyyətli istifadənin təsir dairəsini kəskin artırır — **tövsiyə olunmur**, əgər riski tam anlamırsansa.

**ADB haqqında:** ADB girişi bu serverlə birbaşa əlaqəli deyil, amma cihazını ümumi şəkildə qorumaq üçün bilməyə dəyər — telefonuna ADB girişi olan hər kəs demək olar ki tam nəzarətə malikdir (tətbiq qurmaq/silmək, faylları çıxarmaq, logları oxumaq, ekranı güzgüləmək). ADB girişini heç vaxt bu serverin tunel linki ilə birlikdə paylaşma.

## Məsuliyyətin Rədd Edilməsi

Bu beta proqram təminatıdır. Müəllif bu serverin işlədilməsindən, şəbəkəyə açılmasından, ya da ona qoşulan hər hansı AI/client tərəfindən görülən əməliyyatlardan yaranan data itkisi, cihaz zədəsi, icazəsiz giriş və ya hər hansı başqa nəticəyə görə **heç bir məsuliyyət daşımır**. Onu necə konfiqurasiya etdiyinə, kimə giriş verdiyinə və onun üzərindən nəyin işlədilməsinə icazə verdiyinə görə **sən məsuliyyət daşıyırsan**. Tam hüquqi mətn üçün [LICENSE](../LICENSE) faylına bax. Öz riskinlə istifadə et.

## AI Təhlükəsizlik Bələdçisi

Əgər bu serverə bir AI qoşursansa, [`prompts/ai-usage-guide.txt`](../prompts/ai-usage-guide.txt) bələdçisini onun sistem təlimatlarının bir hissəsi kimi ver — bu, AI-dən geri dönməz əməliyyatlardan əvvəl səndən təsdiq istəməsini xahiş edir.

## Lisenziya

MIT + əlavə istifadə şərtləri — bax [LICENSE](../LICENSE).
