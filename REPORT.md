# REPORT — Инфраструктурный репозиторий

Студент: Tynchybek_uulu_Azim
Репозиторий: https://github.com/Sarkushev/Devops_midterm

---

## Блок 1. Основы

### 1.1 Настройка и первый коммит

```bash
git config --global user.name "Имя Фамилия"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git clone git@github.com:Sarkushev/Devops_midterm.git
cd Devops_midterm
printf '.env\n*.log\n*.retry\n' > .gitignore
git add .gitignore
git commit -m "chore: add .gitignore"
```

Вывод `git commit`:
```
[main (root-commit) 1acb0c3] chore: add .gitignore
 1 file changed, 3 insertions(+)
 create mode 100644 .gitignore
```
Вывод `git push -u origin main`:
```
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 245 bytes | 245.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To github.com:Sarkushev/Devops_midterm.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```
Создан пустой репозиторий, склонирован по SSH. Первый коммит содержит только `.gitignore` со строками `.env`, `*.log`, `*.retry`.

### 1.2 Файлы проекта

```bash
cp -r ../infra-project-starter/. .   # не копируйте .git, если он есть в архиве
echo "DB_PASSWORD=secret123" > .env
echo "test log" > app.log
git status
```
Вывод `git status` (`.env` и `app.log` не показаны):
```
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .env.example
        README.md
        ansible/
        docker/

nothing added to commit but untracked files present (use "git add" to track)
```

```bash
git add README.md .env.example
git commit -m "docs: add README and .env.example"
git add docker/
git commit -m "feat(docker): add Dockerfile and index page"
git add ansible/
git commit -m "feat(ansible): add inventory, config and playbook"
git push -u origin main
```
Вывод `git push` (верхние строки на скриншоте обрезаны):
```
Compressing objects: 100% (14/14), done.
Writing objects: 100% (17/17), 1.79 KiB | 914.00 KiB/s, done.
Total 17 (delta 2), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (2/2), done.
To github.com:Sarkushev/Devops_midterm.git
   1acb0c3..a37cf79  main -> main
```
Вывод `git log --oneline`:
```
a37cf79 (HEAD -> main, origin/main) feat(ansible): add inventory, config and playbook
c4d3d85 feat(docker): add Dockerfile and index page
93c6522 docs: add README and .env.example
1acb0c3 chore: add .gitignore
```
Файлы `.env` и `app.log` игнорируются из-за правил в `.gitignore`, поэтому на сервер не попали.

### 1.3 Просмотр изменений

```bash
Add-Content README.md "`nCourse: DevOps midterm"
git diff
git add README.md
git diff
git diff --staged
git commit -m "docs: add course name to README"
git push
```
Вывод `git diff` до `git add`:
```
warning: in the working copy of 'README.md', LF will be replaced by CRLF the next time Git touches it
diff --git a/README.md b/README.md
index 4c126a3..dd4e4e3 100644
--- a/README.md
+++ b/README.md
@@ -4,4 +4,5 @@
 
 Репозиторий для практики по Git, Docker и Ansible.
 
-Автор: Tynchybek_uulu_Azim
\ No newline at end of file
+Автор: Tynchybek_uulu_Azim
+Course: DevOps midterm
```
Вывод `git diff` после `git add` (пусто):
```
```
Вывод `git diff --staged`:
```
diff --git a/README.md b/README.md
index 4c126a3..dd4e4e3 100644
--- a/README.md
+++ b/README.md
@@ -4,4 +4,5 @@
 
 Репозиторий для практики по Git, Docker и Ansible.
 
-Автор: Tynchybek_uulu_Azim
\ No newline at end of file
+Автор: Tynchybek_uulu_Azim
+Course: DevOps midterm
```
Вывод `git commit` и `git push`:
```
[main a25d851] docs: add course name to README
 1 file changed, 2 insertions(+), 1 deletion(-)

Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 12 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 333 bytes | 333.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To github.com:Sarkushev/Devops_midterm.git
   982a10d..a25d851  main -> main
