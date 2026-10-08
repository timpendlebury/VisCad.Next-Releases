# Verify and optionally trust T. Pendlebury's signed installer

VisCADNext uses a self-signed code-signing certificate. The
publisher identity is asserted by its owner, not verified by a public
certificate authority. You decide whether to trust it. Corporate PCs may
require IT approval; trust for one account does not apply to every account or
policy context. SmartScreen may still warn, and Smart App Control or enterprise
policy may block the application. Do not disable protections.

Download the installer and **T-Pendlebury-code-signing.cer**. The .cer contains
only the public certificate. You must never receive or import a PFX, private
key or signing password to use this application. The certificate/fingerprint
are separate from Velopack's installer, update package and feed.

Independently confirm this fingerprint through a channel you already associate
with T. Pendlebury. A certificate and hash from the same untrusted download
alone do not establish identity.

| Public identity | Value |
| --- | --- |
| Subject | `CN=T. Pendlebury` |
| DER certificate SHA-256 | `237BFA480A1489E23BAF28FF8A989F219DF6229FA59FA451D707C6D728919B13` |
| Windows certificate thumbprint | `A2BD4071D5153B7C0E8849A95F47937810C6BA37` |
| Expiry, UTC | `2029-10-05 01:31:14` |

After you have confirmed the fingerprint and chosen to trust the publisher,
open PowerShell in the folder containing the .cer and installer. This block
checks the public identity/profile, explicitly imports only that public
certificate into the current user's Root and TrustedPublisher stores, and
verifies the actual installer. Windows may show a trust confirmation; accept
only if you have chosen to trust this exact certificate.

```powershell
$ErrorActionPreference = 'Stop'
$ExpectedSha256 = '237BFA480A1489E23BAF28FF8A989F219DF6229FA59FA451D707C6D728919B13'
$ExpectedThumbprint = 'A2BD4071D5153B7C0E8849A95F47937810C6BA37'
$CerPath = (Resolve-Path -LiteralPath './T-Pendlebury-code-signing.cer').Path
$InstallerPath = (Resolve-Path -LiteralPath './VisCADNextDesktop-stable-Setup.exe').Path
if ((Get-FileHash -LiteralPath $CerPath -Algorithm SHA256).Hash -cne $ExpectedSha256) { throw 'Certificate fingerprint mismatch. Stop.' }
$Cert = [Security.Cryptography.X509Certificates.X509Certificate2]::new($CerPath)
try {
    $Constraints = @($Cert.Extensions | Where-Object { $_.Oid.Value -eq '2.5.29.19' })
    $Usage = @($Cert.Extensions | Where-Object { $_.Oid.Value -eq '2.5.29.15' })
    $Eku = @($Cert.Extensions | Where-Object { $_.Oid.Value -eq '2.5.29.37' })
    if ($Cert.HasPrivateKey -or $Cert.Subject -cne 'CN=T. Pendlebury' -or $Cert.Thumbprint -cne $ExpectedThumbprint -or
        $Constraints.Count -ne 1 -or $Constraints[0].CertificateAuthority -or
        $Usage.Count -ne 1 -or $Usage[0].KeyUsages -ne [Security.Cryptography.X509Certificates.X509KeyUsageFlags]::DigitalSignature -or
        $Eku.Count -ne 1 -or $Eku[0].EnhancedKeyUsages.Count -ne 1 -or $Eku[0].EnhancedKeyUsages[0].Value -cne '1.3.6.1.5.5.7.3.3') { throw 'Certificate identity/profile mismatch.' }
} finally { $Cert.Dispose() }
$null = Import-Certificate -FilePath $CerPath -CertStoreLocation 'Cert:/CurrentUser/Root'
$null = Import-Certificate -FilePath $CerPath -CertStoreLocation 'Cert:/CurrentUser/TrustedPublisher'
$Signature = Get-AuthenticodeSignature -LiteralPath $InstallerPath
if ($Signature.Status -ne 'Valid' -or $null -eq $Signature.SignerCertificate -or $null -eq $Signature.TimeStamperCertificate) { throw 'Installer signature is not valid and timestamped.' }
$HashAlgorithm = [Security.Cryptography.SHA256]::Create()
try { $SignerHash = [BitConverter]::ToString($HashAlgorithm.ComputeHash($Signature.SignerCertificate.RawData)).Replace('-', '') }
finally { $HashAlgorithm.Dispose() }
if ($SignerHash -cne $ExpectedSha256 -or $Signature.SignerCertificate.Subject -cne 'CN=T. Pendlebury') { throw 'Installer signed by another certificate.' }
$Signature | Format-List Status, StatusMessage
$Signature.SignerCertificate | Format-List Subject, Thumbprint, NotAfter
```

This is deliberate trust in a directly self-signed, non-CA, code-signing-only
certificate for this Authenticode EXE distribution. It does not install a
general-purpose CA or assert that Microsoft or Siemens reviewed the software.
If you decline this trust choice, do not import the certificate.

In-app updates also require Windows to trust this publisher certificate for the
account running VisCADNext. Allowing Setup to run through a Windows warning
does not install certificate trust. If an update reaches 100% and reports an
Authenticode verification failure, file transfer has finished but verification
has failed, so **Restart and Update** stays unavailable. For an untrusted-root
error, verify the exact fingerprint above and follow the trust steps under the
affected Windows account if you choose to trust this publisher, then retry
**Download update**. Other signature or integrity errors require investigation.
Managed PCs may require IT to establish publisher trust.

Finish your VisCADNext and Office work and close the application before running a newer
Setup installer. Version checks and the release download link are under
the Excel **VisCADNext → Updates** ribbon control and the desktop manager.

To revoke your current-user trust later, this independent block removes only
the exact public certificate from those two stores:

```powershell
$ErrorActionPreference = 'Stop'
$Thumbprint = 'A2BD4071D5153B7C0E8849A95F47937810C6BA37'
foreach ($Store in @('Root', 'TrustedPublisher')) {
    $Path = 'Cert:/CurrentUser/' + $Store + '/' + $Thumbprint
    if (Test-Path -LiteralPath $Path) { Remove-Item -LiteralPath $Path }
}
```

Removing trust affects all software signed with that identity, including
ABT AI Assistant. For a managed deployment, ask IT to remove any trust
installed by organizational policy.

[Microsoft Authenticode trust guidance](https://learn.microsoft.com/en-us/windows/win32/dxtecharts/authenticode-signing-for-game-developers),
[SmartScreen](https://learn.microsoft.com/en-us/windows/apps/package-and-deploy/smartscreen-reputation),
[Smart App Control](https://learn.microsoft.com/en-us/windows/apps/develop/smart-app-control/overview).
