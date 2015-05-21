

# EZlogger

[![Build Status](https://drone.io/github.com/zerosignal0/ezlogger/status.png)](https://drone.io/github.com/zerosignal0/ezlogger/latest) ![build_status](https://travis-ci.org/zerosignal0/ezlogger.svg?branch=master) [![Latest Version](https://pypip.in/version/ezlogger/badge.svg)](https://pypi.python.org/pypi/ezlogger/) [![Python Package Downloads](https://pypip.in/download/ezlogger/badge.svg)](https://pypi.python.org/pypi/ezlogger/) [![Python Version Coverage](https://pypip.in/py_versions/ezlogger/badge.svg)](https://pypi.python.org/pypi/ezlogger/) [![License](https://pypip.in/license/ezlogger/badge.svg)](https://pypi.python.org/pypi/ezlogger/)

The purpose of this module is to provide python developers the ability to have an easy to use, standardized library to use for all their logging needs.  Currently the library is able to initialize capture of STDERR/STDOUT in a colored CLI based upon log type along with automatic log streaming to a file as well.

The code base can be found : [here](https://github.com/zerosignal0/ezlogger/blob/master/README.md)

## Standard usage of the library

Firstly you must import the library to your python based application.

```python
   >>> # shared logging handler
   >>> from ezlogger import initialize_logger, \
                            default_argparse, \
                            linenum
```

Setting up the logging stream handler in this method collapses the
argument parser from this file into the ezlogger module parser
allowing both parsers to co-exist in one.

```python
   >>> logger = initialize_logger(default_argparse(), 'name_of_logging_instance')
```

If you '''DO NOT''' wish to use a argument parser within your application, you can bypass this option by
using the following.

```python
   >>> args = default_argparse(pre_parse=True)
   >>> logger = initialize_logger(args, 'name_of_logging_instance')
```

If you wish to enable syslog writing without specifying the arguments via command-line arguments, you can specify them from within your application as follows:

```python
   >>> args = default_argparse(pre_parse=True)
   >>> args.syslog = True # This will enable syslogging
   >>> args.syslog_level = 'warning' # This is optional, can be info, warning or error and controls the syslog facility at which to write logs.
   >>> logger = initialize_logger(args, 'name_of_logging_instance', syslog_server='syslog_addr')
```

*Example of output written to syslog (in this example, error messages):

```
2015 May 11 11:48:37 homebase.local 10.20.30.40 514 ERROR [appname]: test_me.py: [alias]: garyw line:43 class_name: TestClass error: Assertion of a==2 has failed.
```

If you wish to have your logging files auto-rotate daily (UTC midnight), and/or at maximum log file size, you may specify the following. The below example is calling from within a script, however you can also specify the arguments --logfile_rotate and the optional --logsize <value> as well.

```python
   >>> args = default_argparse(pre_parse=True)
   >>> args.logfile_rotate = True # This will enable log file rotation.
   >>> args.logsize = 100 # This is optional and specifies the max logfile size in MB, when reached auto log rollover will occur.
   >>> logger = initialize_logger(args, __script_name__)
```

If you wish to set to specify the directory that the log file will be created, you can specify it with arguments provided by the default_argparse method.  Also you can provide the flag at application runtime from the command prompt.

```python
[garyw@homebase.local ~] $ python testapp.py -ld /tmp/supercoolloggingdir

```

To write data to STDOUT and the logging file, use the following:

```python
logger.debug('line:{}: This is a debug message.'.format(linenum()))
logger.info('line:{}: This is a informational message'.format(linenum()))
logger.warning('line:{}: This is a warning message'.format(linenum()))
logger.error('line:{}: This is a error message'.format(linenum()))
```

* NOTE including linenum is an optional step and can be ignored if preferred.  An example without using linenum:

```python
logger.debug('This is a debug message.')
logger.info('This is a informational message')
logger.warning('This is a warning message')
logger.error('This is a error message')
```
