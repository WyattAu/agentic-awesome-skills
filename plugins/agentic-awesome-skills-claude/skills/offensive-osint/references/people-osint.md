- [TruePeopleSearch](https://www.truepeoplesearch.com/); free U.S. people search.
- [Clearbit](https://clearbit.com/); company/individual data enrichment.
- [FaceCheck](https://facecheck.id/) / [FaceSeek](https://faceseek.online/); reverse face search.
- [TrueCaller](https://www.truecaller.com/); caller ID + spam blocking.
- [ThatsThem](https://thatsthem.com/); reverse phone search.
- [Infobel](https://infobel.com/); non-USA phone search.
- [FreeCarrierLookup](https://freecarrierlookup.com/); carrier/type (US).
- [NumlookupAPI](https://numlookupapi.com/) [Freemium]; programmatic carrier checks.
| Instagram | [Picuki](https://www.picuki.com/); profile view without account |
| X/Twitter | [snscrape](https://github.com/snscrape/snscrape); historical scraper references; verify current support and platform access rules, without bypassing login or rate controls |
| Facebook (research) | [Meta Content Library](https://transparency.meta.com/researcher); CrowdTangle successor (researcher-gated) |
| YouTube/Twitch | [Social Blade](https://socialblade.com/); analytics |
| TikTok | [Tokboard](https://tokboard.com/); trends + profile analytics |
| Reddit | [Reveddit](https://www.reveddit.com/); removed content; [RedTrack.social](https://redtrack.social/); user history |
| Bluesky | [Firesky](https://firesky.tv/); real-time firehose; [SkyView](https://bsky.jazco.dev/); follower graphs |
| Mastodon | [FediSearch](https://fedisearch.skorpil.cz/); cross-instance search; [Fedifinder](https://fedifinder.glitch.me/); find Twitter users on Mastodon |
- [OpenCorporates](https://opencorporates.com/); world's largest open company DB.
- [SEC EDGAR](https://www.sec.gov/edgar.shtml); U.S. company filings.
- [OpenOwnership Register](https://register.openownership.org/); beneficial ownership.
- [MuckRock](https://www.muckrock.com/); FOIA repository + request tracking.
- [EU Tenders (TED)](https://ted.europa.eu/); EU procurement notices.
- [World Bank Projects](https://projects.worldbank.org/); project + procurement records.
- [UK Companies House](https://find-and-update.company-information.service.gov.uk/); UK companies + officers + filings.
- **GSXT**; [gsxt.gov.cn](https://www.gsxt.gov.cn/) National Enterprise Credit Info; cross-check with Tianyancha / Qichacha.
- **USCC (Unified Social Credit Code)**; 18-character entity ID assigned to all CN legal entities. Do not parse it with the old region-first layout. Use the issuing authority’s published standard and checksum rules; preserve the complete identifier for joins. Useful for joining GSXT records to ICP filings.
- **ICP Beian**; [beian.miit.gov.cn](https://beian.miit.gov.cn/); filing applicability depends on hosting, service and jurisdiction; verify current official requirements before making a compliance claim. Where a public filing exists, the filing links the domain to a USCC, which links to the legal entity in GSXT.
- [OpenSanctions](https://www.opensanctions.org/); aggregated.
- [OCCRP Aleph](https://aleph.occrp.org/); investigative documents, leaks, company records.