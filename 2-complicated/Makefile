.PHONY: all clean

MKDIR = mkdir
RM = rm
RMFLAGS = -rf

CC = gcc

DIR_OBJS = objs
DIR_EXES = exes
DIR_DEPS = deps
DIRS = $(DIR_OBJS) $(DIR_EXES) $(DIR_DEPS)
SRCS = $(wildcard *.c)
OBJS = $(patsubst %.c, %.o, $(SRCS))
OBJS := $(addprefix $(DIR_OBJS)/, $(OBJS))
DEPS = $(SRCS:.c=.dep)
DEPS := $(addprefix $(DIR_DEPS)/, $(DEPS))
EXE = complicated
EXE := $(addprefix $(DIR_EXES)/, $(EXE))

ifeq ($(wildcard $(DIR_DEPS)), )
DEP_DIR_DEPS := $(DIR_DEPS)
endif
ifeq ($(wildcard $(DIR_OBJS)), )
DEP_DIR_OBJS := $(DIR_OBJS)
endif

all: $(EXE)

ifneq ($(MAKECMDGOALS), clean)
-include $(DEPS)        # 可以不缩进，可以空格缩进，但不能用 TAB 缩进
endif

$(DIRS):
	$(MKDIR) $@

$(EXE): $(DIR_EXES) $(OBJS)
	$(CC) -o $@ $(filter %.o, $^)

$(DIR_OBJS)/%.o: $(DEP_DIR_OBJS) %.c	# 这里用了模式
	$(CC) -o $@ -c $(filter %.c, $^)

# 下面主要利用 gcc -MM 选项列出 .c 源文件对其他文件的依赖关系
$(DIR_DEPS)/%.dep: $(DEP_DIR_DEPS) %.c
	@echo "Making $@ ..."
	@set -e;\
	$(RM) $(RMFLAGS) $@.tmp ;\
	$(CC) -E -MM $(filter %.c, $^) > $@.tmp ;\
	sed 's,\(.*\)\.o[ :]*,objs/\1.o $@: ,g' <$@.tmp > $@ ;\
	$(RM) $(RMFLAGS) $@.tmp


clean:
	$(RM) $(RMFLAGS) $(DIRS)

