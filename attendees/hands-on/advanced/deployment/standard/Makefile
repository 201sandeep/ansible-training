build:
	docker build -t training/ansible:nginx -f nginx/Dockerfile.nginx .
       
build-haproxy:
	docker build -t training/ansible:haproxy -f haproxy/Dockerfile.haproxy . 

build-mysql:
	docker build -t training/ansible:mysql -f mysql/Dockerfile.mysql .

build-tomcat:
	docker build -t training/ansible:tomcat -f tomcat/Dockerfile.tomcat .

	
build-all:
	make build
	make build-mysql
	make build-tomcat
	make build-haproxy

run-dbserver:
	docker rm -f dbserver || true
	docker run -h dbserver --name dbserver -e MYSQL_ROOT_PASSWORD=root -p3306:3306 -itd training/ansible:mysql

run-webapp-a:
	docker rm -f webappa || true
	docker run -h webappa --name webappa -p8080:8080 -itd training/ansible:tomcat

run-webapp-b:
	docker rm -f webappb  || true
	docker run -h webappb --name webappb -p8081:8080 -itd training/ansible:tomcat

run-webapp-c:
	docker rm -f webappc  || true
	docker run -h webappb --name webappc -p8082:8080 -itd training/ansible:tomcat

run-webapp-d:
	docker rm -f webappd  || true
	docker run -h webappd --name webappd -p8083:8080 -itd training/ansible:tomcat

run-nginx-1:
	docker rm -f nginx1 || true
	docker run -h nginx1 --name nginx1 -p8050:80 --link webappa:webappa --link webappb:webappb --link webappc:webappc --link webappd:webappd -itd training/ansible:nginx

run-nginx-2:
	docker rm -f nginx2 || true
	docker run -h nginx2 --name nginx2 -p8052:80 --link webappa:webappa --link webappb:webappb --link webappc:webappc --link webappd:webappd -itd training/ansible:nginx

run-haproxy:
	docker rm -f haproxy || true
	docker run -h haproxy --name haproxy -p80:80 --link nginx1:nginx1 --link nginx2:nginx2 -itd training/ansible:haproxy

run-all:
	make run-dbserver
	make run-webapp-a
	make run-webapp-b
	make run-webapp-c
	make run-webapp-d
	make run-nginx-1
	make run-nginx-2
	make run-haproxy

start-lab:
	make build-all
	make run-all