```
**Почему второй `git diff` пустой:** `git diff` без аргументов сравнивает рабочую директорию с индексом. После `git add` изменение попало в индекс, и рабочая копия совпадает с ним, поэтому различий нет. Изменения, уже лежащие в индексе, показывает `git diff --staged` (сравнение индекса с последним коммитом).

---

## Блок 2. История и ветки

### 2.1 История коммитов

```bash
git log --oneline
git log --oneline --graph --all
git show HEAD~1
```
Вывод `git log --oneline`:
```
a25d851 (HEAD -> main, origin/main, origin/HEAD) docs: add course name to README
982a10d docs: add author to README
4af168e docs: describe project in README
a37cf79 feat(ansible): add inventory, config and playbook
c4d3d85 feat(docker): add Dockerfile and index page
93c6522 docs: add README and .env.example
1acb0c3 chore: add .gitignore
```
Вывод `git log --oneline --graph --all`:
```
* a25d851 (HEAD -> main, origin/main, origin/HEAD) docs: add course name to README
* 982a10d docs: add author to README
* 4af168e docs: describe project in README
* a37cf79 feat(ansible): add inventory, config and playbook
* c4d3d85 feat(docker): add Dockerfile and index page
* 93c6522 docs: add README and .env.example
* 1acb0c3 chore: add .gitignore
```
Вывод `git show HEAD~1`:
```
commit 982a10d4e0634eb10329027f5d1c624597e797dc
Author: Tynchybek_uulu_Azim <tynchybek_uulu_a@iuca.kg>
Date:   Tue Oct 27 07:08:34 2026 +0600

    docs: add author to README

diff --git a/README.md b/README.md
index 275a476..4c126a3 100644
--- a/README.md
+++ b/README.md
@@ -2,4 +2,6 @@
 
 Учебный инфраструктурный репозиторий курса 
 
-Репозиторий для практики по Git, Docker и Ansible.
\ No newline at end of file
+Репозиторий для практики по Git, Docker и Ansible.
+
+Автор: Tynchybek_uulu_Azim
\ No newline at end of file
```
`HEAD~1` — это родитель текущего коммита, то есть предыдущий коммит: `982a10d docs: add author to README`. История линейная (граф без ответвлений), так как веток пока не было.

### 2.2 Ветка и слияние

```bash
git switch -c feature/healthcheck
echo 'HEALTHCHECK CMD wget -qO- http://localhost/ || exit 1' >> docker/Dockerfile
git commit -am "feat(docker): add healthcheck"
git switch main
git merge feature/healthcheck
git branch -d feature/healthcheck
```
Создание ветки и проверка:
```
PS> git switch -c feature/healthcheck
Switched to a new branch 'feature/healthcheck'
PS> git branch
* feature/healthcheck
  main
```
Вывод `git diff` после добавления строки в Dockerfile:
```
warning: in the working copy of 'docker/Dockerfile', LF will be replaced by CRLF the next time Git touches it
diff --git a/docker/Dockerfile b/docker/Dockerfile
index cd3628e..f1a8261 100644
--- a/docker/Dockerfile
+++ b/docker/Dockerfile
@@ -1,2 +1,4 @@
 FROM nginx:1.27-alpine
 COPY index.html /usr/share/nginx/html/
+
+HEALTHCHECK CMD wget -qO- http://localhost/ || exit 1
```
Вывод `git merge` (должно быть `Fast-forward`):
```
Updating a25d851..f5ec569
Fast-forward
 docker/Dockerfile | 2 ++
 1 file changed, 2 insertions(+)
```
Вывод `git branch -d` и `git push`:
```
Deleted branch feature/healthcheck (was f5ec569).

Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 12 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 466 bytes | 466.00 KiB/s, done.
Total 4 (delta 1), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To github.com:Sarkushev/Devops_midterm.git
   a25d851..f5ec569  main -> main
