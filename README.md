# VisCADNext

VisCADNext provides Excel-hosted engineering tools and a desktop installation manager. This repository contains public distribution documentation and approved release assets. The source repository is private. Download the Setup EXE from the latest stable release below.

> VisCADNext is an independent, unofficial third-party tool developed in a personal capacity. It is not an official Siemens product and is not affiliated with, sponsored, endorsed or supported by Siemens AG or any other vendor or manufacturer. To the extent permitted by applicable law, the software is provided "as is", without warranty of any kind, express or implied. No ongoing support, updates or maintenance are promised. Use of the tool is at the user's own risk. Users should maintain appropriate project backups and independently review and validate all outputs and changes before applying them.

Install the [latest approved Setup](https://github.com/timpendlebury/VisCad.Next-Releases/releases/latest/download/VisCADNextDesktop-stable-Setup.exe) after reviewing [certificate guidance](CODE_SIGNING.md). An initial installer may not be published yet. You need no GitHub account or token.

Requires Windows x64, 64-bit Microsoft Excel, and the x64 .NET 10 Desktop Runtime for the Excel add-in. The desktop manager and catalogue editor include their own .NET runtime. Install the Desktop Runtime from [Microsoft](https://dotnet.microsoft.com/en-us/download/dotnet/10.0); the manager reports whether it is available. Microsoft Visio and Siemens ABT products remain external prerequisites for features that use them.

Save your work and close Excel, Visio, the catalogue editor and VisCADNext normally before installation or upgrade. Open the installed **VisCADNext** shortcut and use **Register / repair Excel add-in**. The manager checks Excel architecture, runtime and running sessions before updating only this add-in's registration. Registration is an explicit user action. If automatic registration is unavailable, use Excel **File → Options → Add-ins → Manage: Excel Add-ins → Go → Browse**, then select `%LocalAppData%\VisCADNextDesktop\current\AddIn\VisCad.Next.x64.xll`. Enterprise policy may require your IT team to permit the add-in.

The Excel ribbon includes **About** and **Updates**. **Check for updates**, **Download update**, cancellation, **Later**, release notes and **Open Setup installer page** are available. Offline failures do not block Excel work. Automatic **Restart and Update** is unavailable until safe update application is qualified; use the Setup fallback after closing applications normally. Do not start Excel or VisCADNext while Setup is applying an upgrade.

User data stays outside the installation: an existing `C:\VisCadNext` layout is honoured; new users use `%LocalAppData%\VisCADNext`. Upgrades and uninstall must preserve catalogues, projects, workbooks and preferences. Back up user data before upgrading. The legacy VBA project template is not included; import an existing workbook through the established VisCADNext workflow.

When moving from a beta, the manager detects a VisCADNext XLL registered at another location and blocks registration. In Excel, open **File → Options → Add-ins → Manage: Excel Add-ins → Go**, clear the old VisCADNext entry, then close Excel normally and use **Register / repair Excel add-in** again. The installer does not remove or replace another add-in registration automatically.

Before uninstalling, close Excel normally and use **Unregister this add-in** in the VisCADNext manager, following any manual Excel instructions it displays. Then uninstall VisCADNext through Windows **Installed apps**. Your catalogue, templates, workbooks and other user data remain outside the installation directory.
