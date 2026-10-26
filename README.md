| __Quentin HARDY__    |
| ------------- |
| __quentin.hardy@protonmail.com__  |
| __quentin.hardy@bt.com__    |


ODAT 
====

__ODAT__ (Oracle Database Attacking Tool) is an open source __penetration testing__ tool that tests the security of __Oracle Databases remotely__.

Usage examples of ODAT:
* You have an Oracle database listening remotely and want to find valid __SIDs__ and __credentials__ in order to connect to the database
* You have a valid Oracle account on a database and want to __escalate your privileges__ to become DBA or SYSDBA
* You have a Oracle account and you want to __execute system commands__ (e.g. __reverse shell__) in order to move forward on the operating system hosting the database

Tested on Oracle Database __10g__,  __11g__,  __12c__, __18c__ and __19c__.

__ODAT linux standalone__ version at [https://github.com/quentinhardy/odat/releases/](https://github.com/quentinhardy/odat/releases/). Notice it is recommended to use the development version  (*git clone*), *master-python3* branch (python 3 version).

Changelog
====
* Version __5.1.1__ (__27/04/2022__):
  * print a warning message when an "ORA-12519 - TNS:no appropriate service handler found" occured. This error can be triggered by the database server (listener) when too many connections have been established (when bruteforcing credentials for example). In this case, __--sleep__ option should be used (e.g. *--sleep 0.2*).
* Version __5.1__ (__12/03/2021__):
  * new option in _all_ module: __--nmap-file__ for loading all Oracle databases from a nmap XML outpout file
  * new option in _all_ module: __-l__ for loading all targets from a text file (ip:port or ip on each line) 
  * In _all_ module, a TCP connection is tested on each target before to start ODAT tests now. __--timeout-tcp-check__ can be used to manage timeout value.
  * In _all_ module, a test is done on each target now for checking if the TNS listener is well configured. This test is done after TCP connection test.
  * new global option: __--client-driver__. Allows to set the client name for database connection (__client_driver__ from __V$SESSION_CONNECT_INFO__). 
    Useful for applications whose end-users are not aware python-oracledb is being used. By default, 'sql*plus' now.
  * ODAT accepts a __sqlnet.ora__ file now. A default file is in __conf/__. The timeout TCP & OUTBOUND (TNS) connection is set to 60 seconds by default now.  
* Version __5.0__ (__08/02/2021__):
  * Important new module: __Service Name Guesser__, for searching valid Service Names. Specific module named __*snguesser*__ and integrated in *all* module too. By default, *all* module searches all valid Service Names now after searching SIDs. SIDs can be identical to Service Name. When a Service Name is found but not a SID, Service Name can be used as a SID for connection.
  * __--basic-info__ option in _search_ module for getting some basic technical information about the database/instance. It gets Service Name, SID, databases, Oracle Database Vault status, Java status, hostname, ip address of the server, password policy, current system pivileges, current roles, pathches (when >= 12c and user is privileged), etc. Requires high pivileges for very interesting information (e.g. password policy & lock status).
  * Reverse shell implemented in _DbmsScheduler_ module when the target is Windows. Starts a minimal http server, makes the Oracle Database download the powershell code and executes it remotely. The user has an interactive PS shell. Downloaded file is removed automatically.
  * _--make-download_ option implemented in _DbmsScheduler_ in order to make download a Windows target a script/binary file.
  * Bug fix in output in _DbmsScheduler_ module (_printOSCmdOutput()_).
  * Better explanations in _DbmsScheduler_ module (options).
* Version __4.3__ (__28/06/2020__):
  * new option: _--both-ul_. Try each password in lower case and upper case if it is not done in credential file(s)
  * new option: _--random-order_. Test accounts in random order. By default, it uses the text file order
  * all passwords are tested in lower case now by default (account files modified from upper to lower case)
  * some new accounts
* Version __4.2__ (__04/02/2020__):
  * minimal interactive SQL shell in search module
  * new feature for downloading files with DBMS_XSLPROCESSOR.read2clob in DbmsXslprocessor module
  * improvement for having remote OS after authentication
  * bug fix (e.g. terminal size)
* Version __4.1__ (__01/15/2020__):
  * Option for connection with SERVICE NAME instead of SID
  * Encoding set to UTF-8 by default for some operations (e.g. NVARCHAR2 and NCLOB, VARCHAR2 and CLOB)
  * Multiple bug fixes (encoding problems)
* Version __4.0__ (__08/10/2019__):
  * Odat with python 3, stable version
* Version __3.0__ (__01/09/2019__):
  * Compatible with Oracle Database 18c
  * New option in Java module for setting the path to shell (e.g. /usr/bin/sh when CentOS)
* Version __2.3.2__ (__16/07/2019__):
  * Bug fixes:
    * external table and false negative
* Version __2.3.1__ (__10/08/2018__):
  * New option in Java module for exploiting CVE-2018-3004: It allows an authenticated user to arbitrarily write to files on the OS (Privilege escalation with bypass built in Oracle JVM security). More details [http://obtruse.syfrtext.com/2018/07/oracle-privilege-escalation-via.html](http://obtruse.syfrtext.com/2018/07/oracle-privilege-escalation-via.html). 
* Version __2.3__ (__06/03/2018__):
  * Bug fixes:
    * Some database credentials could not be detected in passwordguesser module (when it tries to connect as SYSDBA/SYSOPER before). Important bug fix!
    * PPoutput missing in smb module (thx to dirkjanm)
    * False positive in tnspoison module
    * Sometimes, impossible to get version from VSNNUM
  * Improvements:
    * Performence improvements when you bruteforce accounts.
    * Option for connection with a TNS Connection String
    * New trivial credentials in accounts file
    * Better read file method in dbmsLob module (thx to omair2084)
* Version __2.2.1__ (__12/04/2017__):
  * Bug fixes: 
    * Version in tnscmd ([info](https://github.com/quentinhardy/odat/commit/9b7ec57cfed0fe74381edff28b73532562acaa2c))
    * External module (Catch an Oracle error)
    * Catch error when "connection to server failed"
    * Tnscmd: "The server SID must be given with the '-d SID' option
    * TypeError Bug Fix in tnspoison module
    * TNS poisoning module when port!=1521 (Integer bug) 
  * Improvements:
    * New SIDs
* Version __2.2__ (__25/03/2016__):
  * A new module (_tnspoison_) for exploiting the TNS listener poisoning attack (CVE-2012-1675). Big thanks to [Joxean Koret](http://seclists.org/fulldisclosure/2012/Apr/204) and [donctl](https://github.com/donctl).
  * Some bug fixes
  * Better help menu
  * [__WIKI__ pages](https://github.com/quentinhardy/odat/wiki) in the ODAT repository
* Version __2.1__ (__04/03/2016__) :
  * A new module (_cve_) for exploiting some CVE (Common Vulnerabilities and Exposures). CVE-2012-3137 (perhaps this number, I'm not sure...) implemented at the moment: A user authenticated can modify all tables who can select even if he can't modify them normally (no ALTER privilege).
  * new option (__--accounts-files__) for remote authentication attack which uses 2 distinct files: a login list and password list.
  * Print 10g passwords for oclHashcat compatibility.
  * bug fixes (listening with __nc__).
* Version __2.0__ (__21/02/2016__) :
  * A new module (_privesc_) for using system privileges of an Oracle user (e.g. CREATE ANY PROCEDURE) in order to gain privileged access (i.e. DBA). System privileges that can be used by ODAT in this version:
    * CREATE ANY PROCEDURE: execution of arbitrary requests with APEX_040200's privileges (e.g. modification of Oracle users' passwords)
    * CREATE PROCEDURE and EXECUTE ANY PROCEDURE: execution of arbitrary requests as SYS (e.g. gives DBA role to a user)
    * CREATE ANY TRIGER (and CREATE PROCEDURE): execution of arbitrary requests as SYS (e.g. gives DBA role to a user)
    * ANALYZE ANY (and CREATE PROCEDURE): execution of arbitrary requests as SYS (e.g. gives DBA role to a user)
    * CREATE ANY INDEX (and CREATE PROCEDURE): execution of arbitrary requests as SYS (e.g. gives DBA role to a user)
  * The module _privesc_ can be used to get all system privileges and roles granted. It shows system privileges that can be used to gain privileged access.
  * new option (-vvv) for showing SQL requests sent by ODAT in debugs
  * standalone version moved to *releases* ([https://github.com/quentinhardy/odat/releases/](https://github.com/quentinhardy/odat/releases/))
* Version __1.6__ (__14/07/2015__) :
  * new feature to detect if a target is vulnerable to TNS poisoning (CVE-2012-1675)
  * new module named *unwrapper* to unwrap PL/SQL source code wrapped, from a file or a remote database
  * some improvements done
* Version __1.5__ (__17/03/2015__) :
  * new module named *search* in order to search in column names
  * some improvements done (ex: output of tables)
  * new option : output encoding
* Version __1.4__ (__07/12/2014__) :
  * fix some false positives
  * improve the CVE-2012-3137 module: check more easily if the vulnerability can be exploited
* Version __1.3__ (__07/10/2014__) : 
  * add the *-C* option in the *all* module. This module can be used to use file which contains credentials (disable the *-U* and *-P* option)
  * add the *tnscmd* module to get TNS *alias*, database *version* (thanks to VSNNUM) and TNS *status*
  * bug fix: name server can be given to the *-s* option
* Version __1.2__ (__08/08/2014__) : 
  * add the *SMB* module to capture a SMB authentication
  * add an option (*SHOW_SQL_REQUESTS_IN_VERBOSE_MODE*) in *Constants.py* to show SQL requests sent to the database server
* Version __1.1__ (__28/07/2014__) : 
  * add the *DBMS_LOB* module useful in order to download files stored on a remote server through Oracle Database.
  * bug fix: java source code: "getenv no longer supported, use properties and -D instead"
* Version __1.0__ (__26/06/2014__) : 
  * first ODAT version.

Features
====

Thanks to ODAT, you can:

* search __valid SID__ on a remote Oracle Database listener via:
  * a dictionary attack
  * a brute force attack
  * ALIAS of the listener
* search Oracle __accounts__ using:
  * a dictionary attack
  * each Oracle user like the password (need an account before to use this attack)
* __execute system commands__ on the database server using:
  * DBMS_SCHEDULER
  * JAVA
  * external tables
  * oradbg
* __download files__ stored on the database server using:
  * UTL_FILE
  * DBMS_XSLPROCESSOR
  * external tables
  * CTXSYS
  * DBMS_LOB
* __upload files__ on the database server using:
  * UTL_FILE
  * DBMS_XSLPROCESSOR
  * DBMS_ADVISOR
* __delete files__ using:
  * UTL_FILE
* __gain privileged access__ using these following system privileges combinations (see help for *privesc* module commands):
  * CREATE ANY PROCEDURE
  * CREATE PROCEDURE and EXECUTE ANY PROCEDURE
  * CREATE ANY TRIGER (and CREATE PROCEDURE)
  * ANALYZE ANY (and CREATE PROCEDURE)
  * CREATE ANY INDEX (and CREATE PROCEDURE)
* __send/reveive HTTP requests__ from the database server using:
  * UTL_HTTP
  * HttpUriType
* __scan ports__ of the local server or a remote server using:
  * UTL_HTTP
  * HttpUriType
  * UTL_TCP
* __capture a SMB authentication__ through:
  * an index in order trigger a SMB connection
* exploit some CVE: 
  * the [__CVE-2012-3137__](http://cvedetails.com/cve/2012-3137)
      * pickup the session key and salt for arbitrary users
      * attack by dictionary on sessions
  * the [__CVE-2012-????__](https://twitter.com/gokhanatil/status/595853921479991297): A user authenticated can modify all tables who can select even if he can't modify them normally (no ALTER privilege). 
  * the [__CVE-2012-1675__](http://seclists.org/fulldisclosure/2012/Apr/204) (aka TNS poisoning attack)
* __search in column names__ thanks to the *search* module:
  * search a pattern (ex: password) in column names
* __unwrap__ PL/SQL source code (10g/11g and 12c)
* get __system privileges__ and __roles granted__. It is possible to get privileges and roles of roles granted also
* execute arbitrary SELECT requests (aka minimal sql shell)

This list is not exhaustive.

Mind map - ODAT & Oracle Database pentests
====

![Alt text](./pictures/odat_mind_map_v1.0.jpg)

Supported Platforms and dependencies
====

ODAT runs on __Linux__ and __macOS__, including __arm64__ (Apple Silicon, arm64 Kali), because it uses
__python-oracledb__ in __thin mode__: a pure Python driver that does __not__ require the Oracle Instant Client.

__Thick mode__ (Oracle Instant Client) is still available with the global __--thick__ option. It is required
only for:
* servers that require __Native Network Encryption__ or data integrity (`SQLNET.ENCRYPTION_SERVER=REQUIRED` / `SQLNET.CRYPTO_CHECKSUM_SERVER=REQUIRED`),
* databases __older than 12.1__,
* accounts with a __10G password verifier only__.

Use __thincheck.py__ (see below) to know if thin mode can be used against a target before running ODAT.

Dependencies (Python 3):
* __oracledb__ (replaces the deprecated cx_Oracle)
* passlib
* pycryptodome
* scapy
* python-libnmap
* colorlog (recommended)
* termcolor (recommended)
* argcomplete (recommended)
* openpyxl (for the search module xlsx export)
* PySocks (recommended, for the __--socks__ option)

Installation
====

Thin mode needs no Oracle Instant Client, so a clone and a `pip install` are enough on Linux and macOS,
including arm64.

* Clone the repository to get the ODAT source code:
```bash
git clone https://github.com/quentinhardy/odat.git
cd odat/
```

* Update wiki pages in this repository for getting the ODAT documentation locally:
```bash
git submodule init
git submodule update
```

* Install the Python dependencies, in a virtual environment (recommended):
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

* Run ODAT:
```bash
./odat.py -h
```

* Optionally, enable argcomplete tab completion:
```bash
activate-global-python-argcomplete
```

Thick mode (optional, Oracle Instant Client)
====

Only needed for Native Network Encryption, databases older than 12.1 or 10G password verifiers (see above).
Install the Oracle Instant Client __Basic__ package for your platform
([Linux x86-64](https://www.oracle.com/database/technologies/instant-client/linux-x86-64-downloads.html),
[Linux ARM64](https://www.oracle.com/database/technologies/instant-client/linux-arm-aarch64-downloads.html),
[macOS](https://www.oracle.com/database/technologies/instant-client/macos-intel-x86-downloads.html)),
make it reachable (e.g. `ldconfig` on Linux or `LD_LIBRARY_PATH`/`DYLD_LIBRARY_PATH`), then add __--thick__:
```bash
./odat.py all -s 10.0.0.5 -p 1521 --thick
```

Checking if thin mode can be used (thincheck.py)
====

```bash
./thincheck.py -s 10.0.0.5 -p 1521 -n ORCLPDB1
```

It connects with a random non-existent username (which can not lock a real account):
* __ORA-01017__: thin mode can be used (no Native Network Encryption required, database >= 12.1).
* __DPY-3001__: Native Network Encryption or data integrity is required, use __--thick__.
* __DPY-3010__: database older than 12.1, use __--thick__.

A single credential can be tested with __-U__ (and __-P__), which also prints the database version, the
network services, the roles and the system privileges of the account:
```bash
./thincheck.py -s 10.0.0.5 -p 1521 -n ORCLPDB1 -U scott
```

Going through a SOCKS proxy (--socks)
====

Database connections and TNS listener commands can go through a SOCKS proxy, for example an SSH dynamic
port forward (`ssh -D 1080 jumphost`). Thin mode only (thick mode uses its own sockets: use proxychains).
```bash
./odat.py all -s db.internal -p 1521 --socks socks5h://127.0.0.1:1080
```
With __socks5h://__ (or __socks4a://__), host names are resolved by the proxy, so internal host names that
only resolve on the remote network can be used.

Docs and examples
====

Docs and examples are on the [__WIKI__](https://github.com/quentinhardy/odat/wiki) 


Donation
====
If you want to support my work doing a donation, I will appreciate a lot:

* Via BTC: 36FugL6SnFrFfbVXRPcJATK9GsXEY6mJbf
