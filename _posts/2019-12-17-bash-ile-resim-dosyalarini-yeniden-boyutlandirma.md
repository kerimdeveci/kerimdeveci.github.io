---
title: "Bash ile resim dosyalarini yeniden boyutlandirma"
last_modified_at: 2016-03-09T16:20:02-05:00
categories:
  - Blog
tags:
  - Swift
  - turkce
---

## Bash ile resim dosyalarini terminalden yeniden boyutlandirma

Gelistime yaparken siklikla karsilastigimiz bir durum olan resimleri yeniden boyutlandirma isini terminalden kisa bir komut ile nasil kolayca yapabilcegiz arasiyda iken karsima `sips` geldi.

```bash
sips -Z 320 imagesFolder/* --out resizedFolder/
```

- `-Z` flag i orjinal aspect ratio'yu korur.
- `320` resmin en uzun boyutunun olacagi piksel cozunurlugudur
- Eger `--out` flagi ayarlanmadiysa `sips` orjinal dosyanin uzerine yazar.
