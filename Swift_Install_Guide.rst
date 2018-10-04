.. _static/myscript:
Devtack-Swift-install 
=====================
Devstack 기반의 Swift install Guide 문서이다.

`Devstack <https://docs.openstack.org/devstack/latest/>`_ Install문서를 참고하여 작성함 

Prerequisites
------------------------------
* DevStack setup requires to have 1 VM/ BM machine with internet connectivity.
* Devstack은 현재 Ubuntu16.04 및 CentOS 7을 지원하며, Devstack은 공식적으로 Ubuntu16.04를 권장함에 따라 Ubuntu16.04 설치함.
* Install Git

.. code-block:: none

	apt-get install git

* Ubuntu 기본 패키지 설치를 위하여 소스정보를 업데이트 필요함

.. code-block:: none

	sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
	sudo sed 's/kr.archive.ubuntu.com/mirror.kakao.com/g' /etc/apt/sources.list
	sudo apt-get update
	sudo apt-get upgrade
   
Add stack User
------------------------------

.. code-block:: none

  sudo useradd -s /bin/bash -d /opt/stack -m stack

* stack 사용자는 시스템을 많이 변경하므로 sudo 권한이 꼭 필요함(sudo 명령어에 따른 password를 묻지 않도록 설정)

.. code-block:: none

	echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack
	sudo su - stack

Download DevStack
------------------------------

.. code-block:: none

	git clone https://git.openstack.org/openstack-dev/devstack
	cd devstack
	
Devstack configuration file
------------------------------

.. code-block:: none

	vi local.conf
	
or

.. code-block:: none	

	cp /smaple/local.conf ./
	vi local.conf 

Devstack configuration file setup
------------------------------

.. code-block:: none

	[[local|localrc]]
	ADMIN_PASSWORD=secret
	DATABASE_PASSWORD=$ADMIN_PASSWORD
	RABBIT_PASSWORD=$ADMIN_PASSWORD
	SERVICE_PASSWORD=$ADMIN_PASSWORD




