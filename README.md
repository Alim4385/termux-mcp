<div align="center">

# 📱 Termux MCP Server

<sub>Zero-dependency Model Context Protocol server for Termux (Android)</sub>

<br>

[![English](https://img.shields.io/badge/lang-English-2f81f7?style=for-the-badge)](docs/README.en.md)
[![Azərbaycanca](https://img.shields.io/badge/dil-Az%C9%99rbaycanca-3fb950?style=for-the-badge)](docs/README.az.md)
[![Türkçe](https://img.shields.io/badge/dil-T%C3%BCrk%C3%A7e-e5534b?style=for-the-badge)](docs/README.tr.md)

<br>

**👆 Click your language above to read the full documentation.**
**👆 Yuxarıdan öz dilini seç, tam sənədləşməni oxu.**
**👆 Yukarıdan dilini seç, tam belgelendirmeyi oku.**

</div>

---

<div align="center">

⚠️ **BETA SOFTWARE** — gives shell access to your device. Read the security section in your language's docs before exposing this to any network. See [LICENSE](LICENSE) for terms and disclaimer.

</div>

## Repo Structure

```
termux-mcp/
├── server.js              — the MCP server (single file, zero dependencies)
├── package.json
├── LICENSE                 — MIT + usage disclaimer
├── docs/
│   ├── README.en.md        — About (English) — what it is, why, features
│   ├── README.az.md        — About (Azerbaijani)
│   ├── README.tr.md        — About (Turkish)
│   ├── GUIDE.en.md         — Full step-by-step setup guide (English)
│   ├── GUIDE.az.md         — Full step-by-step setup guide (Azerbaijani)
│   └── GUIDE.tr.md         — Full step-by-step setup guide (Turkish)
└── prompts/
    └── ai-usage-guide.txt   — safety guide for any AI connecting to this server
```

Want to add your language? Fork this repo, copy `docs/README.en.md` and `docs/GUIDE.en.md` to `docs/README.<lang>.md` and `docs/GUIDE.<lang>.md`, translate both, and open a pull request. Add your language's badge to this file.
