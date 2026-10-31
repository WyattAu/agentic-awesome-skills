# Intake: asking the right questions once
2. **Mine the message first.** Anything stated or clearly implied is not asked again. Language, currency, city, named services, named stack, named platform, named users; all count as answered.
4. **Only architecture-, persona- or scope-changing questions.** If the answer would only change a detail you can put in the Assumptions table, don't ask; assume and tag `[ASSUMED]`.
> کاربرهای اصلی داخل ایران هستن؟ (default: بله; RTL، تقویم جلالی، پرداخت داخلی، ورود با شماره موبایل)
- One sentence: what outcome does the user get that they can't get today? (default: your own reading from the message; state it and ask them to correct it)
- Primary users in one line each; you propose 2–4 based on research; ask them to confirm/rename/remove. (default: your proposal)
- Preferred or forbidden languages/frameworks/databases/clouds. (default: none; the stack is chosen by the agentic-engineering criteria)
- Expected scale in year one (users/day, records, media volume). (default: small; single region, single DB; note the scale ceiling in Non-goals)
- One epic per session is the working model; are sessions short (≈1–2 h) or long (half-day)? This sets epic size. (default: half-day → epics of 3–7 stories)
- Deadline or milestone (closed beta, public launch, demo)? (default: none; order by dependency and value)
- Each skipped question becomes an `[ASSUMED; <one-clause reason>]` row.
چند سوال کوتاه قبل از ریسرچ عمیق; هر کدوم رو خواستی رد کن، «پیش‌فرض» یعنی همه‌ی پیش‌فرض‌ها قبول.
7. درگاه پرداخت مشخصی دارید؟ (پیش‌فرض: یکی از درگاه‌های دارای مجوز شاپرک; در ریسرچ بررسی می‌شه)
8. زبان/فریم‌ورک/دیتابیس ترجیحی یا ممنوع؟ (پیش‌فرض: هیچ; انتخاب با معیارهای Agentic Engineering)