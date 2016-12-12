# Symbol Packages
# 符号包

In addition to building packages for nuget.org or other sources, NuGet also supports creating associated symbol packages and publishing them to the [SymbolSource repository](http://www.symbolsource.org/Public).
>除为 nuget.org 或其它源生成的包之外，NuGet 同时支持创建关联的符号包，以及发布它们到  [SymbolSource repository](http://www.symbolsource.org/Public)。

Package consumers can then add https://nuget.smbsrc.net/ to their symbol sources in Visual Studio. This allows consumers to step into your package code in the Visual Studio debugger.
>包消费者可以随后添加在 https://nuget.smbsrc.net/ 到 Visual Studio 中的符号源。这允许消费者在 Visual Studio 调试器中单步执行你的包代码。

## Creating a symbol package
## 创建一个符号包

To create a symbol package, follow these conventions:
>要创建一个符号包，需遵守下列的约定：

- Name the primary package (with your code) `{identifier}.nupkg` and include all your files except `.pdb` files.
- >主包（带有你的代码）的命名 `{identifier}.nupkg`，并包含除 `.pdb` 文件外的所有文件。
- Name the symbol package `{identifier}.symbols.nupkg` and include your assembly DLL, `.pdb` files, XMLDOC files, source files (see the sections that follow).
- >符号包的命名 `{identifier}.symbols.nupkg`，并包含程序集 DLL，`.pdb` 文件，XMLDOC 文件，源代码文件（参阅下面的章节）。

You can create both packages with the `-Symbols` option, either from a nuspec file or a project file:
>无论是从 nuspec 文件还是从项目文件，你都可以使用 `-Symbols` 选项创建包：

<code class="bash hljs">
	nuget pack MyPackage.nuspec -Symbols
</code>

<code class="bash hljs">
	nuget pack MyProject.csproj -Symbols
</code>

Note that `pack` requires Mono 4.4.2 on Mac OS X and does not work on Linux systems. On a Mac, you must also convert Windows pathnames in the `.nuspec` file to Unix-style paths.
>注意 `pack` 在 Mac OS X 上需要 Mono 4.4.2，并且在 Linux 系统上无法工作。在 Mac，你也必须转换 `.nuspec` 文件中的路径为 Unix 网格的路径。

## Symbol package structure
## 符号包的结构

A symbol package can target multiple target frameworks in the same way that a library package does, so the structure of the `lib` folder should be exactly the same as the primary package, only including `.pdb` files alongside the DLL.
>符号包与库程序包一样可以指定多个目标框架，因此 `lib` 文件夹应该与主包是一致的，但仅包含 `.pdb` 文件以及 DLL。

For example, a symbol package that targets .NET 4.0 and Silverlight 4 would have this layout:
>例如，一个目标为  .NET 4.0 和 Silverlight 4 的符号包应该是这样的布局：
	
	\lib
		\net40
			\MyAssembly.dll
			\MyAssembly.pdb
		\sl40
			\MyAssembly.dll
			\MyAssembly.pdb

Source files are then placed in a separate special folder named `src`, which must follow the relative structure of your source repository. This is because PDBs contain absolute paths to source files used to compile the matching DLL, and they need to be found during the publishing process. A base path (common path prefix) can be stripped out. For example, consider a library built from these files:
>源代码文件应该被放置到单独的文件夹  `src`，它必须遵循你的源代码仓库的结构。这是因为 PDBs 包含了编译 DLL 时使用的源代码文件的绝对路径，并且它们在发布处理期间需要被查找。
基本路径（常用路径前缀）可以被去掉。例如，考虑从这些文件中构建一个库：

	C:\Projects
		\MyProject
			\Common
				\MyClass.cs
			\Full
				\Properties
					\AssemblyInfo.cs
				\MyAssembly.csproj (producing \lib\net40\MyAssembly.dll)
			\Silverlight
				\Properties
					\AssemblyInfo.cs
				\MySilverlightExtensions.cs
				\MyAssembly.csproj (producing \lib\sl4\MyAssembly.dll)

Apart from the `lib` folder, a symbol package would need to contain this layout:
>除了 `lib` 文件夹，一个符号包将需要包含这些布局：

	\src
		\Common
			\MyClass.cs
		\Full
			\Properties
				\AssemblyInfo.cs
		\Silverlight
			\Properties
				\AssemblyInfo.cs
			\MySilverlightExtensions.cs

## Referring to files in the nuspec
## 在 nuspec 中引用文件

A symbol package can be built by conventions, from a folder structure as described in the previous section, or by specifying its contents in the `files` section of the manifest. For example, to build the package shown in the previous section, use the following in the `.nuspec` file:
>一个符号包可以通过约定来构建，从上一章节描述的文件夹结构或在清单  `files` 节中指定它的内容。例如，使用下面的 `.nuspec` 文件构建上一章节中演示的包。

    <files>
      <file src="Full\bin\Debug\*.dll" target="lib\net40" /> 
	  <file src="Full\bin\Debug\*.pdb" target="lib\net40" /> 
      <file src="Silverlight\bin\Debug\*.dll" target="lib\sl40" /> 
	  <file src="Silverlight\bin\Debug\*.pdb" target="lib\sl40" /> 
      <file src="**\*.cs" target="src" />
    </files>

## Publishing a symbol package
## 发布一个符号包

1. For convenience, first save your API key with NuGet (see [publish a package](/ndocs/create-packages/publish-a-package), which will apply to both nuget.org and symbolsource.org, because symbolsource.org will check with nuget.org to verify that you are the package owner.
1. >为方便起见，首先保存你的 NuGet 使用的 API 密钥（参阅 [publish a package](/ndocs/create-packages/publish-a-package)），它将应用于 nuget.org 和 symbolsource.org，因为 symbolsource.org 将通过 nuget.org 来验证你是包的所有者。

	<code class="bash hljs>   
		nuget SetApiKey Your-API-Key
	</code>

2. After publishing your primary package to nuget.org, push the symbol package as follows, which will automatically use symbolsource.org as the target because of the `.symbols` in the filename:
2. >在你发布主包到 nuget.org 后，如下所示发面符号包将自动使用 symbolsource.org 作为目标，因为文件名称中带有 `.symbols`：
	
	<code class="bash hljs>
 		nuget push MyPackage.symbols.nupkg
	</code>


3. To publish to a different symbol repository, or to push a symbol package that doesn't follow the naming convention, use the `-Source` option:
3. >要发布到不同的符号仓库或推送一个不符合名称约定的符号包，请使用 `-Source` 选项。

	<code class="bash hljs>
 		nuget push MyPackage.symbols.nupkg -source https://nuget.smbsrc.net/
	</code>


4. You can also push both primary and symbol packages to both repositories at the same time using the following:
4. >你也可以使用下面的方式在同一时间推送主包和符号包到仓库。
	
	<code class="bash hljs>
	 	nuget push MyPackage.nupkg
	</code>

	In this case, NuGet will publish `MyPackage.symbols.nupkg`, if present, to symbolsource.org (https://nuget.smbsrc.net/), after it publishes the primary package to nuget.org.
	>在这种情况下，NuGet 将发布 `MyPackage.symbols.nupkg`，如果存在，它发布主包到 nuget.org 后发布它到 symbolsource.org (https://nuget.smbsrc.net/)。


