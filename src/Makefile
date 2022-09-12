rebuild:
	docker build --no-cache -t maestrow/n8n-devapi .

build:
	docker build -t maestrow/n8n-devapi .

run:
	docker run -ti --rm maestrow/n8n-devapi

attach:
	docker exec -ti n8n-devapi sh

push:
	docker push maestrow/n8n-devapi

up:
	docker-compose up -d

stop:
	docker-compose stop

start:
	docker-compose start

down:
	docker-compose down