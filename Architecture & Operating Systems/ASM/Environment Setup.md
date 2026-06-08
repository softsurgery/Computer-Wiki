## Local Environment Setup

Assembly language is dependent upon the instruction set and the architecture of the processor. In this tutorial, we focus on Intel-32 processors like Pentium. To follow this tutorial, you will need −
- An IBM PC or any equivalent compatible computer
- A copy of Linux operating system
- A copy of NASM assembler program

There are many good assembler programs, such as −

- Microsoft Assembler (MASM)
- Borland Turbo Assembler (TASM)
- The GNU assembler (GAS)

We will use the NASM assembler, as it is −

- Free. You can download it from various web sources.
- Well documented and you will get lots of information on net.
- Could be used on both Linux and Windows.

## Installing NASM

If you select "Development Tools" while installing Linux, you may get NASM installed along with the Linux operating system and you do not need to download and install it separately. For checking whether you already have NASM installed, take the following steps −

- Open a Linux terminal.
- Type **whereis nasm** and press ENTER.
- If it is already installed, then a line like, _nasm: /usr/bin/nasm_ appears. Otherwise, you will see just _nasm:_, then you need to install NASM.

To install NASM, take the following steps −
- Check [The netwide assembler (NASM)](http://www.nasm.us/) website for the latest version. 
- Download the Linux source archive `nasm-X.XX.ta.gz`, where `X.XX` is the NASM version number in the archive.
- Unpack the archive into a directory which creates a subdirectory `nasm-X. XX`.
- cd to `nasm-X.XX` and type **./configure**. This shell script will find the best C compiler to use and set up Makefiles accordingly.
- Type **make** to build the nasm and ndisasm binaries.
- Type **make install** to install nasm and ndisasm in /usr/local/bin and to install the man pages.

This should install NASM on your system. Alternatively, you can use an RPM distribution for the Fedora Linux. This version is simpler to install, just double-click the RPM file.