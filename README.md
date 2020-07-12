The TPCH database and dbgen data generation utility, courtesy of www.tpc.org, were developed to provide an approach to benchmarking and include:

The tpch Database structure
The tpch dbgen utility, a utility to populate the database with a specified amount of data (Scale Factor)
The tpch benchmark queries, a set of pre-defined data warehouse queries to run against the database
We will show the details of the creation of the tpch database and it's population using the dbgen utility to generate data.



tpch-kit
TPCh Data Generation

# TPC-H

1. cd dbgen && cp makefile.suite makefile

# Modifications
The following modifications have been added on top of the official TPC-H kit:

modify dbgen to not print trailing delimiter
add option for dbgen to output to stdout
add compile support for macOS
add define for PostgreSQL to support LIMIT N for qgen
adjust Makefile defaults

# Linux
Make sure the required development tools are installed:

# Ubuntu:

# sudo apt-get install git make gcc
CentOS/RHEL:

# sudo yum install git make gcc

# macOS

Make sure the required development tools are installed:

xcode-select --install
Then run the following commands to clone the repo and build the tools:

brew install git make gcc

# Edit few files to make it compatible with Hive
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

4. Generating the data
 Scale factors
Scale factors used for the test database must be chosen from the set of fixed scale factors defined as follows:

Scale factor (SF)	1	10	30	100	300	1000	3000	10000	30000	100000
Database sizes	1GB	10GB	30GB	100GB	300GB	1000GB	3000GB	10000GB	30000GB	100000GB
where:

GB stands for gigabyte, defined to be 2^30 bytes.

Dbgen
Copy Dbgen to the root directory (tpch_2_16_0\tpch_2_15_0\dbgen)
and start the following command to generate 1GB:

dbgen -vf -s 1

TPC-H Population Generator (Version 2.14.3)
Copyright Transaction Processing Performance Council 1994 - 2010
Generating data for suppliers table/
Preloading text ... 100%
done.
Generating data for customers tabledone.
Generating data for orders/lineitem tablesdone.
Generating data for part/partsupplier tablesdone.
Generating data for nation tabledone.
Generating data for region tabledone.


It will create all .tbl files in same directory.

Support
1.1 - Open failed for .\dists.dss
 
TPC-H Population Generator (Version 2.14.3)
Copyright Transaction Processing Performance Council 1994 - 2010
Open failed for .\dists.dss at c:\users\gerard\desktop\tpc\tpch_2_14_3\dbgen\bm_utils.c:308
By default, the generator use the b option and search the default file dists.dss. Try to use the -h operator.

1.2 - fatal error U1001: syntax error : illegal character '{'
C:\Users\gerard\Desktop\tpc\tpch_2_14_3\dbgen>nmake /f makefile
Microsoft (R) Program Maintenance Utility Version 10.00.30319.01
Copyright (C) Microsoft Corporation.  All rights reserved.

makefile(127) : fatal error U1001: syntax error : illegal character '{' in macro
Stop.
In the makefile, replace:

{ with (
} with (


1.3 : macos fatal error: 'malloc.h' file not found

`#include <malloc.h>` `#include <sys/malloc.h>`

make changes in varsub.c file.
#include <sys/malloc.h>`
