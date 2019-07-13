---
layout: post
title: Revit插件中使用MaterialDesignThemes
date: 2019-07-13
excerpt: "Revit插件中使用MaterialDesignThemes一些心得"
tags: [Revit]
comments: true
---

前段时间发现了MaterialDesignThemes这个库，它将Google的Material Design带到了WPF中。尝试了下在Revit插件中使用这个库。

## 安装
直接从NuGet中搜索MaterialDesignThemes安装即可。

## 主题颜色相关配置
正常WPF应用的话，直接在App.xaml中添加如下配置即可。
~~~ xml
<ResourceDictionary>
	<ResourceDictionary.MergedDictionaries>
		<ResourceDictionary Source="pack://application:,,,/MaterialDesignThemes.Wpf;component/Themes/MaterialDesignTheme.Light.xaml" />
		<ResourceDictionary Source="pack://application:,,,/MaterialDesignThemes.Wpf;component/Themes/MaterialDesignTheme.Defaults.xaml" />
		<ResourceDictionary Source="pack://application:,,,/MaterialDesignColors;component/Themes/Recommended/Primary/MaterialDesignColor.DeepPurple.xaml" />
		<ResourceDictionary Source="pack://application:,,,/MaterialDesignColors;component/Themes/Recommended/Accent/MaterialDesignColor.Lime.xaml" />
	</ResourceDictionary.MergedDictionaries>            
</ResourceDictionary>
~~~
但是Revit插件是Library项目，没有App.xaml。我们自己新建一个Settings.xaml, 添加上述的配置代码。然后，在每个窗口中引用这个Settings.xaml。
~~~ xml
<Window.Resources>
	<ResourceDictionary>
		<ResourceDictionary.MergedDictionaries>
			<ResourceDictionary Source="../Settings.xaml"/>
		</ResourceDictionary.MergedDictionaries>
	</ResourceDictionary>
</Window.Resources>
~~~
## 窗口的配置
这个和正常WPF应用没有区别，直接添加就可以。
~~~ xml
<Window 
	TextElement.Foreground="{DynamicResource MaterialDesignBody}"
	Background="{DynamicResource MaterialDesignPaper}"
	TextElement.FontWeight="Medium"
	TextElement.FontSize="14"
	FontFamily="pack://application:,,,/MaterialDesignThemes.Wpf;component/Resources/Roboto/#Roboto"
>
~~~

## 一些问题
### 报错System.Windows.Markup.XamlParseException
<figure>
	<img height="300" src="https://github.com/hotWing/hotwing.github.io/blob/master/assets/img/post/MaterialDesignThemesError.png?raw=true">
</figure>
是因为XAML引用的一些外部dll中的控件，程序会从Revit安装目录中去找这些dll, 最简单的就是把MaterialDesignThemes的所有dll都放到Revit安装目录中就行。或者在NuGet中搜索安装Costura.Fody插件，这个插件可以把所有dll都打包进你的插件dll。

