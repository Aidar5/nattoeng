Module telnetlib
----------------

Module telnetlib is part of standard Python library. This is the telnet client implementation.

.. note::

    It is also possible to connect via telnet using pexpect. Plus of telnetlib is that this module is part of standard Python library.
    
Telnetlib resembles pexpect, but has several differences. The most notable difference is that telnetlib requires the transfer of a byte string, rather than normal one.

The connection is performed as follows:

.. code:: python

    In [1]: telnet = telnetlib.Telnet('192.168.100.1')

Method read_until specifies till which line the output should be read. However, as an argument, it is necessary to pass bytes, not the usual string:

.. code:: python

    In [2]: telnet.read_until(b'Username')
    Out[2]: b'\r\n\r\nUser Access Verification\r\n\r\nUsername'

Method read_until returns everything it has read before the specified string.

Method write() is used for data transmission. Byte string has to be passed to it:

.. code:: python

    In [3]: telnet.write(b'cisco\n')

Read output till *Password* and pass the password:

.. code:: python

    In [4]: telnet.read_until(b'Password')
    Out[4]: b': cisco\r\nPassword'

    In [5]: telnet.write(b'cisco\n')

You can now specify what should be read untill invitation and then send the command:

.. code:: python

    In [6]: telnet.read_until(b'>')
    Out[6]: b': \r\nR1>'

    In [7]: telnet.write(b'sh ip int br\n')

After sending a command, you can continue to use read_until() method:

.. code:: python

    In [8]: telnet.read_until(b'>')
    Out[8]: b'sh ip int br\r\nInterface                  IP-Address      OK? Method Status                Protocol\r\nEthernet0/0                192.168.100.1   YES NVRAM  up                    up      \r\nEthernet0/1                192.168.200.1   YES NVRAM  up                    up      \r\nEthernet0/2                19.1.1.1        YES NVRAM  up                    up      \r\nEthernet0/3                192.168.230.1   YES NVRAM  up                    up      \r\nEthernet0/3.100            10.100.0.1      YES NVRAM  up                    up      \r\nEthernet0/3.200            10.200.0.1      YES NVRAM  up                    up      \r\nEthernet0/3.300            10.30.0.1       YES NVRAM  up                    up      \r\nR1>'

Or use another read method read_very_eager(). When using read_very_eager() method, you can send multiple commands and then read all available output:

.. code:: python

    In [9]: telnet.write(b'sh arp\n')

    In [10]: telnet.write(b'sh clock\n')

    In [11]: telnet.write(b'sh ip int br\n')

    In [12]: all_result = telnet.read_very_eager().decode('utf-8')

    In [13]: print(all_result)
    sh arp
    Protocol  Address          Age (min)  Hardware Addr   Type   Interface
    Internet  10.30.0.1               -   aabb.cc00.6530  ARPA   Ethernet0/3.300
    Internet  10.100.0.1              -   aabb.cc00.6530  ARPA   Ethernet0/3.100
    Internet  10.200.0.1              -   aabb.cc00.6530  ARPA   Ethernet0/3.200
    Internet  19.1.1.1                -   aabb.cc00.6520  ARPA   Ethernet0/2
    Internet  192.168.100.1           -   aabb.cc00.6500  ARPA   Ethernet0/0
    Internet  192.168.100.2         124   aabb.cc00.6600  ARPA   Ethernet0/0
    Internet  192.168.100.3         143   aabb.cc00.6700  ARPA   Ethernet0/0
    Internet  192.168.100.100       160   aabb.cc80.c900  ARPA   Ethernet0/0
    Internet  192.168.200.1           -   0203.e800.6510  ARPA   Ethernet0/1
    Internet  192.168.200.100        13   0800.27ac.16db  ARPA   Ethernet0/1
    Internet  192.168.230.1           -   aabb.cc00.6530  ARPA   Ethernet0/3
    R1>sh clock
    *19:18:57.980 UTC Fri Nov 3 2017
    R1>sh ip int br
    Interface                  IP-Address      OK? Method Status                Protocol
    Ethernet0/0                192.168.100.1   YES NVRAM  up                    up
    Ethernet0/1                192.168.200.1   YES NVRAM  up                    up
    Ethernet0/2                19.1.1.1        YES NVRAM  up                    up
    Ethernet0/3                192.168.230.1   YES NVRAM  up                    up
    Ethernet0/3.100            10.100.0.1      YES NVRAM  up                    up
    Ethernet0/3.200            10.200.0.1      YES NVRAM  up                    up
    Ethernet0/3.300            10.30.0.1       YES NVRAM  up                    up
    R1>

.. warning::

    You should always set time.sleep(n) before using read_very_eager.

With read_until() will be a slightly different approach. You can execute the same three commands, but then get the output one by one because of reading till invitation string:

