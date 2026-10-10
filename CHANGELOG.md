# Changelog

All notable changes to TableGrip. The format follows [Keep a Changelog](https://keepachangelog.com/), and versions
follow [Semantic Versioning](https://semver.org/).

## [1.0.2] - 2026-10-11

### Fixed
- **Release notes showed stray asterisks.** Text written between single stars in the update window's notes and in
  What's new is now shown in italics instead of with the stars around it.

## [1.0.1] - 2026-10-11

### Added
- **How far behind you are.** When several versions were skipped, the update notice, the update window and the status
  line say so (for example *Version 1.0.0 is available (8 updates behind)*), and *What's new* lists the notes of every
  version you skipped, newest first. The update still goes straight to the newest version.

### Removed
- **The update address box** in Preferences: TableGrip always checks the official releases on GitHub. *Check now* and the
  start-up check stay.
- **The Diagnostics setting** (saving a crash report file) and the feature behind it. Unexpected errors are shown in a
  dialog and written to the log as before.

## [1.0.0] - 2026-10-11

First stable release. The look is new (frosted glass, floating panels; the original look stays available as *Classic*),
and everything else from the 0.9 series is unchanged. The installers are not code-signed yet.

### Changed
- **A glassmorphism look.** Frosted, translucent panels float over a soft, colourful abstract backdrop (blue, violet,
  pink and teal shapes, a fine grain on top). Each panel shows a blurred copy of the backdrop behind it,
  tinted white in the light theme and deep navy in the dark theme, with a bright hairline edge. Primary actions and links
  use a blue accent. Text colours are checked against the glass over every backdrop colour.
- **Floating layout.** The navigator, the workspace, the row details and the status bar are separate rounded cards with
  gaps between them; tabs are pills; the connection name in the title bar is a command bar (click it, or Ctrl+K). A
  2 px line in the environment's colour runs along the top of the workspace, so production is unmistakable.
- **The start screen** is two glass cards (brand and actions, connections). Dialogs, the connection editor, the colour
  picker and the command palette are glass sheets over the same backdrop.
- **Choose your look.** Preferences > Appearance has a new **Look** choice: *Glass* (the new frosted look) or *Classic*
  (the original flat look: opaque dark or light panels in the original colours, no backdrop art). The colour scheme
  (System, Light, Dark) and the layout (floating or docked) work with both; Glass effects apply to the Glass look.
- **Preferences > Appearance** gains *Glass effects* (Auto follows Windows' "Transparency effects" and High Contrast; On;
  Off gives opaque panels with the same layout and colours) and *Layout* (Floating, or Docked with flush panels).
- **Refresh no longer draws a blue line** across the top of the workspace: the command bar already fills as it
  progresses. The line remains while a connection opens (the command bar is not shown yet) and inside a tab that is
  running a query or loading a table.
- `docs/roadmap.md`: the suggested features to add next, ranked.

### Fixed
- **Drop-down lists and menus were see-through** on the glass look, so their entries overlapped the text behind them
  (the Preferences font list, for example). Drop-downs, menus and the SQL completion list are opaque now.

## [0.9.11] - 2026-10-10

### Changed
- **Database logos instead of letters.** The database type picker, the start screen and the open-connection list now show
  each engine's logo (white, on a disc in the engine's colour): MySQL, MariaDB, SQLite, PostgreSQL, SQL Server, DuckDB,
  Cassandra, CockroachDB, Snowflake, Redshift, Redis, MongoDB, DynamoDB, Elasticsearch, ClickHouse, BigQuery, libSQL and
  D1. Engines without a logo (Vertica, Greenplum) keep their two letters. The logos come from Simple Icons (CC0) and are
  trademarks of their owners.

### Fixed
- **The colour picker's wheel and sliders did nothing when dragged.** The picker opened as a pop-up window that closed
  itself as soon as the mouse was pressed inside it. It is now a sheet inside the editor (click the dimmed area, press Esc
  or the tick to close), so the wheel and the sliders follow the mouse. Enter in its hex box applies the colour instead
  of saving the connection.

## [0.9.10] - 2026-10-10

### Added
- **A colour picker for connections.** Click the colour block next to *Status color* in the connection editor: a colour
  wheel (hue and saturation) inside a ring that shows the colour in force, a hex box, copy and done buttons, and
  saturation and brightness sliders. The star saves the colour to a list of your own (kept between sessions; click a saved
  colour to use it, right-click to remove it).
- **Your own environment tags.** The *Tag* drop-down keeps the usual list (None, local, development, testing, staging,
  production) and now ends with **Custom...**: type a name of your own (up to 24 characters, e.g. *QA* or *client-x*). Tags
  you create stay in the list for the next connection, and the **bin** button next to the drop-down deletes one (after
  asking; connections that used it simply have no tag). Custom tags show in the start screen, the connection rail and
  the title-bar pill like the built-in ones; only *production* turns confirmations on by itself.

### Changed
- **TableGrip is licensed under the MIT License.** The source code stays private; the program is free to use. `LICENSE`
  and `THIRD-PARTY-NOTICES.md` (the open-source components it is built on, with their licences) now ship in every build
  and are published with the downloads.
- **The new / edit connection window is redesigned.** Captions sit on the left with the fields to their right, in rounded
  sheets: *Name*, *Status color* (the colour in force, the swatches and the environment *Tag* drop-down), the server fields
  (*Host/IP*, *Port*, *User*, *Password* with its storage drop-down, *Database*, *SSL mode*, certificates), an **Over SSH**
  button that lights up while the tunnel is on and opens its own sheet (server, user, password, **SSH key**), then *Group*
  and *Safe mode*. **Save**, **Test** and **Connect** are at the bottom; the **x** or Esc closes it. Nothing was removed.

## [0.9.9] - 2026-10-10

### Added
- **Any colour for a connection.** Under *Colour* in the connection editor, next to the swatches, type red / green / blue
  (0-255) or a `#RRGGBB` value; the preview dot and the boxes follow whatever you pick.

### Fixed
- **Button text is centred.** Text in the dialog buttons (OK, Cancel, Save, ...) sat at the left edge; it is centred now.
  The start screen's icon buttons keep their left alignment.

## [0.9.8] - 2026-10-10

### Added
- **A proper update experience.** TableGrip looks for a new version when it starts and, if there is one, opens
  *A new version of TableGrip is available!* with the release notes. **Install Update** downloads it with a progress
  bar, then **Install and Restart** installs it. **Remind Me Later** keeps an orange notice on the start screen
  (with **What's new** and **Update**), and **Skip This Version** hides that version until a newer one appears.
- Help > Check for updates and Preferences > Check now open the same window.

### Changed
- The start-up update check is **on by default** (turn it off in Preferences or in the update window). It contacts only
  the release page and sends nothing about you or your data. If you had it off before, it is on again once.

### Fixed
- **All popups can be moved.** The connection editor, engine picker, backup / restore, password prompt and the command
  palette (drawn inside the window) can be dragged by their empty areas, like the dialogs; they return to the centre
  each time they open.

## [0.9.7] - 2026-10-10

### Fixed
- **Dialogs can be moved.** About, Preferences, Export, Import, Open connection, Confirm and Prompt are borderless windows
  and could not be dragged. Drag any empty part of a dialog to move it; buttons, inputs and lists behave as before.

## [0.9.6] - 2026-10-10

### Fixed
- **Check for updates and the Help links work for everyone.** 0.9.5 looked for updates in the source repository, which is
  private, so every check ended with "404 (Not Found)". Downloads, the update feed, the user guide, the changelog and the
  issue tracker now live in the public `Apizr00/tablegrip-releases` repository, and the app uses that address.
- The update prompt shows the release notes.

### Note
- Copies of 0.9.5 and older still look at the old address: install 0.9.6 once with the installer; updates work from then on.

## [0.9.5] - 2026-10-10

### Fixed
- **Check for updates works out of the box.** With no update address set it used to only say "No update address is set".
  It now looks at the project's GitHub releases when you ask (Help > Check for updates, or *Check now* in Preferences),
  asks before downloading, and restarts to install a small delta package. An address you enter still takes priority, and
  `github.com/owner/repo` addresses are understood. The start-up check stays opt-in and off by default.
- Releases now carry the update feed files (`releases.<rid>.json` and the `.nupkg` packages) next to the installers; see
  `docs/release.md`.

## [0.9.4] - 2026-10-10

### Added
- **Copy as** in the results grid's right-click menu: copy the selected cells with a header line (tab-separated, **Ctrl+Shift+C**),
  as CSV, as JSON, or as SQL INSERT statements in the connection's dialect. Rows keep the order on screen.

## [0.9.3] - 2026-10-10

### Changed
- **Several connections open at once in one window.** The connection button now opens the picked connection next to the
  current one instead of in a new window. A rail of connection tabs on the left (shown from two connections on) switches
  between them; each connection keeps its own sidebar, tabs and unsaved changes. The window's **X** closes the active
  connection, then the next, then the application.
- The "open another connection" button shows a database with a plus instead of a power symbol.

## [0.9.2] - 2026-10-10

### Added
- **Select several tables** in the sidebar (Ctrl+click, Shift+click, **Ctrl+A**) and export their data together: one file per
  table (CSV, JSON, Excel) or one SQL script, with a right-click menu (open in new tab, open structure, copy name, export).
- **Search in every table**: Ctrl+F focuses the search box of the open table or query result; query results get a
  search box that narrows the loaded rows and shows how many match.
- **Open another connection** from the connection button in the title bar: a searchable list (Open connection) that opens the
  chosen connection in a **new window**, so several connections can be used at the same time. **New...** starts the new-connection
  flow in a new window.

### Changed
- The window's **X** button closes the connection first and returns to the start screen; pressing it again closes the
  application. (Alt+F4 still closes the window at once.)
- Polished header and footer: an environment chip and styled engine / database text in the connection pill, a segmented
  Data | Structure switch, a connection indicator, and a grouped pager with the query time in the footer.
- The filter panel is one line per condition (column, operator, value, Apply, remove, add).

## [0.9.1] - 2026-10-10

### Added
- **Backup database** and **Restore databases** on the start screen: a whole database as one SQL script (structure and
  rows, foreign-key order) and a restore that runs a script as one transaction, with confirmation and safe-mode checks.
- **Export / Import connections** (JSON, never containing passwords) and a right-click menu on the connection list
  (New, Export Connections, Import Connections).
- Application menu (**⋮**) on the start screen: File, View, Tools, Help, and an **About TableGrip** window.

### Changed
- The start screen no longer shows the line of keyboard hints under the buttons (the shortcuts still work and are in the menu).

## [0.9.0] - 2026-10-10

First release candidate: every planned feature is in. What is left before 1.0 is outside the code: a real code-signing
certificate, a test on a clean machine, and a hosted update feed (see `docs/release.md`).

### Added
- **Databases:** SQLite, MySQL / MariaDB, PostgreSQL and SQL Server. One connection opens one database.
- **Start screen** with saved connections (groups, environment tags, colours), recent files, drag-and-drop of database files.
- **Table browser:** server-side sorting and filtering, paging, a Structure tab (columns, keys, indexes, DDL), a details panel for the selected row.
- **Editing:** inline edits staged and reviewed as SQL before they are written, in one transaction; undo and redo; insert, duplicate and delete rows.
- **SQL editor:** syntax highlighting, autocomplete that knows the tables and aliases in the statement, run statement / selection / script, cancel, query history, saved queries.
- **Export** to CSV, JSON, SQL and Excel; **import** from CSV with column mapping and validation before anything is written.
- **Command palette** (Ctrl+K) and **search everything** (Ctrl+P); light, dark and system themes; preferences.
- **SSH tunnels** (password or key, host keys remembered with trust-on-first-use), **TLS modes** (disabled to verify-identity),
  **safe mode** per connection (confirm changes, read-only), **Windows Integrated Authentication** for SQL Server.
- **Windows 10 / 11 integration:** per-monitor DPI awareness, remembered window position that survives a disconnected
  monitor, x64 and ARM64 builds, installer with automatic updates.
- **Opt-in diagnostics:** crash report files (no queries, no data) and update checks. Both are off by default.

### Changed
- New application icon and logo (installer, taskbar, start screen, dialogs).

### Security
- Passwords and SSH secrets are stored only in `secrets.json`, encrypted for the current Windows user (DPAPI).
- Query history masks `IDENTIFIED BY '...'`, `PASSWORD '...'` and similar literals.
- Error messages from drivers are scrubbed of `Password=` / `Pwd=` values before they are shown.

### Accessibility
- Every control has a name for screen readers; list entries are announced with their meaning (table, view, connection and environment).
- Toolbar buttons are reachable with Tab and show a focus ring; clicking them with the mouse does not steal the editor's focus.
- Text colours meet WCAG AA contrast (4.5:1) in both themes; a test keeps them that way.

### Fixed
- Closing the app showed an "unexpected problem" dialog (the service container was disposed synchronously).
- On a second monitor the window jumped to the primary monitor when a database was opened, and could start partly off screen.

### Known limitations
See the end of `docs/architecture.md`.
