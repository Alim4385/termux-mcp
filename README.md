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
│   ├── README.en.md        — full English documentation
│   ├── README.az.md        — full Azerbaijani documentation
│   └── README.tr.md        — full Turkish documentation
└── prompts/
    └── ai-usage-guide.txt   — safety guide for any AI connecting to this server
```

Want to add your language? Fork this repo, copy `docs/README.en.md` to `docs/README.<lang>.md`, translate it, and open a pull request. Add your language's badge to this file.
