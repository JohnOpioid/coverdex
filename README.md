# coverdex

Сжатое зеркало обложек [libretro-thumbnails](https://github.com/libretro-thumbnails) в WebP 300 px.

Оригинальные обложки — PNG 512 px весом около 285 KB каждая. Для экранов ретро-консолей
(640×480 и меньше) это избыточно, поэтому коллекция пережата: **сжатие примерно в 16 раз
при том же визуальном результате**. Это позволяет держать обложки для десятков систем
в репозитории, который CDN отдаёт быстро.

## Структура

Структура повторяет оригинальный репозиторий libretro, меняется только расширение:

```
<machine>/Named_Boxarts/<name>.webp
<machine>/Named_Snaps/<name>.webp
<machine>/Named_Titles/<name>.webp
```

Имена систем совпадают с libretro, включая пробелы и дефисы. Например:

```
Nintendo - Nintendo DS/Named_Boxarts/Mario Kart DS (Europe).webp
```

## Как использовать

Прямая ссылка на файл:

```
https://raw.githubusercontent.com/JohnOpioid/coverdex/main/{machine}/{folder}/{fileBase}.webp
```

В [mini-scraper](https://github.com/sinedied/mini-scraper) и его веб-интерфейсе это
добавляется как пользовательский источник с шаблоном:

```
{machine}/{folder}/{fileBase}.webp
```

Плейсхолдеры:

| Плейсхолдер | Значение |
|---|---|
| `{machine}` | система libretro, например `Nintendo - Nintendo DS` |
| `{repo}` | то же имя с подчёркиваниями вместо пробелов |
| `{folder}` | `Named_Boxarts`, `Named_Snaps` или `Named_Titles` |
| `{file}` | имя исходного файла с расширением (`Mario Kart DS (Europe).png`) |
| `{fileBase}` | имя без расширения — нужно для `.webp` |
| `{type}` | тип обложки: `boxart`, `snap`, `title` |

## Параметры сжатия

| Параметр | Значение |
|---|---|
| Формат | WebP, качество 82 |
| Размер | 300 px по большей стороне, без апскейла |
| Инструмент | [sharp](https://sharp.pixelplumbing.com/) (libvips) |
| Сжатие | 322 KB PNG → 19 KB WebP (6.1 %) |

## Обновление

```bash
# одна система
node tools/mirror-libretro.mjs --machine "Nintendo - Nintendo DS" --out .mirror

# только обложки
node tools/mirror-libretro.mjs --machine "Sega - Mega Drive - Genesis" --types Named_Boxarts

# продолжить прерванный прогон — готовые файлы пропускаются
node tools/mirror-libretro.mjs --machine "Nintendo - Nintendo DS" --out .mirror
```

Прогон возобновляемый: наличие готового `.webp` проверяется перед скачиванием.

## Источник данных и лицензия

Обложки взяты из проекта [libretro-thumbnails](https://github.com/libretro-thumbnails/libretro-thumbnails).
Авторские права на изображения принадлежат их владельцам. Репозиторий содержит
производную работу (пережатые копии) и предоставляется для некоммерческого
использования вместе со скраппером.
