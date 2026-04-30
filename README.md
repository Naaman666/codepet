[![Update CodePet stats](https://github.com/Naaman666/codepet/actions/workflows/update-stats.yml/badge.svg)](https://github.com/Naaman666/codepet/actions/workflows/update-stats.yml)

# CodePet

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

## Setup

A workflow-nak szüksége van egy `GH_PAT` repository secretre, mert az alapértelmezett `GITHUB_TOKEN` a `/user/repos` endpointon csak az aktuális repót látja — PAT nélkül a `data.json` csendben hibás adatot kapna (csak a codepet repó commitjai látszanának). Ha a secret hiányzik, a workflow szándékosan elhasal explicit hibaüzenettel.

1. **PAT létrehozása** — GitHub → Settings → Developer settings → Personal access tokens:
   - **Classic PAT**: `repo` scope (privát repók read access-éhez is).
   - **Fine-grained PAT**: minden olyan repóra, amit be akarsz számolni, `Contents: Read` permission.
2. **Secret beállítása** — a codepet repóban: Settings → Secrets and variables → Actions → New repository secret:
   - Name: `GH_PAT`
   - Value: a fent létrehozott token.
3. **Tesztelés** — Actions → Update CodePet stats → Run workflow.
