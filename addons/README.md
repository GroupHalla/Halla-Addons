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
