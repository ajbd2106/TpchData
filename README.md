tpch-kit
TPCh Data Generation

# TPC-H
1. cd dbgen && cp makefile.suite makefile

Modifications
The following modifications have been added on top of the official TPC-H kit:

modify dbgen to not print trailing delimiter
add option for dbgen to output to stdout
add compile support for macOS
add define for PostgreSQL to support LIMIT N for qgen
adjust Makefile defaults

Linux
Make sure the required development tools are installed:

Ubuntu:

sudo apt-get install git make gcc
CentOS/RHEL:

sudo yum install git make gcc
Then run the following commands to clone the repo and build the tools:

git clone https://github.com/gregrahn/tpch-kit.git
cd tpch-kit/dbgen
make MACHINE=LINUX DATABASE=POSTGRESQL
macOS
Make sure the required development tools are installed:

xcode-select --install
Then run the following commands to clone the repo and build the tools:

--
`vi makefile`

```
################
## CHANGE NAME OF ANSI COMPILER HERE
################
CC      = gcc
# Current values for DATABASE are: INFORMIX, DB2, TDAT (Teradata)
#                                  SQLSERVER, SYBASE, ORACLE, VECTORWISE
# Current values for MACHINE are:  ATT, DOS, HP, IBM, ICL, MVS,
#                                  SGI, SUN, U2200, VMS, LINUX, WIN32
# Current values for WORKLOAD are:  TPCH
DATABASE= HIVE
MACHINE = LINUX
WORKLOAD = TPCH
```
------------

`vi tpcd.h`

```
#ifdef HIVE
#define GEN_QUERY_PLAN "EXPLAIN"
#define START_TRAN "START TRANSACTION"
#define END_TRAN "COMMIT"
#define SET_OUTPUT ""
#define SET_ROWCOUNT "limit %d;\n"
#define SET_DBASE "use %s;\n"
#endif

---------

3. make

`make`

4.

`./dbgen -s 1`

It will create all .tbl files in same directory.

1. macos fatal error: 'malloc.h' file not found

`#include <malloc.h>` `#include <sys/malloc.h>`

make changes in varsub.c file.
#include <sys/malloc.h>`