1.Найдите полный хеш и комментарий коммита, хеш которого начинается на aefea.
        $ git show aefea
        commit aefead2207ef7e2aa5dc81a34aedf0cad4c32545
        Update CHANGELOG.md
2.Какому тегу соответствует коммит 85024d3?
        $ git show 85024d3
        tag: v0.12.23
3.Сколько родителей у коммита b8d720? Напишите их хеши.
        $ git show b8d720 -s
        2 родителя
        56cd7859e0 9ea88f22fc
4.Перечислите хеши и комментарии всех коммитов которые были сделаны между тегами v0.12.23 и v0.12.24.
        $ git log --oneline v0.12.23..v0.12.24
        33ff1c03bb (tag: v0.12.24) v0.12.24
        b14b74c493 [Website] vmc provider links
        3f235065b9 Update CHANGELOG.md
        6ae64e247b registry: Fix panic when server is unreachable
        5c619ca1ba website: Remove links to the getting started guide's old location
        06275647e2 Update CHANGELOG.md
        d5f9411f51 command: Fix bug when using terraform login on Windows
        4b6d06cc5d Update CHANGELOG.md
        dd01a35078 Update CHANGELOG.md
        225466bc3e Cleanup after v0.12.23 release
5.Найдите коммит в котором была создана функция func providerSource, ее определение в коде выглядит так func providerSource(...) (вместо троеточия перечислены аргументы).
        git log -S'func providerSource(' --oneline
        git show 8c928e8358
        8c928e8358
6.Найдите все коммиты в которых была изменена функция globalPluginDirs.
        git grep -p globalPluginDirs
        plugins.go
        git log -L :globalPluginDirs:plugins.go |grep commit
        commit 78b12205587fe839f10d946ea3fdc06719decb05
        commit 52dbf94834cb970b510f2fba853a5b49ad9b1a46
        commit 41ab0aef7a0fe030e84018973a64135b11abcd70
        commit 66ebff90cdfaa6938f26f908c7ebad8d547fea17
        commit 8364383c359a6b738a436d1b7745ccdce178df47
7.Кто автор функции synchronizedWriters?
        git log -S'func synchronizedWriters'
        git show 5ac311e2a91e381e2f52234668b49ba670aa0fe5
        
        Author: Martin Atkins