```
Так как в `main` не было новых коммитов, Git просто передвинул указатель main вперёд (fast-forward) без merge-коммита.

### 2.3 Конфликт слияния

Исходный файл `ansible/group_vars/all.yml`:
```
---
app_port: 8080
app_user: deploy
```
Команды (в PowerShell замена значения делается через `-replace`):
```
git switch -c fix/app-port
(Get-Content ansible/group_vars/all.yml) -replace 'app_port: 8080','app_port: 9090' | Set-Content ansible/group_vars/all.yml
git commit -am "fix: set app_port to 9090"
git switch main
(Get-Content ansible/group_vars/all.yml) -replace 'app_port: 8080','app_port: 8000' | Set-Content ansible/group_vars/all.yml
git commit -am "fix: set app_port to 8000"
git merge fix/app-port
```
Вывод:
```
[fix/app-port aa87a3b] fix: set app_port to 9090
 1 file changed, 1 insertion(+), 1 deletion(-)
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
[main 8c5be62] fix: set app_port to 8000
 1 file changed, 1 insertion(+), 1 deletion(-)
Auto-merging ansible/group_vars/all.yml
CONFLICT (content): Merge conflict in ansible/group_vars/all.yml
Automatic merge failed; fix conflicts and then commit the result.
```
Содержимое `all.yml` **с маркерами**:
```yaml
---
<<<<<<< HEAD
app_port: 8000
=======
app_port: 9090
>>>>>>> fix/app-port
app_user: deploy
```
Итоговый файл (выбрано 8000, маркеры удалены):
```yaml
---
app_port: 8000
app_user: deploy
```
Команды разрешения конфликта:
```
Set-Content ansible/group_vars/all.yml "---","app_port: 8000","app_user: deploy"
git add ansible/group_vars/all.yml
git commit -m "merge: resolve app_port conflict"
git log --oneline --graph
git push
```
Вывод:
```
[main 2439c63] merge: resolve app_port conflict

*   2439c63 (HEAD -> main) merge: resolve app_port conflict
|\
| * aa87a3b (fix/app-port) fix: set app_port to 9090
* | 8c5be62 fix: set app_port to 8000
|/
* f5ec569 (origin/main, origin/HEAD) feat(docker): add healthcheck
* a25d851 docs: add course name to README
* 982a10d docs: add author to README
* 4af168e docs: describe project in README
* a37cf79 feat(ansible): add inventory, config and playbook
* c4d3d85 feat(docker): add Dockerfile and index page
* 93c6522 docs: add README and .env.example
* 1acb0c3 chore: add .gitignore

To github.com:Sarkushev/Devops_midterm.git
   f5ec569..2439c63  main -> main
```
Обе ветки изменили одну и ту же строку, поэтому Git не смог слить автоматически. Я вручную выбрал значение и завершил слияние merge-коммитом.

---

## Блок 3. Удалённый репозиторий и Merge Request

### 3.1 Получение изменений с сервера

Отредактировал README.md через веб-интерфейс (Edit), коммит в main.
```bash
git pull
git log --oneline
```
Вывод:
```
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 2), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (3/3), 955 bytes | 106.00 KiB/s, done.
From github.com:Sarkushev/Devops_midterm
Updating 2439c63..9ac123f
Fast-forward
 README.md | 2 ++
 1 file changed, 2 insertions(+)

9ac123f (HEAD -> main, origin/main, origin/HEAD) Add section for Block 3 in README
2439c63 merge: resolve app_port conflict
8c5be62 fix: set app_port to 8000
aa87a3b (fix/app-port) fix: set app_port to 9090
f5ec569 feat(docker): add healthcheck
a25d851 docs: add course name to README
982a10d docs: add author to README
4af168e docs: describe project in README
a37cf79 feat(ansible): add inventory, config and playbook
c4d3d85 feat(docker): add Dockerfile and index page
93c6522 docs: add README and .env.example
1acb0c3 chore: add .gitignore
```
Коммит `9ac123f` создан на GitHub через веб-интерфейс, `git pull` подтянул его без конфликтов (Fast-forward).

### 3.2 Merge Request

```bash
git switch -c feature/ansible-curl
# добавить в ansible/playbook.yml задачу:
#    - name: Install curl
#      ansible.builtin.package:
#        name: curl
#        state: present
git commit -am "feat(ansible): add curl installation task"
git push -u origin feature/ansible-curl
```
Вывод `git diff` перед коммитом:
```
diff --git a/ansible/playbook.yml b/ansible/playbook.yml
index 9814f7b..13ca2bc 100644
--- a/ansible/playbook.yml
+++ b/ansible/playbook.yml
@@ -9,6 +9,11 @@
         name: nginx
         state: present
 
