# 第9章 Swing用户界面组件
	本章内容：
	* Swing与模型-视图-控制器设计模式
	* 布局管理概述
	* 文本输入
	* 选择组件
	* 菜单
	* 复杂的布局管理
	* 对话框

1. 本章将介绍构造功能更加齐全的图形用户界面（GUI）所需要的一些重要工具。

## 9.1 Swing和模型-视图-控制器设计模式

#### 9.1.1 设计模式
1. 容器和组件是“组合（composite）”模式。带滚动条的面板是“装饰器（decorator）”模式。布局管理器是“策略（strategy）”模式。

#### 9.1.2 模型-视图-控制器模式
1. 每个组件都有三个要素：
* 内容，如：按钮的状态（是否按下），或者文本框的文本。
* 外观（颜色，大小等）。
* 行为（对事件的反应）。
2. 面向对象设计中的一个基本原则：限制一个对象拥有的功能数量
3. 模型-视图-控制器（MVC）模式告诉我们如何实现这种设计，实现三个独立的类：
* 模型（model）：存储内容。
* 视图（view）：显示内容。
* 控制器（controller）：处理用户输入。
模式必须实现改变内容和查找内容的方法。主机：模型是完全不可见的。显示存储在模型中的数据是视图的工作。
4. 模型-视图-控制器模式的一个优点是一个模型可以有多个视图，其中每个视图可以显示全部内容或不同形式。
5. 控制器负责处理用户输入事件。然后决定是否把这些事件转化成对模型或视图的改变。

#### 9.1.3 Swing按钮的模型-视图-控制器分析
1. 对于大多数组件来说，模型类将实现一个名字以Model结尾的接口。实现了此接口的类可以定义各个按钮的状态。
2. ButtonModel接口的属性
| 属性名 | 值 |
|--------|--------|
|actionCommand|与按钮关联的动作命令字符串|
|mnemonic|按钮的快捷键|
|armed|如果按钮被按下且鼠标仍在按钮上则为true|
|enabled|如果按钮是可选择的则为true|
|pressed|如果按钮被按下且鼠标按键没有释放则为true|
|rollover|如果鼠标在按钮之上则为true|
|selected|如果按钮已经被选择（用于复选框和单选按钮）则为true|
3. 模型不存储按钮标签或者图标。
4. 同样的模型（即DefaultButtonModel）可用于下压按钮、单选按钮、复选框、甚至是菜单项。当然，这些按钮都有各自不同的视图和控制器。
5. 通常，每个Swing组件都有一个相关的后缀为UI的视图对象，但并不是所有的Swing组件都有专门的控制器对象。
6. JButton究竟是什么？事实上，它仅仅是一个继承了JComponent的包装器类，JComponent包含了一个DefaultButtonModel对象，一些视图数据（例如按钮标签和图标）和一个负责按钮视图的BasicButtonUI对象。

## 9.2 布局管理概述
1. 组件放置在容器中，布局管理器决定容器中的组件具体放置的位置和大小。
2. 文本域和其他的用户界面元素都继承于Component类，组件可以放置在面板这样的容器中。由于Container类继承于Conponent类，所以容器也可以放置在另一个容器中。
3. 继承层次有两点显得有点混乱。首先，像JFrame这样的顶层窗口是Container的子类，所以也是Component的子类，但却不能放在其他容器内。另外，JComponent是Container的子类，但不直接继承Component，因此，可以将其他组件添置到JButton中。（但无论如何，这些组件无法显示出来）。
4. 每个容器都有一个默认的布局管理器，但可以重新进行设置。
5. java.awt.Container 1.0
	* Void SetLayout(LayoutManager m)
	为容器设置布局管理器。
	* Component add(Component c)
	* Component add(Component c,Object constraints) 1.1
	将组件添加到容器中，并返回组件的引用。
	参数：c 要添加的组件。constraints 布局管理器理解的标识符。
6. java.awt.FlowLayout 1.0
	* FlowLayout()
	* FlowLayout(int align)
	* FlowLayout(int align,int hgap,int vgap)
	构造一个新的FlowLayout对象。
	参数：align LEFT、CENTER或者RIGHT。hgap 以像素为单位的水平间距（如果是负值，则强行重叠）。vgap 以像素为单位的垂直间距（如果为负值，则强行重叠）。

#### 9.2.1 边框布局
1. 边框布局管理器（border layout manager）是每个JFrame的内容窗格的默认布局管理器。流布局管理器完全控制每个组件的放置位置，边框布局管理器则不然，它允许为每个组件选择一个放置位置。可以选择把组件放在内部窗格的中部、北部、南部、东部或者西部。
2. 当容器被缩放时，边缘组件的尺寸没有改变，而中部组件的大小会发生变化。在添加组件时可以指定BorderLayout类中的CENTER、NORTH、SOUTH、EAST和WEST常量。并非需要占有所有的位置，如果没有提供任何值，系统默认为CENTER。
3. 与流布局不用，边框布局会扩展所有组件的尺寸以便填充满可用空间（刘布局将维持每个组件的最佳尺寸）。
4. java.awt.BorderLayout 1.0
	* BorderLayout()
	* BorderLayout(int hgap,int vgap)
	构造一个新的BorderLayout对象。
	参数：hgap 以像素为单位的水平间距（如果为负值，则强行重叠），vgap 以像素为单位的垂直间距（如果为负值，则情形重叠）。

#### 9.2.2 网格布局
1. 网格布局像电子数据表一样，按行列排列所有的组件。不过，它的每个单元大小都是一样的。
2. java.awt.GridLayout 1.0
	* GridLayout(int rows,int columns)
	* GridLayout(int rows,int columns,int hgap,int vgap)
	构造一个新的GridLayout对象。row或者columns可以为零，但不能同时为零，指定的每行或每列的组件数量可以任意的。
	参数：rows 网格的行数，columns 网格的列数，hgap 以像素为单位的水平间距（如果为负值，则强行重叠），vgap 以像素为单位的垂直间距（如果为负值，则强行重叠）。

## 9.3 文本输入
1. 文本域（JTextField）和文本区（JTextArea）组件用于获取文本输入。文本域只能接收单行文本的输入，而文本区能够接收多行文本的输入。JPassword也只能接收单行文本的输入，但不会将输入的内容显示出来。
这三个类都继承于JTextComponent类。由于JTextComponent是一个抽象类，所以不能够构造这个类的对象。
2. javax.swing.text.JTextComponent 1.2
	* String getText()
	* void setText(String text)
	获取或设置文本组件中的文本。
	* boolean isEditable()
	* void setEditable(boolean b)
	获取或设置editable特性，这个特性决定了用户是否可以编辑文本组件中的内容。

#### 9.3.1 文本域
1. 把文本域添加到窗口的常用方法是将它添加到面板或者其他容器中，这与添加按钮完全一样。
2. 列数只是给AWT设定首选（preferred）大小的一个提示。如果布局管理器需要缩放这个文本域，它会调整文本域的大小。
3. 使用setColumns方法改变了一个文本域的大小之后，需要调用包含这个文本框的容器的revalidate方法。revalidate方法会重新计算容器内所有组件的大小，并且对它们重新进行布局。调用revalidate方法之后，布局管理器会重新设置容器的大小，然后就可以看到改变尺寸后的文本域了。
revalidate方法是JComponent类中的方法。它并不是马上就改变组件大小，而是给这个组件加一个需要改变大小的标记。这样就避免了多个组件改变大小时带来的重复计算。但是，如果想重新计算一个JFrame中的所有组件，就必须调用validate方法--JFrame没有扩展JComponent。
4. javax.swing.JTextField 1.2
	* JTextField(int cols)
	构造一个给定列数的空JTextField对象。
	* JTextField(String text,int cols)
	构造一个给定列数、给定初始字符串的JTextField对象。
	* int getColumns()
	* void setColumns(int cols)
	获取或设置文本域使用的列数。
