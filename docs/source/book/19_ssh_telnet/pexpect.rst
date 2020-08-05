Module pexpect
--------------

Module pexpect allows to automate interactive connections such as:

* telnet 
* ssh 
* ftp

.. note::

    Pexpect is an implementation of *expect* in Python.

First, pexpect module needs to install:

::

    pip install pexpect


The logic of pexpect is:

* some program is running
* pexpect expects a certain output (invitation, password request, etc.) 
* after receiving the output, it sends commands/data
* last two actions are repeated as many as necessary

Pexpect has two main tools:

* function ``run()`` 
* class ``spawn``

``pexpect.run()``
~~~~~~~~~~~~~~~~~

Function  ``run()`` allows you to call a program and return its output.

For example:

.. code:: python

    In [1]: import pexpect

    In [2]: output = pexpect.run('ls -ls')

    In [3]: print(output)
    b'total 44\r\n4 -rw-r--r-- 1 vagrant vagrant 3203 Jul 14 07:15 1_pexpect.py\r\n4 -rw-r--r-- 1 vagrant vagrant 3393 Jul 14 07:15 2_telnetlib.py\r\n4 -rw-r--r-- 1 vagrant vagrant 3452 Jul 14 07:15 3_paramiko.py\r\n4 -rw-r--r-- 1 vagrant vagrant 3127 Jul 14 07:15 4_netmiko.py\r\n4 -rw-r--r-- 1 vagrant vagrant  718 Jul 14 07:15 4_netmiko_telnet.py\r\n4 -rw-r--r-- 1 vagrant vagrant  300 Jul  8 15:31 devices.yaml\r\n4 -rw-r--r-- 1 vagrant vagrant  413 Jul 14 07:15 netmiko_function.py\r\n4 -rw-r--r-- 1 vagrant vagrant  876 Jul 14 07:15 netmiko_multiprocessing.py\r\n4 -rw-r--r-- 1 vagrant vagrant 1147 Jul 14 07:15 netmiko_threading_data_list.py\r\n4 -rw-r--r-- 1 vagrant vagrant 1121 Jul 14 07:15 netmiko_threading_data.py\r\n4 -rw-r--r-- 1 vagrant vagrant  671 Jul 14 07:15 netmiko_threading.py\r\n'

    In [4]: print(output.decode('utf-8'))
    total 44
    4 -rw-r--r-- 1 vagrant vagrant 3203 Jul 14 07:15 1_pexpect.py
    4 -rw-r--r-- 1 vagrant vagrant 3393 Jul 14 07:15 2_telnetlib.py
    4 -rw-r--r-- 1 vagrant vagrant 3452 Jul 14 07:15 3_paramiko.py
    4 -rw-r--r-- 1 vagrant vagrant 3127 Jul 14 07:15 4_netmiko.py
    4 -rw-r--r-- 1 vagrant vagrant  718 Jul 14 07:15 4_netmiko_telnet.py
    4 -rw-r--r-- 1 vagrant vagrant  300 Jul  8 15:31 devices.yaml
    4 -rw-r--r-- 1 vagrant vagrant  413 Jul 14 07:15 netmiko_function.py
    4 -rw-r--r-- 1 vagrant vagrant  876 Jul 14 07:15 netmiko_multiprocessing.py
    4 -rw-r--r-- 1 vagrant vagrant 1147 Jul 14 07:15 netmiko_threading_data_list.py
    4 -rw-r--r-- 1 vagrant vagrant 1121 Jul 14 07:15 netmiko_threading_data.py
    4 -rw-r--r-- 1 vagrant vagrant  671 Jul 14 07:15 netmiko_threading.py

``pexpect.spawn``
~~~~~~~~~~~~~~~~~

Class ``spawn`` supports more features. It allows you to interact with the called program by sending data and waiting for a response.

For example, you can initiate SSH connecton:

.. code:: python

    In [5]: ssh = pexpect.spawn('ssh cisco@192.168.100.1')

After executing this line, the connection is established. Now you must specify which line to expect. In this case, wait for the password request:

.. code:: python

    In [6]: ssh.expect('[Pp]assword')
    Out[6]: 0

Note how the line that pexpect expects is described:
``[Pp]assword``. This is a regular expression that describes a *password* or *Password* string. That is, the expect() method can be used to pass a regular expression as an argument.

