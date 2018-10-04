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
	
* Installing python-systemd package on Ubuntu 16.04

.. code-block:: none

	sudo apt-get install -y python-systemd	
	
* Need to update package source information for Ubuntu Basic Package installation

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
	cd ./devstack
	
Devstack configuration file
------------------------------

.. code-block:: none

	cp ./samples/local.conf .
	vi local.conf 

Devstack SAIO configuration file setup
------------------------------

.. code-block:: none

	[[local|localrc]]
	HOST_IP=10.1.0.6
    FLAT_INTERFACE=eth0
	ADMIN_PASSWORD=secret
	DATABASE_PASSWORD=$ADMIN_PASSWORD
	RABBIT_PASSWORD=$ADMIN_PASSWORD
	SERVICE_PASSWORD=$ADMIN_PASSWORD
	enable_service s-proxy s-object s-container s-account
	SWIFT_REPLICAS=1
	SWIFT_HASH=66a3d6b56c1f479c8b4e70ab5c2000f5
	enable_service h-eng h-api h-api-cfn h-api-cw
	enable_plugin heat git://git.openstack.org/openstack/heat
	FLOATING_RANGE=192.168.42.128/25

 * Set FLAT_INTERFACE to the Ethernet interface that connects the host to your local network. 
   This is the interface that should be configured with the static IP address mentioned above.
 * Set FLOATING_RANGE to a range not used on the local network. ex) 192.168.42.128/25
   This configures IP addresses ending in 225-254 to be used as floating IPs.
 * Set SWIFT_REPLICAS to every object in Swift is replicated across different devices and nodes
 
Devstack SAIO installation start
------------------------------

.. code-block:: none

	./stack.sh

	
Devstack SAIO start complete Report 
------------------------------
.. code-block:: none

	=========================
	DevStack Component Timing
	(times are in seconds)
	=========================
	run_process           37
	test_with_retry        3
	apt-get-update        10
	pip_install          500
	osc                  243
	wait_for_service      32
	git_timed            328
	dbsync                65
	apt-get              367
	-------------------------
	Unaccounted time     599
	=========================
	Total runtime        2184

This is your host IP address: 10.1.0.6
This is your host IPv6 address: ::1
Horizon is now available at http://10.1.0.6/dashboard
Keystone is serving at http://10.1.0.6/identity/
The default users are: admin and demo
The password: secret