+    - name: Install curl
+      ansible.builtin.package:
+        name: curl
+        state: present
+
     - name: Create application user
       ansible.builtin.user:
         name: "{{ app_user }}"
```
Вывод `git commit` и `git push`:
```
[feature/ansible-curl 69ee7b7] feat(ansible): add curl installation task
 1 file changed, 5 insertions(+)

Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 12 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 394 bytes | 394.00 KiB/s, done.
Total 4 (delta 3), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (3/3), completed with 3 local objects.
remote: Create a pull request for 'feature/ansible-curl' on GitHub by visiting:
remote:      https://github.com/Sarkushev/Devops_midterm/pull/new/feature/ansible-curl
To github.com:Sarkushev/Devops_midterm.git
 * [new branch]      feature/ansible-curl -> feature/ansible-curl
branch 'feature/ansible-curl' set up to track 'origin/feature/ansible-curl'.
```
Описание MR: «Добавлена задача установки curl в ansible/playbook.yml. Нужна, чтобы на веб-серверах можно было проверять доступность сервисов с самой машины.»
Ссылка на PR (на GitHub Merge Request называется Pull Request): https://github.com/Sarkushev/Devops_midterm/pull/1
```bash
git switch main
git pull
```
```
Switched to branch 'main'
Your branch is up to date with 'origin/main'.

remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (1/1), 924 bytes | 308.00 KiB/s, done.
From github.com:Sarkushev/Devops_midterm
   9ac123f..d01eb09  main       -> origin/main
Updating 9ac123f..d01eb09
Fast-forward
 ansible/playbook.yml | 5 +++++
 1 file changed, 5 insertions(+)
```

### 3.3 Уборка веток

```bash
git branch -d feature/ansible-curl
git push origin --delete feature/ansible-curl   # если платформа не удалила сама
git fetch --prune
git branch -a
```
```
PS> git branch -d feature/ansible-curl
Deleted branch feature/ansible-curl (was 69ee7b7).
PS> git push origin --delete feature/ansible-curl
To github.com:Sarkushev/Devops_midterm.git
 - [deleted]         feature/ansible-curl
PS> git fetch --prune
PS> git branch -a
  fix/app-port
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
```
Ветка `feature/ansible-curl` удалена и локально, и на сервере; `git fetch --prune` убрал устаревшую ссылку на неё. Ветка `fix/app-port` осталась от задания 2.3.

---

## Блок 4. Docker

```bash
git switch main
git pull
git switch -c feature/docker-page
# изменить docker/index.html, добавить EXPOSE 80 после COPY, раздел в README
docker build -t infra-web:1.0 docker/
docker run -d --name infra-web -p 8080:80 infra-web:1.0
curl.exe http://localhost:8080
docker rm -f infra-web
git add -A
git commit -m "feat(docker): add personal page and expose port"
git push -u origin feature/docker-page
# открыть MR в main и слить
```
Последние строки `docker build -t infra-web:1.0 docker/`:
```
[+] Building 7.9s (7/7) FINISHED                                        docker:desktop-linux
 => [1/2] FROM docker.io/library/nginx:1.27-alpine@sha256:65645c7bb6a0661892a8b03b89d0743208a18dd2f3f17a54ef4b76fb8e2f2a10   3.6s
 => [2/2] COPY index.html /usr/share/nginx/html/                                                           0.2s
 => exporting to image                                                                                     1.1s
 => => exporting layers                                                                                    0.1s
 => => exporting manifest sha256:9a164a83ded55ac5b9ba0e2038f94f755e728a8f7f6e39baa5d1b361e96ed1e8          0.0s
 => => exporting config sha256:133d51150cd8c71826df792f7cb6c08052bb82e9ac14ddf5cd80698d6d1b1896            0.0s
 => => naming to docker.io/library/infra-web:1.0                                                           0.0s
 => => unpacking to docker.io/library/infra-web:1.0                                                        0.8s
