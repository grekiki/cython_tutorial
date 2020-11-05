import SCons
from SCons.Builder import Builder
from SCons.Action import Action
import Cython
import distutils
import os
import shutil
import subprocess
import sys
import platform

#def cython_action(target, source, env):
#    print target, source, env
#    from Cython.Compiler.Main import compile as cython_compile
#    res = cython_compile(str(source[0]))

cythonAction = Action("$CYTHONCOM")
env = Environment()
def create_builder(env):
    try:
        cython = env['BUILDERS']['Cython']
    except KeyError:
        cython = SCons.Builder.Builder(
                  action = cythonAction,
                  emitter = {},
                  suffix = cython_suffix_emitter,
                  single_source = 1)
        env['BUILDERS']['Cython'] = cython

    return cython

def cython_suffix_emitter(env, source):
    return ".cpp"

def generate(env):
    env["CYTHON"] = "cython"
    env["CYTHONCOM"] = "$CYTHON $CYTHONFLAGS -o $TARGET $SOURCE"
    env["CYTHONCFILESUFFIX"] = ".c"

    c_file, cxx_file = SCons.Tool.createCFileBuilders(env)

    c_file.suffix['.pyx'] = cython_suffix_emitter
    c_file.add_action('.pyx', cythonAction)

    c_file.suffix['.py'] = cython_suffix_emitter
    c_file.add_action('.py', cythonAction)

    create_builder(env)


create_builder(env)
generate(env)

env.Cython("hello.pyx")

# gcc -pthread -Wno-unused-result -Wsign-compare -DNDEBUG -g -fwrapv -O3 -Wall -fPIC -I/home/grekiki/.pyenv/versions/3.8.2/include/python3.8 -c hello.c -o build/hello.o


# creating build/lib.linux-x86_64-3.8


# gcc -pthread -shared -L/home/grekiki/.pyenv/versions/3.8.2/lib -Wl,-rpath=/home/grekiki/.pyenv/versions/3.8.2/lib -L/home/grekiki/.pyenv/versions/3.8.2/lib -Wl,-rpath=/home/grekiki/.pyenv/versions/3.8.2/lib build/hello.o -L/home/grekiki/.pyenv/versions/3.8.2/lib -o hello.so
