# 第10章 部署应用程序和applet 426-470
	本章内容：
	* JAR文件
	* Java Web Start
	* applet
	* 应用程序首选项存储

## 10.1 JAR文件
1. 一个JAR文件既可以包含类文件，也可以包含诸如图像和声音这些其他类型的文件。此外，JAR文件是压缩的，它使用了大家熟悉的ZIP压缩格式。
2. pack200是一种较通常的ZIP压缩算法更加有效的压缩类文件的方式。Oracle声称，对类文件的压缩率接近90%。
3. 可以使用jar工具制作JAR文件（在默认的JDK安装中，位于jdk/bin目录下）。创建一个新的JAR文件应该使用的常见命令格式为`jar cvf JARFileName File1 File2 ...`。
4. .jar程序选项
| 选项 | 说明 |
|--------|--------|
|c|创建一个新的或者空的存档文件并加入文件。如果指定的文件名是目录，jar程序将会对它们进行递归处理。|
|C|暂时改变目录，例如`jar cvf JARFileName.jar -C classes *.class`改变classes子目录，以便增加这些类文件|
|e|在清单文件中创建一个条目|
|f|将JAR文件名指定为第二个命令行参数。如果没有这个参数，jar命令会将结果写在标准输出上（在创建JAR文件时）或者从标准输入中读取它（在解压或者列出JAR文件内容时）|
|i|简历索引文件（用于加快对大型归档的查找）|
|m|将一个清单文件（manifest）添加到JAR文件中。清单是对归档内容和来源的说明。每个归档有一个默认的清单文件。但是，如果想验证归档文件的内容，可以提供自己的清单文件|
|M|不为条目创建清单文件|
|t|显示内容表|
|u|更新一个已有的JAR文件|
|v|生成详细的输出结果|
|x|解压文件。如果提供一个或多个文件名，只解压这些文件；否则，解压所有文件|
|O|存储，不进行ZIP压缩|

#### 10.1.1 清单文件
1. 除了类文件、图像和其他资源外，每个JAR文件还包含衣蛾用于描述归档特征的清单文件（manifest）。
2. 清单文件被命名为MANIFEST.MF，它位于JAR文件的一个特殊META-INF子目录中。
3. 清单条目被分成多个节。第一节被称为主节（main section）。它作用域整个JAR文件。随后的条目用来指定已命名条目的属性，这些已命名的条目可以是某个文件、包或者URL，他们都必须起始于名为Name的条目。节与节之间用空行分开。
4. 要想编辑清单文件，需要将希望添加到清单文件中的行放到文本文件中，然后运行：`jar cfm JARFileName MainfestFileName ...`。
5. 要想更新一个已有的JAR文件的清单，则需要将增加的部分放置到一个文本文件中，然后执行下列命令：`jar ufm MyArchive.jar manifest-additions.mf`。

#### 10.1.2 可运行JAR文件
1. 可以使用jar命令中的e选项指定程序的入口点，即通常需要在调用Java程序加载器时指定的类：`jar cvfe MyProgram.jar com.mycompany.mypkg.MainAppClass files to add`。或者，可以在清单中指定应用程序的主类，包括以下形式的语句：`Main-Class:com.mycompany.mypkg.MainAppClass`。不要讲扩展名.class添加到主类名中。
2. 清单文件的最后一行必须以换行符结束。否则。清单文件将无法被正确地读取。常见的错误是创建了一个只包含Main-Class而没有行结束符的文本文件。
3. 根据操作系统的配置，用户甚至可以通过双击JAR文件图标来启动应用程序。下面是各种操作系统的操作方式：
* 在Windows平台中，Java运行时安装器将建立一个扩展名为.jar的文件与javaw -jar 命令相关联来启动文件（与java命令不同，javaw命令不打开shell窗口）。
* 在Solaris平台中，操作系统能够是被JAR文件的“魔数”格式，并用java -jar命令启动它。
* 在Mac OS X平台中，操作系统能够识别.jar扩展名文件。当双击JAR文件时就会执行Java程序可以运行。