```
Образ собрался успешно (7 из 7 шагов, без ошибок).
Вывод `docker run` и `docker ps`:
```
e1fa061a97f4daa5c716f920f271bc8da9a0d2a47ae3edbf5171a6a7b4a7cf0a

CONTAINER ID   IMAGE           COMMAND                  CREATED          STATUS                             PORTS                                     NAMES
e1fa061a97f4   infra-web:1.0   "/docker-entrypoint.…"   16 seconds ago   Up 15 seconds (health: starting)   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   infra-web
```
Вывод `curl.exe http://localhost:8080` (в PowerShell `curl` — псевдоним `Invoke-WebRequest`, поэтому используется `curl.exe`):
```
<!DOCTYPE html><html lang="ru"><head><meta charset="utf-8"><title>Infra project</title></head><body><h1>Infra project</h1><p>Автор: Tynchybek_uulu_Azim</p></body></html>
```
Контейнер запустился, порт 8080 хоста проброшен на порт 80 nginx, страница с именем отдаётся.
Вывод `git commit` и `git push`:
```
[feature/docker-page 96b9f66] feat(docker): add personal page and expose port
 3 files changed, 10 insertions(+), 5 deletions(-)

Enumerating objects: 11, done.
Counting objects: 100% (11/11), done.
Delta compression using up to 12 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 728 bytes | 728.00 KiB/s, done.
Total 6 (delta 3), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (3/3), completed with 3 local objects.
remote: Create a pull request for 'feature/docker-page' on GitHub by visiting:
remote:      https://github.com/Sarkushev/Devops_midterm/pull/new/feature/docker-page
To github.com:Sarkushev/Devops_midterm.git
 * [new branch]      feature/docker-page -> feature/docker-page
branch 'feature/docker-page' set up to track 'origin/feature/docker-page'.
```
PR (Pull Request): https://github.com/Sarkushev/Devops_midterm/pull/2

Вывод `git switch main` и `git pull` после слияния PR:
```
Switched to branch 'main'
Your branch is up to date with 'origin/main'.

remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (1/1), 928 bytes | 464.00 KiB/s, done.
From github.com:Sarkushev/Devops_midterm
   d01eb09..e768d1c  main       -> origin/main
Updating d01eb09..e768d1c
Fast-forward
 README.md         | 7 +++++++
 docker/Dockerfile | 2 ++
 docker/index.html | 6 +-----
 3 files changed, 10 insertions(+), 5 deletions(-)
```

---

## Блок 5. Исправление ошибок и контрольные вопросы

### 5.1 Отмена изменений и правка коммита

Отмена несохранённого изменения:
```
PS> Add-Content docker/Dockerfile "# extra line"
PS> git diff
diff --git a/docker/Dockerfile b/docker/Dockerfile
index eb27c86..5999a8d 100644
--- a/docker/Dockerfile
+++ b/docker/Dockerfile
@@ -4,3 +4,4 @@ COPY index.html /usr/share/nginx/html/
 EXPOSE 80
 
 HEALTHCHECK CMD wget -qO- http://localhost/ || exit 1
+# extra line
PS> git restore docker/Dockerfile
PS> git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```
Коммит с опечаткой, исправление через `--amend` и отправка:
```
PS> Add-Content README.md "`nExtra note."
PS> git commit -am "docs: updte readme"
[main 7f11276] docs: updte readme
 1 file changed, 2 insertions(+), 1 deletion(-)
PS> git log --oneline -1
7f11276 (HEAD -> main) docs: updte readme
PS> git commit --amend -m "docs: update readme"
[main 30e2c5d] docs: update readme
 Date: Tue Oct 27 13:20:44 2026 +0600
 1 file changed, 2 insertions(+), 1 deletion(-)
