---
layout: page
title: Native Docker Clustering
permalink: /linux/containers/docker/swarm/Native_Docker_Clustering/Securing_your_Swarm_Cluster/
---

# Docker Swarm: Native Docker Clustering [2016, ENG] > Module 5: Securing your Swarm Cluster


<br/>

<div align="center">
    <img src="//files.sysadm.ru/img/linux/containers/docker/swarm/native-docker-clustering/pic3.png" border="0" alt="Native Docker Clustering">
</div>

<br/>


Я пока не собираюсь глубоко разбираться в security. Тут и до security непоняного много.


### CREATE CA

<br/>

    <!-- **core 07**

    $ vagrant ssh core-07

    $ sudo su - -->

    # openssl genrsa -out ca-key.pem 2048

    # openssl req -config /usr/lib/ssl/openssl.cnf -new -key ca-key.pem -x509 -days 1825 -out ca-cert.pem



<br/>

### CREATE MANAGER AND NODE KEYS

	# openssl genrsa -out manager1-key.pem 2048
	# openssl req -subj "/CN=manager1" -new -key manager1-key.pem -out manager1.csr
	# echo subjectAltName = IP:10.0.1.5,IP:127.0.0.1 > extfile.cnf
	# openssl x509 -req -days 365 -in manager1.csr -CA ca-cert.pem -CAkey ca-key.pem -CAcreateserial -out manager1-cert.pem -extfile extfile.cnf

	# openssl genrsa -out manager2-key.pem 2048
	# openssl req -subj "/CN=manager2" -new -key manager2-key.pem -out manager2.csr
	# echo subjectAltName = IP:10.0.2.5,IP:127.0.0.1 > extfile.cnf
	# openssl x509 -req -days 365 -in manager2.csr -CA ca-cert.pem -CAkey ca-key.pem -CAcreateserial -out manager2-cert.pem -extfile extfile.cnf

	# openssl genrsa -out manager3-key.pem 2048
	# openssl req -subj "/CN=manager3" -new -key manager3-key.pem -out manager3.csr
	# echo subjectAltName = IP:10.0.3.5,IP:127.0.0.1 > extfile.cnf
	# openssl x509 -req -days 365 -in manager3.csr -CA ca-cert.pem -CAkey ca-key.pem -CAcreateserial -out manager3-cert.pem -extfile extfile.cnf

	# openssl genrsa -out node1-key.pem 2048
	# openssl req -subj "/CN=node1" -new -key node1-key.pem -out node1.csr
	# echo subjectAltName = IP:10.0.4.5,IP:127.0.0.1 > extfile.cnf
	# openssl x509 -req -days 365 -in node1.csr -CA ca-cert.pem -CAkey ca-key.pem -CAcreateserial -out node1-cert.pem -extfile extfile.cnf

	# openssl genrsa -out node2-key.pem 2048
	# openssl req -subj "/CN=node2" -new -key node2-key.pem -out node2.csr
	# echo subjectAltName = IP:10.0.5.5,IP:127.0.0.1 > extfile.cnf
	# openssl x509 -req -days 365 -in node2.csr -CA ca-cert.pem -CAkey ca-key.pem -CAcreateserial -out node2-cert.pem -extfile extfile.cnf

	# openssl genrsa -out node3-key.pem 2048
	# openssl req -subj "/CN=node3" -new -key node3-key.pem -out node3.csr
	# echo subjectAltName = IP:10.0.6.5,IP:127.0.0.1 > extfile.cnf
	# openssl x509 -req -days 365 -in node3.csr -CA ca-cert.pem -CAkey ca-key.pem -CAcreateserial -out node3-cert.pem -extfile extfile.cnf



### CREATE CLIENT KEYS

	# openssl genrsa -out client-key.pem 2048
	# openssl req -subj "/CN=client" -new -key client-key.pem -out client.csr
	# echo extendedKeyUsage = clientAuth > extfile.cnf
	# openssl x509 -req -days 365 -in client.csr -CA ca-cert.pem -CAkey ca-key.pem -CAcreateserial -out client-cert.pem -extfile extfile.cnf



    # ls -l


<br/>

