---
title: RCWA 程序
tags:
  - RCWA
published: true
hideInList: true
isTop: false
sticky: 101
category: 学习
abbrlink: 47032
date:
feature:
top:
---

# RCWA程序

## RCWA 一维程序

```matlab
% 不同厚度仿真 exemple2_1D


clc;clear;close all;
addpath('reticolo_allege_v9'); addpath('shuju');addpath('script');addpath('RCWA');
t1 = clock;
incident_angle = 0;
wavelength_range(:,1) = 0.4:0.001:2; % 波长范围

grating_period = 1; % 光栅周期
incident_refractive_index = 1; % 入射介质的折射率
k_parallel = incident_refractive_index * sin(incident_angle * pi / 180); % 入射角的相位角
polarization = -1; % -1:TM   1:TE   
parm = res0(polarization); parm.not_io = 1; % 初始化参数
% parm.sym.x = 1; % 利用对称性
fourier_series_M = 41; % Fourier series
fourier_series = 20; 

% 1  MgF2     % 11 W       % 21 VO2-hot
% 2  SiO2     % 12 Ti      % 22 VO2-cool
% 3  Al2O3    % 13 Fe      % 23 VO2-hot
% 4  Si3N4    % 14 Cr      % 24 DAST-Voltage
% 5  Si3N41直 % 15 Mo*     % 25 ALON
% 6  TiO2     % 16 Au      % 26 ITO
% 7  SiN      % 17 Cu      % 27 Ag*
% 8  Ge       % 18 Al
% 9  PMMA     % 19 Ag 
% 10 SiO2     % 20 VO2-cool
nk_Martix = conj(nk_data(wavelength_range*1000));
%% 结构尺寸和折射率
struct.period = 1; 
struct.height(:,1) =  [1.00   1.00   1]; % h1 h2 ...
struct.width(:,1) =  [0.4    0.6    0.8];   % w1 w2 ...
struct.nkindex(:,1) = [19   12      19];
% FDTD
struct.unit = 1e-6; % 1e-6 μm; 1e-9 nm
struct.mesh = 0.02; % mesh 
struct.mesh_M = 0.01;
struct.wave_start = min(wavelength_range);
struct.wave_stop = max(wavelength_range);
struct.wave_step = wavelength_range(2,1)-wavelength_range(1,1);

%% 结构纹理 textures
x_bloack = 51;
for iz = 1:length(struct.height)
    textures{iz}(1) = {[-struct.width(iz,1)/2,struct.width(iz,1)/2]}; % 设置结构宽度
    struct.nk(:,iz) = nk_Martix(:,struct.nkindex(iz,1)); % 设置结构折射率
end

scale_one = grating_period / x_bloack;
for io = 1:x_bloack
    x_coordinate(io) = scale_one * (io - 1) - grating_period/2;
end
textures{3}(1) = {x_coordinate};
textures{iz+1} = 1;


for method = 1:1  % 1: RCWA 2:FDTD
switch method
    %% 1 RCWA
    case 1
        for ix = 1:length(fourier_series_M)
            fourier_series = fourier_series_M(ix);
            R = zeros(length(wavelength_range), 1); T = R; A = R; 
            textures_local = textures; aa_save = {}; profil_save = {}; textures_save = {1:length(wavelength_range)};
            parfor wave_num = 1:length(wavelength_range)                 
                textures = textures_local;
                for izz = 1:iz;textures{izz}(2) = {[1, struct.nk(wave_num,izz)]};end;
                b_ix = ones(1,length(textures{1, 3}{1, 1}));
                for ia = 1:length(textures{1, 3}{1, 1})
                    if rem(ia, 2) == 0
                        b_ix(ia) = 2;
                    end
                end
                textures{3}(2) = {b_ix};

                aa = res1(wavelength_range(wave_num), struct.period, textures, fourier_series, k_parallel, parm);
                profile = {[0,  struct.height', 0], [length(textures), 1:(numel(textures)-1), length(textures)]};
                % 计算
                ef = res2(aa, profile);
                R(wave_num,1) = sum(ef.inc_top_reflected.efficiency);
                T(wave_num,1) = sum(ef.inc_top_transmitted.efficiency);
                A(wave_num,1) = 1 - sum(ef.inc_top_reflected.efficiency) - sum(ef.inc_top_transmitted.efficiency);
                textures_save{wave_num} = textures; aa_save{wave_num} = aa; profil_save{wave_num} = profile;
            end
            RCWA.R(:,ix) = R; RCWA.T(:,ix) = T; RCWA.A(:,ix) = A;
            t2 = clock; tc(t2,t1);
            RCWA.T_matrix(:,ix) = T;
        end
        RCWA.RTA = [R T A]; RCWA.R = R; RCWA.T = T; RCWA.A = A; 

        textures = textures_save{1}; aa = aa_save{1};
        profile = profil_save{1}; profile{1}(1) = 2; profile{1}(end) = 2;
        x = linspace(-struct.period/2, struct.period/2, max(x_bloack*2, 100));
        [tab1, z, o] = res3(x, aa, profile, 1, parm);         
        plot_func(3,[1 2 1 1],x,z,real(o),25, 1.5, -1000,500,900,400,'X label range (μm)','Z label (μm)','截面折射率');      
        %% 绘制透射谱
        plot_func(2,[1 2 2 0],wavelength_range,RCWA.T_matrix,0, 25,1.5,  -500,500,500,400,'Wavelength (μm)','Transmission','RCWA 透射');
        t2 = clock; tc(t2,t1);

    %% 2 FDTD
    case 2
        for mesh_num = 1:length(struct.mesh_M)
            struct.mesh = struct.mesh_M(mesh_num)
            FDTD = fun_1D(struct); % main function
            t2 = clock; tc(t2,t1);
        end
        plot_func(2, 0,FDTD.wave,FDTD.T,0, 25,1.5,  -500,500,500,400,'Wavelength (μm)','R/T/A','FDTD 透射');
end;end
%% 绘制折射率
% plot_func(2,wavelength_range,RTA.T,  0,25,1.5,  500,500,500,400,'Wavelength (μm)','Transmission','透射率与阶数的关系');
% plot_func(2,wavelength_range,RTA.R,  0,25,1.5,  1000,500,500,400,'Wavelength (μm)','Transmission','透射率与阶数的关系');
% plot_func(2, FDTD.wave,[RCWA.T FDTD.T],0, 25,1.5,  -500,500,500,400,'Wavelength (μm)','R/T/A','RCWA/FDTD 透射');





```