Method expect() returned number 0 as a result of the work. This number indicates that a match has been found and that this element with index zero. The index appears here because you can transfer a list of strings. For example, you can transfer a list with two elements:

.. code:: python

    In [7]: ssh = pexpect.spawn('ssh cisco@192.168.100.1')

    In [8]: ssh.expect(['password', 'Password'])
    Out[8]: 1

Note that it now returns 1. This means that *Password* word matched.

Now you can send the password using *sendline* command:

.. code:: python

    In [9]: ssh.sendline('cisco')
    Out[9]: 6

Command *sendline* sends a string, automatically adds a line feed character to it based on the value of os.linesep and then returns a number indicating how many bytes were written.

.. note::

    Pexpect has several options for sending commands, not just sendline.

To get into enable mode expect-sendline cycle repeats:

.. code:: python

    In [10]: ssh.expect('[>#]')
    Out[10]: 0

    In [11]: ssh.sendline('enable')
    Out[11]: 7

    In [12]: ssh.expect('[Pp]assword')
    Out[12]: 0

    In [13]: ssh.sendline('cisco')
    Out[13]: 6

    In [14]: ssh.expect('[>#]')
    Out[14]: 0

Now we can send a command:

.. code:: python

    In [15]: ssh.sendline('sh ip int br')
    Out[15]: 13

After sending the command, pexpect must be pointed till which moment it should read the output. We specify that it should read untill #:

.. code:: python

    In [16]: ssh.expect('#')
    Out[16]: 0

Command output is in *before* attribute:

.. code:: python

    In [17]: ssh.before
    Out[17]: b'sh ip int br\r\nInterface                  IP-Address      OK? Method Status                Protocol\r\nEthernet0/0                192.168.100.1   YES NVRAM  up                    up      \r\nEthernet0/1                192.168.200.1   YES NVRAM  up                    up      \r\nEthernet0/2                19.1.1.1        YES NVRAM  up                    up      \r\nEthernet0/3                192.168.230.1   YES NVRAM  up                    up      \r\nEthernet0/3.100            10.100.0.1      YES NVRAM  up                    up      \r\nEthernet0/3.200            10.200.0.1      YES NVRAM  up                    up      \r\nEthernet0/3.300            10.30.0.1       YES NVRAM  up                    up      \r\nR1'

Since the result is displayed as a sequence of bytes you should convert it to a string:

.. code:: python

    In [18]: show_output = ssh.before.decode('utf-8')

    In [19]: print(show_output)
    sh ip int br
    Interface                  IP-Address      OK? Method Status                Protocol
    Ethernet0/0                192.168.100.1   YES NVRAM  up                    up
    Ethernet0/1                192.168.200.1   YES NVRAM  up                    up
    Ethernet0/2                19.1.1.1        YES NVRAM  up                    up
    Ethernet0/3                192.168.230.1   YES NVRAM  up                    up
    Ethernet0/3.100            10.100.0.1      YES NVRAM  up                    up
    Ethernet0/3.200            10.200.0.1      YES NVRAM  up                    up
    Ethernet0/3.300            10.30.0.1       YES NVRAM  up                    up
    R1

The session ends with a close() call:

.. code:: python

    In [20]: ssh.close()

Special characters in shell
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Pexpect does not interpret special shell characters such as ``>``,
``|``, ``*``.

For example, in order make command ``ls -ls | grep SUMMARY`` work, shell must be run as follows:

.. code:: python

    In [1]: import pexpect

    In [2]: p = pexpect.spawn('/bin/bash -c "ls -ls | grep pexpect"')

    In [3]: p.expect(pexpect.EOF)
    Out[3]: 0

    In [4]: print(p.before)
    b'4 -rw-r--r-- 1 vagrant vagrant 3203 Jul 14 07:15 1_pexpect.py\r\n'

    In [5]: print(p.before.decode('utf-8'))
    4 -rw-r--r-- 1 vagrant vagrant 3203 Jul 14 07:15 1_pexpect.py

pexpect.EOF
~~~~~~~~~~~

In the previous example we met pexpect.EOF.

.. note::

    EOF — end of file

This is a special value that allows you to react to the end of a command or session that has been run in spawn.

When calling ``ls -ls`` command, pexpect does not receive an interactive session. Command is simply executed and that ends its work.

