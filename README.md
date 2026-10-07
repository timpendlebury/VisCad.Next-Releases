# VisCADNext

VisCADNext provides Excel-hosted engineering tools and a desktop installation manager, developed by T. Pendlebury. This repository contains public distribution documentation and approved release assets. Application source is maintained privately.

> VisCADNext is independently developed and maintained by T. Pendlebury in a personal capacity. It is not affiliated with, endorsed by, or supported by Siemens, Microsoft, or other vendors named in the application. Existing licence terms apply. The software is supplied without warranty; support and continued maintenance are not guaranteed. Users are responsible for backups, checking generated engineering information, and validating results before use.

Install the [latest approved Setup](https://github.com/timpendlebury/VisCad.Next-Releases/releases/latest/download/VisCADNextDesktop-stable-Setup.exe) after reviewing [certificate guidance](CODE_SIGNING.md). An initial installer may not be published yet. You need no GitHub account or token.

Requires Windows x64, 64-bit Microsoft Excel, and the x64 .NET 10 Desktop Runtime for the Excel add-in. The desktop manager and catalogue editor include their own .NET runtime. Install the Desktop Runtime from [Microsoft](https://dotnet.microsoft.com/en-us/download/dotnet/10.0); the manager reports whether it is available. Microsoft Visio and Siemens ABT products remain external prerequisites for features that use them.

Save your work and close Excel, Visio, the catalogue editor and VisCADNext normally before installation or upgrade. Open the installed **VisCADNext** shortcut and use **Register / repair Excel add-in**. The manager checks Excel architecture, runtime and running sessions before updating only this add-in's registration. Registration is an explicit user action. If automatic registration is unavailable, use Excel **File → Options → Add-ins → Manage: Excel Add-ins → Go → Browse**, then select `%LocalAppData%\VisCADNextDesktop\current\AddIn\VisCad.Next.x64.xll`. Enterprise policy may require your IT team to permit the add-in.

The Excel ribbon includes **About** and **Updates**. **Check for updates**, **Download update**, cancellation, **Later**, release notes and **Open Setup installer page** are available. Offline failures do not block Excel work. Automatic **Restart and Update** is unavailable until safe update application is qualified; use the Setup fallback after closing applications normally. Do not start Excel or VisCADNext while Setup is applying an upgrade.

User data stays outside the installation: an existing `C:\VisCadNext` layout is honoured; new users use `%LocalAppData%\VisCADNext`. Upgrades and uninstall must preserve catalogues, projects, workbooks and preferences. Back up user data before upgrading. The legacy VBA project template is not included; import an existing workbook through the established VisCADNext workflow.

When moving from a beta, the manager detects a VisCADNext XLL registered at another location and blocks registration. In Excel, open **File → Options → Add-ins → Manage: Excel Add-ins → Go**, clear the old VisCADNext entry, then close Excel normally and use **Register / repair Excel add-in** again. The installer does not remove or replace another add-in registration automatically.