.. code:: python

    In [14]: telnet.write(b'sh arp\n')

    In [15]: telnet.write(b'sh clock\n')

    In [16]: telnet.write(b'sh ip int br\n')

    In [17]: telnet.read_until(b'>')
    Out[17]: b'sh arp\r\nProtocol  Address          Age (min)  Hardware Addr   Type   Interface\r\nInternet  10.30.0.1               -   aabb.cc00.6530  ARPA   Ethernet0/3.300\r\nInternet  10.100.0.1              -   aabb.cc00.6530  ARPA   Ethernet0/3.100\r\nInternet  10.200.0.1              -   aabb.cc00.6530  ARPA   Ethernet0/3.200\r\nInternet  19.1.1.1                -   aabb.cc00.6520  ARPA   Ethernet0/2\r\nInternet  192.168.100.1           -   aabb.cc00.6500  ARPA   Ethernet0/0\r\nInternet  192.168.100.2         126   aabb.cc00.6600  ARPA   Ethernet0/0\r\nInternet  192.168.100.3         145   aabb.cc00.6700  ARPA   Ethernet0/0\r\nInternet  192.168.100.100       162   aabb.cc80.c900  ARPA   Ethernet0/0\r\nInternet  192.168.200.1           -   0203.e800.6510  ARPA   Ethernet0/1\r\nInternet  192.168.200.100        15   0800.27ac.16db  ARPA   Ethernet0/1\r\nInternet  192.168.230.1           -   aabb.cc00.6530  ARPA   Ethernet0/3\r\nR1>'

    In [18]: telnet.read_until(b'>')
    Out[18]: b'sh clock\r\n*19:20:39.388 UTC Fri Nov 3 2017\r\nR1>'

    In [19]: telnet.read_until(b'>')
    Out[19]: b'sh ip int br\r\nInterface                  IP-Address      OK? Method Status                Protocol\r\nEthernet0/0                192.168.100.1   YES NVRAM  up                    up      \r\nEthernet0/1                192.168.200.1   YES NVRAM  up                    up      \r\nEthernet0/2                19.1.1.1        YES NVRAM  up                    up      \r\nEthernet0/3                192.168.230.1   YES NVRAM  up                    up      \r\nEthernet0/3.100            10.100.0.1      YES NVRAM  up                    up      \r\nEthernet0/3.200            10.200.0.1      YES NVRAM  up                    up      \r\nEthernet0/3.300            10.30.0.1       YES NVRAM  up                    up      \r\nR1>'

An important difference between read_until() and read_very_eager() is how they react to the lack of output.

Method read_until() waits for a certain string. By default, if it does not exist, method will "freeze". Timeout option allows you to specify how long to wait for the desired string:

.. code:: python

    In [20]: telnet.read_until(b'>', timeout=5)
    Out[20]: b''

If no string appears during the specified time, an empty string is returned.

Method read_very_eager() simply returns an empty string if there is no output:

.. code:: python

    In [21]: telnet.read_very_eager()
    Out[21]: b''

Method expect() allows you to specify a list with regular expressions. It works like pexpect but telnetlib always has to pass a list of regular expressions.

You can then transfer byte strings or compiled regular expressions:

.. code:: python

    In [22]: telnet.write(b'sh clock\n')

    In [23]: telnet.expect([b'[>#]'])
    Out[23]:
    (0,
     <_sre.SRE_Match object; span=(46, 47), match=b'>'>,
     b'sh clock\r\n*19:35:10.984 UTC Fri Nov 3 2017\r\nR1>')

Method expect() returns the tuple of their three elements:

* index of matched expression 
* object Match 
* byte string that contains everything read till regular expression including regular expression

Accordingly, if necessary you can continue working with these elements:

.. code:: python

    In [24]: telnet.write(b'sh clock\n')

    In [25]: regex_idx, match, output = telnet.expect([b'[>#]'])

    In [26]: regex_idx
    Out[26]: 0

    In [27]: match.group()
    Out[27]: b'>'

    In [28]: match
    Out[28]: <_sre.SRE_Match object; span=(46, 47), match=b'>'>

    In [29]: match.group()
    Out[29]: b'>'

    In [30]: output
    Out[30]: b'sh clock\r\n*19:37:21.577 UTC Fri Nov 3 2017\r\nR1>'

    In [31]: output.decode('utf-8')
    Out[31]: 'sh clock\r\n*19:37:21.577 UTC Fri Nov 3 2017\r\nR1>'

Method close() closes connection:

.. code:: python

    In [32]: telnet.close()

Telnetlib usage example
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Working principle of telnetlib resembles pexpect, so the example below should be clear.

File 2_telnetlib.py:

.. literalinclude:: /pyneng-examples-exercises/examples/19_ssh_telnet/2_telnetlib.py
  :language: python
  :linenos:

telnetlib is very similar to pexpect:

  * ``with telnetlib.Telnet(ip) as t`` - class Telnet represents connection to server. 
  * in this case only the IP address is passed but it is also possible to pass a connection port 
  * ``read_until`` - similar to ``expect`` in pexpect module. 
    Specifies till which string the output should be read.
  * ``write`` - pass a string
  * ``read_very_eager`` - read everything that comes

.. note::

    Use of Telnet object as context manager is added in version 3.6

Execution of the script:

::

    $ python 2_telnetlib.py "sh ip int br"
    Username: cisco
    Password:
    Enter enable secret:
    Connection to device 192.168.100.1

    R1#terminal length 0
    R1#sh ip int br
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

    R2#terminal length 0
    R2#sh ip int br
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
    R2#
    Connection to device 192.168.100.3

    R3#terminal length 0
    R3#sh ip int br
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
    R3#

