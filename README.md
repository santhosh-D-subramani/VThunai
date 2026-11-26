# VThunai

I created this app to check my daily store while I am at the office.

VThunai is a free, open-source, cross-platform companion application built with Flutter for Android and iOS. It brings daily store tracking, loadout and preset management, match analytics, rank goal tracking, party and XMPP social features, and game reference data into a single mobile client.

## Features

### Authentication and Accounts
- **Riot Games Sign-In**: In-app WebView authentication flow (`RiotLoginScreen` and `RiotAuthService`) for acquiring Riot access, entitlement, and PAS tokens.
- **Multi-Account Switching**: Switch between saved Riot accounts via `AccountSwitcherBottomSheet`.

### Store, Bundles, and Wishlist
- **Daily Shop**: View daily store offers, pricing, and reset timers (`DailyShopScreen`), with home screen widget synchronization (`WidgetSyncService` via `home_widget`).
- **Shop History and Wishlist**: Track past daily shop rotations (`ShopHistoryScreen`) and monitor desired skins (`SkinWishlistScreen`).
- **Bundles and Media**: Browse featured and historical weapon bundles (`AllBundlesScreen`, `BundleDetailScreen`) and inspect skin chromas, levels, and preview videos (`SkinDetailsScreen`, `VideoPlayerScreen`).

### Loadout and Presets
- **Inventory Customization**: Manage equipped weapon skins, buddies, sprays, and player cards (`LoadoutMainScreen`, `WeaponsListScreen`, `WeaponCustomizeScreen`, `ItemSelectionScreen`, `AccessoryInspector`).
- **Loadout Presets**: Create, edit, preview, and share full loadout configurations (`LoadoutPresetsScreen`, `PresetEditorScreen`, `PresetCollectionScreen`).
- **Auto-Loadouts**: Configure rule-based automatic loadout switching (`AutoLoadoutsScreen`, `AutoLoadoutRule`).

### Profile, Stats, and Progression
- **Career and Match History**: Inspect player career summaries, match history, and round-by-round match breakdowns (`ProfileScreen`, `CareerStatsScreen`, `MatchDetailScreen`, `MatchRoundDetailScreen`).
- **Rank and XP Tracking**: Track rank progression, estimate rank goals, and forecast XP (`RankGoalScreen`, `RankHistoryService`, `XpForecastService`).
- **Battlepass, Contracts, and Collection**: Monitor battlepass tiers (`BattlepassScreen`), agent gear and contracts (`AgentGearScreen`, `ContractsScreen`), and estimated collection value (`CollectionValueScreen`).
- **Leaderboards and Player Lookup**: View regional leaderboards (`LeaderboardScreen`), search players (`PlayerSearchScreen`), and manage bookmarked profiles (`BookmarkedPlayersScreen`).

### Party, Social, and Game Info
- **Party and Friends**: View and manage party state (`PartyScreen`) and friend lists (`FriendsScreen`).
- **Riot XMPP Chat**: Real-time presence and messaging over Riot XMPP (`RiotXmppService`, `SocialChatScreen`).
- **Game Info Hub**: Reference screens for agents, maps, and weapons (`HubScreen`, `GameInfoHubScreen`, `AgentsScreen`, `MapsScreen`, `WeaponsScreen`, `WeaponDetailScreen`).

### Appearance, Localization, and Utilities
- **Material 3 Theming**: Dynamic color support (`dynamic_color`), light/dark/system modes, pure black dark mode, and configurable seed colors (`AppearanceScreen`, `ThemeNotifier`).
- **Localization**: Supports English (`en`) and Tamil (`ta`) via `flutter_localizations` and ARB files in `lib/l10n/`.
- **Configuration and Diagnostics**: In-app HenrikDev API key management (`KeyManagerScreen`), remote configuration (`RemoteConfigService`, `RemoteConfigScreen`), cache management (`CacheManagerScreen`), quick actions (`QuickActionsService`), and diagnostic logs (`LogsScreen`).

## Tech Stack

