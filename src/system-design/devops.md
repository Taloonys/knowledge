---
tags: [system-design]
aliases: [devops]
---

> Тестирование, CI/CD.

* Во все пункты ниже можно добавить Gitlab, в нём буквально всё...

* Инфарструктура
	* Terraform
	* Ansible
* CI/CD
	* Jenkins
	* Github Actions
* CI
	* Harbor - в нём можно хранить контейнеры
* Секреты
	* Любые env (древнее) - файлики и т.п.
	* Docker secrets - механизм докера
	* Vault - общий сервис для хранения секретов
* Kubernetes
	* Argo - Строго CD
	* Умеет в масштабирование
	* Репликацию
	* Load balance
	* Helm (и их чарты) - некоторый описатель сервисов