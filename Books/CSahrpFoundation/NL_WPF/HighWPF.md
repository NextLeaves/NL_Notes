title: High WPF
date: 2017-10-27 20:00:00
tag: High CSharp

---

* 高级桌面编程：
	* 路由命令来替代事件
	* Menu控件和路由命令来创建菜单
	* 如何使用XAML样式来设置控件和应用程序的样式
	* 创建值转换器
	* 使用时间线来创建动画
	* 如何定义和引用静态及动态资源
	* 在常用空间不满足时，创建用户空间

<!--more-->

## Main Window ##

1. 菜单控件
	* Item
	* MenuItem：Header="_File";**菜单名需要下划线**
	* Menu.Icon
2. 路由命令和菜单
	* 使用理由路由命令的三大理由：
		1. 想在应用程序多处不同的位置触发事件
		2. UI元素的控制问题，比如若没有需要保存文件时，保存文件的选项为不可选
		3. 想让处理事件的代码与代码隐藏文件脱离

3. CommandBindings属性->CommandBinding属性->Command/CanExecute/Execute
4. **记忆逻辑：**
	1. <Window.CommandBindings>，选择CommandBinding的属性Command="ApplicationCommands.Close"
	2. 实现CommandCanExecute/CommandExecute脚本代码
	3. 对应MenuItem或者空间对应的属性command修改为对应的函数

`

	 <Window.CommandBindings>
	    <CommandBinding Command="ApplicationCommands.Close"
	        CanExecute="CommandCanExecute" Executed="CommandExecuted" />
	    <CommandBinding Command="ApplicationCommands.Save" 
	        CanExecute="CommandCanExecute" Executed="CommandExecuted" />	 	
	  </Window.CommandBindings>

	private void CommandCanExecute(object sender, CanExecuteRoutedEventArgs e)
	    {
	      if (e.Command == ApplicationCommands.Close)
	        e.CanExecute = true;
	      if (e.Command == ApplicationCommands.Save)
	        e.CanExecute = false;
	      e.Handled = true;
	    }
	
	    private void CommandExecuted(object sender, ExecutedRoutedEventArgs e)
	    {
	      if (e.Command == ApplicationCommands.Close)
	        this.Close();
	      e.Handled = true;
	    }


---

## 创建空间并设置样式 ##

1. WPF一个最具有特性的特点是：开发人员能够完全的控制和设计用户界面的外观和操作方式

### 样式 ###

1. 批量设置要应用到控件上的某些属性（**批量设置属性**）
2. FrameworkElement->Style->Setter<Property/Value>
3. 将样式转变为资源时，样式就十分的有用，因为资源可供重复使用
4. **记忆逻辑：**
	1. <Button.Style> Style属性，设置对象
	2. 对应什么类型的Style，TargetType
	3. 然后Setter，设置控件的值
	4. 对于颜色设计，所使用Brush类，如：SolidColorBrush Color="Azure"

### 模板 ###

1. 在其基础上设置控件外观的控件（**设计控件外观的控件**）
2. ControlTemplate(Template/TargetType)
3. **记忆逻辑：**
	1. <Button.Style>下的Style类的TargetType
	2. Setter类下设置 Property为Template，value值为ControlTemplate类
	3. ControlTemplate类的TargetType为对应控件

`

	<!--样式-->
	<Button.Style>
		<Style TargetType="Button">
			<Setter Property="Foreground">
				<Setter.Value>
					<SolidColorBrush Color="Azure" />
				</Setter.Value>
			</Setter>
		</Style>
	</Button>


	<!--模板-->
	<Button>
        <Button.Style>
            <Style TargetType="Button">
                <Setter Property="Template">
                    <Setter.Value>
                        <ControlTemplate TargetType="Button">

                        </ControlTemplate>
                    </Setter.Value>
                </Setter>
            </Style>
        </Button.Style>
    </Button>

---

### 值转换器 ###

1. 需要实现接口:IValueConverter
	* object Converter(object value,Type targetType,object parameter,CultureInfo culture)
	* object ConverterBack(object value,Type targetType,object parameter,CultureInfo culture)
2. ValueConversion特性
	* 明确要转换的源类型和目标类型
3. 添加静态资源、实现**静态绑定**(Converter属性)

### 触发器 ###

1. WPF中的事件几乎无所不包，比如按钮的点击，应用程序的启动，事件的关闭
2. 有几类触发器（Trigger），均继承于TriggerBase基类
3. 每个控件都有Trigger属性，可以直接在控件上面定义触发器
4. **触发器的配置如下：**
	1. Trigger要监视的控件的属性，应使用Trigger.Property属性
	2. 何时激活Trigger，应使用Trigger.Value属性
	3. 要定义Trigger的操作，应设置Trigger.Setters属性下的Setter对象集合

`

	<Trigger Property="MyBooleanValue" Value="True">
		<Setter Property="Opacity" Value="0.5">
	<Trigger>

	<Style TargetType="Button">
		<Style.Triggers>
			<Trigger Property="IsMouseOver" Value="true">
				<Setter Property="Foreground" Value="Yellow">
			</Trigger>
		</Style.Triggers>
	</Style>