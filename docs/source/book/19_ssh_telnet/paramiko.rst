Module paramiko
---------------

Paramiko is an implementation of the SSHv2 protocol on Python. Paramiko provides client-server functionality. We will consider only client functionality.

Since Paramiko is not part of standard Python module library, it needs to be installed:

::

    pip install paramiko

Example of using Paramiko (3_paramiko.py file):

.. literalinclude:: /pyneng-examples-exercises/examples/19_ssh_telnet/3_paramiko.py
  :language: python
  :linenos:


Comments to the script:

* ``client = paramiko.SSHClient()`` - this class represents connection to SSH server. It performs client authentication. 
* ``client.set_missing_host_key_policy(paramiko.AutoAddPolicy())`` 

  * ``set_missing_host_key_policy`` - sets which policy to use when connecting to a server whose key is unknown. 
  * ``paramiko.AutoAddPolicy()`` - policy that automatically adds new host name and key to the local HostKeys object.

* ``client.connect(IP, username=USER, password=PASSWORD, look_for_keys=False, allow_agent=False)``

  * ``client.connect`` - method that connects to SSH server and authenticates the connection 

    * ``hostname`` - host name or IP address
    * ``username`` - username
    * ``password`` - password
    * ``look_for_keys`` - by default paramiko performs key authentication. To disable this, put the flag in False
    * ``allow_agent`` - paramiko can connect to a local SSH agent. This is necessary when working with keys and since in this case authentication is done by login/password, it should be disabled. 

  * ``with client.invoke_shell() as ssh`` - after execution of previous command there is already a connection to the server. Method ``invoke_shell`` allows to set an interactive SSH session with server.
  * o	Within the established session, commands are executed and data are obtained:

    * ``ssh.send`` - sends specified string to session
    * ``ssh.recv`` - receives data from session. In brackets, the maximum value in bytes that can be obtained is indicated. This method returns a received string
    * 	In addition, somewhere in between of sending and receiving commands, there is a ``time.sleep``. It specifies how long to wait before the script continues to run. This is done to await the execution of command on equipment

This is the script result:

::

    $ python 3_paramiko.py "sh ip int br"
    Username: cisco
    Password:
    Enter enable secret:
    Connection to device 192.168.100.1

    R1>enable
    Password:
    R1#terminal length 0


    R1#
    sh ip int br
    Interface              IP-Address      OK? Method Status                Protocol
    FastEthernet0/0        192.168.100.1   YES NVRAM  up                    up
    FastEthernet0/1        unassigned      YES NVRAM  up                    up
    FastEthernet0/1.10     10.1.10.1       YES manual up                    up
    FastEthernet0/1.20     10.1.20.1       YES manual up                    up
    FastEthernet0/1.30     10.1.30.1       YES manual up                    up
    FastEthernet0/1.40     10.1.40.1       YES manual up                    up
    FastEthernet0/1.50     10.1.50.1       YES manual up                    up
    FastEthernet0/1.60     10.1.60.1       YES manual up                    up
    FastEthernet0/1.70     10.1.70.1       YES manual up                    up
    R1#
    Connection to device 192.168.100.2

    R2>enable
    Password:
    R2#terminal length 0
    R2#
    sh ip int br
    FastEthernet0/0        192.168.100.2   YES NVRAM  up                    up
    FastEthernet0/1        unassigned      YES NVRAM  up                    up
    FastEthernet0/1.10     10.2.10.1       YES manual up                    up
    FastEthernet0/1.20     10.2.20.1       YES manual up                    up
    FastEthernet0/1.30     10.2.30.1       YES manual up                    up
    FastEthernet0/1.40     10.2.40.1       YES manual up                    up
    FastEthernet0/1.50     10.2.50.1       YES manual up                    up
    FastEthernet0/1.60     10.2.60.1       YES manual up                    up
    FastEthernet0/1.70     10.2.70.1       YES manual up                    up
    R2#
    Connection to device 192.168.100.3

    R3>enable
    Password:
    R3#terminal length 0
    R3#
    sh ip int br
    Interface                  IP-Address      OK? Method Status                Protocol
    FastEthernet0/0        192.168.100.3   YES NVRAM  up                    up
    FastEthernet0/1        unassigned      YES NVRAM  up                    up
    FastEthernet0/1.10     10.3.10.1       YES manual up                    up
    FastEthernet0/1.20     10.3.20.1       YES manual up                    up
    FastEthernet0/1.30     10.3.30.1       YES manual up                    up
    FastEthernet0/1.40     10.3.40.1       YES manual up                    up
    FastEthernet0/1.50     10.3.50.1       YES manual up                    up
    FastEthernet0/1.60     10.3.60.1       YES manual up                    up
    FastEthernet0/1.70     10.3.70.1       YES manual up                    up
    R3#

Please note that the output includes both entering enable password and terminal length command.

This is because paramiko collects the entire output to buffer. And when ``recv`` (for example, ``ssh.recv(1000)``), paramiko returns everything from buffer. After ``recv`` execution, buffer is empty.

Therefore, if you only need to get the output of *sh ip int br*, then you should leave ``recv``, but not apply *print*:

.. code:: python

        ssh.send('enable\n')
        ssh.send(ENABLE_PASS + '\n')
        time.sleep(1)

        ssh.send('terminal length 0\n')
        time.sleep(1)
        #Тут мы вызываем recv, но не выводим содержимое буфера
        ssh.recv(1000)

        ssh.send(COMMAND + '\n')
        time.sleep(3)
        result = ssh.recv(5000).decode('utf-8')
        print(result)

