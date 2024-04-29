---
title: match
tags: [MATLAB]
published: true
hideInList: true
isTop: false
abbrlink: 20929
date: 
feature:
sticky: 101
top: 
category: 学习 # 分类 MATLAB
---
---
# match

## 主程序

```matlab
clc;clear;close all;t1  =clock;
[num, boy_txt, raw] = xlsread('C:\Users\Administrator\Desktop\boy.xlsx');fclose('all');boy_txt = boy_txt(2:end,:);
[num, girl_txt, raw] = xlsread('C:\Users\Administrator\Desktop\girl.xlsx');fclose('all');girl_txt = girl_txt(2:end,:);

for ix = 1:length(girl_txt(:,2)) girl_W1_M(ix) = W1_structure(girl_txt{ix,2}); end
for ix = 1:length(boy_txt(:,2)) boy_W1_M(ix) = W1_structure(boy_txt{ix,2}); end

for ix = 1:length(girl_txt(:,2)) girl_W2_M(ix) = W1_structure(girl_txt{ix,3}); end
for ix = 1:length(boy_txt(:,2)) boy_W2_M(ix) = W1_structure(boy_txt{ix,3}); end

for ix = 1:length(girl_txt(:,2))
    for iy =1:length(boy_txt(:,2))       
        
        girl_W1_one = girl_W1_M(ix);
        boy_W1_one = boy_W1_M(iy);
        match_result(ix, iy) = W1_match(girl_W1_one, boy_W1_one) * W1_match(boy_W1_one, girl_W1_one);


        girl_W2_one = girl_W2_M(ix);
        boy_W2_one = boy_W2_M(iy);
        
        W2_total(ix, iy) = ( W2_cal(girl_W2_one, boy_W2_one) + W2_cal(boy_W2_one, girl_W2_one) ) *0.5
        
    end
end



%% girl - match 
t2=clock;tc(t2,t1);

```

## W1_match

```HTML

function Y = W1_match(girl_W1_one, boy_W1_one)

    match_W1 = 1;
    match_score = 0;
    if abs( boy_W1_one.height(1) - girl_W1_one.height(2) ) <= girl_W1_one.height(3) 
        match_score = 1; 
    end
    match_W1 = match_W1 * match_score;

    match_score = 0;
    if abs( boy_W1_one.weight(1) - girl_W1_one.weight(2) ) <= girl_W1_one.weight(3) 
        match_score = 1;
    end
    match_W1 = match_W1 * match_score;

    match_score = 0; % 判断元素是否在数组中        
    if ismember(boy_W1_one.course(1), girl_W1_one.course(2:end) )
        match_score = 1;
    end
    match_W1 = match_W1 * match_score;

    match_score = 0; % 判断元素是否在数组中        
    if ismember(boy_W1_one.grade(1), girl_W1_one.grade(2:end) )
        match_score = 1;
    end
    match_W1 = match_W1 * match_score;

    match_score = 0; % 判断元素是否在数组中        
    if ismember(boy_W1_one.constellation(1), girl_W1_one.constellation(2:end) )
        match_score = 1;
    end
    match_W1 = match_W1 * match_score;

    match_score = 0;
    if abs( boy_W1_one.birth(1) - girl_W1_one.birth(2) ) <= girl_W1_one.birth(3) 
        match_score = 1;
    end
    match_W1 = match_W1 * match_score;

    match_score = 0;
    if distance(boy_W1_one.my(2), boy_W1_one.my(1), girl_W1_one.my(2), girl_W1_one.my(1), referenceEllipsoid('WGS 84', 'kilometer')) <=  girl_W1_one.my(3)
        match_score = 1;
    end
    match_W1 = match_W1 * match_score;

    match_score = 0;
    if distance(boy_W1_one.your(2), boy_W1_one.my(1), girl_W1_one.your(2), girl_W1_one.my(1), referenceEllipsoid('WGS 84', 'kilometer')) <=  girl_W1_one.your(3)
        match_score = 1;
    end
    match_W1 = match_W1 * match_score;

Y = match_W1;
```



## W2_cal

```matlab
function Y = W2_cal(girl_W2_one, boy_W2_one)

    field_names = fieldnames(boy_W2_one);
    n_instances = numel(field_names);
    boy_W2_one_Matrix = zeros(n_instances,3);
    for i = 1:n_instances boy_W2_one_Matrix(i,:) = boy_W2_one.(field_names{i});end
    for i = 1:n_instances girl_W2_one_Matrix(i,:) = girl_W2_one.(field_names{i});end
    
    sum1 = 0;
    for iz = 1:length(boy_W2_one_Matrix(:,1))
        sum1 = sum1 + girl_W2_one_Matrix(iz,1) * W2_fx(girl_W2_one_Matrix(iz,3), boy_W2_one_Matrix(iz,2));
    end
    W2_score = sum1 / (sum(girl_W2_one_Matrix(:, 1)) + 0.0001 );
    
Y = W2_score;
```

## W2_fx

```matlab
function Y = W2_fx(x, y)
x = x / 100;
y = y / 100;

Y = 0;
if abs(x-y)<= 0.6
    Y = -abs(x-y) * 1 / 0.6 + 1;
end
if abs(x-y)<= 0.1
    Y = 1;
end
```

