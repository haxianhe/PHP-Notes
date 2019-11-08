# PHP 常见概念

## MVC

MVC是一个**设计模式**，它强制性的使应用程序的**输入**、**处理**和**输出**分开。使用MVC应用程序被分成三个核心部件：**模型**（M）、**视图**（V）、**控制器**（C），它们各自处理自己的任务。

* **视图：视图是用户看到并与之交互的界面**。对老式的Web应用程序来说，视图就是由HTML元素组成的界面，在新式的Web应用程序中，HTML依旧在视图中扮演着重要的角色，但一些新的技术已层出不穷，它们包括Adobe Flash和象XHTML，XML/XSL，WML等一些标识语言和Web services。如何处理应用程序的界面变得越来越有挑战性。MVC一个大的好处是它能为你的应用程序处理很多不同的视图。在视图中其实没有真正的处理发生，不管这些数据是联机存储的还是一个雇员列表，作为视图来讲，它只是作为一种输出数据并允许用户操纵的方式。
* **模型：模型表示企业数据和业务规则**。在MVC的三个部件中，模型拥有最多的处理任务。例如它可能用象EJBs和ColdFusion Components这样的构件对象来处理数据库。被模型返回的数据是中立的，就是说模型与数据格式无关，这样一个模型能为多个视图提供数据。由于应用于模型的代码只需写一次就可以被多个视图重用，所以减少了代码的重复性。
* **控制器：控制器接受用户的输入并调用模型和视图去完成用户的需求**。所以当单击Web页面中的超链接和发送HTML表单时，控制器本身不输出任何东西和做任何处理。它只是接收请求并决定调用哪个模型构件去处理请求，然后确定用哪个视图来显示模型处理返回的数据。

现在我们总结MVC的处理过程，首先控制器接收用户的请求，并决定应该调用哪个模型来进行处理，然后模型用业务逻辑来处理用户的请求并返回数据，最后控制器用相应的视图格式化模型返回的数据，并通过表示层呈现给用户。

## OOP

**面向对象编程**（Object Oriented Programming，OOP，面向对象程序设计）是一种计算机编程架构。OOP 的一条基本原则是，计算机程序是由单个能够起到子程序作用的单元或对象组合而成。OOP 达到了软件工程的三个主要目标：**重用性、灵活性和扩展性**。为了实现整体运算，每个对象都能够接收信息、处理数据和向其它对象发送信息。OOP 主要有以下的概念和组件：

* **组件** － 数据和功能一起在运行着的计算机程序中形成的单元，组件在 OOP 计算机程序中是模块和结构化的基础。
* **抽象性** － 程序有能力忽略正在处理中信息的某些方面，即对信息主要方面关注的能力。
* **封装性** － 也叫做信息封装：确保组件不会以不可预期的方式改变其它组件的内部状态；只有在那些提供了内部状态改变方法的组件中，才可以访问其内部状态。每类组件都提供了一个与其它组件联系的接口，并规定了其它组件进行调用的方法。
* **继承性** － 允许在现存的组件基础上创建子类组件，这统一并增强了多态性和封装性。典型地来说就是用类来对组件进行分组，而且还可以定义新类为现存的类的扩展，这样就可以将类组织成树形或网状结构，这体现了动作的通用性。
* **多态性** － 组件的引用和类集会涉及到其它许多不同类型的组件，而且引用组件所产生的结果得依据实际调用的类型。

由于抽象性、封装性、重用性以及便于使用等方面的原因，以组件为基础的编程在脚本语言中已经变得特别流行。

## ORM

对象-关系映射（Object/Relation Mapping，简称ORM），是随着面向对象的软件开发方法发展而产生的。面向对象的开发方法是当今企业级应用开发环境中的主流开发方法，关系数据库是企业级应用环境中永久存放数据的主流数据存储系统。对象和关系数据是业务实体的两种表现形式，业务实体在内存中表现为对象，在数据库中表现为关系数据。内存中的对象之间存在关联和继承关系，而在数据库中，关系数据无法直接表达多对多关联和继承关系。因此，对象-关系映射\(ORM\)系统一般以中间件的形式存在，主要实现程序对象到关系数据库数据的映射。

面向对象是从软件工程基本原则（如耦合、聚合、封装）的基础上发展起来的，而关系数据库则是从数学理论发展而来的，两套理论存在显著的区别。为了解决这个不匹配的现象,对象关系映射技术应运而生。

## CURD

CURD是一个数据库技术中的缩写词，一般的项目开发的各种参数的基本功能都是CURD。它代表创建（Create）、更新（Update）、读取（Read）和删除（Delete）操作。CURD 定义了用于处理数据的基本原子操作。之所以将CURD 提升到一个技术难题的高度，是因为完成一个涉及在多个数据库系统中进行CURD操作的汇总相关的活动，其性能可能会随数据关系的变化而有非常大的差异。

CURD在具体的应用中并非一定使用create、update、read和delete字样的方法，但是他们完成的功能是一致的。例如，ThinkPHP就是使用add、save、select和delete方法表示模型的CURD操作。

## ActiveRecord

ActiveRecord也属于ORM层，由Rails最早提出，遵循标准的ORM模型：表映射到记录，记录映射到对象，字段映射到对象属性。配合遵循的命名和配置惯例，能够很大程度的快速实现模型的操作，而且简洁易懂。

ActiveRecord的主要思想是：

1. 每一个数据库表对应创建一个类，类的每一个对象实例对应于数据库中表的一行记录；通常表的每个字段在类中都有相应的Field；
2. ActiveRecord同时负责把自己持久化，在ActiveRecord中封装了对数据库的访问，即CURD;
3. ActiveRecord是一种领域模型\(Domain Model\)，封装了部分业务逻辑；

ActiveRecord比较适用于：

1. 业务逻辑比较简单，当你的类基本上和数据库中的表一一对应时, ActiveRecord是非常方便的，即你的业务逻辑大多数是对单表操作；
2. 当发生跨表的操作时, 往往会配合使用事务脚本\(Transaction Script\)，把跨表事务提升到事务脚本中；
3. ActiveRecord最大优点是简单, 直观。 一个类就包括了数据访问和业务逻辑. 如果配合代码生成器使用就更方便了；

   这些优点使ActiveRecord特别适合WEB快速开发。

## 单一入口

单一入口通常是指一个项目或者应用具有一个统一（但并不一定是唯一）的入口文件，也就是说项目的所有功能操作都是通过这个入口文件进行的，并且往往入口文件是第一步被执行的。

单一入口的好处是项目整体比较规范，因为同一个入口，往往其不同操作之间具有相同的规则。另外一个方面就是单一入口带来的好处是控制较为灵活，因为拦截方便了，类似如一些权限控制、用户登录方面的判断和操作可以统一处理了。

或者有些人会担心所有网站都通过一个入口文件进行访问，是否会造成太大的压力，其实这是杞人忧天的想法。