#### 10.1.3 资源
1. 在applet和引用程序中使用的类通常需要使用一些相关的数据文件，例如：
* 图像和声音文件。
* 带有消息字符串和按钮标签的文本文件。
* 二进制数据文件，例如，描述地图布局的文件。
在Java中，这些关联的文件被称为资源（resource）。
2. 类加载器知道如何搜索类文件，直到在类路径、存档文件或Web服务器上找到为止。利用资源机制，对于非类文件也可以同样方便地进行操作，下面是必要的步骤：
1）获取具有资源的Class对象，例如，AboutPancl.class。
2）如果资源是一个图像或声音文件，那么就需要调用getresource(filename)获得作为URL的资源位置，然后利用getImage或getAudioClip方法进行读取。
3）与图像或声音文件不同，其他资源可以使用getResourceAsStream方法读取文件中的数据。
重点在于类加载器可以记住如何定位类，然后在同一位置查找关联的资源。
3. 一个以“/”开头的资源名被称为绝对资源名。它的定位方式与类在包中的定位方式一样。
4. 文件的自动装载是利用资源加载特性完成的。没有标准的方法来解释资源文件的内容。每个程序必须拥有解释资源文件的方法。
5. java.lang.Class 1.0
	* URL getResource(String name) 1.1
	* InputStream getResourceAsStream(String name) 1.1
	找到与类位于同一位置的资源，返回一个可以加载资源的URL或者输入流。如果没有找到资源，则返回null，而且不会抛出异常或者发生I/O错误。

#### 10.1.4 密封
1. 可以将Java包密封（scal）以保证不会有其他的类加入到其中。如果在代码中使用了包可见的类、方法和域，就可能希望密封包。如果不密封，其他类就有可能放在这个包中，进而访问包可见的特性。
2. 要想密封一个包，需要将包中的所有类放到一个JAR文件中。在默认情况下，JAR文件中的包时没有密封的。可以在清单文件的主节中加入下面一行：`Sealed:true`来改变全局的默认设定。对于每个单独的包，可以通过在JAR文件的枪弹中增加一节，来指定是否想要密封这个包。
3. 要想密封一个包，需要创建一个包含清单指令的文本文件。然后用常规的方式运行jar命令：`jar cvfm MyArchive.jar manifest.mf files to add`。

## 10.2 Java Web Start
1. Java Web Start是一项在Internet上发布应用程序的技术。Java Web Start应用程序包含下列主要特性：
* Java Web Start应用程序一般通过浏览器发布。只要Java Web Start应用程序下载到本地就可以启动它，而不需要浏览器。
* Java Web Start应用程序并不在浏览器窗口内。它将显示在浏览器外的一个属于自己的框架中。
* Java Web Start应用程序不使用浏览器的Java实现。浏览器只是在加载Java Web Start应用程序描述符时启动一个外部应用程序。这与启动诸如Adobe Acrobat或RealAudio这样的辅助应用程序的所使用的机制一样。
* 数字签名应用程序可以被赋予访问本地机器的任意权限。未签名的应用程序只能运行在“沙箱”中，它可以阻止具有潜在危险的操作。
2. 要想准备一个通过Java Web Start发布的应用程序，应该将其打包到一个或多个JAR文件中。然后创建一个Java Network Launch Protocol(JNLP)格式的描述符文件。将这些文件放置在Web服务器上。
还需要确保Web服务器对扩展名为.jnlp的文件报告一个application/x-java-jnlp-file的MIME类型（浏览器利用MIME类型确定启动哪一种辅助应用程序）。

