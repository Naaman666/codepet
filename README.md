[![Update CodePet stats](https://github.com/Naaman666/codepet/actions/workflows/update-stats.yml/badge.svg)](https://github.com/Naaman666/codepet/actions/workflows/update-stats.yml)

# BSR CodePet

Pixel-art "tamagotchi" a GitHub commit-aktivitásod alapján. Az oldal a `data.json` fájlból olvassa az adatokat, amelyet egy GitHub Actions workflow frissít automatikusan.

## data.json mezők

| Mező | Típus | Leírás |
|------|-------|--------|
| `user` | string | GitHub felhasználónév |
| `commits` | number | Commitok száma az elmúlt `window_days` napban |
| `window_days` | number | Az időablak hossza napokban (alapértelmezett: 30) |
| `streak` | number | Jelenlegi napi streak (napok) |
| `repos` | number | Aktív repók száma az időablakban |
| `top_lang` | string | Az owner repóiban leggyakoribb elsődleges nyelv |
| `updated_at` | string | Utolsó frissítés időpontja ISO 8601 formátumban |

## Fejlődési szintek (tier-ek)

| Tier | Min. commit | Szín |
|------|-------------|------|
| Scrap | 0 | szürke |
| Core | 10 | kék |
| Forge | 30 | zöld |
| Arc | 60 | narancs |
| Apex | 100 | lila |

## Hangulat (mood)

A robot hangulata a commitszám alapján változik:

- 0 commit → Alvó üzemmód
- ≥ 5 → Feltöltöm az akkumulátort
- ≥ 15 → Rendszerek aktívak
- ≥ 30 → Turbó mód
- ≥ 60 → APEX PROTOKOLL

## Workflow

Az `update-stats.yml` workflow naponta lefut és frissíti a `data.json`-t a GitHub API segítségével, majd commitolja a változást a `main` ágra. A `deploy-pages.yml` a `main` ágra érkező push eseményekre fut, és az oldalt GitHub Pages-re publikálja.
