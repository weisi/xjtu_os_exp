Guide on Adding a System Call in Linux 3.2
=======================================================

:Authors: Weisi Dai (multiple1902) <multiple1902@gmail.com>,
          Xiang Ming (Heavengod) <sinxcosm@sina.com>
:Copyright: `Creative Commons BY-NC-SA 2.0 License <http://creativecommons.org/licenses/by-nc-sa/2.0/>`_

#. Get the source file of a vanilla Linux kernel, untar to a directory::

    /media/store/kernel/osexp/linux-3.2.9

#. Write your system call in ``kernel/sys.c``::

    asmlinkage long sys_osexpcall(int i){
        printk(KERN_DEBUG "Hi world with %d", i);
        return(1);
    }

#. Modify ``arch/x86/include/asm/unistd_32.h``::

    #define __NR_osexpcall  223

#. Modify ``arch/x86/kernel/syscall_table_32.S``, at the corresponding position::

    .long sys_osexpcall

#. Compile the kernel, and install it. Reboot to make it effective.

#. Write a unit test::

    #include "unistd.h"
    #define __NR_osexpcall 223

    long osexpcall(int i){
        return syscall(__NR_osexpcall, i);
    }

    int main(){
        osexpcall(31416);
        return 0;
    }

#. ... and run it. Execute ``dmesg`` to see the kernel output::

    $ [root@osexp test]# uname -r
    3.2.9-OSEXP-00001-ge765c2d-dirty
    $ [root@osexp test]# gcc syscall.c
    $ [root@osexp test]# ./a.out 
    $ [root@osexp test]# dmesg | tail -n 1
    [  222.131298] Hi world with 31416

References
==========

#. Xiang Ming. (2012). God Ming's guide on Kernel Compilation and System Calls. Not Public.
#. Cory Hekimian-Williams. (2008). Adding A System Call To Linux (Fedora 9). Retrieved from http://hekimian-williams.com/?p=20