#### 10.2.1 沙箱
1. 安全管理器（security manager）将检查有权使用所有系统的资源。在默认情况下，只允许执行那些无害的操作，要想运行执行其他的操作，代码必须得到数字签名，用户必须通过数字认证。
2. 在所有平台上，远程代码可以做什么呢？它可以显示图像、播放音乐、获得用户的键盘输入和鼠标点击，以及将用户的输入送回加载代码所在的主机。这些功能足以能够显示信息和图片，或者获得用户为订单输入的信息。这种受限制的执行环境称为沙箱（sandbox）。在沙箱中运行的代码不行修改或查看用户系统。
3. 在沙箱中的程序有下列限制：
* 不能运行任何本地的可执行程序。
* 不能从本地计算机文件系统中读取任何信息，也不能往本地计算机文件系统中写入任何信息。
* 不能查看除Java版本信息和少数几个无害的操作系统详细信息外的人格有关本地计算机的信息。特别是，在沙箱中的代码不能查看用户名、e-mail地址等信息。
* 远程加载的程序不能与除下载程序所在的服务器之外的任何主机通信，这个服务器被称为源主机（origination host）。这条规则通常称为“远程代码只能与家人通话。”这条规则将确保用户不会被代码探查到内部网络资源。
* 所有弹出式窗口都会带一个警告消息。这条消息起到了安全的作用，以确保用户不会为本地应用程序弄错窗口。令人担心的是，一个可信赖的用户可以访问网页，并被蒙骗运行远程代码，然后输入密码或信用卡号，这些信息将被送回服务器。

#### 10.2.2 签名代码
1. 应用程序可以直接请求拥有一个桌面应用程序所许可的全部权限，相当多的Java Web Start应用程序就是这样做的。只要将下列标签添加到JNLP文件中就可以达到这个目的：`<security><all-permissions></security>`。
2. 要想要沙箱外运行，java Web Start应用程序的JAR文件必须进行数字签名。签名的JAR文件将携带一个可以证明签名者身份的证书。加密技术可以确保这种证书不可被伪造，并且检测任何企图破坏签名文件的行为。

#### 10.2.3 JNIP API
1. JNLP API允许未签名的应用程序在沙箱中运行，同时通过一种安全的途径访问本地资源。
2. API提供了下面的服务：
* 加载和保存文件
* 访问剪贴板
* 打印
* 下载文件
* 在默认的浏览器中显示一个文档
* 保存和恢复持久性配置信息
* 确信只运行一个应用程序的实例
3. 要访问服务，需要使用ServiceManager。如果服务不可用，滴啊用将抛出UnavailableServiceException。
4. 如果想要编译使用了JNLP API的程序，那就必须在类路径中包含javaws.jar文件。这个文件在JDK的jre/lib子目录下。
5. 数据必须由InputStream传递。
6. 要想从文件中读物数据，需要使用FileOpenService。它的openFileDialog对话框接收应用程序提供的初始路径名和文件扩展名，并返回一个FileContents对象。然后调用getInputStream和getOutputStream方法来读写文件数据。如果用户没有选择文件，openFileDialog方法将返回null。
7. 注意，应用程序不知道文件名或文件锁放置的位置。相反地，如果想要打开一个特定的文件，需要使用ExtendedService。
8. 要想在默认浏览器中显示一个文档，就需要使用BasicService接口。注意，有些系统可能没有默认的浏览器。
9. 处于起步阶段的PersistenceService允许应用程序保存少量的配置信息，并且在应用程序下次运行时恢复。这种机制类似于HTTP的cookie，使用URL键进行持久性存储。URL并不需要指向一个真正的web资源，服务仅仅使用它们来构造一个方便的具有层次结构的命名机制。对于任何给定的URL键，应用程序可以存储任意的二进制数据（存储可能会限制数据块的大小）。
10. 因为应用程序时彼此分离的，一个特定的应用程序只能使用从codebase开始的URL键值（codebase在JNLP文件中指定）。
11. 应用程序可以调用BasicService类的getCodeBase方法查看它的codebase。
12. 要想访问与某个特定键关联的信息，需要调用get方法。这个方法将返回一个FileContents对象，通过这个对象可以对键数据进行读写。
13. jacax.jnlp.ServiceManager
	* static String[] getServiceNames()
	返回所有可用的服务名称。
	* static Object lookup(String name)
	返回给定名称的服务。
14. javax.jnlp.BasicService
	* URL getCodeBase()
	返回应用程序的codebase。
	* boolean isWebBrowserSupported()
	如果Web Start环境可以启动浏览器，返回true。
	* boolean showDocument(URL url)
	尝试在浏览器中显示一个给定的URL。如果请求成功，返回true。