PS> git log --oneline -1
30e2c5d (HEAD -> main) docs: update readme
PS> git push
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 12 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 311 bytes | 311.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To github.com:Sarkushev/Devops_midterm.git
   e768d1c..30e2c5d  main -> main
```
Хэш коммита изменился (`7f11276` → `30e2c5d`): `--amend` создаёт новый коммит вместо прежнего. Опечатка исправлена до `git push`, поэтому на сервере коммита с `updte` нет.

### 5.2 Отмена опубликованного коммита

Коммит, который удаляет `*.log` из `.gitignore`, и его отправка на сервер:
```
PS> (Get-Content .gitignore) | Where-Object { $_ -ne '*.log' } | Set-Content -Encoding ascii .gitignore
PS> git diff
diff --git a/.gitignore b/.gitignore
index 1cd9497..fb7e627 100644
--- a/.gitignore
@@ -1,3 +1,2 @@
 .env
-*.log
 *.retry
PS> git commit -am "chore: remove *.log from .gitignore"
[main e2896a8] chore: remove *.log from .gitignore
 1 file changed, 1 deletion(-)
PS> git push
   30e2c5d..e2896a8  main -> main
PS> git log --oneline -1
e2896a8 (HEAD -> main, origin/main, origin/HEAD) chore: remove *.log from .gitignore
```
Отмена через `git revert` (хэш `e2896a8` — это `HEAD`) и отправка:
```
PS> git revert HEAD --no-edit
[main e9dc00c] Revert "chore: remove *.log from .gitignore"
 Date: Tue Oct 27 13:22:51 2026 +0600
 1 file changed, 1 insertion(+)
PS> git push
   e2896a8..e9dc00c  main -> main
PS> git log --oneline -3
e9dc00c (HEAD -> main, origin/main, origin/HEAD) Revert "chore: remove *.log from .gitignore"
e2896a8 chore: remove *.log from .gitignore
30e2c5d docs: update readme
```
Исходный коммит остался в истории, а новый коммит `e9dc00c` вернул строку `*.log` в `.gitignore`.

**Почему revert, а не удаление из истории:** после `git push` историю уже могли скачать другие, и её переписывание (reset/rebase + force-push) сломало бы их копии и вызвало расхождения. `git revert` добавляет новый коммит, отменяющий изменения, и история остаётся целостной и безопасной для всех.

### 5.3 Контрольные вопросы

**1. Что такое коммит и зачем нужна область индекса?**
Коммит — это снимок состояния проекта с автором, датой, сообщением и ссылкой на родительский коммит. Индекс (staging area) — промежуточная зона между рабочей директорией и коммитом: в ней я собираю именно те изменения, которые хочу включить в следующий коммит. Так можно делать небольшие осмысленные коммиты, даже если изменено много файлов.

**2. Что такое ветка в Git?**
Ветка — это лёгкий подвижный указатель на коммит. При новом коммите в текущей ветке указатель сдвигается вперёд. Ветки позволяют вести независимую разработку и потом объединять результаты.

**3. Что такое HEAD?**
HEAD — указатель на текущее положение: обычно на текущую ветку (а через неё на последний коммит), то есть на то, что сейчас загружено в рабочую директорию. Следующий коммит станет потомком коммита, на который указывает HEAD.

**4. Чем `git fetch` отличается от `git pull`?**
`git fetch` только скачивает новые объекты и обновляет удалённые ветки (`origin/main`), не трогая рабочие файлы и локальные ветки. `git pull` — это `fetch` плюс слияние (merge или rebase) в текущую ветку, поэтому он сразу меняет локальную работу.

**5. Почему файл `.env` нельзя добавлять в репозиторий?**
В нём лежат секреты (пароли, токены, ключи). Попав в репозиторий, они остаются в истории даже после удаления файла, видны всем, у кого есть доступ, и могут утечь при публикации. Вместо этого в репозитории хранят `.env.example` без значений, а `.env` добавляют в `.gitignore`.