- **Framework**: Flutter (Dart SDK `^3.13.7`)
- **State Management**: `flutter_riverpod` (`^3.2.1`)
- **Navigation**: `zenrouter` (`^1.1.0`)
- **Networking and Media**: `http`, `webview_flutter`, `video_player`, `cached_network_image`, `flutter_cache_manager`
- **UI and Charts**: `dynamic_color`, `google_fonts`, `fl_chart`, `flutter_markdown`
- **Platform Integration**: `shared_preferences`, `home_widget`, `quick_actions`, `share_plus`, `url_launcher`, `package_info_plus`, `path_provider`

## Project Structure

```text
.
├── .github/workflows/
│   └── build_ios.yml          # GitHub Actions workflow for unsigned iOS IPA builds
├── android/                   # Android native project
├── assets/                    # Static assets
├── config/
│   └── remote_config.json     # Default remote configuration payload
├── ios/                       # iOS native project and Runner configuration
├── lib/
│   ├── common/                # Shared screens and loading views
│   ├── l10n/                  # Localization ARB files and generated delegates (en, ta)
│   ├── models/                # Data models (matches, presets, rules, notifications, XMPP)
│   ├── providers/             # Riverpod providers for app state and data fetching
│   ├── router/                # ZenRouter configuration and tab/route definitions
│   ├── screens/               # Feature-grouped UI screens and widgets
│   ├── services/              # Riot Auth, Valorant API, XMPP, local, cache, and sync services
│   ├── theme/                 # Material 3 theme builder, config model, and notifier
│   ├── utils/                 # Calculators, formatters, asset resolvers, and sharing helpers
│   ├── widgets/               # Reusable UI components
│   ├── constants.dart         # Application constants
│   ├── main.dart              # Application entry point and initialization
│   └── vthunai.dart           # Root widget, theming, routing, and home widget listener
├── l10n.yaml                  # Localization generation config
└── pubspec.yaml               # Dependencies and Flutter configuration
```

## Getting Started

### Prerequisites
- Flutter SDK compatible with Dart `^3.13.7`
- Android Studio / Android SDK (for Android builds)
- Xcode and CocoaPods on macOS (for iOS builds)

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/santhosh-D-subramani/VThunai.git
   cd VThunai
   ```

2. Install dependencies and generate localization files:
   ```bash
   flutter pub get
   flutter gen-l10n
   ```

3. Run the application:
   ```bash
   flutter run
   ```

### Optional Configuration

- **HenrikDev API Key**: Some endpoints in `ValorantApi` use the HenrikDev API. You can supply a key at build/run time:
  ```bash
  flutter run --dart-define=HENRIK_API_KEY=your_api_key_here
  ```
  Alternatively, you can set and test a custom API key directly inside the app under **Settings > Key Manager**.

- **Remote Config**: On startup, `RemoteConfigService` fetches configuration from `config/remote_config.json` on the `main` branch. This URL can be customized or bypassed in **Settings > Remote Config**.

## CI/CD

The repository includes a GitHub Actions workflow at `.github/workflows/build_ios.yml` that builds an unsigned iOS `.ipa` (`VThunai.ipa`) on `macos-26` using Xcode 26.4.1 and Flutter `3.47.8` on pushes to `main` or manual `workflow_dispatch`.

## Credential Handling and Privacy

- Authentication is performed directly through the official Riot Games sign-in page inside an in-app WebView.
- Account tokens and session data are stored locally on your device via `SharedPreferences` and are never transmitted to any third-party developer server.
- Network requests are sent only to official Riot endpoints, community game-data/stats endpoints (such as `valorant-api.com` and `api.henrikdev.xyz`), and the configured remote config URL.

## Use at Your Own Risk

VThunai is an unofficial, fan-made tool that interacts with undocumented Riot client, storefront, and XMPP endpoints. Functionality may break at any time due to Riot API or authentication changes, and usage of third-party clients is at your own discretion and risk.

## License and Copyright

Copyright (c) 2026 Santhosh Subramani.

This project is free and open-source software licensed under the **GNU General Public License v3.0 (GPL-3.0)**. See the `LICENSE` file for full license terms.

## Legal Disclaimer

VThunai was created under Riot Games' "Legal Jibber Jabber" policy using assets owned by Riot Games. VThunai is not endorsed by, sponsored by, or officially affiliated with Riot Games, Inc., and does not reflect the views or opinions of Riot Games or anyone officially involved in producing or managing Riot Games properties. Riot Games, VALORANT, and all associated logos, characters, names, and assets are trademarks or registered trademarks of Riot Games, Inc.
