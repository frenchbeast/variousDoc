## Install JDownloader2 on Pine64

#### Install Java

```bash
# apt-get install -y openjdk-8-jdk
```

#### Install JDownloader2
```bash
# mkdir /usr/local/JDownloader2
# download the "Other" jar file
# ...
```

#### Create a startup script
```bash
# vi /etc/init.d/jdownloader
#! /bin/sh
# /etc/init.d/jdownloader
### BEGIN INIT INFO
# Provides: jDownloader2
# Required-Start: networking
# Required-Stop:
# Default-Start: 2 3 4 5
# Default-Stop: 0 1 6
# Short-Description: jDownloader2 Server Daemon
# Description: T_Send's Starts/Stops/Restarts/Status the jDownloader2 Server Daemon
### END INIT INFO
# Author: T_Send  Contact: t_send@itnu.pl  Web: www.itnu.pl

set -e
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
DESC="jDownloader2 Server"
NAME="jdownloader"
## USER for new files ownership & make sure USER has read/write permission to DOWNLOAD-DIR
USER=root
GROUP=root
UMASK=022
DIR="/usr/local/JDownloader2"
SCRIPT="/etc/init.d/$NAME"
PIDFILE="$DIR/JDownloader.pid"
JAVA="/usr/bin/java"
PARM="-Djava.awt.headless=true -jar $DIR/JDownloader.jar -guiless"
STATUS="$JAVA $PARM"

. /lib/lsb/init-functions

start_daemon () {
        start-stop-daemon --start \
            --chuid $USER \
                        --group $GROUP \
                        --umask $UMASK \
            --oknodo \
            --background \
            --make-pidfile --pidfile $PIDFILE \
            --exec $JAVA -- $PARM
        log_end_msg $?
}

case "$1" in
    start)
        log_daemon_msg "Starting daemon" "$DESC "
        start_daemon
        ;;
    stop)
        log_daemon_msg "Stopping daemon" "$DESC "
        start-stop-daemon --stop \
            --pidfile $PIDFILE
        log_end_msg $?
        ;;
    restart)
        log_daemon_msg "Restarting daemon" "$DESC "
        start-stop-daemon --stop
            log_end_msg $?
        start_daemon
        ;;
    status)
        status_of_proc "$STATUS" "$DESC " && exit 0 || exit $?
            ;;
    *)
        log_action_msg "Usage: $SCRIPT {start|stop|restart|status}" || true
        exit 2
        ;;
esac

exit 0
# /etc/init.d/jdownloader start
```