## RCWA 二维程序

```matlab
%   2  D    exemple_general_2Dres0
% 所有长度必须以与波长角以度为单位的相同单位表示


clc; clear; close all;
addpath('reticolo_allege_v9'); addpath('shuju');addpath('script');addpath('RCWA');
t1 = clock;
% 常规设置 % 技术参数
wavelength_range(:,1) = 0.41:0.05:2.5; % 波长范围
incident_wavelength = 2;
incident_angle = 0;
incident_refractive_index = 1; % 入射介质的折射率
k_parallel = incident_refractive_index * sin(incident_angle * pi / 180);
angle_delta = 0; %  射平面的角度
fourier_series_M = 20;
fourier_series = [20, 20]; % fourier ordres

% 纹理描述，包括基材和上层以及均质介质
%% 结构尺寸和折射率
struct.period_x = 2;
struct.period_y = 2; 
period = [struct.period_x struct.period_y];
struct.height(:,1) =  [0.6  0.0     0]; % h1 h2 ...
struct.width(:,1) =  [1     0     0];   % w1 w2 ...
struct.nkindex(:,1) = [19   13      11];
% FDTD
struct.unit = 1e-6; % 1e-6 μm; 1e-9 nm
struct.mesh_xy = 0.05; % mesh 
struct.mesh_z = 0.1;
struct.wave_start = min(wavelength_range);
struct.wave_stop = max(wavelength_range);
struct.wave_step = wavelength_range(2,1)-wavelength_range(1,1);

% 1  MgF2     % 11 W       % 21 VO2-hot
% 2  SiO2     % 12 Ti      % 22 VO2-cool
% 3  Al2O3    % 13 Fe      % 23 VO2-hot
% 4  Si3N4    % 14 Cr      % 24 DAST-Voltage
% 5  Si3N41直 % 15 Mo*     % 25 ALON
% 6  TiO2     % 16 Au      % 26 ITO
% 7  SiN      % 17 Cu      % 27 Ag*
% 8  Ge       % 18 Al
% 9  PMMA     % 19 Ag 
% 10 SiO2     % 20 VO2-cool
nk_Martix = conj(nk_data(wavelength_range*1000));

parm = res0; parm.not_io = 1; % 初始化默认参数
parm.sym.x = 0; % x=x0 :plan de symetrie seulement si beta0(1)=0    par defaut parm.sym.x=[]  pas de symetrie en x
parm.sym.y = 0; % y=y0 :plan de symetrie seulement si beta0(2)=0   par defaut parm.sym.y=[]  pas de symetrie en y
parm.sym.pol = -1; % 1 TE  -1:TM 
parm.res1.trace = 0; % 绘制纹理 –period/2 to period/2
parm.res1.champ = 1; % 精确计算电磁场


for method = 1:2  % 1: RCWA 2:FDTD
    switch method
        %% 1 RCWA
        case 1
            for ix = 1:length(fourier_series_M)
                fourier_series = [1 1] * fourier_series_M(ix);

                R = zeros(length(wavelength_range), 1); T = zeros(length(wavelength_range), 1); A = zeros(length(wavelength_range), 1);
                parm_local = parm; struct_local = struct;
                for wave_num = 1:length(wavelength_range)
                    parm = parm_local;  struct = struct_local; textures=[];
                    incident_wavelength = wavelength_range(wave_num);
                    %%
                    for iz = 1:length(struct.height)
                        struct.nk(:,iz) = nk_Martix(:,struct.nkindex(iz,1)); % 设置结构折射率
                        textures{iz} = {1, [0, 0, struct.width(iz,1), struct.width(iz,1), struct.nk(wave_num,iz), 1]};
                    end
                    textures{iz+1} = 1;


                    profile = {[1, struct.height', 1], [length(textures), 1:(numel(textures)-1), length(textures)]}; % 厚度 / 折射率索引
                    [aa, nef] = res1(incident_wavelength, period, textures, fourier_series, k_parallel, angle_delta, parm);
                    ef = res2(aa, profile);
                    R(wave_num,1) = sum(ef.TMinc_top_reflected.efficiency);
                    T(wave_num,1) = sum(ef.TMinc_bottom_transmitted.efficiency);
                    A(wave_num,1) = 1 - sum(ef.TMinc_top_reflected.efficiency) - sum(ef.TMinc_bottom_transmitted.efficiency);
                    t2 = clock; tc(t2,t1);
                end %  wavelength_range

                RCWA.R(:,ix) = R; RCWA.T(:,ix) = T; RCWA.A(:,ix) = A; RCWA.T_matrix(:,ix) = T;
                t2 = clock; tc(t2,t1);

            end
            RCWA.RTA = [R T A]; RCWA.R = R; RCWA.T = T; RCWA.A = A; 
            plot_func(2,0, wavelength_range,RCWA.T,0, 25,1.5,  -500,500,500,400,'Wavelength (μm)','R/T/A','RCWA 透射');
            
        case 2
            FDTD = fun_2D(struct);
            plot_func(2,0, FDTD.wave,[RCWA.T FDTD.T],0, 25,1.5,  -500,500,500,400,'Wavelength (μm)','R/T/A','FDTD 透射');
            t2 = clock; tc(t2,t1);
    end;end


t2 = clock; tc(t2,t1);
plot_func(2,0, wavelength_range,RCWA.T_matrix  ,0, 25,1.5,  -500,500,500,400,'Wavelength (μm)','R/T/A','RCWA 透射');


%% 绘制 XY 面纹理
for iz = 1:length(struct.height)
    struct.nk(:,iz) = nk_Martix(:,struct.nkindex(iz,1)); % 设置结构折射率
    textures{iz} = {1, [0, 0, struct.width(iz,1), struct.width(iz,1), struct.nk(1,iz), 1]};
end
textures{iz+1} = 1;
parm.res1.trace = 1; % 绘制纹理
[aa, nef] = res1(incident_wavelength, period, textures, fourier_series, k_parallel, angle_delta, parm); axis tight;
%% 绘制XZ面纹理
parm.res3.trace = 1;
x = linspace(-period(1) / 2, period(1) / 2, 51); y = 0;
[tab1, z, o] = res3(x, y, aa, profile, [1, 1], parm); % profil1
%% 绘制YZ面纹理
parm.res3.trace = 1;
y = linspace(-period(1) / 2, period(1) / 2, 51); x = 0;
[tab1, z, o] = res3(x, y, aa, profile, [1, 1], parm); % profil1
%% 绘制 Z = 固定值 XY面场
%{ 1
x = linspace(-period(1) / 2, period(1) / 2, 51);
y = linspace(-period(2) / 2, period(2) / 2, 51); z0 = 1.5;
parm.res3.npts = [0, 0, 0, 1, 0]; % 每片的点数
parm.res3.champs = [1, 1, 1]; % 带有对象轮廓的 Ex Hx 自动追踪
[e, z, o] = res3(x, y, aa, profile, [1, 1], parm);
%}



%% 计算 研究光栅入射的平面波的衍射
% ef = res2(aa, profil1);


%{ 
enerh = sum(ef.TEinc_top_reflected.efficiency); enerb = sum(ef.TEinc_top_transmitted.efficiency);
disp(['TE incident du haut : energie en haut  ', num2str(enerh), ' energie en bas ', num2str(enerb), ' pertes ', num2str(1 - enerh - enerb)]);
enerh = sum(ef.TEinc_bottom_reflected.efficiency); enerb = sum(ef.TEinc_bottom_transmitted.efficiency);
disp(['TE incident du bas : energie en haut  ', num2str(enerh), ' energie en bas ', num2str(enerb), ' pertes ', num2str(1 - enerh - enerb)]);
enerh = sum(ef.TMinc_top_reflected.efficiency); enerb = sum(ef.TMinc_top_transmitted.efficiency);
disp(['TM incident du haut : energie en haut  ', num2str(enerh), ' energie en bas ', num2str(enerb), ' pertes ', num2str(1 - enerh - enerb)]);
enerh = sum(ef.TMinc_bottom_reflected.efficiency); enerb = sum(ef.TMinc_bottom_transmitted.efficiency);
disp(['TM incident du bas : energie en haut  ', num2str(enerh), ' energie en bas ', num2str(enerb), ' pertes ', num2str(1 - enerh - enerb)]);
%}

% 绘制 Y = 固定值 XZ面场
%{ 
x = linspace(-period(1) / 2, period(1) / 2, 51); y = 0; % 网格的x,y坐标在x和y中（z中的坐标由计算确定）
einc = [1, 1]; %  u v 框架中入射 e 场的分量默认情况下入射场来自顶部
parm = res0; parm.not_io = 1; % 默认设置
parm.res3.trace = 1; % 绘制图形
[e, z, o] = res3(x, y, aa, profil1, einc, parm);
%}


%% 绘制 X = 固定值 XZ面场
%{ 
x = 0; y = linspace(-period(2) / 2, period(2) / 2, 51); einc = [1, 1];
parm = res0; parm.not_io = 1; %parametres par defaut
parm.res3.npts = 5; % nombre de points par tranche
parm.res3.sens = 0; % champ incident de dessous
parm.res3.trace = 1; % 自动绘图
parm.res3.champs = [1, 1, i];
[e, z, o] = res3(x, y, aa, profil1, einc, parm);
%}

t2 = clock; tc(t2,t1);


```



