.PHONY: *

# имя
export NAME?=$(shell echo $(shell basename $(shell pwd)) | awk '{print tolower($0)}')

# основная цель это создать образ и котейнер для запуска командной оболочки
$(NAME): image container
	@docker-compose up -d

# собрать образ
image:
	@docker-compose build $(NAME)

# создать контейнер
container:
	@docker-compose up --no-start

# удалить контейнер
down:
	@docker-compose down

# командная оболочка
shell:
	@docker-compose run $(NAME) /bin/bash

# информация
info:
	@docker-compose ps
	@docker-compose logs --tail=100
