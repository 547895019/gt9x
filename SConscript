from building import *
Import('rtconfig')

src   = []
cwd   = GetCurrentDir()

# add gt9x src files.
if GetDepend('PKG_USING_GT9X'):
    src += Glob('src/gt9x.c')

if GetDepend('PKG_USING_GT9X_SAMPLE'):
    src += Glob('samples/gt9x_sample.c')

# add gt9x include path.
path  = [cwd + '/inc']

# add src and include to group.
group = DefineGroup('gt9x', src, depend = ['PKG_USING_GT9X'], CPPPATH = path)

Return('group')
