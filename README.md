# Halla-Addons — Catálogo oficial de complementos

Repositório do **catálogo online de complementos do Halla** (`.halla-addon`),
publicado via GitHub Pages em
**<https://grouphalla.github.io/Halla-Addons/>** e consultado pelos clientes
oficiais (Desktop e Mobile) na central de complementos.

## Estrutura

```
addons/<id>/          pacotes .halla-addon publicados + checksums .sha256
api/v1/addons.json    índice do catálogo consumido pelos clientes
index.html            página pública do catálogo
```

O índice `api/v1/addons.json` lista cada complemento com id, versão,
plataformas (`desktop`/`mobile`), capacidades declaradas, descrição e o
caminho de download. Os clientes conferem o **SHA-256** do pacote contra o
`.sha256` publicado antes de instalar.

## Complementos publicados

- **official.radio-voice** — Voz de rádio policial: DSP de comunicador
  (AGC, banda estreita, saturação, squelch e estática) aplicável ao enviar e
  ao ouvir, com regras independentes para fala normal e sussurros.
  Multiplataforma (Desktop e Mobile).

## Empacotar e publicar

Os pacotes são gerados pelo empacotador oficial
(`tools/package_plugin.py` do repositório [Halla](https://github.com/GroupHalla/Halla)),
com o mesmo formato e a mesma ABI C dos clientes. Para publicar uma versão:

1. gere o `.halla-addon` e o `.sha256` (o CI do Halla-Mobile publica os
   `.so` Android do radio-voice como artefato de build);
2. adicione o par em `addons/<id>/`;
3. atualize a entrada correspondente em `api/v1/addons.json` (versão,
   caminho e descrição);
4. publique — o GitHub Pages serve o índice e os pacotes.

Instalado pelo catálogo, um pacote `official.*` **substitui** o complemento
embutido no aplicativo; removido, devolve o embutido ao estado anterior. É
assim que o filtro oficial ganha melhorias sem publicar um novo APK/exe.

## Segurança

Como no cliente: uma biblioteca nativa executa no mesmo processo do app com
os mesmos privilégios; as capacidades do manifesto são informativas. O
catálogo só distribui complementos oficiais — instale de fontes
confiáveis. Veja [`docs/PLUGINS.md`](https://github.com/GroupHalla/Halla/blob/main/docs/PLUGINS.md)
do cliente para o formato do pacote, a ABI e as regras de segurança.
