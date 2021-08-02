---
title: 数学建模-SVD分解
date: 2021-05-14 20:02:52
mathjax: true
tag: [数学建模, 图形处理]
---



# SVD 分解

高等线性代数，奇异值分解。

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210508142258565.png)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210508142457281.png)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210508142517737.png)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210508142537550.png)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210508142626873.png)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210508142703738.png)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210508142831766.png)

```matlab
function [compress_A] = mysvd(A, ratio)
    % 函数作用：使用奇异值分解将矩阵A压缩到指定的特征比例
    % 输入变量：
    %     A：要压缩的m*n维的矩阵
    %     ratio：要保留原矩阵的特征比例（100%表示不压缩）
    % 输出变量：
    %     compress_A: 压缩后的矩阵
    [U,S,V] = svd(A);   % U:m*m S:m*n V:n*n
    eigs = diag(S);  % diag函数可以返回S的主对角线元素，即矩阵A的奇异值，并将其保存到列向量中
    SUM = sum(eigs);  % 计算所有奇异值的总和
    temp = 0;
    for  i=1:length(eigs)
        temp = temp + eigs(i);
        if (temp / SUM) > ratio
            break
        end
    end
    disp(['压缩后保留原矩阵的比例特征为：',num2str(roundn(100*temp/SUM,-2)),'%'])
    compress_A= U(:,1:i)*S(1:i,1:i)*V(:,1:i)';
end
```



![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210508143029688.png)

## 图片处理

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210508142915370.png)



```matlab
function [] = photo_compress(photo_address, save_address, ratio, greycompress)
    % 函数作用：利用SVD对图形进行压缩
    % 输入变量：
    %     photo_address：要压缩的图片存放的位置（建议输入完整的路径）
    %     save_address：将压缩后的图片保存的位置（建议输入完整的路径）
    %     ratio：要保留原矩阵的特征比例（100%表示不压缩）
    %     greycompress： 如果该值等于1，则会彩色的原图片转换为灰色图片后再压缩；默认值为0，表示不进行转换
    % 输出变量：
    %     无（不需要输出，因为函数运行过程中已经将图片保存）
    if nargin == 3  % 判断用户输入的参数
        greycompress = 0;
    end
    
    img = double(imread(photo_address));
    % 图片保存的对象是 'uint8' 类型，需要将其转换为double类型才能进行奇异值分解的操作
    % 注意:  img是图形的像素矩阵，如果是彩色图片则是三维矩阵，如果是灰色图片(R=G=B)则是二维矩阵
    % '赫本.jpg'是灰色的图片，得到的img类型是[914×1200]double
    % '千与千寻.jpg'是彩色的图片，得到的img类型是[768×1024×3]double
    % 因此我们可利用第三个维度的大小来判断图片是否为灰色的
    % 灰色图片的只有两个维度，所以size(img ,3) == 1
 
    if (greycompress == 1) && (size(img ,3) == 3)
        img = double(rgb2gray(imread(photo_address)));
    end

    if size(img ,3) == 3
        R=img(:,:,1);       % RGB色彩模式三要素：红色
        G=img(:,:,2);       % RGB色彩模式三要素：绿色
        B=img(:,:,3);       % RGB色彩模式三要素：蓝色
        disp(['正在压缩:  ',photo_address,'的红色要素'])
        r = mysvd(R, ratio);
        disp(['正在压缩:  ',photo_address,'的绿色要素'])
        g = mysvd(G, ratio);
        disp(['正在压缩:  ',photo_address,'的蓝色要素'])
        b = mysvd(B, ratio);
        compress_img=cat(3,r,g,b);  % 根据三个RGB矩阵（压缩后的r、g、b）生成图片对象
    else
        disp(['正在压缩灰色图片:  ',photo_address])
        compress_img = mysvd(img, ratio);
    end

    imwrite(uint8(compress_img), save_address);
end
```
