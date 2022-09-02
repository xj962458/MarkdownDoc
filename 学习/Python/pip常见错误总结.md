## 1、pip install error: “Python.h: No such file or directory”
```bash
src/kerberos.c:18:10: fatal error: Python.h: No such file or directory
       #include <Python.h>
                ^~~~~~~~~~
      compilation terminated.
```

解决：
安装python版本对应的devel包, 比如
```bash
yum install python38-devel  # for py38
dnf install python39-devel  # for py39
```


## 2、pip install error: “ERROR: Can not execute `setup.py`”
```bash
ERROR: Can not execute `setup.py` since setuptools is not available in the build environment.
```

解决：
```bash
pip install -U setuptools
```


## 3、pip install error: No such file or directory: 'curl-config’
```bash
self._execute_child(args, executable, preexec_fn, close_fds,
        File "/usr/lib64/python3.9/subprocess.py", line 1821, in _execute_child
          raise child_exception_type(errno_num, err_msg, err_filename)
      FileNotFoundError: [Errno 2] No such file or directory: 'curl-config'
```
解决：
```bash
dnf what-provides curl-config #输出是libcurl-devel
dnf install libcurl-devel
```


### 4、pip install error: “lber.h: No such file or directory”

```bash
Modules/common.h:15:10: fatal error: lber.h: No such file or directory
       #include <lber.h>
                ^~~~~~~~
      compilation terminated.
      error: command '/usr/bin/gcc' failed with exit code 1
      [end of output]
  
  note: This error originates from a subprocess, and is likely not a problem with pip.
  ERROR: Failed building wheel for python-ldap
Failed to build python-ldap
```

Fix:

```bash
 $ rpm -qa |grep openldap
openldap-clients-2.4.46-18.el8.x86_64
openldap-2.4.46-18.el8.x86_64
 $ sudo dnf install openldap-devel
```

```bash
Modules/common.h:15:10: fatal error: lber.h: No such file or directory
       #include <lber.h>
                ^~~~~~~~
      compilation terminated.
      error: command '/usr/bin/gcc' failed with exit code 1
      [end of output]
  
  note: This error originates from a subprocess, and is likely not a problem with pip.
  ERROR: Failed building wheel for python-ldap
Failed to build python-ldap
```
解决：
```bash
rpm -qa |grep openldap openldap-clients-2.4.46-18.el8.x86_64 openldap-2.4.46-18.el8.x86_64
dnf install openldap-devel
```


## 5、pip install error: “libxml/xmlreader.h: No such file or directory”
```bash
ext/ov_xml_reader.c:20:10: fatal error: libxml/xmlreader.h: No such file or directory 
	#include <libxml/xmlreader.h> ^~~~~~~~~~~~~~~~~~~~ compilation terminated. error: command '/usr/bin/gcc' failed with exit code 1 [end of output] note: This error originates from a subprocess, and is likely not a problem with pip. error: legacy-install-failure × Encountered error while trying to install package. ╰─> ovirt-engine-sdk-python
```

解决：
```bash
dnf install install libxml2-devel
```

## 6、TLS/SSL问题
```bash
WARNING: pip is configured with locations that require TLS/SSL, however the ssl module in Python is not available.
```

解决:
将anaconda3的以下目录添加到环境变量`PATH`中，然后重新打开终端
```bash
anaconda3/
anaconda3/condabin  
anaconda3/Scripts  
anaconda3/Librarybin  
```

## 7、文件夹权限问题