## 画图程序

使用：

```matlab
plot_func(2,[1 2 2 0],wavelength_range,RCWA.T_matrix,0, 25,1.5,  -500,500,500,400,'Wavelength (μm)','Transmission','RCWA 透射');
```





```matlab


function Y = plot_func(dimension,matrix,x, y, z,  fontsize, linewidth, x_dis, y_dis, width, height, x_label, y_label, title1)

%% 绘制单个图形
if matrix(1) == 0
    if dimension == 2
        figure('color',[1 1 1], 'position', [x_dis, y_dis, width, height]); % [位置横纵 宽 高]
        plot([0 0],[0 0],'r','LineWidth',1.5);hold on;
        set(gca,'FontSize',fontsize,'FontName','Times New Roman', 'LineWidth', linewidth);
        xlabel(x_label,'FontName','Times New Roman','FontSize',fontsize);
        ylabel(y_label,'FontName','Times New Roman','FontSize',fontsize);
        title(title1,'FontName','Monospaced','FontSize',fontsize);
        axis tight;
        for ix = 1:length(y(1,:))
            plot(x, y(:,ix),  'Color', rand(1,3), 'LineWidth', linewidth);hold on;
        end
        xlim([min(x), max(x)]);
        
    end
    if dimension == 3
        figure('color',[1 1 1], 'position', [x_dis, y_dis, width, height]); % [位置横纵 宽 高]
        h = pcolor(x, y, z); % 绘制图像，并返回句柄
        set(h, 'EdgeColor', 'none');
        axis tight; % 自动调整坐标轴的范围，使其紧贴着数据的最小值和最大值
        colorbar;colormap jet;
        set(gca, 'TickDir', 'in', 'Box', 'off', 'LineWidth', linewidth);
        set(gca,'FontName','Times New Roman','FontSize',fontsize); % Times New Roman
        xlabel(x_label,'FontName','Times New Roman','FontSize',fontsize);
        ylabel(y_label,'FontName','Times New Roman','FontSize',fontsize);
        title(title1,'FontName','Monospaced','FontSize',fontsize);
    end
end


%% 一图绘制多个图形
if matrix(1) ~= 0
    if matrix(4) == 1
        figure('color',[1 1 1], 'position', [x_dis, y_dis, width, height]);
    end
    if dimension == 2
    subplot(matrix(1),matrix(2),matrix(3));
        plot([0 0],[0 0],'r','LineWidth',1.5);hold on;
        set(gca,'FontSize',fontsize,'FontName','Times New Roman', 'LineWidth', linewidth);
        xlabel(x_label,'FontName','Times New Roman','FontSize',fontsize);
        ylabel(y_label,'FontName','Times New Roman','FontSize',fontsize);
        title(title1,'FontName','Monospaced','FontSize',fontsize);
        axis tight;
        for ix = 1:length(y(1,:))
            plot(x, y(:,ix),  'Color', rand(1,3), 'LineWidth', linewidth);hold on;
        end
        xlim([min(x), max(x)]);hold on;
    end

    if dimension == 3
        subplot(matrix(1),matrix(2),matrix(3));
%     figure('color',[1 1 1], 'position', [x_dis, y_dis, width, height]); % [位置横纵 宽 高]
    h = pcolor(x, y, z); % 绘制图像，并返回句柄
    set(h, 'EdgeColor', 'none');
    axis tight; % 自动调整坐标轴的范围，使其紧贴着数据的最小值和最大值
    colorbar;colormap jet;
    set(gca, 'TickDir', 'in', 'Box', 'off', 'LineWidth', linewidth);
    set(gca,'FontName','Times New Roman','FontSize',fontsize); % Times New Roman
    xlabel(x_label,'FontName','Times New Roman','FontSize',fontsize);
    ylabel(y_label,'FontName','Times New Roman','FontSize',fontsize);
    title(title1,'FontName','Monospaced','FontSize',fontsize);
    end
end

end

```



