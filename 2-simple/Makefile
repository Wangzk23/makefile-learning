.PHONY : clean

CC = gcc
RM = rm

EXE = simple
SRCS = $(wildcard *.c)
#OBJS = main.o foo.o
OBJS = $(patsubst %.c,%.o,$(SRCS))

$(EXE) : ${OBJS}
	$(CC) -o $@ $^

#main.o : main.c
#	$(CC) -o $@ -c $^
#foo.o : foo.c
#	$(CC) -o $@ -c $^

%.o : %.c
	$(CC) -o $@ -c $^

clean:
	$(RM) $(EXE) $(OBJS)
