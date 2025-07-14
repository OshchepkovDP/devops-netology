Вопросы:
Какому тегу соответствует коммит 85024d3?
Ответ:git log --decorate 85024d3
commit 85024d3100126de36331c6982bfaac02cdab9e76 (tag: v0.12.23)
Коммиту 85024d3 соответствует тег v0.12.23

Сколько родителей у коммита b8d720? Напишите их хеши.
Ответ:git show b8d720
commit b8d720f8340221f2146e4e4870bf2ee0bc48f2d5
Merge: 56cd7859e0 9ea88f22fc
У коммита b8d720 два родителя 56cd7859e0 9ea88f22fc

Перечислите хеши и комментарии всех коммитов, которые были сделаны между тегами v0.12.23 и v0.12.24.
Ответ:git log v0.12.23..v0.12.24 --oneline
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
В основном, это были указания на изменеия в фале CHANGELOG.md и устранение ошибок новой версии.

Найдите коммит, в котором была создана функция func providerSource, её определение в коде выглядит так: func providerSource(...) (вместо троеточия перечислены аргументы).
Ответ:git log -S'func providerSource' --all
commit 8c928e83589d90a031f811fae52a81be7153e82f
Author: Martin Atkins <mart@degeneration.co.uk>
Date:   Thu Apr 2 18:04:39 2020 -0700

    main: Consult local directories as potential mirrors of providers
    
    This restores some of the local search directories we used to include when
    searching for provider plugins in Terraform 0.12 and earlier. The
    directory structures we are expecting in these are different than before,
    so existing directory contents will not be compatible without
    restructuring, but we need to retain support for these local directories
    so that users can continue to sideload third-party provider plugins until
    the explicit, first-class provider mirrors configuration (in CLI config)
    is implemented, at which point users will be able to override these to
    whatever directories they want.
    
    This also includes some new search directories that are specific to the
    operating system where Terraform is running, following the documented
    layout conventions of that platform. In particular, this follows the
    XDG Base Directory specification on Unix systems, which has been a
    somewhat-common request to better support "sideloading" of packages via
    standard Linux distribution package managers and other similar mechanisms.
    While it isn't strictly necessary to add that now, it seems ideal to do
    all of the changes to our search directory layout at once so that our
    documentation about this can cleanly distinguish "0.12 and earlier" vs.
    "0.13 and later", rather than having to document a complex sequence of
    smaller changes.
    
    Because this behavior is a result of the integration of package main with
    package command, this behavior is verified using an e2etest rather than
    a unit test. That test, TestInitProvidersVendored, is also fixed here to
    create a suitable directory structure for the platform where the test is
    being run. This fixes TestInitProvidersVendored.

Функция func providerSource была создана в коммите 8c928e83589d90a031f811fae52a81be7153e82f автором Martin Atkins.
так как он вносит существенные изменения по работе с провайдерами и по времени является самым ранним по созданию.

Найдите все коммиты, в которых была изменена функция globalPluginDirs.
Ответ:git log -S'globalPluginDirs' --all
commit fcdb5d2e558b2ea5e1a63713aca2ba9848ad2ada (origin/f-plugin-finder)
Author: Martin Atkins <mart@degeneration.co.uk>
Date:   Thu Sep 2 11:46:12 2021 -0700

    WIP centralized plugin finder

commit 8364383c359a6b738a436d1b7745ccdce178df47
Author: Martin Atkins <mart@degeneration.co.uk>
Date:   Thu Apr 13 18:05:58 2017 -0700

    Push plugin discovery down into command package
    
    Previously we did plugin discovery in the main package, but as we move
    towards versioned plugins we need more information available in order to
    resolve plugins, so we move this responsibility into the command package
    itself.
    
    For the moment this is just preserving the existing behavior as long as
    there are only internal and unversioned plugins present. This is the
    final state for provisioners in 0.10, since we don't want to support
    versioned provisioners yet. For providers this is just a checkpoint along
    the way, since further work is required to apply version constraints from
    configuration and support additional plugin search directories.
    
    The automatic plugin discovery behavior is not desirable for tests because
    we want to mock the plugins there, so we add a new backdoor for the tests
    to use to skip the plugin discovery and just provide their own mock
    implementations. Most of this diff is thus noisy rework of the tests to
    use this new mechanism.
Данные коммиты вносят изменения в функцию globalPluginDirs

Кто автор функции synchronizedWriters?
Ответ:git log -S'synchronizedWriters' --all
commit 5ac311e2a91e381e2f52234668b49ba670aa0fe5
Author: Martin Atkins <mart@degeneration.co.uk>
Date:   Wed May 3 16:25:41 2017 -0700

    main: synchronize writes to VT100-faker on Windows
    
    We use a third-party library "colorable" to translate VT100 color
    sequences into Windows console attribute-setting calls when Terraform is
    running on Windows.
    
    colorable is not concurrency-safe for multiple writes to the same console,
    because it writes to the console one character at a time and so two
    concurrent writers get their characters interleaved, creating unreadable
    garble.
    
    Here we wrap around it a synchronization mechanism to ensure that there
    can be only one Write call outstanding across both stderr and stdout,
    mimicking the usual behavior we expect (when stderr/stdout are a normal
    file handle) of each Write being completed atomically.

Автором функции synchronizedWriters является Martin Atkins, что можно увидеть 
из представленного коммита, где прописано создание механизма синхронизации
