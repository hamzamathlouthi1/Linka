# Linka

Tunisia's influencer–business marketplace — a Flutter app connecting local
creators with brands to discover, launch, and manage collaborations.

Built responsive-first: one codebase adapts between a compact mobile layout
and a full web/desktop experience (landing page, side nav rail, multi-column
grids).

## Tech stack

- Flutter 3.3+ / Dart 3.3+
- Material 3 (`useMaterial3: true`)
- [`google_fonts`](https://pub.dev/packages/google_fonts) — Inter + Poppins
- No backend yet — screens read from in-memory mock data in `lib/models/models.dart`, designed to be swapped for real API calls without touching UI code

## Getting started

```bash
flutter pub get
flutter run            # mobile/emulator
flutter run -d chrome   # web
```

No API keys or environment setup required — everything runs on mock data out of the box.

## Project structure

```
lib/
├── main.dart                 # App entry point, applies AppTheme
├── theme/
│   ├── app_colors.dart       # Brand palette + AppGradients (red/white, kept subtle)
│   └── app_theme.dart        # ThemeData: typography, buttons, inputs, nav, page transitions
├── utils/
│   ├── responsive.dart       # Breakpoints, ResponsiveLayout, MaxWidthBox
│   └── page_transitions.dart # Shared fade + slide-up route transition
├── models/
│   └── models.dart           # Influencer / Campaign models + mock data
├── widgets/
│   ├── linka_button.dart      # Brand button (gradient fill, hover, press-scale)
│   ├── linka_card.dart        # Base card shell (hover lift, press-scale)
│   ├── linka_logo.dart        # Wordmark, light/dark variants
│   ├── linka_text_field.dart  # Styled form field w/ password visibility toggle
│   ├── stat_bar.dart          # Metric row + status pill, animated counters
│   ├── marketplace_cards.dart # InfluencerCard / CampaignCard
│   ├── animated_counter.dart  # Count-up number animation for stats
│   ├── fade_slide_in.dart     # Staggered fade + slide-up entrance wrapper
│   └── shimmer_box.dart       # Skeleton loading placeholder (no extra package)
└── screens/
    ├── splash_screen.dart      # Mobile welcome screen / Web landing page
    ├── auth_screen.dart        # Login / signup, Creator vs Brand role toggle
    ├── app_shell.dart          # Post-login shell: bottom nav (mobile) / rail (web)
    ├── marketplace_screen.dart # Discover creators (brand) / open campaigns (creator)
    ├── dashboard_screen.dart   # Campaign tracking, filterable table/list
    └── profile_screen.dart     # Own profile or a creator's public profile
```

## Design notes

- **Roles**: the app renders different content depending on `UserRole.brand` vs
  `UserRole.creator` — set at signup in `auth_screen.dart` and threaded through
  `AppShell`.
- **Responsive breakpoints** are centralized in `lib/utils/responsive.dart`
  (`Breakpoints.mobile/tablet/desktop`) — update them there rather than
  hardcoding widths in screens.
- **Motion**: page pushes use a shared fade + upward-slide transition
  (`LinkaPageTransitionsBuilder`, wired into `AppTheme`), tab switches inside
  `AppShell` cross-fade, and lists/grids reveal with a staggered
  `FadeSlideIn` cascade rather than popping in all at once.
- **Loading state**: `MarketplaceScreen` briefly shows shimmer skeleton cards
  before "real" results fade in — this is simulated with a fixed delay for
  now; swap it for your actual fetch state when wiring up an API.
- **Brand palette**: strictly red (`AppColors.red`) and white, with
  `AppGradients` providing subtle depth (hero backgrounds, filled buttons)
  rather than flat fills. Keep new UI within this palette rather than
  introducing new accent colors.

## Swapping in a real backend

Everything currently reads from `mockInfluencers` / `mockCampaigns` in
`lib/models/models.dart`. Screens only depend on the `Influencer` and
`Campaign` classes, not on how the data arrives — replace the mock lists with
API/repository calls and the UI layer shouldn't need to change.

## Known limitations

- No persistence — auth is a mock flow, nothing is stored between runs.
- No real state management library (Provider/Riverpod/Bloc) is wired in yet;
  state is local `setState` per screen.
- `flutter analyze` / a full build hasn't been run against this exact copy —
  run it locally before shipping.
