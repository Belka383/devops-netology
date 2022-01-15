# Devops Netology

## Комментарии по ДЗ "3.1. Работа в терминале, лекция 1"
1) Установите средство виртуализации Oracle VirtualBox: `установлен VirtualBox - v. 6.1.30 r148432 (Qt5.6.2)`; 
2) Установите средство автоматизации Hashicorp Vagrant: `установлен Vagrant -v. 2.2.19`;
3) В вашем основном окружении подготовьте удобный для дальнейшей работы терминал: `установлен Windows Terminal`;
4) С помощью базового файла конфигурации запустите Ubuntu 20.04 в VirtualBox посредством Vagrant:
- Заменено содержимое Vagrantfile по умолчанию следующим:
```shell
Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-20.04"
end
```

5) Ознакомьтесь с графическим интерфейсом VirtualBox, посмотрите как выглядит виртуальная машина, которую создал для вас Vagrant, какие аппаратные ресурсы ей выделены. Какие ресурсы выделены по умолчанию?
```
- Оперативной памяти: 1024 Мб
- Количество процессоров: 2
- Объём жесткого диска: 64 Гб
```

6) Ознакомьтесь с возможностями конфигурации VirtualBox через Vagrantfile: документация. Как добавить оперативной памяти или ресурсов процессора виртуальной машине?
```shell
Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-20.04"
  
      config.vm.provider "virtualbox" do |v|
        v.memory = 2048
        v.cpus = 4
    end
end
```

7) Команда `vagrant ssh` из директории, в которой содержится Vagrantfile, позволит вам оказаться внутри виртуальной машины без каких-либо дополнительных настроек. Попрактикуйтесь в выполнении обсуждаемых команд в терминале Ubuntu.
- например, при чтении manual использовал `/` для поиска по ключевым словам.

8) Ознакомиться с разделами man bash, почитать о настройках самого bash:
- какой переменной можно задать длину журнала history, и на какой строчке manual это описывается?
```shell
HISTFILESIZE
              The maximum number of lines contained in the history file.  When this variable is assigned a value, the history file is
              truncated, if necessary, to contain no more than that number of lines by removing the oldest entries.  The history file
              is also truncated to this size after writing it when a shell exits.  If the value is 0, the history file  is  truncated
              to  zero  size.   Non-numeric  values and numeric values less than zero inhibit truncation.  The shell sets the default
              value to the value of HISTSIZE after reading any startup files
строки (757 - 761)

HISTSIZE
              The number of commands to remember in the command history (see HISTORY below).  If the value is  0,  commands  are  not
              saved  in  the  history  list.   Numeric  values less than zero result in every command being saved on the history list
              (there is no limit).  The shell sets the default value to 500 after reading any startup files.
строки (771 - 773)
```
- что делает директива ignoreboth в bash?
```
ignoreboth, ignorespace и ignoredups - значения для переменной HISTCONTROL

- ignoreboth является сокращением от ignorespace и ignoredups.
- ignorespace приводикт к тому, что строки, начинающиеся с пробела, не сохраняются в списке истории. .
- ignoredups приводит к тому, что строки, соответствующие предыдущей записи истории, не сохраняются.
```

9) В каких сценариях использования применимы скобки {} и на какой строчке man bash это описано?
- `{}` являются зарезервированными словами и должны встречаться там, где разрешено распознавание зарезервированного слова: в различных условных циклах, условных операторах, при ограничении тела функции.
- строки (215 - 218).

10) С учётом ответа на предыдущий вопрос, как создать однократным вызовом touch 100000 файлов? Получится ли аналогичным образом создать 300000? Если нет, то почему?
- создаём в текущей директории 100000 фалов
```shell
touch {000001..100000}.txt
```
- Создать 300000 файлов не получится, так как будет превышение stack_size заданного по умолчанию.