15. javax.jnlp.FileCpntents
	* InputStream getInputStream()
	返回一个读取文件内容的输入流。
	* OutputStream getOutputStream(boolean overwrite)
	返回一个对文件进行写操作的输出流。如果overwrite为true，文件中已有的内容将被覆盖。
	* String getName()
	返回文件名（但不是完整的目录路径）。
	* boolean canRead()
	* boolean canWrite()
	如果后台文件可以读写，返回true。
16. javax.jnlp.FileOpenService
	* FileContents openFileDialog(String pathHint,String[] extensions)
	* FileContents[] openMultiFileDialog(String pathHint,String[] extensions)
	显示警告信息并打开文件选择器。返回文件的内容描述符或者用户选择的文件。如果用户没有选择文件，返回null。
17. javax.jnlp.FileSaveService
	* FileContents saveFileDialog(String pathHint,String[] extensions,InputStream data,String nameHint)
	* FileContents saveAsFileDialog(String pathHint,String[] extensions,FileContents data)
	显示警告信息并打开文件选择器。写入数据，并返回文件的内容描述符或者用户选择的文件。如果用户没有选择文件，返回null。
18. javax.jnlp.PersistenceService
	* long create(URL key,long maxsize)
	对于给定的键创建一个持久性存储条目。返回由持久性存储授予这个条目的最大存储空间。
	* void delete(URL key)
	删除给定键的条目。
	* String[] getNames(URL url)
	返回由给定的URL开始的全部键的相对键名。
	* FileContents get(URL key)
	返回用来修改与给定键相关的数据的内容描述符。如果没有与键相关的条目，抛出FileNotFoundException。

## 10.3 applet
1. applet是一种包含在HTML网页中的Java应用程序。HTML网页必须告诉浏览器要加载哪个applet以及每个applet放置在页面的哪个位置。

#### 10.3.1 一个简单的applet
1. 一个applet就是一个扩展于JApplet类，它是Swing applets的超类。
2. 如果applet包含Swing组件，就必须扩展JApplet类。在普通的Applet类中的Swing组件不能正确地显示出来。
3. 想执行applet，需要执行下面两个步骤：
（1）将Java源文件编译成类文件。
（2）创建一个HTML文件，告诉浏览器首先加载哪个类文件以及如何设定applet的大小。
4. 将应用程序转化为applet的基本步骤：
（1）创建一个HTML页面，并用适当的标记加载applet代码。
（2）创建一个JApplet类的子类。将这个子类标记为公有。否则，不能加载applet。
（3）删去应用程序中的main方法。不要为应用程序构造框架窗口。应用程序将显示在浏览器中。
（4）将框架窗口构造器中的初始化代码移到applet的init方法中。不需要明确地构造applet对象，浏览器负责实例化并调用init方法。
（5）删除对setSize的调用。在applet中，大小由HTML中的width和height参数确定定义。
（6）删除对setDefaultCloseOperation的调用。不能关闭applet，退出浏览器时它将会终止运行。
（7）如果应用程序调用setTitle，要删除这个调用。applet没有标题栏。（当然，可以使用HTML的title标记为网页设置标题。）
（8）不要调用setVisible(true)。applet会自动显示。
5. java.applet.Applet 1.0
	* void init()
	首次加载applet时调用这个方法。覆盖这个方法，并且将所有的初始化代码放在这里。
	* void start()
	覆盖这个方法，将用户每次访问包含applet的网页时需要执行的代码放入其中。典型的操作时重新激活线程。
	* void stop()
	覆盖这个方法，将用户退出浏览器时需要执行的代码放入其中。
	* void resize(int width,int height)
	请求调整applet的尺寸。如果这个方法能够在网页上使用就太好了。遗憾的是，由于与当前的布局机制冲突，因此，在当前的浏览器中不起作用。

