APID=hello-embedded-python
INC=/usr/local/python3/include/python3.7m/
libpath=/usr/local/python3/lib/
LIB=python3.7m
cflg=-g
CC=gcc


.phony:clean

all:$(APID).o
	@echo "xxxxXXXX"
	gcc  $(APID).o -L$(libpath) -l $(LIB) -o $(APID)

%.o:%.c
	@echo "xxxx"
	$(CC) $(cflg) -I $(INC) -c $< -o $@


clean:
	@rm -rf $(APID)
