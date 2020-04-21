{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# 线性回归的概念"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "1、线性回归的原理  \n",
    "  \n",
    "2、线性回归损失函数、代价函数、目标函数  \n",
    "  \n",
    "3、优化方法(梯度下降法、牛顿法、拟牛顿法等)  \n",
    "  \n",
    "4、线性回归的评估指标  \n",
    "  \n",
    "5、sklearn参数详解"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## 1、线性回归的原理\n",
    "\n",
    "\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "进入一家房产网，可以看到房价、面积、厅室呈现以下数据："
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "\n",
    "<table>\n",
    "    <tr>\n",
    "        <th>面积($x_1$)</th>\n",
    "        <th>厅室数量($x_2)$</th>\n",
    "        <th>价格(万元)(y)</th>\n",
    "    </tr>\n",
    "    <tr>\n",
    "        <th>64</th>\n",
    "        <th>3</th>\n",
    "        <th>225</th>\n",
    "    </tr>\n",
    "    <tr>\n",
    "        <th>59</th>\n",
    "        <th>3</th>\n",
    "        <th>185</th>\n",
    "    </tr>\n",
    "    <tr>\n",
    "        <th>65</th>\n",
    "        <th>3</th>\n",
    "        <th>208</th>\n",
    "    </tr>\n",
    "    <tr>\n",
    "        <th>116</th>\n",
    "        <th>4</th>\n",
    "        <th>508</th>\n",
    "    </tr>\n",
    "    <tr>\n",
    "        <th>……</th>\n",
    "        <th>……</th>\n",
    "        <th>……</th>\n",
    "    </tr>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "我们可以将价格和面积、厅室数量的关系习得为$f(x)=\\theta_0+\\theta_1x_1+\\theta_2x_2$，使得$f(x)\\approx y$，这就是一个直观的线性回归的样式。"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<td bgcolor=#87CEEB>\n",
    "    <font size =2>\n",
    "小练习：这是国内一个房产网站上任意搜的数据，有兴趣可以找个网站观察一下，还可以获得哪些可能影响到房价的因素？可能会如何影响到实际房价呢？</font>\n",
    "</td>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 线性回归的一般形式：\n",
    "有数据集$\\{(x_1,y_1),(x_2,y_2),...,(x_n,y_n)\\}$,其中,$x_i = (x_{i1};x_{i2};x_{i3};...;x_{id}),y_i\\in R$<br> \n",
    "其中n表示变量的数量，d表示每个变量的维度。  \n",
    "可以用以下函数来描述y和x之间的关系："
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "\\begin{align*}\n",
    "f(x) \n",
    "&= \\theta_0 + \\theta_1x_1 + \\theta_2x_2 + ... + \\theta_dx_d  \\\\\n",
    "&= \\sum_{i=0}^{d}\\theta_ix_i \\\\\n",
    "\\end{align*}"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "如何来确定$\\theta$的值，使得$f(x)$尽可能接近y的值呢？均方误差是回归中常用的性能度量，即："
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$J(\\theta)=\\frac{1}{2}\\sum_{j=1}^{n}(h_{\\theta}(x^{(i)})-y^{(i)})^2$$<br>  "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "我们可以选择$\\theta$，试图让均方误差最小化："
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 极大似然估计（概率角度的诠释）"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "下面我们用极大似然估计，来解释为什么要用均方误差作为性能度量"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "我们可以把目标值和变量写成如下等式："
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$\n",
    "y^{(i)} = \\theta^T x^{(i)}+\\epsilon^{(i)}\n",
    "$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$\\epsilon$表示我们未观测到的变量的印象，即随机噪音。我们假定$\\epsilon$是独立同分布，服从高斯分布。（根据中心极限定理）"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$\n",
    "p(\\epsilon^{(i)}) = \\frac{1}{\\sqrt{2\\pi}\\sigma}exp\\left(-\\frac{(\\epsilon^{(i)})^2}{2\\sigma^2}\\right)$$\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "因此，"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$\n",
    "p(y^{(i)}|x^{(i)};\\theta) = \\frac{1}{\\sqrt{2\\pi}\\sigma}exp\\left(-\\frac{(y^{(i)}-\\theta^T x^{(i)})^2}{2\\sigma^2}\\right)\n",
    "$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "我们建立极大似然函数，即描述数据遵从当前样本分布的概率分布函数。由于样本的数据集独立同分布，因此可以写成"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$\n",
    "L(\\theta) = p(\\vec y | X;\\theta) = \\prod^n_{i=1}\\frac{1}{\\sqrt{2\\pi}\\sigma}exp\\left(-\\frac{(y^{(i)}-\\theta^T x^{(i)})^2}{2\\sigma^2}\\right)\n",
    "$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "选择$\\theta$，使得似然函数最大化，这就是极大似然估计的思想。"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "为了方便计算，我们计算时通常对对数似然函数求最大值："
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "\\begin{align*}\n",
    "l(\\theta) \n",
    "&= log L(\\theta) = log \\prod^n_{i=1}\\frac{1}{\\sqrt{2\\pi}\\sigma}exp\\left(-\\frac{(y^{(i)}-\\theta^T x^{(i)})^2} {2\\sigma^2}\\right) \\\\\n",
    "& = \\sum^n_{i=1}log\\frac{1}{\\sqrt{2\\pi}\\sigma}exp\\left(-\\frac{(y^{(i)}-\\theta^T x^{(i)})^2}{2\\sigma^2}\\right) \\\\\n",
    "& = nlog\\frac{1}{{\\sqrt{2\\pi}\\sigma}} - \\frac{1}{\\sigma^2} \\cdot \\frac{1}{2}\\sum^n_{i=1}((y^{(i)}-\\theta^T x^{(i)})^2\n",
    "\\end{align*}"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "显然，最大化$l(\\theta)$即最小化 $\\frac{1}{2}\\sum^n_{i=1}((y^{(i)}-\\theta^T x^{(i)})^2$。"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "这一结果即均方误差，因此用这个值作为代价函数来优化模型在统计学的角度是合理的。"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## 2、线性回归损失函数、代价函数、目标函数\n",
    "* 损失函数(Loss Function)：度量单样本预测的错误程度，损失函数值越小，模型就越好。\n",
    "* 代价函数(Cost Function)：度量全部样本集的平均误差。\n",
    "* 目标函数(Object Function)：代价函数和正则化函数，最终要优化的函数。"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "常用的损失函数包括：0-1损失函数、平方损失函数、绝对损失函数、对数损失函数等；常用的代价函数包括均方误差、均方根误差、平均绝对误差等。"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<td bgcolor=#87CEEB>思考题：既然代价函数已经可以度量样本集的平均误差，为什么还要设定目标函数？</td>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### 回答：\n",
    "当模型复杂度增加时，有可能对训练集可以模拟的很好，但是预测测试集的效果不好，出现过拟合现象，这就出现了所谓的“结构化风险”。结构风险最小化即为了防止过拟合而提出来的策略，定义模型复杂度为$J(F)$，目标函数可表示为："
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$\\underset{f\\in F}{min}\\, \\frac{1}{n}\\sum^{n}_{i=1}L(y_i,f(x_i))+\\lambda J(F)$$"
   ]
  },
  {
   "attachments": {
    "image.png": {
     "image/png": "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uJHRTKYffnep+uBctG/fPhoErrnm61//Ov785z9HJI8NFj8R9V9bOT4+3tEHq8glbPwKBoDly+vF60of8vfDGLyu9PHLPDgdZH7HDmDjRiAtDRg9GrBC38T11U1B5/2wTvrh/6fT3207fhtDCSHsaNvJNv3wm6UxOPmNuXpfblrrKaUf1nt9t6/+fXPyjB/mgQHOvV788LvL4NpeV/rwexTNXLhC8eP1/wSSXwREQASaQqCyElizBmBMnwkTqLxsSmu6VgREQAREQAREQAREQAREQAQuEZDi5xILHYmACIiAowToaPvpp0BhITB0KDBwIODhLJ+OslNnIiACIiACIiACIiACIiAC4RGQ4ic8TqolAiIgApYSKCsDVq0CEhOByZOBtkyVoiICIiACIiACIiACIiACIiACFhOQ4sdioGpOBERABK5F4Px5YNMmgPF8MjOB1NRr1dY5ERABERABERABERABERABEWgaASl+msZPV4uACIhA2AQOHQJWrwa6dAGmTgUSEsK+VBVFQAREQAREQAREQAREQAREICoCUvxEhU0XiYAIiED4BM6eBZjBvKQEGDEC6NUr/GtVUwREQAREQAREQAREQAREQASaQkCKn6bQ07UiIAIi0AiBvXuBdeuA3r2BadOsSdHeSJc6LQIiIAIiIAIiIAIiIAIiIAJ1BKT4qUOhAxEQARGwjkBVFbB2LVBeDmRlGfcu61pXSyIgAiIgAiIgAiIgAiIgAiIQHgEpfsLjpFoiIAIiEDYBBm7euBFISwPGjAHi4sK+VBVFQAREQAREQAREQAREQAREwFICUvxYilONiYAIBJnAiRPAmjUAY/qMHw906hRkGhq7CIiACIiACIiACIiACIiAGwhI8eOGWZAMIiACniewZQvwySfAkCHAzTcDzZp5fkgagAiIgAiIgAiIgAiIgAiIgA8ISPHjg0nUEERABGJH4Phx4MMPjTtXdjaQlBQ7WdSzCIiACIiACIiACIiACIiACFxOQIqfy4nobxEQAREIg0BNDVBQAGzdCtx2G9CvXxgXqYoIiIAIiIAIBITAhQstAzJSDVMEREAE3E9Aih/3z5EkFAERcBmBo0eB1auNdU9ODtC6tcsElDgiIAIiIAIiEGMCJ06kxVgCdS8CIiACIhAiIMVPiITeRUAERKARAufPA/n5QFERcMcdQJ8+jVyg0yIg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"
    }
   },
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "![image.png](attachment:image.png)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "例如有以上6个房价和面积关系的数据点，可以看到，当设定$f(x)=\\sum_{j=0}^{5}\\theta_jx_j$时，可以完美拟合训练集数据，但是，真实情况下房价和面积不可能是这样的关系，出现了过拟合现象。当训练集本身存在噪声时，拟合曲线对未知影响因素的拟合往往不是最好的。\n",
    "通常，随着模型复杂度的增加，训练误差会减少；但测试误差会先增加后减小。我们的最终目的时试测试误差达到最小，这就是我们为什么需要选取适合的目标函数的原因。"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## 3、线性回归的优化方法"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 1、梯度下降法"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "设定初始参数$\\theta$,不断迭代，使得$J(\\theta)$最小化：\n",
    "$$\\theta_j:=\\theta_j-\\alpha\\frac{\\partial{J(\\theta)}}{\\partial\\theta}$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "\\begin{align*}\n",
    "\\frac{\\partial{J(\\theta)}}{\\partial\\theta} \n",
    "&= \\frac{\\partial}{\\partial\\theta_j}\\frac{1}{2}\\sum_{i=1}^{n}(f_\\theta(x)^{(i)}-y^{(i)})^2 \\\\\n",
    "&= 2*\\frac{1}{2}\\sum_{i=1}^{n}(f_\\theta(x)^{(i)}-y^{(i)})*\\frac{\\partial}{\\partial\\theta_j}(f_\\theta(x)^{(i)}-y^{(i)}) \\\\\n",
    "&= \\sum_{i=1}^{n}(f_\\theta(x)^{(i)}-y^{(i)})*\\frac{\\partial}{\\partial\\theta_j}(\\sum_{j=0}^{d}\\theta_jx_j^{(i)}-y^{(i)}))\\\\\n",
    "&= \\sum_{i=1}^{n}(f_\\theta(x)^{(i)}-y^{(i)})x_j^{(i)} \\\\\n",
    "\\end{align*}"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "即："
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$\n",
    "\\theta_j = \\theta_j + \\alpha\\sum_{i=1}^{n}(y^{(i)}-f_\\theta(x)^{(i)})x_j^{(i)}\n",
    "$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "注：下标j表示第j个参数，上标i表示第i个数据点。"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "将所有的参数以向量形式表示，可得："
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$\n",
    "\\theta = \\theta + \\alpha\\sum_{i=1}^{n}(y^{(i)}-f_\\theta(x)^{(i)})x^{(i)}\n",
    "$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "由于这个方法中，参数在每一个数据点上同时进行了移动，因此称为批梯度下降法，对应的，我们可以每一次让参数只针对一个数据点进行移动，即："
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$\n",
    "\\theta = \\theta + \\alpha(y^{(i)}-f_\\theta(x)^{(i)})x^{(i)}\n",
    "$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "这个算法成为随机梯度下降法，随机梯度下降法的好处是，当数据点很多时，运行效率更高；缺点是，因为每次只针对一个样本更新参数，未必找到最快路径达到最优值，甚至有时候会出现参数在最小值附近徘徊而不是立即收敛。但当数据量很大的时候，随机梯度下降法经常优于批梯度下降法。"
   ]
  },
  {
   "attachments": {
    "image.png": {
     "image/png": "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"
    }
   },
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "![image.png](attachment:image.png)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "当J为凸函数时，梯度下降法相当于让参数$\\theta$不断向J的最小值位置移动"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "梯度下降法的缺陷：如果函数为非凸函数，有可能找到的并非全局最优值，而是局部最优值。"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 2、最小二乘法矩阵求解"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "令<br>\n",
    "$$ X = \\left[ \\begin{array} {cccc}\n",
    "(x^{(1)})^T\\\\\n",
    "(x^{(2)})^T\\\\\n",
    "\\ldots \\\\\n",
    "(x^{(n)})^T\n",
    "\\end{array} \\right] $$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "其中，"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$x^{(i)} = \\left[ \\begin{array} {cccc}\n",
    "x_1^{(i)}\\\\\n",
    "x_2^{(i)}\\\\\n",
    "\\ldots \\\\\n",
    "x_d^{(i)}\n",
    "\\end{array} \\right]$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "由于"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$Y = \\left[ \\begin{array} {cccc}\n",
    "y^{(1)}\\\\\n",
    "y^{(2)}\\\\\n",
    "\\ldots \\\\\n",
    "y^{(n)}\n",
    "\\end{array} \\right]$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$h_\\theta(x)$可以写作"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$h_\\theta(x)=X\\theta$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "对于向量来说，有"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$z^Tz = \\sum_i z_i^2$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "因此可以把损失函数写作"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$J(\\theta)=\\frac{1}{2}(X\\theta-Y)^T(X\\theta-Y)$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "为最小化$J(\\theta)$,对$\\theta$求导可得："
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "\\begin{align*}\n",
    "\\frac{\\partial{J(\\theta)}}{\\partial\\theta} \n",
    "&= \\frac{\\partial}{\\partial\\theta} \\frac{1}{2}(X\\theta-Y)^T(X\\theta-Y) \\\\\n",
    "&= \\frac{1}{2}\\frac{\\partial}{\\partial\\theta} (\\theta^TX^TX\\theta - Y^TX\\theta-\\theta^T X^TY - Y^TY) \\\\\n",
    "\\end{align*}"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "中间两项互为转置，由于求得的值是个标量，矩阵与转置相同，因此可以写成"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "\\begin{align*}\n",
    "\\frac{\\partial{J(\\theta)}}{\\partial\\theta} \n",
    "&= \\frac{1}{2}\\frac{\\partial}{\\partial\\theta} (\\theta^TX^TX\\theta - 2\\theta^T X^TY - Y^TY) \\\\\n",
    "\\end{align*}"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "令偏导数等于零，由于最后一项和$\\theta$无关，偏导数为0。"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "因此，"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$\\frac{\\partial{J(\\theta)}}{\\partial\\theta}  = \\frac{1}{2}\\frac{\\partial}{\\partial\\theta} \\theta^TX^TX\\theta - \\frac{\\partial}{\\partial\\theta} \\theta^T X^TY\n",
    "$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "利用矩阵求导性质，<br>  \n",
    "\n",
    "\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$\n",
    "\\frac{\\partial \\vec x^T\\alpha}{\\partial \\vec x} =\\alpha \n",
    "$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$\n",
    "和\n",
    "$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$\\frac{\\partial A^TB}{\\partial \\vec x} = \\frac{\\partial A^T}{\\partial \\vec x}B + \\frac{\\partial B^T}{\\partial \\vec x}A$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    " "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "\n",
    "\\begin{align*}\n",
    "\\frac{\\partial}{\\partial\\theta} \\theta^TX^TX\\theta \n",
    "&= \\frac{\\partial}{\\partial\\theta}{(X\\theta)^TX\\theta}\\\\\n",
    "&= \\frac{\\partial (X\\theta)^T}{\\partial\\theta}X\\theta + \\frac{\\partial (X\\theta)^T}{\\partial\\theta}X\\theta \\\\\n",
    "&= 2X^TX\\theta\n",
    "\\end{align*}"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$\\frac{\\partial{J(\\theta)}}{\\partial\\theta} = X^TX\\theta - X^TY\n",
    "$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "令导数等于零，"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$X^TX\\theta = X^TY$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$\\theta = (X^TX)^{(-1)}X^TY\n",
    "$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "注：CS229视频中吴恩达的推导利用了矩阵迹的性质，可自行参考学习。"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 3、牛顿法"
   ]
  },
  {
   "attachments": {
    "image.png": {
     "image/png": "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"
    }
   },
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "![image.png](attachment:image.png)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "通过图例可知(参考吴恩达CS229),"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$f(\\theta)' = \\frac{f(\\theta)}{\\Delta},\\Delta = \\theta_0 - \\theta_1$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$可求得，\\theta_1 = \\theta_0 - \\frac {f(\\theta_0)}{f(\\theta_0)'}$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "重复迭代，可以让逼近取到$f(\\theta)$的最小值"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "当我们对损失函数$l(\\theta)$进行优化的时候，实际上是想要取到$l'(\\theta)$的最小值，因此迭代公式为："
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$\n",
    "\\theta :=\\theta-\\frac{l'(\\theta)}{l''(\\theta)}\n",
    "$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$\n",
    "当\\theta是向量值的时候，\\theta :=\\theta - H^{-1}\\Delta_{\\theta}l(\\theta)\n",
    "$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "其中，$\\Delta_{\\theta}l(\\theta)$是$l(\\theta)$对$\\theta_i$的偏导数，$H$是$J(\\theta)$的海森矩阵，<br>\n",
    "$$H_{ij} = \\frac{\\partial ^2l(\\theta)}{\\partial\\theta_i\\partial\\theta_j}$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "问题：请用泰勒展开法推导牛顿法公式。"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Answer：将$f(x)$用泰勒公式展开到第二阶，"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$f(x) = f(x_0) + f'(x_0)(x - x_0)+\\frac{1}{2}f''(x_0)(x - x_0)^2$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "对上式求导，并令导数等于0，求得x值"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$f'(x) = f'(x_0) + f''(x_0)x -f''(x_0)x_0 = 0$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "可以求得，"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$x = x_0 - \\frac{f'(x_0)}{f''(x_0)}$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "牛顿法的收敛速度非常快，但海森矩阵的计算较为复杂，尤其当参数的维度很多时，会耗费大量计算成本。我们可以用其他矩阵替代海森矩阵，用拟牛顿法进行估计，"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 4、拟牛顿法"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "拟牛顿法的思路是用一个矩阵替代计算复杂的海森矩阵H，因此要找到符合H性质的矩阵。"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "要求得海森矩阵符合的条件，同样对泰勒公式求导$f'(x) = f'(x_0) + f''(x_0)x -f''(x_0)x_0$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "令$x = x_1$，即迭代后的值，代入可得："
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$f'(x_1) = f'(x_0) + f''(x_0)x_1 - f''(x_0)x_0$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "更一般的，"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$f'(x_{k+1}) = f'(x_k) + f''(x_k)x_{k+1} - f''(x_k)x_k$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$f'(x_{k+1}) - f'(x_k)  = f''(x_k)(x_{k+1}- x_k)= H(x_{k+1}- x_k)$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$x_k$为第k个迭代值"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "即找到矩阵G，使得它符合上式。\n",
    "常用的拟牛顿法的算法包括DFP，BFGS等，作为选学内容，有兴趣者可自行查询材料学习。"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## 4、线性回归的评价指标"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "均方误差(MSE):$\\frac{1}{m}\\sum^{m}_{i=1}(y^{(i)} - \\hat y^{(i)})^2$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "均方根误差(RMSE)：$\\sqrt{MSE} = \\sqrt{\\frac{1}{m}\\sum^{m}_{i=1}(y^{(i)} - \\hat y^{(i)})^2}$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "平均绝对误差(MAE)：$\\frac{1}{m}\\sum^{m}_{i=1} | (y^{(i)} - \\hat y^{(i)} | $"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "但以上评价指标都无法消除量纲不一致而导致的误差值差别大的问题，最常用的指标是$R^2$,可以避免量纲不一致问题"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$$R^2: = 1-\\frac{\\sum^{m}_{i=1}(y^{(i)} - \\hat y^{(i)})^2}{\\sum^{m}_{i=1}(\\bar y - \\hat y^{(i)})^2} =1-\\frac{\\frac{1}{m}\\sum^{m}_{i=1}(y^{(i)} - \\hat y^{(i)})^2}{\\frac{1}{m}\\sum^{m}_{i=1}(\\bar y - \\hat y^{(i)})^2} = 1-\\frac{MSE}{VAR}$$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "我们可以把$R^2$理解为，回归模型可以成功解释的数据方差部分在数据固有方差中所占的比例，$R^2$越接近1，表示可解释力度越大，模型拟合的效果越好。"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## 5、sklearn.linear_model参数详解："
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "fit_intercept : 默认为True,是否计算该模型的截距。如果使用中心化的数据，可以考虑设置为False,不考虑截距。注意这里是考虑，一般还是要考虑截距\n",
    "\n",
    "normalize: 默认为false. 当fit_intercept设置为false的时候，这个参数会被自动忽略。如果为True,回归器会标准化输入参数：减去平均值，并且除以相应的二范数。当然啦，在这里还是建议将标准化的工作放在训练模型之前。通过设置sklearn.preprocessing.StandardScaler来实现，而在此处设置为false\n",
    "\n",
    "copy_X : 默认为True, 否则X会被改写\n",
    "\n",
    "n_jobs: int 默认为1. 当-1时默认使用全部CPUs ??(这个参数有待尝试)\n",
    "\n",
    "可用属性：\n",
    "\n",
    "coef_:训练后的输入端模型系数，如果label有两个，即y值有两列。那么是一个2D的array\n",
    "\n",
    "intercept_: 截距\n",
    "\n",
    "可用的methods:\n",
    "\n",
    "fit(X,y,sample_weight=None):\n",
    "X: array, 稀疏矩阵 [n_samples,n_features]\n",
    "y: array [n_samples, n_targets]\n",
    "sample_weight: 权重 array [n_samples]\n",
    "在版本0.17后添加了sample_weight\n",
    "\n",
    "get_params(deep=True)： 返回对regressor 的设置值\n",
    "\n",
    "predict(X): 预测 基于 R^2值\n",
    "\n",
    "score： 评估\n",
    "\n",
    "参考https://blog.csdn.net/weixin_39175124/article/details/79465558"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<table align =\"left\";background-color=\"#87CEEB\">\n",
    "<tr>\n",
    "    <td bgcolor=\"#87CEEB\"><font size=2>练习题：请用以下数据（可自行生成尝试，或用其他已有数据集）</font></td>\n",
    "</tr>\n",
    "<tr>\n",
    "<td  bgcolor=\"#87CEEB\"><font size=2>1、首先尝试调用sklearn的线性回归函数进行训练；</font></td>\n",
    "</tr>\n",
    "<tr>\n",
    "<td bgcolor=\"#87CEEB\"><font size=2>2、用最小二乘法的矩阵求解法训练数据；</font></td>\n",
    "</tr>\n",
    "<tr>    \n",
    "<td  bgcolor=\"#87CEEB\"><font size=2>3、用梯度下降法训练数据；</font></td>\n",
    "</tr>\n",
    "<tr>\n",
    "    <td  bgcolor=\"#87CEEB\"><font size=2>4、比较各方法得出的结果是否一致。</font></td>\n",
    "</tr>\n",
    "</table>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "生成数据"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [],
   "source": [
    "#生成数据\n",
    "import numpy as np\n",
    "#生成随机数\n",
    "np.random.seed(1234)\n",
    "x = np.random.rand(500,3)\n",
    "#构建映射关系，模拟真实的数据待预测值,映射关系为y = 4.2 + 5.7*x1 + 10.8*x2，可自行设置值进行尝试\n",
    "y = x.dot(np.array([4.2,5.7,10.8]))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### 1、先尝试调用sklearn的线性回归模型训练数据"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "估计的参数值为：[ 4.2  5.7 10.8]\n",
      "R2:1.0\n",
      "预测值为: [85.2]\n"
     ]
    }
   ],
   "source": [
    "import numpy as np\n",
    "from sklearn.linear_model import LinearRegression\n",
    "import matplotlib.pyplot as plt\n",
    "%matplotlib inline\n",
    "\n",
    "# 调用模型\n",
    "lr = LinearRegression(fit_intercept=True)\n",
    "# 训练模型\n",
    "lr.fit(x,y)\n",
    "print(\"估计的参数值为：%s\" %(lr.coef_))\n",
    "# 计算R平方\n",
    "print('R2:%s' %(lr.score(x,y)))\n",
    "# 任意设定变量，预测目标值\n",
    "x_test = np.array([2,4,5]).reshape(1,-1)\n",
    "y_hat = lr.predict(x_test)\n",
    "print(\"预测值为: %s\" %(y_hat))\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### 2、最小二乘法的矩阵求解"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "估计的参数值：[ 4.2  5.7 10.8]\n",
      "预测值为: [85.2]\n"
     ]
    }
   ],
   "source": [
    "class LR_LS():\n",
    "    def __init__(self):\n",
    "        self.w = None      \n",
    "    def fit(self, X, y):\n",
    "        # 最小二乘法矩阵求解\n",
    "        #============================= show me your code =======================\n",
    "        self.w = np.linalg.inv(X.T.dot(X)).dot(X.T).dot(y)\n",
    "        #============================= show me your code =======================\n",
    "    def predict(self, X):\n",
    "        # 用已经拟合的参数值预测新自变量\n",
    "        #============================= show me your code =======================\n",
    "        y_pred = X.dot(self.w)\n",
    "        #============================= show me your code =======================\n",
    "        return y_pred\n",
    "\n",
    "if __name__ == \"__main__\":\n",
    "    lr_ls = LR_LS()\n",
    "    lr_ls.fit(x,y)\n",
    "    print(\"估计的参数值：%s\" %(lr_ls.w))\n",
    "    x_test = np.array([2,4,5]).reshape(1,-1)\n",
    "    print(\"预测值为: %s\" %(lr_ls.predict(x_test)))\n",
    "\n",
    "    "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "3、梯度下降法"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "估计的参数值为：[ 4.20000001  5.70000003 10.79999997]\n",
      "预测值为：[85.19999995]\n"
     ]
    }
   ],
   "source": [
    "class LR_GD():\n",
    "    def __init__(self):\n",
    "        self.w = None     \n",
    "    def fit(self,X,y,alpha=0.02,loss = 1e-10): # 设定步长为0.002,判断是否收敛的条件为1e-10\n",
    "        y = y.reshape(-1,1) #重塑y值的维度以便矩阵运算\n",
    "        [m,d] = np.shape(X) #自变量的维度\n",
    "        self.w = np.zeros((d)) #将参数的初始值定为0\n",
    "        tol = 1e5\n",
    "        #============================= show me your code =======================\n",
    "        while tol > loss:\n",
    "            h_f = X.dot(self.w).reshape(-1,1) \n",
    "            theta = self.w + alpha*np.mean(X*(y - h_f),axis=0) #计算迭代的参数值\n",
    "            tol = np.sum(np.abs(theta - self.w))\n",
    "            self.w = theta\n",
    "        #============================= show me your code =======================\n",
    "    def predict(self, X):\n",
    "        # 用已经拟合的参数值预测新自变量\n",
    "        y_pred = X.dot(self.w)\n",
    "        return y_pred  \n",
    "\n",
    "if __name__ == \"__main__\":\n",
    "    lr_gd = LR_GD()\n",
    "    lr_gd.fit(x,y)\n",
    "    print(\"估计的参数值为：%s\" %(lr_gd.w))\n",
    "    x_test = np.array([2,4,5]).reshape(1,-1)\n",
    "    print(\"预测值为：%s\" %(lr_gd.predict(x_test)))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## 参考"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "吴恩达 CS229课程"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "周志华 《机器学习》"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "李航 《统计学习方法》"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "https://hangzhou.anjuke.com/"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "https://www.jianshu.com/p/e0eb4f4ccf3e"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "https://blog.csdn.net/qq_28448117/article/details/79199835"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "https://blog.csdn.net/weixin_39175124/article/details/79465558"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "hide_input": false,
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.7.6"
  },
  "toc": {
   "base_numbering": 1,
   "nav_menu": {},
   "number_sections": true,
   "sideBar": true,
   "skip_h1_title": false,
   "title_cell": "Table of Contents",
   "title_sidebar": "Contents",
   "toc_cell": false,
   "toc_position": {},
   "toc_section_display": true,
   "toc_window_display": false
  },
  "varInspector": {
   "cols": {
    "lenName": 16,
    "lenType": 16,
    "lenVar": 40
   },
   "kernels_config": {
    "python": {
     "delete_cmd_postfix": "",
     "delete_cmd_prefix": "del ",
     "library": "var_list.py",
     "varRefreshCmd": "print(var_dic_list())"
    },
    "r": {
     "delete_cmd_postfix": ") ",
     "delete_cmd_prefix": "rm(",
     "library": "var_list.r",
     "varRefreshCmd": "cat(var_dic_list()) "
    }
   },
   "types_to_exclude": [
    "module",
    "function",
    "builtin_function_or_method",
    "instance",
    "_Feature"
   ],
   "window_display": false
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
