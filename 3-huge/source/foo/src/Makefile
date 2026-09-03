.PHONY: all clean

MKDIR = mkdir
RM = rm
RMFLAGS = -rf

CC = gcc
AR = ar
ARFLAGS = crs

DIR_OBJS = objs
DIR_EXES = ../../../build/exes
DIR_DEPS = deps
DIR_LIBS = ../../../build/libs
DIRS = $(DIR_OBJS) $(DIR_EXES) $(DIR_DEPS) $(DIR_LIBS)
RMS = $(DIR_OBJS) $(DIR_DEPS)

EXE = 
ifneq ($(EXE), "")
	EXE := $(addprefix $(DIR_EXES)/, $(EXE))
	RMS += $(EXE)
endif

LIB = libfoo.a
ifneq ($(LIB), "")
LIB := $(addprefix $(DIR_LIBS)/, $(LIB))
RMS += $(LIB)
endif

SRCS = $(wildcard *.c)
OBJS = $(patsubst %.c, %.o, $(SRCS))
OBJS := $(addprefix $(DIR_OBJS)/, $(OBJS))
DEPS = $(SRCS:.c=.dep)
DEPS := $(addprefix $(DIR_DEPS)/, $(DEPS))

ifneq "$(EXE)" ''
all: $(EXE)
endif

ifneq ($(LIB), )
all: $(LIB)
endif

ifneq ($(MAKECMDGOALS), clean)
-include $(DEPS)
endif

$(DIRS):
	$(MKDIR) $@

$(EXE): $(DIR_EXES) $(OBJS)
	$(CC) -o $@ $(filter %.o, $^)

$(LIB): $(DIR_LIBS) $(OBJS)
	$(AR) $(ARFLAGS) $@ $(filter %.o, $^)

$(DIR_OBJS)/%.o: %.c	# 这里用了模式
	if [ ! -e $(DIR_OBJS) ]; then \
		$(MKDIR) $(DIR_OBJS) ;\
	fi ;\
	$(CC) -o $@ -c $(filter %.c, $^)

# 下面主要利用 gcc -MM 选项列出 .c 源文件对其他文件的依赖关系
$(DIR_DEPS)/%.dep: $(DIR_DEPS) %.c
	@echo "Making $@ ..."
	@set -e;\
	$(RM) $(RMFLAGS) $@.tmp ;\
	$(CC) -E -MM $(filter %.c, $^) > $@.tmp ;\
	sed 's,\(.*\)\.o[ :]*,objs/\1.o $@: ,g' <$@.tmp > $@ ;\
	$(RM) $(RMFLAGS) $@.tmp ;\
	touch $(DIR_DEPS)/*.dep


clean:
	$(RM) $(RMFLAGS) $(RMS)

