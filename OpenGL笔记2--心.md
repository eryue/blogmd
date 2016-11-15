---
title: OpenGL笔记2--画个心
date: 2016-11-13
categories: technology
tags: OpenGL
---

### 准备
链接GLFW和GLEW。

GLFW：创建窗口和OpenGL上下文以及处理用户输入。

GLEW：获取OpenGL函数指针

接下来就是初始化GLFW和GLEW，并且使用GLFW创建一个窗口：

```cpp

```

### Shader

### 绘制

```cpp
void genHeart_2(Vector3f *vers, int offset, int segments,  
             float ox, float oy, float R = 0.05)  
{  
    for (int i = 0; i < segments; i++)  
    {  
        float theta = 2.0f * PI * float(i) / float(segments);  
  
        float x = R * 16 * pow(sin(theta), 3);  
        float y = R * (13 * cos(theta) - 5*cos(2*theta)  
            - 2*cos(3*theta) - cos(4*theta));  
  
        vers[offset+i] = Vector3f(ox+x, oy+y, 0.0f);  
    }  
}
```

参考资料：https://learnopengl-cn.github.io/