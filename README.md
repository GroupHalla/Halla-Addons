# Halla-Addons — Central oficial de complementos

Este repositório publica o **site de complementos do Halla** no GitHub Pages:

> **https://grouphalla.github.io/Halla-Addons/**

Os aplicativos **Halla Desktop** e **Halla Mobile** consultam o catálogo
[`api/v1/addons.json`](api/v1/addons.json) deste site para instalar e atualizar
complementos (`.halla-addon`) sem precisar de uma nova versão do aplicativo.
Todo pacote é validado por **SHA-256** antes da instalação.

## Complementos oficiais publicados

| Complemento | Plataformas | Distribuição |
|---|---|---|
| Voz de rádio policial (`official.radio-voice`) | 🖥️ Desktop · 📱 Mobile | Incluído no aplicativo |
| Overlay oficial da call (`official.talking-overlay`) | 🖥️ Desktop | Incluído no aplicativo |

## Estrutura

```
index.html            Site do catálogo (busca, filtros por plataforma/categoria, detalhes)
api/v1/addons.json    Catálogo consumido pelos aplicativos e pelo site (versão 1)
addons/               Pacotes .halla-addon publicados + checksums
```

O site é um arquivo único sem dependências de build: ele lê o próprio
`api/v1/addons.json` e renderiza busca (sem acento também), filtros por
plataforma (Todos/Desktop/Mobile/Ambos), por categoria, ordenação e a página de
detalhes de cada complemento. Os filtros entram no hash da URL — por exemplo,
`#pl=mobile&cat=audio` — então dá para compartilhar links já filtrados.

## Formato do catálogo (v1)

```json
{
  "version": 1,
  "generatedAt": "<ISO-8601>",
  "addons": [
    {
      "id": "official.radio-voice",
      "name": "Voz de rádio policial",
      "version": "1.0.0",
      "author": "Halla-DEV",
      "description": "...",
      "official": true,
      "platforms": ["desktop", "mobile"],
      "distribution": "bundled",
      "capabilities": ["audio.capture", "audio.playback"],
      "category": "audio",
      "icon": "📻",
      "longDescription": "...",
      "highlights": ["..."],
      "setupSteps": ["..."],
      "downloadUrl": null,
      "sha256": null
    }
  ]
}
```

Campos:

- `platforms` — etiquetas de disponibilidade: `desktop`, `mobile` (ambas = os dois aplicativos).
- `distribution` — `bundled` (já vem no aplicativo) ou `download` (pacote separado).
- `downloadUrl` / `sha256` — obrigatórios quando `distribution` = `download`;
  a URL deve ser HTTPS e o SHA-256 (64 hex) corresponde ao arquivo `.halla-addon`.
- `capabilities` — permissões declaradas pelo manifesto do pacote
  (mesmas da API de plugins documentada em `docs/PLUGINS.md` do repositório Halla).
- `category` (site) — categoria de exibição: `audio`, `interface`, `fun`, `utility`.
- `icon` (site) — emoji usado no card do catálogo (opcional; existe fallback por categoria).
- `longDescription`, `highlights`, `setupSteps` (site) — textos opcionais da página de
  detalhes; ausentes, o site usa a `description` e passos genéricos.

Novos campos são adicionados de forma retrocompatível; mudanças incompatíveis
abrem um novo caminho versionado (`api/v2/...`) mantendo o anterior disponível.

## Publicar um complemento

1. Empacote com `tools/package_plugin.py` do repositório [Halla](https://github.com/GroupHalla/Halla):
   o ZIP `.halla-addon` contém `manifest.json` (`type: "native"`, `apiVersion: 1`) e a
   biblioteca da plataforma.
2. Copie o `.halla-addon` e o `.sha256` para `addons/` com o nome
   `<id>-<versão>-<plataforma>.halla-addon` (ex.: `official.radio-voice-1.1.0-windows-x64.halla-addon`).
3. Adicione/atualize a entrada em `api/v1/addons.json`: `version`, `distribution: "download"`,
   `downloadUrl` (URL absoluta HTTPS do arquivo em `addons/`) e `sha256`.
4. Atualize `generatedAt` e envie o commit — o GitHub Pages publica na hora.

A inclusão de um pacote comunitário não o torna oficial: apenas entradas com
`official: true` são mantidas pela equipe Halla. O aplicativo sempre exibe o
aviso de execução nativa e valida o SHA-256 antes de instalar.