#### 10.3.2 applet的HTML标记和属性
1. 在applet标记中可以使用下列属性：
* width,height
这些属性是必要的，它以像素为单位，设定applet的宽度和高度。在applet查看器中，这是applet的初始大小。在applet查看器中可以调整创建的窗口大小。在浏览器中，不能调整applet的大小，因此，需要预测一下applet所需要的空间，让用户能够看到一个显示效果良好的结果。
* align
这个属性指定applet的对齐方式。属性值与HTMLing标记的align属性相同。
* vspace,hspace
这些可选属性指定applet上下（vspace）及两侧（hspace）的像素值。
* code
这个属性指定applet的类文件名称。这个名称不是相对于codebase的，就是在没有指定codebase时相对于当前页面的。
路径名必须与applet类的包相匹配。
code属性只用于指定包含applet类的类名。当然，applet还可以包含其他类文件。当浏览器的类加载器加载包含applet的类时，它会意识到需要很多的类文件，并自动地加载这些类文件。
* codebase
这个可选属性指出用于定位类文件的URL。可以使用绝对URL，甚至是不同的服务器。最常见的情况是，使用指向子目录的相对URL。
* archive
这个可选项列出JRA文件，或包含类的文件和applet需要的其他资源。这些文件将在加载applet前就从服务器端获得而来。这种技术明显地加快了加载过程的速度。这是因为只需要一个HTTP的请求来加载包含多个小文件的JAR文件。JAR文件之间用逗号分隔。
* object
这个标签用来指定包含序列化（serialized）的applet对象的文件名（当把所有的实例域写到一个文件中时，对象就被序列化了）。在显示applet时，对象将从文件中反序列化，从面还原到原先的状态。当使用这个属性时，不会调用init方法，而是调用applet的start方法。在序列化applet对象之前，应该调用stop方法。这个特性对于实现持久性浏览器时非常有用的。持久性浏览器可以自动重新加载applet并且将其恢复到上一次浏览器关闭时的状态。这是一个特殊的特性，网页设计人员通常不会遇到这个特性。
在每个applet标记中，必须有code和object属性。
* name
脚本编写人员希望为applet指定name属性，这样在编写脚本时，可以使用这个属性来代表相应的applet。Netscape和Internet Explorer都允许利用JavaScript调用页面中的applet方法。
要使同一个页面上的两个applet之间可以直接地通信，name属性也是一个要素，必须为每个当前的applet实例指定一个名字。然后将这个名字字符串传递给AppletContext类的getApplet方法。
* alt
Java有可能在浏览器中被系统管理员禁用。此时可以利用alt属性显示信息。
如果浏览器不能处理applet，那么就会忽略不识别的applet和param标记。位于<applet>与</applet>标记之间的所有文本会由浏览器显示出来。相反，支持Java的浏览器不会显示<applet>与</applet>标记之间的文本。对于仍旧使用旧版本浏览器的用户，可以在这些标记中显示提示信息。

#### 10.3.3 object 标记
1. object标记是HTML4.0标准的一部分，W3协会建议用它取代applet标记。

#### 10.3.4 使用参数向applet传递信息
1. 与应用程序可以使用命令行信息一样，applet也可以使用嵌入在HTML中的参数。这是由使用被称为param的HTML标记连同自定义属性完成的。可以使用Applet类中的getParameter方法获得参数值。
2. 只能在applet的init方法中调用getParameter方法，而不是在构造器中调用。当applet构造器被执行时，参数还没准备好。由于许多重要的applet布局都是由参数决定的，所以建议不要为applet提供构造器，而是将所有的初始化代码放到init方法中。
3. java.applet.Applet 1.0
	* public String getParameter(String name)
	获得正在加载的applet网页中由param标记定义的参数值。字符串名区分大小写。
    * public String getAppletInfo()
    很多applet作者覆盖这个方法。它用于返回一个包含当前applet作者、版本号和版权信息的字符串。通过在applet类中覆盖这个方法，就可以创建这些信息。
    * public String[ ][ ] getParameterInfo()
    可以覆盖这个方法，用于返回一个applet支持的param标记选项的数组。这个数组每行有三项：名字、类型和有关参数的描述。

