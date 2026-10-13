# BorrowBox · Share more. Buy less.

Campus resource-sharing platform for students. This is the **Phase 1 frontend prototype**
(HTML, CSS, JavaScript, sample data in the browser, no backend yet).

The look is carried over from the Campus Cart prototype: same paper and ink palette, lime accent,
Manrope headings with an italic serif emphasis word, same `@kristujayanti.com` sign-up rule and
the same admin-removes-listings idea.

## Run it

1. Open the folder in VS Code.
2. Right-click `index.html` and choose **Open with Live Server** (recommended, because the pages
   share data through localStorage). Opening `index.html` directly in Chrome also works.
3. On the landing page click **Try the demo account**, or create an account with any
   `name@kristujayanti.com` address.

Admin: open `admin.html` (also linked in the footer).
Email `admin@gmail.com`, password `admin@123`.

Fonts load from Google Fonts, so the first load needs internet.

## The 10 screens

| # | Screen | File / route |
|---|--------|--------------|
| 1 | Landing / Login | `index.html` |
| 2 | Student Dashboard | `app.html#/dashboard` |
| 3 | Browse Items (search and filters) | `app.html#/browse` |
| 4 | Item Details | `app.html#/item/ID` |
| 5 | List an Item | `app.html#/list` |
| 6 | Borrow Request (borrower form) and owner inbox | `#/request/ID`, `#/requests` |
| 7 | My Borrowings | `app.html#/borrowings` |
| 8 | My Items | `app.html#/my-items` |
| 9 | Borrowing History | `app.html#/history` |
| 10 | Admin Dashboard | `admin.html` |
| + | Profile (edit name and programme, change password) | `app.html#/profile`, from the avatar menu |

## Files

| File | Job |
|------|-----|
| `data.js` | Sample data, localStorage helpers and every action (request, accept, return, report). Written so each function can later become an API call. |
| `art.js` | Item illustrations as inline SVG, so no image files are needed. |
| `auth.js` | Landing page: log in, register, demo account. |
| `app.js` | Student app: hash router and all student screens. |
| `admin.js` | Admin login and the five admin tabs. |
| `styles.css`, `app.css` | Shared styles and landing page, then app and admin screens. |

## Request life cycle

`Pending` → owner accepts → `Accepted` (item is *Reserved*) → handover confirmed → `Borrowed`
(item is *Currently borrowed*) → `Returned`. An owner can also `Declined` a request.
A `Borrowed` request past its due date is shown as `Overdue`.

## Data model (matches the planned MySQL tables)

- `users` → `bb_users`: name, email, password, role, joined
- `items` → `bb_items`: owner, name, category, description, condition, photo, location, availability, available from/until
- `borrow_requests` → `bb_requests`: item, borrower, owner, request date, borrow from/until, status
- `borrowing_history` → `bb_history`: item, borrower, owner, borrow date, due date, return date, status

## What is mock in Phase 1

- Passwords are stored as plain text in localStorage and the admin login is checked in the browser.
  In Phase 2 use bcrypt and server-side sessions or JWT.
- Dates in the sample data are relative to today, so the screens always look current.
  **Reset sample data** (avatar menu) rebuilds them.
- Uploaded photos are shrunk and stored in the browser, so they only appear on that device.

## Phase 2 hand-off (Member 2)

1. Create the four MySQL tables above.
2. Build Express routes for the functions at the bottom of `data.js`
   (`createItem`, `createRequest`, `respondToRequest`, `confirmCollected`, `markReturned`, `removeItem`, `reportItem`).
3. Replace the `load`/`save` helpers with `fetch()` calls and move the admin check to the server.
4. Verify the college email before an account becomes active.