### COPY KEYS

    На нодах сначала нужно создать .docker

    mkdir .docker
    chmod 777 .docker/


    # scp ./ca-cert.pem ubuntu@manager1:/home/ubuntu/.docker/ca.pem


**Manager1**

    scp -i eu-west-1-key.pem ./ca-cert.pem ubuntu@manager1:/home/ubuntu/.docker/ca.pem

    scp -i eu-west-1-key.pem ./manager1-cert.pem ubuntu@manager1:/home/ubuntu/.docker/cert.pem

	scp -i eu-west-1-key.pem ./manager1-key.pem ubuntu@manager1:/home/ubuntu/.docker/key.pem


**Manager2**


	scp -i eu-west-1-key.pem ./ca-cert.pem ubuntu@manager2:/home/ubuntu/.docker/ca.pem

	scp -i eu-west-1-key.pem ./manager2-cert.pem ubuntu@manager2:/home/ubuntu/.docker/cert.pem

	scp -i eu-west-1-key.pem ./manager2-key.pem ubuntu@manager2:/home/ubuntu/.docker/key.pem


**Manager3**    

	scp -i eu-west-1-key.pem ./ca-cert.pem ubuntu@manager3:/home/ubuntu/.docker/ca.pem

	scp -i eu-west-1-key.pem ./manager3-cert.pem ubuntu@manager3:/home/ubuntu/.docker/cert.pem

	scp -i eu-west-1-key.pem ./manager3-key.pem ubuntu@manager3:/home/ubuntu/.docker/key.pem


**Node1**      

	scp -i eu-west-1-key.pem ./ca-cert.pem ubuntu@node1:/home/ubuntu/.docker/ca.pem

    scp -i eu-west-1-key.pem ./node1-cert.pem ubuntu@node1:/home/ubuntu/.docker/cert.pem

	scp -i eu-west-1-key.pem ./node1-key.pem ubuntu@node1:/home/ubuntu/.docker/key.pem


**Node2**      

	scp -i eu-west-1-key.pem ./ca-cert.pem ubuntu@node2:/home/ubuntu/.docker/ca.pem

	scp -i eu-west-1-key.pem ./node2-cert.pem ubuntu@node2:/home/ubuntu/.docker/cert.pem

	scp -i eu-west-1-key.pem ./node2-key.pem ubuntu@node2:/home/ubuntu/.docker/key.pem


**Node3**  

    scp -i eu-west-1-key.pem ./ca-cert.pem ubuntu@node3:/home/ubuntu/.docker/ca.pem

	scp -i eu-west-1-key.pem ./node3-cert.pem ubuntu@node3:/home/ubuntu/.docker/cert.pem

	scp -i eu-west-1-key.pem ./node3-key.pem ubuntu@node3:/home/ubuntu/.docker/key.pem


**Client**

    scp -i eu-west-1-key.pem ./ca-cert.pem ubuntu@client:/home/ubuntu/.docker/ca.pem

	scp -i eu-west-1-key.pem ./client-cert.pem ubuntu@client:/home/ubuntu/.docker/cert.pem

	scp -i eu-west-1-key.pem ./client-key.pem ubuntu@client:/home/ubuntu/.docker/key.pem


<br/>

### DOCKER DAEMON restarts


**На всех нодах**

	# vim /etc/default/docker


Заменили на (было закомментировано):

	DOCKER_OPTS="-H tcp://0.0.0.0:2376 --tlsverify --tlscacert=/home/ubuntu/.docker/ca.pem --tlscert=/home/ubuntu/.docker/cert.pem --tlskey=/home/ubuntu/.docker/key.pem"

<br/>

	# service docker start
	# service docker status

    # ps -elf | grep docker


<br/>

