# PyQt4中的事件和信号

> **Events and Signals in PyQt4**

> In this part of the PyQt4 programming tutorial, we will explore events and singnals occuring in applications.

在PyQt4教程的这部分中，我们将探讨应用中事件和信号的发生。

> Events
    -------

> Events are an important part in any GUI program. Events are generated by users or by the system. When we call the application's exec_() method, the application enters the main loop. The main loop fetches events and sends them to the objects. Trolltech has introduced a unique signal and slot mechanism.

事件是GUI程序的重要部分，由用户或者系统产生。当我们调用应用的 `exec_()` 方法，应用进入主循环。主循环获取事件并把它们发往对象。Trolltech引入了独一无二的信号和槽机制。

> Events are an important part in any GUI program. All GUI applications are event-driven. An application reacts to different event types which are generated during its life. Events are generated mainly by the user of an application. But they can be generated by other means as well. e.g. internet connection, window manager, timer. In the event model, there are three participants:

事件是GUI程序的主要部分，所有GUI应用程序都是事件驱动的。应用在它的生命周期中产生的不同事件交互。事件主要由用户产生，但是它们也可以由其他方式产生，如：互联网，窗口管理器，定时器。在事件模型中，由三个参与者：

>    * event source
    * event object
    * event target
    
* 事件来源
* 事件对象
* 事件目标

> The event source is the object whose state changes. It generates Events. The event object (Event) encapsulates the state changes in the event source. The event target is the object that wants to be notified. Event source object delegates the task of handling an event to the event target.

**事件对象** 是指状态改变的对象，它产生了事件。 **事件对象** （Event）封装了事件元的状态改变。 **事件目标** 是事件对象想要
想要通知的对象。事件来源对象代理了事件的目标要处理的任务。

> When we call the application's exec_() method, the application enters the main loop. The main loop fetches events and sends them to the objects. Signals and slots are used for communication between objects. A signal is emitted when a particular event occurs. A slot can be any Python callable. A slot is called when a signal connected to it is emitted.

当我们调用了程序 ``exec_()``方法，程序进入了主循环。主循环捕获事件并把它们发往对象。信号和槽用于对象之间的通讯。
当一个特殊的事件发生时，将发射 **信号** ， **槽** 可以是任何Python调用，当链接到槽的信号发射，该槽将被调用。

## 信号和槽
> **Signals & Slots**

> This is a simple example, demonstrating signals and slots in PyQt4.

这是一个演示PyQt4信号和槽的简单例子。

::

    #!/usr/bin/python
    # -*- coding: utf-8 -*-

    # sigslot.py

    import sys
    from PyQt4 import QtGui, QtCore


    class Example(QtGui.QWidget):
  
        def __init__(self):
            super(Example, self).__init__()
        
            self.initUI()
    
        def initUI(self):

            lcd = QtGui.QLCDNumber(self)
            slider = QtGui.QSlider(QtCore.Qt.Horizontal, self)

            vbox = QtGui.QVBoxLayout()
            vbox.addWidget(lcd)
            vbox.addWidget(slider)

            self.setLayout(vbox)
            self.connect(slider,  QtCore.SIGNAL('valueChanged(int)'), lcd, 
                QtCore.SLOT('display(int)'))

            self.setWindowTitle('Signal & slot')
            self.resize(250, 150)


    app = QtGui.QApplication(sys.argv)
    ex = Example()
    ex.show()
    sys.exit(app.exec_())

> In our example, we display an lcd number and a slider. We change the lcd number by dragging the slider.

在我们的例子中，我们显示了一个LCD数字和一个滑块，通过拖动滑块来改变LCD的值。


:: 
    self.connect(slider,  QtCore.SIGNAL('valueChanged(int)'), lcd, QtCore.SLOT('display(int)'))
    
> Here we connect a valueChanged() signal of the slider to the display() slot of the lcd number.

这里我们连接滑块的 `valueChanged()` 信号到LCD数字的 `display()` 槽。

> The connect method has four parameters. The sender is an object that sends a signal. The signal is the signal, which is emitted. The receiver is the object, that receives the signal. Finally the slot is the method, that reacts to the signal.

`connect` 方法有4个参数， `sender` 是发送信号的对象， `signal` 是发射的信号， `receiver` 是接收信号的对象， 最后， `slog` 是对信号反应的方法。

![Signal & slot][signal-slot]

图：信号和槽

## 重新实现事件处理程序
> **Reimplementing event handler**

Events in PyQt4 are processed often by reimplementing event handlers.

PyQt4中的事件经常被重新实现。

