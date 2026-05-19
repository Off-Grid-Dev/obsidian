
**Central Obsidian file for planning, documentation, and Kanban tracking** _Last updated: March 21, 2026_ _App name:_ recipe-buddy _Goal:_ A mobile-first React Native app (iOS + Android) that helps users navigate recipes efficiently with a guided, step-by-step cooking experience. This is the **single source of truth** for the entire project. Use this file to build your Kanban board (in Obsidian or GitHub Projects), create tickets, track progress, and document decisions.

## Project Overview

Recipe Buddy turns your existing web app (built with QwikJS) into a native mobile experience. Users will:

- Land on a welcoming home screen
- Browse recipes in a clean grid
- View detailed recipe overviews
- Step through the recipe literally (progressive guided mode with timers, checkboxes, and next/previous controls)

**Key differentiator vs. web version:** Real-time step guidance, offline-first, native feel (swipes, haptics, notifications), and seamless mobile UX.

**Target users:** Home cooks who want to follow recipes hands-free while in the kitchen.

**Success metrics (post-MVP):**

- 100% offline recipe access
- <2s navigation between screens
- Guided step completion rate >80% in testing

## Current Tech Stack (verified March 2026)

- **Expo SDK**: 55 (latest stable – released early 2026)
    - React Native 0.76–0.77 range (Expo SDK 55 aligns with RN 0.76+ New Architecture default)
    - React 19.x
    - New Architecture (Fabric + TurboModules) enabled by default
- **Navigation**: **Expo Router** v4 (file-based routing – official recommendation)
    - Automatic deep linking, web fallback, platform folders
- **Language**: TypeScript
- **Styling**:
    - Custom theme object (theme.ts) with your existing colors/spacing
    - StyleSheet.create + possible future NativeWind / Tamagui / gluestack-ui
- **State Management** (recommended):
    - Local: React Context + useReducer
    - App-wide: **Zustand** (lightweight & popular in Expo 2026 ecosystem)
- **Data Storage** (MVP):
    - assets/recipes.json (static copy from web app)
    - Future: Expo SQLite or AsyncStorage for progress
- **Expo modules** (install as needed):
    - expo-router
    - expo-constants, expo-status-bar, expo-linking
    - expo-haptics (step feedback)
    - expo-keep-awake (cooking screen)
    - expo-notifications (timer alerts – phase 2)
- **Development**:
    - npx expo start --dev-client or npx expo start
    - EAS Build for production

**Recommended install commands** (run these in your project root):

npx expo install expo-router react-native-safe-area-context react-native-screens expo-linking expo-constants expo-status-bar npx expo install expo-haptics expo-keep-awake

## App Screens & Navigation Flow

Using Expo Router file-based routing:

app/ ├── index.tsx → Landing / Welcome ├── (tabs)/ │ ├── _layout.tsx → Bottom tabs layout │ ├── recipes/ │ │ ├── index.tsx → Recipes list (grid) │ │ └── [id].tsx → Recipe overview │ └── profile/ │ └── index.tsx → User profile (placeholder for now) ├── recipe/[id]/steps.tsx → Guided step-through screen └── _layout.tsx → Root layout

**Flow**:

1. **Landing / Welcome** (/) Hero + "Browse Recipes" → Recipes tab, "Profile" → Profile tab
2. **Recipes List** (/(tabs)/recipes) 2-column grid, title + prep/cook time + tags No images in MVP
3. **Recipe Overview** (/(tabs)/recipes/[id]) Ingredients, instructions summary, times, servings Big "Start Cooking" button → steps screen
4. **Step-Through Recipe** (/recipe/[id]/steps) Full-screen guided mode Current step large, timer if present, checkbox, notes field Swipe / buttons: Prev / Next / Finish Progress indicator (step X of Y)

## Data Model (MVP)

TypeScript

```
interface Recipe {
  id: string;
  title: string;
  description?: string;
  prepTime: number;       // minutes
  cookTime: number;
  servings: number;
  ingredients: Ingredient[];
  steps: Step[];
  tags?: string[];
}

interface Ingredient {
  name: string;
  amount: string;
  unit?: string;
}

interface Step {
  id: number;
  instruction: string;
  timer?: number;         // seconds
  optional?: boolean;
}
```

Initial data: assets/recipes.json

## Features – MVP Scope

**Phase 1 – Core**

- All 4 screens
- Expo Router navigation
- Theme with your colors/spacing
- Load recipes from JSON
- Basic step progression (next/prev + checkbox)

**Phase 2 – Guidance**

- Current step highlighting
- Timers (countdown + keep-awake)
- Progress persistence (Zustand + AsyncStorage)
- Haptic feedback on step change

**Phase 3 – Polish**

- Search bar + tag filter
- Dark mode support
- Favorites (local storage)
- Loading / error states

## Project TODO List (Kanban-ready)

### Phase 0: Setup & Migration

- Install Expo Router + core deps
- Configure app.json (name, slug, scheme: "recipebuddy")
- Create constants/theme.ts and migrate colors/spacing
- Set up TypeScript + folder structure
- Copy recipes.json from web project
- Verify npx expo start works on device/simulator

### Phase 1: Navigation & Screens

- Root _layout.tsx with tabs
- Landing screen (index.tsx)
- Recipes grid (/(tabs)/recipes/index.tsx)
- Recipe detail (/(tabs)/recipes/[id].tsx)
- Step-through screen (/recipe/[id]/steps.tsx)
- Navigation links & "Start Cooking" button

### Phase 2: Step Logic

- Zustand store: currentRecipe, currentStep, completedSteps
- Step checkboxes & completion
- Timer component (useInterval or setTimeout)
- Progress bar / step counter
- Save progress to AsyncStorage

### Phase 3: Polish & Extras

- Search + filter UI
- Dark mode (useColorScheme + theme toggle)
- Favorites heart icon
- Basic error boundaries / loading spinners

### Phase 4: Build & Test

- Test on physical device (iOS + Android)
- Add app icon & splash screen
- Run eas build --platform all (first test build)
- Document any blockers / decisions here

**Estimated MVP timeline:** 10–16 days (part-time)

## Folder Structure (2026 Expo best practice)

recipe-buddy/ ├── app/ │ ├── (tabs)/ │ ├── recipe/[id]/ │ ├── _layout.tsx │ └── index.tsx ├── src/ │ ├── components/ │ ├── hooks/ │ ├── store/ │ ├── types/ │ └── utils/ ├── constants/ │ └── theme.ts ├── assets/ │ └── recipes.json ├── app.json └── README.md

## Immediate Next Steps

1. Paste this entire content into a new file in Obsidian (e.g. Recipe-Buddy-Project.md)
2. Convert the TODO section into a Kanban board
3. Run the dependency install commands shown above
4. Create an initial git commit
5. Start working through Phase 0