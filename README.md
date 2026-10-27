# Infra project

Учебный инфраструктурный репозиторий курса 

Репозиторий для практики по Git, Docker и Ansible.

Автор: Tynchybek_uulu_Azim
Course: DevOps midterm

Блок 3

## Запуск в Docker

```bash
docker build -t infra-web:1.0 docker/
docker run -d --name infra-web -p 8080:80 infra-web:1.0
```
Extra note.