::

    #!/usr/bin/python
    # -*- coding: utf-8 -*-

    # escape.py

    import sys
    from PyQt4 import QtGui, QtCore


    class Example(QtGui.QWidget):
  
        def __init__(self):
            super(Example, self).__init__()

            self.setWindowTitle('Escape')
            self.resize(250, 150)
        
        def keyPressEvent(self, event):
            if event.key() == QtCore.Qt.Key_Escape:
                self.close()


    app = QtGui.QApplication(sys.argv)
    ex = Example()
    ex.show()
    sys.exit(app.exec_())
    
> In our example, we reimplement the keyPressEvent() event handler.

在我们的例子中，我们重新实现了 `keyPressEvent()` 处理。

::

    def keyPressEvent(self, event):
        if event.key() == QtCore.Qt.Key_Escape:
            self.close()

> If we click the escape button, the application terminates.

如果我们按下了escape按钮，程序将退出。

## 事件发送者
> **Event sender**

> Sometimes it is convenient to know, which widget is the sender of a signal. For this, PyQt4 has a sender() method.

有时需要方便的知道哪个组件发出的信号，PyQt4有 `sender()` 方法。

::

    #!/usr/bin/python
    # -*- coding: utf-8 -*-

    # sender.py

    import sys
    from PyQt4 import QtGui, QtCore


    class Example(QtGui.QMainWindow):
  
        def __init__(self):
            super(Example, self).__init__()

            self.initUI()
        
        def initUI(self):
      
            button1 = QtGui.QPushButton("Button 1", self)
            button1.move(30, 50)

            button2 = QtGui.QPushButton("Button 2", self)
            button2.move(150, 50)
      
            self.connect(button1, QtCore.SIGNAL('clicked()'), 
                self.buttonClicked)
            
            self.connect(button2, QtCore.SIGNAL('clicked()'), 
                self.buttonClicked)            

            self.statusBar().showMessage('Ready')
            self.setWindowTitle('Event sender')
            self.resize(290, 150)


        def buttonClicked(self):
      
            sender = self.sender()
            self.statusBar().showMessage(sender.text() + ' was pressed')

    app = QtGui.QApplication(sys.argv)
    ex = Example()
    ex.show()
    sys.exit(app.exec_())
    
> We have two buttons in our example. In the buttonClicked() method we determine, which button we have clicked by calling the sender() method.

这个了例子中由两个按钮，在 `buttonClicked()` 方法中我们通过调用 `sender()` 方法确定点击了哪个按钮。

::

    self.connect(button1, QtCore.SIGNAL('clicked()'), 
        self.buttonClicked)
    
    self.connect(button2, QtCore.SIGNAL('clicked()'), 
        self.buttonClicked) 
    
> Both buttons are connected to the same slot.

两个按钮都连接了同一个信号。

::

    def buttonClicked(self):
  
        sender = self.sender()
        self.statusBar().showMessage(sender.text() + ' was pressed')

> We detemine the signal source by calling the sender() method. In the statusbar of the application, we show the label of the button pressed.

通过调用 `sender()` 方法我们确定信号来源。在程序的状态栏，我们显示按下的按钮的标签。

Figure: Event sender

图：事件发送者

## 发射信号
> **Emitting signals**

> Objects created from QtCore.QObject can emit signals. If we click on the button, a clicked() signal is generated. In the following example we will see, how we can emit signals.

从 `QtCore.QObject` 继承的对象可以发射信号。如果点击按钮，将产生一个 `clicked()` 信号。在接下来的例子中可以看到如何发射信号。

::

    #!/usr/bin/python
    # -*- coding: utf-8 -*-

    # emit.py

    import sys
    from PyQt4 import QtGui, QtCore


    class Example(QtGui.QWidget):
  
        def __init__(self):
            super(Example, self).__init__()

            self.initUI()
        
        def initUI(self):

            self.connect(self, QtCore.SIGNAL('closeEmitApp()'), 
                QtCore.SLOT('close()'))

            self.setWindowTitle('emit')
            self.resize(250, 150)


        def mousePressEvent(self, event):
            self.emit(QtCore.SIGNAL('closeEmitApp()'))


    app = QtGui.QApplication(sys.argv)
    ex = Example()
    ex.show()
    sys.exit(app.exec_())

> We create a new signal called closeEmitApp(). This signal is emitted, during a mouse press event.

我们创建一个名为 `closeEmitApp()` 的新信号，在鼠标的按下实践中发射该信号。

::

    def mousePressEvent(self, event):
        self.emit(QtCore.SIGNAL('closeEmitApp()'))
        
> Emitting a signal with the emit() method.

通过 `emit()` 方法发射信号。

::

    self.connect(self, QtCore.SIGNAL('closeEmitApp()'), 
        QtCore.SLOT('close()'))

> Here we connect the manually created closeEmitApp() signal with the close() slot.

这里我们把手工创建的 `closeEmitApp()` 信号和 `close()` 槽连接。

> In this part of the PyQt4 tutorial, we have covered signals and slots.

在PyQt4教程的这部分，我们涵盖了信号和槽。