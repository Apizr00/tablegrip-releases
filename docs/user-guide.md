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

**Production lock.** On a connection tagged *production*, a destructive statement (`DROP`, `TRUNCATE`, `UPDATE` or
`DELETE` without a `WHERE`), committing deleted rows, and restoring a backup each ask you to **type the connection's
name** before they go on, instead of just clicking OK. Turn it off in Preferences > Safety. Read-only connections still
refuse writes.

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
  to the whole table, and to export. The operators are listed in groups: `=` `<>` `<` `>` `<=` `>=`; `IN` / `NOT IN` (values
  separated by commas or lines; put quotes around a value that contains a comma); `IS NULL` / `IS NOT NULL`; `BETWEEN` /
  `NOT BETWEEN` (two boxes, both ends included); `LIKE` (your own pattern: `%` any text, `_` one character); and the plain
  text searches *Contains*, *Not contains*, *Has prefix* and *Has suffix*, which treat `%` and `_` as ordinary characters.
- **Edit** a cell by double-clicking or pressing F2. Changes are *staged* (coloured) until you commit. **Preview** shows
  the exact SQL; **Commit** writes it in one transaction, and nothing is saved if one statement fails.
  Tables without a primary key are read-only.
- **Copy** the selected cells with Ctrl+C (tab-separated, full values). Right-click > **Copy as** adds a header line
  (**Ctrl+Shift+C**), CSV, JSON, or SQL INSERT statements for the selected rows and columns.
- **Follow relationships.** A foreign key cell that holds a value ends in an arrow (**→**): click it to open the table it
  points at, showing just that row (a column that is in several keys asks which). Right-click a cell > **Follow
  relationship** also lists *Rows that point at this row*: every table with a foreign key to this one, opened showing the
  rows that reference the row you are on. The arrows need only the table's own keys; the second list reads the keys of
  the whole database the first time (up to 500 tables) and keeps them for a few minutes.
- **Right-click a cell** for: *Edit value*, *Set value* (NULL, empty string), *Reload rows*, *Paste* (the clipboard
  text as one value into the cell; *Paste to selected cells* into every selected cell; staged like typing, so it can be
  undone), *New row*, *Duplicate row*, *Copy* and *Copy as*, *Copy all values of this column*, *Quick filter* (equals,
  does not equal, contains, is NULL, is not NULL, or clear), *Follow relationship*, *Import CSV*, *Export rows* and
  *Delete row*. The menu acts on the cell under the pointer.
- **Structure** shows columns, keys, indexes and the CREATE statement.
- **Analyze columns** (Structure side) counts, for every column, the share of NULLs, the distinct values and the smallest
  and largest value. It only reads; a table of more than two million rows is analyzed on its first 200,000. The Structure
  side also shows how big the table and its indexes are (MySQL, MariaDB, PostgreSQL, SQL Server and SQLite with its dbstat
  table), and the cell value of each key can be followed with the arrow described above.
- **Find usages.** Right-click a table in the sidebar (or a column on the Structure side, or press Ctrl+K and search
  *Find usages*) to list the foreign keys that point at it or leave it, and the views, routines and triggers whose stored
  text mentions it (a whole-word name search, so it says "mentions"). Double-click a row to open the table, or to read the
  routine in a query tab. Do this before dropping or renaming something.

## SQL editor
| Action | Keys |
|---|---|
| Run the statement at the caret, or the selection | Ctrl+Enter or F5 |
| Run every statement | Ctrl+Shift+Enter |
| Stop | Esc |
| Suggest names | Ctrl+Space (also automatic after two letters or a dot) |

**Parameters.** Write `:name` in a query (`WHERE customer_id = :id`) and TableGrip asks for the value each time it runs;
the values are sent as parameters, never pasted into the text. Leave a box empty or type NULL for NULL; numbers and
true / false are sent as such, anything else as text (put quotes around a number to send it as text, or write
`CAST(:d AS date)` for a date). Colons inside strings, comments and `::` casts are left alone. History keeps the query
with its `:name` placeholders, so running it again asks again.

**Snippets.** Type a trigger word and press **Tab** to expand a snippet, or pick it from the suggestions (marked
*snip*). In its text, `${1:name}` is a place to type with a default (Tab moves to the next one; the same number repeats
what you typed) and `$0` is where the caret finishes. A few starters ship with TableGrip (`sel`, `selc`, `ins`, `upd`,
`del`, `join`, `grp`, `dup`); **Snippets...** in the workspace menu adds your own, and *Save the selection as a snippet*
(Ctrl+K) turns the SQL you selected into one. Yours are kept in `snippets.json`.

**History.** Every run is added to **History** in the sidebar; type in the search box to find one. Right-click >
**Pin to top** keeps a query at the top of the list, and pinned queries survive *Clear history*. **Save query** keeps
one under **Queries**.

