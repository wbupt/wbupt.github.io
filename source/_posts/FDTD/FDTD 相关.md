---
title: FDTD 相关
tags:
  - FDTD
top: 
abbrlink: f386
---
## 导出折射率

```python
wavelength=linspace(0.25,25,1000);  #设定选取波段，采样数量

f = c/(wavelength*1e-6); # 计算频率，国际单位制
 
i = getindex('Ti (Titanium) - Palik',f); #读取折射率
 
n = real(i); #读取折射率实部
k = imag(i); # 折射率虚部（消光系数）

nk = [wavelength, n, k];

matlabsave('Ti',nk); 
```

## 保存电场强度

```python
A=getresult("XZ","E");
XZ=A.getattribute("E");
matlabsave('XZ',XZ); # 保存监视器XZ面的电场强度
```





## 常用代码

```python
#	mesh1
setnamed('mesh1','x',0);
setnamed('mesh1','x span',y2);
setnamed('mesh1','y',0);
setnamed('mesh1','y span',y2);
setnamed('mesh1','z max',0-h_1);
setnamed('mesh1','z min',-h1-h2-h3-h_1);

setnamed('mesh1','dx',mesh_a);
setnamed('mesh1','dy',mesh_a);
setnamed('mesh1','dz',mesh_z);

#	mesh2
setnamed('mesh2','x',0);
setnamed('mesh2','x span',w);
setnamed('mesh2','y',0);
setnamed('mesh2','y span',w);
setnamed('mesh2','z min',-t_sub+h_2);
setnamed('mesh2','z max',-t_sub+h6+h5+h4+h_2);

setnamed('mesh2','dx',mesh_a);
setnamed('mesh2','dy',mesh_a);
setnamed('mesh2','dz',mesh_z);
```



```matlab
deleteall;
addrect;
set("x",0);
set("x span",(h1) * 2* scale);
set("y max",0); # y
set("y min", -thick); 
set("material", material); set("index", nk);

w = [50,    14,    12,    68,    38,    74,    20,    52,    32,    38,     2]*1e-9;

for ( i=1:1:4 )
{
    addrect;    
    set("x",(sum(w(1:2*i))+w(2*i+1)/2)*scale);
    set("x span",w(2*i+1)*scale);
    set("y max",0); set("y min", -thick); 
    set("material", material); set("index", nk);
    #set("x",(h1+h2+h3/2)*scale);     
    addrect;
    set("x",-(sum(w(1:2*i))+w(2*i+1)/2)*scale);
    set("x span",w(2*i+1)*scale);
    set("y max",0); set("y min", -thick); 
    set("material", material); set("index", nk);  
    }
```

![image-20230506110907422](https://cdn.staticaly.com/gh/yangmulao/blogcdn@master/img/image-20230506110907422.png)

## 绘制图形

```python
plot_func(3,[1 2 2 0],  1:length(field(1,:)),1:length(field(:,1)), field, 15,1.5,  100,100,1000,400,'X','Z','|Ez|^2 & FOM');
set(gca, 'YDir', 'reverse'); 
title(sprintf('|Ez|^2 & FOM = %4f', -FOM ));
```

