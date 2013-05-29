# Hack Rsyslog
## Usage
1. Unpack rsyslog-7.2.7.tar.gz
2. copy omfwd.c to rsyslog-7.2.7/tools,
    copy rsconf.c to rsyslog-7.2.7/runtime
3. Install prerequisites
    * libestr libee can be found at rsyslog website,
      _remember to follow the installation instruction on the website_
    * use apt-get to install libjson0 libjson0-dev uuid-dev zlib1g-dev
4. Compile and install rsyslog
    1. `#./configure --prefix=/usr --libdir=/usr/sbin/lib`
    2. `#make`
    3. `#make install`
    4. `#service rsyslog restart` (If rsyslogd is already running)

## To Run rsyslogd in Debug Mode
   `#rsyslogd -dn > logfile`
    press ctrl-C when have sufficient data
## Modifications
### omfwd.c
Added code to send raw ethernet packet (line 697~779)
and commented out original sending function (line 791~793).
`setuid(0)` on line 711 just ensures we have root privilege.
The message to send is stored in variable _psz_ by rsyslog.
Log source, level and other information are stored in _psz_ in plain text.

### rsconf.c
rsyslog drops root privilege during initialization by calling `setuid()` on line 536.
We commented that line out to keep our root privilege.
