---
title: OpenGL笔记1--初识OpenGL
date: 2016-11-13
categories: technology
tags: OpenGL
---

OpenGL是一个访问图形硬件设备的API。它建立了一个规范(https://www.opengl.orgregistry/)， 由OpenGL库的开发者(通常是显卡的生产商)来具体实现。即使没有图形硬件也可以使用软件来实现OpenGl，只要严格遵循OpenGL规范。显卡驱动就包含了这些实现，所以平常我们会更新显卡驱动来解决某些bug。

OpengGL允许它的实现者(即显卡生产商)以扩展的方式添加某个新的特性，开发者就可以通过检查显卡是否支持该扩展来决定是否使用，不支持就使用旧的方案。当一个扩展比较流行或者有用后，它有可能会成为OpenGL规范。

OpenGL是个庞大的状态机(State Machine)，使用客户端-服务器的方式实现，客户端(应用程序)通过OpenGL提供的接口来向服务器(图形硬件提供的OpenGL实现)提交命令，这些命令其实就是访问、设置这些状态。OpenGL的这些状态我们可以认为是一个Context，一个客户端有一个Context，维护着一套状态机。OpenGL Context就像一个状态列表，包含了一系列的状态或者OpenGL对象(一些状态的集合)。每个状态都有一个目标位置(唯一标识)，通过目标位置(比如GL_ARRAY_BUFFER)或者某个针对特定目标位置(比如glBindVertexArray就是操作顶点数组对象)的接口我们就可以操作这些状态： 

```c
// 开启深度测试
glEnable(GL_DEPTH_TEST);
// 伪代码
glSetStateValue(STATE_KEY, value);
```

操作OpenGL对象：
```c
// 创建对象
GLuint vao;
glGenVertexArrays(1, &vao);
// 绑定对象至上下文，也就是说Context当前的顶点数组对象指向了新创建的vao
glBindVertexArray(vao);
// 设置对象的属性(状态)的值
	// 这里需要设置vao的GL_ARRAY_BUFFER，所以又创建了一个对象vbo
	GLuint vbo;
	glGenBuffers(1, &vbo);
	// 然后把它绑定到vao的GL_ARRAY_BUFFER，以便后面设置它(vbo)的属性
	glBindBuffer(GL_ARRAY_BUFFER, vbo);
	glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), &vertices, GL_STATIC_DRAW);

// 使用默认值，即Context当前的顶点数组对象指向了默认的顶点数组状态
glBindVertexArray(0);
```

我们可以创建多个对象vao，然后每一个对象都经历上面的流程。每个对象设置不同的值，这样我们就可以想使用哪个对象，就绑定哪个对象。

参考资料：https://learnopengl-cn.github.io/