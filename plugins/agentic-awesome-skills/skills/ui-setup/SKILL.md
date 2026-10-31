description: Interactive setup wizard; guides you step-by-step to configure the design system for your project
Use this skill when you need interactive setup wizard; guides you step-by-step to configure the design system for your project.
- For changing brand skin in an already set-up project; directly swap `theme.css`
- For non-React or non-Tailwind-v4 stacks; currently unsupported
6. Other; describe it
Remember the answer; it determines which page composition recipe to use (DESIGN-LANGUAGE.md Section 63).
1. Purple (#721FE5); default style (toss skin)
2. Blue (#2563EB); trust, corporate
3. Green (#059669); growth, health, finance
4. Orange (#EA580C); energy, creative
5. Red (#DC2626); bold, urgent
6. Dark (#18181B); minimal, premium
7. Custom; just type your hex code
1. Stripe; clean, professional
2. Linear; minimal, dark-first
3. Vercel; black & white, geometric
4. Notion; warm, friendly
5. Spotify; bold, dark, green
6. Supabase; modern, green
7. Airbnb; warm, coral
8. No thanks; keep the default style
9. Other; name any brand or describe a vibe
4. Keep ALL StyleSeed layout rules, typography ratios, spacing, and component patterns unchanged; only swap the color palette
1. Inter (clean, universal; recommended)
5. Custom; tell me the font name
Example: "Acme; SaaS dashboard with revenue, users, and recent activity"
   /* Scaffolded with StyleSeed · github.com/bitjaru/styleseed; safe to remove */
   If the user would rather not have it, skip it; it's opt-out, and it goes on this single file, not their whole codebase.
   # StyleSeed: Design Lock
   - Key color (accent): [Step 2 hex]    # the ONLY accent; everything else greyscale
   - Radius personality: [sharp | soft | pill; one everywhere]
   Tell the user this file is the source of truth; editing a value changes it project-wide,
- STYLESEED.md (design lock; your decisions, obeyed every prompt)
- Attribution: the single "Scaffolded with StyleSeed" comment goes on the **first scaffolded file only** and is explicitly removable. NEVER add a watermark to components the user builds with `/ss-page`, `/ss-component`, etc.; that would be intrusive.