### START NEW CONSUL SERVERS

	MANAGER1

	# docker -H tcp://manager1:2376 --tlsverify --tlscacert=/home/ubuntu/.docker/ca.pem --tlscert=/home/ubuntu/.docker/cert.pem --tlskey=/home/ubuntu/.docker/key.pem run --restart=unless-stopped -d -h consul1 --name consul1 -v /mnt:/data     -p 10.0.1.5:8300:8300     -p 10.0.1.5:8301:8301     -p 10.0.1.5:8301:8301/udp     -p 10.0.1.5:8302:8302     -p 10.0.1.5:8302:8302/udp     -p 10.0.1.5:8400:8400     -p 10.0.1.5:8500:8500     -p 172.17.0.1:53:53/udp     progrium/consul -server -advertise 10.0.1.5 -join 10.0.2.5


	MANAGER2

	# docker -H tcp://manager2:2376 --tlsverify --tlscacert=/home/ubuntu/.docker/ca.pem --tlscert=/home/ubuntu/.docker/cert.pem --tlskey=/home/ubuntu/.docker/key.pem run --restart=unless-stopped -d -h consul2 --name consul2 -v /mnt:/data     -p 10.0.2.5:8300:8300     -p 10.0.2.5:8301:8301     -p 10.0.2.5:8301:8301/udp     -p 10.0.2.5:8302:8302     -p 10.0.2.5:8302:8302/udp     -p 10.0.2.5:8400:8400     -p 10.0.2.5:8500:8500     -p 172.17.0.1:53:53/udp     progrium/consul -server -advertise 10.0.1.5 -join 10.0.1.5

	MANAGER3

	# docker -H tcp://manager3:2376 --tlsverify --tlscacert=/home/ubuntu/.docker/ca.pem --tlscert=/home/ubuntu/.docker/cert.pem --tlskey=/home/ubuntu/.docker/key.pem run --restart=unless-stopped -d -h consul3 --name consul3 -v /mnt:/data     -p 10.0.3.5:8300:8300     -p 10.0.3.5:8301:8301     -p 10.0.3.5:8301:8301/udp     -p 10.0.3.5:8302:8302     -p 10.0.3.5:8302:8302/udp     -p 10.0.3.5:8400:8400     -p 10.0.3.5:8500:8500     -p 172.17.0.1:53:53/udp     progrium/consul -server -advertise 10.0.1.5 -join 10.0.1.5


	# docker -H tcp://manager1:2376 --tlsverify --tlscacert=/home/ubuntu/.docker/ca.pem --tlscert=/home/ubuntu/.docker/cert.pem --tlskey=/home/ubuntu/.docker/key.pem run --restart=unless-stopped -h mgr1 --name mgr1 -d -p 3376:2376 -v /home/ubuntu/.docker:/certs:ro swarm manage --tlsverify --tlscacert=/certs/ca.pem --tlscert=/certs/cert.pem --tlskey=/certs/key.pem --host=0.0.0.0:2376 --replication --advertise 10.0.1.5:2376 consul://10.0.1.5:8500/


<br/>

### START CONSUL CLIENTS

	NODE1

		# docker -H tcp://node1:2376 --tlsverify --tlscacert=/home/ubuntu/.docker/ca.pem --tlscert=/home/ubuntu/.docker/cert.pem --tlskey=/home/ubuntu/.docker/key.pem run --restart=unless-stopped -d -h consul-agt1 --name consul-agt1 \
		-p 8300:8300 \
		-p 8301:8301 -p 8301:8301/udp \
		-p 8302:8302 -p 8302:8302/udp \
		-p 8400:8400 \
		-p 8500:8500 \
		-p 8600:8600/udp \
		progrium/consul -rejoin -advertise 10.0.4.5 -join 10.0.1.5

	NODE2

		# docker -H tcp://node2:2376 --tlsverify --tlscacert=/home/ubuntu/.docker/ca.pem --tlscert=/home/ubuntu/.docker/cert.pem --tlskey=/home/ubuntu/.docker/key.pem run --restart=unless-stopped -d -h consul-agt2 --name consul-agt2 \
		-p 8300:8300 \
		-p 8301:8301 -p 8301:8301/udp \
		-p 8302:8302 -p 8302:8302/udp \
		-p 8400:8400 \
		-p 8500:8500 \
		-p 8600:8600/udp \
		progrium/consul -rejoin -advertise 10.0.5.5 -join 10.0.1.5

	NODE3

		# docker -H tcp://node3:2376 --tlsverify --tlscacert=/home/ubuntu/.docker/ca.pem --tlscert=/home/ubuntu/.docker/cert.pem --tlskey=/home/ubuntu/.docker/key.pem run --restart=unless-stopped -d -h consul-agt3 --name consul-agt3 \
		-p 8300:8300 \
		-p 8301:8301 -p 8301:8301/udp \
		-p 8302:8302 -p 8302:8302/udp \
		-p 8400:8400 \
		-p 8500:8500 \
		-p 8600:8600/udp \
		progrium/consul -rejoin -advertise 10.0.6.5 -join 10.0.1.5


