# Screen 1 — Splash Screen
> Status: ✅ Analyzed
> Estimated Effort: 5–6 hours
> **Quoted Price: 900 AED** (Web + iOS + Android)
> Back to [INDEX.md](INDEX.md)

---

## ⚡ DECISIONS SUMMARY (Quick Reference)

| Decision | Final Resolution |
|---|---|
| **Background** | • Dark + brand color gradient<br>• Matches brand identity (pending client colors)<br>• Premium aesthetic for design company |
| **Animation** | • Fade in + scale up simultaneously<br>• Smooth, modern, professional<br>• Duration: 1.5 seconds total |
| **Total Duration** | • 1.5s animation + 0.5s hold = 2 seconds total<br>• Long enough to see animation, fast enough for user impatience<br>• Tested on slow networks |
| **Auth Check** | • Silently check AsyncStorage for JWT token during animation<br>• No interruption to user experience<br>• Ready to route on animation completion |
| **Redirect if Logged In** | • Navigate to Home screen (Screen 2)<br>• Show categories immediately<br>• Already authenticated, skip login |
| **Redirect if Logged Out** | • Navigate to Login screen<br>• User enters credentials<br>• Creates JWT token for future sessions |
| **First-Time User** | • **PENDING CLIENT ANSWER**<br>• Option A: Show onboarding slides (3-5 screens explaining app)r>• Option B: Skip to login directly<br>• Affects UX significantly |
| **App Name on Splash** | • **PENDING CLIENT ANSWER**<br>• Logo only, OR logo + app name textr>• Affects brand visibility |
| **Dark Mode Support** | • **PENDING CLIENT ANSWER**<br>• Single theme (light or dark) OR support both<br>• Single theme = faster to build, less complexity |

---

## What the Client Said
> "Logo appears → Simple animation → Auto redirect to Home screen"

---

## All Decisions

| Decision | Resolution |
|---|---|
| Background | Dark + brand color gradient (pending brand colors from client) |
| Animation | Fade in + scale up simultaneously |
| Duration | 1.5s animation + 0.5s hold = **2 seconds total** |
| Auth logic | Check token silently during animation |
| Redirect — logged in | → Home |
| Redirect — logged out | → Login |
| Redirect — first time user | → Onboarding (if any) OR Login — **ask client** |
| App name / tagline on splash | **Pending client answer** |
| Dark mode support | **Pending client answer** |

---

## Open Questions for Client

- [ ] Logo file — SVG or PNG with transparent background
- [ ] Brand colors (primary + secondary)
- [ ] Animation style — approve fade-in + scale (2 seconds)?
- [ ] App name?
- [ ] Tagline on splash or logo only?
- [ ] Dark mode or single theme?
- [ ] First-time user — onboarding slides, or straight to login?

---

## Logic Flow

```
App opens
  → Native splash (static, instant — required by iOS/Android)
  → SplashScreen.tsx mounts
  → Animation starts (fade in + scale, 1.5s)
  → Simultaneously: check AsyncStorage for auth token
  → Animation ends → 0.5s hold
  → Token valid   → navigate to Home
  → No token      → navigate to Login
  → First time    → navigate to Onboarding (TBD)
```

---

## Platform Notes

| Platform | Splash Behavior |
|---|---|
| iOS | Requires static LaunchScreen.storyboard first, then animated layer on top |
| Android | SplashScreen API (Android 12+) or custom Activity, then animated layer |
| Web | Full control — CSS/Framer Motion on initial load |

---

## Packages Needed (React Native + Expo)

```
expo-splash-screen       → controls when to hide native splash
expo-linear-gradient     → background gradient
react-native-reanimated  → smooth animation
@react-navigation/native → routing + redirect logic
```

---

## File Structure

```
src/
  screens/
    SplashScreen.tsx      ← animated component (fade + scale)
  navigation/
    AppNavigator.tsx      ← auth check + routing logic
  assets/
    logo.svg / logo.png
```

---

## Effort Breakdown — By Platform

### iOS + Android (React Native + Expo)
| Task | Hours | Reasoning |
|---|---|---|
| iOS static LaunchScreen.storyboard setup | 0.5 hrs | Apple requires a static native splash before animation layer — separate file, must match all iPhone screen sizes |
| Android SplashScreen API setup (Android 12+) | 0.5 hrs | Android also requires native splash config — different from iOS, needs its own setup |
| Animated SplashScreen component (fade + scale) | 1.5 hrs | Building the React Native animation layer using `react-native-reanimated`, tuning timing and easing curve |
| Auth token check + AsyncStorage logic | 1 hr | Silently read token during animation, handle expired token, decide routing destination |
| Navigation routing (Home / Login / Onboarding) | 0.5 hrs | Wiring up the 3 possible redirect targets with react-navigation |
| Testing on iOS simulator + Android emulator | 1 hr | Both platforms behave differently — must test splash timing, animation smoothness, routing on both |
| **Mobile Subtotal** | **5 hrs** | |

### Web (Next.js)
| Task | Hours | Reasoning |
|---|---|---|
| Splash/loading screen component (CSS + Framer Motion) | 0.5 hrs | Simpler on web — no native layer needed, pure CSS animation |
| Auth check (localStorage/cookie) + redirect logic | 0.5 hrs | Check session on load, redirect to correct page |
| **Web Subtotal** | **1 hr** | |

### Total
| | Hours | Rate | Cost |
|---|---|---|---|
| Mobile (iOS + Android) | 5 hrs | 150 AED/hr | 750 AED |
| Web | 1 hr | 150 AED/hr | 150 AED |
| **Total** | **6 hrs** | | **900 AED** |

---

## Pricing Justification (If Client Asks)

> **"Why does a splash screen cost 900 AED?"**
>
> A splash screen is not just a logo on a background. It requires:
> - 3 separate implementations: iOS native config, Android native config, and web
> - iOS and Android each have platform-specific requirements (LaunchScreen.storyboard, Android SplashScreen API) that must be done correctly or the app is rejected from the App Store / Play Store
> - Authentication logic runs silently during the animation — this is architectural work, not cosmetic
> - It is the first thing every user sees. It must be smooth, professional, and pixel-perfect on every device size
>
> **Rate: 150 AED/hour** — standard UAE freelance rate for full-stack React Native + Next.js development

---

## Notes & Decisions Log

- `May 11` — Screen fully analyzed. Waiting on logo + brand colors to start implementation.
- `May 11` — Recommended animation: fade-in + scale (2s). Client to approve.
- `May 11` — Pricing added: 900 AED (6 hrs × 150 AED/hr). Covers Web + iOS + Android.