5. javax.swing.JComponent 1.2
	* void revalidate()
	重新计算组件的位置和大小。
	* void setFont(Font f)
	设置组件的字体。
6. java.awt.Component 1.0
	* void validate()
	重新计算组件的位置和大小。如果组件是容器，容器中包含的所有组件的位置和大小也被重新计算。
	* Font getFont()
	获取组件的字体。

#### 9.3.2 标签和标签组件
1. 标签是容纳文本的组件，他们没有任何的修饰（例如没有边缘），也不能响应用户输入。可以利用标签标识组件。
2. 要想用标识符标识不带标签的组件，应该
1）用相应的文本构造一个JLabel组件。
2）将标签组件放置在距离需要标识的组件足够近的地方，以便用户可以知道标签所标识的组件。
3. JLabel的构造器允许指定初始文本和图标，也可以选择内容的排列方式。可以用Swing Constants接口中常量来指定排列方式。
4. 包含HTML标签的第一个组件需要延迟一段时间才能显示出来，这是因为需要加载相当复杂的HTML显示代码。
5. 标签也可以放置在容器中。
6. java.swing.JLabel 1.2
	* JLabel(String text)
	* JLabel(Icon icon)
	* JLabel(String text,int align)
	* JLabel(String text,Icon icon,int align)
	构造一个标签。
	参数：text 标签中的文本，icon 标签中的图标，align 一个SwingConstants的常量LEFT（默认）、CENTER或 RIGHT。
	* String getText()
	* void setText(String text)
	获取或设置标签的文本。
	* Icon getIcon()
	* void setIcon(Icon icon)
	获取或设置标签的图标。

#### 9.3.3 密码域
1. 密码域是一种特殊类型的文本域。为了避免有不良企图的人看到密码，用户输入的字符不显示出来。每个输入的字符都有回显字符（echo character）表示，典型的回显字符是星号（*）。Swing提供了JPasswordField类来实现这样的文本域。
2. javax.swing.JPasswordField 1.2
	* JPasswordField(String text,int columns)
	构造一个新的密码域对象。
	* void setEchoChar(char echo)
	为密码域设置回显字符。注意：独特的观感可以选择自己的回显字符。0表示重新设置为默认的回显字符。
	* char[] getPassword()
	返回密码域中的文本。为了安全起见，在使用之后应该覆写返回的数组内容（密码并不是以String的形式返回，这是因为字符串在被垃圾回收器回收之前会一直驻留在虚拟机中）。

#### 9.3.4 文本区
1. 用户并不受限于输入执行的行数和列数。当输入过长时，文本会滚动。
2. 如果文本区的文本超出线束的范围，那么剩下的文本就会被裁减掉。可以通过开启换行特性来避免裁剪过长的行。

#### 9.3.5 滚动窗格
1. 在Swing中，文本区没有滚动条。如果需要滚动条，可以将文本区插入到滚动窗格（scroll pane）中。
2. JTextArea组件只显示无格式的文本，没有特殊字体或者格式设置。如果想要显示格式化文本（如HTML），就需要使用JEditorOane类。
3. javax.swing.JTextArea 1.2
	* JTextArea()
	* JTextArea(int rows,int cols)
	* JTextArea(String text,int rows,int cols)
	构造一个新的文本对象。
	* void setColumns(int cols)
	设置文本区应该使用的首选列数。
	* void setRows(int rows)
	设置文本区应该使用的首选行数。
	* void append(String newText)
	将给定的文本追加到文本区中已有文本的尾部。
	* void setLineWrap(boolean wrap)
	打开或关闭换行。
	* void setWrapStyleWord(boolean word)
	如果word是true，超长的行会在自边框处换行。如果为false，超长的行被截断而不考虑字边框。
	* void setTabSize(int c)
	将制表符（tab stop）设置为c列。注意，制表符不会被转换为空格，但可以让文本对齐到下一个制表符处。
4. javax.swing.JScrollPane 1.2
	* JScrollPane(Component c)
	创建一个滚动窗格，用来显示指定组件的内容。当组件内容超过显示范围时，滚动条会自动地出现。

## 9.4 选择组件

#### 9.4.1 复选框
1. 复选框自动地带有标识标签。用户通过点击某个复选框来选择相应的选项，再点击则取消选取。当复选框获得焦点时，用户也可以通过按空格键来切换选择。
2. javax.swing.JCheckBox 1.2
	* JCheckBox(String label)
	* JCheckBox(String label,Icon icon)
	构造一个复选框，初始没有被选择。
	* JCheckBox(String label,boolean state)
	用给定的标签和初始化状态构造一个复选框。
	* boolean isSelected()
	* void setSelected(boolean state)
	获取或设置复选框的选择状态。

#### 9.4.2 单选按钮
1. 当用户选择另一项的时候，前一项就自动地取消选择。这样一组选框通常称为单选按钮组（Radio Button Group）。
2. javax.swing.JRadioButton 1.2
	* JRadioButton(String label,Icon icon)
	构造一个单选按钮，初始没有被选择。
	* JRadioButton(String label,boolean state)
	用给定的标签和初始状态构造一个单选按钮。
3. javax.swing.ButtonGroup 1.2
	* void add(AbstractButton b)
	将按钮添加到组中。
	* ButtonModel getSelection()
	返回被选择的按钮的按钮模型。
4. javax.swing.ButtonModel 1.2
	* String getActionCommand()
	返回按钮模型的动作命令。
5. javax.swing.AbstractButton 1.2
	* void setActionCommand(String s)
	设置按钮以及模型的动作命令。

