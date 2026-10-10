# TableGrip user guide

## Connecting

1. **Create connection** (Ctrl+N), choose the database type, fill in host, port, user, password and **database name**.
   Each connection opens exactly one database.
2. **Test** checks the settings without saving. **Save** keeps it in the list; **Connect** saves and opens it.
3. SQLite files can also be opened directly (Ctrl+O) or dropped onto the window.

### Passwords
- *Store securely* encrypts the password for your Windows account. It is never written to `connections.json`.
- *Ask every time* asks when you connect and keeps it in memory only.
- *No password* is for servers that do not need one.

### Over SSH
Turn on **Over SSH** in the connection form, then enter the SSH server, user and either a password or a private key file.
The database host is the address *as seen from the SSH server*. The first time you connect, TableGrip shows the server's
fingerprint; compare it with the one your administrator gave you before you trust it. If it ever changes, TableGrip
refuses to connect and tells you why.

### SSL mode
| Mode | Meaning |
|---|---|
| Disabled | No encryption. |
| Preferred | Encrypt if the server offers it. |
| Required | Always encrypt; the certificate is not checked. |
| Verify CA | Also check that the certificate comes from the CA you give (or a trusted one). |
| Verify identity | Also check that the certificate matches the host name you typed. |

Through an SSH tunnel the host name is `127.0.0.1`, so use *Verify CA* there.

### Tag
The *Tag* drop-down marks what a connection is for: **None**, **local**, **development**, **testing**, **staging** or
**production**. Choose **Custom...** to type a name of your own (up to 24 characters); your tags stay in the list for
the next connection. While one of your own tags is chosen, the bin button next to the drop-down deletes it from the list
(after asking; connections that used it simply have no tag). The built-in tags cannot be deleted. The tag is shown next to the name on the start screen and in the title-bar pill. Only
**production** switches safe mode to *Confirm changes* by itself.