12) В man bash поищите по /\[\[. Что делает конструкция `[[ -d /tmp ]]`?
```shell
RESERVED WORDS
       Reserved words are words that have a special meaning to the shell.  The following words are recognized as reserved when unquoted and either
       the first word of a simple command (see SHELL GRAMMAR below) or the third word of a case or for command:

       ! case  coproc  do done elif else esac fi for function if in select then until while { } time [[ ]]
строки (129 - 132)
```
- конструкция `[[ -d /tmp ]]` проверяет условие -d /tmp и возвращает статус (0 или 1) при наличии/отсутствии каталога /tmp.

13. Основываясь на знаниях о просмотре текущих (например, PATH) и установке новых переменных; командах, которые мы рассматривали, добейтесь в выводе type -a bash в виртуальной машине наличия первым пунктом в списке:
```shell
bash is /tmp/new_path_directory/bash
bash is /usr/local/bin/bash
bash is /bin/bash
```
- для этого выполняем:
```shell
vagrant@vagrant:~$ mkdir /tmp/new_path_directory
vagrant@vagrant:~$ cp /bin/bash /tmp/new_path_directory/
vagrant@vagrant:~$ PATH=/tmp/new_path_directory/:$PATH
```

14. Чем отличается планирование команд с помощью batch и at?
- batch - выполняет команды, когда позволяют уровни загрузки системы;
другими словами, когда нагрузка среднее значение падает ниже 1,5 или значения, указанного при вызове atd;
- at - выполняет команды в указанное время (один раз).

15. Завершите работу виртуальной машины чтобы не расходовать ресурсы компьютера и/или батарею ноутбука.
- выключим vm штатным образом, выполнив:
```shell
vagrant halt
```


## Комментарии по ДЗ "2.4. Инструменты Git"
1) Найдите полный хеш и комментарий коммита, хеш которого начинается на aefea.
- для этого можно воспользоваться `git show aefea --pretty=format:"%H, %s"`:
- получим полных хеш коммита: `aefead2207ef7e2aa5dc81a34aedf0cad4c32545` и комментарий: `Update CHANGELOG.md`;
---
2) Какому тегу соответствует коммит 85024d3?
- для этого можно воспользоваться так же `git show`:
`git show 85024d3` - получим полный хеш коммита `85024d3100126de36331c6982bfaac02cdab9e76` и что ему соответствует  тег `v0.12.23`;
- так же можно переключиться на коммит: `git checkout 85024d3` и сделать `git reflog` - увидим, что HEAD стоит на коммите `85024d310`, который в слвою очередь соответствует тегу `v0.12.23`. 
---
3) Сколько родителей у коммита b8d720? Напишите их хеши.
- для этого можно воспользоваться `git log`, как `git log --pretty=%P -n 1 b8d720` - получим:
`56cd7859e05c36c06b56d013b55a252d0bb7e158` `9ea88f22fc6269854151c571162c5bcf958bee2b`;
- `git show -s --pretty=%P b8d720` - вернёт так же родителей коммита; 
- `git rev-list --parents -n 1 b8d720` вернёт: `b8d720f8340221f2146e4e4870bf2ee0bc48f2d5` `56cd7859e05c36c06b56d013b55a252d0bb7e158` `9ea88f22fc6269854151c571162c5bcf958bee2b`, где первый хеш - это полный хеш коммита `b8d720`;
---
4) Перечислите хеши и комментарии всех коммитов которые были сделаны между тегами v0.12.23 и v0.12.24.
- для этого можно воспользоваться `git log v0.12.23..v0.12.24 --pretty=oneline` - получим:
####
```shell
33ff1c03bb960b332be3af2e333462dde88b279e (tag: v0.12.24) v0.12.24
b14b74c4939dcab573326f4e3ee2a62e23e12f89 [Website] vmc provider links
3f235065b9347a758efadc92295b540ee0a5e26e Update CHANGELOG.md
6ae64e247b332925b872447e9ce869657281c2bf registry: Fix panic when server is unreachable
5c619ca1baf2e21a155fcdb4c264cc9e24a2a353 website: Remove links to the getting started guide's old location
06275647e2b53d97d4f0a19a0fec11f6d69820b5 Update CHANGELOG.md
d5f9411f5108260320064349b757f55c09bc4b80 command: Fix bug when using terraform login on Windows4b6d06cc5dcb78af637bbb19c198faff37a066ed Update CHANGELOG.md
dd01a35078f040ca984cdd349f18d0b67e486c35 Update CHANGELOG.md
225466bc3e5f35baa5d07197bbc079345b77525e Cleanup after v0.12.23 release
```
---
5) Найдите коммит в котором была создана функция func providerSource, ее определение в коде выглядит так func providerSource(...) (вместо троеточего перечислены аргументы).
- воспользуемся git log и выведем краткую информацию о коммите: `git log -S 'func providerSource(' --pretty=format:"%h, %s, %cd, %an"` - получим:
```shell
8c928e835, main: Consult local directories as potential mirrors of providers, Mon Apr 6 09:24:23 2020 -0700, Martin Atkins.
```
---
6) Найдите все коммиты в которых была изменена функция globalPluginDirs.
####
для этого сначала найдём упоминания этой функции в файлах: `git grep -p 'globalPluginDirs('` - получим:
####
```shell
 commands.go=func initCommands(
 commands.go:            GlobalPluginDirs: globalPluginDirs(),
 commands.go=func credentialsSource(config *cliconfig.Config) (auth.CredentialsSource, error) {
 commands.go:    helperPlugins := pluginDiscovery.FindPlugins("credentials", globalPluginDirs())
 plugins.go=import (
 plugins.go:func globalPluginDirs() []string {
```
####
- то есть, в plugins.go инициализирована функция globalPluginDirs.
- найдём все коммиты, в которых были изменения функции globalPluginDirs, в файле plugins.go: `git log -L :globalPluginDirs:plugins.go --pretty=format:"%h, %s"` - получим:
####
```shell
78b122055, Remove config.go and update things using its aliases
52dbf9483, keep .terraform.d/plugins for discovery
41ab0aef7, Add missing OS_ARCH dir to global plugin paths
66ebff90c, move some more plugin search path logic to command
8364383c3, Push plugin discovery down into command package
```
7) Кто автор функции synchronizedWriters?
- для этого воспользуемся `git log -S 'synchronizedWriters' --reverse --pretty=format:"%h, %an"` - первый коммит - это коммит автора функции synchronizedWriters, то есть:
####
```shell
5ac311e2a, Martin Atkins
```