## 带各种材料折射率

···python

```matlab
% 不同厚度仿真 exemple2_1D
clc;clear;close all;
addpath('reticolo_allege_v9'); addpath('shuju');addpath('script');addpath('RCWA');
t1 = clock;
incident_angle = 0;
wavelength_range(:,1) = 0.4:0.001:4; % 波长范围
wavelength_range(:,1) = 0.4; % 波长范围

grating_period = 1; % 光栅周期
incident_refractive_index = 1; % 入射介质的折射率
k_parallel = incident_refractive_index * sin(incident_angle * pi / 180); % 入射角的相位角
polarization = 1; % -1:TM   1:TE   
parm = res0(polarization); parm.not_io = 1; % 初始化参数
% parm.sym.x = 1; % 利用对称性
fourier_series_M = 20; % Fourier series
fourier_series = 20; 
% nk = sqrt(3);
% 1  MgF2     % 11 W       % 21 VO2-hot
% 2  SiO2     % 12 Ti      % 22 VO2-cool
% 3  Al2O3    % 13 Fe      % 23 VO2-hot
% 4  Si3N4    % 14 Cr      % 24 DAST-Voltage
% 5  Si3N41直 % 15 Mo*     % 25 ALON
% 6  TiO2     % 16 Au      % 26 ITO
% 7  SiN      % 17 Cu      % 27 Ag*
% 8  Ge       % 18 Al
% 9  PMMA     % 19 Ag 
% 10 SiO2     % 20 VO2-cool
nk_Martix = conj(nk_data(wavelength_range*1000));
%% 结构尺寸和折射率
struct.period = grating_period; 
struct.height(:,1) =  [3   3   1]; % h1 h2 ...
struct.width(:,1) =  [0.4    0.6    0.8];   % w1 w2 ...
struct.nkindex(:,1) = [19   19      19];
% FDTD
struct.unit = 1e-6; % 1e-6 μm; 1e-9 nm
struct.mesh = 0.02; % mesh 
struct.mesh_M = 0.01;
struct.wave_start = min(wavelength_range);
struct.wave_stop = max(wavelength_range);
struct.wave_step = wavelength_range(2,1)-wavelength_range(1,1);

%% 结构纹理 textures
area(:,1) = [25 7 6 34 19 37 10 26 16 19 1]/struct.period; % 透镜纹理


x_bloack = 11;
for iz = 1:length(struct.height)
    textures{iz}(1) = {[-struct.width(iz,1)/2,struct.width(iz,1)/2]}; % 设置结构宽度
    struct.nk(:,iz) = nk_Martix(:,struct.nkindex(iz,1)); % 设置结构折射率
end

scale_one = grating_period / x_bloack;
for io = 1:x_bloack
    x_coordinate(io) = scale_one * (io - 1) - grating_period/2;
end
textures{3}(1) = {x_coordinate};
textures{iz+1} = 1;


for method = 1:1  % 1: RCWA 2:FDTD
switch method    
    case 1 %% 1 RCWA
        for ix = 1:length(fourier_series_M)
            fourier_series = fourier_series_M(ix);
            R = zeros(length(wavelength_range), 1); T = R; A = R; 
            textures_local = textures; aa_save = {}; profil_save = {}; textures_save = {1:length(wavelength_range)};
            parfor wave_num = 1:length(wavelength_range)                 
                textures = textures_local;
                for izz = 1:iz;textures{izz}(2) = {[1, struct.nk(wave_num,izz)]};end;
                b_ix = ones(1,length(textures{1, 3}{1, 1}));
                for ia = 1:length(textures{1, 3}{1, 1})
                    if rem(ia, 2) == 0
                        b_ix(ia) = 2;
                    end
                end
                textures{3}(2) = {b_ix};

                aa = res1(wavelength_range(wave_num), struct.period, textures, fourier_series, k_parallel, parm);
                profile = {[0,  struct.height', 0], [length(textures), 1:(numel(textures)-1), length(textures)]};
                % 计算
                ef = res2(aa, profile);
                R(wave_num,1) = sum(ef.inc_top_reflected.efficiency);
                T(wave_num,1) = sum(ef.inc_top_transmitted.efficiency);
                A(wave_num,1) = 1 - sum(ef.inc_top_reflected.efficiency) - sum(ef.inc_top_transmitted.efficiency);
                textures_save{wave_num} = textures; aa_save{wave_num} = aa; profil_save{wave_num} = profile;
            end
            RCWA.R(:,ix) = R; RCWA.T(:,ix) = T; RCWA.A(:,ix) = A;
            t2 = clock; tc(t2,t1);
            RCWA.T_matrix(:,ix) = T;
        end
        RCWA.RTA = [R T A]; RCWA.R = R; RCWA.T = T; RCWA.A = A; 

        textures = textures_save{1}; aa = aa_save{1};
        profile = profil_save{1}; profile{1}(1) = 2; profile{1}(end) = 2;
        x = linspace(-struct.period/2, struct.period/2, x_bloack*3);
        [e, z, o] = res3(x, aa, profile, 1, parm);         
        plot_func(3,[1 2 1 1],x,z,real(o),15, 1.5, -1000,500,900,400,'X','Z','截面折射率');      
        %% 绘制透射谱
%         plot_func(2,[1 3 2 0],wavelength_range,RCWA.T_matrix,0, 15,1.5,  -500,500,500,400,'Wavelength','T','RCWA 透射');
        plot_func(3,[1 2 2 0],x,z,e(:,:,1).*conj(e(:,:,1)),15, 1.5, -1000,500,900,400,'X','Z','(E)^2');      
        
        t2 = clock; tc(t2,t1);

    %% 2 FDTD
    case 2
        for mesh_num = 1:length(struct.mesh_M)
            struct.mesh = struct.mesh_M(mesh_num); FDTD = fun_1D(struct); t2 = clock; tc(t2,t1);
        end
        plot_func(2, 0,FDTD.wave,FDTD.T,0, 25,1.5,  -500,500,500,400,'Wavelength (μm)','R/T/A','FDTD 透射');
end;end






```