### Colour
Each connection has a colour dot on the start screen and in the connection rail. Under *Colour* pick one of the swatches
(the first one is the database type's default) or any colour of your own: type **R**, **G**, **B** (0-255) or a
`#RRGGBB` value, and press Tab or click away to apply it. Click the colour block next to *Status color* for the **colour
picker**: a wheel for hue and saturation, a brightness and a saturation slider, the hex value with copy and done buttons,
and a star that saves the colour to your own list (click a saved colour to use it, right-click to remove it).

### Safe mode
Per connection: **Off**, **Confirm changes** (every statement that writes or changes the schema asks first; destructive
ones are marked) and **Read-only** (writes are refused). Marking a connection *production* turns Off into Confirm.
Safe mode is a guard against mistakes, not a security boundary: for a hard guarantee use a database account without
write privileges.

## Backup and restore

On the start screen, **Backup database...** and **Restore databases...** work on any saved connection (or a recent
SQLite file), without opening it in the workspace. Inside a database, the same two actions are in the **⋮** menu.

- **Backup** writes one SQL script: the structure the database reports for each table and view, then the rows as
  multi-row INSERT statements. Tables come in foreign-key order and views last. Options: structure, data, and
  `DROP TABLE IF EXISTS` before each table so a restore replaces what is there.
- **Restore** runs a SQL script (a TableGrip backup, or any dump) on the chosen connection **as one transaction**, so on
  PostgreSQL, SQL Server and SQLite a failure leaves the database exactly as it was. MySQL and MariaDB commit every
  CREATE/DROP on their own, so a failed restore there can be partly applied; the message says so. You are asked to
  confirm first, with the number of statements and how many existing objects the script drops. A read-only connection
  refuses to restore, and a connection in *Confirm* mode shows the same question.
- The script is held in memory while it runs, so a file above 256 MB is refused. For a backup that large use the
  database's own tools (`pg_dump`, `mysqldump`, `sqlcmd`).
- Not included: users and privileges, stored procedures and functions, triggers the engine does not report with the
  table, and (PostgreSQL) sequence positions.

## Moving connections to another computer

Right-click empty space in the connection list, or use **File** in the **⋮** menu:

- **New** creates a connection or opens a database file.
- **Export Connections** writes all connections, or just the selected one, to a JSON file. **Passwords and SSH secrets are
  never included**; they are asked for again.
- **Import Connections** adds the connections of such a file. They get new identities and a free name ("Shop (2)"), so
  nothing is overwritten.

## The menu

The **⋮** button at the top right of the start screen opens a menu: **File** (new, open, import, export, backup,
restore, exit), **View** (theme), **Tools** (preferences) and **Help** (user guide, report a problem, releases, check for
updates, **About TableGrip**).

## Several connections at once

The **new connection button** (a database with a plus) at the top left of a connection opens the **Open connection** list. Pick one (search, group folding,
**New...**) and it opens **in the same window**; the connection you are in stays open.

Once two or more connections are open, a **rail of connection tabs** appears on the far left, one entry per connection
(coloured by its environment tag, captioned "Name : database"). Click an entry to switch. Each connection keeps its own
sidebar, tabs, selection and unsaved changes while you work in another one. Middle-click an entry, use its small **x**, or
right-click and choose **Close connection** to close just that one.

The window's **X** closes the active connection (the next one becomes active); with none left you are back on the start
screen, and one more press closes the application. If any open connection has unsaved changes you are asked first.

## Working with tables
- Click a table to preview it, double-click to keep the tab. Click a column header to sort (on the server).
- **Select several tables** with Ctrl+click or Shift+click, or all of them with **Ctrl+A**, then right-click for **Export**
  (CSV, JSON, SQL INSERT or Excel) or press Ctrl+Shift+E. The data goes to one file per table in a folder you choose, or to a
  single .sql file for all of them.
- **Search** every table: **Ctrl+F** puts the cursor in the search box of the open table (server side, all columns), or of a
  query result (the loaded rows, with a "12 of 250 rows" count). Ctrl+F in the sidebar filters the list of tables.
- **Filter** with conditions (column, operator, value; **+** adds a line, **-** removes it) or the quick search; filters apply
  to the whole table, and to export.
- **Edit** a cell by double-clicking or pressing F2. Changes are *staged* (coloured) until you commit. **Preview** shows
  the exact SQL; **Commit** writes it in one transaction, and nothing is saved if one statement fails.
  Tables without a primary key are read-only.
- **Copy** the selected cells with Ctrl+C (tab-separated, full values). Right-click > **Copy as** adds a header line
  (**Ctrl+Shift+C**), CSV, JSON, or SQL INSERT statements for the selected rows and columns.
- **Structure** shows columns, keys, indexes and the CREATE statement.

## SQL editor
| Action | Keys |
|---|---|
| Run the statement at the caret, or the selection | Ctrl+Enter or F5 |
| Run every statement | Ctrl+Shift+Enter |
| Stop | Esc |
| Suggest names | Ctrl+Space (also automatic after two letters or a dot) |

Every run is added to **History** in the sidebar. **Save query** keeps one under **Queries**.

## Export and import
- **Export** (Ctrl+Shift+E): CSV, JSON, SQL INSERTs or Excel. A table can be exported whole (current sort and filter) or
  just the loaded rows.
- **Import CSV** (Ctrl+Shift+I): the delimiter is detected, columns are matched by name and can be remapped, and the file
  is validated *before* anything is written. An error leaves the table untouched.

## Keyboard
| Keys | Action |
|---|---|
| Ctrl+K | Command palette |
| Ctrl+P | Search tables and saved queries |
| Ctrl+N / Ctrl+O | New connection / open file |
| Ctrl+T / Ctrl+W | New query tab / close tab |
| Ctrl+Tab, Ctrl+Shift+Tab | Next / previous tab |
| F5 | Refresh |
| Ctrl+F | Filter the sidebar (or search connections) |
| Ctrl+, | Preferences |
| Ctrl+Z / Ctrl+Y | Undo / redo staged edits |
| Ctrl+C / Ctrl+Shift+C | Copy selected cells / copy with a header line |
| Tab / Shift+Tab | Move between controls; every button is reachable |

## Updates and crash reports
Updates are checked when TableGrip starts (you can turn that off in Preferences); crash report files are **off by default**.
- **When a new version exists** TableGrip opens a window like other desktop apps: *A new version of TableGrip is
  available!* with the release notes. **Install Update** downloads it (with a progress bar) and then **Install and
  Restart** installs it; your connections and settings are kept, and it asks first if you have unsaved changes.
  **Remind Me Later** closes the window, and the start screen keeps an orange notice with **What's new** and **Update**.
  **Skip This Version** hides that version until a newer one appears.
- **Help > Check for updates** (or *Check now* in Preferences) looks once, whenever you like, and offers a skipped version
  again. It works for copies installed with the installer, not for the portable zip.
- *Look for a new version when TableGrip starts* (on by default) does the same check on every start, also available as a
  checkbox in the update window. It contacts only github.com, or the update address you enter instead (an https://
  address or a folder; a `github.com/owner/repo` address works too), and sends nothing about you or your data.
- *Save a crash report file* writes a text file to `%APPDATA%\TableGrip\crashes` when something goes wrong. It contains
  the exception type, a scrubbed message and the stack trace, and no queries, connection settings or data. Nothing is sent
  anywhere; attach the file to a bug report if you want to share it.

## Where TableGrip keeps its data
`%APPDATA%\TableGrip`: `connections.json`, `settings.json`, `history.json`, `favorites.json`, `known_hosts.json`,
`window.json` and `secrets.json` (encrypted). Delete the folder to start from scratch. Set the `TABLEGRIP_DATA_DIR`
environment variable to use another folder (useful for a second, separate profile).
