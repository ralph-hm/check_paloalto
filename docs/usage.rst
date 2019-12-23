=====
Usage
=====

Command-line usage::

    usage: check_paloalto [-h] -H HOST -T TOKEN [-v] [-t TIMEOUT] [--reset]
                      [--version]
                      {diskspace,certificates,load,useragent,environmental,sessinfo,thermal,throughput}
                      ...

    positional arguments:
      {diskspace,certificates,load,useragent,environmental,sessinfo,thermal,throughput}
        diskspace           check used diskspace.
        certificates        check the certificate store for expiring certificates:
                            Outputs is a warning, if a certificate is in range.
        load                check the CPU load.
        useragent           check for running useragents.
        environmental       check if an alarm is found.
        sessinfo            check important session parameters.
        thermal             check the temperature.
        throughput          check the throughput.

    optional arguments:
      -h, --help            show this help message and exit

    Connection:
      -H HOST, --host HOST  PaloAlto Server Hostname
      -T TOKEN, --token TOKEN
                            Generated Token for REST-API access

    Debug:
      -v, --verbose         increase output verbosity (use up to 3 times)
      -t TIMEOUT, --timeout TIMEOUT
                            abort check execution after so many seconds (use 0 for
                            no timeout)
      --reset               Deletes the cookie file for the throughput check.

    Info:
      --version             show program's version number and exit


To check your Palo Alto Firewall, there are several commands available.

diskspace
---------
usage::

    usage: check_paloalto diskspace [-h] [-w WARN] [-c CRIT]

    optional arguments:
      -h, --help            show this help message and exit
      -w WARN, --warn WARN  Warning if diskspace is greater. (default: 85)
      -c CRIT, --crit CRIT  Critical if disksace is greater. (default: 95)

example::

    $ check_paloalto -H HOST -T TOKEN diskspace
    $ DISKSPACE OK - sda2: 70%, sda5: 51%, sda6: 58%, sda8: 78% | sda2=70%;85;95 sda5=51%;85;95 sda6=58%;85;95 sda8=78%;85;95

certificates
------------
usage::

    usage: check_paloalto certificates [-h] [-ex EXCLUDE] [-r RANGE]

    optional arguments:
      -h, --help            show this help message and exit
      -ex EXCLUDE, --exclude EXCLUDE
                            Exclude certificates from check by name.
      -r RANGE, --range RANGE
                            Warning if days until certificate expiration is in
                            range: Represents a threshold range. The general
                            format is "[@][start:][end] (default: 0:20)

example::

    $ check_paloalto -H HOST -T TOKEN certificates
    $ CERTIFICATE WARNING - Certificate1 expires in 8 days

load
----
usage::

    usage: check_paloalto load [-h] [-w WARN] [-c CRIT]

    optional arguments:
      -h, --help            show this help message and exit
      -w WARN, --warn WARN  Warning if CPU load is greater. (default: 85)
      -c CRIT, --crit CRIT  Critical if CPU load is greater. (default: 95)

example::

    $ check_paloalto -H HOST -T TOKEN load
    $ LOAD OK - CPU0: 0.0%, CPU1: 1.0%, CPU2: 4.0%, CPU3: 5.0%, CPU4: 6.0%, CPU5: 5.0% | CPU0=0.0%;85;95;0;100 CPU1=1.0%;85;95;0;100 CPU2=4.0%;85;95;0;100 CPU3=5.0%;85;95;0;100 CPU4=6.0%;85;95;0;100 CPU5=5.0%;85;95;0;100

environmental
-------------
usage::

    usage: check_paloalto environmental [-h]

    optional arguments:
      -h, --help  show this help message and exit

example::

    $ check_paloalto -H HOST -T TOKEN environmental
    $ ENVIRONMENTAL OK - No alarms found.


sessinfo
--------
usage::

    usage: check_paloalto sessinfo [-h]

    optional arguments:
      -h, --help  show this help message and exit

example::

    $ check_paloalto -H HOST -T TOKEN sessinfo
    $ SESSINFO OK - Active sessions: 6582 / Throughput (kbps): 24304 | session=6582;20000;50000;0;262142 throughput_kbps=24304;;;0


thermal
-------
usage::

    usage: check_paloalto thermal [-h] [-w WARN] [-c CRIT]

    optional arguments:
      -h, --help            show this help message and exit
      -w WARN, --warn WARN  Warning if temperature is greater. (default: 40)
      -c CRIT, --crit CRIT  Critical if temperature is greater. (default: 45)

example::

    $ check_paloalto -H HOST -T TOKEN thermal
    $ THERMAL OK - Temperature @ Ocelot is 29 degrees Celsius, Temperature @ Switch is 33 degrees Celsius, Temperature @ Cavium is 36 degrees Celsius, Temperature @ Intel PHY is 24 degrees Celsius | 'Temperature @ Cavium'=36.5;40;45;5.0;60.0 'Temperature @ Intel PHY'=24.2;40;45;5.0;60.0 'Temperature @ Ocelot'=29.9;40;45;5.0;60.0 'Temperature @ Switch'=33.8;40;45;5.0;60.0

throughput
----------
usage::

    usage: check_paloalto throughput [-h] -i [INTERFACE]

    optional arguments:
      -h, --help            show this help message and exit
      -i [INTERFACE], --interface [INTERFACE]
                            PA interface name, seperate by comma.
                            if interface name is 'all', every interface throughput will be requested

example::

    $ check_paloalto -H HOST -T TOKEN throughput -i ethernet1/1
    $ THROUGHPUT OK - Input is 5.74 Mb/s - Output is 11.81 Mb/s | 'in_bps_ethernet1/1'=5743432.0;;;0 'out_bps_ethernet1/1'=11807524.0;;;0

    $ check_paloalto -H HOST -T TOKEN throughput -i ethernet1/1,ethernet1/2
    $ THROUGHPUT OK - Input is 44.12 Mb/s - Output is 24.59 Mb/s | 'in_bps_ethernet1/1'=5895616.0;;;0 'in_bps_ethernet1/2'=38225768.0;;;0 'out_bps_ethernet1/1'=15926620.0;;;0 'out_bps_ethernet1/2'=8661100.0;;;0

    $ check_paloalto -H HOST -T TOKEN throughput -i all
    $ THROUGHPUT OK - Input is 44.12 Mb/s - Output is 24.59 Mb/s | 'in_bps_ethernet1/1'=5895616.0;;;0 'in_bps_ethernet1/2'=38225768.0;;;0 'out_bps_ethernet1/1'=15926620.0;;;0 'out_bps_ethernet1/2'=8661100.0;;;0

To get all available names of your interfaces, please have a look at
https://www.paloaltonetworks.com/documentation/61/pan-os/pan-os/getting-started/configure-interfaces-and-zones.html

useragents
----------
usage::

    usage: check_paloalto useragent [-h] [-w WARN] [-c CRIT]

    optional arguments:
      -h, --help            show this help message and exit
      -w WARN, --warn WARN  Warning if agent is not responding for a given amount
                            of seconds. (default: 60)
      -c CRIT, --crit CRIT  Critical if agent is not responding for a given amount
                            of seconds. (default: 240)


example::

    $ check_paloalto -H HOST -T TOKEN useragent
    $ USERAGENT OK - All agents are connected and responding. | 'Agent: Agent1 - HOST1(vsys: vsys1) Host: 192.168.1.1(192.168.1.1):5007'=1;60;240
