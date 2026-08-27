# Pacotes publicados

Os arquivos `.halla-addon` distribuídos pelo catálogo ficam neste diretório,
junto do checksum `.sha256` correspondente.

Convenção de nome:

```
<id>-<versão>-<plataforma>.halla-addon
<id>-<versão>-<plataforma>.halla-addon.sha256
```

Plataformas atualmente aceitas pelos manifestos:

- `windows-x64`, `linux-x64`, `linux-arm64` (Desktop)
- `android-arm64`, `android-arm`, `android-x86_64`, `android-x86` (Mobile)

Enquanto um complemento estiver com `distribution: "bundled"` no catálogo
(`api/v1/addons.json`), ele não tem arquivo aqui — já vem dentro do aplicativo
e a atualização acompanha as releases do Halla Desktop / Halla Mobile.

## Pacotes multiplataforma

Um mesmo pacote pode carregar as bibliotecas de várias plataformas de uma vez
(o manifesto declara todas em `platforms`); o nome usa `halla-addon` sem
sufixo de plataforma. É o caso do primeiro pacote oficial:

- `official-radio-voice/official-radio-voice-1.1.1.halla-addon` — **Voz de
  rádio policial** v1.1.1 com `radio_voice.dll` (Windows x64, build MSVC do
  Halla Desktop) e `libradio_voice.so` para as quatro ABIs Android (build NDK
  do Halla Mobile). Instalado pelo catálogo, substitui o complemento embutido
  de mesmo id; removido, devolve o embutido ao estado anterior.

O fluxo de publicação é: as libs saem dos workflows de CI dos aplicativos
(artefatos `halla-radio-voice-windows` e `halla-radio-voice-android`),
`tools/package_plugin.py` do repo Halla monta o ZIP reproduzível, e o
checksum vira o `sha256` do catálogo.

