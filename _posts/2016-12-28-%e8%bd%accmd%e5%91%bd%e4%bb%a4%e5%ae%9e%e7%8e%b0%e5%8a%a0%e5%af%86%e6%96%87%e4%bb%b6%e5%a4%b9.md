---
layout: post
title: '[转]cmd命令实现加密文件夹'
published: true
author: moli
comments: true
date: 2008-12-07 07:12:00
tags: [ ]
categories:
    - uncategorized
permalink: /archives/116.html
---
1、运行cmd 2、在cmd窗口中输入如下命令： md D:\test..\ (在D盘创建文件夹名为test.) D:\test.这个文件夹普通方式是无法打开的，不信自己可以试； 3、在运行中输入命令： d:\test..\ 即可以打开文件夹test.，并可以在文件夹内操作，和一般文件夹一样，记住，只能通过这个方式可以打开D盘的文件夹test.； 4、如果需要删除这个文件夹，请在cmd窗口中用命令： rd D:\test..\  建立真正隐藏的文件夹,放敏感资料 


  先说一下：不是attrib 的那种！ 大家都知道autorun.inf免疫的原理吧。 这里我也说略略说一下，方便新手学习下。



  其中用到了1个指令是 mkdir：意思是建立文件夹。 不明白的可以命令行下输入：mkdir /?



  原理如下：



  在驱动器根目录建立一个不可删除的文件夹，叫做"autorun.inf"，利用windows同目录文件不允许重名这个特点，使病毒无法写入autorun.inf ，破坏病毒的启动。就这么简单。



  举个例子，现在我们免疫d:盘，如下操作：



  1: 打开cmd窗口 2: d: 3: md autorun.inf (建立"autorun.inf"文件夹) 4: cd autorun.inf (进入"autorun.inf"文件夹) 5: md tiger..\ (创建不可删除的文件夹)



  这样子，d:盘里面会出现一个名为autorun.inf的文件夹，内有一个名为"tiger."的子文件夹,无法删除的。成功。 对于每一个驱动器，建议都免疫一下。 废话就说到这里。



  开始今天的正题： 上面说的无法删除的目录，可以用来放任何文件，就算里面放了文件，你看这个文件夹的大小，也是空的。呵呵！好像里面是没有文件的，真好。但毫无疑问，肯定是要占空间的。但是有个缺点就是，大家都看得见这个目录，如要特殊用途，就有点不方便了吧，嘿嘿，今天不小心发现了下面这种办法。不仅能放文件，而且看都看不见，我到目前为止都还不知道它到底放到哪里去了。



  在命令行下，在任何目录下，使用dir命令可以看见当前目录下的文件和文件夹。这个应该都知道。 你应该还看见了一个叫".."和一个叫"."的目录。 ".."目录代表上一级目录，"."代表本目录。



  但是这两个目录在图形界面模式下是不显示的，好了，很好，我的目标就是这个。 把上面的操作改成这样。



  1: 打开cmd窗口 2: d: 3: md tiger (建立"tiger"文件夹) 4: cd tiger (进入"tiger"文件夹) 5: md &#8230;\ (创建不可删除且隐藏的文件夹)



  (md ..\ 不行，我试过了=="拒绝访问")



  现在打开d盘，去tiger目录看看！发现什么了？呵呵！ 什么也没有吧！ 非常好！ 如何打开这个目录呢？ 开始->运行->输入"D:\tiger\&#8230;\"，就可以打开了，复制粘贴随便你吧！不管怎么样，可以保证两点：1：看不见；2：可以放东东。



  如何删除？ 先保证"D:\tiger\.."目录为空，如果不为空，先删除里面的文件。 然后如下操作即可：



  1: 打开cmd窗口 2: d: 4: cd tiger 5: rd e2e2~1



  (为什么是rd e2e2~1==>请看下文)



  原理分析： windows分为长文件名和短文件名。 比如你的C:\Program Files的短文件名就是：PROGRA~1 dir c:\ /x 就可以看见。



  /X 显示为非 8dot3 文件名产生的短名称。格式是 /N 的格式， 短名称插在长名称前面。如果没有短名称，在其位置则 显示空白。



  为什么说这个呢？ 其实windows目录中，我猜想是允许同目录下同名文件存在的。 只要短文件名不同就可以了，遗憾的是，我现在还没有找到设置短文件名的办法。 想法也没有办法测试。



  如果你进入D:\tiger\..目录 命令:"cd d:\tiger"&#8211;>cd e2e2~1(cd &#8230;\不行哦) 看见了吧!这里我们只能使用短文件名访问,也就是e2e2~1, 复制粘贴都可以的! 之所以能隐藏！原因是：对于长文件名为".."的目录，windows都将其隐藏，而没有判断短文件名，这是我们利用的地方，嘿嘿！很好！ 之所以可以放文件！因为这个东西本来就存在。 当然是不能删除的了！(除非用命令行&#8211;〉必须使用短文件名) 大家如果要编程实现！只要注意e2e2~1就可以了！其他随便玩！