#### 10.3.5 访问图像和音频文件
1. applet可以处理图像和音频。图像必须是GIF、PNG或JPEG格式。音频文件必须是AU、AIFF、WAV或MIDI格式。动画支持GIF，并且能显示动画效果。
2. 需要利用相对的URL指定图像和音频文件的位置。通常，由调用getDocumentBase或getCodease方法获得基URL。前者可以包含applet的HTML网页的URL；后者可以获得applet的codebase的URL。
3. 可以通过getImage和getAudioClip方法获得基URL和文件的存储位置。
4. 要想播放音频剪辑，只需要调用play方法即可。还可以调用Applet类中的play方法而不用加载音频剪辑。
5. java.applet.Applet 1.0
	* URL getDocumentBase()
	获得包含applet的网页的URL。
    * URL getCodeBase()
    获得codebase目录的URL，applet是由这个目录加载的。返回的URL既可以是由codebase属性指定的引用目录的绝对URL，在没有指定codebase的情况下，也可以是HTML文件的目录。
    * void play(URL url)
    * void play(URL url,String name)
    第一个方法播放由URL指定的音频文件。第二个方法利用字符串提供相对于第一个参数URL的路径。如果没有找到音频剪辑，则不做任何操作。
    * AudioClip getAudioClip(URL url)
    * AudioClip getAudioClip(URL url,String name)
    第一个方法获得URL指定的音频剪辑。第二个方法使用字符串来提供相对于第一个参数的URL的路径。如果没有找到音频剪辑，则返回null。
    * Image getImage(URL url)
    * Image getImage(URL url)
    返回一个由URL指定的图像对象，这个对象封装一个图像。如果图像不存在，则立即返回null。否则，将启动一个独立的线程来装载图像。

#### 10.3.6 applet上下文
1. applet在浏览器或者applet查看器中运行。applet可以请求浏览器为它提供服务。
2. 如果想在浏览器之间进行通信，那么需要applet调用getAppletContext方法。这个方法将返回一个实现了AppletContext接口的对象。可以将AppletContext接口的具体实现认为是打开了一条applet与环境浏览器之间的通信道路。
3. applet间的通信
一个网页可以包含多个applet。如果网页中包含多个来自同一个codebase的applet，则它们之间就可以互相通信。
getApplets方法返回一个枚举对象（enumeration object）。
applet不能与其他网页上的applet进行通信。
4. 在浏览器中显示信息
通过使用AppletContext类的方法可以访问环境浏览器的两个区域：状态行和网页显示区域。
可以使用showStatus信息在浏览器底部的状态行中显示一个字符串。
利用showDocument方法，可以请求浏览器显示不同的网页。
5. showDocument方法
| 目标参数 | 位置 |
|--------|--------|
| “self”或者none | 在当前框架中显示文档 |
| ”_parent“ | 在父框架中显示文档 |
| “_top”| 在最顶层框架中显示文档 |
| “_blank” | 在新的、未命名的、顶层窗口中显示文档 |
| 其他字符串 | 在指定名称的框架中显示文档。如果没有相应名称的框架，就打开一个新窗口，并给这个窗口指定名称 |
6. java.applet.Applet 1.2
	* public AppletContext getAppletContext()
	获得applet浏览器环境的一个句柄。在大多数浏览中，可以使用这个信息控制运行applet的浏览器。
    * void showStatus(String msg)
    显示浏览器状态栏中指定的字符串。
7. java.applet.AppletContext 1.0
	* Enumration <Applet> getApplets()
	返回在同一个上下文中（也就是在同一个网页中）所有applet的枚举值。
    * Applet getApplet(String name)
    返回当前上下文中给定名称的applet。如果不存在返回null。只能在当前网页中进行查找。
    * void showDocument(URL url)
    * void showDocument(URL url,String target)
    在浏览器的框架中显示一个新网页。第一种形式在当前页中显示新页面。第二种形式用target参数指定目标框架。

## 10.4 应用程序首选项存储