## Plans, diagrams and comparing data
- **Explain (query plan).** In a query tab, **Explain** (Ctrl+L) shows how the engine would run the statement at the caret
  or the selection, step by step: the operation, what it works on, estimated rows and cost, and each step's share of the
  work. Steps that deserve a look are marked: a red edge for the likely hot spot (a full scan of a big table, a step with
  half the cost, a sort or hash that spilled to disk) and an amber one for remarks (a whole index read, a temporary sort,
  an estimate ten times off). Nothing is executed. On PostgreSQL, **Analyze** runs a *read* and reports real row counts and
  times; it is not offered for a statement that changes data. MySQL and MariaDB show the tabular plan, SQLite its query
  plan and SQL Server the estimated showplan (with the missing indexes it suggests). *Copy plan as text* puts the plan in the
  clipboard.
- **ER diagram.** **ER diagram...** in the workspace menu (or *Show in ER diagram...* on a table) draws the tables of the
  database with their columns and the foreign keys between them, related tables side by side and tables without relations
  below, in crow's-foot notation (a foot at the table that holds the key, bars at the table it points at, a circle where the
  key may be NULL); primary key columns come first. The window is resizable, **Full screen** (F11) fills the screen, and **Fit**
  zooms so the whole picture is visible. Choose a table and *within N relations* to see only its neighbourhood, show all columns, only key columns or just
  names, zoom with Ctrl+wheel, click a table to highlight its relations, double-click to open it, and **Export PNG** or
  **Export SVG**. It reads the keys of up to 500 tables.
- **Compare data.** **Compare data...** in the workspace menu compares two tables by key, each from this connection or
  another saved one (so staging against production, or a table against its backup). The key columns default to the
  primary key. It lists the rows that exist only in the source, only in the target, and the rows whose values differ
  (column by column), then writes the script that would make the target match the source: INSERTs, UPDATEs of the changed
  columns and, if you tick it, DELETEs. Values are compared by meaning (1 equals 1.0, true equals 1). **Nothing is changed**:
  review the script, copy or save it, or open it in a query tab and run it there, where safe mode, the production lock and
  the change log apply. The target is read into memory, so it is limited to 500,000 rows.

## Change log and connection health
Both are in the workspace menu (the button beside the sidebar) and in the command palette (Ctrl+K).
- **Change log** lists what was committed on this connection (tick *All connections* for every one): table edits you
  committed, statements run from a query tab that change data or structure, and CSV imports, each with the time and the
  SQL as it ran, values included. Search it, copy the SQL of an entry, or clear it. The file (`audit.json` in your data
  folder) stays on this computer; passwords typed into a statement are masked.
- **Connection health** times a few do-nothing statements and shows the typical round trip with a rating, plus the server
  version, TLS and (MySQL, MariaDB, PostgreSQL, SQL Server) uptime. A slow round trip means the network, VPN or tunnel is
  slow; if it is fast, a slow query is slow inside the server. It changes nothing.

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

## Updates
Updates are checked when TableGrip starts (you can turn that off in Preferences).
- **When a new version exists** TableGrip opens a window like other desktop apps: *A new version of TableGrip is
  available!* with the release notes. **Install Update** downloads it (with a progress bar) and then **Install and
  Restart** installs it; your connections and settings are kept, and it asks first if you have unsaved changes.
  **Remind Me Later** closes the window, and the start screen keeps an orange notice with **What's new** and **Update**.
  **Skip This Version** hides that version until a newer one appears.
- **Several versions behind.** The notice and the window say how many updates you are behind (for example *Version 1.0.0
  is available (8 updates behind)*), and the release notes list every version you skipped, newest first. The update
  itself goes straight to the newest version.
- **Help > Check for updates** (or *Check now* in Preferences) looks once, whenever you like, and offers a skipped version
  again. It works for copies installed with the installer, not for the portable zip.
- *Look for a new version when TableGrip starts* (on by default) does the same check on every start, also available as a
  checkbox in the update window. It contacts only github.com and sends nothing about you or your data.

## Appearance
**Preferences > Appearance** has the **Look**, the colour scheme (System, Light, Dark) and two more choices:
- **Look**: *Glass* (frosted panels over a colourful backdrop) or *Classic* (the original flat look: opaque panels in
  the original colours, no backdrop art). The colour scheme and the layout work with both.
- **Glass effects**: *Auto* uses translucent panels while Windows' *Transparency effects* are on (and never in High
  Contrast), *On* always, *Off* gives opaque panels with the same layout and colours. Panels show a blurred copy of
  the colourful backdrop behind them.
- **Layout**: *Floating* (separate rounded panels with gaps) or *Docked* (flush panels, no gaps). Both layouts work with either look.

The title bar's connection name is a command bar: click it, or press **Ctrl+K**, to jump to a table, a tab or an action.
A thin line in the environment's colour along the top of the workspace shows which environment you are in.

## Where TableGrip keeps its data
`%APPDATA%\TableGrip`: `connections.json`, `settings.json`, `history.json`, `favorites.json`, `known_hosts.json`,
`window.json` and `secrets.json` (encrypted). Delete the folder to start from scratch. Set the `TABLEGRIP_DATA_DIR`
environment variable to use another folder (useful for a second, separate profile).
