# NuGet CLI Reference
# NuGet CLI 参考

The NuGet Command Line Interface (CLI) provides the full extent of NuGet functionality to install, create, publish, and manage packages. Refer to the [Install Guide](/ndocs/guides/install-nuget) for installation instructions.

Available commands are listed below. Also see the section on [Environment variables](#environment-variables) for how they're used with nuget.exe.

<table>
    <tr>
        <th>Command <br /> 命令</th>
        <th>Description <br /> 描述</th>
        <th>NuGet Version <br /> 版本</th>
    </tr>
    <tr>
        <td><a href="#add">add</a></td>
        <td>Adds a package to a non-HTTP package source using hierarchical layout. For HTTP sources, use <em>push</em>.</td>
        <td>3.3+</td>
    </tr>
    <tr>
        <td><a href="#config">config</a></td>
        <td>Gets or sets NuGet configuration values.</td>
        <td>All</td>
    </tr>
    <tr>
        <td><a href="#delete">delete</a></td>
        <td>Removes or unlists a package from a package source.</td>
        <td>All</td>
    </tr>
    <tr>
        <td><a href="#help">help or ?</a></td>
        <td>Displays help information or help for a command.</td>
        <td>All</td>
    </tr>
    <tr>
        <td><a href="#init">init</a></td>
        <td>Adds packages from a folder to a package source using hierarchical layout.</td>
        <td>3.3+</td>
    </tr>
    <tr>
        <td><a href="#install">install</a></td>
        <td>Installs a package into the current project but does not modify the project or packages.config.</td>
        <td>All</td>
    </tr>
    <tr>
        <td><a href="#list">list</a></td>
        <td>Displays packages from a given source.</td>
        <td>All</td>
    </tr>
    <tr>
        <td><a href="#locals">locals</a></td>
        <td>Clears or lists packages in various caches or the global packages folder, or identifies those folders.</td>
        <td>3.3+</td>
    </tr>
    <tr>
        <td><a href="#mirror">mirror</a></td>
        <td>Mirrors a package and its dependencies from a source to a target repository.</td>
        <td>Deprecated in 3.2+</td>
    </tr>
    <tr>
        <td><a href="#pack">pack</a></td>
        <td>Creates a NuGet package from a .nuspec or project file.</td>
        <td>2.7+</td>
    </tr>
    <tr>
        <td><a href="#push">push</a></td>
        <td>Publishes a package to a package source. <br /> 将包发布到包源。</td>
        <td>All</td>
    </tr>
    <tr>
        <td><a href="#restore">restore</a></td>
        <td>Restores all packages referenced by packages.config or project.json.</td>
        <td>2.7+</td>
    </tr>
    <tr>
        <td><a href="#setapikey">setapikey</a></td>
        <td>Saves an API key for a given package source.</td>
        <td>All</td>
    </tr>
    <tr>
        <td><a href="#sources">sources</a></td>
        <td>Manages package sources in configuration files.</td>
        <td>All</td>
    </tr>
    <tr>
        <td><a href="#spec">spec</a></td>
        <td>Generates a .nuspec file, using tokens if generating the file from a Visual Studio project.</td>
        <td>All</td>
    </tr>
    <tr>
        <td><a href="#update">update</a></td>
        <td>Updates a project's packages to the latest available versions.</td>
        <td>All</td>
    </tr>
</table>


## add

*Version 3.3+*

Adds a specified package to a non-HTTP package source (a folder or UNC path) in a hierarchical layout, wherein folders are created for the package ID and version number. For example:

    \\myserver\packages
      └─<packageID>
        └─<version>
          ├─<packageID>.<version>.nupkg
          ├─<packageID>.<version>.nupkg.sha512
          └─<packageID>.nuspec

When restoring or updating against the package source, hierarchical layout provides significantly better performance.

To expand all the files in the package to the destination package source, use the `-expand` switch. This typically results in additional subfolders appearing in the destination, such as `tools` and `lib`.

#### Usage

    nuget add <packagePath> -source <sourcePath> [options]

where &lt;packagePath&gt; is the pathname to the package to add, and &lt;sourcePath&gt; specifies the folder-based package source to which the package will be added. HTTP sources are not supported.

#### Options

<table>
    <tr>
        <td>expand</td>
        <td>If provided, all the files in the package are added to your package source.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the command.</td>
    </tr>
    <tr>
        <td>fileconflictaction</td>
        <td><em>(2.5+)</em> Specifies the action to take when asked to overwrite or ignore existing files referenced by the project. Values are <em>overwrite, ignore, none</em>.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the output: <em>normal</em>, <em>quiet</em>, <em>detailed</em>.</td>
    </tr>
</table>

#### Examples

    nuget add foo.nupkg -source c:\bar\

    nuget add foo.nupkg -source \\bar\packages\


## config

Gets or sets NuGet config values. For additional usage, see [Configuring NuGet Behavior](/ndocs/consume-packages/configuring-nuget-behavior). For details on allowable key names, refer to the [NuGet config file reference](/ndocs/schema/nuget.config-file).

#### Usage

    nuget config -set <name>=<value> [<name>=<value> ...] [options]

where &lt;name&gt; and &lt;value&gt; specify a key-value pair to be set in the configuration. You can specify as many pairs as desired.

In NuGet 3.4+, &lt;value&gt; can be use environment variables.

#### Options

<table>
    <tr>
        <td>configfile</td>
        <td><em>(2.5+)</em> The NuGet configuration file to modify. If not specified,
        <em>%AppData%\NuGet\NuGet.config</em> is used.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the command.</td>
    </tr>
    <tr>
        <td>noninteractive</td>
        <td>Suppresses prompts for user input or confirmations.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the output: <em>normal</em>, <em>quiet</em>, <em>detailed (2.5+)</em>.</td>
    </tr>
</table>

#### Examples

    nuget config -set repositoryPath=c:\packages -configfile c:\my.config

    nuget config -set repositoryPath=%PACKAGE_REPO% -configfile %ProgramData%\NuGet\NuGetDefaults.Config

    nuget config -set HTTP_PROXY=http://127.0.0.1 -set HTTP_PROXY.USER=domain\user


## delete

Deletes or unlists a package from a package source. For nuget.org, the action is to [unlist the package](/ndocs/policies/deleting-packages).
>从包源中删除或下架一个包。在 nuget.org 中，这个行为称为 [unlist the package](/ndocs/policies/deleting-packages)。

#### Usage
#### 用法

    nuget delete <packageID> <packageVersion> [options]

where &lt;packageID&gt; and &lt;packageVersion&gt; identify the exact package to delete or unlist. The exact behavior depends on the source. For local folders, for instance, the package is deleted; for nuget.org the package is unlisted.
>&lt;packageID&gt; and &lt;packageVersion&gt; 标识了将要删除或下架的包。确切的行为依赖于包源。以本地文件夹为例包将被删除；对于 nuget.org 包则被下架。

#### Options
#### 选项

<table>
    <tr>
        <td>apikey</td>
        <td>
            The API key for the target repository. If not present,  the one specified in <em>%AppData%\NuGet\NuGet.config</em> is used. <br />
            目标存储库的 API 键。如果未指定，将使用 %AppData%\NuGet\NuGet.config 中指定的。
        </td>
    </tr>
    <tr>
        <td>configfile</td>
        <td>
            <em>(2.5+)</em> The NuGet configuration file to modify. If not specified, <em>%AppData%\NuGet\NuGet.config</em> is used. <br />
            (2.5+) 将修改的 NuGet 配置文件。如果未指定，将使用 %AppData%\NuGet\NuGet.config。
        </td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the command. <br /> 显示命令的帮助信息。</td>
    </tr>
    <tr>
        <td>noninteractive</td>
        <td>Suppresses prompts for user input or confirmations. <br /> 取消用户输入或确认的提示。</td>
    </tr>
    <tr>
        <td>source</td>
        <td>
            Specifies the server URL. Supported URLs for nuget.org include <em>https://www.nuget.org, https://www.nuget.org/api/v3, https://www.nuget.org/api/v2/package</em>. For private feeds, substitute the host name, for example, <em>%hostname%/api/v3</em>. <br />
            指定服务器 URL。对于 nuget.org，支持 <em>https://www.nuget.org, https://www.nuget.org/api/v3, https://www.nuget.org/api/v2/package</em> 等 URL。对于私有订阅源，需要替代主机名称，例如，<em>%hostname%/api/v3</em>。
        </td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>
            Specifies the amount of details displayed in the output: <em>normal</em>, <em>quiet</em>, <em>detailed (2.5+)</em>. <br />
            指定输出中显示的详细程序：normal, quiet, detailed (2.5+)。
        </td>
    </tr>
</table>

#### Examples
#### 示例

    nuget delete MyPackage 1.0

    nuget delete MyPackage 1.0 -source http://package.contoso.com/source -apikey A1B2C3


## help

Displays general help information and help information about specific commands.

#### Usage

    nuget help [command] [options]
    nuget ? [command] [options]

where [command] identifies a specific command for which to display help.

<div class="block-callout-warning">
    <strong>Note</strong><br>
    With some commands, be mindful to specify <em>help</em> first, as with <em>nuget help install</em>, because there is a package named "help" on nuget.org. If you give the command <em>nuget install help</em>, you'll not get help on the install command but will instead install the help package.
</div>

#### Options

<table>
    <tr>
        <td>all</td>
        <td>Print detailed help for all available commands; ignored if a specific command is given.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the help command itself.</td>
    </tr>
    <tr>
        <td>markdown</td>
        <td>Print detailed help in markdown format when used with -all. Ignored otherwise.</td>
    </tr>
    <tr>
        <td>noninteractive</td>
        <td>Suppresses prompts for user input or confirmations.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the output: <em>normal</em>, <em>quiet</em>, <em>detailed (2.5+)</em>.</td>
    </tr>
</table>

#### Examples

    nuget help
    nuget help push
    nuget ?
    nuget push -?
    nuget help -all -markdown


## init

*Version 3.3+*

Copies all the packages from a flat folder to a destination folder using a hierarchical layout as described for the [add command](#add) above. That is, using `init` is equivalent to using the `add` command on each package in the folder.

As with `add`, the destination must be either a local folder or a UNC path; HTTP package repositories such as nuget.org or private servers are not supported.


#### Usage

    nuget init <source> <destination> [options]

where &lt;source&gt; is the folder containing packages and &lt;destination&gt; is the local folder or UNC pathname to which the packages will be copied.

#### Options

<table>
    <tr>
        <td>expand</td>
        <td>Adds the files in the package(s) to destination package source.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the command.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the output: <em>normal</em>, <em>quiet</em>, <em>detailed (2.5+)</em>.</td>
    </tr>
</table>

#### Examples

    nuget init c:\foo c:\bar
    nuget init \\foo\packages \\bar\packages -expand


## install

Downloads and installs a package into a project using the specified package sources. If no sources are specified, those listed in the global configuration file, `%APPDATA%\NuGet\NuGet.Config`, will be used. See [Configuring NuGet Behavior](/ndocs/consume-packages/configuring-nuget-behavior) for additional details.

If no specific packages are specified, `install` installs all packages listed in the project's `packages.config` file, making it similar to [`restore`](#restore). In this case, `install` does not work with projects that use `project.json`.

The `install` command does not modify a project file or `packages.config`; in this way it's similar to `restore` in that it only adds packages to disk but does not change a project's dependencies.

To add a dependency, either add a project through the Package Manager UI or Console in Visual Studio, or modify `packages.config` and then run either `install` or `restore`. For projects using `project.json`, you can modify that file and then run `restore`.


#### Usage

    nuget install <packageID | configFilePath> [options]

where &lt;packageID&gt; names the package to install (using the latest version), or &lt;configFilePath&gt; identifies the `packages.config` file that lists the packages to install. You can indicate a specific version with the `-version` option.


#### Options

<table>
    <tr>
        <td>configfile</td>
        <td><em>(2.5+)</em> The NuGet configuration file to modify. If not specified,
        <em>%AppData%\NuGet\NuGet.config</em> is used.</td>
    </tr>
    <tr>
        <td>excludeversion</td>
        <td>Excludes the version number from the installation folder.</td>
    </tr>
    <tr>
        <td>fileconflictaction</td>
        <td><em>(2.5+)</em> Specifies the action to take when asked to overwrite or ignore existing files referenced by the project. Values are <em>overwrite, ignore, none</em>.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the command.</td>
    </tr>
    <tr>
        <td>nocache</td>
        <td>Prevents NuGet from using packages from local machine caches.</td>
    </tr>
    <tr>
        <td>noninteractive</td>
        <td>Suppresses prompts for user input or confirmations.</td>
    </tr>
    <tr>
        <td>outputdirectory</td>
        <td>Specifies the folder in which packages are installed. If no folder is specified, the current folder is used.</td>
    </tr>
    <tr>
        <td>prerelease</td>
        <td>Allows prerelease packages to be installed. This flag is not required when restoring packages with <em>packages.config</em>.</td>
    </tr>
    <tr>
        <td>requireconsent</td>
        <td>Verifies that restoring packages is enabled before downloading and installing the packages. For details, see <a href="/ndocs/consume-packages/package-restore">Package Restore</a>.</td>
    </tr>
    <tr>
        <td>solutiondirectory</td>
        <td>Specifies root folder of the solution for which to restore packages.</td>
    </tr>
    <tr>
        <td>source</td>
        <td>Specifies a list of package sources to use.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the output: <em>normal</em>, <em>quiet</em>, <em>detailed (2.5+)</em>.</td>
    </tr>
    <tr>
        <td>version</td>
        <td>Specifies the version of the package to install.</td>
    </tr>
</table>

#### Examples

    nuget install elmah

    nuget install packages.config

    nuget install ninject -outputdirectory c:\proj


##  list

Displays a list of packages from a given source. If no sources are specified, all sources defined in the global configuration file, `%AppData%\NuGet\NuGet.config`, are used. If `NuGet.config` specifies no sources, then `list` uses the default feed (nuget.org).

#### Usage

    nuget list [search terms] [options]

where the optional search terms will filter the displayed list. Search terms are applied to the names of packages, tags, and package descriptions.

#### Options

<table>
    <tr>
        <td>allversions</td>
        <td>List all versions of a package. By default, only the latest package version is displayed.</td>
    </tr>
    <tr>
        <td>configfile</td>
        <td><em>(2.5+)</em> The NuGet configuration file to modify. If not specified,
        <em>%AppData%\NuGet\NuGet.config</em> is used.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the command.</td>
    </tr>
    <tr>
        <td>noninteractive</td>
        <td>Suppresses prompts for user input or confirmations.</td>
    </tr>
    <tr>
        <td>prerelease</td>
        <td>Includes prerelease packages in the list.</td>
    </tr>
    <tr>
        <td>source</td>
        <td>Specifies a list of packages sources to search.</td>
    </tr>
    <tr>
        <td>verbose</td>
        <td>Displays a detailed list of information for each package.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the output: <em>normal</em>, <em>quiet</em>, <em>detailed (2.5+)</em>.</td>
    </tr>
</table>

#### Examples

    nuget list

    nuget list -verbose -allversions

## locals

*Version 3.3+*

Clears or lists local NuGet resources such as the http-request cache, packages cache, and the machine-wide global packages folder. The `locals` command can also be used to display a list of those locations. For more information, see [Managing the NuGet Cache](/ndocs/consume-packages/managing-the-nuget-cache).

#### Usage

    nuget locals <cache> [options]

where &lt;cache&gt; is one of `all`, `http-cache`, `packages-cache`, `global-packages`, and `temp` *(3.4+)*.


#### Options

<table>
    <tr>
        <td>clear</td>
        <td>Clear the specified cache.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the command.</td>
    </tr>
    <tr>
        <td>list</td>
        <td>List the location of the specified cache, or the locations of all caches when used with <em>all</em>.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the output: <em>normal</em>, <em>quiet</em>, <em>detailed</em>.</td>
    </tr>
</table>

#### Examples

    nuget locals all -list
    nuget locals http-cache -clear


## mirror

*Deprecated in 3.2+*

Mirrors a package and its dependencies from the specified source repositories to the target repository.

<div class="block-callout-info">
    <strong>Note</strong><br>
    To enable this command for NuGet versions before 3.2, go to <a href="https://nuget.codeplex.com/releases">https://nuget.codeplex.com/releases</a>, select the newest stable release, download NuGet.ServerExtensions.dll and Nuget-Signed.exe to your local disk and rename the Nuget-Signed.Exe to nuget.exe.
</div>

#### Usage
    nuget mirror <packageID | configFilePath> <listUrlTarget> <publishUrlTarget> [options]

where &lt;packageID&gt; is the package to mirror, or &lt;configFilePath&gt; identifies the `packages.config` file that lists the packages to mirror.

The &lt;listUrlTarget&gt; specifies the source repository, and &lt;publishUrlTarget&gt; specifies the target repository.

If your target repository is on https://machine/repo that's running [NuGet.Server](/ndocs/hosting-packages/nuget.server), the list and push urls will be *https://machine/repo/nuget* and *https://machine/repo/api/v2/package*, respectively.

#### Options

<table>
    <tr>
        <td>apikey</td>
        <td>The API key for the target repository. If not present,  the one specified in <em>%AppData%\NuGet\NuGet.config</em> is used.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the command.</td>
    </tr>
    <tr>
        <td>nocache</td>
        <td>Prevents NuGet from using packages from local machine caches.</td>
    </tr>
    <tr>
        <td>noop</td>
        <td>Logs what would be done but does not perform the actions; assumes success for push operations.</td>
    </tr>
    <tr>
        <td>prerelease</td>
        <td>Includes prerelease packages in the mirroring operation.</td>
    </tr>
    <tr>
        <td>source</td>
        <td>A list of package sources to mirror. If no sources are specified, the ones defined in <em>%AppData%\NuGet\NuGet.config</em> are used, defaulting to nuget.org if none are specified.</td>
    </tr>
    <tr>
        <td>timeout</td>
        <td>Specifies the timeout, in seconds, for pushing to a server. The default is 300 seconds (5 minutes).</td>
    </tr>
    <tr>
        <td>version</td>
        <td>The version of the package to install. If not specified, the latest version is mirrored.</td>
    </tr>
</table>

#### Examples

    nuget mirror packages.config  https://MyRepo/nuget https://MyRepo/api/v2/package -source https://nuget.org/api/v2 -apikey myApiKey -nocache

    nuget mirror Microsoft.AspNet.Mvc https://MyRepo/nuget https://MyRepo/api/v2/package -version 4.0.20505.0

    nuget mirror Microsoft.Net.Http https://MyRepo/nuget https://MyRepo/api/v2/package -prerelease


## pack
*Version 2.7+*

Creates a NuGet package based on the specified nuspec or project file. Note that the `pack` command requires MSBuild and will not work on Linux systems. On Mac OS X, you need to have Mono 4.4.2 installed.

With Mono, you also need to adjust non-local paths in the .nuspec file to Unix-style paths, as nuget.exe will not itself convert Windows pathnames.

#### Usage

    nuget pack <nuspecPath | projectPath> [options]

where &lt;nuspecPath&gt; and &lt;projectPath&gt; specify the `.nuspec` or project file, respectively.

#### Options

<table>
    <tr>
    </tr>
    <tr>
        <td>basepath</td>
        <td>Sets the base path of the files defined in the nuspec file.</td>
    </tr>
    <tr>
        <td>build</td>
        <td>Specifies that the project should be built before building the package.</td>
    </tr>
    <tr>
        <td>exclude</td>
        <td>Specifies one or more wildcard patterns to exclude when creating a package.</td>
    </tr>
    <tr>
        <td>excludeemptydirectories</td>
        <td>Prevent inclusion of empty directories when building the package.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the command.</td>
    </tr>
    <tr>
        <td>includereferencedprojects</td>
        <td>Indicates that the built package should include referenced projects either as dependencies or as part of the package. If a referenced project has a corresponding nuspec file that has the same name as the project, then that
        referenced project is added as a dependency. Otherwise, the referenced project is added as part of the package.</td>
    </tr>
    <tr>
        <td>minclientversion</td>
        <td>Set the <em>minClientVersion</em> attribute for the created package. This value will override the value of the existing <em>minClientVersion</em> attribute (if any) in the .nuspec file.</td>
    </tr>
    <tr>
        <td>msbuildversion</td>
        <td>Specifies the version of MSBuild to be used with this command. Supported values are 4, 12, 14. By default the MSBuild in your path is picked, otherwise it defaults to the highest installed version of MSBuild.</td>
    </tr>
    <tr>
        <td>nodefaultexcludes</td>
        <td>Prevents default exclusion of NuGet package files and files and folders starting with a dot, such as <em>.svn</em>.</td>
    </tr>
    <tr>
        <td>nopackageanalysis</td>
        <td>Specifies that pack should not run package analysis after building the package.</td>
    </tr>
    <tr>
        <td>outputdirectory</td>
        <td>Specifies the folder in which the created package is stored. If no folder is specified, the current folder is used.</td>
    </tr>
    <tr>
        <td>properties</td>
        <td>Specifies a list of token=value pairs, separated by semicolons, where each occurrence of $token$ in the nuspec file will be replaced with the given value. Values can be strings in quotation marks.</td>
    </tr>
    <tr>
        <td>suffix</td>
        <td>Appends a suffix to the internally generated version number, typically used for appending build or other pre-release identifiers. For example, using <code>-suffix nightly</code> will create a package with a version number like <code>1.2.3.45-nightly</code>.</td>
    </tr>
    <tr>
        <td>symbols</td>
        <td>Specifies that the package contains sources and symbols. When used with a with a nuspec file, this creates a regular NuGet package file and the corresponding symbols package.</td>
    </tr>
    <tr>
        <td>tool</td>
        <td>Specifies that the output files of the project should be placed in the tool folder.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the output: <em>normal</em>, <em>quiet</em>, <em>detailed</em>.</td>
    </tr>
    <tr>
        <td>version</td>
        <td>Overrides the version number from the nuspec file.</td>
    </tr>
</table>

#### Excluding development dependencies

Some NuGet packages are useful as development dependencies, which help you author your own library, but aren't necessarily needed as actual package dependencies.

The `pack` command will ignore `package` entries in `packages.config`
that have the `developmentDependency` attribute set to `true`. These entries will not be include as a dependencies in the created package.

For example, consider the following `packages.config` file in the source project:

    <?xml version="1.0" encoding="utf-8"?>
    <packages>
        <package id="jQuery" version="1.5.2" />
        <package id="netfx-Guard" version="1.3.3.2" developmentDependency="true" />
        <package id="microsoft-web-helpers" version="1.15" />
    </packages>

For this project, the package created by `nuget pack` will have a dependency on `jQuery` and `microsoft-web-helpers` but not `netfx-Guard`.

#### Examples

    nuget pack

    nuget pack foo.nuspec

    nuget pack foo.csproj

    nuget pack foo.csproj -Build -symbols -properties owners=janedoe,xiaop;version="1.0.5"

    # create a package from project foo.csproj, using MSBuild version 12 to build the project
    nuget pack foo.csproj -Build -symbols -properties owners=janedoe,xiaop;version="1.0.5" -MSBuildVersion 12

    nuget pack foo.nuspec -version 2.1.0

    nuget pack foo.nuspec -version 1.0.0 -minclientversion 2.5


## push

Pushes a package to a package source and publishes it.
>推送包到包源，并发布它。

NuGet's default configuration is obtained by loading `%AppData%\NuGet\NuGet.config`, then loading any `nuget.config` or `.nuget\nuget.config` files starting from root of drive and ending in current directory (see [Configuring NuGet Behavior](/ndocs/consume-packages/configuring-nuget-behavior))
>NuGet 的默认配置是通过加载 `%AppData%\NuGet\NuGet.config`，然后加载任意的 `nuget.config` 或 `.nuget\nuget.config` 文件（在驱动器根到当前目录）。（参阅 [Configuring NuGet Behavior](/ndocs/consume-packages/configuring-nuget-behavior)）

#### Usage
#### 用法

    nuget push <packagePath> [options]

where &lt;packagePath&gt; identifies the package to push to the server.
>&lt;packagePath&gt; 代表包将要被推送的服务器。

#### Options
#### 选项

<table>
    <tr>
        <td>apikey</td>
        <td>
            The API key for the target repository. If not present,  the one specified in <em>%AppData%\NuGet\NuGet.config</em> is used. <br />
            目标存储库的 API 键。如果未指定，将使用 <em>%AppData%\NuGet\NuGet.config</em> 中指定的。
        </td>
    </tr>
    <tr>
        <td>configfile</td>
        <td>
            <em>(2.5+)</em> The NuGet configuration file to modify. If not specified, <em>%AppData%\NuGet\NuGet.config</em> is used. <br />
            <em>(2.5+)</em> 将修改的 NuGet 配置文件。如果未指定，将使用 <em>%AppData%\NuGet\NuGet.config</em>。
        </td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the command. <br /> 显示命令的帮助信息。</td>
    </tr>
    <tr>
        <td>noninteractive</td>
        <td>Suppresses prompts for user input or confirmations. <br /> 取消用户输入或确认的提示。</td>
    </tr>
    <tr>
        <td>source</td>
        <td>
            Specifies the server URL. With NuGet 2.5+, NuGet will identify a UNC or local folder source and simply copy the file there instead of pushing it using HTTP.  Also, starting with NuGet 3.4.2, this is a mandatory parameter unless the NuGet.config file specifies a <em>DefaultPushSource</em> value. <br />
            指定服务 URL。在 NuGet 2.5+ 中，当指定一个 UNC 或本地文件夹源时仅简单的复制而不需要使用 HTTP 进行推送。
            同样的，从 NuGet 3.4.2 开始，这是一个强制性参数除非在配置文件 NuGet.config 中指定了 <em>DefaultPushSource</em> 的值。
        </td>
    </tr>
    <tr>
        <td>timeout</td>
        <td>
            Specifies the timeout, in seconds, for pushing to a server. The default is 300 seconds (5 minutes). <br />
            指定推送到服务器的超时时间，以秒为单位。默认为 300 秒（即 5 分钟）。
        </td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>
            Specifies the amount of details displayed in the output: <em>normal</em>, <em>quiet</em>, <em>detailed (2.5+)</em>. <br />
            指定输出中显示的详细程序：<em>normal</em>, <em>quiet</em>, <em>detailed (2.5+)</em>。
        </td>
    </tr>
</table>

#### Examples
#### 示例

    nuget push foo.nupkg

    nuget push foo.symbols.nupkg

    nuget push foo.nupkg -Timeout 360

    nuget push *.nupkg

    nuget.exe push -source \\mycompany\repo\ mypackage.1.0.0.nupkg

    nuget push foo.nupkg 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -Source https://www.nuget.org/api/v2/package

    nuget push foo.nupkg 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a

    nuget push foo.nupkg 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -s https://customsource/



## restore

*Version 2.7+*

NuGet 2.7+: Downloads and installs any packages missing from the `packages` folder.

NuGet 3.3+ with projects using `project.json`: Generates a `project.lock.json` file and a `<project>.nuget.props` file, if needed. (Both files can be omitted from source control.)

NuGet 4.0+ with project in which package references are included in the project file directly: Generates a `<project>.nuget.props` file, if needed, in the `obj` folder. (The file can be omitted from source control.)

#### Usage

    nuget restore <projectPath> [options]

where &lt;projectPath&gt; specifies the location of a solution, a `packages.config` file, or a `project.json` file. See [Remarks](#remarks) below for behavioral details.

#### Options

<table>
    <tr>
        <td>configfile</td>
        <td>The NuGet configuration file to modify. If not specified,
        <em>%AppData%\NuGet\NuGet.config</em> is used.</td>
    </tr>
    <tr>
        <td>disableparallelprocessing</td>
        <td>Disables parallel NuGet project restores and package downloads.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the command.</td>
    </tr>
    <tr>
        <td>msbuildversion</td>
        <td>Specifies the version of MSBuild to be used with this command. Supported values are 4, 12, 14, 15. By default the MSBuild in your path is picked, otherwise it defaults to the highest installed version of MSBuild.</td>
    </tr>
    <tr>
        <td>nocache</td>
        <td>Prevents NuGet from using packages from local machine caches.</td>
    </tr>
    <tr>
        <td>noninteractive</td>
        <td>Suppresses prompts for user input or confirmations.</td>
    </tr>
    <tr>
        <td>outputdirectory or -packagesdirectory</td>
        <td>Specifies the folder in which packages are installed. If no folder is specified, the current folder is used.</td>
    </tr>
    <tr>
        <td>requireconsent</td>
        <td>Verifies that restoring packages is enabled before downloading and installing the packages. For details, see <a href="/ndocs/consume-packages/package-restore">Package Restore</a>.</td>
    </tr>
    <tr>
        <td>solutiondirectory</td>
        <td>Specifies the solution folder. Not valid when restoring
        packages for a solution.</td>
    </tr>
    <tr>
        <td>source</td>
        <td>Specifies list of package sources to use for the restore.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the output: <em>normal</em>, <em>quiet</em>, <em>detailed (2.5+)</em>.</td>
    </tr>
</table>

#### Remarks

The restore command is executed in the following steps:

1. Determine the operation mode of the restore command.

    <table>
        <tr>
            <th>projectPath file type</th>
            <th>Behavior</th>
        </tr>
        <tr>
            <td>Solution (folder)</td>
            <td>NuGet looks for a .sln file and uses that if found; otherwise gives an error. $(SolutionDir)\.nuget is used as the starting folder.</td>
        </tr>
        <tr>
            <td>.sln file</td>
            <td>Restore packages identified by the solution; gives an error if -solutiondirectory is used. $(SolutionDir)\.nuget is used as the starting folder.</td>
        </tr>
        <tr>
            <td>packages.config,<br>project.json, or <br>project file</td>
            <td>Restore packages listed in this file, resolving and installing dependencies.</td>
        </tr>
        <tr>
            <td>Other file type</td>
            <td>File is assumed to be a .sln file as above; if it's not a solution, NuGet gives an error.</td>
        </tr>
        <tr>
            <td>(projectPath not specified)</td>
            <td>
                <ul>
                    <li>NuGet looks for solution files in the current folder. If a single file is found, that one is used to restore packages; if multiple solutions are found, NuGet gives an error.</li>
                    <li>If there are no solution files, NuGet looks for a packages.config or project.json and uses that to restore packages.</li>
                    <li>If no solution file, packages.config, or project.json is found, NuGet gives an error.</li>
                </ul>
            </td>
        </tr>
    </table>


2. Determine the packages folder using the following priority order (NuGet gives an error if none of these folders are found):

    * The `%userprofile%\.nuget\packages` value in `project.json`.
    * The folder specified with `-packagesdirectory`.
    * The `repositoryPath` vale in `nuget.config`
    * The folder specified with `-solutiondirectory`
    * `$(SolutionDir)\packages`


3. When restoring packages for a solution, NuGet does the following:
    * Loads the solution file.
    * Restores solution level packages listed in `$(SolutionDir)\.nuget\packages.config` into the `packages` folder.
    * Restore packages listed in `$(ProjectDir)\packages.config` into the `packages` folder. For each package specified, restore the package in parallel unless `-disableparallelprocessing` is specified.

#### Examples

    # Restore packages for a solution file
    nuget restore a.sln

    # Restore packages for a solution file, using MSBuild version 14.0 to load the solution and its project(s)
    nuget restore a.sln -MSBuildVersion 14

    # Restore packages for a project's packages.config file, with the packages folder at the parent
    nuget restore proj1\packages.config -PackagesDirectory ..\packages

    # Restore packages for the solution in the current folder, specifying package sources
    nuget restore -source "https://www.nuget.org/api/v2;https://www.myget.org/F/nuget"


##  setapikey

Saves an API key for a given server URL into `NuGet.Config` so that it doesn't need to be entered for subsequent commands.

#### Usage

    nuget setapikey <key> -source <url> [options]

where &lt;source&gt; identifies the server and &lt;key&gt; is the key or password to save. If &lt;source&gt; is omitted, nuget.org is assumed.

#### Options

<table>
    <tr>
        <td>configfile</td>
        <td><em>(2.5+)</em> The NuGet configuration file to modify. If not specified,
        <em>%AppData%\NuGet\NuGet.config</em> is used.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the command.</td>
    </tr>
    <tr>
        <td>noninteractive</td>
        <td>Suppresses prompts for user input or confirmations.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the output: <em>normal</em>, <em>quiet</em>, <em>detailed (2.5+)</em>.</td>
    </tr>
</table>

#### Examples

    nuget setapikey 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a

    nuget setapikey 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -source https://example.com/nugetfeed


## sources

Manages the list of sources located in `%AppData%\NuGet\NuGet.config` or the specified configiration file.

#### Usage

    nuget sources <operation> -Name <name> -Source <source>

where &lt;operation&gt; is one of *List, Add, Remove, Enable, Disable,* or *Update*, &lt;name&gt; is the name of the source, and &lt;source&gt; is the source's URL.

#### Options

<table>
    <tr>
        <td>configfile</td>
        <td><em>(2.5+)</em> The NuGet configuration file to modify. If not specified,
        <em>%AppData%\NuGet\NuGet.config</em> is used.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the command.</td>
    </tr>
    <tr>
        <td>noninteractive</td>
        <td>Suppresses prompts for user input or confirmations.</td>
    </tr>
    <tr>
        <td>password</td>
        <td>Specifies the password for authenticating with the source.</td>
    </tr>
    <tr>
        <td>storepasswordincleartext</td>
        <td>Indicates to store the password in unencrypted text instead of
        the default behavior of storing an encrypted form.</td>
    </tr>
    <tr>
        <td>username</td>
        <td>Specifies the user name for authenticating with the source.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the output: <em>normal</em>, <em>quiet</em>, <em>detailed (2.5+)</em>.</td>
    </tr>
</table>

<div class="block-callout-info">
    <strong>Note</strong><br>
    Make sure to add the sources' password under the same user context as the NuGet.exe is later used to access the package source. The password will be stored encrypted in the config file and can only be decrypted in the same user context as it was encrypted. So for example when you use a build server to restore NuGet packages the password must be encrypted with the same windows user that the build server task will run under.
</div>

#### Examples

    nuget sources Add "MyServer" \\myserver\packages

    nuget sources Disable "MyServer"

    nuget source Enable "nuget.org"


## spec

Generates a `.nuspec` file for a new package. If run in the same folder as a project file (`.csproj`, `.vbproj`, `.fsproj`), `spec` creates a tokenized `.nuspec` file. For additional information, see [Creating a Package](/ndocs/create-packages/creating-a-package).

#### Usage

    nuget spec [<packageID>] [options]

where &lt;packageID&gt; is an optional package identifier to save in the `.nuspec` file.

#### Options

<table>
    <tr>
        <td>assemblypath</td>
        <td>Specifies the path to the assembly to use for metadata.</td>
    </tr>
    <tr>
        <td>configfile</td>
        <td><em>(2.5+)</em> The NuGet configuration file to modify. If not specified,
        <em>%AppData%\NuGet\NuGet.config</em> is used.</td>
    </tr>
    <tr>
        <td>force</td>
        <td>Overwrites any existing .nuspec file.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the command.</td>
    </tr>
    <tr>
        <td>noninteractive</td>
        <td>Suppresses prompts for user input or confirmations.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the output: <em>normal</em>, <em>quiet</em>, <em>detailed (2.5+)</em>.</td>
    </tr>
</table>

#### Examples

    nuget spec

    nuget spec MyPackage

    nuget spec -a MyAssembly.dll


## update

Updates all packages in a project the latest available versions. It is recommended to run ['restore'](#restore) before running the `update`.

Note: `update` does not with projects using `project.json`.

The `update` command will also update assembly references in the project file, provided those references already exist. If an updated package has an added assembly, a new reference will *not* be added. New package dependencies will also not have their assembly references added. To include these operations as part of an update, update the package in Visual Studio using the Package Manager UI or the Package Manager Console.

This command can also be used to update nuget.exe itself using the *-self* flag.

#### Usage

    nuget update <configPath> [options]

where &lt;configPath&gt; identifies either a `packages.config` or solution file that lists the project's dependencies.

#### Options

<table>
    <tr>
        <td>configfile</td>
        <td><em>(2.5+)</em> The NuGet configuration file to modify. If not specified,
        <em>%AppData%\NuGet\NuGet.config</em> is used.</td>
    </tr>
    <tr>
        <td>fileconflictaction</td>
        <td><em>(2.5+)</em> Specifies the action to take when asked to overwrite or ignore existing files referenced by the project. Values are <em>overwrite, ignore, none</em>.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the command.</td>
    </tr>
    <tr>
        <td>id</td>
        <td>Specifies a list of package IDs to update.</td>
    </tr>
    <tr>
        <td>msbuildversion</td>
        <td>Specifies the version of MSBuild to be used with this command. Supported values are 4, 12, 14, 15. By default the MSBuild in your path is picked, otherwise it defaults to the highest installed version of MSBuild.</td>
    </tr>
    <tr>
        <td>noninteractive</td>
        <td>Suppresses prompts for user input or confirmations.</td>
    </tr>
    <tr>
        <td>prerelease</td>
        <td>Allows updating to prerelease versions. This flag is not required when updating prerelease packages that are already installed.</td>
    </tr>
    <tr>
        <td>repositorypath</td>
        <td>Specifies the local folder where packages are installed.</td>
    </tr>
    <tr>
        <td>safe</td>
        <td>Specifies that only updates with the highest version available within the same major and minor version as the installed package will be installed.</td>
    </tr>
    <tr>
        <td>self</td>
        <td><em>(1.4+)</em> Updates nuget.exe to the latest version; all other arguments are ignored.</td>
    </tr>
    <tr>
        <td>source</td>
        <td>Specifies a list of package sources to use for the updates.</td>
    </tr>
    <tr>
        <td>verbose</td>
        <td>Shows verbose output while updating.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the output: <em>normal</em>, <em>quiet</em>, <em>detailed (2.5+)</em>.</td>
    </tr>
    <tr>
        <td>version</td>
        <td>When used with one package ID, specifies the version of the package to update.</td>
    </tr>
</table>

#### Examples

    nuget update

    # update packages installed in solution.sln, using MSBuild version 14.0 to load the solution and its project(s).
    nuget update solution.sln -MSBuildVersion 14

    nuget update -safe

    nuget update -self

## Environment variables

The behavior of nuget.exe can be configured through a number of environment variables, which affect nuget.exe on machine, user, or process levels.

In general, options specified directly on the command line or in the NuGet configuration files have precedence, but there are a few exceptions such as *FORCE_NUGET_EXE_INTERACTIVE*. If you find that nuget.exe behaves differently between machines, an environment variable could be the cause. For example, Azure Web Apps Kudu (used during deployment) has *NUGET_XMLDOC_MODE* set to *skip* to speed up package restore performance and save disk space.

<table>
  <tr>
    <th>Variable</th>
    <th>Description</th>
    <th>Remarks</th>
  </tr>
  <tr>
    <td>http_proxy</td>
    <td>Http proxy used for NuGet HTTP operations.</td>
    <td>This would be specified as <em>http://&lt;username&gt;:&lt;password&gt;@proxy.com</em> .</td>
  </tr>
  <tr>
    <td>no_proxy</td>
    <td>Configures domains to bypass from using proxy.</td>
    <td>Specified as domains separated by comma (,).</td>
  </tr>
  <tr>
    <td>EnableNuGetPackageRestore</td>
    <td>Flag for if NuGet should implicitly grant consent if that's required by package on restore.</td>
    <td>Specified flag is specified as <em>true</em> or <em>1</em>, any other value treated as flag not set.</td>
  </tr>
  <tr>
    <td>NUGET_EXE_NO_PROMPT</td>
    <td>Prevents nuget.exe for prompting for credentials.</td>
    <td>Any value except null or empty string will be treated as this flag set to true</td>
  </tr>
  <tr>
    <td>FORCE_NUGET_EXE_INTERACTIVE</td>
    <td>Global environment variable to force interactive mode.</td>
    <td>Any value except null or empty string will be treated as this flag set to true</td>
  </tr>
  <tr>
    <td>NUGET_PACKAGES</td>
    <td>Path to where packages are stored / cached.</td>
    <td>Specified as absolute path.</td>
  </tr>
  <tr>
    <td>NUGET_FALLBACK_PACKAGES</td>
    <td>Global fallback packages folders.</td>
    <td>Absolute folder paths separated by semicolon (;).</td>
  </tr>
  <tr>
    <td>NUGET_HTTP_CACHE_PATH</td>
    <td>HTTP cache folder.</td>
    <td>Specified as absolute path.</td>
  </tr>
  <tr>
    <td>NUGET_PERSIST_DG</td>
    <td>Flag indicating if dg files (data collected from MSBuild) should be persisted.</td>
    <td>Specified as <em>true</em> or <em>false</em> (default), if NUGET_PERSIST_DG_PATH not set will be stored to temporary directory (NuGetScratch folder in current environment temp directory).</td>
  </tr>
  <tr>
    <td>NUGET_PERSIST_DG_PATH</td>
    <td>Path to persist dg files.</td>
    <td>Specified as absolute path, this option is only used when <em>NUGET_PERSIST_DG</em> is set to true.</td>
  </tr>
  <tr>
    <td>NUGET_RESTORE_MSBUILD_ARGS</td>
    <td>Sets additional MSBuild arguments.</td>
    <td></td>
  </tr>
  <tr>
    <td>NUGET_RESTORE_MSBUILD_VERBOSITY</td>
    <td>Sets the MSBuild log verbosity.</td>
    <td>Default is <em>quiet</em> ("/v:q"). Possible values <em>q[uiet]</em>, <em>m[inimal]</em>, <em>n[ormal]</em>, <em>d[etailed]</em>, and <em>diag[nostic]</em>.</td>
  </tr>
  <tr>
    <td>NUGET_SHOW_STACK</td>
    <td>Determines whether the full exception (including stack trace) should be displayed to the user.</td>
    <td>Specified as <em>true</em> or <em>false</em> (default).</td>
  </tr>
  <tr>
    <td>NUGET_XMLDOC_MODE</td>
    <td>Determines how assemblies XML documentation file extraction should be handled.</td>
    <td>Supported modes are <em>skip</em> (do not extract XML documentation files), <em>compress</em> (store XML doc files as a zip archive) or <em>none</em> (default, treat XML doc files as regular files).</td>
  </tr>
</table>