#### 9.4.3 边框
1. 如果在一个窗口中有多组单选按钮，就需要用可视化的形式指明哪些按钮属于同一组。Swing提供了一组很有用的边框（borders）来解决这个问题。
有几种不同的边框可供选择。
1）调用BorderFactory的静态方法创建边框。可选的分格：凹斜面、凸斜面、蚀刻、直线、蒙版。
2）如果愿意的话，可以给边框添加标题，具体的实现方法是将边框传递给：BorderFactory.createTitledBorder。
3）如果确实想把一切凸显出来，可以将几种边框组合起来使用：BorderFactory.createCompoundBorder。
4）调用JComponent类中setBorder方法将结果边框添加到组件中。
2. 不同的边框有不同的用于设置边框的宽度和颜色的选项。
3. javax.swing.BorderFactory 1.2
	* static Border createLineBorder(Color color)
	* static Border createLineBorder(Color color,int thickness)
	创建一个简单的直线边框。
	* static MatteBorder createMatteBorder(int top,int left,int bottom,int right,Color color)
	* static MatteBorder createMatteBorder(int top,int left,int bottom,int right,Icon tileIcon)
	创建一个用color颜色或一个重复（repeating）图标填充的粗的边框。
	* static Border createEmptyBorder()
	* static Border createEmptyBorder(int top,int left,int bottom,int right)
	创建一个空边框。
	* static Border createEtchedBorder()
	* static Border createEtchedBorder(Color highlight,Color shadow)
	* static Border createEtcheBorder(int type)
	* static Border createEtchedBorder(int type,Color highlight,Color shadow)
	创建一个具有3D效果的直线边框。
	参数：highlight,shadow 用于3D效果的颜色， type EtchedBorder.RAISED和EtchedBorder.LOWERED之一。
	* static Border createBevelBorder(int type)
	* static Border createBevelBorder(int type,Color highlight,Color shadow)
	* static Border createLoweredBevelBorder()
	* static Border createRaisedBevelBorder()
	创建一个具有凹面或凸面的边框。
    参数：type BevelBorder.LOWERED和BevelBorder.RAISED之一。highlight,shadow 用于3D效果的颜色。
    * static TitledBorder createTitledBorder(String title)
    * static TitledBorder createTitledBorder(Border border)
    * static TitledBorder createTitledBorder(Border border,String title)
    * static TitledBorder createTitledBorder(Border border,String title,int justification,int position)
    * static TitledBorder createTitledBorder(Border border,String title,int justification,int position)
    * static TitledBorder createTitledBorder(Border border,String title,int justification,int position,Font font)
    * static TitledBorder createTitledBorder(Border border,String title,int justification,int position,Font font ,Color color)
    创建一个具有给定特性的带标题的边框。
    参数：title 标题字符串。border 用标题装饰的边框。 justification TitledBorder常量LEFT、CENTER、RIGHT、LEADING、trAILING或DEFAULT_JUSTIFICATION(left)之一。position TitledBorder常量ABOVE_TOP、TOP、BELOW_TOP、ABOVE_BOTTOM、BOTTOM、BELOW_BOTTOM或DEFAULT_POSITION(top)之一。font 标题的字体。color 标题的颜色。
    * static CompoundBorder createCompoundBorder(Border outSideBorder，Border indideBorder)
    将两个边框组合成一个新的边框。
4. javax.swing.border.SoftBevelBorder 1.2
	* SoftBevelBorder(int type)
	* SodtBevelBorder(int type,Color highliht,Color shadow)
	创建一个带有柔和边角的斜面边框。
    参数：type BevelBorder.LOWERED和BevelBorder.RAISED之一。highlight,shadow 用于3D效果的颜色。
5. javax.swing.border.LineBorder 1.2
	* public LineBorder(Color color,int thickness,boolean roundedCorners)
	用指定的颜色和粗细创建一个直线边框。
6. javax.swing.JComponent 1.2
	* void setBorder(Border border)
	设置这个组件的边框。

#### 9.4.4 组合框
1. 如果下拉列表框被设置成可编辑（editable），就可以像编辑文本一样编辑当前的选项内容。鉴于这个原因，这种组合被称为组合框（combo box），它将文本域的灵活性与一组预定义的选项组合起来。JComboBox类提供了组合框的组件。
2. 调用setEditable方法可以让组合框可编辑。注意，编辑只会影响当前项，而不会改变列表内容。
3. 可以调用getSelectedItem方法获取当前的选项，如果组和框是可编辑的，当前选项则是可以编辑的。不过，对于可编辑组合框，其中的选项可以是任何类型，这取决于编辑器。
4. 当用户从组合框中选择一个选项时，组合框就将产生一个动作事件。为了判断哪个选项被选择，可以通过事件参数调用getSource方法来得到发送事件的组合框引用，接着调用getSelectedItem方法获取当前选择的选项。需要把这个方法的返回值转化为相应的类型，通常是String型。
5. javax.swing.JComboBox 1.2
	* boolean isEditable()
	* void setEditable(boolean b)
	获取或设置组合框的可编辑特性。
    * void addItem(Object item)
    把一个选项添加到选项列表中。
    * void insertItemAt(Object item,int index)
    将一个选项添加到选项列表的指定位置。
    * void removeItem(Object item)
    从选项列表中删除一个选项。 
    * void removeItemAt(int index)
    删除指定位置的选项。
    * void removeAllItems()
    从选项列表中删除所有选项。
    * Object getSelectedItem()
    返回当前选择的选项。

#### 9.4.5 滑动条
1. 组合框可以让用户从一组离散值中进行选择。滑动条允许进行连续值得选择。
2. 当用户滑动滑动条时，滑动条的值就会在最小值和最大值之间变化。当值发生变化时，ChangeEvent就会发送给所有变化的监听器。为了得到这些改变的通知，需要条用addChangeListener方法并且安装一个实现了ChangeListener接口的对象。这个接口只有一个方法StateChanged。在这个方法中，可以获取滑动条的当前值。
3. 可以通过显示标尺（tick）对滑动条进行修饰。
4. 可以通过下列方法为达标吃添加标尺标记标签（tick mark labels）：`slider.setPaintLabels(true);`。
5. 如果标尺的标记或者标签不显示，请检查一下是否调用了setPaintTicks(true)和setPaintLabels(true)。
6. 要想影藏滑动条移动的轨迹，可以调用：`slider.setPaintTrack(false)`。
7. javax.swing.JSlider 1.2
	* JSlider()
	* JSlider(int direction)
	* JSlider(int min,int max)
	* JSlider(int min,int max,int initialValue)
	* JSlider(int direction,int min,int max,int initialValue)
	用给定的方向、最大值、最小值和初始化值构造一个水平滑动条。
    参数：direction SwingConstants.HORIZONTAL或SwingConstants.VERTICAL之一。默认为水平。 mi,max 滑动条的最大值、最小值。默认值为0到100。 initialValue 滑动套的初始化值。默认值为50。
    * void setPaintTicks(boolean b)
    如果b为true，显示标尺。
    * void setMajorTickSpacing(int units)
    * void setMinorTickSpacing(int units)
    用给定的滑动条单位的倍数设置大标尺和小标尺。
    * void setPaintLabels(boolean b)
    如果b是true，显示标尺标签。
    * void setLabelTable(Dictionary table)
    设置用于作为标尺标签的组件。表中的每一个键/值对都采用new Integer(value)/component的格式。
    * void setSnapToTicks(boolean b)
    如果b是true，每一次调整后滑块都要对齐到最接近的标尺处。
    * void setPaintTrack(boolean b)
    如果b是true，显示滑动条滑动的轨迹。

## 9.5 菜单
1. 位于窗口顶部的菜单栏（menu bar）包括了下拉菜单的名字。点击一个名字就可以打开包含菜单项（menu items）和子菜单（submenus）的菜单。当用户点击菜单项时，所有的菜单都会被关闭并且将一条消息发送给程序。

#### 9.5.1 菜单创建
1. 在通常情况下，菜单项触发的命令也可以通过其他用户界面元素（如工具栏上的按钮）激活。
2. javax.swing.JMenu
	* JMenu(String labe)
    用给定标签构造一个菜单。
    * JMenuItem add(JMenuItem item)
    添加一个菜单项（或一个菜单）。
    * JMenuItem add(String label)
    用给定标签将一个菜单项添加到菜单中，并返回这个菜单项。
    * JMenuItem add(Action a)
    用给定动作将一个菜单项添加到菜单中，并返回这个菜单项。
    * void addSeparator()
    将一个分隔符行（separator line）添加到菜单中。
    * JMenuItem insert(JMenuItem menu,int index)
    将一个新菜单项（或子菜单）添加到菜单的指定位置。
    * JMenuItem insert(JMenuItem menu,int index)
    将一个新菜单项（或子菜单）添加到菜单的指定位置。
    * JMenuItem insert(Action a,int index)
    用给定动作在菜单的指定位置添加一个新菜单项。
    * void insertSeparator(int index)
    将一个分隔符添加到菜单中。
    参数：index 添加分隔符的位置。
    * void remove(int index)
    * void remove(JMenuItem item)
    从菜单中删除指定的菜单项。
