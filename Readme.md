# Antlr4BuildTasks

This package is a build tool for Antlr4 C# programs with the Antl4.Runtime.Standard package.
It is based on Harwell's excellent [Antlr4cs](https://github.com/tunnelvisionlabs/antlr4cs),
also known as the NuGet package [Antlr4.CodeGenerator](https://www.nuget.org/packages/Antlr4.CodeGenerator/).
That package is fine, but it is several versions behind the current
Antlr4 tool. It also integrates the build rules, IDE support, templates, and tool, which you may
not want. Antlr4BuildTasks is a package
that focuses on only the build rules, and uses the currently maintained version of the Antlr tool
and runtime.

To use this package, add the Antlr4BuildTasks and Antlr4.Runtime.Standard packages
to your project, you can use the "NuGet Package Manager", or add the following lines to your .csproj file:

    <ItemGroup>
        <PackageReference Include="Antlr4.Runtime.Standard" Version="4.8" />
        <PackageReference Include="Antlr4BuildTasks" Version="8.2" />
    </ItemGroup>
    
Then, you will need to tag each Antlr4 grammar file you want the Antlr tool to run on. You can change the
"Build Action" property for the .g4 file from "None" to "ANTLR 4 grammar". Or, you can add the following lines
to your .csproj file for your "MyGrammar.g4" file:

    <ItemGroup>
        <Antlr4 Include="MyGrammar.g4" />
    </ItemGroup>
    
# Setting arguments to the Antlr tool

You can set the arguments to the Antlr Tool in Visual Studio by modifying the properties
for each grammar file. Or, you can modify the .csproj file to include the parameters you are
interested in setting:

    <ItemGroup>
        <Antlr4 Include="MyGrammar.g4">
            <Listener>false</Listener>
            <Visitor>false</Visitor>
            <GAtn>true</GAtn>
            <Package>foo</Package>
            <Error>true</Error>
        </Antlr4>
    </ItemGroup>


The tool takes the following parameters:

* &lt;Listener&gt; -- A bool that specifies whether you want an
Antlr Listener class generated by the tool.
* &lt;Visitor&gt; -- A bool that specifies whether you want an
Antlr Visitor class generated by the tool.
* &lt;GAtn&gt; -- A bool that specifies whether you want
Antlr to generate the ATN Dot diagrams.
* &lt;Package&gt; -- An C# identifier that specifies the namespace to wrap
the generated classes in.
* &lt;Error&gt; -- Use this to specify whether you want the tool to
flag warnings as errors and stop a build.
* &lt;LibPath&gt; -- A string that specifies the path for token and grammar files
for the Antlr tool.
* &lt;Encoding&gt; -- A string that specifies the encoding of the input grammars.
* &lt;DOptions&gt; -- A list of &lt;option&gt;=&lt;value&gt;, passed to the Antlr tool. E.g.,
language=Java.

Antlr generates files that may produce a lot of compiler warnings. To ignore those,
add the following &lt;PropertyGroup&gt; to you .csproj file.

    <PropertyGroup Condition="'$(Configuration)|$(Platform)'=='Debug|AnyCPU'">
        <NoWarn>3021;1701;1702</NoWarn>
    </PropertyGroup>

# Release notes

## Release notes for v8.2 (17 Aug 2020):

* Fix for Linux build.
