<!--
SPDX-FileCopyrightText: © 2025 SBT Localization https://sbt.localization.com.ua
SPDX-FileContributor: Serhii Olendarenko <sergey.olendarenko@gmail.com>

SPDX-License-Identifier: GPL-3.0-only
-->

# SBT Infinity Tools

**<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" width="16" height="16"><path d="M8 16A8 8 0 1 1 8 0a8 8 0 0 1 0 16Zm3.78-9.72a.751.751 0 0 0-.018-1.042.751.751 0 0 0-1.042-.018L6.75 9.19 5.28 7.72a.751.751 0 0 0-1.042.018.751.751 0 0 0-.018 1.042l2 2a.75.75 0 0 0 1.06 0Z"></path></svg> Українська** <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" width="16" height="16"><path d="M10 13a1 1 0 1 1 0-2 1 1 0 0 1 0 2Zm0-4a1 1 0 1 1 0-2 1 1 0 0 1 0 2Zm-4 4a1 1 0 1 1 0-2 1 1 0 0 1 0 2Zm5-9a1 1 0 1 1-2 0 1 1 0 0 1 2 0ZM7 8a1 1 0 1 1-2 0 1 1 0 0 1 2 0ZM6 5a1 1 0 1 1 0-2 1 1 0 0 1 0 2Z"></path></svg> [<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" width="16" height="16"><path d="M0 8a8 8 0 1 1 16 0A8 8 0 0 1 0 8Zm8-6.5a6.5 6.5 0 1 0 0 13 6.5 6.5 0 0 0 0-13Z"></path></svg> English](./README.en.md)

Тут зібрані наші допоміжні інструменти для роботи з іграми на базі Infinity Engine, такими як Baldur’s Gate, Baldur’s Gate II, Planescape: Torment та ін.

Читайте про локалізацію текстур у Planescape: Torment у нашій [повній статті](https://sbt.localization.com.ua/article/lokalizatsiia-tekstur-u-planescape-torment).

## Збирання проєкту

```
go build
```
🙂

## Видобуток кадрів з анімацій у форматі BAM v2

Анімації в розширених виданнях (Enhanced Editions) згаданих вище ігор зберігаються у форматі [BAM v2](https://gibberlings3.github.io/iesdp/file_formats/ie_formats/bam_v2.htm). Це бінарний формат, що містить в собі інформацію про кадри, кожний з яких складається з одного або декількох блоків. Самі блоки при цьому зберігаються в окремих файлах формату `mosXXXX.PVRZ`, де `XXXX` — це номер файла з чотирьох цифр.

Ці PVRZ-файли нерідко являють собою текстурні атласи, в котрих одне фінальне зображення порізане на купу шматочків для ефективнішого зберігання. 

Для експорту всіх окремих кадрів з BAM-анімації у вигляді PNG-файлів є команда `extract-bam`:
```
./infinity-tools extract-bam path/to/config.toml
```

### Конфігурація

Тут файл `config.toml` містить в собі інформацію про те, які вхідні файли обробляти й куди класти результат. Наприклад:
```toml
[Input]
bam = "animation.BAM"

[InputMos]
1000 = "mos2000.PNG"
1001 = "mos2001.PNG"

[Output]
extract = "output"
```
Рядок `bam` вказує на оригінальну анімацію, а розділ `InputMos` містить список усіх PVRZ, від яких залежить BAM-файл, експортованих як PNG. Для видобутку того й іншого найкраще використовувати інструмент під назвою [Near Infinity](https://github.com/NearInfinityBrowser/NearInfinity/wiki).

А `extract` — це шлях до теки, в яку збережуться результати видобутку.

Всі шляхи можуть буть як абсолютні, так і відносні (до розташування самого TOML-файла).

## Оновлення текстурних атласів для анімацій

Попри назву, команда `update-bam` не оновлює саму анімацію, але оновлює текстури, що з нею повʼязані. (Маєте на думці кращу назву — зробіть PR 😉).

Для цього в конфігураційному файлі треба також вказати список оновлених (перемальованих) кадрів:
```toml
[Input]
bam = "animation.BAM"

[InputMos]
1000 = "mos2000.PNG"
1001 = "mos2001.PNG"

[NewFrames]
1 = "Frame1.png"
6 = "Frame6.png"
7 = "Frame7.png"

[Output]
update = "override"
```

Зверніть увагу, що не обовʼязково зазначати всі кадри. Якщо якісь з них пропущені, програма натомість братиме блоки з «оригінальних» текстур, вказаних в `InputMos`.

Використання команди аналогічне до попередньої:
```
./infinity-tools update-bam path/to/config.toml
```

## Приклади

Ви можете знайти наші конфігураційні файли та перемальовані кадри для **Planescape: Torment: Enhanced Edition** у теці [`inputs`](./inputs/).

## Ліцензія

Без напрацювань з відкритим кодом від купи людей, робити локалізацію ігор було б значно складніше, якщо взагалі можливо. Відкриваючи код нашого маленького інструмента, ми хочемо бодай якось віддячити за це спільноті й сподіваємося, що комусь це стане в пригоді.

Тому весь код у цьому репозиторії доступний під ліцензією [GPL 3.0](./LICENSES/GPL-3.0-only.txt), а всі графічні ресурси (окрім оригінальних ресурсів з гри) — під ліцензією <a href="https://creativecommons.org/licenses/by-sa/4.0/">CC BY-SA 4.0</a><img src="https://mirrors.creativecommons.org/presskit/icons/cc.svg" alt="" style="max-width: 1em;max-height:1em;margin-left: .2em;"><img src="https://mirrors.creativecommons.org/presskit/icons/by.svg" alt="" style="max-width: 1em;max-height:1em;margin-left: .2em;"><img src="https://mirrors.creativecommons.org/presskit/icons/sa.svg" alt="" style="max-width: 1em;max-height:1em;margin-left: .2em;">

<a href="https://github.com/sbtlocalization/infinity-tools">SBT Infinity Tools</a> © 2025 by <a href="https://sbt.localization.com.ua">SBT Localization</a>