<br/>

### START SWARM MANAGERS

	MNAGER1

		# docker -H tcp://manager1:2376 --tlsverify --tlscacert=/home/ubuntu/.docker/ca.pem --tlscert=/home/ubuntu/.docker/cert.pem --tlskey=/home/ubuntu/.docker/key.pem run --restart=unless-stopped -h mgr1 --name mgr1 -d -p 3376:2376 -v /home/ubuntu/.docker:/certs:ro swarm manage --tlsverify --tlscacert=/certs/ca.pem --tlscert=/certs/cert.pem --tlskey=/certs/key.pem --host=0.0.0.0:2376 --replication --advertise 10.0.1.5:2376 consul://10.0.1.5:8500/

	MANAGER2

		# docker -H tcp://manager2:2376 --tlsverify --tlscacert=/home/ubuntu/.docker/ca.pem --tlscert=/home/ubuntu/.docker/cert.pem --tlskey=/home/ubuntu/.docker/key.pem run --restart=unless-stopped -h mgr2 --name mgr2 -d -p 3376:2376 -v /home/ubuntu/.docker:/certs:ro swarm manage --tlsverify --tlscacert=/certs/ca.pem --tlscert=/certs/cert.pem --tlskey=/certs/key.pem --host=0.0.0.0:2376 --replication --advertise 10.0.2.5:2376 consul://10.0.2.5:8500/

	MANAGER3

		# docker -H tcp://manager3:2376 --tlsverify --tlscacert=/home/ubuntu/.docker/ca.pem --tlscert=/home/ubuntu/.docker/cert.pem --tlskey=/home/ubuntu/.docker/key.pem run --restart=unless-stopped -h mgr3 --name mgr3 -d -p 3376:2376 -v /home/ubuntu/.docker:/certs:ro swarm manage --tlsverify --tlscacert=/certs/ca.pem --tlscert=/certs/cert.pem --tlskey=/certs/key.pem --host=0.0.0.0:2376 --replication --advertise 10.0.3.5:2376 consul://10.0.3.5:8500/

<br/>

### START SWARM JOIN CONTAIENRS ON EACH NODE

	NODE1

		# docker -H tcp://node1:2376 --tlsverify --tlscacert=/home/ubuntu/.docker/ca.pem --tlscert=/home/ubuntu/.docker/cert.pem --tlskey=/home/ubuntu/.docker/key.pem run -d -h join --name join swarm join --advertise=10.0.4.5:2376 consul://10.0.4.5:8500/

	NODE2

		# docker -H tcp://node2:2376 --tlsverify --tlscacert=/home/ubuntu/.docker/ca.pem --tlscert=/home/ubuntu/.docker/cert.pem --tlskey=/home/ubuntu/.docker/key.pem run -d -h join --name join swarm join --advertise=10.0.5.5:2376 consul://10.0.5.5:8500/

	NODE3

		# docker -H tcp://node3:2376 --tlsverify --tlscacert=/home/ubuntu/.docker/ca.pem --tlscert=/home/ubuntu/.docker/cert.pem --tlskey=/home/ubuntu/.docker/key.pem run -d -h join --name join swarm join --advertise=10.0.6.5:2376 consul://10.0.6.5:8500/



<br/>


    **client**

    $ export DOCKER_HOST=manager1:3376
    $ docker version

    $ export DOCKER_TLS_VERIFY=1
    $ export DOCKER_CERT_PATH=/home/ubuntu/.docker

    $ ls -l .docker/
    (3 сертификата)

    $ docker version
    (получаем данные о клиенте и сервере)
