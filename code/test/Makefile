#
# Makefile for building user programs to run on top of Nachos
#
#  Use "make" to build the test executable(s)
#  Use "make clean" to remove .o files and .coff files
#  Use "make distclean" to remove all files produced by make, including
#     the test executables
#
# This is a GNU Makefile.  It must be used with the GNU make program.
# At UW, the GNU make program is /software/gnu/bin/make.
# In many other places it is known as "gmake".
# You may wish to include /software/gnu/bin/ early in your command
# search path, so that you will be using GNU make when you type "make".
#
# Several things to be aware of:
#
#    It should not be necessary to build the test executables for
#     every type of host machine on which Nachos runs.  You should
#     be able to build them once, and then use them regardless of
#     the host machine type.  That is because the test executables
#     run on the simulated MIPS machine, and not on the host.
#
#    However:
#	(1) if you are experiencing problems with the test executables,
#	    it would be prudent to rebuild them on the host machine
#	    on which you are currently running Nachos.  To do this,
#    	    just type "make distclean", and then "make"
#
#	(2) the procedure used to build the test executables does
#	    depend on the host machine you are on.  All of the machine
#	    dependencies are isolated in the Makefile.dep file.
#	    It should be possible to build the test executables on
#	    any MFCF machine.   In the MFCF environment, this makefile
#           should automatically figure out what type of host you are
#	    on, and should use the appropriate procedure.
#           However, if you are working outside the MFCF environment,
#           you will need to build a cross-compiler, build coff2noff,
#           and edit Makefile.dep in this directory before you
#           can build the test programs.
#
#    Nachos assumes that the location of the program startup routine (the
# 	location the kernel jumps to when the program initially starts up)
#       is at location 0.  This means: start.o must be the first .o passed 
# 	to ld, in order for the routine "Start" to be loaded at location 0
#
#    When you make the test programs, you will see messages like these:
#		numsections 3 
#		Loading 3 sections:
#		        ".text", filepos 0xd0, mempos 0x0, size 0x440
#		        ".data", filepos 0x510, mempos 0x440, size 0x0
#		        ".bss", filepos 0x0, mempos 0x440, size 0x12c0
#    These messages are normal.  They come from the coff2noff program.
#    They are useful in that they tell you how big the various parts of your
#     compiled user program are, and where in the address space
#     coff2noff is going to place them.   This information is also
#     recorded in the header of the executable file that coff2noff
#     creates.  See the method AddrSpace::Load (in userprog/addrspace.cc)
#     for an example of how this header is used by the Nachos OS to set up the
#     address space for a new process that will run the executable.
#
#
# Adding New Test Programs:
#
#     You are free to write new test programs, and to modify the
#	existing programs.   If you write a new program, you will
# 	need to modify this makefile so that the new program will
#       get built.
#     You will need to make the following changes for each program
#       you add:
#		(1) add the program's name to PROGRAMS variable definition
#	 	(2) add dependencies and build commands for the new
#			program.  The easiest way to do this is to
#			copy the dependencies and commands for an
#			existing program, and then change the names.
#
#	For example, if you write a test program in foo.c, for which
#	the executable is to be called foo, you should do the following:
#
#	change the PROGRAMS definition to look like this:
#
#		PROGRAMS = halt shell matmult sort foo
#
#	add these dependencies/commands:
#
#		foo.o: foo.c
#			$(CC) $(CFLAGS) -c foo.c
#		foo: foo.o start.o
#			$(LD) $(LDFLAGS) start.o foo.o -o foo.coff
#			$(COFF2NOFF) foo.coff foo
#
#       Be careful when you copy the commands!  The commands
# 	must be indented with a *TAB*, not a bunch of spaces.
#
#
#############################################################################
# Makefile.dep contains all machine-dependent definitions
# If you are trying to build coff2noff somewhere outside
# of the MFCF environment, you will almost certainly want
# to visit and edit Makefile.dep before doing so
#############################################################################

include Makefile.dep

CC = $(GCCDIR)gcc
AS = $(GCCDIR)as
LD = $(GCCDIR)ld

INCDIR =-I../userprog -I../lib
CFLAGS = -G 0 -c $(INCDIR) -B../../../usr/local/nachos/lib/gcc-lib/decstation-ultrix/2.95.2/ -B../../../usr/local/nachos/decstation-ultrix/bin/

ifeq ($(hosttype),unknown)
PROGRAMS = unknownhost
else
# change this if you create a new test program!
PROGRAMS = halt shell matmult sort segments\
			createfile delete cat copy concatenate testSeek\
			fileclient echoclient exec execv pingpong ping pong testSem threadSem test
endif

all: $(PROGRAMS)

start.o: start.S ../userprog/syscall.h
	$(CC) $(CFLAGS) $(ASFLAGS) -c start.S

halt.o: halt.c
	$(CC) $(CFLAGS) -c halt.c
halt: halt.o start.o
	$(LD) $(LDFLAGS) start.o halt.o -o halt.coff
	$(COFF2NOFF) halt.coff halt

add.o: add.c
	$(CC) $(CFLAGS) -c add.c

add: add.o start.o
	$(LD) $(LDFLAGS) start.o add.o -o add.coff
	$(COFF2NOFF) add.coff add

shell.o: shell.c
	$(CC) $(CFLAGS) -c shell.c
shell: shell.o start.o
	$(LD) $(LDFLAGS) start.o shell.o -o shell.coff
	$(COFF2NOFF) shell.coff shell

sort.o: sort.c
	$(CC) $(CFLAGS) -c sort.c
sort: sort.o start.o
	$(LD) $(LDFLAGS) start.o sort.o -o sort.coff
	$(COFF2NOFF) sort.coff sort

