# Sorts int/float 4x faster than numpy by using parallel_sort from C++


## the bigger the array, the bigger the difference


### pip install npfastsortcpp 

It is basically a self-compiling Windows version of [https://stackoverflow.com/a/28663374/15096247](https://stackoverflow.com/a/28663374/15096247)


## Make sure you use the right MSVC C++ x64/x86 build tools (Link at the end of the description)


```python
# import the module:
from npfastsortcpp import parallelsort



# optional (if you want to check if the fast
# version is used) 
# If the fast version can not be imported, 
# you will see a warning when you call parallelsort
from npfastsortcpp import npfastsortconfig
npfastsortconfig.enablewarning=True
```



```
The first time you import the module, you will see this:
______________________________
Compiling ... 
If you get an error, download:
 https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=Community&channel=Release&version=VS2022&source=VSLandingPage&passive=false&cid=2030
and install: MSVC ..... C++ x64/x86 build tools
______________________________
Compiling C:/Users/Gamer/anaconda3/envs/stopjogo/lib/site-packages/npfastsortcpp/npparallelsortcpp.pyx because it changed.
[1/1] Cythonizing C:/Users/Gamer/anaconda3/envs/stopjogo/lib/site-packages/npfastsortcpp/npparallelsortcpp.pyx
running build_ext
building 'npparallelsortcpp' extension
"C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.34.31933\bin\HostX86\x64\cl.exe" /c /nologo /O2 /W3 /GL /DNDEBUG /MD -IC:\Users\Gamer\anaconda3\envs\stopjogo\include -IC:\Users\Gamer\anaconda3\envs\stopjogo\Include "-IC:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.34.31933\include" "-IC:\Program Files\Microsoft Visual Studio\2022\Community\VC\Auxiliary\VS\include" "-IC:\Program Files (x86)\Windows Kits\10\include\10.0.22621.0\ucrt" "-IC:\Program Files (x86)\Windows Kits\10\\include\10.0.22621.0\\um" "-IC:\Program Files (x86)\Windows Kits\10\\include\10.0.22621.0\\shared" "-IC:\Program Files (x86)\Windows Kits\10\\include\10.0.22621.0\\winrt" "-IC:\Program Files (x86)\Windows Kits\10\\include\10.0.22621.0\\cppwinrt" "-IC:\Program Files (x86)\Windows Kits\NETFXSDK\4.8\include\um" /EHsc /TpC:/Users/Gamer/anaconda3/envs/stopjogo/lib/site-packages/npfastsortcpp/npparallelsortcpp.cpp /Fobuild\temp.win-amd64-cpython-39\Release\Users/Gamer/anaconda3/envs/stopjogo/lib/site-packages/npfastsortcpp/npparallelsortcpp.obj
npparallelsortcpp.cpp
C:/Users/Gamer/anaconda3/envs/stopjogo/lib/site-packages/npfastsortcpp/npparallelsortcpp.cpp(2746): warning C4244: '=': conversion from 'long' to 'char', possible loss of data
"C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.34.31933\bin\HostX86\x64\link.exe" /nologo /INCREMENTAL:NO /LTCG /DLL /MANIFEST:EMBED,ID=2 /MANIFESTUAC:NO /LIBPATH:C:\Users\Gamer\anaconda3\envs\stopjogo\libs /LIBPATH:C:\Users\Gamer\anaconda3\envs\stopjogo /LIBPATH:C:\Users\Gamer\anaconda3\envs\stopjogo\PCbuild\amd64 "/LIBPATH:C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.34.31933\lib\x64" "/LIBPATH:C:\Program Files (x86)\Windows Kits\NETFXSDK\4.8\lib\um\x64" "/LIBPATH:C:\Program Files (x86)\Windows Kits\10\lib\10.0.22621.0\ucrt\x64" "/LIBPATH:C:\Program Files (x86)\Windows Kits\10\\lib\10.0.22621.0\\um\x64" /EXPORT:PyInit_npparallelsortcpp build\temp.win-amd64-cpython-39\Release\Users/Gamer/anaconda3/envs/stopjogo/lib/site-packages/npfastsortcpp/npparallelsortcpp.obj /OUT:C:\Users\Gamer\anaconda3\envs\stopjogo\lib\site-packages\npfastsortcpp\npparallelsortcpp.cp39-win_amd64.pyd /IMPLIB:build\temp.win-amd64-cpython-39\Release\Users/Gamer/anaconda3/envs/stopjogo/lib/site-packages/npfastsortcpp\npparallelsortcpp.cp39-win_amd64.lib
   Creating library build\temp.win-amd64-cpython-39\Release\Users/Gamer/anaconda3/envs/stopjogo/lib/site-packages/npfastsortcpp\npparallelsortcpp.cp39-win_amd64.lib and object build\temp.win-amd64-cpython-39\Release\Users/Gamer/anaconda3/envs/stopjogo/lib/site-packages/npfastsortcpp\npparallelsortcpp.cp39-win_amd64.exp
Generating code
Finished generating code
```



### **If you get an error, download:**

[https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=Community&channel=Release&version=VS2022&source=VSLandingPage&passive=false&cid=2030](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=Community&channel=Release&version=VS2022&source=VSLandingPage&passive=false&cid=2030)


### **and install:**

MSVC ..... C++ x64/x86 build tools


### Let's compare numpy and npfastsortcpp


```python

from npfastsortcpp import parallelsort
from time import perf_counter
import numpy as np
arr=np.random.randint(1, 15000000 + 1,size=15000000)
#arr=np.random.randn(5000000)
arr2 = arr.copy()
start=perf_counter()
parallelsort(arr) # Sorting is in place! The function returns None!
print(f'npfastsortcpp: {perf_counter() - start}')
start=perf_counter()
np.sort(arr2)
print(f'numpy: {perf_counter() - start}')
npfastsortcpp: 0.27110049999998864
numpy: 0.9681288999999964



arr=np.random.randn(5000000)
arr2 = arr.copy()
start=perf_counter()
parallelsort(arr) # Sorting is in place! The function returns None!
print(f'npfastsortcpp: {perf_counter() - start}')
start=perf_counter()
np.sort(arr2)
print(f'numpy: {perf_counter() - start}')
npfastsortcpp: 0.11203969999999686
numpy: 0.35269389999999134
```

