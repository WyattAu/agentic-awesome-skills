# Naming and Functions: Clean Code Chapters 2 and 3
Do not differentiate names by adding noise words. `ProductInfo`, `ProductData`, `Product`; what's the difference? Same with `getActiveAccount` vs. `getActiveAccountInfo`. If the distinction is real, name the distinction.
If you can't read the name aloud in a code review, it's a bad name. `genymdhms` is a real-world example from the book; `generation_timestamp` is the fix.
`User`, `Invoice`, `Account`; classes are things. `saveInvoice`, `computeTotal`, `notifyUser`; methods are actions. A class named `ProcessInvoice` and a method named `Invoice` are both wrong.
Mixing levels is the most common subtle defect. Do not put an HTTP call, a SQL query, a regex parse, and a business rule in the same function; those are four levels.
Zero is best. One is fine. Two is OK. Three "should be avoided." Four or more "requires very special justification"; usually means you should pass a config object.
If two functions share a non-trivial block, extract it. But; see [dry-kiss-yagni.md](dry-kiss-yagni.md) for when this is wrong (Sandi Metz's "wrong abstraction" caveat).