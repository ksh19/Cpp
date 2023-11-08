- MainWindow.xaml
```ruby
<?xml version="1.0" encoding="utf-8"?>
<Window
    x:Class="App4.MainWindow"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App4"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:canvas="using:Microsoft.Graphics.Canvas.UI.Xaml"
    mc:Ignorable="d">
    <Grid>
        <canvas:CanvasControl PointerMoved="CanvasControl_PointerMoved"
                              Draw="CanvasControl_Draw"
                              ClearColor="CornflowerBlue"/>
    </Grid>
</Window>

```


- MainWindow.xaml.h
```ruby
// Copyright (c) Microsoft Corporation and Contributors.
// Licensed under the MIT License.

#pragma once

#include "MainWindow.g.h"
#include <winrt/Microsoft.Graphics.Canvas.UI.Xaml.h>
#include <winrt/Microsoft.UI.Xaml.Input.h>
#include <winrt/Microsoft.UI.Input.h>

namespace winrt::App4::implementation
{
    struct MainWindow : MainWindowT<MainWindow>
    {
        MainWindow();

        int32_t MyProperty();
        void MyProperty(int32_t value);

        float px;
        float py;

        void CanvasControl_PointerMoved(winrt::Windows::Foundation::IInspectable const& sender, winrt::Microsoft::UI::Xaml::Input::PointerRoutedEventArgs const& e);
        void CanvasControl_Draw(winrt::Microsoft::Graphics::Canvas::UI::Xaml::CanvasControl const& sender, winrt::Microsoft::Graphics::Canvas::UI::Xaml::CanvasDrawEventArgs const& args);
    };
}

namespace winrt::App4::factory_implementation
{
    struct MainWindow : MainWindowT<MainWindow, implementation::MainWindow>
    {
    };
}

```


- MainWindow.xaml.cpp
```ruby
// Copyright (c) Microsoft Corporation and Contributors.
// Licensed under the MIT License.

#include "pch.h"
#include "MainWindow.xaml.h"
#if __has_include("MainWindow.g.cpp")
#include "MainWindow.g.cpp"
#endif

using namespace winrt;
using namespace Microsoft::UI::Xaml;

// To learn more about WinUI, the WinUI project structure,
// and more about our project templates, see: http://aka.ms/winui-project-info.

namespace winrt::App4::implementation
{
    MainWindow::MainWindow()
    {
        InitializeComponent();
        px = 200;
        py = 200;
    }

    int32_t MainWindow::MyProperty()
    {
        throw hresult_not_implemented();
    }

    void MainWindow::MyProperty(int32_t /* value */)
    {
        throw hresult_not_implemented();
    }
}

using namespace winrt::Microsoft::Graphics::Canvas::UI::Xaml;
struct winrt::Windows::UI::Color col = winrt::Microsoft::UI::Colors::Orange();
void winrt::App4::implementation::MainWindow::CanvasControl_PointerMoved(winrt::Windows::Foundation::IInspectable const& sender, winrt::Microsoft::UI::Xaml::Input::PointerRoutedEventArgs const& e)
{
    CanvasControl canvas = sender.as<CanvasControl>();
    px = e.GetCurrentPoint(canvas).Position().X;
    py = e.GetCurrentPoint(canvas).Position().Y;
    canvas.Invalidate();
}


void winrt::App4::implementation::MainWindow::CanvasControl_Draw(winrt::Microsoft::Graphics::Canvas::UI::Xaml::CanvasControl const& sender, winrt::Microsoft::Graphics::Canvas::UI::Xaml::CanvasDrawEventArgs const& args)
{
    //args.DrawingSession().DrawEllipse(px, py, 80, 60, col, 8);
    //args.DrawingSession().DrawText(L"Hello!", px - 25, py - 15, winrt::Microsoft::UI::Colors::Yellow());
    args.DrawingSession().DrawCircle(150, 150, 100, col, 5);
    args.DrawingSession().DrawLine(300, 300, 500, 200, col, 5);
    args.DrawingSession().DrawRectangle(100, 400, 200, 100, col, 5);
    args.DrawingSession().FillRoundedRectangle(400, 500, 100, 200, 20, 20, col);
    args.DrawingSession().FillEllipse(700, 150, 200, 50, col);
    args.DrawingSession().FillCircle(700, 150, 100, col);
    args.DrawingSession().DrawText(L"Visual Studio_WinUI3", 600, 400, winrt::Microsoft::UI::Colors::Red());
}

```
![image](https://github.com/ksh19/Cpp/assets/102785836/1d2f5b4c-888a-4dde-958d-cf0abc551c6d)
