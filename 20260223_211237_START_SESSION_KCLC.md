KCLC-IMPL-20260223-008-CLDE-zoom-renumber-and-provision

Resume Zoom phone migration. Extension renumbering plan is ready; 3 questions need answers before executing. Also investigate sipXcom SIP attack prevention (discovery only).

Checkpoint: CHECKPOINT_20260223_2112_zoom-ext-renumber-plan.md
Model: sonnet — Zoom API calls + renumbering execution

Priorities:
1. Answer 3 open questions (see checkpoint) then execute renumbering
2. sipXcom SIP attack investigation (discovery only, no changes)

Critical facts:
- Cutover Feb 28, 2026 — HARD DEADLINE (5 days)
- 14 physical Polycom VVX devices — all still unregistered in Zoom
- Tanya Thompson (ext 703) is TFH, NOT KCLC — ext 703 is wrong for her
- Proposed renumbering: sipXcom 2XX → Zoom 7XX (straight digit swap)
- Fire Station (716) + Police Station (717) exist in Zoom with DIDs but NO sipXcom counterpart — MACs unknown
- 4 rooms need Zoom common area phones created: Bus Station (700), KCLC Admin (705), The Market (709), The Paint Shop (710)
- 10 existing Zoom rooms need extensions renumbered (705→702, 706→703, etc.)
- +13522680205 still PENDING port, still unassigned
- sipXcom server SSH details NOT in ~/.ssh/config — need IP/auth to connect
