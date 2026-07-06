# Bank Account System

A Windows console application written in **C** simulating a simple bank account system — account creation, login, balance checking, and money transfers — using binary files as storage instead of a database. Built as a **Dev-C++** project targeting Windows (`windows.h`, `getch()`, MinGW).

## Features

- Create a new account (name, address, date of birth, Aadhar number, phone number, account type, username/password)
- Login by username, with password entry masked as `*` on screen
- View full account details after login
- Send money to another username
- Check balance — sums all incoming transfers recorded for the logged-in user
- Logout with a simple animated "logging out" console effect

## Tech Stack

C · Windows API (`windows.h` for cursor positioning) · Dev-C++ / MinGW · flat binary files for persistence (no database)

## Data Storage

Two binary files, written next to the executable, store all application state:

| File | Contents |
|------|----------|
| `username.txt` | Appended `accountDetails` structs — one per created account |
| `mon.txt` | Appended `money` structs — one per money transfer, recording sender, receiver, and amount |

There's no delete/update — both files only ever grow via `fwrite` in append (`"ab"`) mode, and are scanned fully with `fread` on every lookup.

## Build & Run

This project was built with **Dev-C++ 5.11** on Windows and depends on `windows.h`/`conio.h`-style console functions (`getch`, `system("cls")`), so it's Windows-only as written.

**Using Dev-C++:**
1. Open `Bank Account System.dev` in Dev-C++.
2. Build and run (F9 / F11) — `Makefile.win` is generated from the same project.

**Using MinGW from the command line (Windows):**

```
gcc main.c -o "Bank Account System.exe"
"Bank Account System.exe"
```

On first run, choose option `1` to create an account; on later runs, choose `2` to log in with the username/password you set.

## Known Limitations

This is a learning project, so a few things are worth knowing about before relying on it for anything beyond a console-app exercise:

- **Passwords aren't actually saved or checked.** `CreateBankAccount()` reads a password into a local array but never writes it into the `accountDetails` struct or the file, and `login()` reads a password but never compares it against anything — login succeeds purely by matching the username.
- Money transfers in `SendMoney()` aren't validated against the sender's actual balance — you can send more money than you have, and the sender's own balance isn't decremented anywhere.
- No input-length validation on `scanf("%s", ...)` calls into fixed-size buffers (e.g. `username[50]`), which is a classic buffer-overflow risk in C.
- All data is stored unencrypted in plain binary files in the working directory.
- The UI relies on absolute console cursor positioning (`displayCoordinates`), so it only looks right in a standard-sized Windows console window.

## License

Licensed under **CC0 1.0 Universal** (public domain dedication) — see [LICENSE](LICENSE).
