# VisCADNext — Releases

Windows installers and the stable update feed for VisCADNext, an Excel-hosted engineering tool. Application source is maintained in a separate private repository.

> VisCADNext is an independent, unofficial third-party tool developed in a personal capacity. It is not an official Siemens product and is not affiliated with, sponsored, endorsed or supported by Siemens AG or any other vendor or manufacturer. To the extent permitted by applicable law, the software is provided "as is", without warranty of any kind, express or implied. No ongoing support, updates or maintenance are promised. Use of the tool is at the user's own risk. Users should maintain appropriate project backups and independently review and validate all outputs and changes before applying them.

## Download and install

The current stable release is [VisCADNext 1.1.12](https://github.com/timpendlebury/VisCad.Next-Releases/releases/tag/v1.1.12).

This release removes installation-note defaults from the new Siemens room sensors and restores their saved PointConfig selections, including equivalent QPA2360 choices and current-output signals. PointConfig still asks you to choose when saved values match different sensor configurations.

1. Download the [signed Windows x64 installer](https://github.com/timpendlebury/VisCad.Next-Releases/releases/latest/download/VisCADNextDesktop-stable-Setup.exe) and review the [certificate guidance](CODE_SIGNING.md).
2. Save your work, back up project files, and close Excel, Visio and VisCADNext before running Setup.
3. For 64-bit Excel, open the installed **VisCADNext** shortcut, select **Register / repair Excel add-in**, then reopen Excel. For 32-bit Excel, use the manual registration instructions below.

Downloads and update checks do not require a GitHub account.

<details>
<summary>Manual registration, beta upgrades and uninstalling</summary>

- **Manual registration:** In Excel, open **File → Options → Add-ins → Manage: Excel Add-ins → Go → Browse**. From `%LocalAppData%\VisCADNextDesktop\current\AddIn`, select `VisCad.Next.x64.xll` for 64-bit Excel or `VisCad.Next.x86.xll` for 32-bit Excel. Managed PCs may require IT approval.
- **Moving from beta:** Clear the old VisCADNext entry in Excel's Add-ins dialog, close Excel, then use **Register / repair Excel add-in**. Legacy data is not imported automatically.
- **Uninstalling:** Close Excel, select **Unregister this add-in** in the manager and follow any manual Excel instructions it displays, then uninstall through Windows **Installed apps**. User data is preserved.

</details>

## Prerequisites

- Windows x64.
- Microsoft Excel: **64-bit is preferred and the default; 32-bit is also supported**.
- [.NET 10 Desktop Runtime](https://dotnet.microsoft.com/en-us/download/dotnet/10.0) matching Excel's bitness: **x64** for 64-bit Excel or **x86** for 32-bit Excel.
- Microsoft Visio for drawing output.
- Siemens ABT products for features that use them.

Both 64-bit and 32-bit Excel add-ins are included in the release build. The manager registers the 64-bit add-in by default; 32-bit Excel uses manual registration.

These products are installed separately. The VisCADNext desktop manager includes its own .NET runtime.

## Updates

Open **VisCADNext → Updates** in Excel or use the desktop manager. Update checks run automatically; downloads and installation require your action.

**Certificate trust is required for semi-automatic updates through VisCADNext.** Update checks and downloads can run without it, but verification fails and **Restart and Update** remains unavailable. Running Setup through a Windows warning does not establish trust. Follow the [certificate verification and trust instructions](CODE_SIGNING.md) for the Windows account running VisCADNext, then retry **Download update**.

Choose **Download update**, then **Restart and Update** in the manager. Save your work and close Excel, Visio and VisCADNext normally. Busy files or active operations postpone the update; applications are never force-closed. Do not reopen applications while the upgrade is being applied. **Later** defers installation, and the Setup installer is also available.

<details>
<summary>Release validation</summary>

The release owner confirmed the sensor fixes using the unsigned local 1.1.12 test installer. The public signed candidate has equivalent application and catalogue inputs; its signing and package verification are separate from those local tests.

Normal installation, update and Excel/Visio use were previously reported as passed for signed 1.1.11 on a Windows PC with 64-bit Excel. The installer and update implementation is unchanged in 1.1.12. Those earlier passes are carried forward; a separate owner test of the exact signed 1.1.12 installer, update and Office flows has not been reported. Some failure and edge cases remain unqualified, and separate 32-bit Excel qualification remains pending. See the [release notes](https://github.com/timpendlebury/VisCad.Next-Releases/releases/tag/v1.1.12) for this release's validation scope.

</details>

## Repository contents

This repository holds public distribution documentation, Windows installers and update feeds. It does not contain application source, customer project files, user settings or publication credentials. The `.nupkg` and `releases.stable.json` assets support application updates.

<details>
<summary>Project files, templates and catalogue</summary>

- **User data:** Templates and workbooks are stored in `%LocalAppData%\VisCADNext` and preserved during upgrades and uninstall. Back up this folder before upgrading.
- **Template:** Startup creates `Workbook\VisCad Project Template.xlsm` only when missing, preserving existing edits. The old `C:\VisCadNext` beta folder and `VISCAD_*` data-path overrides are ignored.
- **Visio stencils:** Release-owned `VisCad.vssx` and `VisCad_User.vssx` are refreshed during upgrades. Keep custom stencils under other `.vssx` filenames in the user-data `Visio` folder.
- **Catalogue:** Excel reads the release catalogue without editing it; upgrades replace it. CatalogueEditor is excluded from official installers. Send catalogue corrections to the release owner.

</details>

## Signed installer verification

VisCADNext uses a self-signed code-signing identity. Read the [verification and optional trust instructions](CODE_SIGNING.md), download the [public certificate](T-Pendlebury-code-signing.cer), and check its [SHA-256 fingerprint](CERTIFICATE-SHA256SUMS). In-app updates require Windows to trust this publisher certificate.
