CC = avr-gcc
OBJCOPY = avr-objcopy.exe
#Main program file name
MAIN_APP = Fan
#MAIN hex file path in windows format
MAIN_HEX_PATH =C:\Users\LENOVO\Documents\designsim\M2-EmbSys\Project\3_Implementation\$(MAIN_APP).hex

ifdef OS
   RM = del /q
   FixPath = $(subst /,\,$1)
   EXEC = exe
else
   ifeq ($(shell uname), Linux)
      RM = rm -rf
      FixPath = $1
	  EXEC = out
   endif
endif

#Options for avr-gcc
CFLAGS = -g -Os -o
#Linking options for avr-gcc
LFLAGS = -Os -mmcu=atmega328 -o
#Options for HEX file generation
HFLAGS = -j .text -j .data -O ihex
#Sources files needed for building the application 
SRC = src/$(MAIN_APP).c src/Fanfunc.c 

#The header files needed for building the application
INCLUDE = -Iinc/

TEST_SRC =unity/test_Fan.c\
unity/unit_testing.c\
unity/unity.c

build : $(MAIN_APP).elf	
	$(OBJCOPY) $(HFLAGS) $< $(MAIN_APP).hex

$(MAIN_APP).elf: $(MAIN_APP).o	
	$(CC) $(SRC) $(INCLUDE) $(LFLAGS) $@

$(MAIN_APP).o:$(SRC)
	$(CC) $^ $(INCLUDE) $(CFLAGS) $@

clean:
	rm -f $(MAIN_APP).o	
	rm -f $(MAIN_APP).elf
	rm -f $(MAIN_APP).hex

build_test : 
	gcc $(TEST_SRC) -o $(call FixPath,$(PROJ_NAME).$(EXEC)) -lm	

run_test : build_test
	./$(call FixPath,$(PROJ_NAME).$(EXEC))

staticcheck : 
	cppcheck --enable=all -iunity .