Therefore, if you run this command and set invitation in *expect*, there is an error:

.. code:: python

    In [5]: p = pexpect.spawn('/bin/bash -c "ls -ls | grep SUMMARY"')

    In [6]: p.expect('nattaur')
    ---------------------------------------------------------------------------
    EOF                                       Traceback (most recent call last)
    <ipython-input-9-9c71777698c2> in <module>()
    ----> 1 p.expect('nattaur')
    ...

If EOF passed to *expect*, there will be no error.

Method pexpect.expect
~~~~~~~~~~~~~~~~~~~~

In pexpect.expect as a template can be used:

* regular expression
* EOF - this template allows you to react to the EOF exception
* TIMEOUT - timeout exception (default timeout = 30 seconds)
* compiled re

Another very useful feature of pexpect.expect is that you can pass not one value, but a list.

For example:

.. code:: python

    In [7]: p = pexpect.spawn('/bin/bash -c "ls -ls | grep netmiko"')

    In [8]: p.expect(['py3_convert', pexpect.TIMEOUT, pexpect.EOF])
    Out[8]: 2

Here are some important points:

* when pexpect.expect is called with the list, you can specify different expected strings 
* apart strings, exceptions also can be specified
* pexpect.expect returns number of element that matched

  * in this case number 2 because the EOF exception is number two in the list  

* with this format you can make branches in the program depending on the element which had a match

Example of pexpect use
----------------------------

An example of using pexpect when connecting to equipment and passing show command (file 1_pexpect.py):

.. literalinclude:: /pyneng-examples-exercises/examples/19_ssh_telnet/1_pexpect.py
  :language: python
  :linenos:


Comments to the script:

* command to execute is passed as an argument
* then login, password and password for enable mode are requested

  * passwords are requested with getpass module

* ``ip_list`` - list of IP addresses of devices to which the connection will be established
* connection to devices from the list occurs in the loop
* In spawn class, SSH connection establishes to current address using specified user name 
* after that, the pairs of methods begin to alternate: expect and sendline

  * ``expect`` - waits for substring
  * ``sendline`` - when a line appears, a command is sent

* this happens until the end of the loop and only the last command is different:

  * ``before`` lets you read everything that caught by pexpect before the previous substring in *expect*

.. note::

    Note the line ``ssh.expect('[#>]')``. Method *expect* expects not just a string, but a regular expression.

The script execution is as follows:

::

    $ python 1_pexpect.py "sh ip int br"
    Username: nata
    Password:
    Enter enable secret:
    Connection to device 192.168.100.1
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
    R1
    Connection to device 192.168.100.2
    sh ip int br
    Interface              IP-Address      OK? Method Status                Protocol
    FastEthernet0/0        192.168.100.2   YES NVRAM  up                    up
    FastEthernet0/1        unassigned      YES NVRAM  up                    up
    FastEthernet0/1.10     10.2.10.1       YES manual up                    up
    FastEthernet0/1.20     10.2.20.1       YES manual up                    up
    FastEthernet0/1.30     10.2.30.1       YES manual up                    up
    FastEthernet0/1.40     10.2.40.1       YES manual up                    up
    FastEthernet0/1.50     10.2.50.1       YES manual up                    up
    FastEthernet0/1.60     10.2.60.1       YES manual up                    up
    FastEthernet0/1.70     10.2.70.1       YES manual up                    up
    R2
    Connection to device 192.168.100.3
    sh ip int br
    Interface              IP-Address      OK? Method Status                Protocol
    FastEthernet0/0        192.168.100.3   YES NVRAM  up                    up
    FastEthernet0/1        unassigned      YES NVRAM  up                    up
    FastEthernet0/1.10     10.3.10.1       YES manual up                    up
    FastEthernet0/1.20     10.3.20.1       YES manual up                    up
    FastEthernet0/1.30     10.3.30.1       YES manual up                    up
    FastEthernet0/1.40     10.3.40.1       YES manual up                    up
    FastEthernet0/1.50     10.3.50.1       YES manual up                    up
    FastEthernet0/1.60     10.3.60.1       YES manual up                    up
    FastEthernet0/1.70     10.3.70.1       YES manual up                    up
    R3

Note that since the last *expect* specifies that you should expect a substring ``#``, method *before* showed both the command and the host name.

