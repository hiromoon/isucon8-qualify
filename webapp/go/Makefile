all: build

.PHONY: clean
clean:
	rm -rf torb

deps:
	gb vendor restore

.PHONY: build
build:
	GOPATH=`pwd`:`pwd`/vendor go build -v torb

bench: 
	cd ../../bench && bin/bench -remotes=127.0.0.1 -output result.json && cd -

result:
	jq . < ../../bench/result.json | less

db-init:
	../../db/init.sh

server: 
	DB_DATABASE=torb DB_HOST=localhost DB_PORT=3306 DB_USER=isucon DB_PASS=isucon ./torb
