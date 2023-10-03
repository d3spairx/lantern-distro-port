# lantern-distro-port

Инструменты упаковки бинарных пакетов для различных версий Linux

## Действия:
Мы выбрали для выполнения скриптов окружение GitHubActions. И написали три стадии: с получением и распоковкой deb дистрибутива, с проверкой валидности и генерации метаданных, со скачкой или устанокой дополнительных библиотек, со сборкой и запаковкой для других дистрибутивов.

## Прогресс:
Проект разделился на три скрипта: скачка и верификация, анализ и генерация, и сборка. Вчера возникли проблемы со сборкой и подключением библиотек в других версиях. Сегодня весь день исправляли эти трудности. 

![Схема работы](IMG_20231001_163937_292.jpg)

## Сейчас:
Достигнута цель сборки из Deb в Aur бинарный пакет в окружении GitHub Actions.

## Будущее:
Дописать бинарные пакеты для других систем. Сделать универсальным по для перепаковки других пакетов и банрных файлов.


# Description

Lantern Desktop поддерживается дистрибутивами Linux на базе Debian. Он может работать на других дистрибутивах (Fedora, Arch, Alpine, Void и т. д.), но его необходимо правильно упаковать для этих систем (RPM для Fedora, AUR для Arch и т. д.).

Задача: проанализировать пакеты, выпущенные Lantern для Debian, и создать инструменты сборки для упаковки приложения для различных версий Linux.

Тех задача: Требовалось перепаковать debian или бинарный файл (без исходников) для других дистрибутивов в автоматическом режиме. В качестве входных данных был дан пакет Lantern Desktop.

# Reference

## UNZIP

https://www.debian.org/doc/debian-policy/
`ar -x` - unpack on debian
Control (pack meta, hash sums) + Data
Unpack data -> usr: bin, doc
.tar.gz - `tar -xzf`
__Universal__: `dpkg-deb -e` :preinst, postinst, prerm, postrm

## PACKAGING

Options:

- docker

- manual packing and linking

- chroot

- __GitHub Actions__

__Selected way__: Github Actions

Package-specific applications:

1. Arch - AUR:

- `mpkg`
Conclusion - mpkg with linking
- `makepkg`

2. RPM - Fedora:

Classic `rpm-tools` - https://rpm-packaging-guide.github.io/
`alien`  - from deb to rpm : GH Actions/debian env
`rpkg` - build and linking [due](https://www.reddit.com/r/linuxquestions/comments/fag9ej/best_tutorial_for_automating_creation_of_deb_rpm/i)

3. Alpine:

https://wiki.alpinelinux.org/wiki/Creating_an_Alpine_package
`dh-make`

4. Debian:

`dpkg`

5. Nix

<https://nixos.wiki/wiki/Packaging/Binaries>

Conclusion:
`makepkg` - AUR
`rpkg` - RPM

[1] Deep Linking - https://wiki.archlinux.org/title/Creating_packages_for_other_distributions

## META ANALYSING

[PKG Format](http://refspecs.linux-foundation.org/LSB_3.2.0/LSB-Core-generic/LSB-Core-generic/pkgformat.html)

RPM
Fedora - RPM
<https://web.archive.org/web/20050312181634/http://www.hut.fi/~tkarvine/rpm-build-as-user.html>
<https://web.archive.org/web/20060219023154/http://www.rpm.org/max-rpm/rpm.8.html>


MISC:
[DPMD](https://github.com/FooBarWidget/debian-packaging-for-the-modern-developer)

# Process 

 SHEME

1. GitHub Actions

2. Scripts:

- Скачка, распаковка, верификация

- анализ, генерация

- *no needed* скачка, компляция библиотек

- сборка

3. Libraries bug

4. Сборка в Aur

MakePkg + Linking

## BUG

1. PKGBUILD not executed - missing libraries.
`libcap libappindicator-gtk3` - lib linking
Add Libraries to package, linked them manually.

## BACKLOG

RPM
Generating by:

- `rpkg` or `rpm-tools` on Fedora environment

- `alien` on Debian env


## QUESTIONS

1) Sources or binaries?
Binaries.

2) Dependences?
analysed - `libcap libappindicator-gtk3`

# PRESENTATION 

Тех реализация, прогресс за хакатон, потенциал.
[КОД](https://github.com/d3spairx/lantern-distro-port/)

1. Problem

- Проанализировать пакеты Lantern Desktop


- Создать инструменты сборки и упаковки

2. Log

- gh a
- scripts
- Cборка aur
- Другие системы

3. Собранный aur пакет.

4. Схема + прогресс

5. RPM генерация.

6. Полезность: возможность портирования других пакетов.

7. Участники и мотивация.

# DOC 

1. Init environment and choose target systems (with applications).

`.yaml` - build configuration for GH Actions [YAML](https://github.com/d3spairx/lantern-distro-port/blob/main/.github/workflows/main.yaml) ([doc](https://docs.github.com/ru/actions/examples/using-scripts-to-test-your-code-on-a-runner))

- `dpkg-deb` - extract deb packet

2. Build for different systems
   
- `makepkg` - build aur packet [AUR](https://github.com/d3spairx/lantern-distro-port/releases/tag/7.4.0)

[PKGBUILD](https://github.com/d3spairx/lantern-distro-port/blob/main/PKGBUILD) - build script with
+ metadata
- change desktop
- add lib link

Build RPM packet by (REF TO BACKLOG):
- `rpkg` [doc](https://linux.die.net/man/1/rpkg) or `rpm-tools`[doc](https://rpm-packaging-guide.github.io/#packaging-software) on Fedora environment
- `alien` on Debian env