3. javax.swing.JMenuItem 1.2
	* JMenuItem(String label)
	用给定标签构造一个菜单项。
    * JMenuItem(Acton a) 1.3
    为给定动作构造一个菜单项。
4. javax.swing.AbstractButton 1.2
	* void setAction(Action a) 1.3
	为这个按钮或菜单项设置动作。
5. javax.swing.JFrame 1.2
	* void setJMenuBar(JMenuBar menubar)
	为这个框架设置菜单栏。

#### 9.5.2 菜单项中的图标
1. JMenuItem类扩展了AbstractButton类。与按钮一样，菜单可以包含文本标签、图标，也可以两者都包含。既可以利用JMenuItem(String,Icon)或者JMenuItem(Icon)构造器为菜单指定一个图标，也可以利用JmenuItem类中的setIcon方法（继承自AbstractButton类）指定一个图标。
2. javax.swing.JMenuItem 1.2
	* JMenuItem(String label,Icon icon)
	用给定的标签和图标构造一个菜单项。
3. javax.swing.AbstractButton 1.2
	* void setHorizontalTextPosition(int pos)
	设置文本对应图标的水平位置。
    参数：pos SwingConstants.RIGHT（文本在图标的右侧）或SwingConstants.LEFT。
4. javax.swing.AbstractAction 1.2
	* AbstractAction(String name,Icon smallIcon)
	用给定的名字和图标构造一个抽象的动作。

#### 9.5.3 复选框和单选按钮菜单项
1. 复选框和单选按钮菜单项在文本旁边显示了一个复选框或一个单选按钮。当用户选择一个菜单框时，菜单项就会自动地在选择和未选择间进行切换。
2. javax.swing.JCheckBoxMenuItem 1.2
	* JCheckBoxMenuItem(String label)
	用给定的标签构造一个复选框菜单项。
    * JCheckBoxMenuItem(String label,boolean state)
    用给定的标签个给定的初始状态（true为选定）构造一个复选框菜单。
3. javax.swing.JRadioButtonMenuItem 1.2
	* JRadioButtonMenuItem(String label)
	用给定的标签构造一个单选按钮菜单项。
    * JRadioButtonMenuItem(String label,boolean state)
    用给定的标签和给定的初始状态（true为选定）构造一个单选按钮菜单项。
4. javax.swing.AbstractButton 1.2
	* boolean isSelected()
	* void setSelected(boolean state)
	获取或设置这个菜单项的选择状态（true为选定）。

#### 9.5.4 弹出菜单
1. 弹出菜单（pop-up menu）是不固定在菜单栏中随处浮动的菜单。
2. 通常，当用户点击某个鼠标键时弹出菜单。这就是所谓的弹出式触发器（pop-up trigger）。在Window或者Linux中，弹出式触发器是鼠标右键。
3. javax.swing.JPopupMenu 1.2
	* void show(Component c,int x,int y)
	显示一个弹出菜单。
    参数：c 显示弹出菜单的组件。 x,y 弹出菜单左上角的坐标（c的坐标空间内）。
    * boolean isPopupTrigger(MouseEvent event) 1.3
	如果鼠标事件是弹出菜单触发器，则返回true。
4. java.awt.event.MouseEvent 1.1
	* boolean isPopupTrigger()
	如果鼠标事件是弹出菜单触发器，则返回true。
5. javax.swing.JComponent 1.2
	* JPopupMenu getComponentPopupMenu() 5.0
	* void setComponentPopupMenu(JPopupMenu popup)
	获取或设置用于这个组件的弹出菜单。
    * boolean getInheritsPopupMenu() 5.0
    * void setInheritsPopupMenu(boolean b) 5.0
    获取或设置inheritsPopupMenu特性。如果这个特性被设置或这个组件的弹出菜单为null，则应用上一级弹出菜单。

#### 9.5.5 快捷键和加速器
1. 可以通过在菜单项的构造器中指定一个快捷字母来为菜单项设置快捷键。
2. 快捷键会自动地显示在菜单项中，并带有一条下划线。
3. 在Java SE 1.4中，可以调用setDiaplayedMnemonicIndex方法指定希望加下划线的字符。
4. 如果有一个Action对象，就可以把快捷键作为Action.MNEMONIC_KEY的键值添加到对象中。
5. 只能在菜单项的构造器中设定快捷键祖母，而不是在菜单构造器中。如果想为菜单设置快捷键，需要调用setMnemonic方法。
6. 可以同时按下ALT键和菜单的快捷键来实现在菜单栏中选择一个顶层菜单的操作。
7. 可以使用快捷键从当前打开的菜单中选择一个子菜单或者菜单项。而加速器是在不打开菜单的情况下选择菜单项的快捷键。
8. 当用户按下加速器组合键时，就会自动地选择相应的菜单项，同时激活一个动作事件，这与手动地选择这个菜单项一样。
9. 加速器只能关联到菜单项上，不能关联到菜单上。加速器键并不实际打开菜单。它将直接地激活菜单关联的动作事件。
10. javax.swing.HMenuItem 1.2
	* JMenuItem(String label, int mnemonic)
	用给定的标签和快捷键字符构成一个菜单项。
    参数：label 菜单项的标签。 mnemonic 菜单项的快捷键字符，在标签中这个字符下面会有一个下划线。
    * void setAccelerator(KeyStroke k)
    将k设置为这个菜单项的加速器。加速器显示在标签的旁边。
11. javax.swing.AbstractButton 1.2
	* void setMnemonic(int mnemonic)
	设置按钮的快捷字符。该字符会在标签中以下划线的形式显示。
    * void setDisplayedMnemonicIndex(int index) 1.4
    将按钮文本中的index字符设定为带下划线。如果不希望第一个出现的快捷键字符带下划线，就可以使用这个方法。

#### 9.5.6 启用和禁用菜单项
1. 启动和禁用菜单项有两种策略。每次环境发生变化就对相关的菜单项或动作调用setEnabled。另一种方式是在现实菜单之前禁用这些菜单栏。
2. javax.swing.JMenuItem 1.2
	* void setEnabled(boolean b)
	启用或禁用菜单项。
3. javax.swing.event.MenuListener 1.2
	 * void menuSelected(MenuEvent e)
	 在菜单被选择但尚未打开之前调用。
     * void menuDeselected(MenuEvent e)
     在菜单被取消选择并且已经关闭之后被调用。
     * void menuCanceled(MenuEvent e)
     当菜单被取消时被调用。例如，用户单击菜单以外的区域。

#### 9.5.7 工具栏
1. 工具栏是在程序中提供的快速访问常用命令的按钮栏。
2. 工具栏的特殊之处在于可以将它随处移动。可以将它拖拽到框架的四个边框上。
3. 工具栏只有位于采用边框布局或者任何支持North、East、South和Weat约束布局管理器的容器内才能够被拖拽。
4. 工具栏可以完全脱离框架。这样的工具栏将包含在自己的框架中。当关闭包含工具栏的框架时，它会回到原始的框架中。