## Комментарии по ДЗ "2.1. Системы контроля версий"

Так как мы добавили два файла `.gitignore`: в корневом каталоге - игнорировать пока ничего не будем, оставляем его пустым.
А второй файл взят с `https://github.com/github/gitignore`.

Описание шаблонов, приведённых в этом файле `.gitignore`:

- `**/.terraform/*` - игнорируем всё содержимое в каталоге `.terraform` - как файлы, так и вложенные каталоги;

- `*.tfstate` - игнорируем все файлы с расширением `*.tfstate` в каталоге `terraform` и во всех вложенных каталогах;
- `*.tfstate.*` - игнорируем все файлы, в названии которых есть `.tfstate.`  в каталоге `terraform` и во всех вложенных каталогах;

- `crash.log` - игнорируем файлы `crash.log` в каталоге `terraform` и во всех вложенных каталогах;

- `*.tfvars` - игнорируем все файлы с расширением `.tfvars` в каталоге `terraform` и во всех вложенных каталогах;

- `override.tf` - игнорируем все файлы `override.tf` в каталоге `terraform` и во всех вложенных каталогах;
- `override.tf.json` - игнорируем все файлы `override.tf.json` в каталоге `terraform` и во всех вложенных каталогах;
- `*_override.tf` - игнорируем все файлы файлы с расширением `.tf` в каталоге `terraform` и во всех вложенных каталогах, наименование которых заканчивается на `_override`;
- `*_override.tf.json` - игнорируем все файлы файлы с расширением `.json` в каталоге `terraform` и во всех вложенных каталогах, наименование которых заканчивается на `_override.tf`;

- `.terraformrc` - игнорируем  файлы `.terraformrc` в каталоге `terraform` и во всех вложенных каталогах;
- `terraform.rc` - игнорируем  файлы `terraform.rc` в каталоге `terraform` и во всех вложенных каталогах;