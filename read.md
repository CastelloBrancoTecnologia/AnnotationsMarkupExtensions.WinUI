﻿# WinUI 3 Markup Extension for Annotations

This project provides a WinUI 3 MarkupExtension called AnnotateExtension to bind UI properties (like Header, Description, PlaceHolder, MaxLength, etc.) directly from ViewModel annotations. It allows you to use data annotations on properties in your ViewModel and reflect them in your UI without manual XAML bindings.

# Features

- Automatically binds annotations like DisplayName, MaxLength, MinLength, Range, and custom attributes to UI elements.

- Supports CommunityToolkit.MVVM for easy ViewModel creation.

- Flexible fallback mechanism for finding fields with lowercase or underscore prefixes.

- Built-in logic for setting InputScope based on data annotations like EmailAddress, Password, Phone, etc.

# License

- This project is licensed under the MIT License.

Example ViewModel

Here is an example of a SignInViewModel using CommunityToolkit.MVVM. 

All fields are required, and annotations like MaxLength, MinLength, and Range are used to configure the form behavior.

``csharp

using CommunityToolkit.Mvvm.ComponentModel;
using System;
using System.ComponentModel.DataAnnotations;

public partial class SignInViewModel : ObservableObject
{
    [ObservableProperty]
    [Required]
    [StringLength(80, MinimumLength = 5)]
    [LowerCase]
    [Display(Name = "Full Name", Description = "Enter your full name", Prompt = "e.g., John Doe")]
    private string _name;

    [ObservableProperty]
    [Required]
    [StringLength(32, MinimumLength = 5)]
    [LowerCase]
    [Display(Name = "Username", Description = "Enter your desired username", Prompt = "e.g., johndoe")]
    private string _username;

    [ObservableProperty]
    [Required]
    [StringLength(16, MinimumLength = 6)]
    [Password]
    [Display(Name = "Password", Description = "Enter a secure password", Prompt = "e.g., ********")]
    private string _password;

    [ObservableProperty]
    [Required]
    [Range(typeof(DateTime), "01/01/1900", "12/31/2100")]
    [Display(Name = "Date of Birth", Description = "Select your date of birth", Prompt = "dd/mm/yyyy")]
    private DateTime _dateOfBirth;

    [ObservableProperty]
    [Required]
    [Range(0.5, 2.2)]
    [Display(Name = "Body Height", Description = "Enter your height in meters", Prompt = "e.g., 1.75")]
    private double _bodyHeight;
}
``

XAML Sign-In Form

This is a sample WinUI 3 sign-in form that uses the AnnotateExtension to bind UI properties like Header, Description, and Prompt directly from the ViewModel annotations.

``xml

<Page
    x:Class="YourAppNamespace.Views.SignInPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:l="using:YourAppNamespace.ViewModels"
    xmlns:y="using:YourAppNamespace.MarkupExtensions"
    mc:Ignorable="d"
    DataContext="{x:Bind l:SignInViewModel}">
    
    <StackPanel Spacing="20" Padding="20">
        <!-- Name -->
        <TextBox 
            Header="{y:Annotate Name, Header}" 
            Description="{y:Annotate Name, Description}"
            PlaceholderText="{y:Annotate Name, PlaceHolder}" 
            MaxLength="{y:Annotate Name, MaxLength}" 
            InputScope="{y:Annotate Name, BestInputScope}"
            Text="{x:Bind Name, Mode=TwoWay}" />

        <!-- Username -->
        <TextBox 
            Header="{y:Annotate Username, Header}" 
            Description="{y:Annotate Username, Description}" 
            PlaceholderText="{y:Annotate Username, PlaceHolder}" 
            MaxLength="{y:Annotate Username, MaxLength}" 
            InputScope="{y:Annotate Username, BestInputScope}"
            Text="{x:Bind Username, Mode=TwoWay}" />

        <!-- Password -->
        <PasswordBox 
            Header="{y:Annotate Password, Header}" 
            Description="{y:Annotate Password, Description}" 
            PlaceholderText="{y:Annotate Password, PlaceHolder}" 
            MaxLength="{y:Annotate Password, MaxLength}" 
            InputScope="{y:Annotate Password, BestInputScope}"
            Password="{x:Bind Password, Mode=TwoWay}" />

        <!-- Date of Birth -->
        <DatePicker 
            Header="{y:Annotate DateOfBirth, Header}" 
            Description="{y:Annotate DateOfBirth, Description}" 
            PlaceholderText="{y:Annotate DateOfBirth, PlaceHolder}" 
            Date="{x:Bind DateOfBirth, Mode=TwoWay}" />

        <!-- Body Height -->
        <TextBox 
            Header="{y:Annotate BodyHeight, Header}" 
            Description="{y:Annotate BodyHeight, Description}" 
            PlaceholderText="{y:Annotate BodyHeight, PlaceHolder}" 
            InputScope="{y:Annotate BodyHeight, BestInputScope}" 
            Text="{x:Bind BodyHeight, Mode=TwoWay}" />
        
        <!-- Submit Button -->
        <Button Content="Submit" Click="OnSubmit"/>
    </StackPanel>
</Page>
``

# Key Features in XAML:

- Header: Binds to the Display(Name) attribute from the ViewModel.

- Description: Binds to the Display(Description) attribute.

- PlaceholderText: Binds to the Display(Prompt) attribute.

- MaxLength: Binds to the MaxLength or StringLength annotation.

- InputScope: Automatically chooses the best input scope based on data types like Email, Password, or custom logic.

# Installation & Usage

- Add the AnnotateExtension package  to your WinUI 3 project. -

- Annotate your ViewModel properties using attributes like [Display], [StringLength], [Range], [Required], etc.

 - Use the AnnotateExtension in XAML to bind UI properties like Header, MaxLength, and PlaceholderText directly from the ViewModel.

With this setup, you can automatically keep your UI and data logic in sync using the power of data annotations and the flexibility of WinUI 3.