segments.o: segments.c
	$(CC) $(CFLAGS) -c segments.c
segments: segments.o start.o
	$(LD) $(LDFLAGS) start.o segments.o -o segments.coff
	$(COFF2NOFF) segments.coff segments

matmult.o: matmult.c
	$(CC) $(CFLAGS) -c matmult.c
matmult: matmult.o start.o
	$(LD) $(LDFLAGS) start.o matmult.o -o matmult.coff
	$(COFF2NOFF) matmult.coff matmult

testReadWrite.o: testReadWrite.c
	$(CC) $(CFLAGS) -c testReadWrite.c
testReadWrite: testReadWrite.o start.o
	$(LD) $(LDFLAGS) start.o testReadWrite.o -o testReadWrite.coff
	$(COFF2NOFF) testReadWrite.coff testReadWrite

createfile.o: createfile.c
	$(CC) $(CFLAGS) -c createfile.c
createfile: createfile.o start.o
	$(LD) $(LDFLAGS) start.o createfile.o -o createfile.coff
	$(COFF2NOFF) createfile.coff createfile

cat.o: cat.c
	$(CC) $(CFLAGS) -c cat.c
cat: cat.o start.o
	$(LD) $(LDFLAGS) start.o cat.o -o cat.coff
	$(COFF2NOFF) cat.coff cat

copy.o: copy.c
	$(CC) $(CFLAGS) -c copy.c
copy: copy.o start.o
	$(LD) $(LDFLAGS) start.o copy.o -o copy.coff
	$(COFF2NOFF) copy.coff copy

concatenate.o: concatenate.c
	$(CC) $(CFLAGS) -c concatenate.c
concatenate: concatenate.o start.o
	$(LD) $(LDFLAGS) start.o concatenate.o -o concatenate.coff
	$(COFF2NOFF) concatenate.coff concatenate

delete.o: delete.c
	$(CC) $(CFLAGS) -c delete.c
delete: delete.o start.o
	$(LD) $(LDFLAGS) start.o delete.o -o delete.coff
	$(COFF2NOFF) delete.coff delete

fileclient.o: fileclient.c
	$(CC) $(CFLAGS) -c fileclient.c
fileclient: fileclient.o start.o
	$(LD) $(LDFLAGS) start.o fileclient.o -o fileclient.coff
	$(COFF2NOFF) fileclient.coff fileclient

echoclient.o: echoclient.c
	$(CC) $(CFLAGS) -c echoclient.c
echoclient: echoclient.o start.o
	$(LD) $(LDFLAGS) start.o echoclient.o -o echoclient.coff
	$(COFF2NOFF) echoclient.coff echoclient

testSeek.o: testSeek.c
	$(CC) $(CFLAGS) -c testSeek.c
testSeek: testSeek.o start.o
	$(LD) $(LDFLAGS) start.o testSeek.o -o testSeek.coff
	$(COFF2NOFF) testSeek.coff testSeek

testSyscall.o: testSyscall.c
	$(CC) $(CFLAGS) -c testSyscall.c
testSyscall: testSyscall.o start.o
	$(LD) $(LDFLAGS) start.o testSyscall.o -o testSyscall.coff
	$(COFF2NOFF) testSyscall.coff testSyscall
exec.o: exec.c
	$(CC) $(CFLAGS) -c exec.c
exec: exec.o start.o
	$(LD) $(LDFLAGS) start.o exec.o -o exec.coff
	$(COFF2NOFF) exec.coff exec

execv.o: execv.c
	$(CC) $(CFLAGS) -c execv.c
execv: execv.o start.o
	$(LD) $(LDFLAGS) start.o execv.o -o execv.coff
	$(COFF2NOFF) execv.coff execv

pingpong.o: pingpong.c
	$(CC) $(CFLAGS) -c pingpong.c
pingpong: pingpong.o start.o
	$(LD) $(LDFLAGS) start.o pingpong.o -o pingpong.coff
	$(COFF2NOFF) pingpong.coff pingpong

test.o: test.c
	$(CC) $(CFLAGS) -c test.c
test: test.o start.o
	$(LD) $(LDFLAGS) start.o test.o -o test.coff
	$(COFF2NOFF) test.coff test

ping.o: ping.c
	$(CC) $(CFLAGS) -c ping.c
ping: ping.o start.o
	$(LD) $(LDFLAGS) start.o ping.o -o ping.coff
	$(COFF2NOFF) ping.coff ping

pong.o: pong.c
	$(CC) $(CFLAGS) -c pong.c
pong: pong.o start.o
	$(LD) $(LDFLAGS) start.o pong.o -o pong.coff
	$(COFF2NOFF) pong.coff pong
testSem.o: testSem.c
	$(CC) $(CFLAGS) -c testSem.c
testSem: testSem.o start.o
	$(LD) $(LDFLAGS) start.o testSem.o -o testSem.coff
	$(COFF2NOFF) testSem.coff testSem

threadSem.o: threadSem.c
	$(CC) $(CFLAGS) -c threadSem.c
threadSem: threadSem.o start.o
	$(LD) $(LDFLAGS) start.o threadSem.o -o threadSem.coff
	$(COFF2NOFF) threadSem.coff threadSem	

clean:
	$(RM) -f *.o *.ii
	$(RM) -f *.coff

distclean: clean
	$(RM) -f $(PROGRAMS)

unknownhost:
	@echo Host type could not be determined.
	@echo make is terminating.
	@echo If you are on an MFCF machine, contact the instructor to report this problem
	@echo Otherwise, edit Makefile.dep and try again.
