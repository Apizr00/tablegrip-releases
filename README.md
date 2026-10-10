<h1 align="center">TableGrip</h1>

<p align="center">A fast, minimalist database client for Windows: SQLite, MySQL, MariaDB, PostgreSQL and SQL Server.</p>

TableGrip is a native Windows application (WPF, .NET 10) for Windows 10 and 11, on x64 and ARM64. It lets you browse,
query and edit a database in one small window, and shows every change as SQL before it is written.

This repository only carries the **downloads and release notes**. The source code is private.

## Download
Get the latest installer from the [Releases page](../../releases/latest):

| You have | Download |
|---|---|
| Windows on a normal PC (Intel / AMD) | `TableGrip-win-x64-Setup.exe` |
| Windows on ARM (Snapdragon, Surface Pro X ...) | `TableGrip-win-arm64-Setup.exe` |
| No installer wanted | the `...-Portable.zip` of your architecture (it cannot update itself) |

No .NET installation is needed. The builds are **not code-signed yet**, so Windows SmartScreen warns on first run
(More info > Run anyway).

## Updates
Installed copies can update themselves: **Help > Check for updates**. Nothing is contacted until you ask, or turn on
*Look for a new version when TableGrip starts* in Preferences.

## Documentation and help
- [User guide](docs/user-guide.md)
- [Changelog](CHANGELOG.md)
- [Report a problem or request a feature](../../issues)
