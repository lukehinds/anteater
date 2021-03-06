===============
Virus Total API
===============

API Key
-------

In order to use the VirusTotal API, you will first require an API key. These are free to get
and can be obtained by signing up to the service `here <https://www.virustotal.com/#/join-us>`_. 

Once you have your key, it needs to be set as an environment variable.

If you're using CI, then see refer to the relevant CI document section in 
these docs for examples of how to achieve this.

If either `--ips`, ``--urls`` or ``--bincheck`` are called as arguments (in 
any combination including all three at once), then the VirusTotal API will be
queried for information on the following:

Public IP Addresses
-------------------

If `--ips` is passed as arguments, anteater will perform a scan for 
public / external IP Addresses. Once an address is found, the IP is sent to 
the Virus Total API and if the IP Address has past assocations with malicous 
or malware hosting domains, a failure is registered and a report is provided.

An example report can be seen `here <https://www.virustotal.com/#/ip-address/90.156.201.27>`_.

If you wish to whitelist an IP address, make an entry into your `ignore_list` or project
specific ignore_list:

    ip_ignore:
      - '173.217.16.206'
      - '92.47.16.221'

URLs
----

If ``--urls`` is passed as arguments, anteater will perform a scan for URL's. 
If an URL is found, the URL is sent to the Virus Total API which then 
compares the URL to a large list of URL blacklisting services.

An example report can be seen `here <https://www.virustotal.com/#/url/fb69ecad84eb86b1afddcca17aec38daea196e7c883b22ff88a7c39fd8fbdf1a/detection>`_.

If you wish to whitelist an IP address, make an entry into your `ignore_list` or project
specific ignore_list:

    url_ignore:
      - 'http://www.apache.org'
      - 'https://github.com'

Binaries
--------

If ``--bincheck`` is passed as arguments, anteater will send a hash of the 
binary to the Virus Total API which then compares the binary to an aggregation 
of Virus Scanner results. If no existing report is available, anteater will 
send the complete binary file to Virus Total for a new scan.

If you wish to whitelist an binary, make an entry into your `ignore_list` or project
specific ignore_list:

binaries:
  path/to/example.png:
    - 609feaed93afbea14c6b10c6effc986f39d1deb0a372ac088129bb22bbca8834

* Note: The sha256 checksum showed above, will be outputed in anteaters logs when it finds a binary.

Rate limit
----------

Use of the public Virus Total API requires a rate limit of no more than three
requests per minute, unless you have use of a private API account.

Public or Private can be set within the `anteater.conf` file, and anteater
will then use the appropriate rate limit:

``vt_rate_type = public``

The values are ``public`` for the public API, and ``private`` for the private
API.

Redis is requried for rate litmiting as means to track global rate requests.

All that is required for the Redis set up, is the installation of Redis and 
running redis with its default values.

The Dockerfile will deploy redis for you. Refer to `installation`_ for more
details.