#### 10.4.1 属性映射
1. 属性映射（property map）是一种存储键/值对的数据结构。属性映射经常被用来存放配置信息。它有三个特性：键和值都是字符串、键/值对很容易地写入文件或从文件读出、用二级表存放默认值。实现属性映射的Java类被称为Properties。
2. 习惯上，将程序属性存储在用户主目录的某个子目录下。目录名通常选择由一个（UNIX系统中）圆点开始。这个约束表示来自用户的一个隐藏的系统目录。
3. java.util.Properties 1.0
	* Properties()
	创建一个空的属性映射。
    * Properties(Properties defaults)
    用一组默认值创建空的属性映射。
    参数：default 用于查找的默认值属性。
    * String getProperty(String key)
    获得一个属性映射。返回对应键的字符串。如果键没有在表中出现，返回在默认值表中与键对应的字符串。如果键也没有出现在默认值表中，则返回null。
    * String getProperty(String key,String defaultValue)
    如果没有找到键，返回具有默认值的属性。如果键没有在表中出现，则返回对应键的字符串或默认字符串。
    参数：key 要获得的相关字符串的键。defaultVaule 如果键不存在，则返回这个字符串。
    * void load(InputStream in)throws IOException
    从输入流中加载一个属性映射。
    参数：in 输入流。
    * void store(OutputStream out,String header) 1.2
    将属性映射存放到一个输出流中。
    参数：out 输出流。header 存储文件中的第一行标题。
4. java.lang.System 1.0
	* Properties getProperties()
	获得全部系统属性。应用程序必须有访问全部系统属性的权限，否则将抛出安全异常。
    * String getProperty(String key)
    返回具有给定键名的系统属性。应用程序必须有访问全部系统属性的权限，否则将抛出安全异常。下列特性总可以得到：java.version、java.vendor、java.vendor.url、java.class.version、os.name、os.version、os.arch、file.separator、path.separator、line.separator、java.specification.version、java.vm.specification.version、java.vm.specification.vendor、java.vm.spacification.name、java.vm.version、java.vm.vendor、java.vm.name。

#### 10.4.2 Preferences API
1. Properties类能够简化读取和保存配合信息的过程。但是，使用属性文件存在下列不足：
* 配置文件不能存放在用户的主目录中。这是因为某些操作系统（如Windows 9x）没有主目录的概念。
* 没有标准的为配置文件命名的规则。当用户安装了多个Java应用程序时，会增加配置文件名冲突的可能性。
2. java.util.prefs.Preferences 1.4
	* Preferences userRoot()
	返回调用应用程序的用户首选项的根节点。
    * Preferences systemRoot()
    返回系统范围的首选项的根节点。
    * Preferences node(String path)
    返回从当前节点经过给定路径可以到达的节点。如果path是绝对路径（以/开始），则从包含首选项的树根开始查找这个节点。如果给定的路径不存在这样的节点，则创建它。
    * Preferences userNodeForPackage(Class cl)
    * Preferences systemNodeForPackage(Class cl)
    返回当前用户树或者系统树中的节点。这个节点的绝对路径符合类cl的包名。
    * String[] keys()
    返回属于这个节点的所有键。
    * String get(String key,String defval)
    * int getInt(String key,int defval)
    * long getLong(String key,long defval)
    * float getFloat(String key,float defval)
    * double getDouble(String key,double defval)
    * boolean getBoolean(String key,boolean defval)
    * byte[] getByteArray(String key,byte[] defval)
    返回与给定键关联的值。如果没有与之关联的值，或者关联值得类型不符合要求，或者首选项存储不可用，则返回默认值。
    * void put(String key,String value)
    * void putInt(String key,int value)
    * void putLong(String key,ling value)
    * void putFloat(String key,float value)
    * void putDouble(String key,double value)
    * void putBoolean(String key,boolean value)
    * void putByteArray(String key,byte[] value)
    将键/值存放在节点中。
    * void exportSubtree(OutputStream out)
    将这个节点及子节点的首选项写到指定流中。
    * void exportNode(OutputStream out)
    将这个节点（不包括子节点）首选项写到指定流中。
    * void importPreferences(InputStream in)
    导入指定流中的首选项。

