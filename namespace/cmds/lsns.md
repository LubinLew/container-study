# lsns 命令



NAME
       lsns - list namespaces

SYNOPSIS
       lsns [options] [namespace]



DESCRIPTION

lsns lists information about all the currently accessible namespaces or about the given namespace.  
The namespace identifier is an inode number.

The  default  output is subject to change.
So whenever possible, you should avoid using default outputs in your scripts.
Always explicitly define expected
columns by using the --output option together with a columns list in environments where a stable output is required.

NSFS column, printed when net is specified for --type option, is special; it uses multi-line cells.
Use the option --nowrap is for switching to "," separated single-line representation.

Note that lsns reads information directly from the /proc filesystem and for non-root users it may return incomplete information.
The current /proc filesystem may be unshared and affected by a PID namespace (see unshare --mount-proc for more details). 
lsns is not able to see persistent namespaces without  processes where the namespace instance is held by a bind mount to /proc/pid/ns/type.

OPTIONS
       -J, --json
              Use JSON output format.

       -l, --list
              Use list output format.

       -n, --noheadings
              Do not print a header line.

       -o, --output list
              Specify which output columns to print.  Use --help to get a list of all supported columns.

              The default list of columns may be extended if list is specified in the format +list (e.g. lsns -o +PATH).

       --output-all
              Output all available columns.

       -p, --task pid
              Display only the namespaces held by the process with this pid.

       -r, --raw
              Use the raw output format.

       -t, --type type
              Display  the  specified type of namespaces only.  The supported types are mnt, net, ipc, user, pid, uts and cgroup.  This option may be given more than
              once.

       -u, --notruncate
              Do not truncate text in columns.

       -W, --nowrap
              Do not use multi-line text in columns.

       -V, --version
              Display version information and exit.

       -h, --help
              Display help text and exit.

AUTHORS
       Karel Zak <kzak@redhat.com>

SEE ALSO
       nsenter(1), unshare(1), clone(2), namespaces(7)


The lsns command is part of the util-linux package and is available from https://www.kernel.org/pub/linux/utils/util-linux/.

> https://man7.org/linux/man-pages/man8/lsns.8.html
>
> https://en.wikipedia.org/wiki/Linux_namespaces