#### 9.5.8 工具提示
1. 当光标停留在某个按钮上片刻时，工具提示就会被激活。工具提示文本显示在一个有颜色的矩形里。当用户移开鼠标时，工具提示就会自动地消失。
2. javax.swing.JToolBar 1.2
	* JToolBar()
	* JToolBar(String titleString)
	* JToolBar(int orientation)
	* JToolBar(String titleString,int orientation)
	用给定的标题字符串和方向构造一个工具栏。Orientation可以是SwingConstants.HORIZONTAL(默认)或SwingConstants.VERTICAL。
    * JButton add(Action a)
    用给定的动作名、图标、简要的说明和动作回调构造一个工具栏中的新按钮。
    * void addSeparator()
    将一个分隔符添加到工具栏的尾部。
3. javax.swing.JComponent 1.2
	* void setToolTipText(String text)
	设置当鼠标停留在组件上时显示在工具提示中的文本。

## 9.6 复杂的布局管理
1. Java布局管理器是一种用于组件布局的好方法。应用布局管理器，布局就可以使用组件间关系的指令来完成布局操作。

#### 9.6.1 网格组布局
1. 网格足布局是所有布局管理器之母。可以将网格组布局看成是没有任何限制的网格布局。在网格组布局中，行和列的尺寸可以改变。可以将相邻的单元合并以适应较大的组件（很多字处理器以及HTML都利用这个功能编辑表格：一旦需要就合并相邻的单元格）。组件不需要填充这个单元格区域，并可以指定它们在单元格内的对齐方式。
2. 要想使用网格组管理器进行布局，必须经过下列过程：
* 建立一个GridBagLayout的对象。不需要指定网格的行数和列数。布局管理器会根据后面所给的信息猜测出来。
* 将GridBagLayout对象设置成组件的布局管理器。
* 为每个组件建立一个GridBagConstraints对象。设置GridBagConstraints对象的域以便指出组件在网格组中的布局方案。
* 最后，通过下面的调用添加组件的约束：`add(component,constraints);`。
3. GridBagConstraints对象的几个最重要的约束
（1）gridx、girdy、gridwidth和gridheight参数
这些约束定义了组件在网格中的位置。gridx和girdy指定了被添加组件左上角的行、列位置。gridwidth和gridheight制定了组件占据的行数和列数。
（2）增量域
在网格布局中，需要为每个区域设置增量域（weightx和weighty）。如果将增量设置为0，则这个区域将永远为初始尺寸。如果将所有区域的增量都设置为0，容器就会集聚在为它分配的区域中间，而不是通过拉伸来填充它。
从概念上讲，增量参数属于行和列的属性，而不属于某个单独的单元格。但却需要在单元格上指定它们，这是因为网格组布局并不暴露行和列。行和列的增量等于每行或每列单元格的增量最大值。因此，如果想让一行或一列的大小保持不变，就需要将这行、这列的所有组件的增量都设置为0。
注意，增量并不实际给出列的相对大小。当容器超过首选大小时，增量表示分配给每个区域的扩展比例值。
（3）fill和anchor参数
如果不希望组件拉伸至整个区域，就需要设置fill约束。它有四个有效值：GridBagConstraints.NONE、GridBagConstraints.HORIZONTAL、GridBagConstraints.VERTICAL和GridBagConstraints.BOTH。
（4）填充
可以通过设置GridBagLayout的insets域在组件周围增加附件的空白区域。通过设置Insets对象的left、top、right和bottom指定组件周围的空间量。这被称为外部填充（或外边距）（external padding）。
通过设置ipadx和ipady指定内部填充（或内外距）（internal padding）。这两个值被加到组件的最小宽度和最小高度上。这样可以保证组件不会收缩至最小尺寸之下。
（5）指定gridx,gridy,gridwidth和gridheight参数的另一种方法
需要通过为gridheight和gridwidth域指定一个适当的值来设置组件横跨的行数和列数。除此之外，如果组件扩展至最后一行或最后一列，则不要给出一个实际的数值，而是用常量GridBagConstraints.REMAINDER替代，这样会告诉布局管理器这个组件时本行上的最后一个组件。
（6）使用帮助类来管理（tame）网格组约束
4. java.awt.GridBagConstraints 1.0
	* int girdx,girdy
	指定单元格的起始行和列。默认值为0.
   	* int gridwidth,girdheight
   	指定单元格的行和列的范围。默认值为1.
    * double weightx,weighty
    指定单元格扩大时的容量。默认值为0.
    * int anchor
    表示组件在单元格内的对齐方式。可以选择的绝对位置有：NORTHWEAT、NORTH、NORTHEAST、WEST、CENTER、EAST、SOUTHWEST、SOUTH、SPUTHEAS。或者各个方向上的相对位置：FIRST_LINE_START、LINE_START、FIRST_LINE_END、PAUSE_START、CENTER、PAGE_END、LAST_LINE_START、LINE_END、LAST_LINE_END。如果应用程序有可能从右向左，或者自顶至底排列文本，就应该使用后者。默认值为CENTER。
    * int fill
    指定组件在单元格内的填充行为，取值为NONE、BOTH、HORIZONTAL，或者VERTICAL。默认值为NONE。
    * int ipadx,ipady
    指定组件周围的“内部”填充。默认值为0。
    * Insets insets
    指定组件边框周围的“外部”填充。默认为不填充。
    * GridBagConstraints(int gridx,int gridy,int girdwidth,int gridheight,double weightx,double weighty,int anchor,int fill,Insets insets,int ipadx,int ipady) 1.2
    用参数中给定的多有域值构造GridBagConstains。Sun建议这个构造器只用在自动代码生成器只用在自动生成器中，因为它会使得代码非常难于阅读。

#### 9.6.2 组布局
1. javax.swing.GroupLayout 6
	* GroupLayout(Container host)
	构造一个GroupLayout对象，用于布局host容器中的组件（注意：host容器仍然需要调用setLayout）。
    * void setHorizontalGroup(GroupLayout.Group g)
    * void setVerticalGroup（GroupLayout.Group g）
    设置用于控制水平或垂直容器的组。
    * void linkSize(Component... components)
    * void linkSize(int axis,Component... components)
    强制给定的几个组件具有相同的尺寸，或者在指定的坐标轴上有相同的尺寸（SwingConstants.HORIZONTAL或者SwingContants.VERTICAL）。
    * GroupLayout.SequentialGroup createSequentialGroup()
    创建一个组，用于并行地布局子组件。
    * GroupLayout.ParallelGroup createParallelGroup()
    * GroupLayout.ParallerGroup createParallerGroup(GroupLayout.Alignment align)
    * GroupLayout.ParallelGroup createParallelGroup(GroupLayout.Alignment align,boolean resizable)
    创建一个组，用于并行地布局子组件。
    参数：align BASELINE、LEADING(默认值)、TRAILING或CENTER。resizable 如果组可以调整大小，这个值为true(默认值)；如果首选的尺寸是最小尺寸或最大尺寸，这个值为false。
    * boolean getHonorsvisibility()
    * void setHonorsvisibility(boolean b)
    获取或设置honorsVisibility特性。当这个值为true(默认值)时，不可见的组件不参与布局。当这个值为false时，好像可见的组件一样，这些组件也参与布局。这个特性主要用于想要临时影藏一些组件，而又不希望改变布局的情况。
    * boolean getAutoCreateCaps()
    * void setAutoCreateaps(boolean b)
    * boolean getAutoCreateContainerCaps()
    * void setAutoCreateContainerCaps(boolean b)
    获取或设置autoCreateCaps和autoCreateContainerCaps特性。当这个值为true时，将自动地在组件或容器边框之间添加空隙。默认值是false。在手工地构造GroupLayout时，true值很有用。
