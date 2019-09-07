all: build

.PHONY: clean
clean:
	rm -rf torb

deploy:
	GOPATH=`pwd`:`pwd`/vendor GOOS=linux GOARCH=amd64 go build -v torb
	ssh isucon 'sudo systemctl stop isucon.service'
	scp ./torb isucon:~/isucon8-qualify/webapp/go
	ssh isucon 'sudo systemctl start isucon.service'

deps:
	gb vendor restore

.PHONY: build
build:
	GOPATH=`pwd`:`pwd`/vendor go build -v torb

bench: 
	cd ../../bench && bin/bench -remotes=127.0.0.1 -output result.json && cd -

bench-remote: 
	cd ../../bench && bin/bench -remotes=52.195.6.115 -output result.json && cd -

result:
	jq . < ../../bench/result.json | less

db-start:
	docker run -d -v `pwd`/scripts/mysql/conf.d:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=root --name isucon-mysql-local -p 3306:3306 mysql:5.7;

db-clean:
	docker ps -aq | xargs docker stop | docker container prune -f
	

db-init:
	MYSQL_HOST=127.0.0.1 MYSQL_PWD=root bash ../../db/init-user.sh
	MYSQL_HOST=127.0.0.1 MYSQL_PWD=root bash ../../db/init.sh

server: 
	DB_DATABASE=torb DB_HOST=127.0.0.1 DB_PORT=3306 DB_USER=isucon DB_PASS=isucon ./torb