2. javax.swing.GroupLayout.Group
	* GroupLayout.Group addComponent(Component c)
	* GroupLayout.Group addComponent(Component c,int minimumSize,int preferredSize,int maximumSize)
	添加一个组件至本组中。尺寸参数可以是一个实际值（非负值），或者是一个特定的常量GroupLayout.DEFAULT_SIZE或GroupLayout.PREFERRED_SIZE。当使用DEFAULT_SIZE，将调用组件的getMinimumSize、getPreferredSize或getMaximumSize。当使用PREFERRED_SIZE时，将调用组件的getPreferredSize方法。
    * GroupLayout.Group addCap(int size)
    * GroupLayout.Group addCap(int minimumSize,int preferredSize,int maximumSize)
    添加一个固定的或可调节的空隙。
    * GroupLayout.Group addGroup(GroupLayout.Group g)
    将一个给定的组添加到本组中。
3. javax.swing.GroupLayout.ParallelGroup
	* GroupLayout.ParallelGroup addComponent(Component c,GroupLayout.Alignment align)
	* GroupLayout.ParallelGroup addComponent(Component c,GroupLayout.Alignment align,int minimumSize,int preferredSize,int maximumSize)
	* GroupLayout.ParallelGroup addGroup(GroupLayout.Group g,GroupLayout.Alignment align)
	利用给定的对齐方式（BASELINE、LEADING、TRAILINC或CENTER）添加一个组件或组至本组中。
3. javax.swing.GroupLayout.SequentialGroup
	* GroupLayout.SequentialGroup addContainerCap()
	* GroupLayout.SequentialGroup addContainerCap(int preferredSize,int maximumSize)
	为分割组件和容器的边缘添加一个间隙。
    * GroupLayout.SequentialGroup addPreferredCap(LayoutStyle.ComponentPlacement type)
    为分隔组件添加一个间隙。间隙的类型是LayoutStyle.ComponentPlacement.RELATED或LayoutStyle.ComponentPlacement。

#### 9.6.3 不使用布局管理器
1. 下面是将一个组件定位到某个绝对定位的步骤：
（1）将布局管理器设置为null。
（2）将组件添加到容器中。
（3）指定想要放置的位置和大小。
2. java.awt.Component 1.0
	* void setBounds(int x,int y,int edth,int height)
	移动并调整组件的尺寸。
    参数：x,y 组件新的左上角位置。width,height 组件新的尺寸。

#### 9.6.4 定制布局管理器
1. 定制布局管理器必须实现LayoutManager接口，并且需要覆盖下面5个方法：`void addLayoutComponent(String s,Component c);`,`void removeLayout(Component c);`,`Dimension preferredLayoutSize(Container parent);`、`Dimension minimumLayoutSize(Container parent)`、`void layoutContainer(Container parent)`。
在添加或删除一个组件时会抵用前面两个方法。如果不需要保存组件的任何附加信息，那么可以让这两个方法什么都不做。接下来的两个方法计算组件的最小布局和首选布局所需要的空间。两者通常相等。第5个方法真正地实施操作，它调用所有组件的setBounds方法。
2. java.awt.LayoutManager 1.0
	* void addLayoutComponent(String name,Component cmp)
	将组件添加到布局中。
    参数：name 组件位置的标识符。 comp 被添加的组件。
    * void removeLayoutComponent(Component comp)
    从本布局中删除一个组件。
    * Dimension preferredLayoutSize(Container cont)
    返回本布局下的容器的首选尺寸。
    * Dimension minimumLayoutSize(Container cont)
    返回本布局中下容器的最小尺寸。
    * void layoutContainer(Container cont)
    摆放容器内的组件。

#### 9.6.5 遍历顺序
1. 当把很多组件添加到窗口中时，需要考虑遍历顺序（traversal order）的问题。
2. 调用compnent.setFocusable(false);可以从焦点遍历中删除一个组件。这对于不接受键盘输入、自行绘制的组件很有用。

## 9.7 对话框
1. 与大多数的窗口系统一样，AWT也分为模式对话框和无模式对话框。所谓模式对话框是指在结束对它的处理之前，不允许用户与应用程序的其余窗口进行交互。模式对话框主要用于在程序继续运行之前获取用户提供的信息。
所谓无模式对话框是指允许用户同时在对话框和应用程序的其他窗口中输入信息。使用无模式对话框的最好例子就是工具栏。工具栏可以停靠在任何地方，并且用户可以在需要的时候，同时与应用程序窗口和工具栏进行交互。
2. Swing有一个很容易使用的类JOptionPane，它可以弹出一个简单的对话框，而不必编写任何对话框的相关代码。

#### 9.7.1 选项对话框
1. Swing有一套简单的对话框，用于获取用户的一些简单信息。JOptionPane有4个用于显示这些对话框的静态方法：
	showMessageDialog：显示一条消息并等待用户点击OK
    showConfirmDialog：显示一条消息并等待用户确认（与OK/Cancel类似）
    showOptionDialog：显示一条消息并获得用户在一组选项中的选择
    showInputDialog：显示一条消息并获得用户输入的一行文本
2. 输入对话框有一个用于接收用户输入的额外组件。它既可能是用于输入任何字符串的文本域，也可能是允许用户从中选择的组合框。
3. 这些对话框的确切布局和为标准消息类型选择的图标都取决于具体的观感。
4. 每个对话框类型都有一个方法，可以用来提供自己的图标，以替代原来的图标。
5. 可以为每个对话框类型指定一条消息。这的消息既可以是字符串、图标、用户界面组件，也可以是其他类型的对象。下面是显示消息对象的基本方法：
String: 绘制字符串
Icon： 显示图标
Component： 显示组件
Object[]： 显示数组中的所有对象，一次叠加
任何其他对象： 调用toString方法来显示结果字符串
6. 唯一底部的按钮取决于对话框类型和选项类型。当调用showMessageDialog和showInputDailog时，只能看到一组标准按钮（分别是OK/Cancel）。当调用showConfirmDialog时，可以选择下面四种选项类型之一：DEFAULT_OPTION、YES_NO_OPTION、YES_NO_CANCEL_OPTION、OK_CANCEL_OPTION。
7. 使用showOptionDialog可以指定任意的选项。这里需要为选项提供一个对象数组。每个数组元素可以是下列类型之一：
String：使用字符串标签创建一个按钮
Icon：使用图标创建一个按钮
Component：显示这个组件
其他类型的对象：使用toString方法，然后用结果字符串作为标签创建按钮。
8. 这些方法的返回值：
showMessageDialog 无
showConfirmDialog 表示被选项的一个整数
showOptionDialog 表示被选项的一个整数
showInputDialog 用户选择或输入的字符串
showConfirmDialog和showOptionDialog返回一个整数用来表示用户选择了哪个按钮。对于选项对话框来说，这个值就是被选的选项的索引值或者是CLOSED_OPTION（此时用户没有选择可选项，而是关闭了对话框），对于确认对话框，返回值可以是下列值之一：OK_OPTION、CANCEL_OPTION、YES_OPTION、NO_OPTION、CLOSED_OPTION。
9. 消息字符串中可以包含换行符（'\n'）。这样就可以让字符串多行显示。
10. javax.swing.JOptionPane 1.2
	* static void showMessageDialog(Component parent,Object message,String title,int messageType,Icon icon)
	* static void showMessageDialog(Component parent,Object message,String title,int messageType)
	* static void showMessageDialog(Component parent,Object message)
	* static void showInternalMessageDialog(Component parent,Object message,String title,int messageType,Icon icon)
	* static void showInternalMessageDialog(Component parent,Object message,String title,int messageType)
	* static void showInternalMessageDialog(Component parent,Object message)
	显示一个消息对话框或者一个内部消息对话框（内部对话框完全显示在所在的框架内）。
    参数：parent 父组件（可以为null）。 message 显示在对话框中的消息（可以是字符串、图标、组件或者一个这些类型的数组）。title 对话框标题栏中的字符串。messageType 取值为ERROR_MESSAGE、INFORMATION_MESSAGE、WARNING_MESSAGE、QUESTION_MESSAGE、PLAIN_MESSAGE之一。icon 用于替代标准图标的图标。
	* static int showConfirmDialog(Component parent,Object message,String title,int optionType,int messageType,Icon icon)
	* static int showConfirmDialog(Component parent,Object message,String title,int optionType,int messageType)
	* static int showConfirmDialog(Component parent,Object message,String title,int optionType)
	* static int showConfirmDialog(Component parent,Object message)
	* static int showInternalConfirmDialog(Component parent,Object message,String title,int optionType,int messageType,Icon icon)
	* static int showInternalConfirmDialog(Cmponent parent,Object message,String title,int optionType,int messageType)
	* static int showInternalConfirmDialog(Component parent,Object message,String title,int optionType)
	* static int showInternalConfirmDialog(Component parent,Object message)
	显示一个确认对话框或者内部确认对话框（内部对话框完全显示在所在的框架内）。返回用户选择的选项（取值为OK_OPTION，CANCEL_OPTION，YES_OPTION，NO_OPTION0）;如果用户关闭对话框将返回CLOSED_OPTION。
    参数：parent 父组件（可以为null）。 message 显示在对话框中的消息（可以是字符串、图标、组件或者一个这些类型的数组）。 title 对话框标题栏中的字符串。 messageType 取值为ERROR_MESSAGE、INFORMATION_MESSAGE、WARNING_MESSAGE、QUESTION_MESSAGE、PLAN_MESSAGE之一。optionType 取值为DEFAULT_OPTION、YES_NO_OPTION、YES_NO_CANCEL_OPTION、OK_CANCEL_OPTION之一。icon 用于替代标准图标的图标。
    * static int showOptionDialog(Component parent,Object message,String title,int optionType,int messageType,Icon icon,Object[] options,Object default)
    * static int showInternalOptionDialog(Component parent,Object message,String title,int optionType,int messageType,Icon icon,Object[] options,Object default)
    显示一个选项对话框或者内部选项对话框（内部对话框完全显示在所在的框架内）。返回用户选择的选项索引；如果用户取消对话框返回CLOSED_OPTION。
    参数：parent 父组件（可以为null）。message 显示在对话框中的消息（可以是字符串，图标，组件或者一个这些类型的数组）。title 对话框标题栏中的字符串。messageType 取值为ERROR_MESSAGE、INFORMATION_MESSAGE、WARNING_MESSAGE、QUESTION_MESSAGE、PLAIN_MESSAGE之一。optionType 取值为DEFAULT_OPTION、YES_NO_OPTION、YES_NO_CANCEL_OPTION、OK_CANCEL_OPTION之一。icon 用来替代标准图标的图标。options 一组选项（可以是字符串、图标或者组件）。default 呈现给用户的默认值。
    * static Object showInputDialog(Component parent,Object message,String title,int messageType,Icon icon,Object[] values,Object default)
    * static String showInputDialog(Component parent,Object message,String title,int messageType)
    * static String showInputDialog(Component parent,Object message)
    * static String showInputDialog(object message)
    * static String showInputDialog(Component parent,Object message,Object default) 1.4
    * static String showInputDialog(Object message,Object default) 1.4
    * static String showInternalInputDialog(Component parent,Object message,String title,int messageType,Icon icon,Object[] values,Object default)
    * static Strng showInternalInputDialog(Component parent,Object message,String title,int messageType)
    * static String showInternalInputDialog(Component parent,Object message)
    显示一个输入对话框或者内部输入对话框（内部对话框完全显示在所在的框架内）。返回用户输入的字符串；如果用户取消对话框返回null。
    参数：parent 父组件（可以为null）。message 显示在对话框中的消息（可以是字符串、图标、组件或者一个这些类型的数组）。title 对话框标题栏中的字符串。 messageType 取值为ERROR_MESSAGE、INFORMATION_MESSAGE、WARNING_MESSAGE、QUESTION_MESSAGE、PLAI_MESSAGE之一。icon 用于替代标准图标的图标。values 在组合框中显示的一组值。default 呈现给用户的默认值。

#### 9.7.2 创建对话框
1. 要想创建一个对话框，需要从JDialog派生一个类。具体过程如下：
（1）在对话框构造器中，调用超类JDialog的构造器。
（2）添加对话框的用户界面组件。
（3）添加事件处理器。
（4）设置对话框的大小。
2. 在调用超类构造器时，需要提供拥有者框架（owner frame）、对话框标题及模式特征。
3. 拥有者框架控制对话框的显示位置，如果将拥有者标识为null，那么对话框将由一个隐藏框架所拥有。
4. 模式特征将指定对话框处于显示状态时，应用程序中其他窗口是否被锁住。无模式对话框不会锁住其他窗口，而有模式对话框将锁住应用程序中的所有其他窗口（除对话框的子窗口外）。用户经常使用的工具栏就是无模式对话框，另一方面，如果想强迫用户在继续操作之前提供一些必要的信息就应该使用模式对话框。
5. javax.swing.JDialog 1.2
	* public JDialog(Frame parent,String title,boolean modal)
	构造一个对话框。在没有明确地让对话框显示之前，它是不可见的。
    参数：parent 对话框拥有者的框架。 title 对话框的标题。 modal True代表模式对话框（模式对话框阻塞其他窗口的输入）。

#### 9.7.3 数据交换
1. 很多对话框都有默认按钮。如果用户按下一个触发器键就自动地选择了它。默认按钮通常用加粗的轮廓给予特别的标识。
2. javax.swing.SwingUtilities 1.2
	* Container getAncestorOfClass(Clas c,Component comp)
	返回给定组件的最先的父容器。这个组件属于给定的类或者其子类之一。
3. javax.swing/JComponent 1.2
	* JRootPane getRootPane()
	获得最靠近这个组件的根窗格，如果这个组件的祖先没有根窗格返回null。
4. javax.swing.JRootPane 1.2
	* void setDefaultButton(JButton button)
	设置根窗格的默认按钮。要想禁用默认按钮，需要将参数设置为null。
5. javax.swing.JButton 1.2
	* boolean isDefaultButton()
	如果这个按钮是它的根窗格的默认按钮，返回true。

#### 9.7.4 文件对话框
1. Swing中提供了JFileChooser类，它可以显示一个文本对话框，其外观与本地应用程序中使用的文件对话框基本一样。JFileChooser是一个模式对话框。注意，JFileChooser类并不是JDialog类的子类。需要调用showOpenDialog，而不是调用setVisible(true)显示打开文件的对话框，或者调用showSaveDialog显示保存文件的对话框。接收文件的按钮被自动地标签为Open或者Save。也可以调用sowDialog方法为按钮设定标签。
2. 建立文件对话框并且获取用户选择信息的步骤：
（1）建立一个JFileChooser对象。与JDialog类的构造器不同，它不需要指定父组件。允许在多个框架中重用一个文件选择器。
（2）调用setCurrentDirectory方法设置当前目录。
（3）如果有一个想要作为用户选择的默认文件名，可以使用setSelectedFile方法进行指定。
（4）如果允许用户在对话框中选择多个文件，需要调用setMultiSelectionEnabled方法。当然，这是可选的。
（5）如果想让对话框仅显示某一种类型的文件，西药设置文件过滤器。
（6）在默认情况下，用户在文件选择器中只能选择文件。如果希望选择目录，需要调用setFileSelectionMode方法。参数值为：JFileChooser.FILES_ONLY（默认值），JFileChooser.DIRECTORIES_ONLY或者JFileChooser.FILES_AND_DIRECTORIES。
（7）调用showOpenDialog或者showSaveDialog方法显示对话框。
（8）调用getSelectedFile()或者getSelectedFiles()方法获取用户选择的一个或多个文件。这些方法将返回一个文件对象或者一组文件对象。如果需要知道文件对象名时，可以调用getPath方法。
3. 若想限制显示的文件，需要创建一个实现了抽象类javax.swing.filechooser.FileFilter的对象。文件选择器将每个文件传递给文件过滤器，只有文件过滤器接受的文件才被最终显示出来。
4. 用户可以从文件对话框底部的组合框中选择过滤器。在默认情况下，All File过滤器总是显示在组合框中。如果想禁用All files过滤器，需要调用：`chooser.setAcceptAllFileFilterUsed(false)`。
5. 如果为加载和保存不同类型的文件重用一个文件选择器，就需要调用：`chooser.resetChoosablefILTERS()`，这样可以在添加新文件过滤器之前清除旧文件过滤器。
6. 可以通过为文件选择器显示的每个文件提供特定的图标和文件描述来定制文件选择器。者需要应用一个扩展于javax.swing.fileChooser包中的FileView类的对象。在通常情况下，不需要提供文件视图-可插观感会提供。然而，如果想让某种特定的文件类型显示不同的图标，就需要安装自己的文件视图。这要扩展FileView并实现下面5个方法：Icon getIcon(File f);、String getName(File f);、String getDescription(File f);、String getTypeDescription(File f);、Boolean isTraversable(File f);，然后，调用setFileView方法将文件视图安装到文件选择器中。
7. 文件选择器为每个希望显示的文件或目录调用这些方法。如果方法返回的图标、名字或描述信息为null，那么文件选择器将会构造当前观感的默认文件视图。这样处理很好，器援用是这样只需要处理具有不同显示的文件类型。
8. 文件选择器调用isTravesable方法来决定是否在用户点击一个目录的时候打开这个目录。
9. javax.swing.JFileChooser 1.2
	* JFileChooser()
	创建一个可用于多框架的文件选择器对话框。
    * void setCurrentDirectory(File file)
    设置文件对话框的初始目录。
    * void setSelectedFile(File file)
    * void setSelectedFiles(File[] file)
    设置文件对话框的默认文件选择。
    * void setMultiSelectionEnabled(boolean b)
    设置或清除多选模式。
    * void setFileSelectionMode(int mode)
    设置用户选择模式，只可以选择文件（默认），只可以选择目录，或者文件和目录均可以选择。mode参数的取值可以是JFileChooser.FILES_ONLY、JFileChooser.DIRECTORIES_ONLY和JFileChooser.FILES_AND_DIRECTORIES之一。
    * int showOpenDialog(Component parent)
    * int showSaveDialog(Component parent)
    * int showDialog(Component parent,String approveButtonText)
    显示按钮标签为Open，Save或者approveButtonText字符串的对话框，并返回APPROVE_OPTION、CANCEL_OPTION（或者用户选择取消按钮或者离开了对话框）或者ERROR_OPTION（如果发生错误）。
    * File getSelectedFile()
    * File[] getSelectedFiles()
    获取用户选择的一个文件或多个文件（如果用户没有选择文件，返回null）。
    * void setFileFilter(FileFilter filter)
    设置文件对话框的文件过滤器。所有让filter.accept返回true的文件都会被显示，并且将过滤器添加到可选过滤器列表中。
    * void addChooseableFileFilter(FileFilter filter)
    将文件过滤器添加到可选过滤器列表中。
    * void setAcceptAllFileFilterUsed(boolean b)
    在过滤器组合框中包括或者取消All files过滤器。
    * void resetChoosableFileFilters()
    清除可选过滤器列表。除非All filtes过滤器被显式地清除，否则它仍然会存在。
    * void setFileView(FileView view)
    设置一个文件视图来提供文件选择器显示信息。
    * void setAccessory(JComponent component)
    设置一个附件组件。
10. javax.swing.fileCooser.FileFilter 1.2
	* boolean accept(File f)
	如果文件选择器可以显示这个文件，返回true。
    * String getDescription()
    返回这个文件过滤器的说明信息，例如，Image Fils(*.gif,*.jpeg)。
11. javax.swing.fileChooser.FileNameExtensionFiler 6
	* FileNameExtensFilter(String description,String... extensions)
	利用给定的描述构造一个文件过滤器。这些描述限定了被接收的所有目录和文件其名称结尾的句点之后所包含的扩展字符串。
12.  javax.swing.filechooser.FileView 1.2
	* String getName(File f)
	返回文件f的文件名，或者null。通常这个方法返回f.getName()。
    * String getDescription(File f)
    返回文件f的可读性描述，或者null。例如，如果f是HTML文档，那么这个方法有可能返回它的标题。
    * String getTypeDescription(File f)
    返回文件f的类型的可读性描述。例如，如果f是HTML文档，那么这个方法有可能返回Hypertext document字符串。
    * Icon getIcon(File f)
    返回文件f的图标，或者null。例如，如果f是JPEG文件，那么这个方法有可能返回简略的图标。
    * Boolean isTraversable(File f)
    如果f是用户可以打开的目录，返回Boolean.TRUE。如果目录在概念上是符合文档，那么这个方法有可能返回false。与所有的FileView方法一样，这个方法有可能返回null，用于标示文件选择器应该使用默认视图。

#### 9.7.5 颜色选择器
1. Swing还提供了一种选择器-JColorChooser。可以利用这个选择器选取颜色。与JFileChooser一样，颜色选择器也是一个组件，而不是一个对话框，但是它包含了用于创建包含颜色选择器组件的对话框方法。
2. 显示颜色选择器对话框，需要提供：
	* 一个父组件。
	* 对话框的标题。
	* 选择模式/无模式对话框的标志。
	* 颜色选择器。
	* OK和Cancel按钮的监听器。
3. javax.swing.JColorChooser 1.2
	* JColorChooser()
	构造一个初始颜色为白色的颜色选择器。
    * Color getColor()
    * void setColor(Color c)
    获取和设置颜色选择器的当前颜色。
    * static Color showDialog(Component parent,String title,Color initialColor)
    显示包含颜色选择器的模式对话框。
    参数：parent 对话框显示在上面的组件。 title 对话框框架的标题。 initialColor 颜色选择器的初始颜色。
    * static JDialog createDialog(Component parent,String title,boolean modal,JColorChooser chooser,ActionLstener okListener,ActionListener cancelListener)
    创建一个包含颜色选择器的对话框。
    参数：parent 对话框显示在上面的组件。title 对话框框架的标题。modal 如果直到对话框关闭，这个调用都被阻塞，则返回true。chooser 添加到对话框中的颜色选择器。okListener,cancelListener OK和Cancel按钮的监听器。